# <img src="https://lucide.dev/api/icons/bar-chart-3" alt="" width="20" height="20" style="vertical-align:middle"> Apple Silicon Benchmarks

> Real speed measurements on Q4_K_M — tokens per second for popular models across all Apple Silicon chips.

**🇷🇺 Russian version:** [apple-silicon.ru.md](apple-silicon.ru.md)

---

[← Local models](../README.md) · [Tools comparison](../tools.md)

---

Sources: MLJourney, CraftRigs, MacYou, LLMCheck.

| Chip | 8B tok/s | 14B tok/s | 27–30B tok/s | 70B tok/s |
|------|----------|-----------|-------------|-----------|
| **M1 8 GB** | 12–14 | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> |
| **M1 16 GB** | 18–24 | 11–12 | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> |
| **M2 16 GB** | 22–28 | 12–14 | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> |
| **M2 Pro 32 GB** | 30–38 | 18–20 | — | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> |
| **M2 Max 64 GB** | 50–65 | 30–35 | — | 10–12 |
| **M3 16 GB** | 22–33 | 12–15 | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> |
| **M3 Pro 36 GB** | 35–39 | 20–22 | 12–15 | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> |
| **M3 Max 128 GB** | 65–75 | 40–45 | 25–30 | 10–14 |
| **M4 16 GB** | 25–35 | 14–17 | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> |
| **M4 Pro 48 GB** | 38–50 | 22–26 | 18–25 | 7–11 |
| **M4 Max 64 GB** | 65–85 | 40–50 | 28–35 | 12–18 |
| **M4 Max 128 GB** | 75–95 | 45–55 | 30–40 | 15–22 |
| **M4 Ultra 192 GB** | 100–140 | 60–80 | 40–60 | 25–35 |

> <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> = does not fit in RAM / constant swap (0.4–2.2 tok/s).
> All numbers are **generation**, not prefill. Prefill is typically 10–30× faster.
> For NVIDIA/Windows/Linux performance will be significantly higher (RTX 4090 does 80–120 tok/s on 8B).

---

## Speed by task (M1 16 GB, Q4_K_M)

| Task | Model | tok/s | Note |
|------|-------|-------|------|
| Autocomplete (FIM) | Qwen 2.5 Coder 1.5B | 30+ | <img src="https://lucide.dev/api/icons/zap" alt="" width="20" height="20" style="vertical-align:middle"> |
| Main coder | Qwen 2.5 Coder 7B | 22–25 | <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> |
| Fast chat | Qwen 3.5 4B | 28–35 | <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> |
| Balanced chat | Qwen 3.5 9B | 10–13 | <img src="assets/icons/level-yellow.svg" alt="" width="12" height="12" style="vertical-align:middle"> |
| Reasoning | DeepSeek R1 Distill 14B | 8–10 | <img src="assets/icons/level-yellow.svg" alt="" width="12" height="12" style="vertical-align:middle"> |

---

## MLX vs llama.cpp on M4 Pro

**M4 Pro 24 GB, Qwen3-Coder-30B MoE** (source: asiai.dev):

| Metric | LM Studio (MLX) | Ollama (llama.cpp) | Difference |
|--------|-----------------|-------------------|------------|
| **Throughput** | **102 tok/s** | 70 tok/s | **+46%** |
| TTFT | 291 ms | **175 ms** | Ollama faster |
| GPU Power | **12.4 W** | 15.4 W | **-20%** |
| Process Memory | **21.4 GB** | 41.6 GB | **-49%** |

## MLX backend in Ollama

Since March 2026, Ollama can automatically switch to MLX backend on Mac with 32 GB+ RAM:
- Qwen 3.5-35B-A3B: 58 → **112 tok/s** on M5 Max (+93%)
- Works with Qwen models. Llama/Mistral support coming.

---

| If you want | Go to |
|-------------|-------|
| **Choose a model** for your Mac | [models.md](../models.md) |
| **Compare tools** for max speed | [tools.md](../tools.md) |
| **How much memory do you need** | [memory-and-context.md](../memory-and-context.md) |
| **Back to navigation** | [README.md](../README.md) |

---

**In section:** [getting-started](../getting-started.md) · [running-models](../running-models.md) · [models](../models.md) · [catalog](../catalog.md) · [quantization](../quantization.md) · [memory-and-context](../memory-and-context.md) · [tools](../tools.md) · [advanced-setup](../advanced-setup.md) · [troubleshooting](../troubleshooting.md) · [apple-silicon](apple-silicon.md)  
**Related sections:** [Zero Level](../../basics/README.md) · [AI Agents](../../agents/README.md) · [Use Cases](../../use-cases/README.md)  
**Navigation:** [← Local Models](../README.md) · [↑ Back to main](../../README.md) · [🇷🇺 Русский](apple-silicon.ru.md)
