# ⚙️ Advanced Ollama Setup

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

Tune Ollama behavior with environment variables.

```bash
# Per session
export OLLAMA_KV_CACHE_TYPE=q4_0
ollama serve

# Permanent (add to ~/.zshrc)
echo 'export OLLAMA_KV_CACHE_TYPE=q4_0' >> ~/.zshrc
echo 'export OLLAMA_FLASH_ATTENTION=1' >> ~/.zshrc
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
source ~/.zshrc
```

---

## 2. Modelfile — custom models

**Modelfile** is like a Dockerfile but for AI models. You can customize parameters, system prompts, and templates.

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
| `seed` | 0 | Random seed (for reproducibility) | `42` for consistent answers |
| `repeat_penalty` | 1.1 | Repetition penalty | `1.2` if model loops |

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

### Streaming (for chat apps)

Without `"stream": false`, Ollama returns tokens one by one:
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
```

### Using with Continue.dev

```json
{
  "models": [
    {
      "title": "Local chat",
      "provider": "openai",
      "model": "qwen3.5:4b",
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

---

## 5. Parallel requests and multiple models

```bash
export OLLAMA_NUM_PARALLEL=2
ollama serve
```

Now two apps (e.g., Continue.dev + Open WebUI) can use the same model simultaneously.

```bash
export OLLAMA_MAX_LOADED_MODELS=3
ollama serve
```

Example with 3 models loaded:
1. `qwen3.5:4b` — fast chat (3.4 GB)
2. `qwen2.5-coder:7b` — coding (4.7 GB)
3. `all-minilm` — embeddings (0.1 GB)

---

## 6. Speculative decoding (speedup)

A **small fast model** drafts tokens, a **large model** validates them.

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
- Low RAM (draft model takes extra space)
- Model already fast (>50 tok/s)
- Draft from a different family (low quality guesses)

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
