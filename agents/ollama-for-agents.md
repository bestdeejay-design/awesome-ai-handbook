# <img src="https://lucide.dev/api/icons/target" alt="" width="20" height="20" style="vertical-align:middle"> Ollama for Agents

> Ollama API capabilities for building agents: tool calling, JSON mode, structured output, streaming, and integration with agent frameworks.

**🇷🇺 Russian version:** [ollama-for-agents.ru.md](ollama-for-agents.ru.md)

---

[← AI agents](README.md) · [Frameworks](frameworks.md) · [Patterns](patterns.md)

---

## Contents

1. [Ollama API capabilities](#1-ollama-api-capabilities)
2. [Tool calling (Function Calling)](#2-tool-calling-function-calling)
3. [OpenAI-compatible SDK](#3-openai-compatible-sdk)
4. [Structured output (JSON mode)](#4-structured-output-json-mode)
5. [Streaming](#5-streaming)
6. [Ollama launch for agents](#6-ollama-launch-for-agents)
7. [Tool calling without definition](#7-tool-calling-without-definition)
8. [Useful tools](#8-useful-tools)
9. [Reasoning parameter think](#9-reasoning-parameter-think)
10. [Whats next](#10-whats-next)

---

## 1. Ollama API capabilities

Ollama provides an OpenAI-compatible API for local models. Key features for agents:

| Feature | Endpoint | Description |
|---------|----------|-------------|
| Chat | `/api/chat` | Multi-turn chat with tool calling |
| JSON mode | `format:` parameter | Guaranteed JSON output |
| Tool calling | `tools:` parameter | The model decides when to call tools |
| Streaming | `stream: true` | Token-by-token output |
| Structured output | `response_format` | Pydantic-validated responses |
| Embeddings | `/api/embed` | Text-to-vector |

---

## 2. Tool calling (Function Calling)

Ollama supports tool calling natively via the API. Models need to be trained for this — not all local models support it.

### Direct API call

```bash
curl http://localhost:11434/api/chat -d '{
  "model": "qwen3.5:4b",
  "messages": [{"role": "user", "content": "Whats the weather in Tokyo?"}],
  "tools": [{
    "type": "function",
    "function": {
      "name": "get_weather",
      "description": "Get weather for a city",
      "parameters": {
        "type": "object",
        "properties": {
          "city": {"type": "string"},
          "units": {"type": "string", "enum": ["celsius", "fahrenheit"]}
        },
        "required": ["city"]
      }
    }
  }],
  "stream": false
}'
```

### Models with good tool calling

| Model | Tool calling quality | Notes |
|-------|---------------------|-------|
| Qwen 2.5 Coder (7B, 14B) | <img src="https://lucide.dev/api/icons/star" alt="" width="20" height="20" style="vertical-align:middle"><img src="https://lucide.dev/api/icons/star" alt="" width="20" height="20" style="vertical-align:middle"><img src="https://lucide.dev/api/icons/star" alt="" width="20" height="20" style="vertical-align:middle"> | Best for coding tools |
| Qwen 3.5 (4B, 8B) | <img src="https://lucide.dev/api/icons/star" alt="" width="20" height="20" style="vertical-align:middle"><img src="https://lucide.dev/api/icons/star" alt="" width="20" height="20" style="vertical-align:middle"><img src="https://lucide.dev/api/icons/star" alt="" width="20" height="20" style="vertical-align:middle"> | Best all-rounder |
| DeepSeek Coder V2 | <img src="https://lucide.dev/api/icons/star" alt="" width="20" height="20" style="vertical-align:middle"><img src="https://lucide.dev/api/icons/star" alt="" width="20" height="20" style="vertical-align:middle"> | Good but slower |
| Llama 3.1 (8B) | <img src="https://lucide.dev/api/icons/star" alt="" width="20" height="20" style="vertical-align:middle"><img src="https://lucide.dev/api/icons/star" alt="" width="20" height="20" style="vertical-align:middle"> | Acceptable |
| Mistral (7B) | <img src="https://lucide.dev/api/icons/star" alt="" width="20" height="20" style="vertical-align:middle"> | Limited |
| Phi-4 (14B) | <img src="https://lucide.dev/api/icons/star" alt="" width="20" height="20" style="vertical-align:middle"><img src="https://lucide.dev/api/icons/star" alt="" width="20" height="20" style="vertical-align:middle"> | Good with function calling |

---

## 3. OpenAI-compatible SDK

### 3.1 Basic chat

```python
from openai import OpenAI

client = OpenAI(
    base_url="http://localhost:11434/v1",
    api_key="ollama"  # any value works
)

response = client.chat.completions.create(
    model="qwen3.5:4b",
    messages=[{"role": "user", "content": "Hello! How are you?"}]
)
print(response.choices[0].message.content)
```

### 3.2 Tool calling via OpenAI SDK

```python
client = OpenAI(
    base_url="http://localhost:11434/v1",
    api_key="ollama"
)

tools = [{
    "type": "function",
    "function": {
        "name": "get_weather",
        "description": "Get weather for a city",
        "parameters": {
            "type": "object",
            "properties": {
                "city": {"type": "string", "description": "City name"}
            },
            "required": ["city"]
        }
    }
}]

response = client.chat.completions.create(
    model="qwen3.5:4b",
    messages=[{"role": "user", "content": "Whats the weather in Tokyo?"}],
    tools=tools
)

message = response.choices[0].message
if message.tool_calls:
    for tool_call in message.tool_calls:
        name = tool_call.function.name
        args = tool_call.function.arguments
        print(f"Model called: {name}({args})")
else:
    print(f"Model response: {message.content}")
```

### 3.3 Complete agent loop

```python
import json
from openai import OpenAI

client = OpenAI(
    base_url="http://localhost:11434/v1",
    api_key="ollama"
)

# Real functions the agent will call
def search_web(query: str) -> str:
    """Search the web"""
    return f"Search results for '{query}': 42 pages found"

def calculate(expression: str) -> str:
    """Calculate math expression"""
    try:
        return str(eval(expression))
    except:
        return "Calculation error"

AVAILABLE_TOOLS = {
    "search_web": search_web,
    "calculate": calculate,
}

TOOL_SCHEMAS = [{
    "type": "function",
    "function": {
        "name": "search_web",
        "description": "Search the web",
        "parameters": {
            "type": "object",
            "properties": {
                "query": {"type": "string", "description": "Search query"}
            },
            "required": ["query"]
        }
    }
}, {
    "type": "function",
    "function": {
        "name": "calculate",
        "description": "Calculate a math expression",
        "parameters": {
            "type": "object",
            "properties": {
                "expression": {"type": "string", "description": "Expression"}
            },
            "required": ["expression"]
        }
    }
}]

def agent_loop(user_input: str, max_steps: int = 5):
    """Full agent loop on OpenAI SDK + Ollama."""

    messages = [{"role": "user", "content": user_input}]

    for step in range(max_steps):
        response = client.chat.completions.create(
            model="qwen3.5:4b",
            messages=messages,
            tools=TOOL_SCHEMAS
        )

        msg = response.choices[0].message
        messages.append(msg)

        if msg.tool_calls:
            for tc in msg.tool_calls:
                name = tc.function.name
                args = json.loads(tc.function.arguments)
                print(f"  Step {step+1}: {name}({args})")

                result = AVAILABLE_TOOLS[name](**args)

                messages.append({
                    "role": "tool",
                    "tool_call_id": tc.id,
                    "content": result
                })
        else:
            return msg.content

    return "Step limit reached"

# Test
result = agent_loop("What is 2 + 2 * 3? And find info about AI agents")
print(f"\nAnswer: {result}")
```

### 3.4 Structured output via OpenAI SDK

```python
from pydantic import BaseModel
from openai import OpenAI

client = OpenAI(base_url="http://localhost:11434/v1", api_key="ollama")

class UserData(BaseModel):
    name: str
    age: int
    city: str

completion = client.beta.chat.completions.parse(
    model="qwen3.5:4b",
    messages=[{"role": "user", "content": "Extract: Ivan, 25, Moscow"}],
    response_format=UserData,
)

user = completion.choices[0].message.parsed
print(f"Name: {user.name}, Age: {user.age}, City: {user.city}")
```

### 3.5 Streaming

```python
stream = client.chat.completions.create(
    model="qwen3.5:4b",
    messages=[{"role": "user", "content": "Write a story about a robot"}],
    stream=True
)

for chunk in stream:
    if chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="", flush=True)
```

---

## 4. Structured output (JSON mode)

Ollama returns structured output strictly by JSON Schema:

```bash
curl http://localhost:11434/api/chat -d '{
  "model": "qwen3.5:4b",
  "messages": [{"role": "user", "content": "Extract: name, age, city from: Ivan, 25, Moscow"}],
  "format": {
    "type": "object",
    "properties": {
      "name": {"type": "string"},
      "age": {"type": "integer"},
      "city": {"type": "string"}
    },
    "required": ["name", "age", "city"]
  },
  "stream": false
}'
```

---

## 5. Endpoint compatibility

| OpenAI endpoint | Ollama endpoint | Description |
|----------------|----------------|-------------|
| `/v1/chat/completions` | `/v1/chat/completions` | Chat (with tool calling) |
| `/v1/completions` | `/v1/completions` | Text generation |
| `/v1/embeddings` | `/v1/embeddings` | Embeddings |
| `/v1/models` | `/v1/models` | Model list |

---

## 6. Ollama launch for agents

Ollama integrates with popular agent tools:

```bash
ollama launch claude       # Claude Code uses Ollama
ollama launch codex        # Codex uses Ollama
ollama launch openclaw     # AI assistant in WhatsApp/Telegram
ollama launch cline        # Cline VS Code integration
```

---

## 7. Web search via API

```bash
curl http://localhost:11434/api/experimental/web_search -d '{
  "query": "latest AI news 2026"
}'
```

---

## 8. Useful tools

| Tool | Platform | Feature |
|------|----------|---------|
| [**Enchanted**](https://github.com/gluonfield/enchanted) | macOS + iOS | Native SwiftUI, connect to Ollama by URI |
| [**MindWork AI Studio**](https://github.com/MindWorkAI/AI-Studio) | macOS/Win/Linux | Local + cloud providers in one GUI |
| [**Open WebUI**](https://github.com/open-webui/open-webui) | Web (Docker) | RAG, multimodality, users |
| **LM Studio** | macOS/Win/Linux | GUI for downloading/testing models |
| [**Continue**](https://github.com/continuedev/continue) | VS Code / JetBrains | AI autocomplete with local model |

Full tool catalog in [local-models/tools.md](../local-models/tools.md).

---

## 9. Reasoning parameter think

Reasoning models (DeepSeek R1, Phi-4-reasoning) support `think` parameter in Ollama:

```bash
# Without reasoning (fast response)
curl http://localhost:11434/api/chat -d '{
  "model": "deepseek-r1:7b",
  "messages": [{"role": "user", "content": "What is 2+2?"}],
  "options": {"think": false}
}'

# Maximum reasoning
curl http://localhost:11434/api/chat -d '{
  "model": "deepseek-r1:7b",
  "messages": [{"role": "user", "content": "Solve x + 3x + 2 = 0"}],
  "options": {"think": "high"}
}'
```

---

## 10. Whats next

| If you want | Go to |
|-------------|-------|
| Understand agent architecture patterns | [patterns.md](patterns.md) |
| Choose a framework | [frameworks.md](frameworks.md) |
| Full Ollama setup guide | [../local-models/advanced-setup.md](../local-models/advanced-setup.md) |
| Back to navigation | [README.md](README.md) |
---

---

**In section:** [architecture](architecture.md) · [evaluation](evaluation.md) · [frameworks](frameworks.md) · [memory](memory.md) · [multi-agent](multi-agent.md) · [ollama-for-agents](ollama-for-agents.md) · [orchestrators](orchestrators.md) · [patterns](patterns.md) · [prompting](prompting.md) · [ready-made](ready-made.md) · [safety](safety.md) · [skills](skills.md)  
**Related sections:** [Zero Level](../basics/README.md) · [Local Models](../local-models/README.md) · [Use Cases](../use-cases/README.md) · [Resources](../resources/README.md)  
**Navigation:** [← AI Agents](README.md) · [↑ Back to main](../README.md) · [🇷🇺 Русский](ollama-for-agents.ru.md)

