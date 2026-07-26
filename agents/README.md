# <img src="https://lucide.dev/api/icons/bot" alt="" width="20" height="20" style="display:inline;vertical-align:middle"> AI Agents

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
| [ready-made.md](ready-made.md) | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> Ready-made AI agents catalog | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> | 7 min |
| [orchestrators.md](orchestrators.md) | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> Sub-agent orchestrators | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> | 6 min |
| [skills.md](skills.md) | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-yellow.svg" alt="" width="12" height="12" style="vertical-align:middle"> Skills, MCP servers, plugins | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-yellow.svg" alt="" width="12" height="12" style="vertical-align:middle"> | 6 min |
| [architecture.md](architecture.md) | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> AI agent architecture | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> | 11 min |
| [patterns.md](patterns.md) | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> Patterns: ReAct, Multi-Agent, Reflection | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> | 14 min |
| [frameworks.md](frameworks.md) | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> LangGraph, CrewAI, Agno comparison | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> | 12 min |
| [multi-agent.md](multi-agent.md) | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> Multi-agent systems | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> | 18 min |
| [memory.md](memory.md) | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> Long-term agent memory | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> | 9 min |
| [prompting.md](prompting.md) | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> System prompts for agents | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> | 11 min |
| [ollama-for-agents.md](ollama-for-agents.md) | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> Tool calling, JSON schema | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> | 10 min |
| [safety.md](safety.md) | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> Safety, guardrails | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> | 16 min |
| [evaluation.md](evaluation.md) | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> Testing, regression | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> | 10 min |

### Tutorials — `agents/tutorials/`

| File | Description | Level | Time |
|------|-------------|-------|------|
| [README](tutorials/README.md) | Tutorial index | — | — |
| [01-first-agent.md](tutorials/01-first-agent.md) | <img src="https://lucide.dev/api/icons/wrench" alt="" width="20" height="20" style="display:inline;vertical-align:middle"> ReAct agent in Python + Ollama | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> | 30 min |
| [02-agent-team.md](tutorials/02-agent-team.md) | <img src="https://lucide.dev/api/icons/wrench" alt="" width="20" height="20" style="display:inline;vertical-align:middle"> Multi-agent team (CrewAI) | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-yellow.svg" alt="" width="12" height="12" style="vertical-align:middle"> | 60 min |
| [03-coding-agent.md](tutorials/03-coding-agent.md) | <img src="https://lucide.dev/api/icons/wrench" alt="" width="20" height="20" style="display:inline;vertical-align:middle"> Coding agent (LangGraph) | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-red.svg" alt="" width="12" height="12" style="vertical-align:middle"> | 45 min |

---

## Related Sections

- [<img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> Local Models](../local-models/README.md) — Ollama, models, tool calling
- [<img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> Zero Level](../basics/README.md) — AI basics, installation
- [<img src="https://lucide.dev/api/icons/briefcase" alt="" width="20" height="20" style="display:inline;vertical-align:middle"> Use Cases](../use-cases/README.md) — applying agents

---

**Navigation:** [← Back to main](../README.md) · [<img src="https://lucide.dev/api/icons/book-open" alt="" width="20" height="20" style="display:inline;vertical-align:middle"> Catalog](../catalog.md)
