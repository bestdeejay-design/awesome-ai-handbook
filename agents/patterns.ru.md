# 🧩 Паттерны AI-агентов

> Архитектурные паттерны для построения агентов поверх LLM — от простого ReAct до мульти-агентных систем с кодом.

[← AI-агенты](README.ru.md) · [Архитектура](architecture.ru.md) · [Фреймворки →](frameworks.ru.md)

---

## Содержание

1. [ReAct (Reason + Act)](#1-react-reason--act)
2. [Plan-and-Execute](#2-plan-and-execute)
3. [Reflection / Self-Correction](#3-reflection--self-correction)
4. [Tool Use (Function Calling)](#4-tool-use-function-calling)
5. [Multi-Agent](#5-multi-agent)
6. [Как выбирать паттерн](#6-как-выбирать-паттерн)
7. [Что дальше](#7-что-дальше)

---

## 1. ReAct (Reason + Act)

**Самый популярный паттерн.** Агент чередует рассуждение (Reason) и действие (Act) в цикле.

### Как работает

```
Thought:  Пользователь спрашивает про погоду. Нужно найти информацию.
Action:   call search_web(query="погода Токио")
Observation: "В Токио +22°C, ясно"
Thought:  У меня есть информация. Могу ответить.
Answer:   Сейчас в Токио +22°C и ясно.
```

Каждый цикл: модель **думает**, что делать → **делает** (вызывает инструмент или отвечает) → получает **результат** → думает снова.

### Когда использовать

- ✅ Задачи, где нужен поиск информации
- ✅ Вопросы, требующие вычислений
- ✅ Простые цепочки действий (найти → проанализировать → ответить)
- ❌ Задачи с длинным планом (лучше Plan-and-Execute)

### Реализация на Python

```python
import requests
import json

OLLAMA = "http://localhost:11434/api/chat"

def react_agent(task: str, tools: list, max_steps: int = 5):
    """
    ReAct агент: чередует рассуждение и действия.
    """
    messages = [
        {"role": "system", "content": (
            "Ты — ReAct-агент. Отвечай на запросы пользователя. "
            "Если нужно найти информацию — используй инструменты. "
            "Отвечай только когда у тебя достаточно данных."
        )},
        {"role": "user", "content": task}
    ]
    
    for step in range(max_steps):
        response = requests.post(OLLAMA, json={
            "model": "qwen3.5:4b",
            "messages": messages,
            "tools": tools,
            "stream": False
        })
        msg = response.json()["message"]
        messages.append(msg)
        
        # Если модель вызывает инструмент
        if msg.get("tool_calls"):
            for tc in msg["tool_calls"]:
                name = tc["function"]["name"]
                args = tc["function"]["arguments"]
                print(f"  [Step {step+1}] {name}({json.dumps(args)})")
                
                # Выполняем (в реальности — lookup)
                result = {"result": f"выполнено {name} с {args}"}
                
                messages.append({
                    "role": "tool",
                    "name": name,
                    "content": json.dumps(result)
                })
        else:
            return msg["content"]
    
    return "Не удалось完成 задачу за отведённое число шагов"
```

### Плюсы и минусы

| Плюсы | Минусы |
|-------|--------|
| Простой для понимания и реализации | Может зациклиться |
| Работает с любой моделью | Каждый шаг ест токены |
| Прозрачный — видно каждое решение | Нет планирования наперёд |

### В контексте твоей команды

ReAct подходит для **агентов-исполнителей**: аналитик ищет данные, разработчик читает документацию, тестировщик проверяет результат. Каждый работает в своём цикле.

---

## 2. Plan-and-Execute

Агент сначала составляет план, потом выполняет шаги один за другим.

### Как работает

```
ШАГ 1 — Планирование:
План:
  1. Найти репозиторий проекта на GitHub
  2. Прочитать README
  3. Собрать список зависимостей
  4. Сформировать отчёт

ШАГ 2 — Исполнение:
  → Выполняю шаг 1: search_web("repo awesome-ai-handbook")
  → Результат: найден github.com/bestdeejay-design/awesome-ai-handbook
  → Выполняю шаг 2: read_file("https://github.com/.../README.md")
  → ...
```

### Когда использовать

- ✅ Сложные задачи с 3+ шагами
- ✅ Миграция кода, рефакторинг
- ✅ Исследовательские задачи
- ❌ Простые вопросы (ReAct быстрее)

### Реализация на Python

```python
def plan_and_execute(task: str, tools: list):
    """Plan-and-Execute: сначала план, потом выполнение."""
    
    # Фаза 1: создаём план
    plan_prompt = (
        f"Составь подробный план для задачи: {task}\n"
        "Верни план как список шагов в формате JSON.\n"
        "Каждый шаг: {\"step\": \"описание\", \"tool\": \"имя_инструмента\"}"
    )
    
    plan_response = requests.post(OLLAMA, json={
        "model": "qwen3.5:4b",
        "messages": [{"role": "user", "content": plan_prompt}],
        "format": {
            "type": "object",
            "properties": {
                "steps": {
                    "type": "array",
                    "items": {
                        "type": "object",
                        "properties": {
                            "step": {"type": "string"},
                            "tool": {"type": "string"}
                        }
                    }
                }
            }
        },
        "stream": False
    })
    
    plan = json.loads(plan_response.json()["message"]["content"])
    print(f"📋 План: {len(plan['steps'])} шагов")
    
    # Фаза 2: выполняем шаги
    results = []
    for i, step in enumerate(plan["steps"]):
        print(f"  ▶ Шаг {i+1}: {step['step']}")
        
        # Выполняем инструмент или просто спрашиваем модель
        response = requests.post(OLLAMA, json={
            "model": "qwen3.5:4b",
            "messages": [
                {"role": "system", "content": (
                    f"Ты выполняешь шаг {i+1} плана. "
                    f"Контекст: {step['step']}. "
                    f"Предыдущие результаты: {json.dumps(results)}"
                )},
                {"role": "user", "content": step['step']}
            ],
            "tools": tools,
            "stream": False
        })
        result = response.json()["message"]["content"]
        results.append({"step": i+1, "result": result})
    
    return results
```

### Плюсы и минусы

| Плюсы | Минусы |
|-------|--------|
| Агент не «забывает» цель | Если план плохой — вся задача плохая |
| Прозрачный прогресс | Не может адаптироваться по ходу |
| Можно переиспользовать план | Тратит токены на планирование |

### В контексте твоей команды

Plan-and-Execute идеален для **агента-менеджера проекта**: он составляет план, распределяет задачи по исполнителям и следит за выполнением.

---

## 3. Reflection / Self-Correction

Агент проверяет свой ответ, находит ошибки и улучшает его.

### Как работает

```
ПРОХОД 1 — Генерация:
  Ответ: «Python был создан в 1991 году Гвидо ван Россумом»
  
ПРОХОД 2 — Рефлексия:
  Критика: «Ответ верный, но можно добавить версию и контекст»
  
ПРОХОД 3 — Улучшение:
  Финальный ответ: «Python был создан Гвидо ван Россумом в 1991 году.
  Первая версия (0.9.0) вышла 20 февраля 1991. Сегодня Python — один
  из самых популярных языков программирования в мире.»
```

### Когда использовать

- ✅ Написание текстов (статьи, документация)
- ✅ Код, который нужно проверить перед использованием
- ✅ Сложные рассуждения
- ❌ Простые ответы (избыточно)

### Реализация на Python

```python
def reflection_agent(task: str, iterations: int = 2):
    """Reflection агент: генерирует, критикует, улучшает."""
    
    messages = [
        {"role": "system", "content": "Ты — эксперт. Отвечай на вопросы."},
        {"role": "user", "content": task}
    ]
    
    # Первичная генерация
    response = requests.post(OLLAMA, json={
        "model": "qwen3.5:4b",
        "messages": messages,
        "stream": False
    })
    answer = response.json()["message"]["content"]
    
    for i in range(iterations):
        # Критика
        critique = requests.post(OLLAMA, json={
            "model": "qwen3.5:4b",
            "messages": [
                {"role": "system", "content": (
                    "Ты — строгий критик. Найди ошибки, неточности и "
                    "пропущенные детали в ответе. Будь придирчивым."
                )},
                {"role": "user", "content": f"Задача: {task}\n\nОтвет: {answer}"}
            ],
            "stream": False
        })
        feedback = critique.json()["message"]["content"]
        
        # Улучшение
        improved = requests.post(OLLAMA, json={
            "model": "qwen3.5:4b",
            "messages": [
                {"role": "system", "content": (
                    "Улучши ответ на основе критики. Исправь все ошибки, "
                    "добавь детали. Верни только финальную версию."
                )},
                {"role": "user", "content": (
                    f"Исходный ответ: {answer}\n\n"
                    f"Критика: {feedback}\n\n"
                    f"Улучшенный ответ:"
                )}
            ],
            "stream": False
        })
        answer = improved.json()["message"]["content"]
    
    return answer
```

### Плюсы и минусы

| Плюсы | Минусы |
|-------|--------|
| Заметно лучшее качество ответов | Тратит в 2-3× больше токенов |
| Находит ошибки, которые модель пропустила | Может «переулучшить» и испортить |
| Полезен для важных ответов | Не нужен для простых задач |

### В контексте твоей команды

Reflection — паттерн для **агента-ревьюера**. Он проверяет код, тексты, решения перед тем, как они пойдут дальше. В твоей команде: разработчик написал код → тестировщик проверил → ревьюер оценил качество.

---

## 4. Tool Use (Function Calling)

Самый базовый паттерн: модель вызывает функции, но без цикла — один запрос, один вызов.

### Отличие от ReAct

| Tool Use | ReAct |
|----------|-------|
| Один вызов инструмента | Цикл из нескольких вызовов |
| Нет рассуждения между вызовами | Есть Thought между Action |
| Проще, меньше токенов | Сложнее, но гибче |

### Когда использовать

- ✅ Точное извлечение данных из текста
- ✅ Классификация (выбрать категорию)
- ✅ Перевод в JSON
- ✅ Когда нужен гарантированный формат ответа

### Реализация

```python
def tool_call(model: str, user_input: str, tools: list):
    """Одиночный вызов инструмента — без цикла."""
    response = requests.post(f"{OLLAMA}", json={
        "model": model,
        "messages": [{"role": "user", "content": user_input}],
        "tools": tools,
        "stream": False
    })
    return response.json()["message"]
```

Подробнее про API — в [ollama-for-agents.md](ollama-for-agents.ru.md).

---

## 5. Multi-Agent

Несколько агентов работают вместе, обмениваясь результатами. Это **ключевой паттерн для твоей цели** — команды агентов.

### Архитектуры мульти-агентных систем

```
Supervisor (иерархическая):
  ┌────────────┐
  │ Supervisor │  ← управляет
  └───┬───┬────┘
      │   │
  ┌───┘   └───┐
  ▼           ▼
┌──────┐  ┌──────┐
│ Агент │  │ Агент │
│   A   │  │   B   │
└──────┘  └──────┘

Peer-to-Peer (горизонтальная):
  ┌──────┐     ┌──────┐
  │ Агент │◄───►│ Агент │
  │   A   │     │   B   │
  └──────┘     └──────┘
       ▲           ▲
       └─────┬─────┘
             ▼
        ┌────────┐
        │  Общая  │
        │ задача │
        └────────┘

Swarm (роевая):
  Агенты динамически передают задачи друг другу
  Агент A → (не могу) → Агент B → (сделал) → Агент C → ...
```

### Когда использовать

- ✅ Задачи, требующие разных экспертиз (кодинг + дизайн + DevOps)
- ✅ Большие проекты, которые нужно декомпозировать
- ✅ Когда нужна проверка и баланс (один пишет, второй проверяет)
- ❌ Простые задачи (один агент справится быстрее)

### Простейшая реализация

```python
class Agent:
    """Простой агент с ролью."""
    
    def __init__(self, name: str, role: str, model: str = "qwen3.5:4b"):
        self.name = name
        self.role = role
        self.model = model
    
    def run(self, task: str, context: str = "") -> str:
        response = requests.post(OLLAMA, json={
            "model": self.model,
            "messages": [
                {"role": "system", "content": (
                    f"Ты — {self.name}. Твоя роль: {self.role}."
                )},
                {"role": "user", "content": f"{context}\n\nЗадача: {task}"}
            ],
            "stream": False
        })
        return response.json()["message"]["content"]


class Supervisor:
    """Управляет командой агентов."""
    
    def __init__(self, agents: list[Agent]):
        self.agents = agents
    
    def run(self, project_task: str):
        context = ""
        for agent in self.agents:
            print(f"\n  ▶ {agent.name} работает...")
            result = agent.run(project_task, context)
            print(f"    Результат: {result[:100]}...")
            context += f"\n{agent.name}: {result}"
        return context


# Пример: команда для анализа проекта
pm = Agent("PM", "Менеджер проекта. Ставишь задачи и проверяешь сроки.")
analyst = Agent("Аналитик", "Анализируешь требования и пишешь спецификации.")
dev = Agent("Разработчик", "Пишешь код по спецификации.")

team = Supervisor([pm, analyst, dev])
result = team.run("Создать REST API для управления задачами")
```

Подробнее про мульти-агентные системы — в [multi-agent.md](multi-agent.ru.md) и туториале [02-agent-team.md](tutorials/02-agent-team.ru.md).

---

## 6. Как выбирать паттерн

```python
def choose_pattern(task: str):
    """
    Эвристика: какой паттерн подойдёт для задачи.
    """
    if len(task.split()) < 10:
        return "Tool Use"
    elif "проверь" in task.lower() or "улучши" in task.lower():
        return "Reflection"
    elif any(w in task.lower() for w in ["найди", "поищи", "сколько", "какой"]):
        return "ReAct"
    elif any(w in task.lower() for w in ["спланируй", "сделай проект", "разработай"]):
        return "Plan-and-Execute"
    elif any(w in task.lower() for w in ["команда", "агенты", "распредели"]):
        return "Multi-Agent"
    else:
        return "ReAct"
```

Или проще таблицей:

| Ситуация | Паттерн |
|----------|---------|
| «Сколько будет 2 + 2?» | Tool Use (калькулятор) |
| «Найди информацию о...» | ReAct (поиск → ответ) |
| «Напиши статью про...» | Reflection (черновик → ревью) |
| «Сделай рефакторинг модуля» | Plan-and-Execute (план → шаги) |
| «Запусти проект с нуля» | Multi-Agent (команда) |

---

## 7. Что дальше

| Если хотите | Переходите |
|-------------|-----------|
| Выбрать фреймворк для реализации паттерна | [frameworks.md](frameworks.ru.md) |
| Подключить локальную модель (tool calling, API) | [ollama-for-agents.md](ollama-for-agents.ru.md) |
| Понять архитектуру агента глубже | [architecture.md](architecture.ru.md) |
| Собрать команду агентов | [multi-agent.md](multi-agent.ru.md) |
| Написать первого агента | [tutorials/01-first-agent.ru.md](tutorials/01-first-agent.ru.md) |
| Вернуться к навигации | [README.md](README.ru.md) |
