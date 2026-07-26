# <img src="https://lucide.dev/api/icons/scale" alt="" width="20" height="20" style="vertical-align:middle"> Quantization

> Detailed guide to model compression methods: GGUF K-quants, GPTQ, AWQ, MLX — and how to choose the right one.

**🇷🇺 Russian version:** [quantization.ru.md](quantization.ru.md)

---

[← Local models](../README.md) · [Benchmarks →](benchmarks/apple-silicon.md)

---

## Contents

1. [Why quantize?](#1-why-quantize)
2. [GGUF and K-quants](#2-gguf-and-k-quants)
3. [Quality comparison](#3-quality-comparison)
4. [Quantization by engine](#4-quantization-by-engine)
5. [How to estimate RAM](#5-how-to-estimate-ram)
6. [Tools for quantization](#6-tools-for-quantization)
7. [Further reading](#7-further-reading)
8. [Whats next](#8-whats-next)

---

## 1. Why quantize?

Without quantization, large language models simply do not fit in most consumer devices:

- GPT-3 175B in FP32 would take ~350 GB of memory
- Even a 7B model in FP32 takes ~14 GB
- After 4-bit quantization — only ~3.5-4.5 GB

---

## 2. GGUF and K-quants

**GGUF (GPT-Generated Unified Format)** is the main format for llama.cpp and related tools (Ollama, LM Studio, etc.).

### K-quants in GGUF

These are special quantization schemes developed for llama.cpp that use different bit widths for different weight types based on importance.

| Type | Avg bits/weight | Description | When to use |
|------|----------------|-------------|-------------|
| Q2_K | 2.0 | Very low quality | Only when extremely memory constrained |
| Q3_K_S | 2.5 | Low quality | Very limited resources |
| Q3_K_M | 3.0 | Low-medium quality | Very tight fit |
| Q4_0 | 4.0 | Satisfactory | Basic 4-bit uniform |
| Q4_K_S | 4.3 | Good quality | Balance of size and quality |
| Q4_K_M | **4.8** | **Excellent quality** | **Recommended default** |
| Q5_0 | 5.0 | Very good | When you need a bit more quality |
| Q5_K_S | 5.3 | Excellent quality | Superior balance |
| Q5_K_M | 5.5 | Excellent + | For demanding tasks |
| Q6_K | 6.0 | High quality | For code, math, precise calculations |
| Q8_0 | 8.0 | Near FP16 | When memory allows and max quality needed |
| F16 | 16.0 | Maximum quality | Experiments, baseline comparison |

---

## 3. Quality comparison

### Example: 7B model

| Method | Size | Perplexity (lower = better) | Relative speed |
|--------|------|---------------------------|----------------|
| F16 (FP16) | ~13.5 GB | Baseline (5.12) | 1.0× |
| Q8_0 | ~10.4 GB | 5.15 (~+0.6%) | ~0.9× |
| Q6_K | ~7.8 GB | 5.20 (~+1.6%) | ~0.7× |
| Q5_K_M | ~6.2 GB | 5.25 (~+2.5%) | ~0.6× |
| Q4_K_M | ~4.9 GB | 5.30 (~+3.5%) | ~0.5× |
| Q4_0 | ~4.5 GB | 5.45 (~+6.4%) | ~0.5× |
| Q3_K_M | ~3.6 GB | 5.80 (~+13.3%) | ~0.4× |
| Q2_K | ~2.3 GB | 6.50 (~+27.0%) | ~0.3× |

### Quick rule

- **Q4_K_M** — best balance of quality and size. Start here.
- **Q5_K_M** — when quality is critical and RAM allows.
- **Q8_0 / F16** — for baselines and experiments only.

---

## 4. Quantization by engine

### Ollama / llama.cpp
- **Format**: GGUF
- **Quantizations**: Q2_K, Q3_K_S/M, Q4_0, Q4_K_S/M, Q5_0, Q5_K_S/M, Q6_K, Q8_0
- **Where to look**: model names with suffixes like `Q4_K_M`, `Q5_K_S`

### mlx-lm (Apple Silicon)
- **Format**: MLX (Apple Silicon specific)
- **Quantizations**: 2-bit, 3-bit, 4-bit, 6-bit (grouped)
- **Advantage**: 20-40% faster than GGUF via Metal on Apple Silicon
- **Disadvantage**: fewer pre-trained models available, needs conversion

### GPTQ
- **For**: NVIDIA GPU via transformer frameworks (vLLM, text-generation-inference)
- **Quantizations**: 3-bit, 4-bit, 8-bit
- **Feature**: group quantization, often preserves quality better at same bit width vs GGUF

### AWQ
- **Analog of GPTQ** with a different weight selection algorithm
- **Works well with**: models from 7B to 72B on NVIDIA GPU

---

## 5. How to estimate RAM

### For GGUF models
1. Check the .gguf file size (e.g., 7.2 GB)
2. Add ~1-2 GB for KV cache (depends on context length)
3. Add 0.5-1 GB for framework overhead
4. Total: file_size + 1.5-3.0 GB should be less than available RAM

Example: 6.8 GB model + 2.0 GB = 8.8 GB fits in 16 GB RAM with headroom.

### For MLX formats
Similar but typically 10-20% less overhead due to more efficient storage on Apple Silicon.

---

## 6. Tools for quantization

### llama.cpp
- `quantize` — converts models to different quantizations
- `main` / `server` — runs models
- Supports all GGUF quantizations

### MLX
- `mlx_lm.convert` — converts HF models to MLX format
- `mlx_lm.generate` — generates text

### HuggingFace Transformers + bitsandbytes
- Load models with 8-bit, 4-bit quantization directly via PyTorch
- Good for experiments and fine-tuning
- Requires compatible GPU

---

## 7. Further reading

- [K-quants from llama.cpp](https://github.com/ggml-org/llama.cpp/blob/master/docs/quantization.md)
- [Quantization overview from HuggingFace](https://huggingface.co/docs/transformers/main_classes/quantization)
- [GPTQ paper](https://arxiv.org/abs/2210.17323)
- [AWQ paper](https://arxiv.org/abs/2306.00978)

---

## 8. Whats next

| If you want | Go to |
|-------------|-------|
| **See benchmarks** of different quantizations on Mac | [benchmarks/apple-silicon.md](benchmarks/apple-silicon.md) |
| **Compare tools** by quantization support | [tools.md](tools.md) |
| **Choose a model** for your task | [models.md](models.md) |
| **Back to navigation** | [README.md](../README.md) |

---

**In section:** [getting-started](getting-started.md) · [running-models](running-models.md) · [models](models.md) · [catalog](catalog.md) · [quantization](quantization.md) · [memory-and-context](memory-and-context.md) · [tools](tools.md) · [advanced-setup](advanced-setup.md) · [troubleshooting](troubleshooting.md) · [apple-silicon](benchmarks/apple-silicon.md)  
**Related sections:** [Zero Level](../basics/README.md) · [AI Agents](../agents/README.md) · [Use Cases](../use-cases/README.md)  
**Navigation:** [← Local Models](README.md) · [↑ Back to main](../README.md) · [🇷🇺 Русский](quantization.ru.md)
