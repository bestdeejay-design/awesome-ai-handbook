# 🟡 Tutorial 2: Agent Team for Your Project

> Build a multi-agent system: PM, analyst, developer, and QA working together using CrewAI.

**🇷🇺 Russian version:** [02-agent-team.ru.md](02-agent-team.ru.md)

---

[← Tutorials](README.md) · [First agent](01-first-agent.md) · [Coding agent →](03-coding-agent.md)

---

```bash
pip install crewai crewai-tools
ollama pull qwen3.5:4b
```

```python
from crewai import Agent, Task, Crew, Process, LLM

llm = LLM(model="ollama/qwen3.5:4b", base_url="http://localhost:11434")

# Define agents
pm = Agent(
    role="Project Manager",
    goal="Coordinate the team, distribute tasks, verify results",
    backstory="Experienced PM. Keep the team focused on delivery.",
    llm=llm
)

analyst = Agent(
    role="Analyst",
    goal="Gather requirements and write specs",
    backstory="Detail-oriented analyst.",
    llm=llm
)

developer = Agent(
    role="Developer",
    goal="Implement features per spec",
    backstory="Senior full-stack developer.",
    llm=llm
)

qa = Agent(
    role="QA Engineer",
    goal="Test everything and find bugs",
    backstory="Meticulous tester. Nothing gets past you.",
    llm=llm
)

# Define tasks
task1 = Task(description="Analyze requirements for a TODO app", agent=analyst)
task2 = Task(description="Design architecture", agent=pm)
task3 = Task(description="Implement REST API", agent=developer)
task4 = Task(description="Write tests", agent=qa)

# Assemble team
crew = Crew(
    agents=[pm, analyst, developer, qa],
    tasks=[task1, task2, task3, task4],
    process=Process.hierarchical,
    manager_llm=llm,
    verbose=True
)

result = crew.kickoff()
print(result)
```

---

## How it works

| Role | Task |
|------|------|
| PM | Distributes work, tracks progress |
| Analyst | Requirements, specs |
| Developer | Code |
| QA | Tests, bug reports |

---

## Whats next

| Go to | Description |
|-------|-------------|
| [03-coding-agent.md](03-coding-agent.md) | Coding agent with LangGraph |
| [multi-agent.md](../multi-agent.md) | Multi-agent architecture |
| [memory.md](../memory.md) | Team memory setup |
| Back | [README.md](README.md) |
