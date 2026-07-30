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

## What affects model choice

When choosing a model, consider:

1. **Task** — coding, chat, text analysis, image processing
2. **Available VRAM/RAM** — how much memory you have
3. **Quality needed** — maximum quality or "good enough"
4. **Speed requirements** — real-time response or batch processing

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

For reasoning and math: **Phi-4 Mini** (3.8B) — surprisingly strong for its size; **Phi-4** (14B) — if you have resources, excellent at logic; **Qwen 2.5 Math** series — specifically tuned for mathematical tasks; **DeepSeek Math** 7B/7B-Instruct — strong math model.

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

## MacBook Air M1 16GB reference

Models that work well on 16GB unified memory (M1/M2 chips):

| Task | Model | Size | Quant | Speed (tok/s) | HuggingFace / Ollama |
|------|-------|------|-------|---------------|----------------------|
| **Everyday chat** | Qwen 3.5 9B | 6.6 GB | Q4_K_M | 10-13 | [HF](https://huggingface.co/Qwen/Qwen3.5-9B) / [Ollama](https://ollama.com/library/qwen3.5) |
| **Max speed** | Qwen 3.5 4B | 3.4 GB | Q4_K_M | 28-35 | [HF](https://huggingface.co/Qwen/Qwen3.5-4B) / [Ollama](https://ollama.com/library/qwen3.5) |
| **Coding** | Qwen 2.5 Coder 7B | 4.7 GB | Q4_K_M | 22-25 | [HF](https://huggingface.co/Qwen/Qwen2.5-Coder-7B) / [Ollama](https://ollama.com/library/qwen2.5-coder:7b) |
| **Writing/content** | Llama 3.3 8B | 4.9 GB | Q4_K_M | 14-18 | [HF](https://huggingface.co/meta-llama/Llama-3.3-8B-Instruct) / [Ollama](https://ollama.com/library/llama3.3:8b) |
| **Math/reasoning** | Phi-4 Mini | 2.3 GB | Q4_K_M | 25-30 | [HF](https://huggingface.co/microsoft/Phi-4-mini-instruct) / [Ollama](https://ollama.com/library/phi4-mini) |
| **Vision** | Gemma 4 E4B | 2.5 GB | Q4_K_M | 30-38 | [HF](https://huggingface.co/google/gemma-3-4b-it) / [Ollama](https://ollama.com/library/gemma3:4b) |
| **Tool calling / agents** | LFM2.5 8B-A1B | 5.5 GB | Q4_K_M | 35-45 | [HF](https://huggingface.co/LiquidAI/LFM2-8B-A1B) |
| **Long context** | Qwen 3.5 9B | 6.6 GB | Q4_K_M | 10-13 | [HF](https://huggingface.co/Qwen/Qwen3.5-9B) / [Ollama](https://ollama.com/library/qwen3.5) |

## Estimating RAM usage

### Formula
```
Model memory ≈ (Parameters in billions) × (Bytes per parameter) + KV cache + overhead
```

### Example calculations

| Available RAM | Example model | Calculation |
|--------------|---------------|-------------|
| **4-6 GB** (swap possible) | Qwen 2.5 1.5B Q4_K_M (~1.1 GB) | 1.5B × 0.55 ≈ 0.8 GB + KV (~0.3GB) ≈ 1.1-1.4 GB |
| **8 GB** | Qwen 2.5 3B Q4_K_M (~2.0 GB) | 3B × 0.55 ≈ 1.65 GB + KV (~0.3GB) ≈ 2.0 GB |
| **16 GB** | Qwen 3.5 9B Q4_K_M (~7.0 GB) | 9B × 0.55 ≈ 4.95 GB + KV (~1.0GB) + overhead ≈ 7.0 GB |
| **24 GB** | Llama 3 14B Q4_K_M (~8.7 GB) | 14B × 0.55 ≈ 7.7 GB + KV (~1.5GB) ≈ 9.2 GB |
| **32 GB** | Mixtral 8x7B Q4_K_M (~24 GB MoE) | ~6B active params × 0.55 ≈ 3.3 GB + KV (~2GB) ≈ 8-10 GB effective |
| **48+ GB** | Llama 3 70B Q3_K_M (~39 GB) | 70B × 0.41 ≈ 28.7 GB + KV (~8GB) ≈ 36-40 GB |

### RAM recommendations

| RAM | Best choice | Alternative |
|-----|-------------|-------------|
| **4-6 GB** | TinyLlama 1.1B, Phi-2 2.7B, Qwen 1.5-1.8B | Chat and simple tasks |
| **8 GB** | Qwen 2.5 3B, Phi-3 Mini 3.8B, Mistral 7B | Speed and quality balance |
| **16 GB** | Qwen 3.5 9B, Llama 3.1 8B, Qwen 2.5 Coder 7B | Universal choice |
| **24-32 GB** | Llama 3 14B, Mixtral 8x7B, Command R | Complex tasks and agents |
| **48+ GB** | Llama 3 70B, Mixtral 8x22B | Maximum quality, complex reasoning |

## Specialized models

### Coding
For coding the best choices are: **Qwen 2.5 Coder 7B** — best balance of quality and speed; **DeepSeek Coder V2 Lite** (16B MoE, ~2.4B active) — if you have more resources; **Stable Code 3B** — very fast for autocomplete; **CodeLlama 7B/13B** — good alternative with large ecosystem.

### Vision / Image understanding
- **Gemma 4** (4B) — great balance of size and quality
- **Llama 3.2 Vision** (11B/90B) — maximum quality
- **Pixtral** (12B) — from Mistral, strong multimodal capabilities
- **InternVL2** (2B/4B/8B) — excellent quality for size

### Long context
- **Qwen 3.5** series (8B/14B/32B) — up to 32K tokens
- **Yi** series (6B/9B/34B) — up to 200K tokens
- **GLM-4** (9B) — up to 128K tokens
- **Phi-3-mini-128k** — special version with 128K context

## How to read model names

### Example: `Qwen 2.5 Coder 7B Instruct Q4_K_M`
- **Qwen 2.5** — model family and version
- **Coder** — specialization (can be omitted for general models)
- **7B** — parameter count (7 billion)
- **Instruct** — instruction-tuned version (vs base)
- **Q4_K_M** — quantization type (see quantization guide)

### Main quantization types (GGUF)

| Type | Bits/weight | Quality | Size | When to use |
|------|-------------|---------|------|-------------|
| Q2_K | ~2 | Very low | Very small | Only with critical memory shortage |
| Q3_K_S/M | ~3-3.5 | Low-medium | Small | Limited resources |
| Q4_0 | 4.0 | Satisfactory | Medium | When predictability matters |
| Q4_K_S/M | ~4.3-4.8 | Good-excellent | Medium | Best balance of quality and size |
| Q5_0 | 5.0 | Very good | Above medium | When you have some extra memory |
| Q5_K_S/M | ~5.3-5.5 | Excellent | Above medium | High quality at moderate size |
| Q6_K | ~6.0 | High | Large | For code and critical tasks |
| Q8_0 | 8.0 | Almost FP16 | Large | When memory allows, max quality |
| F16 | 16.0 | Maximum | Very large | Experiments and baseline comparison |

> **Tip**: Start with **Q4_K_M** for most tasks — best balance of quality, size and speed. Need more quality? Go Q5_K_M or Q6_K. Need speed or very low memory? Try Q3_K_M or Q2_K.

## Hardware-specific recommendations

### Apple Silicon (M1/M2/M3/M4)
- Best in GGUF format
- Via Ollama or LM Studio (auto-uses Metal)
- For max speed — mlx-lm (MLX format)
- Avoid large context windows (>16K) on M1/M2 due to limited memory bandwidth

### NVIDIA GPU
- Best via llama.cpp with CUDA or inference frameworks
- GGUF and GPTQ formats work well
- Watch VRAM (not system RAM)
- 8GB VRAM: models up to 7B Q4/Q5
- 12GB VRAM: up to 13B Q4/Q5
- 16GB VRAM: up to 20-24B Q4
- 24GB+ VRAM: try 30-70B with appropriate quantization

### AMD GPU
- Support via ROCm (limited)
- Use llama.cpp or frameworks with CPU fallback
- Similar memory recommendations as NVIDIA

### CPU only
- Stick to models up to 3-4B for acceptable speed
- Use aggressive quantization (Q2_K/Q3_K)
- Expect 1-5 tok/s on modern CPUs
- Best for experiments and light tasks

## Where to get models

### Ollama library
Simplest way to start:
```bash
ollama pull qwen3:8b
ollama run qwen3:8b
```
Browse models at [ollama.com/library](https://ollama.com/library)

### Hugging Face
For advanced users:
- Look for `.gguf` suffix in model names (ready for llama.cpp)
- Or download original models and convert via `llama.cpp/convert_hf_to_gguf.py`
- Popular orgs: `Qwen`, `meta-llama`, `microsoft`, `google`, `NousResearch`, `TheBloke`, `lmstudio-community`

### Top HF collections
- **TheBloke** — almost all popular models in various GGUF quantizations
- **lmstudio-community** — optimized for LM Studio
- **unsloth** — optimized for fast fine-tuning

## Stay updated

Models evolve fast. Recommended:
1. Follow releases from major labs (Meta, Mistral, Qwen, Microsoft, Google)
2. Check favorite HF repos every 2-4 weeks
3. Join r/LocalLLaMA and relevant Discord communities

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
