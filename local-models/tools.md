# <img src="https://lucide.dev/api/icons/wrench" alt="" width="20" height="20" style="vertical-align:middle"> Tools Comparison

> Complete comparison of local inference tools: Ollama, LM Studio, MLX, llama.cpp, vLLM, GPT4All, and more.

**🇷🇺 Russian version:** [tools.ru.md](tools.ru.md)

---

[← Local models](README.md) · [Setup →](advanced-setup.md)

---

## Contents

1. [Overview](#1-overview)
2. [Scenarios: which tool to choose](#2-scenarios-which-tool-to-choose)
3. [Ollama](#3-ollama)
4. [LM Studio](#4-lm-studio)
5. [llama.cpp](#5-llamacpp)
6. [MLX (Apple Silicon)](#6-mlx-apple-silicon)
7. [vLLM](#7-vllm)
8. [GPT4All](#8-gpt4all)
9. [Jan](#9-jan)
10. [Open WebUI](#10-open-webui)
11. [Aider](#11-aider)
12. [Continue.dev](#12-continuedev)
13. [Enchanted](#13-enchanted)
14. [LocalAI](#14-localai)
15. [MindWork AI Studio](#15-mindwork-ai-studio)
16. [Comparison table](#16-comparison-table)
17. [Benchmarks on Apple Silicon](#17-benchmarks-on-apple-silicon)
18. [Scaling and Production](#18-scaling-and-production)
19. [Decision Tree](#19-decision-tree)
20. [Installation guides](#20-installation-guides)
21. [Terminology glossary](#21-terminology-glossary)
22. [What's next](#22-whats-next)

---

## 1. Overview

There are many tools for running local LLMs. They differ in:
- **Supported formats** (GGUF, MLX, AWQ)
- **Hardware support** (CPU, CUDA, Metal)
- **Quantization options** (Q4, Q5, Q8, etc.)
- **API compatibility** (OpenAI, custom)
- **Ease of setup**

---

## 2. Scenarios: which tool to choose

| Scenario | Tool | Why |
|----------|------|-----|
| **Download and test models** (discovery) | **LM Studio** | Built-in HuggingFace search by category, visual browser |
| **Need an API for your app** (integration) | **Ollama** | 3 commands, OpenAI API, any language |
| **Maximum speed on Mac** (performance) | **MLX** (mlx-lm) or LM Studio | +20–40% tok/s, –50% RAM |
| **Need to fine-tune a model** (training) | **MLX** (mlx-lm) | LoRA/QLoRA, native Apple, Python API |
| **Private RAG on documents** (RAG) | **GPT4All** (single user) or **Open WebUI** (team) | LocalDocs / 9 vector databases |
| **AI assistant in VS Code** (coding-IDE) | **Continue.dev + Ollama** | Tab autocomplete, inline editing, @codebase |
| **Autonomous coding agent in CLI** (coding-agent) | **Aider + Ollama** | Architect/editor split, repo map |
| **Production server** (deployment) | **vLLM** (Linux) or LM Studio (Mac) | Continuous batching, PagedAttention |
| **Complete beginner** (beginner) | **LM Studio** | GUI, no terminal commands needed |
| **Maximum privacy** (privacy) | **Ollama + GPT4All** | Everything local, zero external connections |
| **Long context 100K+** (context) | **llama.cpp** (raw) | Speculative decoding, KV cache control |
| **Batch processing thousands of prompts** (batch) | **vLLM** (Linux) or **llama.cpp** (Mac) | Continuous batching |

---

## 3. Ollama

**The most user-friendly option.** One command to download and run any model.

```bash
brew install ollama
ollama run qwen3.5:4b
```

| Parameter | Value |
|-----------|-------|
| **Interface** | CLI + HTTP API |
| **Open Source** | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> MIT |
| **Stars** | 148K+ |
| **Apple Silicon** | Metal (native) + MLX (v0.19+, 32GB+ Mac) |
| **GPU support** | Metal (Mac), CUDA, ROCm, Vulkan |
| **GitHub** | [ollama/ollama](https://github.com/ollama/ollama) |

**How it works under the hood:**
- Wrapper around llama.cpp with automatic model downloading
- Models stored in `~/.ollama/models/blobs/` in GGUF format
- On run: checks locally → downloads if missing → loads into RAM → starts llama.cpp backend
- HTTP API on port 11434, compatible with OpenAI

**GPU Support Matrix:**

| Platform | Backend | Status |
|----------|---------|--------|
| Apple Silicon | Metal | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Native |
| Apple Silicon | MLX | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> (v0.19+, 32GB+ RAM only, limited models) |
| NVIDIA Linux | CUDA | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> |
| NVIDIA Windows | CUDA | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> |
| AMD Linux | ROCm | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> |
| AMD Windows | ROCm | <img src="https://lucide.dev/api/icons/alert-triangle" alt="" width="20" height="20" style="vertical-align:middle"> Experimental |
| Intel GPU | Vulkan | <img src="https://lucide.dev/api/icons/alert-triangle" alt="" width="20" height="20" style="vertical-align:middle"> Limited |
| CPU only | llama.cpp | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Always fallback |

**Key features:**
- OpenAI-compatible API (`/v1/chat/completions`)
- Tool calling (function calling) for agents
- `Modelfile` — custom models (temperature, system prompt, template)
- Automatic GPU detection (CUDA + Metal)
- Docker support
- Parallel requests via `OLLAMA_NUM_PARALLEL`
- Multimodal (vision models)
- `OLLAMA_KV_CACHE_TYPE` — KV cache quantization control
- `OLLAMA_MAX_LOADED_MODELS` — multiple models in memory

**When to choose Ollama:**
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Need a simple API for integration (Continue.dev, Aider, Open WebUI)
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Want one command `ollama run <model>` with no config
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Need cross-platform (Mac + Linux + Windows)
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Ollama 0.19+ — automatic MLX backend for compatible models
- <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> Need maximum speed (MLX is 20–40% faster)
- <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> Need fine-grained inference control (llama.cpp gives more)

**Best for:** General use, agents, beginners, production

---

## 4. LM Studio

**GUI application.** Download, load, and chat with models without terminal.

| Parameter | Value |
|-----------|-------|
| **Interface** | GUI + Headless daemon |
| **Open Source** | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> Closed source |
| **Apple Silicon** | MLX (auto-detection) + llama.cpp |
| **OS** | macOS, Windows, Linux |
| **Website** | [lmstudio.ai](https://lmstudio.ai) |

**Key features:**
- Built-in model search on HuggingFace (by category, size)
- One-click download and load
- Automatic backend selection: MLX for Mac, CUDA for NVIDIA
- Drag-and-drop GGUF files
- Built-in chat with system prompt, template, parameters
- Local API server (OpenAI compatible)
- Headless daemon since v0.3 (runs in background)
- Model comparison mode

**LM Studio vs Ollama on M4 Pro 24GB (Qwen3-Coder-30B MoE):**

| Metric | LM Studio (MLX) | Ollama (llama.cpp) | Difference |
|--------|-----------------|-------------------|------------|
| Throughput | **102 tok/s** | 70 tok/s | **+46%** |
| TTFT | 291 ms | **175 ms** | Ollama faster |
| GPU Power | **12.4 W** | 15.4 W | **–20%** |
| Efficiency | **8.2 tok/s/W** | 4.5 tok/s/W | **+82%** |
| Process Memory | **21.4 GB** | 41.6 GB | **–49%** |

*(Source: asiai.dev)*

**When to choose LM Studio:**
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Just starting out — don't want to use CLI
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Want to quickly test different models
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Need maximum speed on Mac (MLX)
- <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> Need automation / CI (no CLI)
- <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> Need full open source

**Best for:** Beginners who prefer GUI, quick testing, visual model comparison

---

## 5. llama.cpp

**The engine behind Ollama.** Lower level, more control.

```bash
git clone https://github.com/ggml-org/llama.cpp
cd llama.cpp
make -j

# Run a model
./main -m model.gguf -p "Hello" -n 128
```

| Parameter | Value |
|-----------|-------|
| **Interface** | CLI + Server |
| **Open Source** | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> MIT |
| **Stars** | 70K+ |
| **Apple Silicon** | Metal (native) |
| **OS** | All platforms |
| **GitHub** | [ggml-org/llama.cpp](https://github.com/ggml-org/llama.cpp) |

**Unique features:**
- GGUF format: K-quants (Q4_K_M, Q5_K_M) and I-quants (IQ2_XXS, IQ3_XXS)
- All GGUF quantizations (Q2_K through Q8_0)
- Speculative decoding (1.5–2× faster)
- KV cache quantization (q8_0, q4_0)
- Built-in embedding endpoint
- Flash attention
- Batch processing
- LoRA adapters on the fly
- Extensive parameter control

**Speculative decoding (M3 Ultra 192GB, Llama 3.1 70B Q4):**

| Mode | tok/s | Speedup |
|------|-------|---------|
| Direct | 9.4 | 1× |
| **Speculative (70B+70B)** | **11.3** | **1.2×** |
| **Speculative (70B+8B)** | **15.1** | **1.6×** |

**When to choose llama.cpp:**
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Need cross-platform (GGUF everywhere)
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Want full control over inference
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Use non-standard quantizations
- <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> Want simplicity of one command

**Best for:** Power users, fine-tuning, benchmarking, embedding in custom apps

---

## 6. MLX (Apple Silicon)

**Apple's ML framework.** Optimized for M series chips.

```bash
pip install mlx-lm
mlx_lm.generate --model Qwen/Qwen3.5-4B --prompt "Hello"
```

| Parameter | Value |
|-----------|-------|
| **Interface** | Python API + CLI |
| **Open Source** | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Apache 2.0 |
| **Stars** | 18K+ |
| **Apple Silicon** | Native (built by Apple for Apple Silicon) |
| **OS** | macOS only |
| **GitHub** | [ml-explore/mlx](https://github.com/ml-explore/mlx) |

**What is MLX:**
MLX is Apple's **machine learning framework** for Apple Silicon. It includes:
- **mlx** — core (arrays, autograd, optimizers)
- **mlx-lm** — LLM layer (inference + fine-tuning)
- **mlx-examples** — examples (LoRA, QLoRA, adapters)

**Unlike llama.cpp:**
- Not a wrapper — native framework for Apple Silicon
- Supports **training** (fine-tuning, LoRA, QLoRA), not just inference
- Up to 2× faster than llama.cpp on Mac (native optimization)
- Uses uniform quantization (4-bit, 8-bit), not K-quants

**Key features:**
- Up to 46% faster than llama.cpp on Apple Silicon
- 49% less memory usage
- Native Metal acceleration
- 2/3/4/6 bit quantization
- Good for fine-tuning (LoRA)

**Fine-tuning with MLX:**

```python
from mlx_lm import load, generate
model, tokenizer = load("mlx-community/Qwen3-8B-4bit")

# LoRA fine-tuning
from mlx_lm import lora
lora.train_lora(
    model=model,
    tokenizer=tokenizer,
    train_set="data.jsonl",
    num_lora_layers=16,
    lora_rank=8,
)
```

**Performance (M5 Max 128GB, Llama 3.1 70B Q4):**

| Engine | tok/s | Memory |
|--------|-------|--------|
| **MLX** | **18** | ~39 GB |
| llama.cpp Metal | 14 | ~41 GB |
| Ollama (CPU) | 12 | ~41 GB |

**When to choose MLX:**
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Maximum speed on Mac
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Need fine-tuning / LoRA / QLoRA
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Python ecosystem
- <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> Mac only (not cross-platform)
- <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> Need a GUI

**Best for:** Mac users wanting maximum speed, fine-tuning on Apple Silicon

---

## 7. vLLM

**Production serving engine.** For high-throughput, multi-user scenarios.

```bash
pip install vllm
vllm serve Qwen/Qwen3.5-4B
```

| Parameter | Value |
|-----------|-------|
| **Interface** | HTTP API (OpenAI-compatible) |
| **Open Source** | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Apache 2.0 |
| **Stars** | 45K+ |
| **Apple Silicon** | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> Not supported (NVIDIA/AMD only) |
| **GitHub** | [vllm-project/vllm](https://github.com/vllm-project/vllm) |

**Key technologies:**
- **PagedAttention** — efficient KV cache management (eliminates fragmentation)
- **Continuous batching** — dynamic add/remove requests from batch
- **Prefix caching** — cache common prompt prefixes
- **Tensor parallelism** — distribute model across multiple GPUs
- **Speculative decoding** — built-in
- AWQ/GPTQ quantization
- Kubernetes ready

**When to use vLLM:**
- Production server with high load (10+ concurrent users)
- Batch processing thousands of prompts
- Multi-GPU cluster
- <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> **Not for Mac** (NVIDIA/AMD Linux only)

**Best for:** Production serving, multiple users, high throughput

---

## 8. GPT4All

**Privacy-focused.** Runs entirely on CPU, no GPU needed.

```bash
pip install gpt4all
```

| Parameter | Value |
|-----------|-------|
| **Interface** | GUI + Python API + CLI |
| **Open Source** | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> MIT |
| **Stars** | 72K+ |
| **Apple Silicon** | Metal |
| **GitHub** | [nomic-ai/gpt4all](https://github.com/nomic-ai/gpt4all) |

**LocalDocs (RAG):**
- Built-in document indexing (PDF, TXT, MD, DOCX)
- Vector database (custom Nomic implementation)
- Local file search — no data sent to the cloud
- Folder attachments (up to 10 collections)

**GPT4All vs Open WebUI for RAG:**

| Criteria | GPT4All | Open WebUI |
|----------|---------|------------|
| Setup | One `.dmg` | Docker / pip |
| Documents | LocalDocs | 9 vector databases |
| Multi-user | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> |
| API | Local | OpenAI-compatible |
| Resources | Lightweight | Heavier (Python) |
| Formats | PDF, TXT, MD, DOCX | Any via document loaders |

**Key features:**
- No GPU required
- Built-in RAG
- Local plugin ecosystem
- Very easy setup

**When to choose GPT4All:**
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Only need RAG on local documents
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Minimal setup (one click)
- <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> Need an API server / integrations
- <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> Multi-user access

**Best for:** CPU-only machines, privacy-critical applications

---

## 9. Jan

**Desktop AI client.** Beautiful GUI for running models locally with optional cloud model support.

| Parameter | Value |
|-----------|-------|
| **Interface** | GUI (Electron) |
| **Open Source** | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> AGPL (core), GUI closed |
| **Stars** | 35K+ |
| **Apple Silicon** | Metal |
| **GitHub** | [janhq/jan](https://github.com/janhq/jan) |

**Key features:**
- Model Hub — search, browse, and download models
- Vision model support (via llama.cpp)
- Remote model support (OpenAI, Anthropic, local servers)
- Thread management (conversation history)
- Local server mode

**When to choose Jan:**
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Need a polished desktop client
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Want to combine local and cloud models in one app
- <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> Need lightweight (Electron uses significant RAM)

---

## 10. Open WebUI

**Web interface for Ollama.** Feature-rich web UI with RAG, multi-user, and extensive backend support.

| Parameter | Value |
|-----------|-------|
| **Interface** | Web (Python + Svelte) |
| **Open Source** | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> MIT |
| **Stars** | 70K+ |
| **Install** | `docker run` |
| **GitHub** | [open-webui/open-webui](https://github.com/open-webui/open-webui) |

**Key features:**
- Connect to Ollama, OpenAI, Anthropic, Google, AWS Bedrock
- **9 vector databases:** Chroma, Milvus, Qdrant, Weaviate, PGVector, Elastic, Meilisearch, Pinecone, Supabase
- Agentic RAG — agent decides when and how to search documents
- Multi-user mode (teams)
- Web search, image generation, audio input
- Themes and customization

**When to choose Open WebUI:**
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Need a web interface for a team
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Need advanced RAG with agentic search
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Need support for 9+ vector databases
- <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> Only need single-user (overkill)

---

## 11. Aider

**CLI coding agent.** AI pair programming in your terminal — connects to local models via Ollama.

| Parameter | Value |
|-----------|-------|
| **Interface** | CLI |
| **Open Source** | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Apache 2.0 |
| **Stars** | 25K+ |
| **Local models** | Yes (via Ollama) |
| **GitHub** | [paul-gauthier/aider](https://github.com/paul-gauthier/aider) |

**Architecture:**
- **Architect/Editor** split — one plans, the other writes code
- **Repo map** — automatic repository map for context
- **Map-refine** — refines map as changes are made
- **Lint & test integration** — automatic code verification

**Quality with local models:**
- Qwen 2.5 Coder 7B — ~50% of GPT-4o quality on Aider tasks
- DeepSeek R1 7B — good for refactoring
- Qwen3-Coder-30B — ~70% of GPT-4o

**When to choose Aider:**
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Need an autonomous coding agent in CLI
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Need refactoring via prompts
- <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> Need a GUI / visual interface

---

## 12. Continue.dev

**IDE plugin.** AI assistant for VS Code and JetBrains with local model support.

| Parameter | Value |
|-----------|-------|
| **Interface** | IDE plugin (VS Code, JetBrains) |
| **Open Source** | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Apache 2.0 |
| **Stars** | 25K+ |
| **Local models** | Yes (via Ollama) |
| **GitHub** | [continuedev/continue](https://github.com/continuedev/continue) |

**How it works:**
- Connects to any OpenAI-compatible API (Ollama, LM Studio, vLLM)
- **Chat** — conversation with model inside IDE
- **Inline editing** — select code → prompt → model modifies
- **Tab autocomplete** — completions as you type (via separate model)
- **@codebase** — full repository context via embeddings

**Per-role model configuration:**

```json
{
  "models": [
    {
      "title": "Quick chat",
      "provider": "ollama",
      "model": "qwen3:4b"
    },
    {
      "title": "Autocomplete",
      "provider": "ollama",
      "model": "qwen2.5-coder:1.5b"
    },
    {
      "title": "Complex refactoring",
      "provider": "ollama",
      "model": "qwen2.5-coder:14b"
    }
  ]
}
```

**When to choose Continue.dev:**
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Work in VS Code / JetBrains
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Want an AI assistant without Copilot subscription
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Need tab autocomplete with local models
- <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> Don't use an IDE

---

## 13. Enchanted

**Native macOS/iOS client.** Minimalistic SwiftUI frontend for Ollama.

| Parameter | Value |
|-----------|-------|
| **Interface** | macOS + iOS GUI (SwiftUI) |
| **Open Source** | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> MIT |
| **Apple Silicon** | Native, connects to Ollama |
| **GitHub** | [gluonfield/enchanted](https://github.com/gluonfield/enchanted) |

**Key features:**
- Native macOS and iOS apps (SwiftUI)
- Connects to any Ollama server via URI (`http://localhost:11434`)
- Access from iPhone to Mac over local network
- Minimalistic Apple-style interface
- Markdown rendering, image support

**When to choose Enchanted:**
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Need an iPhone/iPad client in addition to Mac
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Like minimalistic design
- <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> Don't need iOS — Ollama CLI or Open WebUI is simpler

---

## 14. LocalAI

**OpenAI API drop-in replacement.** Full OpenAI API including TTS, STT, and image generation.

| Parameter | Value |
|-----------|-------|
| **Interface** | HTTP API (OpenAI-compatible) |
| **Open Source** | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> MIT |
| **Stars** | 30K+ |
| **Install** | Docker, binaries |
| **GitHub** | [mudler/LocalAI](https://github.com/mudler/LocalAI) |

**Backend support (60+):**
- llama.cpp, transformers, diffusers, whisper, piper-tts, stable-audio
- Image generation, text-to-speech, speech-to-text
- Embeddings, reranking
- Model gallery (YAML configs)

**When to choose LocalAI:**
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Need full OpenAI API (including TTS, STT, images)
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Already have Docker infrastructure
- <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> Only need LLM (overkill)

---

## 15. MindWork AI Studio

**Universal multi-provider GUI.** One interface for local and cloud models.

| Parameter | Value |
|-----------|-------|
| **Interface** | GUI (macOS, Windows, Linux) |
| **Open Source** | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> FSL-1.1-MIT (→ MIT in 2 years) |
| **Stars** | 529 |
| **Apple Silicon** | Native |
| **GitHub** | [MindWorkAI/AI-Studio](https://github.com/MindWorkAI/AI-Studio) |

**Provider support:**

| Type | Providers |
|------|-----------|
| **Local** | Ollama, LM Studio, llama.cpp, vLLM |
| **Cloud** | OpenAI, Anthropic, Google Gemini, Mistral, Perplexity, xAI (Grok), DeepSeek, OpenRouter, Groq |
| **HuggingFace** | Cerebras, Nebius, Together AI, Fireworks and more |

**Key features:**
- Unified interface for local and cloud models — switch without restart
- Assistants for common tasks (translation, document analysis, slide generation)
- Image generation (DALL-E, Stable Diffusion)
- RAG (Qdrant, external data via ERI)
- Lua plugins, i18n, enterprise configurations
- Free for commercial use

**When to choose MindWork AI Studio:**
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Need one GUI for all providers (local + cloud)
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Want to quickly switch between models without configuration
- <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Need assistants for business tasks
- <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> Only need local inference — Ollama or LM Studio is simpler
- <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> Need CLI/API for automation — not a replacement for Ollama

---

## 16. Comparison table

| Feature | Ollama | LM Studio | llama.cpp | MLX | vLLM | GPT4All | Jan | Open WebUI | Aider | Continue.dev | Enchanted | LocalAI | MindWork AI Studio |
|---------|--------|-----------|-----------|-----|------|---------|-----|------------|-------|-------------|-----------|---------|-------------------|
| **Type** | Engine | Engine+GUI | Engine | Engine | Engine | Engine+GUI | Desktop UI | Web UI | CLI Agent | IDE Plugin | Mobile UI | Engine+API | Multi-GUI |
| **Setup** | 1 command | Download | Build from source | pip install | pip/Docker | pip install | Download | `docker run` | pip install | IDE install | Download | Docker | Download |
| **Platform** | Mac/Lin/Win | Mac/Lin/Win | Mac/Lin/Win | Mac only | Linux | Mac/Lin/Win | Mac/Lin/Win | Web | Mac/Lin/Win | IDE | macOS/iOS | Mac/Lin/Win | Mac/Lin/Win |
| **GPU support** | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> CUDA+Metal | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> CUDA+Metal | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> CUDA+Metal | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> (M only) | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> (CUDA) | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> (CPU only) | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Metal | Via backend | Via backend | Via backend | Via backend | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | Via backend |
| **Open Source** | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> MIT | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> MIT | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Apache 2.0 | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Apache 2.0 | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> MIT | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> AGPL (core) | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> MIT | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Apache 2.0 | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Apache 2.0 | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> MIT | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> MIT | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> FSL-1.1 |
| **Quantizations** | K-quants | K-quants | K-quants + I-quants | 2–6 bit | AWQ/GPTQ | K-quants | Via backend | N/A | N/A | N/A | N/A | All formats | N/A |
| **Tool calling** | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> |
| **OpenAI API** | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> |
| **Docker** | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/alert-triangle" alt="" width="20" height="20" style="vertical-align:middle"> (Mac only) | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/alert-triangle" alt="" width="20" height="20" style="vertical-align:middle"> Community | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> |
| **Parallel req.** | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> |
| **Multi-model** | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | Manual | Manual | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> |
| **RAG support** | Manual | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | Manual | Manual | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> (LocalDocs) | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> (9 VDBs) | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> (@codebase) | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> (Qdrant) |
| **Speed (M1 7B)** | 22–25 t/s | 22–28 t/s | 20–25 t/s | 28–35 t/s | N/A | 10–15 t/s | Via backend | N/A | N/A | N/A | N/A | Via backend | N/A |

---

## 17. Benchmarks on Apple Silicon

### 17.1 MLX vs llama.cpp vs Ollama (M5 Max 128GB, Llama 3.1 70B Q4)

| Backend | tok/s | Memory | Difference |
|---------|-------|--------|-----------|
| **MLX** | **18** | ~39 GB | <img src="https://lucide.dev/api/icons/medal" alt="" width="20" height="20" style="vertical-align:middle"> |
| llama.cpp Metal | 14 | ~41 GB | –22% |
| Ollama (CPU) | 12 | ~41 GB | –33% |

*(Source: CraftRigs)*

### 17.2 MLX vs llama.cpp (M4 Max 36GB, Llama 3 8B Q4)

| Backend | Prefill (tok/s) | Generation (tok/s) | Memory |
|---------|----------------|-------------------|--------|
| llama.cpp Metal | 1420 | 71.3 | 5.8 GB |
| MLX 4-bit | 1180 | 65.8 | 6.1 GB |

*(Source: Contra Collective)*

### 17.3 MLX vs llama.cpp (M3 Ultra 192GB, Llama 3.1 70B Q4)

| Backend | Prefill (tok/s) | Generation (tok/s) | Memory |
|---------|----------------|-------------------|--------|
| llama.cpp Metal | 380 | 9.4 | 41 GB |
| MLX 4-bit | 470 | 11.1 | 39 GB |

*(Source: Contra Collective)*

### 17.4 Systematic comparison (arXiv, M2 Ultra 192GB, Qwen-2.5)

| Framework | Max tok/s | TTFT | Throughput |
|-----------|----------|------|-----------|
| **MLX** | **~230** | Medium | Maximum |
| MLC-LLM | ~200 | **Low** | Best TTFT |
| llama.cpp | ~150 | Fast | Lightweight |
| Ollama | ~130 | Slow | Simplicity |
| PyTorch MPS | ~7–9 | — | Not for production |

*(Source: arXiv:2511.05502)*

### 17.5 Speculative decoding acceleration

| Tool | Model | Without SD (tok/s) | With SD (tok/s) | Speedup |
|------|-------|-------------------|----------------|---------|
| **mlx-lm** | Llama 3.3 70B | 11.2 | **23.5** | **2.1×** |
| llama.cpp | Llama 3.3 70B + 8B draft | 9.4 | **15.1** | **1.6×** |

### 17.6 Context window impact on speed

Gemma 4 26B MoE on M3 Max 128GB:

| Context | Prefill (tok/s) | Generation (tok/s) |
|---------|----------------|-------------------|
| 1K | 937 | 41.5 |
| 16K | 1015 | 30.8 |
| 64K | 754 | 15.5 |
| **128K** | **534** | **5.6** |

*(Source: PubliVault)*

### 17.7 M4 Pro 24 GB, Qwen3-Coder-30B MoE

| Metric | LM Studio (MLX) | Ollama (llama.cpp) | Difference |
|--------|-----------------|-------------------|------------|
| **Throughput** | **102 tok/s** | 70 tok/s | **+46%** |
| TTFT | 291 ms | **175 ms** | Ollama faster |
| GPU Power | **12.4 W** | 15.4 W | **–20%** |
| Memory | **21.4 GB** | 41.6 GB | **–49%** |

### 17.8 MLX backend in Ollama

Since March 2026, Ollama can use MLX backend on Mac with 32 GB+ RAM:
- Qwen 3.5-35B-A3B: 58 → **112 tok/s** on M5 Max (+93%)
- Currently works with Qwen models, Llama/Mistral coming

---

## 18. Scaling and Production

### 18.1 Concurrent users

| Tool | Max concurrent | Depends on | Mechanism |
|------|---------------|-----------|----------|
| **vLLM** | 100+ | GPU memory | Continuous batching |
| **Ollama** | 1–4 | RAM, `OLLAMA_NUM_PARALLEL` | Sequential (pre-v0.5), parallel (v0.5+) |
| **LM Studio** | 1–2 | RAM | Server mode |
| **llama.cpp** | 1–8 | RAM, batch size | Server mode |
| **MLX** | 1–2 | RAM | No server mode |
| **GPT4All** | 1 | — | Local only |
| **LocalAI** | 4–8 | RAM, backend | Server mode |

### 18.2 Docker support

| Tool | Docker | Official image |
|------|--------|----------------|
| **Ollama** | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | `ollama/ollama` |
| **LM Studio** | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | — |
| **MLX** | <img src="https://lucide.dev/api/icons/alert-triangle" alt="" width="20" height="20" style="vertical-align:middle"> (Mac only) | `mlx-community` |
| **llama.cpp** | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | `ghcr.io/ggml-org/llama.cpp` |
| **vLLM** | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | `vllm/vllm-openai` |
| **GPT4All** | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | — |
| **Jan** | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | — |
| **Open WebUI** | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | `ghcr.io/open-webui/open-webui` |
| **LocalAI** | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | `localai/localai` |
| **Aider** | <img src="https://lucide.dev/api/icons/alert-triangle" alt="" width="20" height="20" style="vertical-align:middle"> | Community |

### 18.3 GPU memory management

| Tool | Unload | Offload | KV cache control | Multi-GPU |
|------|--------|---------|-----------------|-----------|
| **Ollama** | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> On idle | `OLLAMA_GPU_LAYERS` | `OLLAMA_KV_CACHE_TYPE` | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> |
| **MLX** | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> (manual) | N/A (always GPU) | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> |
| **llama.cpp** | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | `-ngl` | `--cache-type-k`, `--cache-type-v` | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> |
| **vLLM** | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | `--gpu-memory-utilization` | `--kv-cache-dtype` | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> |

---

## 19. Decision Tree

```
Want to run LLMs locally?
│
├─ Complete beginner, no terminal
│  └─ LM Studio ─────────────────────────────── GUI, auto-MLX, model browser
│
├─ Need one command and API
│  └─ Ollama ─────────────────────────────────── brew install + ollama run
│     │
│     ├─ Want a web UI → Open WebUI ─────────── Docker, RAG, multi-user
│     ├─ Want IDE assistant → Continue.dev ──── VS Code, tab autocomplete
│     └─ Want CLI agent → Aider ─────────────── architect/editor, repo map
│
├─ Mac, need maximum speed
│  └─ MLX (via LM Studio or mlx-lm) ─────────── +20–40%, –50% RAM
│
├─ Need production server on Linux
│  └─ vLLM ───────────────────────────────────── Continuous batching, multi-GPU
│
├─ Need RAG on documents
│  ├─ Single user → GPT4All
│  └─ Team → Open WebUI
│
├─ Need training / fine-tuning
│  └─ MLX (mlx-lm) ──────────────────────────── LoRA/QLoRA on Apple Silicon
│
├─ Need full control and flexibility
│  └─ llama.cpp (raw) ───────────────────────── GGUF, speculative decoding
│
├─ Need full OpenAI API (TTS, STT, images)
│  └─ LocalAI ───────────────────────────────── 60+ backends, Docker
│
├─ Need a desktop client for local + cloud
│  └─ Jan ───────────────────────────────────── Model Hub, remote support
│
└─ Need one GUI for all providers
   └─ MindWork AI Studio ────────────────────── Local + cloud, plugins, RAG
```

---

## 20. Installation guides

### Ollama

```bash
# macOS
brew install ollama

# Linux
curl -fsSL https://ollama.com/install.sh | sh

# Run a model
ollama run qwen3:8b

# API
curl http://localhost:11434/api/generate -d '{
  "model": "qwen3:8b",
  "prompt": "Hello!",
  "stream": false
}'

# Modelfile
cat > Modelfile << 'EOF'
FROM qwen3:8b
PARAMETER temperature 0.7
PARAMETER num_ctx 8192
SYSTEM "You are a professional Python developer."
EOF
ollama create my-coder -f Modelfile
```

### LM Studio

```bash
# 1. Download .dmg from lmstudio.ai
# 2. Open → Search → find a model
# 3. Download → Load → Chat
# Server mode:
# Settings → Server → Enable → Port 1234
```

### MLX

```bash
# Install
pip install mlx-lm

# Inference
python -m mlx_lm.generate \
  --model mlx-community/Qwen3-8B-4bit \
  --prompt "Hello, how are you?" \
  --max-tokens 256

# Chat
python -m mlx_lm.chat \
  --model mlx-community/Qwen3-8B-4bit

# HTTP server
python -m mlx_lm.server \
  --model mlx-community/Qwen3-8B-4bit

# Fine-tuning
python -m mlx_lm.lora \
  --model mlx-community/Qwen3-8B-4bit \
  --data data.jsonl \
  --num-layers 16 \
  --lora-rank 8
```

### llama.cpp

```bash
# Build
git clone https://github.com/ggml-org/llama.cpp
cd llama.cpp && make -j

# Download GGUF
wget https://huggingface.co/Qwen/Qwen3-8B-GGUF/resolve/main/qwen3-8b-q4_k_m.gguf

# Run
./main -m qwen3-8b-q4_k_m.gguf \
  -p "Hello!" \
  -n 256 \
  -ngl 99  # all layers on GPU

# Server
./server -m qwen3-8b-q4_k_m.gguf \
  --port 8080 \
  -ngl 99

# Embeddings
./embedding -m qwen3-8b-q4_k_m.gguf \
  -p "Some text for embedding"
```

### vLLM

```bash
# Install (Linux only)
pip install vllm

# Server
python -m vllm.entrypoints.openai.api_server \
  --model Qwen/Qwen3-8B \
  --dtype auto \
  --gpu-memory-utilization 0.9

# API
curl http://localhost:8000/v1/chat/completions -d '{
  "model": "Qwen/Qwen3-8B",
  "messages": [{"role": "user", "content": "Hello!"}]
}'
```

### Aider + Ollama

```bash
# Install
python -m pip install aider-chat

# Run with local model
export OLLAMA_API_BASE=http://localhost:11434
aider --model ollama/qwen2.5-coder:7b

# Architect mode (2 models)
aider --model ollama/qwen2.5-coder:7b \
  --editor-model ollama/qwen3:4b

# Modes
aider --chat-mode ask      # questions about code
aider --chat-mode code     # writing code
aider --chat-mode architect # architect + editor
```

### Open WebUI

```bash
# Docker
docker run -d -p 3000:8080 \
  -v open-webui:/app/backend/data \
  --name open-webui \
  --restart always \
  ghcr.io/open-webui/open-webui:main

# Connect to Ollama
# Settings → Connections → Ollama API URL: http://host.docker.internal:11434
```

### GPT4All

```bash
# macOS
brew install --cask gpt4all

# Or download from https://gpt4all.io/

# Python API
pip install gpt4all
```

### Jan

```bash
# Download from https://jan.ai/
# Or via Homebrew:
brew install --cask jan

# Open → Model Hub → Download → Start chatting
```

### LocalAI

```bash
# Docker
docker run -p 8080:8080 \
  -v $PWD/models:/models \
  localai/localai:latest

# LLM
curl http://localhost:8080/v1/chat/completions -d '{
  "model": "gpt-4",
  "messages": [{"role": "user", "content": "Hello!"}]
}'

# TTS
curl http://localhost:8080/v1/audio/speech -d '{
  "model": "tts-1",
  "input": "Hello, world!",
  "voice": "en_US-amy-medium"
}'
```

### Enchanted

```bash
# Download from App Store (macOS and iOS)
# Or build from source:
git clone https://github.com/gluonfield/enchanted.git
# Open in Xcode → Build → Run

# Make sure Ollama is running on your Mac
# Configure the server URL in settings
# Start chatting from any device on your network
```

### MindWork AI Studio

```bash
# Download from https://mindwork.ai/
# Available for macOS, Windows, Linux

# Or build from source:
git clone https://github.com/MindWorkAI/AI-Studio.git
# Follow build instructions in the repository
```

---

## 21. Terminology glossary

| Term | Meaning |
|------|---------|
| **TTFT** (Time To First Token) | Latency before the first token of a response |
| **Continuous batching** | Dynamically adding/removing requests from a batch during processing |
| **PagedAttention** | Efficient KV cache management that eliminates memory fragmentation |
| **Speculative decoding** | Small "draft" model generates tokens → large model verifies them |
| **KV cache** | Key/Value attention cache — the main RAM consumer for long contexts |
| **GGUF** | Model format for llama.cpp with built-in quantization metadata |
| **Modelfile** | Ollama configuration for creating custom models (analogous to Dockerfile) |

---

## 22. What's next

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
