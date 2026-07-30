# <img src="https://lucide.dev/api/icons/users" alt="" width="20" height="20" style="vertical-align:middle"> Multi-Agent Systems

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

**Your project** — an ideal use case for a multi-agent system:
- PM agent assigns tasks and tracks deadlines
- Analyst gathers requirements
- Developer writes code
- QA verifies
- DevOps deploys
- Designer creates interfaces

---

## 2. Multi-agent system architectures

```
Supervisor (hierarchical):
  ┌──────────────┐
  │  Supervisor  │  ← central coordinator
  └──┬──┬──┬──┬──┘
     │  │  │  │
  ┌──┘  │  │  └──┐
  ▼     ▼  ▼     ▼
┌────┐┌────┐┌────┐┌────┐
│ A1  ││ A2  ││ A3  ││ A4  │  ← specialists
└────┘└────┘└────┘└────┘

Handoff (handover):
  ┌──────┐   ┌──────┐   ┌──────┐
  │ Triage│──▶│ Sales│──▶│Support│
  └──────┘   └──────┘   └──────┘
  Agent decides who to pass the task to

Swarm (peer-based):
  Agents are equal, each can take any task
  ┌──────┐
  │  A1  │◀───┐
  └──────┘    │
  ┌──────┐    │
  │  A2  │◀───┼─── Shared task queue
  └──────┘    │
  ┌──────┐    │
  │  A3  │◀───┘
  └──────┘

GroupChat (discussion):
  All agents in one chat, discuss and reach a decision
  ┌──────┐    ┌──────┐
  │  A1  │◀──▶│  A2  │
  └──────┘    └──────┘
      ▲            ▲
      │    ┌──────┐ │
      └────│  A3  │─┘
           └──────┘
```

---

## 3. Supervisor Pattern

One main agent manages specialists. Supervisor decides who does what.

### How it works

```
User: "Research the AI agent market"

Supervisor:
  └─▶ Analyst: gather market data
  └─▶ Writer: format a report based on the data
  └─▶ Designer: create diagrams

Supervisor collects results → final answer
```

### Implementation

```python
import requests

OLLAMA = "http://localhost:11434/api/chat"
MODEL = "qwen3.5:4b"

class Agent:
    """A specialist agent with a defined role and instruction."""

    def __init__(self, name: str, role: str = "", instruction: str = ""):
        self.name = name
        self.role = role
        self.instruction = instruction

    def run(self, task: str, context: str = "") -> str:
        response = requests.post(OLLAMA, json={
            "model": MODEL,
            "messages": [
                {"role": "system", "content": self.instruction},
                {"role": "user", "content": f"{context}\nTask: {task}"}
            ],
            "stream": False
        })
        return response.json()["message"]["content"]

class Supervisor:
    """Supervisor — manages a team of specialist agents."""

    def __init__(self, agents: list[Agent]):
        self.agents = agents

    def run(self, task: str) -> str:
        results = {}

        for agent in self.agents:
            print(f"  ▶ {agent.name} working...")
            context = "\n".join(f"{name}: {res}" for name, res in results.items())
            result = agent.run(task, context)
            results[agent.name] = result
            print(f"    ✓ {agent.name} completed")

        # Supervisor compiles the final answer
        summary_prompt = (
            f"Task: {task}\n\n"
            f"Team results:\n" +
            "\n".join(f"{name}: {res}" for name, res in results.items()) +
            "\n\nCompile a summary report based on everyone's work."
        )

        response = requests.post(OLLAMA, json={
            "model": MODEL,
            "messages": [
                {"role": "system", "content": "You are the team lead. Collect results into a single report."},
                {"role": "user", "content": summary_prompt}
            ],
            "stream": False
        })
        return response.json()["message"]["content"]


# Example: analysis team
analyst = Agent(
    "Analyst", "Market Researcher",
    "You are an analyst. Find data, form hypotheses, draw conclusions."
)
writer = Agent(
    "Writer", "Technical Author",
    "You are a writer. Create clear, structured texts."
)
designer = Agent(
    "Designer", "Data Visualizer",
    "You are a designer. Suggest how to visualize data."
)

supervisor = Supervisor([analyst, writer, designer])
result = supervisor.run("Analyze the current state of the AI agent market")
print(f"\n📋 Result:\n{result}")
```

### Pros and Cons

| Pros | Cons |
|------|------|
| Clear role distribution | Supervisor is a single point of failure |
| Simple coordination logic | Supervisor may misassign tasks |
| Easy to add new agents | Sequential execution (can be made parallel) |

---

## 4. Handoff Pattern

Agent passes task to another when out of scope.

### How it works

```
User: "I want to order a pizza"

Triage agent:
  → Is this about food ordering? → Hand off to Orders agent
  → Is this about returns? → Hand off to Support agent
  → Neither → Answer myself
```

### Implementation

```python
class HandoffAgent:
    """An agent that can hand off a task to another agent."""

    def __init__(self, name, desc, prompt, can_handoff_to=None):
        self.name = name
        self.desc = desc
        self.prompt = prompt
        self.can_handoff_to = can_handoff_to or []

    def run(self, task):
        """Returns (response, next_agent_or_None)."""
        agents_desc = "\n".join(f"- {a.name}: {a.desc}" for a in self.can_handoff_to)
        extra = (
            f"\n\nIf NOT your task, write HANDOFF: name\nAgents:\n{agents_desc}"
        ) if agents_desc else ""
        r = requests.post(OLLAMA, json={
            "model": "qwen3.5:4b",
            "messages": [{"role": "system", "content": self.prompt + extra}, {"role": "user", "content": task}],
            "stream": False
        })
        content = r.json()["message"]["content"]
        if content.startswith("HANDOFF:"):
            return None, content.split(":")[1].strip()
        return content, None


class HandoffOrchestrator:
    """Orchestrator for handoff agents — routes tasks through the chain."""

    def __init__(self, triage_agent):
        self.triage_agent = triage_agent
        self.agents_map = {}
        self._build_map(triage_agent)

    def _build_map(self, agent):
        self.agents_map[agent.name] = agent
        for child in (agent.can_handoff_to or []):
            self._build_map(child)

    def run(self, task: str, max_hops: int = 5) -> str:
        current = self.triage_agent

        for hop in range(max_hops):
            result, next_name = current.run(task)
            if next_name:
                if next_name in self.agents_map:
                    print(f"  ▶ {current.name} → {next_name}")
                    current = self.agents_map[next_name]
                else:
                    return f"Error: agent {next_name} not found"
            else:
                return result

        return "Too many handoffs between agents"
```

---

## 5. Swarm Pattern

**Agents work as a "swarm" — each can pick up a task from the shared queue.** Suitable for scenarios where tasks are independent and can run in parallel.

```python
from queue import Queue
import threading

class SwarmWorker(threading.Thread):
    """A worker agent in the swarm — runs in its own thread."""

    def __init__(self, name, role, task_q, result_q):
        super().__init__()
        self.name = name
        self.role = role
        self.task_q = task_q
        self.result_q = result_q

    def run(self):
        while True:
            task = self.task_q.get()
            if task is None:
                break
            print(f"  ▶ {self.name} picked up: {task[:50]}...")
            r = requests.post(OLLAMA, json={
                "model": "qwen3.5:4b",
                "messages": [{"role": "system", "content": f"You are {self.name}. {self.role}"}, {"role": "user", "content": task}],
                "stream": False
            })
            self.result_q.put((self.name, task, r.json()["message"]["content"]))
            self.task_q.task_done()


class Swarm:
    """Orchestrator for a swarm of agents."""

    def __init__(self, workers: list[SwarmWorker]):
        self.workers = workers

    def run(self, tasks: list[str]) -> dict:
        task_q = Queue()
        result_q = Queue()

        for w in self.workers:
            w.task_q = task_q
            w.result_q = result_q
            w.start()

        for task in tasks:
            task_q.put(task)

        task_q.join()

        for _ in self.workers:
            task_q.put(None)

        results = {}
        while not result_q.empty():
            name, task, result = result_q.get()
            results[name] = result

        return results
```

---

## 6. GroupChat Pattern

**All agents in one "chat" exchange messages until they reach a solution.** Used for discussions, brainstorming, collective decision-making.

```python
class GroupChat:
    """Round-robin group discussion among agents with summarization."""

    def __init__(self, agents: list, max_rounds: int = 3):
        self.agents = agents
        self.max_rounds = max_rounds
        self.messages = []

    def run(self, topic: str) -> str:
        self.messages = [
            {"role": "system", "content": (
                f"Group discussion: {topic}\n"
                f"Participants: {', '.join(a.name for a in self.agents)}.\n"
                f"Each participant speaks in turn."
            )}
        ]

        for round_num in range(self.max_rounds):
            print(f"\n  📢 Round {round_num + 1}:")

            for agent in self.agents:
                response = requests.post(OLLAMA, json={
                    "model": "qwen3.5:4b",
                    "messages": [
                        {"role": "system", "content": (
                            f"You are {agent.name}. {agent.role}. "
                            f"Participate in the discussion. React to other participants. "
                            f"If you agree with someone — support them. If not — argue your point."
                        )},
                        *self.messages[-6:],
                        {"role": "user", "content": f"Your turn, {agent.name}. What do you think?"}
                    ],
                    "stream": False
                })

                reply = response.json()["message"]["content"]
                self.messages.append({
                    "role": "assistant",
                    "content": f"{agent.name}: {reply}"
                })
                print(f"    {agent.name}: {reply[:80]}...")

        # Summarize the discussion
        summary = requests.post(OLLAMA, json={
            "model": "qwen3.5:4b",
            "messages": [
                {"role": "system", "content": "Summarize the discussion: what decisions were made, what arguments were presented."},
                {"role": "user", "content": "\n".join(m["content"] for m in self.messages[-10:])}
            ],
            "stream": False
        })
        return summary.json()["message"]["content"]
```

---

## 7. Hierarchical team

This pattern combines **Supervisor + specialists** into a hierarchy where each level manages its own subordinates.

```
                     ┌──────────────┐
                     │  Orchestrator │  ← main coordinator
                     │   (SuperPM)   │
                     └──┬──┬──┬──┬──┘
                        │  │  │  │
         ┌──────────────┘  │  │  └──────────────┐
         ▼                 ▼  ▼                 ▼
   ┌──────────┐    ┌──────────┐    ┌──────────┐
   │  Analyst  │    │ Architect│    │   DevOps  │
   └──────────┘    └────┬─────┘    └──────────┘
                        │
          ┌─────────────┼─────────────┐
          ▼             ▼             ▼
    ┌──────────┐  ┌──────────┐  ┌──────────┐
    │  Backend  │  │ Frontend │  │   QA     │
    └──────────┘  └──────────┘  └──────────┘
```

Using CrewAI with hierarchical process:

```python
from crewai import Agent, Task, Crew, Process, LLM

llm = LLM(model="ollama/qwen3.5:4b", base_url="http://localhost:11434")

# === Analyst ===
analyst = Agent(
    role="Systems Analyst",
    goal="Gather and structure project requirements",
    backstory="You are an experienced analyst. Turn vague ideas into clear tasks.",
    llm=llm
)

# === Architect (manages developers) ===
backend_dev = Agent(
    role="Backend Developer",
    goal="Implement server-side API logic",
    backstory="You write reliable Python code with tests.",
    llm=llm
)

frontend_dev = Agent(
    role="Frontend Developer",
    goal="Create the user interface",
    backstory="A specialist in React and TypeScript.",
    llm=llm
)

tester = Agent(
    role="QA Engineer",
    goal="Find bugs before users see them",
    backstory="You are a meticulous tester. You check everything.",
    llm=llm
)

architect = Agent(
    role="Architect",
    goal="Coordinate development: assign tasks to developers and verify quality",
    backstory="You are a technical leader. You understand both backend and frontend.",
    llm=llm
)

# === DevOps ===
devops = Agent(
    role="DevOps Engineer",
    goal="Set up infrastructure, CI/CD, and monitoring",
    backstory="You automate everything that can be automated.",
    llm=llm
)

# === Supervisor (PM) ===
pm = Agent(
    role="Project Manager",
    goal="Coordinate the whole team. After each phase check the result and decide next steps.",
    backstory="You are an experienced PM. Lead the team to release.",
    llm=llm
)

# === Tasks ===
task_analyze = Task(
    description="Analyze requirements: the user wants to build a TODO application with a web interface",
    agent=analyst,
    expected_output="Requirements document: functional and non-functional"
)

task_architect = Task(
    description="Design the architecture: what components are needed, which APIs",
    agent=architect,
    expected_output="Architecture description"
)

task_backend = Task(
    description="Implement REST API for TODO: create, read, update, delete tasks",
    agent=backend_dev,
    expected_output="Python code (FastAPI)"
)

task_frontend = Task(
    description="Create a web interface: task list, add form, delete button",
    agent=frontend_dev,
    expected_output="React code"
)

task_test = Task(
    description="Write tests for the API: verify all CRUD operations",
    agent=tester,
    expected_output="pytest tests"
)

task_deploy = Task(
    description="Configure Docker Compose to run the entire application",
    agent=devops,
    expected_output="docker-compose.yml + instructions"
)

# === Assemble the team ===
crew = Crew(
    agents=[pm, analyst, architect, backend_dev, frontend_dev, tester, devops],
    tasks=[task_analyze, task_architect, task_backend, task_frontend, task_test, task_deploy],
    process=Process.hierarchical,
    manager_llm=llm,
    verbose=True
)

result = crew.kickoff()
```

---

## 8. Multi-agent system challenges

### 8.1 Communication overhead

Every message between agents costs tokens. In a group chat of 5 agents exchanging 10+ messages, context grows quickly.

**Solution:** Limit rounds, use short messages, summarize.

### 8.2 Contradictory decisions

Agents may disagree with each other, and it is unclear whose decision is correct.

**Solution:** Supervisor with veto power, voting, human-in-the-loop.

### 8.3 Cascade errors

One agent's error (wrong data, wrong decision) propagates to all downstream agents.

**Solution:** Validate at each step, retry, fallback agents.

### 8.4 Context loss

Each agent sees only its own part. The big picture can be lost.

**Solution:** Supervisor maintains shared state, regular syncs.

### 8.5 When NOT to use multi-agent

- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Task can be solved by one agent in 1-2 steps
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> No clear split into different expertise areas
- <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> Thinking "more agents = cooler" (no, more agents = more problems)

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
