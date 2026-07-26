# <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> Tutorial 1: First Agent in 30 Minutes

> Build a ReAct agent with search capabilities using Python + Ollama. No frameworks needed.

**🇷🇺 Russian version:** [01-first-agent.ru.md](01-first-agent.ru.md)

---

[← Tutorials](README.md) · [Team →](02-agent-team.md)

---

## Contents

1. [What you will build](#1-what-you-will-build)
2. [Setup](#2-setup)
3. [Create the agent](#3-create-the-agent)
4. [Run it](#4-run-it)
5. [Whats next](#5-whats-next)

---

## 1. What you will build

A ReAct agent that:
- Takes user questions
- Decides whether to search the web or answer directly
- Uses Ollama + Qwen 3.5 for local inference
- Runs in a loop until the task is complete

---

## 2. Setup

```bash
# Make sure Ollama is running
ollama serve

# Pull the model
ollama pull qwen3.5:4b

# Install Python requests
pip install requests
```

---

## 3. Create the agent

Create `agent.py`:

```python
import requests
import json

OLLAMA = "http://localhost:11434/api/chat"

# Define the tool the agent can use
tools = [{
    "type": "function",
    "function": {
        "name": "search_web",
        "description": "Search the web for current information. Use this for news, weather, prices, etc.",
        "parameters": {
            "type": "object",
            "properties": {
                "query": {
                    "type": "string",
                    "description": "Search query. Be specific."
                }
            },
            "required": ["query"]
        }
    }
}]

def agent_loop(user_input, max_steps=5):
    messages = [
        {"role": "system", "content": "You are a ReAct agent. Search the web when you need current information. Answer directly when you know enough."},
        {"role": "user", "content": user_input}
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
                args = json.loads(tc["function"]["arguments"])
                print(f"  Step {step+1}: calling {name}({json.dumps(args)})")
                # Execute the tool (in real apps, plug in actual search)
                result = {"result": f"Search results for: {args.get('query', '')}"}
                messages.append({
                    "role": "tool",
                    "name": name,
                    "content": json.dumps(result)
                })
        else:
            return msg["content"]

    return "Step limit reached"

if __name__ == "__main__":
    result = agent_loop("What was the weather in London yesterday?")
    print(f"\nAnswer: {result}")
```

---

## 4. Run it

```bash
python agent.py
```

You should see the agent think, decide whether to call search, and return an answer.

### Try different questions

- "What is 15 * 37?" — should use search or calculate
- "Explain what a neural network is" — should answer directly
- "Write a Python function to sort a list" — should answer from knowledge

---

## 5. Whats next

| Step | Go to |
|------|-------|
| Add more tools (file read, calculator) | [ollama-for-agents.md](../ollama-for-agents.md) |
| Build a multi-agent team | [02-agent-team.md](02-agent-team.md) |
| Study agent patterns | [patterns.md](../patterns.md) |
| Back | [README.md](README.md) |
---

---

**In section:** [01-first-agent](01-first-agent.md) · [02-agent-team](02-agent-team.md) · [03-coding-agent](03-coding-agent.md)  
**Related sections:** [AI Agents](../README.md) · [Zero Level](../../basics/README.md) · [Local Models](../../local-models/README.md)  
**Navigation:** [← Tutorials](README.md) · [↑ Back to main](../../README.md) · [🇷🇺 Русский](01-first-agent.ru.md)

