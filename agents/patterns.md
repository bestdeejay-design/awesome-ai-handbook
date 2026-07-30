# <img src="https://lucide.dev/api/icons/puzzle" alt="" width="20" height="20" style="vertical-align:middle"> AI Agent Patterns

> Architectural patterns for building agents on top of LLMs — from simple ReAct to multi-agent systems with code.

**🇷🇺 Russian version:** [patterns.ru.md](patterns.ru.md)

---

[← AI agents](README.md) · [Architecture](architecture.md) · [Frameworks →](frameworks.md)

---

## Contents

1. [ReAct (Reason + Act)](#1-react-reason--act)
2. [Plan-and-Execute](#2-plan-and-execute)
3. [Reflection / Self-Correction](#3-reflection--self-correction)
4. [Tool Use (Function Calling)](#4-tool-use-function-calling)
5. [Multi-Agent](#5-multi-agent)
6. [How to choose a pattern](#6-how-to-choose-a-pattern)
7. [Whats next](#7-whats-next)

---

## 1. ReAct (Reason + Act)

**The most popular pattern.** The agent alternates between reasoning (Reason) and action (Act) in a loop.

### How it works

```
Thought:  The user is asking about weather. I need to find information.
Action:   call search_web(query="weather Tokyo")
Observation: "In Tokyo +22C, clear"
Thought:  I have the information. I can answer.
Answer:   Its currently +22C and clear in Tokyo.
```

Each cycle: model **thinks** what to do **does** (calls a tool or responds) receives **result** thinks again.

### When ReAct
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Tasks that need information search
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Questions requiring calculations
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Simple action chains (find → analyze → answer)
- <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> Long-plan tasks (use Plan-and-Execute instead)

### Implementation

```python
import requests
import json

OLLAMA = "http://localhost:11434/api/chat"

def react_agent(task: str, tools: list, max_steps: int = 5):
    """
    ReAct agent: alternates reasoning and actions.
    """
    messages = [
        {"role": "system", "content": (
            "You are a ReAct agent. Answer user queries. "
            "If you need information use tools. "
            "Only answer when you have enough data."
        )},
        {"role": "user", "content": task}
    ]

    for step in range(max_steps):
        response = requests.post(OLLAMA, json={
            "model": "qwen3.5:4b",
            "messages": messages,
            "tools": tools,
            "stream": False
        })
        msg = response.json()["message"]
        messages.append(msg)

        if msg.get("tool_calls"):
            for tc in msg["tool_calls"]:
                name = tc["function"]["name"]
                args = tc["function"]["arguments"]
                print(f"  [Step {step+1}] {name}({json.dumps(args)})")

                result = {"result": f"executed {name} with {args}"}
                messages.append({
                    "role": "tool",
                    "name": name,
                    "content": json.dumps(result)
                })
        else:
            return msg["content"]

    return "Failed to complete the task in the given steps"
```

### Pros and Cons

| Pros | Cons |
|-------|--------|
| Simple to understand and implement | Can loop infinitely |
| Works with any model | Each step consumes tokens |
| Transparent you see each decision | No forward planning |

ReAct fits **executor agents**: analyst finds data, developer reads docs, tester checks results. Each works in its own loop.

---

## 2. Plan-and-Execute

Separates planning from execution in two phases.

### How it works

```
STEP 1 Planning:
Plan:
  1. Find the project repository on GitHub
  2. Read the README
  3. Collect dependencies
  4. Generate report

STEP 2 Execution:
  Executing step 1: search_web("repo awesome-ai-handbook")
  Result: found github.com/bestdeejay-design/awesome-ai-handbook
  Executing step 2: read_file("https://github.com/.../README.md")
  ...
```

### When Plan-and-Execute
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Complex tasks with 3+ steps
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Code migration, refactoring
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Research tasks
- <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> Simple questions (ReAct is faster)

### Implementation

```python
def plan_and_execute(task: str, tools: list):
    """Plan-and-Execute: plan first then execute."""

    # Phase 1: create plan
    plan_prompt = (
        f"Create a detailed plan for: {task}\n"
        "Return the plan as a list of steps in JSON format.\n"
        "Each step: {\"step\": \"description\", \"tool\": \"tool_name\"}"
    )

    plan_response = requests.post(OLLAMA, json={
        "model": "qwen3.5:4b",
        "messages": [{"role": "user", "content": plan_prompt}],
        "format": {
            "type": "object",
            "properties": {
                "steps": {
                    "type": "array",
                    "items": {
                        "type": "object",
                        "properties": {
                            "step": {"type": "string"},
                            "tool": {"type": "string"}
                        }
                    }
                }
            }
        },
        "stream": False
    })

    plan = json.loads(plan_response.json()["message"]["content"])
    print(f"Plan: {len(plan['steps'])} steps")

    # Phase 2: execute steps
    results = []
    for i, step in enumerate(plan["steps"]):
        print(f"  Step {i+1}: {step['step']}")

        response = requests.post(OLLAMA, json={
            "model": "qwen3.5:4b",
            "messages": [
                {"role": "system", "content": (
                    f"You are executing step {i+1} of the plan. "
                    f"Context: {step['step']}. "
                    f"Previous results: {json.dumps(results)}"
                )},
                {"role": "user", "content": step['step']}
            ],
            "stream": False
        })
        result = response.json()["message"]["content"]
        results.append({"step": i+1, "result": result})

    return results
```

### Pros and Cons

| Pros | Cons |
|-------|--------|
| Agent doesnt forget the goal | Bad plan = bad result |
| Clear progress tracking | Cant adapt mid-execution |
| Plan can be reused | Uses tokens on planning |

Plan-and-Execute is ideal for **project manager agents**: create plan, distribute tasks, track progress.

---

## 3. Reflection / Self-Correction

The agent generates a response, critiques it, and improves it.

### How it works

```
PASS 1 Generation:
  Answer: Python was created in 1991 by Guido van Rossum

PASS 2 Reflection:
  Critique: Answer is correct but could add version and context

PASS 3 Improvement:
  Final: Python was created by Guido van Rossum in 1991.
  Version 0.9.0 was released on February 20, 1991. Today Python
  is one of the most popular programming languages.
```

### When Reflection
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Writing text (articles, docs)
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Code that needs review before use
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Complex reasoning
- <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> Simple answers (overkill)

### Implementation

```python
def reflection_agent(task: str, iterations: int = 2):
    """Reflection agent: generate critique improve."""

    messages = [
        {"role": "system", "content": "You are an expert. Answer questions."},
        {"role": "user", "content": task}
    ]

    # Initial generation
    response = requests.post(OLLAMA, json={
        "model": "qwen3.5:4b",
        "messages": messages,
        "stream": False
    })
    answer = response.json()["message"]["content"]

    for i in range(iterations):
        # Critique
        critique = requests.post(OLLAMA, json={
            "model": "qwen3.5:4b",
            "messages": [
                {"role": "system", "content": (
                    "You are a strict critic. Find errors inaccuracies "
                    "and missing details. Be picky."
                )},
                {"role": "user", "content": f"Task: {task}\n\nAnswer: {answer}"}
            ],
            "stream": False
        })
        feedback = critique.json()["message"]["content"]

        # Improvement
        improved = requests.post(OLLAMA, json={
            "model": "qwen3.5:4b",
            "messages": [
                {"role": "system", "content": (
                    "Improve the answer based on the critique. Fix all "
                    "errors add details. Return only the final version."
                )},
                {"role": "user", "content": (
                    f"Original answer: {answer}\n\n"
                    f"Critique: {feedback}\n\n"
                    f"Improved answer:"
                )}
            ],
            "stream": False
        })
        answer = improved.json()["message"]["content"]

    return answer
```

### Pros and Cons

| Pros | Cons |
|-------|--------|
| Significantly better quality | Uses 23x more tokens |
| Catches errors the model missed | Can over-improve and break |
| Useful for important answers | Not needed for simple tasks |

Reflection is a pattern for **reviewer agents**. They check code, text, and decisions before they go further. In your team: developer writes code → QA tests → reviewer evaluates quality.

---

## 4. Tool Use (Function Calling)

A single tool invocation without the ReAct thinking loop.

### Tool Use vs ReAct

| Tool Use | ReAct |
|----------|-------|
| One tool call | Loop of multiple calls |
| No reasoning between calls | Thought between Action |
| Simpler fewer tokens | More complex but flexible |

### When Tool Use
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Extracting data from text
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Classification (choose a category)
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> JSON conversion
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> When guaranteed output format is needed

### Implementation

```python
def tool_call(model: str, user_input: str, tools: list):
    """Single tool call without loop."""
    response = requests.post(OLLAMA, json={
        "model": model,
        "messages": [{"role": "user", "content": user_input}],
        "tools": tools,
        "stream": False
    })
    return response.json()["message"]
```

Detailed API guide in [ollama-for-agents.md](ollama-for-agents.md).

---

## 5. Multi-Agent

Multiple agents working together on a single task.

### Architectures

```
Supervisor (hierarchical):
  ┌────────────┐
  │ Supervisor │  ← manages
  └───┬───┬────┘
      │   │
  ┌───┘   └───┐
  ▼           ▼
┌──────┐  ┌──────┐
│Agent │  │Agent │
│  A   │  │  B   │
└──────┘  └──────┘

Peer-to-Peer (horizontal):
  ┌──────┐     ┌──────┐
  │Agent │◄───►│Agent │
  │  A   │     │  B   │
  └──────┘     └──────┘
       ▲           ▲
       └─────┬─────┘
             ▼
        ┌────────┐
        │ Shared │
        │ task   │
        └────────┘

Swarm:
  Agents dynamically hand off tasks
  Agent A → (can't) → Agent B → (done) → Agent C → ...
```

### When Multi-Agent
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Tasks requiring different expertise (coding + design + DevOps)
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Large projects needing decomposition
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> When review and balance is needed (one writes, another checks)
- <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> Simple tasks (single agent is faster)

### Implementation

```python
class Agent:
    """Simple agent with a role."""

    def __init__(self, name: str, role: str, model: str = "qwen3.5:4b"):
        self.name = name
        self.role = role
        self.model = model

    def run(self, task: str, context: str = "") -> str:
        response = requests.post(OLLAMA, json={
            "model": self.model,
            "messages": [
                {"role": "system", "content": (
                    f"You are {self.name}. Your role: {self.role}."
                )},
                {"role": "user", "content": f"{context}\n\nTask: {task}"}
            ],
            "stream": False
        })
        return response.json()["message"]["content"]


class Supervisor:
    """Manages a team of agents."""

    def __init__(self, agents: list[Agent]):
        self.agents = agents

    def run(self, project_task: str):
        context = ""
        for agent in self.agents:
            print(f"\n  {agent.name} working...")
            result = agent.run(project_task, context)
            print(f"    Result: {result[:100]}...")
            context += f"\n{agent.name}: {result}"
        return context


# Example: project analysis team
pm = Agent("PM", "Project manager. Set tasks and check deadlines.")
analyst = Agent("Analyst", "Analyze requirements and write specs.")
dev = Agent("Developer", "Write code per specification.")

team = Supervisor([pm, analyst, dev])
result = team.run("Create a REST API for task management")
```

Detailed guide in [multi-agent.md](multi-agent.md) and [02-agent-team tutorial](tutorials/02-agent-team.md).

---

## 6. How to choose a pattern

```python
def choose_pattern(task: str):
    """Heuristic: which pattern fits the task."""
    if len(task.split()) < 10:
        return "Tool Use"
    elif "check" in task.lower() or "improve" in task.lower():
        return "Reflection"
    elif any(w in task.lower() for w in ["find", "search", "how many", "what"]):
        return "ReAct"
    elif any(w in task.lower() for w in ["plan", "make project", "develop"]):
        return "Plan-and-Execute"
    elif any(w in task.lower() for w in ["team", "agents", "distribute"]):
        return "Multi-Agent"
    else:
        return "ReAct"
```

| Situation | Pattern |
|-----------|---------|
| "What is 2 + 2?" | Tool Use (calculator) |
| "Find information about..." | ReAct (search answer) |
| "Write an article about..." | Reflection (draft review) |
| "Refactor this module" | Plan-and-Execute (plan steps) |
| "Start a project from scratch" | Multi-Agent (team) |

---

## 7. Whats next

| If you want | Go to |
|-------------|-------|
| Choose a framework for the pattern | [frameworks.md](frameworks.md) |
| Connect local model (tool calling API) | [ollama-for-agents.md](ollama-for-agents.md) |
| Understand agent architecture deeper | [architecture.md](architecture.md) |
| Build an agent team | [multi-agent.md](multi-agent.md) |
| Write your first agent | [tutorials/01-first-agent.md](tutorials/01-first-agent.md) |
| Back to navigation | [README.md](README.md) |
---

---

**In section:** [architecture](architecture.md) · [evaluation](evaluation.md) · [frameworks](frameworks.md) · [memory](memory.md) · [multi-agent](multi-agent.md) · [ollama-for-agents](ollama-for-agents.md) · [orchestrators](orchestrators.md) · [patterns](patterns.md) · [prompting](prompting.md) · [ready-made](ready-made.md) · [safety](safety.md) · [skills](skills.md)  
**Related sections:** [Zero Level](../basics/README.md) · [Local Models](../local-models/README.md) · [Use Cases](../use-cases/README.md) · [Resources](../resources/README.md)  
**Navigation:** [← AI Agents](README.md) · [↑ Back to main](../README.md) · [🇷🇺 Русский](patterns.ru.md)

