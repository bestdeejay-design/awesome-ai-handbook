# 🏗 Фреймворки для AI-агентов

Сравнение инструментов для создания агентов.

[← AI-агенты](README.md) · [Паттерны](patterns.md)

---

## Сравнительная таблица

| Фреймворк | ⭐ | Язык | Ключевая особенность | Для чего лучше всего | GitHub |
|---|---|---|---|---|---|
| **LangGraph** | 10K+ | Python | Графовые агенты, stateful | Сложные workflow, циклы | [langchain-ai/langgraph](https://github.com/langchain-ai/langgraph) |
| **CrewAI** | 25K+ | Python | Ролевые мульти-агенты | Команды агентов с ролями | [joaomdmoura/crewAI](https://github.com/joaomdmoura/crewAI) |
| **AutoGen** | 48K+ | Python | Мульти-агентный диалог | Исследования, сложные задачи | [microsoft/autogen](https://github.com/microsoft/autogen) |
| **Agno** | 15K+ | Python | Быстрый старт, простой API | Начинающие, прототипы | [agno-ai/agno](https://github.com/agno-ai/agno) |
| **OpenAI Agents SDK** | — | Python | Официальный, Responses API | Экосистема OpenAI | [openai/agents-sdk](https://github.com/openai/agents-sdk) |
| **Claude Agent SDK** | — | TypeScript | Официальный Anthropic | Экосистема Claude | [anthropics/claude-agent-sdk](https://github.com/anthropics/claude-agent-sdk) |
| **Mastra** | 10K+ | TypeScript | TypeScript-first | JS/TS разработчики | [mastra-ai/mastra](https://github.com/mastra-ai/mastra) |
| **Agency Swarm** | 3.7K | Python | AI-агентства | Автоматизация агентств | [VRSEN/agency-swarm](https://github.com/VRSEN/agency-swarm) |

---

## Как выбрать

| Сценарий | Рекомендация |
|----------|-------------|
| **Первый агент / прототип** | Agno или OpenAI Agents SDK |
| **Сложный workflow с циклами** | LangGraph |
| **Команда агентов с ролями** | CrewAI |
| **Исследования / мульти-агентные диалоги** | AutoGen |
| **TypeScript‑проект** | Mastra или Claude Agent SDK |
| **Локальная модель через Ollama** | Любой + [ollama-for-agents.md](ollama-for-agents.md) |

---

## LangGraph

Граф состояний: узлы — действия агента, рёбра — переходы. Поддерживает циклы, ветвления, human-in-the-loop.

**Когда:** production workflow с checkpoint'ами и персистентностью.

---

## CrewAI {#crewai}

Агенты с ролями (researcher, writer, reviewer) работают как «команда».

**Когда:** контент-пайплайны, исследования, задачи с делегированием.

---

## AutoGen (Microsoft) {#autogen-microsoft}

Мульти-агентные диалоги: агенты обмениваются сообщениями до решения задачи.

**Когда:** исследовательские сценарии, сложные многошаговые задачи.

---

## Локальный стек

```
Ваш код → LangGraph / CrewAI / Agno
              ↓
         OpenAI SDK (base_url=localhost:11434/v1)
              ↓
            Ollama → Qwen 3.5 4B / Coder 7B
```

Примеры API — в [ollama-for-agents.md](ollama-for-agents.md).
