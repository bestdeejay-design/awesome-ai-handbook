# 🤖 AI-агенты

> Создание и использование автономных AI-агентов на локальных и облачных моделях.

[← Awesome AI Handbook](../README.ru.md) · [🇬🇧 English](README.md)

---

## Что такое AI-агент

**Обычный чат:** вы задали вопрос → модель ответила. И всё.

**AI-агент:** модель сама решает, что делать, вызывает инструменты, анализирует результат и повторяет, пока не достигнет цели.

```
Чат:  Вопрос → Ответ
Агент: Вопрос → Агент решает вызвать инструмент → Получает данные → Формулирует ответ
```

**Ключевые компоненты агента:**
1. **LLM** — языковая модель, которая принимает решения
2. **Инструменты (tools)** — функции, которые модель может вызывать
3. **Цикл «думай → действуй → наблюдай»** — повторяется, пока задача не решена
4. **Память** — контекст между шагами

---

## Файлы раздела

| Файл | Описание | Уровень | Время |
|------|----------|---------|-------|
| [ready-made.ru.md](ready-made.ru.md) | 🟢 Каталог готовых AI-агентов | 🟢 | 7 мин |
| [orchestrators.ru.md](orchestrators.ru.md) | 🔵 Оркестраторы с субагентами | 🔵 | 6 мин |
| [skills.ru.md](skills.ru.md) | 🟡 Скиллы, MCP-серверы, плагины | 🟡 | 6 мин |
| [architecture.ru.md](architecture.ru.md) | 🔵 Архитектура AI-агента | 🔵 | 11 мин |
| [patterns.ru.md](patterns.ru.md) | 🔵 Паттерны: ReAct, Multi-Agent, Reflection | 🔵 | 14 мин |
| [frameworks.ru.md](frameworks.ru.md) | 🔵 LangGraph, CrewAI, Agno — сравнение | 🔵 | 12 мин |
| [multi-agent.ru.md](multi-agent.ru.md) | 🔵 Мульти-агентные системы | 🔵 | 18 мин |
| [memory.ru.md](memory.ru.md) | 🔵 Долговременная память агентов | 🔵 | 9 мин |
| [prompting.ru.md](prompting.ru.md) | 🔵 System prompts для агентов | 🔵 | 11 мин |
| [ollama-for-agents.ru.md](ollama-for-agents.ru.md) | 🔵 Tool calling, JSON schema | 🔵 | 10 мин |
| [safety.ru.md](safety.ru.md) | 🔵 Безопасность, guardrails | 🔵 | 16 мин |
| [evaluation.ru.md](evaluation.ru.md) | 🔵 Тестирование, регрессии | 🔵 | 10 мин |

### Туториалы — `agents/tutorials/`

| Файл | Описание | Уровень | Время |
|------|----------|---------|-------|
| [README](tutorials/README.ru.md) | Индекс туториалов | — | — |
| [01-first-agent.ru.md](tutorials/01-first-agent.ru.md) | 🛠️ ReAct-агент на Python + Ollama | 🟢 | 30 мин |
| [02-agent-team.ru.md](tutorials/02-agent-team.ru.md) | 🛠️ Команда агентов (CrewAI) | 🟡 | 60 мин |
| [03-coding-agent.ru.md](tutorials/03-coding-agent.ru.md) | 🛠️ Coding-агент (LangGraph) | 🔴 | 45 мин |

---

## Связанные разделы

- [🔵 Локальные модели](../local-models/README.ru.md) — Ollama, модели, tool calling
- [🟢 Нулевой уровень](../basics/README.ru.md) — основы AI, установка
- [💼 Use Cases](../use-cases/README.ru.md) — применение агентов

---

**Навигация:** [← На главную](../README.ru.md) · [📖 Каталог](../catalog.ru.md)
