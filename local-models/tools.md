# <img src="https://lucide.dev/api/icons/wrench" alt="" width="20" height="20" style="vertical-align:middle"> Tools Comparison

> Complete comparison of local inference tools: Ollama, LM Studio, MLX, llama.cpp, vLLM, GPT4All.

**🇷🇺 Russian version:** [tools.ru.md](tools.ru.md)

---

[← Local models](README.md) · [Setup →](advanced-setup.md)

---

## Contents

1. [Overview](#1-overview)
2. [Ollama](#2-ollama)
3. [LM Studio](#3-lm-studio)
4. [llama.cpp](#4-llamacpp)
5. [MLX (Apple Silicon)](#5-mlx-apple-silicon)
6. [vLLM](#6-vllm)
7. [GPT4All](#7-gpt4all)
8. [Comparison table](#8-comparison-table)
9. [MLX vs llama.cpp benchmarks](#9-mlx-vs-llamacpp-benchmarks)
10. [Whats next](#10-whats-next)

---

## 1. Overview

There are many tools for running local LLMs. They differ in:
- **Supported formats** (GGUF, MLX, AWQ)
- **Hardware support** (CPU, CUDA, Metal)
- **Quantization options** (Q4, Q5, Q8, etc.)
- **API compatibility** (OpenAI, custom)
- **Ease of setup**

---

## 2. Ollama

**The most user-friendly option.** One command to download and run any model.

```bash
brew install ollama
ollama run qwen3.5:4b
```

**Key features:**
- OpenAI-compatible API (`/v1/chat/completions`)
- Tool calling (function calling) for agents
- Modelfile for customization
- Automatic GPU detection (CUDA + Metal)
- Docker support
- Parallel requests

**Best for:** General use, agents, beginners, production

---

## 3. LM Studio

**GUI application.** Download, load, and chat with models without terminal.

**Key features:**
- Visual model browser and search
- One-click download and load
- Built-in chat interface
- Local API server (OpenAI compatible)
- Model comparison mode

**Best for:** Beginners who prefer GUI, quick testing, visual model comparison

---

## 4. llama.cpp

**The engine behind Ollama.** Lower level, more control.

```bash
git clone https://github.com/ggml-org/llama.cpp
cd llama.cpp
make -j

# Run a model
./main -m model.gguf -p "Hello" -n 128
```

**Key features:**
- All GGUF quantizations (Q2_K through Q8_0)
- Maximum performance tuning
- Server mode with OpenAI API
- Extensive parameter control
- Flash attention

**Best for:** Power users, fine-tuning, benchmarking, embedding in custom apps

---

## 5. MLX (Apple Silicon)

**Apple's ML framework.** Optimized for M series chips.

```bash
pip install mlx-lm
mlx_lm.generate --model Qwen/Qwen3.5-4B --prompt "Hello"
```

**Key features:**
- Up to 46% faster than llama.cpp on Apple Silicon
- 49% less memory usage
- Native Metal acceleration
- 2/3/4/6 bit quantization
- Good for fine-tuning (LoRA)

**Best for:** Mac users wanting maximum speed, fine-tuning on Apple Silicon

---

## 6. vLLM

**Production serving engine.** For high-throughput, multi-user scenarios.

```bash
pip install vllm
vllm serve Qwen/Qwen3.5-4B
```

**Key features:**
- PagedAttention for efficient memory
- Continuous batching
- OpenAI-compatible API
- AWQ/GPTQ quantization
- Kubernetes ready

**Best for:** Production serving, multiple users, high throughput

---

## 7. GPT4All

**Privacy-focused.** Runs entirely on CPU, no GPU needed.

```bash
pip install gpt4all
```

**Key features:**
- No GPU required
- Built-in RAG
- Local plugin ecosystem
- Very easy setup

**Best for:** CPU-only machines, privacy-critical applications

---

## 8. Comparison table

| Feature | Ollama | LM Studio | llama.cpp | MLX | vLLM | GPT4All |
|---------|--------|-----------|-----------|-----|------|---------|
| Setup | 1 command | Download | Build from source | pip install | pip/Docker | pip install |
| Platform | Mac/Lin/Win | Mac/Lin/Win | Mac/Lin/Win | Mac only | Linux | Mac/Lin/Win |
| GPU support | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> CUDA+Metal | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> CUDA+Metal | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> CUDA+Metal | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> (M only) | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> (CUDA) | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> (CPU only) |
| Quantizations | K-quants | K-quants | K-quants | 2-6 bit | AWQ/GPTQ | K-quants |
| Tool calling | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> |
| OpenAI API | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> |
| Docker | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> |
| Parallel req. | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> |
| Multi-model | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | Manual | Manual | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> |
| Speed (M1 7B) | 22-25 t/s | 22-28 t/s | 20-25 t/s | 28-35 t/s | N/A | 10-15 t/s |

---

## 9. MLX vs llama.cpp benchmarks

### M4 Pro 24 GB, Qwen3-Coder-30B MoE

| Metric | LM Studio (MLX) | Ollama (llama.cpp) | Difference |
|--------|-----------------|-------------------|------------|
| **Throughput** | **102 tok/s** | 70 tok/s | **+46%** |
| TTFT | 291 ms | **175 ms** | Ollama faster |
| GPU Power | **12.4 W** | 15.4 W | **-20%** |
| Memory | **21.4 GB** | 41.6 GB | **-49%** |

### MLX backend in Ollama

Since March 2026, Ollama can use MLX backend on Mac with 32 GB+ RAM:
- Qwen 3.5-35B-A3B: 58 → **112 tok/s** on M5 Max (+93%)
- Currently works with Qwen models, Llama/Mistral coming

---

## 10. Whats next

| Go to | Description |
|-------|-------------|
| [advanced-setup.md](advanced-setup.md) | Modelfile, API tuning |
| [benchmarks/apple-silicon.md](benchmarks/apple-silicon.md) | Speed on Mac |
| [quantization.md](quantization.md) | Compression guide |
| Back | [README.md](README.md) |

[← Back to navigation](README.md)

---

**In section:** [getting-started](getting-started.md) · [running-models](running-models.md) · [models](models.md) · [catalog](catalog.md) · [quantization](quantization.md) · [memory-and-context](memory-and-context.md) · [tools](tools.md) · [advanced-setup](advanced-setup.md) · [troubleshooting](troubleshooting.md) · [apple-silicon](benchmarks/apple-silicon.md)  
**Related sections:** [Zero Level](../basics/README.md) · [AI Agents](../agents/README.md) · [Use Cases](../use-cases/README.md)  
**Navigation:** [← Local Models](README.md) · [↑ Back to main](../README.md) · [🇷🇺 Русский](tools.ru.md)
