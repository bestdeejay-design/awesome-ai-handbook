# 🎯 Ollama для агентов

API и возможности Ollama, полезные при построении агентов.

[← AI-агенты](README.md) · [Фреймворки](frameworks.md)

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

## 3. OpenAI-совместимые эндпоинты

Любая библиотека для OpenAI работает с Ollama — просто смените `base_url`:

```python
from openai import OpenAI

client = OpenAI(
    base_url="http://localhost:11434/v1",
    api_key="ollama"  # любое значение
)
response = client.chat.completions.create(
    model="qwen3.5:4b",
    messages=[{"role": "user", "content": "Привет!"}]
)
```

Поддерживаются: `/v1/chat/completions`, `/v1/completions`, `/v1/embeddings`, `/v1/models`.

---

## 4. Быстрые интеграции через `ollama launch`

```bash
ollama launch claude       # Claude Code → использует Ollama
ollama launch codex        # Codex → использует Ollama
ollama launch openclaw     # AI assistant в WhatsApp/Telegram
ollama launch cline        # Cline VS Code integration
```

---

## 5. Экспериментальный веб-поиск

```bash
curl http://localhost:11434/api/experimental/web_search -d '{
  "query": "последние новости AI 2026"
}'
```

Позволяет агенту искать информацию в интернете без отдельного Search API.

---

## 6. Клиенты для повседневной работы

Ollama — сервер (локальный OpenAI API). Для общения удобны клиенты:

| Инструмент | Платформа | Фишка |
|---|---|---|
| [**Enchanted**](https://github.com/gluonfield/enchanted) | macOS + iOS | Нативный SwiftUI, подключение к Ollama по URI |
| [**MindWork AI Studio**](https://github.com/MindWorkAI/AI-Studio) | macOS/Win/Linux | Локальные + облачные провайдеры в одном GUI |
| [**Open WebUI**](https://github.com/open-webui/open-webui) | Веб (Docker) | RAG, мультимодальность, пользователи |
| **LM Studio** | macOS/Win/Linux | GUI для скачивания и теста моделей |
| [**Continue**](https://github.com/continuedev/continue) | VS Code / JetBrains | AI-автодополнение с локальной моделью |

Полный каталог инструментов — в [local-models/tools.md](../local-models/tools.md).

---

## Reasoning: параметр `think`

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
