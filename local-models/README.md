# 🖥 Local Models

Running LLMs on your own hardware — from first installation to advanced tuning.

[← Awesome AI Handbook](../README.md) · [Русская версия](README.ru.md)

---

## 🤔 Where to start — choose your scenario

### 🟢 Absolute beginner
> Never ran a model, not familiar with the terminal, just want to try.

**Start with basics:** [basics/README.md](../basics/README.md) — what AI is, how models work, what hardware you need.

**Then install:** [📖 Environment setup and installation](getting-started.md)
→ Homebrew, Ollama, first model in 10 minutes

Or skip the terminal entirely: **LM Studio** (described in [running-models.md](running-models.md))

### 🟡 I want to understand
> Already installed Ollama, ran a model, but want to know how it works.

**Route:**
1. [▶️ How to find and run a model](running-models.md) — hands-on guide
2. [🎯 Choosing a model for your task](models.md) — what to pick for coding / chat / RAG
3. [💾 Memory and context](memory-and-context.md) — why models dont fit and how to fix it

### 🔵 Advanced user
> Want maximum performance, API tuning, tool comparison.

**Route:**
1. [⚙️ Advanced Ollama setup](advanced-setup.md) — Modelfile, API, env vars
2. [🛠 Tool comparison](tools.md) — Ollama vs LM Studio vs MLX vs llama.cpp
3. [⚖️ Quantization](quantization.md) — Q4, Q5, Q8 — what to choose
4. [📊 Apple Silicon benchmarks](benchmarks/apple-silicon.md) — tok/s on M1–M4

### 📚 Reference (for everyone)
| Section | About |
|---------|-------|
| [📚 Model catalog](catalog.md) | 50+ models with specs |
| [⚠️ Common problems](troubleshooting.md) | Diagnostics and fixes |

---

## 🗺 All sections

| # | File | Audience | Reading time |
|---|------|----------|-------------|
| 1 | [🚀 getting-started.md](getting-started.md) | Beginners | 10 min |
| 2 | [▶️ running-models.md](running-models.md) | Everyone | 15 min |
| 3 | [🎯 models.md](models.md) | Everyone | 10 min |
| 4 | [💾 memory-and-context.md](memory-and-context.md) | Everyone | 10 min |
| 5 | [⚙️ advanced-setup.md](advanced-setup.md) | Advanced | 15 min |
| 6 | [🛠 tools.md](tools.md) | Everyone | 20 min |
| 7 | [⚖️ quantization.md](quantization.md) | Advanced | 10 min |
| 8 | [📊 benchmarks/apple-silicon.md](benchmarks/apple-silicon.md) | Everyone | 5 min |
| 9 | [⚠️ troubleshooting.md](troubleshooting.md) | When needed | 5 min |
| 10 | [📚 catalog.md](catalog.md) | Reference | — |

---

## 📋 Section development plan

- [x] Basic file structure
- [x] Cross-linking: "whats next" links in each file
- [ ] **Detail:** step-by-step instructions in getting-started.md
- [ ] **Detail:** API and Modelfile examples in advanced-setup.md
- [ ] **Detail:** verify and fix all cross-reference links
- [x] **English version:** all section files translated
