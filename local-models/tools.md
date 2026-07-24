# 🛠 Tools Comparison

> Complete comparison of local inference tools: Ollama, LM Studio, MLX, llama.cpp, vLLM, GPT4All.

**🇷🇺 Russian version:** [tools.ru.md](tools.ru.md)

---

[← Local models](../README.md) · [Setup →](advanced-setup.md)

---

Covers: Ollama, LM Studio, MLX, llama.cpp, vLLM, GPT4All, and more.

## Core engines

| Tool | Platform | Format | GPU | Best for |
|------|----------|--------|-----|----------|
| **Ollama** | macOS/Linux/Win | GGUF | CUDA/Metal | General use, agents |
| **LM Studio** | macOS/Linux/Win | GGUF | CUDA/Metal | GUI, beginners |
| **llama.cpp** | macOS/Linux/Win | GGUF | CUDA/Metal | Maximum control |
| **MLX** | macOS (M series) | MLX | Metal | Speed on Apple Silicon |
| **vLLM** | Linux | AWQ/GPTQ | CUDA | Production serving |
| **GPT4All** | macOS/Linux/Win | GGUF | CPU | Privacy, offline |

## Quick comparison

| Feature | Ollama | LM Studio | llama.cpp | MLX | vLLM |
|---------|--------|-----------|-----------|-----|------|
| Setup | 1 command | Download | Build from source | pip install | pip/Docker |
| GPU support | ✅ | ✅ | ✅ | ✅ (M only) | ✅ (NVIDIA) |
| Quantization | K-quants | K-quants | K-quants | 2/3/4/6 bit | AWQ/GPTQ |
| Tool calling | ✅ | ❌ | ❌ | ❌ | ❌ |
| Docker | ✅ | ❌ | ❌ | ❌ | ✅ |
| Multi-model | ✅ | ✅ | Manual | Manual | ✅ |
| Speed (M1 7B) | 22-25 tok/s | 22-28 tok/s | 20-25 tok/s | 25-35 tok/s | N/A |

## MLX vs llama.cpp on Apple Silicon

| Model | Ollama (llama.cpp) | MLX | Difference |
|-------|-------------------|-----|------------|
| 7B Q4 | 22-25 tok/s | 28-35 tok/s | MLX +30% |
| 30B MoE | 70 tok/s | 102 tok/s | MLX +46% |
| Memory | More | Less | MLX -49% |

Since March 2026, Ollama can use MLX backend on Mac with 32 GB+ RAM.

## What's next

| Go to | Description |
|-------|-------------|
| [advanced-setup.md](advanced-setup.md) | Modelfile, API tuning |
| [benchmarks/apple-silicon.md](benchmarks/apple-silicon.md) | Speed on Mac |
| Back | [README.md](../README.md) |

[← Back to navigation](README.md)
