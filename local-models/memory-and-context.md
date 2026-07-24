# 💾 Memory and Context

> Understanding RAM requirements, KV cache, and context windows — and how to fit larger models in limited memory.

**🇷🇺 Russian version:** [memory-and-context.ru.md](memory-and-context.ru.md)

---

[← Local models](README.md) · [Quantization →](quantization.md)

---

## Contents

1. [How much RAM does a model need](#1-how-much-ram-does-a-model-need)
2. [KV cache](#2-kv-cache)
3. [Optimization tricks](#3-optimization-tricks)
4. [Context window](#4-context-window)
5. [Whats next](#5-whats-next)

---

## 1. How much RAM does a model need

```
Total RAM = model_size_in_RAM + KV_cache + framework_overhead
```

### Model size by quantization

| Model | FP16 | Q8_0 | Q5_K_M | Q4_K_M | Q3_K_M | Q2_K |
|-------|------|------|--------|--------|--------|------|
| 7B | 13.5 GB | 10.4 GB | 6.2 GB | 4.9 GB | 3.6 GB | 2.3 GB |
| 13B | 26 GB | 20 GB | 12 GB | 9.5 GB | 7 GB | 4.5 GB |
| 30B | 60 GB | 46 GB | 28 GB | 18 GB | 14 GB | 9 GB |
| 70B | 140 GB | 108 GB | 65 GB | 42 GB | 32 GB | 21 GB |

### KV cache by context length

| Context | 7B | 13B | 30B | 70B |
|---------|-----|------|------|------|
| 2K | 0.25 GB | 0.5 GB | 1 GB | 2.5 GB |
| 8K | 1 GB | 2 GB | 4 GB | 10 GB |
| 32K | 4 GB | 8 GB | 16 GB | 40 GB |
| 128K | 16 GB | 32 GB | 64 GB | 160 GB |

### Quick estimation

1. Check model file size (e.g. 4.9 GB for Q4_K_M 7B)
2. Add 1-2 GB for KV cache (depends on context)
3. Add 0.5-1 GB for overhead
4. Total should be less than available RAM

Example: 4.9 GB + 2 GB + 0.5 GB = 7.4 GB ← fits in 8 GB with headroom

---

## 2. KV cache

KV cache stores attention keys and values for each token. It grows linearly with context length.

### Reducing KV cache memory

```bash
# Use Q4 quantization for KV cache (4x savings)
export OLLAMA_KV_CACHE_TYPE=q4_0

# Enable flash attention (memory + speed)
export OLLAMA_FLASH_ATTENTION=1
```

---

## 3. Optimization tricks

| Trick | Savings | How |
|-------|---------|-----|
| Q4 quantization | 4x model size | `ollama pull model:q4_k_m` |
| KV cache Q4 | 4x KV memory | `OLLAMA_KV_CACHE_TYPE=q4_0` |
| Flash attention | ~20% less RAM | `OLLAMA_FLASH_ATTENTION=1` |
| Shorter context | Proportional | Lower `num_ctx` |
| Context compression | 2-5x | Summarize old messages |
| Close other apps | Variable | Free up RAM |

---

## 4. Context window

```bash
# Run with extended context
OLLAMA_CONTEXT_LENGTH=32768 ollama run qwen3.5:4b

# Or set permanently
export OLLAMA_CONTEXT_LENGTH=32768
ollama serve
```

| Context length | Use case |
|----------------|----------|
| 2K-4K | Short chat, Q&A |
| 8K-16K | Normal conversation, code reviews |
| 32K-64K | Large files, multi-file projects |
| 128K+ | Full codebases, long documents, RAG |

### Context and speed

Longer context = slower generation. Each token has to attend to all previous tokens.
- 4K context: ~20 tok/s
- 32K context: ~12 tok/s
- 128K context: ~5 tok/s

---

## 5. Whats next

| Go to | Description |
|-------|-------------|
| [quantization.md](quantization.md) | Compress models to fit RAM |
| [benchmarks/apple-silicon.md](benchmarks/apple-silicon.md) | Speed on Mac |
| [tools.md](tools.md) | Compare inference engines |
| Back | [README.md](README.md) |
