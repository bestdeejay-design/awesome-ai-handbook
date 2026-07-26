# <img src="https://lucide.dev/api/icons/building-2" alt="" width="20" height="20" style="vertical-align:middle"> AI Agent Frameworks: LangGraph, CrewAI, Agno

> Frameworks for building agents — from simple single-agent to multi-agent teams with local models via Ollama.

**🇷🇺 Russian version:** [frameworks.ru.md](frameworks.ru.md)

---

[← AI agents](README.md) · [Patterns](patterns.md) · [Ollama for agents →](ollama-for-agents.md)

---

## Contents

1. [Comparison table](#1-comparison-table)
2. [Agno — quick start](#2-agno--quick-start)
3. [CrewAI — agent team](#3-crewai--agent-team)
4. [LangGraph — maximum control](#4-langgraph--maximum-control)
5. [Other frameworks](#5-other-frameworks)
6. [How to choose](#6-how-to-choose)
7. [What's next](#7-whats-next)

---

## 1. Comparison table

| Framework | <img src="https://lucide.dev/api/icons/star" alt="" width="20" height="20" style="vertical-align:middle"> | Level | Multi-agent | Ollama | When to use |
|-----------|-----|---------|-------------|--------|-------------|
| **Agno** | 15K+ | Beginner | Team | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | Quick prototype, first agent |
| **CrewAI** | 25K+ | Intermediate | Crew | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | Agent team with roles |
| **LangGraph** | 10K+ | Advanced | Graphs | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | Maximum control |
| **OpenAI Agents SDK** | — | Beginner | Handoff | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | If you know OpenAI API |
| **AutoGen** | 48K+ | Intermediate | GroupChat | <img src="https://lucide.dev/api/icons/alert-triangle" alt="" width="20" height="20" style="vertical-align:middle"> | Multi-agent dialogues |
| **Mastra** | 10K+ | Intermediate | Yes | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | TypeScript projects |

> All examples below use Ollama with `qwen3.5:4b`. Make sure Ollama is running (`ollama serve`).

---

## 2. Agno — quick start

```python
from agno.agent import Agent
from agno.models.ollama import Ollama
from agno.tools.duckduckgo import DuckDuckGoTools

agent = Agent(
    model=Ollama(id="qwen3.5:4b"),
    tools=[DuckDuckGoTools()],
    description="You are a helpful assistant with internet access",
    markdown=True
)

agent.run("What are the trends in AI agents in 2026?")
```

```python
def get_server_status(host: str) -> str:
    """Check server status"""
    import subprocess
    result = subprocess.run(["ping", "-c", "1", host], capture_output=True, text=True)
    return "available" if result.returncode == 0 else "unavailable"
```

```python
    tools=[get_server_status],
    show_tool_calls=True  # 👈 see which tools are being called
)

agent.run("Check the status of google.com")
```

```python
from agno.team import Team

researcher = Agent(
    name="Researcher",
    instructions="Find information and pass it to the editor",
)

writer = Agent(
    name="Editor",
    instructions="Write a brief report based on the data received",
)

team = Team(
    name="Content team",
    members=[researcher, writer],
    instructions="Coordinate the team's work to create content",
)
```

---

## 3. CrewAI — agent team

CrewAI is built around **roles**: each agent has a role, goal, and backstory. Agents join a Crew and execute Tasks.

```python
from crewai import Agent, Task, Crew, Process, LLM

# 1. Create LLM with Ollama
llm = LLM(
    model="ollama/qwen3.5:4b",
    base_url="http://localhost:11434"
)

# 2. Agents with roles
researcher = Agent(
    role="Market researcher",
    goal="Find current data on AI trends",
    backstory="Experienced analyst with 10 years of experience",
    llm=llm,
    allow_delegation=False
)

writer = Agent(
    role="Technical writer",
    goal="Create a clear report based on the data",
    backstory="Writes documentation for complex technologies",
    llm=llm,
)

# 3. Tasks
research_task = Task(
    description="Find 5 key trends in AI agents in 2026",
    agent=researcher,
    expected_output="A list of 5 trends with brief descriptions"
)

write_task = Task(
    description="Based on the trends, write a brief report (3 paragraphs)",
    agent=writer,
    expected_output="Report text in markdown"
)

# 4. Create the crew
crew = Crew(
    agents=[researcher, writer],
    tasks=[research_task, write_task],
    process=Process.sequential,
    verbose=True
)
```

```python
    process=Process.hierarchical,  # 👈 manager delegates
    manager_llm=llm,
)
```

---

## 4. LangGraph — maximum control

```python
from typing import TypedDict, Annotated
from langgraph.graph import StateGraph, START, END
from langgraph.graph.message import add_messages
from langgraph.prebuilt import ToolNode, tools_condition
from langchain_ollama import ChatOllama
from langchain_core.tools import tool

# 1. Agent state
class AgentState(TypedDict):
    messages: Annotated[list, add_messages]

# 2. Tools
@tool
def search_web(query: str) -> str:
    """Search for information on the web"""
    return f"Search results for: {query}"

# 3. Model
llm = ChatOllama(model="qwen3.5:4b", base_url="http://localhost:11434")
llm_with_tools = llm.bind_tools(tools)

# 4. Agent node
def agent_node(state: AgentState):
    return {"messages": [llm_with_tools.invoke(state["messages"])]}

# 5. Build graph
graph = StateGraph(AgentState)
graph.add_node("agent", agent_node)
graph.add_node("tools", ToolNode(tools))
graph.add_edge(START, "agent")
graph.add_conditional_edges("agent", tools_condition, {"tools": "tools", END: END})
graph.add_edge("tools", "agent")

# 6. Compile
agent = graph.compile()

# 7. Run
result = agent.invoke({
    "messages": [("user", "What's new in AI in 2026?")]
})
print(result["messages"][-1].content)
```

### Multi-agent with Supervisor (LangGraph)

```python
from langgraph.graph import StateGraph, START, END, MessagesState

# Researcher agent
def researcher(state: MessagesState):
    response = llm.invoke([
        {"role": "system", "content": "You are a researcher. Find facts and data."},
        *state["messages"]
    ])
    return {"messages": [response]}

# Writer agent
def writer(state: MessagesState):
    response = llm.invoke([
        {"role": "system", "content": "You are a writer. Create texts based on facts."},
        *state["messages"]
    ])
    return {"messages": [response]}

# Supervisor — decides who goes next
def supervisor(state: MessagesState):
    response = llm.invoke([
        {"role": "system", "content": (
            "You are a supervisor. Choose who will work next: "
            "'researcher' to find data, 'writer' to write text, "
            "'FINISH' if the task is complete."
        )},
        *state["messages"]
    ])
    return {"messages": [response]}

# Build graph
graph = StateGraph(MessagesState)
graph.add_node("supervisor", supervisor)
graph.add_node("researcher", researcher)
graph.add_node("writer", writer)

graph.add_edge(START, "supervisor")
graph.add_conditional_edges("supervisor", lambda s: s["messages"][-1].content)
graph.add_edge("researcher", "supervisor")
graph.add_edge("writer", "supervisor")
```

**When LangGraph:**
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Complex workflows with conditions and loops
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Need checkpointing (save state between runs)
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Human-in-the-loop
- <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> Simple tasks (Agno or CrewAI are easier)

---

## 5. Other frameworks

### OpenAI Agents SDK

```python
from agents import Agent, Runner, function_tool

@function_tool
def get_weather(city: str) -> str:
    return f"Weather in {city}: +22°C"

agent = Agent(
    name="Weather assistant",
    instructions="Help with weather forecasts",
    tools=[get_weather],
    model="qwen3.5:4b"  # via Ollama with proxy
)
```

Works if you already know the OpenAI API. Use [forge](https://github.com/antoinezambelli/forge) as a proxy for local models.

### Mastra

```typescript
const agent = new Agent({
  name: 'my-agent',
  model: {
    provider: 'OLLAMA',
    name: 'qwen3.5:4b',
  },
  instructions: 'You are a helpful assistant',
});
```

For TypeScript projects. Good Ollama integration.

### AutoGen

```python
# pip install pyautogen
from autogen import AssistantAgent, UserProxyAgent

llm_config = {
    "config_list": [{
        "model": "qwen3.5:4b",
        "base_url": "http://localhost:11434/v1",
        "api_type": "openai",
        "api_key": "ollama"
    }]
}

agent = AssistantAgent("assistant", llm_config=llm_config)
user = UserProxyAgent("user", code_execution_config=False)
user.initiate_chat(agent, message="Analyze AI trends in 2026")
```

---

## 6. How to choose

| Situation | Framework |
|-----------|-----------|
| First agent, quick prototype | **Agno** |
| Team with roles (PM + dev + QA) | **CrewAI** |
| Complex conditional workflows | **LangGraph** |
| Know OpenAI API, want local | **OpenAI Agents SDK + forge** |
| Multi-agent dialogues | **AutoGen (GroupChat)** |
| TypeScript ecosystem | **Mastra** |

```
CrewAI (Process.hierarchical)
  → PM agent
    → Analyst
    → Developer
    → Tester
    → DevOps
```

Each agent is a CrewAI role. Manager distributes tasks. More details in the [02-agent-team tutorial](tutorials/02-agent-team.md).

---

## 7. What's next

| If you want | Go to |
|-------------|-------|
| Write your first agent without frameworks | [tutorials/01-first-agent.md](tutorials/01-first-agent.md) |
| Build an agent team (your scenario) | [tutorials/02-agent-team.md](tutorials/02-agent-team.md) |
| Connect Ollama to any framework | [ollama-for-agents.md](ollama-for-agents.md) |
| Understand agent architecture | [architecture.md](architecture.md) |
| Back to navigation | [README.md](README.md) |
---

---

**In section:** [architecture](architecture.md) · [evaluation](evaluation.md) · [frameworks](frameworks.md) · [memory](memory.md) · [multi-agent](multi-agent.md) · [ollama-for-agents](ollama-for-agents.md) · [orchestrators](orchestrators.md) · [patterns](patterns.md) · [prompting](prompting.md) · [ready-made](ready-made.md) · [safety](safety.md) · [skills](skills.md)  
**Related sections:** [Zero Level](../basics/README.md) · [Local Models](../local-models/README.md) · [Use Cases](../use-cases/README.md) · [Resources](../resources/README.md)  
**Navigation:** [← AI Agents](README.md) · [↑ Back to main](../README.md) · [🇷🇺 Русский](frameworks.ru.md)

