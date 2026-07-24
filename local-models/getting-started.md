# 🚀 Getting Started

> Step-by-step guide for first-time local AI users.

**🇷🇺 Russian version:** [getting-started.ru.md](getting-started.ru.md)

---

[← Local models](../README.md) · [Running models →](running-models.md)

---

This guide covers: terminal basics, macOS check, Homebrew & Ollama installation, first model run.

```bash
# 1. Check your Mac
# Apple Silicon (M1/M2/M3/M4) recommended
uname -m  # should say "arm64"
sysctl -n hw.memsize  # RAM in bytes

# 2. Install Homebrew (if not installed)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# 3. Install Ollama
brew install ollama

# 4. Start Ollama
ollama serve

# In another terminal:
# 5. Run your first model
ollama run qwen3.5:4b
```

```bash
# Download a coding model
ollama pull qwen2.5-coder:7b

# List installed models
ollama list

# Show model info
ollama show qwen3.5:4b
```

---

## First chat

After `ollama run qwen3.5:4b`, try:
```
>>> What is a neural network?
>>> Write a Python function to sort a list
>>> /bye  # exit
```

---

## What's next

| Step | Go to |
|------|-------|
| Run models from code | [running-models.md](running-models.md) |
| Choose a model | [models.md](models.md) |
| Set up AI coding | [../use-cases/coding.md](../use-cases/coding.md) |
| Back | [README.md](../README.md) |

[← Back to navigation](README.md)
