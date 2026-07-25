# 🚀 How to Find and Run a Model

> Practical guide: from first command to advanced HuggingFace workflows.

**🇷🇺 Russian version:** [running-models.ru.md](running-models.ru.md)

---

[← Local models](../README.md) · [Model selection →](models.md)

---

Covers: Ollama, LM Studio, HuggingFace integration.

## Ollama (recommended)

```bash
# Search for models
ollama search qwen

# Pull and run
ollama run qwen3.5:4b

# Custom model from Modelfile
ollama create mymodel -f ./Modelfile
ollama run mymodel
```

## LM Studio

1. Download from [lmstudio.ai](https://lmstudio.ai)
2. Search and download models from the GUI
3. Load a model and chat
4. Start local API server for dev

## HuggingFace + Ollama

```bash
# Import any GGUF model from HuggingFace
ollama pull hf.co/bartowski/qwen3.5-4b-GGUF

# Manual import
ollama create mymodel --file Modelfile
```

---

## API usage

```python
import requests

r = requests.post("http://localhost:11434/api/generate", json={
    "model": "qwen3.5:4b",
    "prompt": "What is AI?",
    "stream": False
})
print(r.json()["response"])
```

---

## What's next

| Go to | Description |
|-------|-------------|
| [models.md](models.md) | Choosing the right model |
| [advanced-setup.md](advanced-setup.md) | Modelfile, API tuning |
| [tools.md](tools.md) | Compare all inference engines |
| Back | [README.md](../README.md) |

[← Back to navigation](README.md)

---

**In section:** [getting-started](getting-started.md) · [running-models](running-models.md) · [models](models.md) · [catalog](catalog.md) · [quantization](quantization.md) · [memory-and-context](memory-and-context.md) · [tools](tools.md) · [advanced-setup](advanced-setup.md) · [troubleshooting](troubleshooting.md) · [apple-silicon](benchmarks/apple-silicon.md)  
**Related sections:** [Zero Level](../basics/README.md) · [AI Agents](../agents/README.md) · [Use Cases](../use-cases/README.md)  
**Navigation:** [← Local Models](README.md) · [↑ Back to main](../README.md) · [🇷🇺 Русский](running-models.ru.md)
