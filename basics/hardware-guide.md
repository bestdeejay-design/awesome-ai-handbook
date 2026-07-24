# 💻 What Hardware You Need for Local AI

> Busting myths: from a 5-year-old laptop to a dedicated server.

[← Cloud or Local](cloud-vs-local.md) · [Windows Install →](setup-windows.md)

---

## Contents

1. [The Key Thing to Understand](#1-the-key-thing-to-understand)
2. [Three Hardware Tiers](#2-three-hardware-tiers)
3. [Mac: Models and Chips](#3-mac-models-and-chips)
4. [Windows and Linux: CPU vs GPU](#4-windows-and-linux-cpu-vs-gpu)
5. [Quantization: How to Run Models on Weak PCs](#5-quantization-how-to-run-models-on-weak-pcs)
6. [Will My Computer Handle It? — Checklist](#6-will-my-computer-handle-it--checklist)
7. [What's Next](#7-whats-next)

---

## 1. The Key Thing to Understand

**For local AI, memory matters, not the processor.**

- The model occupies space in RAM (system memory) or VRAM (GPU memory)
- Response speed depends on CPU/GPU
- Whether you can run the model at all — depends on memory size

> **Analogy:** imagine the model is a huge book. Memory is your desk size. If the book doesn't fit on the desk — you can't open it. Bigger desk = thicker book you can read. Reading speed depends on how fast you turn pages (processor).

**How much memory different models need:**

| Model | Size (Q4_K_M) | Minimum RAM |
|-------|---------------|-------------|
| Qwen 3.5 4B | ~3.5 GB | 8 GB |
| Qwen 2.5 Coder 7B | ~4.5 GB | 8-16 GB |
| **Llama 3.1 8B** | **~5.5 GB** | **16 GB ⭐** |
| Qwen 3.5 14B | ~9 GB | 16-32 GB |
| Qwen 3 Coder 30B | ~18 GB | 32-48 GB |
| Llama 3 70B | ~40 GB | 64 GB+ |

> **Q4_K_M** — model format compressed to ~4 bits per parameter. Details in [`../local-models/quantization.md`](../local-models/quantization.md).

---

## 2. Three Hardware Tiers

### 🟢 Entry: 8 GB RAM, any processor

**What you can run:** models up to 3B params (Qwen 2.5 1.5B, Phi-3-mini, Gemma 2B).

**Reality:** slow (~5-15 tokens/sec on CPU), but works. Good for simple chat, basic code autocomplete.

```
┌─────────────────────────────────────┐
│ 8 GB RAM / CPU-only                 │
│                                     │
│  Qwen 2.5 1.5B  →  🟢 fast          │
│  Phi-3-mini      →  🟢 fast          │
│  Qwen 2.5 7B     →  🔴 doesn't fit  │
│  Llama 3.1 8B    →  🔴 doesn't fit  │
└─────────────────────────────────────┘
```

### 🟡 Comfortable: 16 GB RAM, Apple Silicon or entry GPU

**What you can run:** models up to 7-9B (Qwen 3.5 7B, Llama 3.1 8B, Qwen 2.5 Coder 7B).

**This is the sweet spot.** On MacBook Air M1/M2/M3 16 GB — **full working workflow**: chat, AI coding, simple agents. On Windows with RTX 3060/4060 — even faster.

```
┌─────────────────────────────────────┐
│ 16 GB RAM / M1-M4 / RTX 3060        │
│                                     │
│  Qwen 3.5 4B       →  🟢 28-35 t/s │
│  Qwen 3.5 7B       →  🟢 18-25 t/s │
│  Llama 3.1 8B      →  🟢 15-20 t/s │
│  Qwen 2.5 Coder 7B →  🟢 22-25 t/s │
│  DeepSeek-R1 7B    →  🟡 10-15 t/s │
└─────────────────────────────────────┘
```

### 🔵 Powerful: 32 GB+ RAM, RTX 3090/4090 or Mac with 48 GB+

**What you can run:** models 14-30B (Qwen 3 Coder 30B, DeepSeek-R1 14B, Qwen 3.5 14B).

**Reality:** GPT-4 replacement for most tasks. Coding, refactoring, agents — all works great.

```
┌─────────────────────────────────────┐
│ 32-64 GB / RTX 3090/4090 / M3 Max   │
│                                     │
│  Qwen 3.5 14B      →  🟢 18-25 t/s │
│  Qwen 3 Coder 30B  →  🟢 15-20 t/s │
│  DeepSeek-R1 14B   →  🟡 12-18 t/s │
│  DeepSeek-R1 32B   →  🟡 8-12 t/s  │
└─────────────────────────────────────┘
```

---

## 3. Mac: Models and Chips

**Apple Silicon (M1, M2, M3, M4) — the best platform for local AI** today. Why:

- **Unified Memory** — GPU and CPU share the same memory. Entire RAM available to model. On Mac with 64 GB you can run 70B models, which on PC would need RTX 4090 at $2000
- **MLX** — Apple's framework for efficient model inference (sometimes 2x faster than Ollama)
- **Energy efficiency** — MacBook can run model on battery, PC with GPU draws 300-450W

### Speed in tokens/sec (Q4_K_M)

| Chip | 7-8B | 14B | 30B | 70B |
|------|------|-----|-----|-----|
| **M1 8 GB** | 12-14 | ❌ | ❌ | ❌ |
| **M1 16 GB** | 18-24 | 11-12 | ❌ | ❌ |
| **M2 16 GB** | 22-28 | 12-14 | ❌ | ❌ |
| **M3 16 GB** | 22-33 | 12-15 | ❌ | ❌ |
| **M4 16 GB** | 25-35 | 14-17 | ❌ | ❌ |
| **M3 Max 128 GB** | 65-75 | 40-45 | 25-30 | 10-14 |
| **M4 Max 128 GB** | 75-95 | 45-55 | 30-40 | 15-22 |

Detailed benchmarks: [`../local-models/benchmarks/apple-silicon.md`](../local-models/benchmarks/apple-silicon.md)

> **Intel Mac:** not supported by LM Studio, slow in Ollama. If you have Intel Mac — consider upgrade or use cloud models.

---

## 4. Windows and Linux: CPU vs GPU

### Without Discrete GPU (CPU only)

CPU inference works, but slower. For models up to 7B — quite usable:

| CPU | Model | t/s |
|-----|-------|-----|
| Intel i5 / Ryzen 5 | 1-3B | 10-20 |
| Intel i7 / Ryzen 7 | 7B | 5-10 |
| Intel i9 / Ryzen 9 | 7B | 8-15 |

**Tip:** use Q4 quantization and small models (up to 3B) on CPU.

### With GPU

| GPU | VRAM | Models | t/s on 7B |
|-----|------|--------|-----------|
| RTX 3060 | 12 GB | up to 14B Q4 | 30-40 |
| RTX 4060 | 8 GB | up to 7B Q4 | 30-40 |
| RTX 3090 | 24 GB | up to 30B Q4 | 50-70 |
| RTX 4090 | 24 GB | up to 30B Q8 | 80-120 |
| 2× RTX 3090 | 48 GB | up to 70B Q4 | 20+ |

---

## 5. Quantization: How to Run Models on Weak PCs

**Quantization** is model compression. Imagine taking a high-res photo and saving as JPEG. Quality drops slightly, but size shrinks 2-4x.

In model world:
- **FP16** (original) — 100% quality, 2 bytes per parameter
- **Q8_0** — 99% quality, 1 byte per parameter
- **Q4_K_M** — 95-97% quality, 0.5 bytes per parameter ⭐ recommended
- **Q3_K** — 90% quality, 0.375 bytes per parameter
- **Q2_K** — 85% quality, 0.25 bytes per parameter (not recommended for code)

**What this means for you:** 7B model in Q4_K_M takes ~4.5 GB instead of ~14 GB. And it's almost as smart.

Details: [`../local-models/quantization.md`](../local-models/quantization.md)

---

## 6. Will My Computer Handle It? — Checklist

### I have Mac on Apple Silicon
✅ Excellent. Install Ollama → [`../local-models/getting-started.md`](../local-models/getting-started.md)

### I have 16+ GB RAM
✅ Excellent, sweet spot. Work with 7-9B models.

### I have 8 GB RAM
🟡 Possible, but must pick small models (1-3B). Start with Phi-3-mini or Qwen 2.5 1.5B.

### I have GPU with 8+ GB VRAM
✅ Excellent. Use Ollama with CUDA — will be fast.

### I have old laptop (5+ years, Intel)
🟡 Possible, but only 1-3B models and slow. Recommend temporarily using cloud services or buying used MacBook Air M1 ($500-700).

### I don't know what I have
```bash
# Mac: open terminal and run
system_profiler SPHardwareDataType | grep -E "Chip|Memory"

# Windows: open PowerShell
Get-ComputerInfo | Select-Object CsProcessors, CsTotalPhysicalMemory

# Linux: open terminal
lscpu | grep "Model name"
free -h | grep "Mem"
```

---

## 7. What's Next

| If You Want To | Go To |
|----------------|-------|
| **Install Ollama on Mac** | [`../local-models/getting-started.md`](../local-models/getting-started.md) |
| **Install on Windows** | [setup-windows.md](setup-windows.md) |
| **Install on Linux** | [setup-linux.md](setup-linux.md) |
| **Understand quantization** | [`../local-models/quantization.md`](../local-models/quantization.md) |
| **Check Mac benchmarks** | [`../local-models/benchmarks/apple-silicon.md`](../local-models/benchmarks/apple-silicon.md) |
| **Back to navigation** | [README.md](README.md) |