# <img src="https://lucide.dev/api/icons/cog" alt="" width="20" height="20" style="vertical-align:middle"> Advanced Ollama Setup

> Modelfile, environment variables, API tuning, running multiple models, speculative decoding.

**🇷🇺 Russian version:** [advanced-setup.ru.md](advanced-setup.ru.md)

---

[← Local models](README.md) · [Tools →](tools.md)

---

## Contents

1. [Environment variables](#1-environment-variables)
2. [Modelfile — custom models](#2-modelfile--custom-models)
3. [HTTP API — access from programs](#3-http-api--access-from-programs)
4. [OpenAI-compatible endpoints](#4-openai-compatible-endpoints)
5. [Parallel requests and multiple models](#5-parallel-requests-and-multiple-models)
6. [Speculative decoding (speedup)](#6-speculative-decoding-speedup)
7. [Whats next](#7-whats-next)

---

## 1. Environment variables

Environment variables are the simplest way to control how Ollama uses memory,
CPU, and network. They must be set **before** starting the server.

### How to set a variable

**Method 1 — temporary (single run):**

```bash
OLLAMA_KV_CACHE_TYPE=q4_0 ollama serve
```

**Method 2 — session-wide:**

```bash
export OLLAMA_KV_CACHE_TYPE=q4_0
ollama serve
```

**Method 3 — permanent (recommended):**

Add lines to `~/.zshrc` (executed every time you open a terminal):

```bash
echo 'export OLLAMA_KV_CACHE_TYPE=q4_0' >> ~/.zshrc
echo 'export OLLAMA_FLASH_ATTENTION=1' >> ~/.zshrc
```

Then apply changes (or open a new terminal):

```bash
source ~/.zshrc
```

### Variable reference

| Variable | Default | Effect | When to change |
|---|---|---|---|
| `OLLAMA_KV_CACHE_TYPE` | `f16` | KV cache quantization. `q4_0` = **4× less RAM**, `q8_0` = 2× less | Always on Mac with ≤16 GB RAM |
| `OLLAMA_FLASH_ATTENTION` | `false` | Flash attention — saves RAM + slightly faster | Enable (`1`) if models dont fit |
| `OLLAMA_NUM_PARALLEL` | `1` | How many requests to handle simultaneously | Multiple apps using one model |
| `OLLAMA_MAX_LOADED_MODELS` | `auto` | How many models stay in RAM at once | If frequently switching models |
| `OLLAMA_KEEP_ALIVE` | `5m` | Minutes to keep model in RAM after last request | Increase if model reloads often |
| `OLLAMA_HOST` | `127.0.0.1:11434` | IP and port | `0.0.0.0:11434` for other devices |
| `OLLAMA_GPU_OVERHEAD` | `0` | Memory reserve (bytes) Ollama wont touch | If Mac slows down from low RAM |
| `OLLAMA_LOAD_TIMEOUT` | `5m` | Model loading timeout | For large models |
| `OLLAMA_NOHISTORY` | `false` | Dont save request history | Privacy |
| `OLLAMA_DEBUG` | `false` | Verbose logs | Debugging |
| `OLLAMA_MAX_QUEUE` | `512` | Maximum request queue length | Under high load |

### Recommended config for Mac 16 GB

```bash
cat >> ~/.zshrc << 'EOF'

# Ollama — optimization for Mac 16 GB
export OLLAMA_KV_CACHE_TYPE=q4_0    # 4× memory savings
export OLLAMA_FLASH_ATTENTION=1      # memory + speed
export OLLAMA_NUM_PARALLEL=2         # 2 parallel requests
export OLLAMA_KEEP_ALIVE=10m         # keep model 10 min
export OLLAMA_GPU_OVERHEAD=536870912 # 512 MB system reserve
EOF
```

Then apply:

```bash
source ~/.zshrc
```

### Verify variables are applied

```bash
# Show all Ollama environment variables
env | grep OLLAMA
```

Restart `ollama serve` (or the Ollama application) after changing variables.

---

## 2. Modelfile — custom models

**Modelfile** is like a Dockerfile but for AI models. With it you can:

- Change the system prompt (how the model behaves)
- Tune generation parameters (temperature, context window)
- Create a specialized version of a model for your specific task

### Basic Modelfile

Create a file named `Modelfile` (no extension):

```dockerfile
FROM qwen3.5:4b
PARAMETER temperature 0.7
PARAMETER num_ctx 8192
SYSTEM "You are a friendly AI assistant. Answer concisely."
```

**What this does:**
- `FROM` — base model `qwen3.5:4b`
- `PARAMETER temperature 0.7` — moderate creativity (0 = deterministic, 2 = chaos)
- `PARAMETER num_ctx 8192` — 8K token context window
- `SYSTEM` — system prompt (how the model should behave)

```bash
# Build the custom model
ollama create my-assistant -f ./Modelfile

# Run it
ollama run my-assistant
```

### Parameter reference

| Parameter | Default | Effect | Example |
|---|---|---|---|
| `temperature` | 0.8 | Creativity (0.0 = strict, 2.0 = chaos) | `0.3` for facts, `1.2` for ideas |
| `num_ctx` | 2048 | Context window in tokens | `8192` for long dialogues |
| `num_predict` | -1 (∞) | Max tokens in response | `512` for short answers |
| `top_k` | 40 | How many top options to consider | `10` for more precise answers |
| `top_p` | 0.9 | Nucleus sampling (0-1) | `0.5` for more confident answers |
| `min_p` | 0.0 | Minimum token probability | `0.05` to filter out garbage |
| `seed` | 0 | Random seed (for reproducibility) | `42` for consistent answers |
| `repeat_penalty` | 1.1 | Repetition penalty | `1.2` if model loops |
| `stop` | — | Stop sequences | `"\n"` to stop at line break |

### Example: Translation model

```dockerfile
FROM qwen3.5:4b
PARAMETER temperature 0.3
SYSTEM "You are a professional translator. Translate between Russian and English. Output only the translation, no comments."
```

### Example: Coding model

```dockerfile
FROM qwen2.5-coder:7b
PARAMETER temperature 0.2
PARAMETER num_ctx 16384
SYSTEM "You are a senior Python developer. Write code with type hints, docstrings, and error handling."
```

### Example: Brainstorming

```dockerfile
FROM qwen3.5:4b
PARAMETER temperature 1.5
PARAMETER top_p 0.95
SYSTEM "You are a creative copywriter. Generate unusual ideas, think outside the box."
```

### TEMPLATE — prompt format

Some models require a special message format:

```dockerfile
FROM llama3.3:8b

TEMPLATE """<|begin_of_text|><|start_header_id|>system<|end_header_id|>

{{ .System }}<|eot_id|><|start_header_id|>user<|end_header_id|>

{{ .Prompt }}<|eot_id|><|start_header_id|>assistant<|end_header_id|>"""
```

You usually don't need to change the template — it's already built into the model.
But if a model behaves oddly, you can override it.

### Inspect existing models

```bash
# Show Modelfile of installed model
ollama show qwen3.5:4b --modelfile

# Show parameters
ollama show qwen3.5:4b --parameters

# Show system prompt
ollama show qwen3.5:4b --system
```

---

## 3. HTTP API — access from programs

Ollama runs an HTTP server on port 11434. Any program can talk to it.

### Basic chat

```bash
curl http://localhost:11434/api/chat -d '{
  "model": "qwen3.5:4b",
  "messages": [
    {"role": "user", "content": "Hello! How are you?"}
  ],
  "stream": false
}'
```

### Text generation

```bash
curl http://localhost:11434/api/generate -d '{
  "model": "qwen3.5:4b",
  "prompt": "Write a haiku about programming",
  "stream": false
}'
```

### JSON mode

```bash
curl http://localhost:11434/api/chat -d '{
  "model": "qwen3.5:4b",
  "messages": [
    {"role": "user", "content": "Extract name, age, city from: My name is Ivan, Im 25, from Moscow"}
  ],
  "format": "json",
  "stream": false
}'
```

**Result:** the model will infer the JSON format:
```json
{"name": "Ivan", "age": 25, "city": "Moscow"}
```

### JSON Schema (structured output)

```bash
curl http://localhost:11434/api/chat -d '{
  "model": "qwen3.5:4b",
  "messages": [
    {"role": "user", "content": "Extract data from: Vasya, 30, engineer"}
  ],
  "format": {
    "type": "object",
    "properties": {
      "name": {"type": "string"},
      "age": {"type": "integer"},
      "occupation": {"type": "string"}
    },
    "required": ["name", "age"]
  },
  "stream": false
}'
```

### Embeddings

```bash
curl http://localhost:11434/api/embed -d '{
  "model": "all-minilm",
  "input": "Text to vectorize"
}'
```

### Memory management

**View loaded models:**

```bash
curl http://localhost:11434/api/ps
```

**Unload a model from memory:**

```bash
curl http://localhost:11434/api/generate -d '{
  "model": "qwen3.5:4b",
  "keep_alive": 0
}'
```

A value of `keep_alive: 0` means "unload immediately". By default, a model stays in memory for 5 minutes.

### Streaming (for chat apps)

Without `"stream": false`, Ollama returns the response token by token.
This creates a "live typing" effect in chat applications:
```bash
curl http://localhost:11434/api/chat -d '{
  "model": "qwen3.5:4b",
  "messages": [{"role": "user", "content": "Tell a story about a robot"}]
}'
```

### List models

```bash
curl http://localhost:11434/api/tags
```

### Python (requests library)

```python
import requests
import json

response = requests.post(
    "http://localhost:11434/api/chat",
    json={
        "model": "qwen3.5:4b",
        "messages": [{"role": "user", "content": "Hello!"}],
        "stream": False
    }
)
print(response.json()["message"]["content"])
```

---

## 4. OpenAI-compatible endpoints

Ollama supports OpenAI API format. Any program that works with OpenAI can work with Ollama — just change the server address.

### Endpoints

| OpenAI endpoint | Ollama endpoint | Description |
|---|---|---|
| `/v1/chat/completions` | `/v1/chat/completions` | Chat |
| `/v1/completions` | `/v1/completions` | Text generation |
| `/v1/embeddings` | `/v1/embeddings` | Embeddings |
| `/v1/models` | `/v1/models` | Model list |

### curl

```bash
curl http://localhost:11434/v1/chat/completions -d '{
  "model": "qwen3.5:4b",
  "messages": [{"role": "user", "content": "Hello!"}]
}'
```

### Python (OpenAI SDK)

```python
from openai import OpenAI

client = OpenAI(
    base_url="http://localhost:11434/v1",
    api_key="ollama"  # any value works, Ollama ignores it
)

response = client.chat.completions.create(
    model="qwen3.5:4b",
    messages=[{"role": "user", "content": "Hello!"}]
)
print(response.choices[0].message.content)
```

### JavaScript

```javascript
import OpenAI from 'openai';

const client = new OpenAI({
  baseURL: 'http://localhost:11434/v1',
  apiKey: 'ollama',
});

const response = await client.chat.completions.create({
  model: 'qwen3.5:4b',
  messages: [{ role: 'user', content: 'Hello!' }],
});

console.log(response.choices[0].message.content);
```

### Using with Continue.dev

In `~/.continue/config.json`:

```json
{
  "models": [
    {
      "title": "Local chat",
      "provider": "openai",
      "model": "qwen3.5:4b",
      "apiBase": "http://localhost:11434/v1",
      "apiKey": "ollama"
    },
    {
      "title": "Autocomplete",
      "provider": "openai",
      "model": "qwen2.5-coder:1.5b",
      "apiBase": "http://localhost:11434/v1",
      "apiKey": "ollama"
    }
  ]
}
```

### Using with Aider

```bash
export OLLAMA_API_BASE=http://localhost:11434
aider --model ollama/qwen2.5-coder:7b
```

### Important differences from real OpenAI API

| Feature | OpenAI | Ollama |
|---|---|---|
| API key | Real key | Any string (or `ollama`) |
| Streaming | Default | Default |
| `max_tokens` | Works | Works |
| `temperature` | Works | Works |
| `tools` / `functions` | Works | Supported (tool calling) |
| `response_format` | `json_object` | `format: "json"` or JSON Schema |
| GPT-4 models | Yes | No (only local) |

---

## 5. Parallel requests and multiple models

By default, Ollama processes requests sequentially: one request — one response.
But you can enable parallel processing.

### Parallel requests to one model

Set `OLLAMA_NUM_PARALLEL` before starting the server:

```bash
export OLLAMA_NUM_PARALLEL=2
ollama serve
```

Now two apps (e.g., Continue.dev in VS Code + Open WebUI chat) can use the same model simultaneously.

**How many can you set?** Depends on your RAM. If a model weighs 4 GB and you have 16 GB,
2–3 is fine. If a model weighs 8 GB on a 16 GB machine — only 1.

**Check how many requests are currently being processed:**

```bash
curl http://localhost:11434/api/ps
```

### Multiple models in memory

```bash
export OLLAMA_MAX_LOADED_MODELS=3
ollama serve
```

Now you can keep up to 3 different models in RAM simultaneously.
Switching between them is instant (no reloading).

Example scenario:
1. `qwen3.5:4b` — fast chat (3.4 GB)
2. `qwen2.5-coder:7b` — coding (4.7 GB)
3. `all-minilm` — embeddings (0.1 GB)

Total: ~8 GB used, which is fine for a 16 GB Mac.

### Manually unload a model

```bash
# Via API
curl http://localhost:11434/api/generate -d '{
  "model": "qwen3.5:4b",
  "keep_alive": 0
}'
```

Or use `ollama ps` to see what's loaded first:

```bash
ollama ps
```

### How to tell a model has been unloaded

Run `ollama ps` — it shows an empty list if no model is loaded.

**Important:** If a model is not in use, Ollama unloads it automatically after
`OLLAMA_KEEP_ALIVE` (default 5 minutes). You can reduce this:

```bash
export OLLAMA_KEEP_ALIVE=30s   # unload after 30 seconds of inactivity
```

---

## 6. Speculative decoding (speedup)

A **small fast model** (draft model) drafts tokens, a **large model** validates them.

**Result:** generation speeds up 1.5–2× without quality loss.

### How it works

1. The draft model (small) quickly guesses the next 5–10 tokens
2. The large model checks them all at once (in parallel)
3. If the draft guessed correctly — accept; if wrong — the large model corrects

### Setup via Modelfile

```dockerfile
FROM qwen2.5-coder:7b
DRAFT qwen2.5-coder:1.5b
```

```bash
ollama create my-coder-sd -f ./Modelfile
ollama run my-coder-sd
```

### Setup via API

```bash
curl http://localhost:11434/api/generate -d '{
  "model": "qwen2.5-coder:7b",
  "prompt": "Write a sorting function in Python",
  "options": {
    "draft": "qwen2.5-coder:1.5b"
  },
  "stream": false
}'
```

### Expected speedup

| Large model | Draft model | Speedup |
|---|---|---|
| 7B–9B | 1.5B same family | ×1.3–1.8 |
| 14B | 1.5B–4B | ×1.5–2.0 |
| 30B–70B | 7B–8B | ×1.5–2.5 |

**Rule:** draft model should be from the same family (Qwen + Qwen, Llama + Llama) and 4–10× smaller.

### Download draft models

```bash
ollama pull qwen2.5-coder:1.5b
ollama pull llama3.2:3b
```

### Performance example (M4 Pro 24 GB, Qwen3-Coder-30B)

| Mode | tok/s | Speedup |
|---|---|---|
| Without speculative decoding | 70 | 1× |
| With draft (Qwen 2.5 Coder 1.5B) | 110–120 | **×1.6** |

**When NOT to use:**
- Low RAM — the draft model takes extra space
- Model is already fast (>50 tok/s) — speedup is negligible
- Draft model from a different family — guess quality drops significantly

---

## 7. Whats next

| If you want | Go to |
|-------------|-------|
| Compare all tools (MLX, llama.cpp, vLLM) | [tools.md](tools.md) |
| Choose quantization (Q4, Q5, Q8) | [quantization.md](quantization.md) |
| Benchmarks on Mac | [benchmarks/apple-silicon.md](benchmarks/apple-silicon.md) |
| Full model catalog | [catalog.md](catalog.md) |
| Fix issues | [troubleshooting.md](troubleshooting.md) |
| Back | [README.md](README.md) |

---

**In section:** [getting-started](getting-started.md) · [running-models](running-models.md) · [models](models.md) · [catalog](catalog.md) · [quantization](quantization.md) · [memory-and-context](memory-and-context.md) · [tools](tools.md) · [advanced-setup](advanced-setup.md) · [troubleshooting](troubleshooting.md) · [apple-silicon](benchmarks/apple-silicon.md)  
**Related sections:** [Zero Level](../basics/README.md) · [AI Agents](../agents/README.md) · [Use Cases](../use-cases/README.md)  
**Navigation:** [← Local Models](README.md) · [↑ Back to main](../README.md) · [🇷🇺 Русский](advanced-setup.ru.md)
