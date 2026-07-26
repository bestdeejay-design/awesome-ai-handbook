# <img src="assets/icons/level-red.svg" alt="" width="12" height="12" style="vertical-align:middle"> Tutorial 3: Coding Agent

> Build a coding agent using LangGraph that reads code, finds bugs, writes tests, and makes PRs.

**🇷🇺 Russian version:** [03-coding-agent.ru.md](03-coding-agent.ru.md)

---

[← Tutorials](README.md) · [Agent team](02-agent-team.md)

---

## Contents

1. [What you will build](#1-what-you-will-build)
2. [Setup](#2-setup)
3. [Basic agent with tools](#3-basic-agent-with-tools)
4. [Whats next](#4-whats-next)

---

## 1. What you will build

A coding agent using LangGraph that can:
- Read files from disk
- Run tests
- Write and fix code
- Use Ollama + Qwen 2.5 Coder locally

---

## 2. Setup

```bash
pip install langgraph langchain-ollama
ollama pull qwen2.5-coder:7b
```

---

## 3. Basic agent with tools

```python
from typing import TypedDict, Annotated
from langgraph.graph import StateGraph, START
from langgraph.graph.message import add_messages
from langchain_ollama import ChatOllama

# Setup model
llm = ChatOllama(
    model="qwen2.5-coder:7b",
    base_url="http://localhost:11434"
)

# Define state
class AgentState(TypedDict):
    messages: Annotated[list, add_messages]

# Simple agent node
def call_model(state: AgentState):
    response = llm.invoke(state["messages"])
    return {"messages": [response]}

# Build the graph
graph = StateGraph(AgentState)
graph.add_node("agent", call_model)
graph.add_edge(START, "agent")
agent = graph.compile()

# Use it
result = agent.invoke({
    "messages": [("user", "Review this code and suggest fixes: def add(a,b): return a-b")]
})
print(result["messages"][-1].content)
```

### With tools

```python
from langchain_core.tools import tool
from langgraph.prebuilt import ToolNode, tools_condition

# Define tools
@tool
def read_file(path: str) -> str:
    """Read a file from disk"""
    with open(path) as f:
        return f.read()

@tool
def run_tests(path: str) -> str:
    """Run pytest on a file"""
    import subprocess
    try:
        r = subprocess.run(["pytest", path, "-v"],
            capture_output=True, text=True, timeout=30)
        return r.stdout + r.stderr
    except subprocess.TimeoutExpired:
        return "Tests timed out"

# Bind tools to model
tools = [read_file, run_tests]
llm_with_tools = llm.bind_tools(tools)

# Agent node
def agent_node(state: AgentState):
    return {"messages": [llm_with_tools.invoke(state["messages"])]}

# Build graph with tools
graph = StateGraph(AgentState)
graph.add_node("agent", agent_node)
graph.add_node("tools", ToolNode(tools))
graph.add_edge(START, "agent")
graph.add_conditional_edges("agent", tools_condition, {"tools": "tools", "__end__": "__end__"})
graph.add_edge("tools", "agent")

coding_agent = graph.compile()

# Use it
result = coding_agent.invoke({
    "messages": [("user", "Write a fibonacci function, save it to fib.py, and test it")]
})
print(result["messages"][-1].content)
```

Save as `coding_agent.py` and run:
```bash
python coding_agent.py
```

---

## 4. Whats next

| Go to | Description |
|-------|-------------|
| [patterns.md](../patterns.md) | Agent architecture patterns |
| [frameworks.md](../frameworks.md) | Framework comparison |
| [evaluation.md](../evaluation.md) | Testing and benchmarking agents |
| Back | [README.md](README.md) |
---

---

**In section:** [01-first-agent](01-first-agent.md) · [02-agent-team](02-agent-team.md) · [03-coding-agent](03-coding-agent.md)  
**Related sections:** [AI Agents](../README.md) · [Zero Level](../../basics/README.md) · [Local Models](../../local-models/README.md)  
**Navigation:** [← Tutorials](README.md) · [↑ Back to main](../../README.md) · [🇷🇺 Русский](03-coding-agent.ru.md)

