# <img src="https://lucide.dev/api/icons/alert-triangle" alt="" width="20" height="20" style="vertical-align:middle"> Common Problems

> Diagnostics and typical issues when running LLMs locally.

**🇷🇺 Russian version:** [troubleshooting.ru.md](troubleshooting.ru.md)

---

[← Local models](../README.md)

---

| Problem | Cause | Solution |
|---------|-------|----------|
| **Out of memory** | Model too large for RAM | Use smaller model or Q4 quantization |
| **Slow generation** | CPU mode, no GPU | Check GPU usage: `ollama ps` |
| **Model not found** | Typo in name | `ollama list` to see available |
| **API connection refused** | Ollama not running | `ollama serve` or check tray icon |
| **GPU not used** | Missing drivers | Install CUDA Toolkit / NVIDIA drivers |
| **Context too short** | Default 2048 | `OLLAMA_CONTEXT_LENGTH=32768` |
| **Model downloads slowly** | Large file (4-40 GB) | Check internet, use a download manager |
| **Chinese text in response** | Wrong model for language | Use Qwen 3.5 for Russian |

## Diagnostics

```bash
# Check Ollama status
curl http://localhost:11434/api/version

# Check running models
ollama ps

# View server logs
journalctl -u ollama -f  # Linux
tail -f ~/.ollama/logs/server.log  # Mac
```

## Reinstall

```bash
# Mac
brew reinstall ollama

# Linux
curl -fsSL https://ollama.com/install.sh | sh

# Windows
# Re-run OllamaSetup.exe
```

---

[← Back](../README.md)

---

**In section:** [getting-started](getting-started.md) · [running-models](running-models.md) · [models](models.md) · [catalog](catalog.md) · [quantization](quantization.md) · [memory-and-context](memory-and-context.md) · [tools](tools.md) · [advanced-setup](advanced-setup.md) · [troubleshooting](troubleshooting.md) · [apple-silicon](benchmarks/apple-silicon.md)  
**Related sections:** [Zero Level](../basics/README.md) · [AI Agents](../agents/README.md) · [Use Cases](../use-cases/README.md)  
**Navigation:** [← Local Models](README.md) · [↑ Back to main](../README.md) · [🇷🇺 Русский](troubleshooting.ru.md)
