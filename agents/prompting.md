# 🎯 Prompting for AI Agents

> Writing system prompts, tool descriptions, and role definitions for agents.
> 80% of agent success is prompt quality, not the model.

**🇷🇺 Russian version:** [prompting.ru.md](prompting.ru.md)

---

[← AI agents](README.md) · [Memory](memory.md) · [Safety →](safety.md)

---

## Contents

1. [How agent prompting differs](#1-how-agent-prompting-differs)
2. [System prompt structure](#2-system-prompt-structure)
3. [Tool descriptions](#3-tool-descriptions)
4. [Prompts for different roles](#4-prompts-for-different-roles)
5. [Few-shot for agents](#5-few-shot-for-agents)
6. [Anti-patterns](#6-anti-patterns)
7. [Whats next](#7-whats-next)

---

## 1. How agent prompting differs

| Regular prompt | Agent prompt |
|---------------|--------------|
| "Answer briefly" | "If unsure, call a tool — dont guess" |
| "Be polite" | "Explain your plan before each action" |
| "Use facts" | "Always verify facts through tools" |
| "Format: JSON" | "Return structured output by schema" |

---

## 2. System prompt structure

```python
SYSTEM_PROMPT = """
## ROLE
You are a Senior Python Developer.

## TOOLS
You have access to:
- read_file(path) — read a file
- write_file(path, content) — write a file
- run_tests(path) — run tests
- search_code(query) — search code

## RULES
- Always read existing code before changes
- Write tests first (TDD)
- If tests fail, fix them dont ask
- Dont delete code unnecessarily

## BOUNDARIES
- Do NOT modify files outside src/
- Do NOT touch config files (.env, config/*)
- Do NOT use sudo or system commands

## OUTPUT FORMAT
- Explain what youll do
- Show the code
- Run tests at the end
"""
```

---

## 3. Tool descriptions

Bad:
```python
{"name": "search", "description": "Search function"}
```

Good — specify WHEN and WHEN NOT:
```python
{
    "name": "search_web",
    "description": (
        "Search the web via Google. "
        "Use THIS tool when you need current data not in your training set. "
        "EXAMPLE: news, prices, dates, weather, exchange rates. "
        "Do NOT use for general questions (what is Python, capital of France)."
    ),
    "parameters": {
        "type": "object",
        "properties": {
            "query": {"type": "string", "description": "Search query. Be specific."}
        },
        "required": ["query"]
    }
}
```

Rules: specify WHEN to use, WHEN NOT to use, give examples, use descriptive names.

---

## 4. Prompts for different roles

### PM Agent
```python
PM_PROMPT = """
You are a Project Manager. Your team: analyst, developer, tester, DevOps.

YOUR TASKS:
1. Break tasks into subtasks
2. Assign each to the right agent
3. Track deadlines
4. Review results

FORMAT:
[Task]: description [Agent]: name [Deadline]: estimate [Done]: criteria
"""
```

### Developer Agent
```python
DEV_PROMPT = """
You are an experienced developer. Write clean, tested code.

PRINCIPLES:
1. Read existing code first
2. Understand architecture
3. Write tests (TDD)
4. Implement
5. Verify tests pass

STYLE: type hints, docstrings, single responsibility, no TODOs/FIXMEs
"""
```

### QA Agent
```python
QA_PROMPT = """
You are a QA engineer. Find bugs before users do.

PROCESS:
1. Read spec
2. Write test cases
3. Write automated tests
4. Run and verify coverage
5. Report bugs with details

CHECK: edge cases, errors, load, security
"""
```

---

## 5. Few-shot for agents

```python
FEW_SHOT = """
Correct tool call example:

User: What is the weather in Moscow?
You should call search_web(query="weather Moscow today")
and return the result.

Correct:
> Let me check...
> [call search_web]
> Weather data shows +22C in Moscow
"""
```

---

## 6. Anti-patterns

- Too long: 2000 word prompt, agent loses focus → structure with ## sections
- Contradictory: "be creative" + "strictly follow rules" → be unambiguous
- Vague triggers: "use tools when needed" → specify exact conditions
- No boundaries: "make the project better" → "improve test coverage, dont change logic"

---

## 7. Whats next

| If you want | Go to |
|-------------|-------|
| Agent safety (guardrails, limits) | [safety.md](safety.md) |
| Build an agent team | [multi-agent.md](multi-agent.md) |
| Write your first agent | [tutorials/01-first-agent.md](tutorials/01-first-agent.md) |
| Back | [README.md](README.md) |
