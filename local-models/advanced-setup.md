# ⚙️ Advanced Ollama Setup

> Modelfile, environment variables, API tuning, running multiple models.

**🇷🇺 Russian version:** [advanced-setup.ru.md](advanced-setup.ru.md)

---

[← Local models](../README.md) · [Tools →](tools.md)

---

## Modelfile

```dockerfile
FROM qwen3.5:4b

# Set parameters
PARAMETER temperature 0.7
PARAMETER top_p 0.9
PARAMETER num_ctx 16384

# Custom system prompt
SYSTEM """You are an expert Python developer.
Answer concisely with code examples."""

# Add template
TEMPLATE """{{ .System }}
User: {{ .Prompt }}
Assistant:"""
```

```bash
ollama create my-coder -f ./Modelfile
ollama run my-coder
```

## Environment variables

```bash
# Network
export OLLAMA_HOST=0.0.0.0:11434  # listen on all interfaces

# Performance
export OLLAMA_CONTEXT_LENGTH=32768
export OLLAMA_FLASH_ATTENTION=1
export OLLAMA_KV_CACHE_TYPE=q8_0
export OLLAMA_NUM_PARALLEL=4  # parallel requests

# Memory
export OLLAMA_GPU_OVERHEAD=512  # MB reserved for GPU
```

## API tuning

```bash
# Increase batch size for faster generation
curl http://localhost:11434/api/generate -d '{
  "model": "qwen3.5:4b",
  "prompt": "Write code...",
  "options": {
    "num_batch": 512,
    "num_ctx": 32768,
    "f16_kv": true
  }
}'
```

## What's next

| Go to | Description |
|-------|-------------|
| [tools.md](tools.md) | Compare inference engines |
| Back | [README.md](../README.md) |
