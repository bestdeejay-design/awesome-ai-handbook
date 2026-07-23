# 🤖 AI-агенты

Создание и использование автономных AI-агентов на локальных и облачных моделях.

[← Главная](../README.ru.md)

---

## Что такое AI-агент

AI-агент = LLM + память + инструменты + цикл принятия решений.

В отличие от обычного чата, агент может планировать, использовать инструменты, помнить контекст и анализировать результаты.

---

## Содержание

| Документ | Описание |
|----------|----------|
| [patterns.md](patterns.md) | ReAct, Plan-and-Execute, Multi-Agent, Reflection |
| [frameworks.md](frameworks.md) | LangGraph, CrewAI, AutoGen, Agno, Mastra… |
| [ollama-for-agents.md](ollama-for-agents.md) | Tool calling, JSON schema, OpenAI API |
| [tutorials/](tutorials/) | Пошаговые гайды 🚧 |

---

## Быстрый путь

```
1. patterns.md          — понять архитектуру
2. ollama-for-agents.md — подключить локальную модель
3. frameworks.md        — выбрать фреймворк
4. use-cases/coding.md  — пример для разработки
```

---

## Локальный стек

```
LangGraph / CrewAI / Agno
        ↓
OpenAI SDK (base_url=http://localhost:11434/v1)
        ↓
Ollama → qwen3.5:4b
```

См. [ollama-for-agents.md](ollama-for-agents.md).
