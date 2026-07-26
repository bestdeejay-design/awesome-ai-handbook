# <img src="https://lucide.dev/api/icons/bot" alt="" width="20" height="20" style="display:inline;vertical-align:middle"> AI-агенты

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
| [ready-made.ru.md](ready-made.ru.md) | <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> Каталог готовых AI-агентов | <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> | 7 мин |
| [orchestrators.ru.md](orchestrators.ru.md) | <img src="assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> Оркестраторы с субагентами | <img src="assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> | 6 мин |
| [skills.ru.md](skills.ru.md) | <img src="assets/icons/level-yellow.svg" alt="" width="12" height="12" style="vertical-align:middle"> Скиллы, MCP-серверы, плагины | <img src="assets/icons/level-yellow.svg" alt="" width="12" height="12" style="vertical-align:middle"> | 6 мин |
| [architecture.ru.md](architecture.ru.md) | <img src="assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> Архитектура AI-агента | <img src="assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> | 11 мин |
| [patterns.ru.md](patterns.ru.md) | <img src="assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> Паттерны: ReAct, Multi-Agent, Reflection | <img src="assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> | 14 мин |
| [frameworks.ru.md](frameworks.ru.md) | <img src="assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> LangGraph, CrewAI, Agno — сравнение | <img src="assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> | 12 мин |
| [multi-agent.ru.md](multi-agent.ru.md) | <img src="assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> Мульти-агентные системы | <img src="assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> | 18 мин |
| [memory.ru.md](memory.ru.md) | <img src="assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> Долговременная память агентов | <img src="assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> | 9 мин |
| [prompting.ru.md](prompting.ru.md) | <img src="assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> System prompts для агентов | <img src="assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> | 11 мин |
| [ollama-for-agents.ru.md](ollama-for-agents.ru.md) | <img src="assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> Tool calling, JSON schema | <img src="assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> | 10 мин |
| [safety.ru.md](safety.ru.md) | <img src="assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> Безопасность, guardrails | <img src="assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> | 16 мин |
| [evaluation.ru.md](evaluation.ru.md) | <img src="assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> Тестирование, регрессии | <img src="assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> | 10 мин |

### Туториалы — `agents/tutorials/`

| Файл | Описание | Уровень | Время |
|------|----------|---------|-------|
| [README](tutorials/README.ru.md) | Индекс туториалов | — | — |
| [01-first-agent.ru.md](tutorials/01-first-agent.ru.md) | <img src="https://lucide.dev/api/icons/wrench" alt="" width="20" height="20" style="display:inline;vertical-align:middle"> ReAct-агент на Python + Ollama | <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> | 30 мин |
| [02-agent-team.ru.md](tutorials/02-agent-team.ru.md) | <img src="https://lucide.dev/api/icons/wrench" alt="" width="20" height="20" style="display:inline;vertical-align:middle"> Команда агентов (CrewAI) | <img src="assets/icons/level-yellow.svg" alt="" width="12" height="12" style="vertical-align:middle"> | 60 мин |
| [03-coding-agent.ru.md](tutorials/03-coding-agent.ru.md) | <img src="https://lucide.dev/api/icons/wrench" alt="" width="20" height="20" style="display:inline;vertical-align:middle"> Coding-агент (LangGraph) | <img src="assets/icons/level-red.svg" alt="" width="12" height="12" style="vertical-align:middle"> | 45 мин |

---

## Связанные разделы

- [<img src="assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> Локальные модели](../local-models/README.ru.md) — Ollama, модели, tool calling
- [<img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> Нулевой уровень](../basics/README.ru.md) — основы AI, установка
- [<img src="https://lucide.dev/api/icons/briefcase" alt="" width="20" height="20" style="display:inline;vertical-align:middle"> Use Cases](../use-cases/README.ru.md) — применение агентов

---

**Навигация:** [← На главную](../README.ru.md) · [<img src="https://lucide.dev/api/icons/book-open" alt="" width="20" height="20" style="display:inline;vertical-align:middle"> Каталог](../catalog.ru.md)
