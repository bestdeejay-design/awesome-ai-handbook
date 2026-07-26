# <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-yellow.svg" alt="" width="12" height="12" style="vertical-align:middle"> Tutorial 2: Agent Team for Your Project

> Build a multi-agent team: PM, analyst, developer, and QA working together using CrewAI + Ollama.

**🇷🇺 Russian version:** [02-agent-team.ru.md](02-agent-team.ru.md)

---

[← Tutorials](README.md) · [First agent](01-first-agent.md) · [Coding agent →](03-coding-agent.md)

---

## Contents

1. [What you will build](#1-what-you-will-build)
2. [Setup](#2-setup)
3. [Define agents](#3-define-agents)
4. [Define tasks](#4-define-tasks)
5. [Run the team](#5-run-the-team)
6. [Whats next](#6-whats-next)

---

## 1. What you will build

A team of AI agents that collaborate on a project:
- **PM** — distributes tasks, tracks progress
- **Analyst** — gathers requirements, writes specs
- **Developer** — implements code
- **QA** — tests everything

---

## 2. Setup

```bash
pip install crewai crewai-tools
ollama pull qwen3.5:4b
```

---

## 3. Define agents

```python
from crewai import Agent, Task, Crew, Process, LLM

llm = LLM(model="ollama/qwen3.5:4b", base_url="http://localhost:11434")

pm = Agent(
    role="Project Manager",
    goal="Coordinate the team, distribute tasks, verify results",
    backstory="Experienced PM who keeps the team focused on delivery.",
    llm=llm
)

analyst = Agent(
    role="Analyst",
    goal="Gather requirements and write specifications",
    backstory="Detail-oriented analyst who turns vague ideas into clear specs.",
    llm=llm
)

developer = Agent(
    role="Developer",
    goal="Implement features according to specification",
    backstory="Senior full-stack developer who writes clean, tested code.",
    llm=llm
)

qa = Agent(
    role="QA Engineer",
    goal="Test everything and find bugs before users do",
    backstory="Meticulous tester who catches everything.",
    llm=llm
)
```

---

## 4. Define tasks

```python
task_analyze = Task(
    description="Analyze requirements for a TODO app with web interface. Users should be able to create, read, update, and delete tasks.",
    agent=analyst,
    expected_output="Requirements document with functional and non-functional specs"
)

task_architect = Task(
    description="Design the architecture based on requirements. What components, what API endpoints.",
    agent=pm,
    expected_output="Architecture description with component list"
)

task_backend = Task(
    description="Implement REST API for TODO: create, read, update, delete tasks. Use FastAPI.",
    agent=developer,
    expected_output="Python code for the API"
)

task_test = Task(
    description="Write tests for the API. Test all CRUD operations, including edge cases.",
    agent=qa,
    expected_output="pytest test file"
)
```

---

## 5. Run the team

```python
crew = Crew(
    agents=[pm, analyst, developer, qa],
    tasks=[task_analyze, task_architect, task_backend, task_test],
    process=Process.hierarchical,  # PM manages the team
    manager_llm=llm,
    verbose=True
)

result = crew.kickoff()
print(result)
```

Save as `team.py` and run:
```bash
python team.py
```

The PM will coordinate: analyst works first, then architect, then developer, then QA.

---

## 6. Whats next

| Go to | Description |
|-------|-------------|
| [03-coding-agent.md](03-coding-agent.md) | Coding agent with LangGraph |
| [multi-agent.md](../multi-agent.md) | Multi-agent architecture deep dive |
| [memory.md](../memory.md) | Setting up team memory |
| Back | [README.md](README.md) |
---

---

**In section:** [01-first-agent](01-first-agent.md) · [02-agent-team](02-agent-team.md) · [03-coding-agent](03-coding-agent.md)  
**Related sections:** [AI Agents](../README.md) · [Zero Level](../../basics/README.md) · [Local Models](../../local-models/README.md)  
**Navigation:** [← Tutorials](README.md) · [↑ Back to main](../../README.md) · [🇷🇺 Русский](02-agent-team.ru.md)

