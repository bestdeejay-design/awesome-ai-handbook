# 🟢 Tutorial 1: First Agent in 30 Minutes

> Build a ReAct agent with search capabilities using Python + Ollama.

**🇷🇺 Russian version:** [01-first-agent.ru.md](01-first-agent.ru.md)

---

[← Back to tutorials](README.md) · [Team →](02-agent-team.md)

---

Youll build a ReAct agent that:
- Accepts user questions
- Decides whether to search the web or answer directly
- Uses Ollama + Qwen 3.5

```bash
ollama pull qwen3.5:4b
```

```python
import requests, json

OLLAMA = "http://localhost:11434/api/chat"

tools = [{
    "type": "function",
    "function": {
        "name": "search_web",
        "description": "Search the web for current information",
        "parameters": {
            "type": "object",
            "properties": {
                "query": {"type": "string", "description": "Search query"}
            },
            "required": ["query"]
        }
    }
}]

def agent_loop(user_input, max_steps=5):
    messages = [
        {"role": "system", "content": "You are a ReAct agent. Search the web when you need current info. Answer when you have enough data."},
        {"role": "user", "content": user_input}
    ]

    for step in range(max_steps):
        r = requests.post(OLLAMA, json={
            "model": "qwen3.5:4b",
            "messages": messages,
            "tools": tools,
            "stream": False
        })
        msg = r.json()["message"]
        messages.append(msg)

        if msg.get("tool_calls"):
            for tc in msg["tool_calls"]:
                print(f"  Step {step+1}: calling {tc['function']['name']}")
                messages.append({
                    "role": "tool",
                    "name": tc["function"]["name"],
                    "content": json.dumps({"result": f"Executed {tc['function']['name']}"})
                })
        else:
            return msg["content"]

    return "Step limit reached"

print(agent_loop("What was the weather in London yesterday?"))
```

---

## Whats next

| Step | Go to |
|------|-------|
| Build a multi-agent team | [02-agent-team.md](02-agent-team.md) |
| Understand patterns | [patterns.md](../patterns.md) |
| Learn about tools | [ollama-for-agents.md](../ollama-for-agents.md) |
| Back | [README.md](README.md) |
