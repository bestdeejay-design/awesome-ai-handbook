# 🧠 Agent Memory: Short-term, Long-term, Vector

> How agents store, retrieve, and manage context — from simple message history to semantic search with vector databases.

**🇷🇺 Russian version:** [memory.ru.md](memory.ru.md)

---

[← AI agents](README.md) · [Prompting →](prompting.md)

---

## Contents

1. [Why agents need memory](#1-why-agents-need-memory)
2. [Types of memory](#2-types-of-memory)
3. [Short-term memory (working)](#3-short-term-memory-working)
4. [Long-term memory (persistent)](#4-long-term-memory-persistent)
5. [Memory in popular frameworks](#5-memory-in-popular-frameworks)
6. [Problems and solutions](#6-problems-and-solutions)
7. [Whats next](#7-whats-next)

---

## 1. Why agents need memory

Without memory, every agent call starts from scratch. The model doesnt remember what it said earlier, what the user asked, or what decisions were made.

**With memory:**
- The agent remembers the conversation context
- Knows the project state between sessions
- Can search past decisions

---

## 2. Types of memory

| Type | Lifespan | Storage | Speed |
|------|----------|---------|-------|
| **Short-term** | One session | Messages list | Instant |
| **Long-term (files)** | Indefinitely | JSON, SQLite | Fast |
| **Long-term (vectors)** | Indefinitely | Chroma, Qdrant | Medium |
| **Episodic** | Per task | Structured logs | Fast |

---

## 3. Short-term memory (working)

The simplest form of memory: the agent sees the entire message history.

```python
messages = [
    {"role": "system", "content": "You are a PM agent..."},
    {"role": "user", "content": "What tasks need to be done?"},
    {"role": "assistant", "content": "We need to: 1. Write API 2. Build UI"},
    {"role": "user", "content": "Estimate timelines for each item"},
    # each new message is added here
]
```

### Context compression

When history gets too long, compress it:

```python
def compress_messages(messages, max_messages=10, model="qwen3.5:4b"):
    """Compress history if its too long."""

    if len(messages) <= max_messages:
        return messages

    # Take system prompt + last N-1 messages
    system = [m for m in messages if m["role"] == "system"][:1]
    recent = messages[-(max_messages-1):]

    # If still too long summarize the middle
    total_tokens = sum(len(m.get("content", "")) for m in messages)
    if total_tokens > 10000:
        middle = messages[1:-(max_messages-1)]
        summary = requests.post("http://localhost:11434/api/chat", json={
            "model": model,
            "messages": [
                {"role": "system", "content": "Summarize the dialogue history briefly"},
                {"role": "user", "content": json.dumps(middle)}
            ],
            "stream": False
        })
        summary_text = summary.json()["message"]["content"]

        return system + [
            {"role": "system", "content": f"Brief history: {summary_text}"}
        ] + recent

    return system + recent
```

### When to compress
- Agent loop exceeds 5 steps
- Total message size > 70% of context window
- At 2-3 steps unnecessary

---

## 4. Long-term memory (persistent)

Short-term memory lives while the agent runs. Long-term memory persists between sessions.

| Method | Storage | When to use |
|-------|---------|-------------|
| **JSON files** | `agent_memory.json` | Simple projects, single user |
| **SQLite** | `memory.db` | Multiple agents, structured data |
| **Vector DB** | Chroma, Qdrant | Semantic memory search |
| **Git** | Commits in repository | For coding agents |

### Example: file-based memory

```python
import os, json

class FileMemory:
    """Simple long-term memory in a JSON file."""

    def __init__(self, filepath="agent_memory.json"):
        self.filepath = filepath
        self.data = self._load()

    def _load(self) -> dict:
        if os.path.exists(self.filepath):
            with open(self.filepath) as f:
                return json.load(f)
        return {"projects": {}, "decisions": [], "facts": []}

    def save(self):
        with open(self.filepath, "w") as f:
            json.dump(self.data, f, ensure_ascii=False, indent=2)

    def remember_fact(self, fact: str):
        """Remember a fact about the project."""
        self.data["facts"].append({
            "fact": fact,
            "timestamp": __import__("datetime").datetime.now().isoformat()
        })
        self.save()

    def get_project_state(self, project: str) -> dict:
        """Get project state."""
        return self.data["projects"].get(project, {})

    def update_project(self, project: str, key: str, value):
        """Update project state."""
        if project not in self.data["projects"]:
            self.data["projects"][project] = {}
        self.data["projects"][project][key] = value
        self.save()
```

### Example: vector memory (ChromaDB)

```python
import chromadb
from chromadb.utils import embedding_functions

class VectorMemory:
    """Long-term memory with semantic search."""

    def __init__(self, collection_name="agent_memory"):
        self.client = chromadb.Client()
        self.collection = self.client.get_or_create_collection(
            name=collection_name,
            embedding_function=embedding_functions.DefaultEmbeddingFunction()
        )

    def add(self, text: str, metadata: dict = None):
        """Add information to memory."""
        self.collection.add(
            documents=[text],
            metadatas=[metadata or {}],
            ids=[str(hash(text))]
        )

    def search(self, query: str, n_results: int = 3) -> list:
        """Find similar records."""
        results = self.collection.query(
            query_texts=[query],
            n_results=n_results
        )
        return results["documents"][0]
```

---

## 5. Memory in popular frameworks

### LangGraph checkpointing

```python
from langgraph.checkpoint.memory import MemorySaver
from langgraph.graph import StateGraph, MessagesState, START
from langchain_ollama import ChatOllama

llm = ChatOllama(model="qwen3.5:4b")

def call_model(state: MessagesState):
    response = llm.invoke(state["messages"])
    return {"messages": [response]}

graph = StateGraph(MessagesState)
graph.add_node("agent", call_model)
graph.add_edge(START, "agent")

memory_saver = MemorySaver()
agent = graph.compile(checkpointer=memory_saver)

config = {"configurable": {"thread_id": "project-123"}}
agent.invoke({"messages": [("user", "Hello!")]}, config)
agent.invoke({"messages": [("user", "What did I just say?")]}, config)
```

### CrewAI built-in memory

```python
from crewai import Crew, Process
from crewai.memory import Memory

crew = Crew(
    agents=[...],
    tasks=[...],
    memory=Memory(
        short_term=True,
        long_term=True,
        entity=True
    )
)
```

### Agno session state

```python
from agno.agent import Agent
from agno.models.ollama import Ollama

agent = Agent(
    model=Ollama(id="qwen3.5:4b"),
    session_state={}
)

agent.session_state["project"] = "awesome-ai-handbook"
agent.run("Remember: we are making an AI handbook")
```

---

## 6. Problems and solutions

| Problem | Description | Solution |
|---------|-------------|----------|
| **Context forgetting** | Model forgets the beginning of dialogue | Summarize every N steps |
| **Context growth** | Too many messages exceed limit | Compression, delete old messages |
| **Conflicting memory** | Agents write different data | Centralized storage, versioning |
| **Data leakage** | Sensitive data in history | Scoping, clear after completion |

---

## 7. Whats next

| If you want | Go to |
|-------------|-------|
| Learn agent prompting | [prompting.md](prompting.md) |
| Orchestrate an agent team | [multi-agent.md](multi-agent.md) |
| Secure your agents | [safety.md](safety.md) |
| Build your own team | [tutorials/02-agent-team.md](tutorials/02-agent-team.md) |
| Back to navigation | [README.md](README.md) |
