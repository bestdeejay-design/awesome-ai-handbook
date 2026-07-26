# <img src="https://lucide.dev/api/icons/target" alt="" width="20" height="20" style="vertical-align:middle"> Models for Different Tasks

> Recommendations for choosing models based on your tasks and hardware.

**🇷🇺 Russian version:** [models.ru.md](models.ru.md)

---

[← Local models](README.md) · [Catalog →](catalog.md)

---

## Contents

1. [Quick picks by task](#1-quick-picks-by-task)
2. [By hardware](#2-by-hardware)
3. [Key considerations](#3-key-considerations)
4. [Coding models](#4-coding-models)
5. [Chat models](#5-chat-models)
6. [Reasoning models](#6-reasoning-models)
7. [Small models for 8 GB](#7-small-models-for-8-gb)
8. [What's next](#8-whats-next)

---

## 1. Quick picks by task

| Task | Recommended model | RAM needed | Ollama command |
|------|------------------|------------|----------------|
| Chat, general Q&A | Qwen 3.5 9B | 8-16 GB | `ollama run qwen3.5:9b` |
| Coding | Qwen 2.5 Coder 7B | 8-16 GB | `ollama run qwen2.5-coder:7b` |
| Code autocomplete (FIM) | CodeGemma 2B | 4-8 GB | `ollama run codegemma:2b` |
| Reasoning | DeepSeek R1 Distill 7B | 8-16 GB | `ollama run deepseek-r1:7b` |
| Math | Phi-4-mini (3.8B) | 4-8 GB | `ollama run phi4-mini` |
| Vision | Llama 3.2 Vision 11B | 8-16 GB | `ollama run llama3.2-vision:11b` |
| Russian language | Qwen 3.5 9B | 8-16 GB | `ollama run qwen3.5:9b` |
| RAG embeddings | nomic-embed-text | 4 GB | `ollama run nomic-embed-text` |

---

## 2. By hardware

| Your RAM | Best model to run | Approx. speed |
|----------|-------------------|---------------|
| 8 GB | Phi-4-mini (3.8B), Qwen 3.5 4B | 25-35 tok/s |
| 16 GB <img src="https://lucide.dev/api/icons/star" alt="" width="20" height="20" style="vertical-align:middle"> | Qwen 2.5 Coder 7B, Qwen 3.5 9B | 15-25 tok/s |
| 24 GB | Qwen3.6-27B (Q4), Gemma 4 26B MoE | 18-28 tok/s |
| 32 GB | Llama 3.1 70B (Q3) | 20-40 tok/s |
| 48 GB | Llama 3.3 70B (Q4) | 10-20 tok/s |
| 64 GB | Gemma 4 31B (Q8) | 8-15 tok/s |
| 128 GB | DeepSeek-V4-Flash, Qwen3.5-397B | 15-30 tok/s |

---

## 3. Key considerations

- **FIM** (Fill-in-the-Middle) — needed for code autocomplete in IDEs. Qwen Coder has it.
- **Tool calling** — needed for agents. Qwen 3.5+, Llama 3.1+
- **Long context** — 128K+ for working with large codebases. Qwen 3.5 supports 256K
- **Multimodal** — vision models for image understanding (Llama 3.2 Vision, Gemma 4)

---

## 4. Coding models

| Model | Size | Context | FIM | HumanEval | Best for |
|-------|------|---------|-----|-----------|----------|
| **Qwen 2.5 Coder 7B** | 4.7 GB | 128K | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | 82% | General coding, multi-file |
| **Qwen 2.5 Coder 1.5B** | 1.1 GB | 32K | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | 45% | Fast autocomplete, 8GB Mac |
| **CodeGemma 2B** | 1.5 GB | 8K | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | 50% | Autocomplete, low RAM |
| **Phi-4-mini (3.8B)** | 2.5 GB | 128K | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | 74% | Math + coding, small RAM |
| **Yi-Coder 9B** | 6 GB | 128K | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | 70% | Long context coding |

---

## 5. Chat models

| Model | Size | Context | Strengths |
|-------|------|---------|-----------|
| **Qwen 3.5 4B** | 3.4 GB | 256K | Fast, multilingual, sweet spot for 8GB |
| **Qwen 3.5 9B** | 6 GB | 256K | Best quality for 16GB Mac |
| **Llama 3.3 8B** | 4.9 GB | 128K | Strong English, reasoning |
| **Gemma 3 9B** | 6 GB | 128K | Apple Silicon optimized |
| **Phi-4-mini (3.8B)** | 2.5 GB | 128K | Amazing for its size, math champ |

---

## 6. Reasoning models

| Model | Size | Best for |
|-------|------|----------|
| DeepSeek R1 Distill 7B | 4.5 GB | 2-3x better reasoning than vanilla 7B |
| DeepSeek R1 Distill 14B | 9 GB | Complex analysis, fits 16GB |
| Phi-4-reasoning (14B) | 9 GB | Code + reasoning |
| QwQ-32B | 20 GB | Math, logic (needs 24GB) |

> Note: Reasoning models are slower (they "think" before answering) but give better results on complex tasks. Not suitable for real-time chat.

---

## 7. Small models for 8 GB

For Macs with 8 GB RAM or CPU-only inference:

| Model | RAM (Q4) | Speed (tok/s) | Best for |
|-------|----------|---------------|----------|
| Phi-4-mini (3.8B) | 2.5 GB | 28 | Coding, math |
| Qwen 3.5 4B | 2.8 GB | 22-26 | Universal chat |
| Llama 3.2 3B | 2.0 GB | 30-40 | Fast chat |
| CodeGemma 2B | 1.5 GB | 35+ | Code autocomplete |
| Qwen 2.5 Coder 1.5B | 1.1 GB | 40+ | Fast coding |

---

## 8. What's next

| Go to | Description |
|-------|-------------|
| [catalog.md](catalog.md) | Full 50+ model catalog with specs |
| [benchmarks/apple-silicon.md](benchmarks/apple-silicon.md) | Speed tests on Mac |
| [memory-and-context.md](memory-and-context.md) | RAM requirements explained |
| Back | [README.md](README.md) |

---

**In section:** [getting-started](getting-started.md) · [running-models](running-models.md) · [models](models.md) · [catalog](catalog.md) · [quantization](quantization.md) · [memory-and-context](memory-and-context.md) · [tools](tools.md) · [advanced-setup](advanced-setup.md) · [troubleshooting](troubleshooting.md) · [apple-silicon](benchmarks/apple-silicon.md)  
**Related sections:** [Zero Level](../basics/README.md) · [AI Agents](../agents/README.md) · [Use Cases](../use-cases/README.md)  
**Navigation:** [← Local Models](README.md) · [↑ Back to main](../README.md) · [🇷🇺 Русский](models.ru.md)
