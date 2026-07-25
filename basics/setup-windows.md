# 🪟 Installing Ollama on Windows

> Complete guide for Windows: native install, WSL2, GPU acceleration, troubleshooting.

**🇷🇺 Russian version:** [setup-windows.ru.md](setup-windows.ru.md)

---

[← Hardware guide](hardware-guide.md) · [Install on Linux →](setup-linux.md)

---

## Contents

1. [Two installation methods](#1-two-installation-methods)
2. [Method 1: Native install (recommended)](#2-method-1-native-install-recommended)
3. [Method 2: Install via WSL2](#3-method-2-install-via-wsl2)
4. [Verification](#4-verification)
5. [Run your first model](#5-run-your-first-model)
6. [Coding setup](#6-coding-setup)
7. [Common issues](#7-common-issues)
8. [Whats next](#8-whats-next)

---

## 1. Two installation methods

| Method | Difficulty | GPU | For |
|--------|-----------|-----|-----|
| **Native** | Easy (one command) | NVIDIA CUDA | Most users |
| **WSL2** | Medium | NVIDIA CUDA | Developers, Continue.dev |

---

## 2. Method 1: Native install (recommended)

1. Download: [ollama.com/download/windows](https://ollama.com/download/windows)
2. Run `OllamaSetup.exe`
3. Wait (Ollama icon appears in system tray)

Open **PowerShell** or **Command Prompt**:
```powershell
ollama --version
```

Ollama starts automatically at login. Icon in system tray.

Check:
```powershell
curl http://localhost:11434/api/version
```

---

## 3. Method 2: Install via WSL2

Open PowerShell as Administrator:
```powershell
wsl --install
```

### Step 2. Install Ollama inside WSL2

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

### Step 3. Use from Windows

Ollama inside WSL2 is available at `localhost:11434` from Windows.
VS Code, Continue.dev and other programs connect to it as local.

---

## 4. Verification

```powershell
# Check version
ollama --version

# Check installed models
ollama list

# Search available models
ollama search qwen
```

---

## 5. Run your first model

```powershell
ollama run qwen3.5:4b
```

Try:
```
Write a Python function that checks if a number is prime
Explain the difference between list and tuple in Python
Translate to Russian: "I need to install an AI model locally"
```

**Commands during chat:**
- `/bye` — exit
- `/clear` — clear history
- `Ctrl+C` — stop generation

---

## 6. Coding setup

### VS Code + Continue

1. Install [VS Code](https://code.visualstudio.com/)
2. Install **Continue** extension
3. Configure:
   ```json
   {
     "models": [{
       "title": "Local Qwen",
       "provider": "ollama",
       "model": "qwen2.5-coder:7b",
       "apiBase": "http://localhost:11434"
     }]
   }
   ```

More: [`../use-cases/coding.md`](../use-cases/coding.md)

---

## 7. Common issues

### Ollama does not see GPU (NVIDIA)
- Install [CUDA Toolkit](https://developer.nvidia.com/cuda-downloads)
- Update NVIDIA drivers
- Check: `ollama ps` should show `100% GPU`

### WSL2 is slow
- Ensure WSL2, not WSL1: `wsl -l -v`
- Disable Large Send Offload in network adapter settings

### Model does not fit in RAM
- Use Q4 quantization
- Choose a smaller model: `phi4-mini` instead of `qwen3.5:7b`

### "connection refused" error
- Make sure Ollama is running (tray icon)
- Check: `curl http://localhost:11434/api/version`

---

## 8. Whats next

| If you want | Go to |
|-------------|-------|
| **Choose models** | [`../local-models/running-models.md`](../local-models/running-models.md) |
| **Set up AI coding** | [`../use-cases/coding.md`](../use-cases/coding.md) |
| **Open WebUI** | `docker run -d -p 3000:8080 ghcr.io/open-webui/open-webui:main` |
| **Back to learning path** | [learning-path.md](learning-path.md) |
| **Back to navigation** | [README.md](README.md) |

---

**In section:** [what-is-ai](what-is-ai.md) · [how-models-work](how-models-work.md) · [cloud-vs-local](cloud-vs-local.md) · [hardware-guide](hardware-guide.md) · [glossary](glossary.md) · [faq](faq.md) · [learning-path](learning-path.md) · [setup-windows](setup-windows.md) · [setup-linux](setup-linux.md)  
**Related sections:** [Local Models](../local-models/README.md) · [AI Agents](../agents/README.md) · [Use Cases](../use-cases/README.md)  
**Navigation:** [← Zero Level](README.md) · [↑ Back to main](../README.md) · [🇷🇺 Русский](setup-windows.ru.md)
