# 👥 Multi-Agent Systems

> How multiple agents work together: architectures, communication, coordination.

**🇷🇺 Russian version:** [multi-agent.ru.md](multi-agent.ru.md)

---

[← AI agents](README.md) · [Architecture](architecture.md) · [Tutorial: team →](tutorials/02-agent-team.md)

---

## Contents

1. [Why multiple agents](#1-why-multiple-agents)
2. [Multi-agent system architectures](#2-multi-agent-system-architectures)
3. [Supervisor Pattern](#3-supervisor-pattern)
4. [Handoff Pattern](#4-handoff-pattern)
5. [Swarm Pattern](#5-swarm-pattern)
6. [GroupChat Pattern](#6-groupchat-pattern)
7. [Hierarchical team](#7-hierarchical-team)
8. [Multi-agent system challenges](#8-multi-agent-system-challenges)
9. [Whats next](#9-whats-next)

---

## 1. Why multiple agents

| Single agent problem | Multi-agent solution |
|---|---|
| Context window fills up quickly | Each agent has narrow context |
| Cant be expert in everything | Each agent specializes |
| Sequential execution | Agents work in parallel |
| No self-verification | One writes, another checks |
| Single point of failure | Other agents continue |

---

## 2. Multi-agent system architectures

Handoff: Triage  Sales  Support — agent decides who to pass to

Swarm: All agents equal, shared task queue

GroupChat: All agents discuss round-robin

---

## 3. Supervisor Pattern

One main agent manages specialists. Supervisor decides who does what.

```python
import requests

OLLAMA = "http://localhost:11434/api/chat"
MODEL = "qwen3.5:4b"

class Agent:
    def __init__(self, name: str, instruction: str):
        self.name = name
        self.instruction = instruction

    def run(self, task: str, context: str = "") -> str:
        r = requests.post(OLLAMA, json={
            "model": MODEL,
            "messages": [
                {"role": "system", "content": self.instruction},
                {"role": "user", "content": f"{context}\nTask: {task}"}
            ]
        })
        return r.json()["message"]["content"]

class Supervisor:
    def __init__(self, agents: list[Agent]):
        self.agents = agents

    def run(self, task: str) -> str:
        results = {}
        for a in self.agents:
            ctx = "\n".join(f"{n}: {r}" for n, r in results.items())
            results[a.name] = a.run(task, ctx)
        prompt = f"Task: {task}\n\nResults:\n" + "\n".join(f"{n}: {r}" for n, r in results.items()) + "\n\nCompile a summary."
        r = requests.post(OLLAMA, json={
            "model": MODEL,
            "messages": [{"role": "system", "content": "You are team lead. Summarize."}, {"role": "user", "content": prompt}]
        })
        return r.json()["message"]["content"]
```

---

## 4. Handoff Pattern

Agent passes task to another when out of scope.

```python
class HandoffAgent:
    def __init__(self, name, desc, prompt, can_handoff_to=None):
        self.name = name; self.desc = desc; self.prompt = prompt
        self.can_handoff_to = can_handoff_to or []

    def run(self, task):
        agents_desc = "\n".join(f"- {a.name}: {a.desc}" for a in self.can_handoff_to)
        extra = f"\n\nIf NOT your task, write HANDOFF: name\nAgents:\n{agents_desc}" if agents_desc else ""
        r = requests.post(OLLAMA, json={
            "model": "qwen3.5:4b",
            "messages": [{"role": "system", "content": self.prompt + extra}, {"role": "user", "content": task}]
        })
        content = r.json()["message"]["content"]
        if content.startswith("HANDOFF:"):
            return None, content.split(":")[1].strip()
        return content, None
```

---

## 5. Swarm Pattern

```python
from queue import Queue
import threading

class SwarmWorker(threading.Thread):
    def __init__(self, name, role, task_q, result_q):
        super().__init__()
        self.name = name; self.role = role
        self.task_q = task_q; self.result_q = result_q

    def run(self):
        while True:
            task = self.task_q.get()
            if task is None: break
            r = requests.post(OLLAMA, json={
                "model": "qwen3.5:4b",
                "messages": [{"role": "system", "content": f"You are {self.name}. {self.role}"}, {"role": "user", "content": task}]
            })
            self.result_q.put((self.name, task, r.json()["message"]["content"]))
            self.task_q.task_done()
```

---

## 6. GroupChat Pattern

All agents discuss in round-robin.

---

## 7. Hierarchical team

Using CrewAI with hierarchical process:
```python
from crewai import Agent, Task, Crew, Process, LLM
llm = LLM(model="ollama/qwen3.5:4b")
analyst = Agent(role="Analyst", goal="Gather requirements", llm=llm)
dev = Agent(role="Developer", goal="Implement code", llm=llm)
tester = Agent(role="QA", goal="Find bugs", llm=llm)
crew = Crew(agents=[analyst, dev, tester], process=Process.hierarchical, manager_llm=llm)
```

---

## 8. Challenges

| Problem | Solution |
|---------|----------|
| Disagreement | Supervisor veto, voting |
| Error cascade | Validate each step, retry |
| State inconsistency | Shared state, regular sync |

When NOT to use: 1-2 step tasks, no clear expertise split needed.

---

## 9. Whats next

| If you want | Go to |
|-------------|-------|
| Build your agent team | [tutorials/02-agent-team.md](tutorials/02-agent-team.md) |
| Agent memory | [memory.md](memory.md) |
| Prompting | [prompting.md](prompting.md) |
| Safety | [safety.md](safety.md) |
| Back | [README.md](README.md) |
---

---

**In section:** [architecture](architecture.md) · [evaluation](evaluation.md) · [frameworks](frameworks.md) · [memory](memory.md) · [multi-agent](multi-agent.md) · [ollama-for-agents](ollama-for-agents.md) · [orchestrators](orchestrators.md) · [patterns](patterns.md) · [prompting](prompting.md) · [ready-made](ready-made.md) · [safety](safety.md) · [skills](skills.md)  
**Related sections:** [Zero Level](../basics/README.md) · [Local Models](../local-models/README.md) · [Use Cases](../use-cases/README.md) · [Resources](../resources/README.md)  
**Navigation:** [← AI Agents](README.md) · [↑ Back to main](../README.md) · [🇷🇺 Русский](multi-agent.ru.md)

