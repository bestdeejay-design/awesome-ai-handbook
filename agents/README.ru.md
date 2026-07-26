# <img src="https://lucide.dev/api/icons/bot" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> AI-агенты

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
| [ready-made.ru.md](ready-made.ru.md) | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#22c55e" stroke="#16a34a" stroke-width="0.5"/></svg> Каталог готовых AI-агентов | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#22c55e" stroke="#16a34a" stroke-width="0.5"/></svg> | 7 мин |
| [orchestrators.ru.md](orchestrators.ru.md) | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> Оркестраторы с субагентами | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> | 6 мин |
| [skills.ru.md](skills.ru.md) | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#eab308" stroke="#ca8a04" stroke-width="0.5"/></svg> Скиллы, MCP-серверы, плагины | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#eab308" stroke="#ca8a04" stroke-width="0.5"/></svg> | 6 мин |
| [architecture.ru.md](architecture.ru.md) | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> Архитектура AI-агента | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> | 11 мин |
| [patterns.ru.md](patterns.ru.md) | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> Паттерны: ReAct, Multi-Agent, Reflection | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> | 14 мин |
| [frameworks.ru.md](frameworks.ru.md) | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> LangGraph, CrewAI, Agno — сравнение | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> | 12 мин |
| [multi-agent.ru.md](multi-agent.ru.md) | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> Мульти-агентные системы | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> | 18 мин |
| [memory.ru.md](memory.ru.md) | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> Долговременная память агентов | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> | 9 мин |
| [prompting.ru.md](prompting.ru.md) | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> System prompts для агентов | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> | 11 мин |
| [ollama-for-agents.ru.md](ollama-for-agents.ru.md) | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> Tool calling, JSON schema | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> | 10 мин |
| [safety.ru.md](safety.ru.md) | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> Безопасность, guardrails | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> | 16 мин |
| [evaluation.ru.md](evaluation.ru.md) | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> Тестирование, регрессии | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> | 10 мин |

### Туториалы — `agents/tutorials/`

| Файл | Описание | Уровень | Время |
|------|----------|---------|-------|
| [README](tutorials/README.ru.md) | Индекс туториалов | — | — |
| [01-first-agent.ru.md](tutorials/01-first-agent.ru.md) | <img src="https://lucide.dev/api/icons/wrench" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> ReAct-агент на Python + Ollama | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#22c55e" stroke="#16a34a" stroke-width="0.5"/></svg> | 30 мин |
| [02-agent-team.ru.md](tutorials/02-agent-team.ru.md) | <img src="https://lucide.dev/api/icons/wrench" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> Команда агентов (CrewAI) | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#eab308" stroke="#ca8a04" stroke-width="0.5"/></svg> | 60 мин |
| [03-coding-agent.ru.md](tutorials/03-coding-agent.ru.md) | <img src="https://lucide.dev/api/icons/wrench" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> Coding-агент (LangGraph) | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#ef4444" stroke="#dc2626" stroke-width="0.5"/></svg> | 45 мин |

---

## Связанные разделы

- [<svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> Локальные модели](../local-models/README.ru.md) — Ollama, модели, tool calling
- [<svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#22c55e" stroke="#16a34a" stroke-width="0.5"/></svg> Нулевой уровень](../basics/README.ru.md) — основы AI, установка
- [<img src="https://lucide.dev/api/icons/briefcase" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> Use Cases](../use-cases/README.ru.md) — применение агентов

---

**Навигация:** [← На главную](../README.ru.md) · [<img src="https://lucide.dev/api/icons/book-open" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> Каталог](../catalog.ru.md)
