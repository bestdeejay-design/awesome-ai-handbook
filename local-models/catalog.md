# 📚 Open-Weight LLM Catalog

> Complete catalog of open-weight models for local inference — coding, chat, reasoning, vision, and MoE.

**🇷🇺 Russian version:** [catalog.ru.md](catalog.ru.md)

---

[← Local models](../README.md) · [How to choose →](models.md)

---

## Contents

1. [Coding models](#1-coding-models)
2. [General chat/instruction models](#2-general-chatinstruction-models)
3. [Reasoning / math models](#3-reasoning-math-models)
4. [Multimodal / Vision models](#4-multimodal-vision-models)
5. [Small fast models (up to 4B)](#5-small-fast-models-up-to-4b)
6. [MoE (Mixture-of-Experts) models](#6-moe-mixture-of-experts-models)
7. [Long context models](#7-long-context-models)
8. [Hidden gems](#8-hidden-gems)
9. [Whats next](#9-whats-next)

---

## 1. Coding models

### Small (up to 14B) — for 8-16 GB RAM

| Model | Params | Context | FIM | HumanEval | Where to run | Ollama |
|-------|--------|---------|-----|-----------|--------------|--------|
| **Stable Code 3B** | 2.7B | 16K | ✅ | ~55% | 4GB VRAM, CPU | `ollama run stable-code:3b` |
| **CodeGemma 2B** | 2B | 8K | ✅ | ~50% | 2GB VRAM | `ollama run codegemma:2b` |
| **Qwen 2.5 Coder 1.5B** | 1.5B | 32K | ✅ | ~45% | 2GB, CPU | `ollama run qwen2.5-coder:1.5b` |
| **Yi-Coder 1.5B** | 1.5B | 128K | ✅ | ~48% | 2GB, CPU | `ollama run yi-coder:1.5b` |
| **Phi-4-mini (3.8B)** | 3.8B | 128K | ❌ | **74.4%** | 3GB | `ollama run phi4-mini` |
| **Qwen 2.5 Coder 7B** | 7B | 128K | ✅ | **82%** | 6GB | `ollama run qwen2.5-coder:7b` |
| **CodeGemma 7B** | 7B | 8K | ✅ | ~65% | 8GB | `ollama run codegemma:7b` |
| **DeepSeek R1 Distill Qwen 7B** | 7B | 32K | ❌ | ~58% | 6GB | `ollama run deepseek-r1:7b` |
| **Yi-Coder 9B** | 9B | 128K | ✅ | ~70% | 8GB | `ollama run yi-coder:9b` |

**Bottom line:** For coding on 16 GB Mac — **Qwen 2.5 Coder 7B** (FIM, 82% HumanEval). For autocomplete (FIM) — **CodeGemma 2B** or **Qwen 2.5 Coder 1.5B** (lightning fast).

### Medium (14B-30B) — for 24-32 GB RAM

| Model | Params | Context | SWE-bench | LiveCodeBench | When to choose |
|-------|--------|---------|-----------|---------------|----------------|
| **Qwen3-Coder-30B-A3B** (MoE) | 30B / 3.3B active | 256K | ~70% | — | Best MoE coder, 256K ctx |
| **Devstral Small 2 (24B)** | 24B dense | 256K | **68.0%** | — | Agent coding, multi-file edits |
| **Codestral 25.01 (22B)** | 22B dense | 256K | — | — | **Best FIM** (95.3%), 80+ languages |
| **gpt-oss-20b** | 20B MoE | 128K | — | — | ~o3-mini, Apache 2.0 |
| **Phi-4 (14B)** | 14B | 16K | — | — | MMLU 84.8%, coding + math |
| **Phi-4-reasoning (14B)** | 14B | 16K | — | — | Reasoning + code |
| **Qwen 2.5 Coder 14B** | 14B | 128K | — | — | FIM, 128K ctx |

### Large (30B+) — for 48-128 GB RAM or cloud

| Model | Params | SWE-bench | LiveCodeBench | License |
|-------|--------|-----------|---------------|---------|
| **Qwen3.6-27B dense** | 27B | **77.2%** | **83.9%** | Apache 2.0 ⭐ **best consumer coding model 2026** |
| **DeepSeek-V4-Flash** | 284B / 13B active | ~78% | ~80% | MIT |
| **Devstral 2 (123B)** | 123B dense | 72.2% | — | Modified MIT |
| **Kimi K2.6** | 1T / 38B active | **80.2%** | 72.4% | Modified MIT |
| **Kimi K3** | **2.8T / ~50B active** | — | — | Modified MIT |
| **GLM-5.1** | 744B / 40B active | — | 73.9% | MIT |
| **DeepSeek-V4-Pro** | 1.6T / 49B active | **80.6%** | **93.5%** | MIT ⭐ **best open-weight coder** |

---

## 2. General chat/instruction models

### 2.1 Qwen 3.5 (Feb 2026) — all Apache 2.0

| Model | Params | Context | Feature |
|-------|--------|---------|---------|
| Qwen3.5-0.8B | 0.8B | 256K | Edge, CPU inference |
| Qwen3.5-2B | 2B | 256K | Mobile |
| Qwen3.5-4B | 4B | 256K | Laptops |
| **Qwen3.5-9B** | **9B** | **256K** | **Sweet spot for 16GB** |
| Qwen3.5-27B | 27B | 256K | 24GB GPU |
| Qwen3.5-35B-A3B | 35B / 3B active (MoE) | 262K | Efficient MoE |
| **Qwen3.5-397B-A17B** | 397B / 17B active | 262K | Flagship (Hybrid DeltaNet) |

### 2.2 Qwen 3.6 (Apr 2026) — Apache 2.0

| Model | Params | Context | Key |
|-------|--------|---------|-----|
| **Qwen3.6-27B** | 27B dense | 262K | **SWE-bench 77.2%, native multimodal** |

**Important:** Qwen3.6-27B is the best local model of 2026 for coding. In Q4_K_M ~17GB, fits in 24GB.

### 2.3 Llama 4 (Apr 2026) — Llama Community License

| Model | Params | Context | MMLU-Pro | Feature |
|-------|--------|---------|----------|---------|
| **Llama 4 Scout** | 109B / 17B active (MoE) | **10M** | — | **Longest context**, 1× H100 |
| **Llama 4 Maverick** | 400B / 17B active (MoE) | 1M | 80.5% | Multimodal |

### 2.4 Llama 3.x — Llama Community License

| Model | Params | Context | When to choose |
|-------|--------|---------|----------------|
| Llama 3.2 1B | 1B | 128K | Edge / CPU |
| Llama 3.2 3B | 3B | 128K | 8GB RAM, fast |
| Llama 3.1 8B | 8B | 128K | Proven baseline |
| Llama 3.3 70B | 70B | 128K | 32GB+, powerful general |

### 2.5 Google Gemma 4 (Apr 2026) — all Apache 2.0

| Model | Params | Context | MMLU-Pro | AIME | Feature |
|-------|--------|---------|----------|------|---------|
| **Gemma 4 E2B** | ~2.3B eff (5.1B total) | 128K | — | — | Phones, IoT, audio |
| **Gemma 4 12B Unified** | ~12B | 256K | — | — | Text + image + audio |
| **Gemma 4 26B A4B MoE** | 25.2B / 3.8B active | 256K | **82.6%** | **88.3%** | **97% quality of 31B at 4B cost** |
| **Gemma 4 31B Dense** | 30.7B | 256K | **85.2%** | **89.2%** | **Best single-GPU model** |

---

## 3. Reasoning / math models

| Model | Params | Context | Where to run |
|-------|--------|---------|--------------|
| **Phi-4-reasoning** | 14B | 16K | 16GB ✅ |
| **Qwen QwQ-32B** | 32B | 32K | 24GB ✅ |
| **DeepSeek-R1-Distill-Qwen-1.5B** | 1.5B | 32K | 4GB ✅ |
| **DeepSeek-R1-Distill-Qwen-7B** | 7B | 32K | 8GB ✅ |
| **DeepSeek-R1-Distill-Qwen-14B** | 14B | 32K | 16GB ✅ |
| **DeepSeek-R1-Distill-Qwen-32B** | 32B | 32K | 24GB ✅ |
| **DeepSeek-R1** (full) | 671B / 37B active | 128K | **97.3%** MATH | Cloud / cluster |

**How to choose:**
- 8GB RAM → R1-Distill-7B (2-3× better reasoning than vanilla 7B)
- 16GB RAM → R1-Distill-14B or Phi-4-reasoning
- 24-32GB RAM → R1-Distill-32B or QwQ-32B
- **Note:** R1 distills **do not support FIM** (code autocomplete). For coding use Qwen Coder.

---

## 4. Multimodal / Vision models

| Model | Params | VRAM (Q4) | Context | Strengths |
|-------|--------|-----------|---------|-----------|
| **Moondream 2** | 1.9B | ~2 GB | — | Ultra-light, photos |
| **PaliGemma 2** | 3B | ~3 GB | — | Google, photos + docs |
| **LLaVA 1.6 7B** | 7B | ~6 GB | — | Classic, photos/docs |
| **MiniCPM-V 4.5 (8B)** | 8B | ~6 GB | — | **Best OCR for documents** |
| **InternVL 2.5 8B** | 8B | ~8 GB | — | **Best for UI/charts/code screenshots** |
| **Llama 3.2 Vision 11B** | 11B | ~8 GB | 128K | Best all-rounder for 8-16GB |
| **Gemma 4 31B Dense** | 30.7B | ~20 GB | 256K | **Best single-GPU vision** |
| **Llama 3.2 Vision 90B** | 90B | ~64 GB | 128K | Best local VLM |

---

## 5. Small fast models (up to 4B)

For 8GB RAM, CPU inference, edge devices.

| Model | Params | RAM (Q4) | MMLU | HumanEval | tok/s (M1 8GB) |
|-------|--------|----------|------|-----------|-----------------|
| **Qwen3.5-0.8B** | 0.8B | ~0.5 GB | — | — | 60+ |
| **Llama 3.2 3B** | 3B | ~2.0 GB | 63.4% | — | 30-40 |
| **Phi-4-mini** | 3.8B | ~2.5 GB | **67.3%** | **74.4%** | ~28 |
| **Gemma 4 E4B** | ~4.5B eff | ~5.0 GB | **69.4%** | 52% | ~20 |

**Bottom line for 8GB Mac:**
- Fast chat → **Llama 3.2 3B** (2GB, 30-40 tok/s)
- Coding → **Phi-4-mini** (2.5GB, best coding in this class)
- Math/reasoning → **Phi-4-mini** (MMLU 67.3%)
- Multimodal → **Gemma 3 4B** (2.5GB, vision!)

---

## 6. MoE (Mixture-of-Experts) models

| Model | Total / Active | Experts | Context | Quality | Where to run |
|-------|---------------|---------|---------|---------|--------------|
| **Qwen3.5-35B-A3B** | 35B / 3B | — | 262K | ~9B level | 16-24GB ✅ |
| **Qwen3-Coder-30B-A3B** | 30B / 3.3B | — | 256K | coding leader | 24GB ✅ |
| **Gemma 4 26B A4B** | 25.2B / 3.8B | 128 (8+1) | 256K | 97% of 31B | **24GB ✅** |
| **gpt-oss-20b** | 20B / ~5B | — | 128K | ~o3-mini | 16GB ✅ |
| **Mixtral 8×7B** | 47B / 13B | 8 | 32K | solid | 32GB ✅ |
| **Qwen3.5-122B-A10B** | 122B / 10B | — | 262K | frontier | 48GB+ |
| **DeepSeek-V4-Flash** | 284B / 13B | — | 1M | budget frontier | 64GB+ |
| **Llama 4 Scout** | 109B / 17B | — | **10M** | solid | 48GB+ |
| **DeepSeek-V4-Pro** | 1.6T / 49B | 384+1 | 1M | **best coder** | 8×H200 |

**Bottom line:** For 24GB Mac — **Gemma 4 26B A4B** or **Qwen3-Coder-30B-A3B**. For 16GB — **Qwen3.5-35B-A3B**.

---

## 7. Long context models

| Model | Context | Params | Real long-context work |
|-------|---------|--------|------------------------|
| **Llama 4 Scout** | **10M** | 109B / 17B | ✅ RULER strong |
| **Kimi K2.6** | **2M** | ~1.2T / 38B | ✅ Agent swarm |
| **DeepSeek-V4-Pro** | **1M** | 1.6T / 49B | ✅ MIT |
| **GLM-5.2** | **1M** | 753B / 40B | ✅ MIT |
| **Qwen3.6-27B** | 262K | 27B dense | ✅ Apache 2.0, 24GB |

---

## 8. Hidden gems

| Model | Size | Why try |
|-------|------|---------|
| **Stable Code 3B** | 2.7B | CodeLlama 7B quality at 3B size. Best sub-3B coder |
| **Yi-Coder 9B** | 9B | 128K context, beats CodeLlama 34B at 9B |
| **Phi-4-mini (3.8B)** | 3.8B | MMLU 67.3%, HumanEval 74.4% — incredible for its size |

### Russian-specific models

| Model | Size | Why |
|-------|------|-----|
| **Vikhr-7B** | 7B | Best open-source Russian model |
| **Saiga-YandexGPT-8B** | 8B | Fine-tuned on YandexGPT 5 Lite |

---

## 9. Quick selection by RAM

| RAM Mac | What to run | tok/s | Quality |
|---------|-------------|-------|---------|
| **8GB** | Phi-4-mini (3.8B) or Qwen3.5-4B | 25-35 🟢 | Basic-good |
| **16GB** | Qwen 2.5 Coder 7B or Gemma 3 9B | 15-25 🟡 | Good |
| **24GB** | Qwen3.6-27B (Q4) or Gemma 4 26B MoE | 18-28 🟡 | Excellent |
| **32GB** | Llama 3.1 70B (Q3) | 20-40 🟢 | Excellent |
| **48GB** | Llama 3.3 70B (Q4) | 10-20 🟡 | Superior |
| **64GB** | Gemma 4 31B (Q8) | 8-15 🟠 | Near FP16 |
| **128GB** | DeepSeek-V4-Flash | 15-30 🟢 | Maximum |

---

## License reference

| License | Models | Commercial use |
|---------|--------|----------------|
| **Apache 2.0** | Qwen 3/3.5/3.6, Gemma 4, Granite Code | ✅ Unlimited |
| **MIT** | DeepSeek V3/V4/R1, GLM-5, Phi-4 | ✅ Unlimited |
| **Llama Community** | Llama 3.x/4 | ✅ >700M MAU — pay |
| **Modified MIT** | Kimi K2/K3 | ⚠️ Check model card |

---

## 10. Whats next

| If you want | Go to |
|-------------|-------|
| **Choose a model** for your task and hardware | [models.md](models.md) |
| **How much memory** does a model need | [memory-and-context.md](memory-and-context.md) |
| **Benchmarks** on Mac | [benchmarks/apple-silicon.md](benchmarks/apple-silicon.md) |
| **Run a model** if you havent yet | [running-models.md](running-models.md) |
| **Back to navigation** | [README.md](../README.md) |

> **Last updated:** July 23, 2026
> **Sources:** HuggingFace model cards, Ollama library, r/LocalLLaMA, official announcements

---

**In section:** [getting-started](getting-started.md) · [running-models](running-models.md) · [models](models.md) · [catalog](catalog.md) · [quantization](quantization.md) · [memory-and-context](memory-and-context.md) · [tools](tools.md) · [advanced-setup](advanced-setup.md) · [troubleshooting](troubleshooting.md) · [apple-silicon](benchmarks/apple-silicon.md)  
**Related sections:** [Zero Level](../basics/README.md) · [AI Agents](../agents/README.md) · [Use Cases](../use-cases/README.md)  
**Navigation:** [← Local Models](README.md) · [↑ Back to main](../README.md) · [🇷🇺 Русский](catalog.ru.md)
