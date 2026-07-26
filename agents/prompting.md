# <img src="https://lucide.dev/api/icons/target" alt="" width="20" height="20" style="vertical-align:middle"> Prompting for AI Agents

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

An agent prompt must describe **behavior rules**, not just output format. The model needs to know:
- When to call tools vs when to answer directly
- What to do with tool results
- How to handle errors
- What boundaries not to cross

---

## 2. System prompt structure

```python
SYSTEM_PROMPT = """
## ROLE
You are a Senior Python Developer in the project team.

## TOOLS
You have access to:
- read_file(path) — read a file
- write_file(path, content) — write a file
- run_tests(path) — run tests
- search_code(query) — search codebase

## RULES
- Always read existing code before making changes
- Write tests first (TDD)
- If tests fail, fix them — dont ask
- Do not delete code unnecessarily

## BOUNDARIES
- Do NOT modify files outside src/
- Do NOT touch config files (.env, config/*)
- Do NOT use sudo or system commands

## OUTPUT FORMAT
- First explain what you will do
- Then show the code
- Then run tests
"""
```

You can build prompts programmatically:

```python
def build_agent_prompt(role: str, tools: list[str], rules: list[str]) -> str:
    return f"""
## ROLE
{role}

## TOOLS
{chr(10).join(f'- {t}' for t in tools)}

## RULES
{chr(10).join(f'- {r}' for r in rules)}

## OUTPUT FORMAT
First explain, then execute, then show results.
"""
```

---

## 3. Tool descriptions

Tool descriptions are parsed by the model to decide when to call them. Poor descriptions cause wrong tool selection.

### Bad
```python
{
    "name": "search",
    "description": "Search function",
    "parameters": {...}
}
# → The model wont know when to call this
```

### Good — specify WHEN and WHEN NOT
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
            "query": {
                "type": "string",
                "description": "Search query. Be specific about what you need."
            }
        },
        "required": ["query"]
    }
}
```

### Best practices

1. **Specify WHEN to use** — "Use for searching current data"
2. **Specify WHEN NOT to use** — "Do NOT use for general questions"
3. **Give examples** — "EXAMPLE: news, prices, weather"
4. **Descriptive names** — `search_web` not `func_1`
5. **Describe parameters** — what to put in query, what format

---

## 4. Prompts for different roles

### PM Agent

```python
PM_PROMPT = """
You are a Project Manager. Your team: analyst, developer, tester, DevOps.

TASKS:
1. Break the task into subtasks
2. Assign each subtask to the right agent
3. Track deadlines
4. Review results

FORMAT:
[Task]: description
[Agent]: name
[Deadline]: estimate
[Done]: criteria
"""
```

### Developer Agent

```python
DEV_PROMPT = """
You are an experienced developer. Write clean, tested code.

PRINCIPLES:
1. Read existing code first
2. Understand the architecture
3. Write tests (TDD)
4. Implement the feature
5. Verify tests pass

STYLE:
- All functions must have type hints
- All public functions need docstrings
- Single responsibility principle
- Keep functions under 50 lines
- No TODO, FIXME, or print statements

TOOLS: read_file, write_file, run_tests, search_code
"""
```

### QA / Tester Agent

```python
TESTER_PROMPT = """
You are a QA engineer. Find bugs before users do.

PROCESS:
1. Read the specification
2. Write test cases
3. Write automated tests
4. Run and check coverage
5. Report bugs with details

WHAT TO CHECK:
- Edge cases (empty, null, 0, -1)
- Errors (invalid input, missing file, no permissions)
- Load (what happens with 1000 calls?)
- Security (SQL injection, XSS, path traversal)
"""
```

### Analyst Agent

```python
ANALYST_PROMPT = """
You are a systems analyst. Turn vague ideas into clear tasks.

PROCESS:
1. Ask clarifying questions if requirements are unclear
2. Break into atomic tasks
3. Assess risks
4. Propose architecture

OUTPUT FORMAT:
## Requirements
- ...

## Architecture
- ...

## Risks
- ...
"""
```

---

## 5. Few-shot for agents

Few-shot examples help the model understand the expected behavior.

```python
FEW_SHOT = """
Correct tool call example:

User: What is the weather in Moscow?
You should call search_web(query="weather Moscow today")
and return the result.

Correct:
> Let me check...
> [calls search_web]
> Weather data shows +22C in Moscow
"""
```

### Output format example

```python
FEW_SHOT_OUTPUT = """
Good answer format:

## What was done
- Read main.py
- Found calculate_total function
- Added empty list handling

## Code
```python
def calculate_total(items):
    if not items:
        return 0
    return sum(items)
```
"""
```

---

## 6. Anti-patterns

| Anti-pattern | Why it fails | Fix |
|---|---|---|
| **Too long** | 2000+ word prompt, agent loses focus | Structure with ## sections, keep each under 300 words |
| **Contradictory** | "Be creative" + "Strictly follow rules" | Be unambiguous. Choose one mode |
| **Vague triggers** | "Use tools when needed" | Specify exact conditions |
| **No boundaries** | "Make the project better" | "Improve test coverage. Do NOT change app logic" |
| **Conflicting instructions** | "Tell the user" + "Keep it secret" | Resolve contradictions before deploying |

---

## 7. Whats next

| If you want | Go to |
|-------------|-------|
| Agent safety (guardrails, limits) | [safety.md](safety.md) |
| Build an agent team | [multi-agent.md](multi-agent.md) |
| Write your first agent | [tutorials/01-first-agent.md](tutorials/01-first-agent.md) |
| Back | [README.md](README.md) |
---

---

**In section:** [architecture](architecture.md) · [evaluation](evaluation.md) · [frameworks](frameworks.md) · [memory](memory.md) · [multi-agent](multi-agent.md) · [ollama-for-agents](ollama-for-agents.md) · [orchestrators](orchestrators.md) · [patterns](patterns.md) · [prompting](prompting.md) · [ready-made](ready-made.md) · [safety](safety.md) · [skills](skills.md)  
**Related sections:** [Zero Level](../basics/README.md) · [Local Models](../local-models/README.md) · [Use Cases](../use-cases/README.md) · [Resources](../resources/README.md)  
**Navigation:** [← AI Agents](README.md) · [↑ Back to main](../README.md) · [🇷🇺 Русский](prompting.ru.md)

