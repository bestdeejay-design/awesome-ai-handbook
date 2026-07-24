# 🎯 Ollama для агентов

API и возможности Ollama, полезные при построении агентов.

[← AI-агенты](README.ru.md) · [Фреймворки](frameworks.ru.md)

---

## 1. Structured output (JSON Schema)

Ollama возвращает ответ строго по JSON Schema — без `outlines` или `lm-format-enforcer`:

```bash
curl http://localhost:11434/api/chat -d '{
  "model": "qwen3.5:4b",
  "messages": [{"role": "user", "content": "Извлеки: имя, возраст, город из: Иван, 25 лет, Москва"}],
  "format": {
    "type": "object",
    "properties": {
      "name": {"type": "string"},
      "age": {"type": "integer"},
      "city": {"type": "string"}
    },
    "required": ["name", "age", "city"]
  },
  "stream": false
}'
# → {"name": "Иван", "age": 25, "city": "Москва"}
```

Идеально для агентов: вызов функций, извлечение данных, AI pipelines.

---

## 2. Tool calling (нативная поддержка)

Модель сама решает, какой инструмент вызвать и с какими аргументами:

```bash
curl http://localhost:11434/api/chat -d '{
  "model": "qwen3.5:4b",
  "messages": [{"role": "user", "content": "Какая погода в Токио?"}],
  "tools": [{
    "type": "function",
    "function": {
      "name": "get_weather",
      "description": "Get weather for a city",
      "parameters": {
        "type": "object",
        "properties": {
          "city": {"type": "string"},
          "units": {"type": "string", "enum": ["celsius", "fahrenheit"]}
        },
        "required": ["city"]
      }
    }
  }],
  "stream": false
}'
```

---

## 3. Python SDK — полный агентский цикл

OpenAI SDK + Ollama дают возможность строить агентов на чистом Python, без дополнительных фреймворков.

### 3.1 Простой чат с моделью

```python
from openai import OpenAI

client = OpenAI(
    base_url="http://localhost:11434/v1",
    api_key="ollama"  # любое значение
)

response = client.chat.completions.create(
    model="qwen3.5:4b",
    messages=[{"role": "user", "content": "Привет! Как дела?"}]
)
print(response.choices[0].message.content)
```

### 3.2 Tool calling через OpenAI SDK

Модель сама решает, какой инструмент вызвать:

```python
from openai import OpenAI

client = OpenAI(
    base_url="http://localhost:11434/v1",
    api_key="ollama"
)

# Описываем инструменты
tools = [{
    "type": "function",
    "function": {
        "name": "get_weather",
        "description": "Получить погоду для города",
        "parameters": {
            "type": "object",
            "properties": {
                "city": {"type": "string", "description": "Название города"}
            },
            "required": ["city"]
        }
    }
}]

# Модель решает, нужен ли инструмент
response = client.chat.completions.create(
    model="qwen3.5:4b",
    messages=[{"role": "user", "content": "Какая погода в Токио?"}],
    tools=tools
)

message = response.choices[0].message
if message.tool_calls:
    for tool_call in message.tool_calls:
        name = tool_call.function.name
        args = tool_call.function.arguments
        print(f"Модель вызвала: {name}({args})")
else:
    print(f"Ответ модели: {message.content}")
```

### 3.3 Полный цикл агента: думай → действуй → наблюдай

```python
from openai import OpenAI
import json

client = OpenAI(
    base_url="http://localhost:11434/v1",
    api_key="ollama"
)

# Реальные функции, которые будет вызывать агент
def search_web(query: str) -> str:
    """Поиск в интернете (заглушка для примера)"""
    return f"Результаты поиска по запросу '{query}': найдено 42 страницы"

def calculate(expression: str) -> str:
    """Вычисление математического выражения"""
    try:
        return str(eval(expression))
    except:
        return "Ошибка вычисления"

AVAILABLE_TOOLS = {
    "search_web": search_web,
    "calculate": calculate,
}

TOOL_SCHEMAS = [{
    "type": "function",
    "function": {
        "name": "search_web",
        "description": "Поиск информации в интернете",
        "parameters": {
            "type": "object",
            "properties": {
                "query": {"type": "string", "description": "Поисковый запрос"}
            },
            "required": ["query"]
        }
    }
}, {
    "type": "function",
    "function": {
        "name": "calculate",
        "description": "Вычисление математического выражения",
        "parameters": {
            "type": "object",
            "properties": {
                "expression": {"type": "string", "description": "Выражение"}
            },
            "required": ["expression"]
        }
    }
}]

def agent_loop(user_input: str, max_steps: int = 5):
    """Полный агентский цикл на OpenAI SDK + Ollama."""
    
    messages = [{"role": "user", "content": user_input}]
    
    for step in range(max_steps):
        response = client.chat.completions.create(
            model="qwen3.5:4b",
            messages=messages,
            tools=TOOL_SCHEMAS
        )
        
        msg = response.choices[0].message
        messages.append(msg)
        
        if msg.tool_calls:
            for tc in msg.tool_calls:
                name = tc.function.name
                args = json.loads(tc.function.arguments)
                
                print(f"  ▶ Шаг {step+1}: {name}({args})")
                
                # Выполняем функцию
                result = AVAILABLE_TOOLS[name](**args)
                
                messages.append({
                    "role": "tool",
                    "tool_call_id": tc.id,
                    "content": result
                })
        else:
            return msg.content
    
    return "Достигнут лимит шагов"

# Тест
result = agent_loop("Сколько будет 2 + 2 * 3? И найди информацию про AI-агентов")
print(f"\nОтвет: {result}")
```

### 3.4 Structured output через OpenAI SDK

```python
from openai import OpenAI
from pydantic import BaseModel

client = OpenAI(
    base_url="http://localhost:11434/v1",
    api_key="ollama"
)

class UserData(BaseModel):
    name: str
    age: int
    city: str

completion = client.beta.chat.completions.parse(
    model="qwen3.5:4b",
    messages=[{"role": "user", "content": "Извлеки: Иван, 25, Москва"}],
    response_format=UserData,
)

user = completion.choices[0].message.parsed
print(f"Имя: {user.name}, Возраст: {user.age}, Город: {user.city}")
```

### 3.5 Streaming — поточный вывод для агентов

```python
stream = client.chat.completions.create(
    model="qwen3.5:4b",
    messages=[{"role": "user", "content": "Напиши рассказ про робота"}],
    stream=True
)

for chunk in stream:
    if chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="", flush=True)
```

Streaming полезен, когда агент выдаёт длинные ответы — пользователь видит процесс в реальном времени.

---

## 4. OpenAI-совместимые эндпоинты

Любая библиотека для OpenAI работает с Ollama — просто смените `base_url`:

| OpenAI endpoint | Ollama endpoint | Описание |
|----------------|----------------|----------|
| `/v1/chat/completions` | `/v1/chat/completions` | Чат (включая tool calling) |
| `/v1/completions` | `/v1/completions` | Генерация текста |
| `/v1/embeddings` | `/v1/embeddings` | Эмбеддинги |
| `/v1/models` | `/v1/models` | Список моделей |

---

## 5. Быстрые интеграции через `ollama launch`

```bash
ollama launch claude       # Claude Code → использует Ollama
ollama launch codex        # Codex → использует Ollama
ollama launch openclaw     # AI assistant в WhatsApp/Telegram
ollama launch cline        # Cline VS Code integration
```

---

## 6. Экспериментальный веб-поиск

```bash
curl http://localhost:11434/api/experimental/web_search -d '{
  "query": "последние новости AI 2026"
}'
```

Позволяет агенту искать информацию в интернете без отдельного Search API.

---

## 7. Клиенты для повседневной работы

Ollama — сервер (локальный OpenAI API). Для общения удобны клиенты:

| Инструмент | Платформа | Фишка |
|---|---|---|
| [**Enchanted**](https://github.com/gluonfield/enchanted) | macOS + iOS | Нативный SwiftUI, подключение к Ollama по URI |
| [**MindWork AI Studio**](https://github.com/MindWorkAI/AI-Studio) | macOS/Win/Linux | Локальные + облачные провайдеры в одном GUI |
| [**Open WebUI**](https://github.com/open-webui/open-webui) | Веб (Docker) | RAG, мультимодальность, пользователи |
| **LM Studio** | macOS/Win/Linux | GUI для скачивания и теста моделей |
| [**Continue**](https://github.com/continuedev/continue) | VS Code / JetBrains | AI-автодополнение с локальной моделью |

Полный каталог инструментов — в [local-models/tools.md](../local-models/tools.ru.md).

---

## 8. Reasoning: параметр `think`

Reasoning-модели (DeepSeek R1, Phi-4-reasoning) поддерживают `think` в Ollama:

```bash
# Без reasoning (быстрый ответ)
curl http://localhost:11434/api/chat -d '{
  "model": "deepseek-r1:7b",
  "messages": [{"role": "user", "content": "Сколько будет 2+2?"}],
  "options": {"think": false}
}'

# Максимальное reasoning
curl http://localhost:11434/api/chat -d '{
  "model": "deepseek-r1:7b",
  "messages": [{"role": "user", "content": "Реши уравнение x² + 3x + 2 = 0"}],
  "options": {"think": "high"}
}'
```

Значения: `false` | `true` | `"low"` | `"medium"` | `"high"` | `"max"`.

---

## Что дальше

| Если хотите | Переходите |
|-------------|-----------|
| Понять архитектурные паттерны агентов | [patterns.md](patterns.ru.md) |
| Выбрать фреймворк | [frameworks.md](frameworks.ru.md) |
| Посмотреть полный гайд по Ollama | [../local-models/advanced-setup.ru.md](../local-models/advanced-setup.ru.md) |
| Вернуться к навигации | [README.md](README.ru.md) |
