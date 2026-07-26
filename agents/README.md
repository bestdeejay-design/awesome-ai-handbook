# <img src="https://lucide.dev/api/icons/bot" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> AI Agents

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
| [ready-made.md](ready-made.md) | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#22c55e" stroke="#16a34a" stroke-width="0.5"/></svg> Ready-made AI agents catalog | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#22c55e" stroke="#16a34a" stroke-width="0.5"/></svg> | 7 min |
| [orchestrators.md](orchestrators.md) | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> Sub-agent orchestrators | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> | 6 min |
| [skills.md](skills.md) | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#eab308" stroke="#ca8a04" stroke-width="0.5"/></svg> Skills, MCP servers, plugins | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#eab308" stroke="#ca8a04" stroke-width="0.5"/></svg> | 6 min |
| [architecture.md](architecture.md) | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> AI agent architecture | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> | 11 min |
| [patterns.md](patterns.md) | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> Patterns: ReAct, Multi-Agent, Reflection | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> | 14 min |
| [frameworks.md](frameworks.md) | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> LangGraph, CrewAI, Agno comparison | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> | 12 min |
| [multi-agent.md](multi-agent.md) | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> Multi-agent systems | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> | 18 min |
| [memory.md](memory.md) | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> Long-term agent memory | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> | 9 min |
| [prompting.md](prompting.md) | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> System prompts for agents | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> | 11 min |
| [ollama-for-agents.md](ollama-for-agents.md) | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> Tool calling, JSON schema | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> | 10 min |
| [safety.md](safety.md) | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> Safety, guardrails | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> | 16 min |
| [evaluation.md](evaluation.md) | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> Testing, regression | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> | 10 min |

### Tutorials — `agents/tutorials/`

| File | Description | Level | Time |
|------|-------------|-------|------|
| [README](tutorials/README.md) | Tutorial index | — | — |
| [01-first-agent.md](tutorials/01-first-agent.md) | <img src="https://lucide.dev/api/icons/wrench" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> ReAct agent in Python + Ollama | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#22c55e" stroke="#16a34a" stroke-width="0.5"/></svg> | 30 min |
| [02-agent-team.md](tutorials/02-agent-team.md) | <img src="https://lucide.dev/api/icons/wrench" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> Multi-agent team (CrewAI) | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#eab308" stroke="#ca8a04" stroke-width="0.5"/></svg> | 60 min |
| [03-coding-agent.md](tutorials/03-coding-agent.md) | <img src="https://lucide.dev/api/icons/wrench" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> Coding agent (LangGraph) | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#ef4444" stroke="#dc2626" stroke-width="0.5"/></svg> | 45 min |

---

## Related Sections

- [<svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> Local Models](../local-models/README.md) — Ollama, models, tool calling
- [<svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#22c55e" stroke="#16a34a" stroke-width="0.5"/></svg> Zero Level](../basics/README.md) — AI basics, installation
- [<img src="https://lucide.dev/api/icons/briefcase" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> Use Cases](../use-cases/README.md) — applying agents

---

**Navigation:** [← Back to main](../README.md) · [<img src="https://lucide.dev/api/icons/book-open" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> Catalog](../catalog.md)
