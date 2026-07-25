# 🏗 Agent Architecture

> Core components, agent loop, state management, and orchestration — what makes an agent work.

**🇷🇺 Russian version:** [architecture.ru.md](architecture.ru.md)

---

[← AI agents](README.md) · [Patterns →](patterns.md)

---

## Contents

1. [Agent components](#1-agent-components)
2. [Agent loop](#2-agent-loop)
3. [State and memory](#3-state-and-memory)
4. [Context window](#4-context-window)
5. [Orchestration](#5-orchestration)
6. [Whats next](#6-whats-next)

---

## 1. Agent components

### 1.1 LLM — decision making model

| Role | What it does | Example |
|------|-------------|---------|
| **Reasoner** | Analyzes situation and plans | "User asked about weather. Need to call weather API" |
| **Decider** | Chooses next step | "I have API result — I can answer" |
| **Generator** | Formulates final response | "Its currently +22C and clear in Tokyo" |

Requirements:
- **Tool calling** — model must support function calling (Qwen 3.5+, Llama 3.1+)
- **Structured output** — model must return JSON by schema
- **Long context** — agent loop quickly fills context, need 32K+ tokens

### 1.2 Tools

| Type | Example | Purpose |
|-----|--------|---------|
| **Search** | Web search, vector search | Find current information |
| **Read** | File reading, PDF, websites | Get data for analysis |
| **Write** | File creation, email sending | Act in the system |
| **Compute** | Calculator, Python code | Precise calculations |
| **API** | External service calls | System integration |
| **Communication** | Telegram, Slack, email | Talk to humans |

Each tool is described by JSON Schema — the model "reads" the description and decides when to call it.

```python
tool = {
    "type": "function",
    "function": {
        "name": "search_web",
        "description": "Search the web",
        "parameters": {
            "type": "object",
            "properties": {
                "query": {"type": "string", "description": "Search query"}
            },
            "required": ["query"]
        }
    }
}
```

### 1.3 Orchestrator (control loop)

**This is my job as Sisyphus.** The orchestrator decides:
- When to hand control to a tool
- When to return response to user
- When to stop (step/time/token limit)
- What to do on error (retry, fallback, inform user)

### 1.4 Memory (State)

The agent must remember what it has done. Details in [memory.md](memory.md):

| Memory type | Stores | Example |
|------------|--------|---------|
| **Short-term** | Current dialogue with model | All messages in loop |
| **Long-term** | Information between sessions | Vector DB with projects |
| **Working** | Current task state | Which step is executing |

---

## 2. Agent loop

The fundamental cycle:

1. **User sends request** → system appends to history
2. **LLM receives full history** (system prompt + all previous steps)
3. **LLM decides:** answer text OR call a tool
4. **If tool:** system executes function → result added as observation → back to step 2
5. **If answer:** text returned to user

### Implementation in Python (no frameworks)

```python
import requests, json

def agent_loop(model: str, user_input: str, tools: list, max_steps: int = 10):
    """Simple agent loop."""
    messages = [
        {"role": "system", "content": "You are a helpful assistant. Use tools when needed."},
        {"role": "user", "content": user_input}
    ]

    for step in range(max_steps):
        response = requests.post("http://localhost:11434/api/chat", json={
            "model": model,
            "messages": messages,
            "tools": tools,
            "stream": False
        })
        data = response.json()
        msg = data["message"]
        messages.append(msg)

        if msg.get("tool_calls"):
            for tc in msg["tool_calls"]:
                func_name = tc["function"]["name"]
                args = tc["function"]["arguments"]
                print(f"  Step {step+1}: calling {func_name}({args})")

                result = execute_tool(func_name, args)

                messages.append({
                    "role": "tool",
                    "name": func_name,
                    "content": json.dumps(result)
                })
        else:
            return msg["content"]

    return "Step limit reached"
```

This is literally the core of any agent. LangGraph, CrewAI, Agno all do the same thing internally.

---

## 3. State and memory

Agent state is all information accumulated during its work.

```python
agent_state = {
    "messages": [],
    "step": 0,
    "max_steps": 10,
    "tools_used": [],
    "errors": [],
    "intermediate_results": {},
    "plan": [],
    "user_intent": "",
}
```

### How frameworks manage state

**LangGraph** — `StateGraph` with typed state:
```python
class AgentState(TypedDict):
    messages: Annotated[list, add_messages]
    next_agent: str
```

**CrewAI** — each agent has internal state (role, goal, memory).

**Agno** — `session_state` for data between runs.

### Common issues

| Problem | Why it happens | Solution |
|---------|---------------|----------|
| **Context overflow** | Each step adds tokens | Step limit, compression |
| **Context loss** | Model forgets dialogue start | Summarize history |
| **Contradictory decisions** | Too much data visible | Clean context, keep focus |
| **Data leakage** | State stores sensitive info | Scoping, clear after completion |

---

## 4. Context window

| Action | Tokens (approx) |
|--------|-----------------|
| System prompt | 200–500 |
| User request | 50–200 |
| Model decision (reasoning) | 200–1000 |
| Tool call (JSON) | 100–300 |
| Tool result | 200–5000 |
| **One loop step** | **~500–5000 tokens** |
| 10 steps | **~5000–50000 tokens** |

On M1 16 GB with `num_ctx: 4096` you hit the limit at 3-5 steps.

### Solutions

1. **Increase `num_ctx`** — if it fits in RAM
2. **Limit steps** — `max_steps=5` for most tasks
3. **Context compression** — summarize when too long
4. **KV cache quantization** — `OLLAMA_KV_CACHE_TYPE=q4_0` gives 4× more space

```python
def compress_if_needed(messages, max_messages=20):
    if len(messages) > max_messages:
        system = messages[0]
        recent = messages[-(max_messages-1):]
        return [system] + recent
    return messages
```

---

## 5. Orchestration

**This is what I do as Sisyphus.** One agent is just a loop. Multiple agents working together is orchestration.

**What the orchestrator decides:**
- Which agent takes the task
- Order of work (sequential / parallel / hierarchical)
- How agents exchange data
- What to do if an agent fails
- When the task is complete

```python
def orchestrate(agents: list, task: str):
    result = None
    for agent in agents:
        context = f"Previous result: {result}" if result else ""
        result = agent.run(f"{task}\n{context}")
    return result
```

Real orchestration is more complex — see [multi-agent.md](multi-agent.md) and [02-agent-team tutorial](tutorials/02-agent-team.md).

---

## 6. Whats next

| If you want | Go to |
|-------------|-------|
| Study specific agent patterns (with code) | [patterns.md](patterns.md) |
| How agents store information | [memory.md](memory.md) |
| How it works with frameworks | [frameworks.md](frameworks.md) |
| Write your first agent | [tutorials/01-first-agent.md](tutorials/01-first-agent.md) |
| Back to navigation | [README.md](README.md) |
---

---

**In section:** [architecture](architecture.md) · [evaluation](evaluation.md) · [frameworks](frameworks.md) · [memory](memory.md) · [multi-agent](multi-agent.md) · [ollama-for-agents](ollama-for-agents.md) · [orchestrators](orchestrators.md) · [patterns](patterns.md) · [prompting](prompting.md) · [ready-made](ready-made.md) · [safety](safety.md) · [skills](skills.md)  
**Related sections:** [Zero Level](../basics/README.md) · [Local Models](../local-models/README.md) · [Use Cases](../use-cases/README.md) · [Resources](../resources/README.md)  
**Navigation:** [← AI Agents](README.md) · [↑ Back to main](../README.md) · [🇷🇺 Русский](architecture.ru.md)

