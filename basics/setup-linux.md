# 🐧 Installing Ollama on Linux

> Complete guide for Linux: CPU, CUDA, Docker, systemd, troubleshooting.

**🇷🇺 Russian version:** [setup-linux.ru.md](setup-linux.ru.md)

---

[← Install on Windows](setup-windows.md) · [Learning path →](learning-path.md)

---

## Contents

1. [Simple install (CPU)](#1-simple-install-cpu)
2. [Install with GPU (NVIDIA CUDA)](#2-install-with-gpu-nvidia-cuda)
3. [Install via Docker](#3-install-via-docker)
4. [Server configuration (systemd)](#4-server-configuration-systemd)
5. [Run your first model](#5-run-your-first-model)
6. [Coding setup](#6-coding-setup)
7. [Common issues](#7-common-issues)
8. [Whats next](#8-whats-next)

---

## 1. Simple install (CPU)

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

```bash
# Check status
systemctl status ollama

# View logs
journalctl -u ollama -f
```

```bash
ollama --version
curl http://localhost:11434/api/version
```

---

## 2. Install with GPU (NVIDIA CUDA)

```bash
# NVIDIA Container Toolkit
sudo apt install nvidia-container-toolkit   # Debian/Ubuntu
sudo dnf install nvidia-container-toolkit   # Fedora
```

---

## 3. Install via Docker

```bash
# With GPU (NVIDIA)
docker run -d --gpus all -p 11434:11434 \
  -v ollama:/root/.ollama \
  --name ollama ollama/ollama

# Without GPU (CPU only)
docker run -d -p 11434:11434 \
  -v ollama:/root/.ollama \
  --name ollama ollama/ollama
```

```bash
docker run -d --gpus all -p 11434:11434 \
  -e OLLAMA_CONTEXT_LENGTH=32768 \
  -e OLLAMA_FLASH_ATTENTION=1 \
  -e OLLAMA_KV_CACHE_TYPE=q8_0 \
  -v ollama:/root/.ollama \
  --name ollama ollama/ollama
```

---

## 4. Server configuration (systemd)

```bash
sudo systemctl edit ollama.service
```

Add:
```
[Service]
Environment="OLLAMA_HOST=0.0.0.0:11434"
Environment="OLLAMA_CONTEXT_LENGTH=32768"
Environment="OLLAMA_FLASH_ATTENTION=1"
Environment="OLLAMA_KV_CACHE_TYPE=q8_0"
```

Restart:
```bash
sudo systemctl daemon-reload
sudo systemctl restart ollama
```

---

## 5. Run your first model

```bash
ollama run qwen3.5:4b
```

After download (~3.4 GB) you will see the `>>>` prompt.

Try:
```
Write a bash script that finds all files larger than 100 MB
Explain the difference between soft and hard links in Linux
```

---

## 6. Coding setup

### VS Code + Continue

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

### Aider

```bash
pip install aider-chat
export OLLAMA_API_BASE=http://127.0.0.1:11434
aider --model ollama_chat/qwen2.5-coder:7b
```

More: [`../use-cases/coding.md`](../use-cases/coding.md)

---

## 7. Common issues

### Permission denied
```bash
sudo usermod -aG docker $USER
```

### GPU not used in Docker
```bash
sudo apt install nvidia-container-toolkit
sudo systemctl restart docker
docker run -d --gpus all ...
```

### Model crashes with out of memory
- Reduce context: `OLLAMA_CONTEXT_LENGTH=4096`
- Use stronger quantization: `qwen3.5:4b:q3_k_m`
- Close other RAM-heavy programs

### "libcuda.so not found"
```bash
sudo apt install nvidia-cuda-toolkit   # Ubuntu/Debian
sudo dnf install cuda                  # Fedora
```

### Stop the server
```bash
sudo systemctl stop ollama
docker stop ollama
```

---

## 8. Whats next

| If you want | Go to |
|-------------|-------|
| **Choose a model for your task** | [`../local-models/models.md`](../local-models/models.md) |
| **Set up AI coding** | [`../use-cases/coding.md`](../use-cases/coding.md) |
| **Compare tools** | [`../local-models/tools.md`](../local-models/tools.md) |
| **Learn about quantization** | [`../local-models/quantization.md`](../local-models/quantization.md) |
| **Back to learning path** | [learning-path.md](learning-path.md) |
| **Back to navigation** | [README.md](README.md) |
