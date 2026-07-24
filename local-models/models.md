# 🎯 Models for Different Tasks

> Recommendations for choosing models based on your tasks and hardware.

**🇷🇺 Russian version:** [models.ru.md](models.ru.md)

---

[← Local models](../README.md) · [Catalog →](catalog.md)

---

## Quick picks

| Task | Recommended model | RAM needed |
|------|------------------|------------|
| Chat, general Q&A | Qwen 3.5 9B | 8-16 GB |
| Coding | Qwen 2.5 Coder 7B | 8-16 GB |
| Code autocomplete | CodeGemma 2B | 4-8 GB |
| Reasoning | DeepSeek R1 Distill 7B | 8-16 GB |
| Math | Phi-4-mini (3.8B) | 4-8 GB |
| Vision | Llama 3.2 Vision 11B | 8-16 GB |
| Russian language | Qwen 3.5 9B | 8-16 GB |
| RAG embeddings | nomic-embed-text | 4 GB |

## By hardware

| Your RAM | Best model to run |
|----------|-------------------|
| 8 GB | Phi-4-mini (3.8B), Qwen 3.5 4B |
| 16 GB | Qwen 2.5 Coder 7B, Qwen 3.5 9B |
| 24 GB | Qwen3.6-27B (Q4), Gemma 4 26B MoE |
| 32 GB | Llama 3.1 70B (Q3) |
| 48 GB | Llama 3.3 70B (Q4) |
| 64 GB | Gemma 4 31B (Q8) |

## Key considerations

- **FIM** (Fill-in-Middle) — needed for code autocomplete. Qwen Coder has it.
- **Tool calling** — needed for agents. Qwen 3.5+, Llama 3.1+
- **Long context** — 128K+ for working with large codebases
- **Multimodal** — vision models for image understanding

---

## What's next

| Go to | Description |
|-------|-------------|
| [catalog.md](catalog.md) | Full 50+ model catalog |
| [benchmarks/apple-silicon.md](benchmarks/apple-silicon.md) | Speed tests on Mac |
| Back | [README.md](../README.md) |
