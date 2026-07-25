# 🤖 AI Agents

> Building and using autonomous AI agents on local and cloud models.

[← Awesome AI Handbook](../README.md) · [🇷🇺 Русский](README.ru.md)

---

## What is an AI Agent

**Regular chat:** you ask a question → the model answers. That's it.

**AI agent:** the model decides what to do, calls tools, analyzes results, and repeats until it reaches its goal.

```
Chat:  Question → Answer
Agent: Question → Agent decides to call a tool → Gets data → Formulates answer
```

**Key components of an agent:**
1. **LLM** — the language model that makes decisions
2. **Tools** — functions the model can call
3. **Think → Act → Observe loop** — repeats until the task is done
4. **Memory** — context between steps

---

## Section Files

| File | Description | Level | Time |
|------|-------------|-------|------|
| [ready-made.md](ready-made.md) | 🟢 Ready-made AI agents catalog | 🟢 | 7 min |
| [orchestrators.md](orchestrators.md) | 🔵 Sub-agent orchestrators | 🔵 | 6 min |
| [skills.md](skills.md) | 🟡 Skills, MCP servers, plugins | 🟡 | 6 min |
| [architecture.md](architecture.md) | 🔵 AI agent architecture | 🔵 | 11 min |
| [patterns.md](patterns.md) | 🔵 Patterns: ReAct, Multi-Agent, Reflection | 🔵 | 14 min |
| [frameworks.md](frameworks.md) | 🔵 LangGraph, CrewAI, Agno comparison | 🔵 | 12 min |
| [multi-agent.md](multi-agent.md) | 🔵 Multi-agent systems | 🔵 | 18 min |
| [memory.md](memory.md) | 🔵 Long-term agent memory | 🔵 | 9 min |
| [prompting.md](prompting.md) | 🔵 System prompts for agents | 🔵 | 11 min |
| [ollama-for-agents.md](ollama-for-agents.md) | 🔵 Tool calling, JSON schema | 🔵 | 10 min |
| [safety.md](safety.md) | 🔵 Safety, guardrails | 🔵 | 16 min |
| [evaluation.md](evaluation.md) | 🔵 Testing, regression | 🔵 | 10 min |

### Tutorials — `agents/tutorials/`

| File | Description | Level | Time |
|------|-------------|-------|------|
| [README](tutorials/README.md) | Tutorial index | — | — |
| [01-first-agent.md](tutorials/01-first-agent.md) | 🛠️ ReAct agent in Python + Ollama | 🟢 | 30 min |
| [02-agent-team.md](tutorials/02-agent-team.md) | 🛠️ Multi-agent team (CrewAI) | 🟡 | 60 min |
| [03-coding-agent.md](tutorials/03-coding-agent.md) | 🛠️ Coding agent (LangGraph) | 🔴 | 45 min |

---

## Related Sections

- [🔵 Local Models](../local-models/README.md) — Ollama, models, tool calling
- [🟢 Zero Level](../basics/README.md) — AI basics, installation
- [💼 Use Cases](../use-cases/README.md) — applying agents

---

**Navigation:** [← Back to main](../README.md) · [📖 Catalog](../catalog.md)
