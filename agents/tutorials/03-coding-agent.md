# 🔴 Tutorial 3: Coding Agent

> A coding agent using LangGraph that reads code, finds bugs, writes tests, and makes PRs.

**🇷🇺 Russian version:** [03-coding-agent.ru.md](03-coding-agent.ru.md)

---

[← Tutorials](README.md) · [Agent team](02-agent-team.md)

---

```bash
pip install langgraph langchain-ollama
ollama pull qwen2.5-coder:7b
```

```python
from typing import TypedDict, Annotated
from langgraph.graph import StateGraph, START
from langgraph.graph.message import add_messages
from langchain_ollama import ChatOllama

llm = ChatOllama(model="qwen2.5-coder:7b", base_url="http://localhost:11434")

class AgentState(TypedDict):
    messages: Annotated[list, add_messages]

def call_model(state: AgentState):
    response = llm.invoke(state["messages"])
    return {"messages": [response]}

# Build graph
graph = StateGraph(AgentState)
graph.add_node("agent", call_model)
graph.add_edge(START, "agent")
agent = graph.compile()

# Run
result = agent.invoke({
    "messages": [("user", "Review this code and suggest fixes: def add(a,b): return a-b")]
})
print(result["messages"][-1].content)
```

## With tools

```python
from langchain_core.tools import tool
from langgraph.prebuilt import ToolNode, tools_condition

@tool
def read_file(path: str) -> str:
    """Read a file from disk"""
    with open(path) as f:
        return f.read()

@tool
def run_tests(path: str) -> str:
    """Run pytest on a file"""
    import subprocess
    r = subprocess.run(["pytest", path, "-v"], capture_output=True, text=True, timeout=30)
    return r.stdout

tools = [read_file, run_tests]
llm_with_tools = llm.bind_tools(tools)

def agent_node(state: AgentState):
    return {"messages": [llm_with_tools.invoke(state["messages"])]}

graph = StateGraph(AgentState)
graph.add_node("agent", agent_node)
graph.add_node("tools", ToolNode(tools))
graph.add_edge(START, "agent")
graph.add_conditional_edges("agent", tools_condition, {"tools": "tools", "__end__": "__end__"})
graph.add_edge("tools", "agent")

coding_agent = graph.compile()

result = coding_agent.invoke({
    "messages": [("user", "Write a fibonacci function, save it, and test it")]
})
print(result["messages"][-1].content)
```

---

## Whats next

| Go to | Description |
|-------|-------------|
| [patterns.md](../patterns.md) | Agent architecture patterns |
| [frameworks.md](../frameworks.md) | Framework comparison |
| Back | [README.md](README.md) |
