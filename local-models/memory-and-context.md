# 💾 Memory and Context

> Understanding RAM requirements, KV cache, and context windows.

**🇷🇺 Russian version:** [memory-and-context.ru.md](memory-and-context.ru.md)

---

[← Local models](../README.md) · [Quantization →](quantization.md)

---

## Memory formula

```
Total RAM needed = model_size_in_RAM + KV_cache + overhead
```

| Model size | Q4_K_M size | KV cache (4K ctx) | KV cache (32K ctx) |
|------------|-------------|-------------------|--------------------|
| 7B | 4.9 GB | 0.5 GB | 4 GB |
| 13B | 9.5 GB | 1 GB | 8 GB |
| 30B | 18 GB | 2 GB | 16 GB |
| 70B | 42 GB | 5 GB | 40 GB |

## Context window

```bash
# Run with extended context
OLLAMA_CONTEXT_LENGTH=32768 ollama run qwen3.5:4b
```

- 4K context = minimal, enough for short chats
- 8K = comfortable chat
- 32K+ = working with code/files
- 128K+ = large codebases, RAG

## KV cache optimization

```bash
# Reduce KV cache memory usage
OLLAMA_KV_CACHE_TYPE=q4_0 ollama run qwen3.5:4b
# q4_0 uses 4× less memory than fp16
```

## What's next

| Go to | Description |
|-------|-------------|
| [quantization.md](quantization.md) | Compress models |
| [benchmarks/apple-silicon.md](benchmarks/apple-silicon.md) | Speed on Mac |
| Back | [README.md](../README.md) |
