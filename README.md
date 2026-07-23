[![English](https://img.shields.io/badge/English-README-blue.svg)](README.md)
[![Русский](https://img.shields.io/badge/Русский-README-blue.svg)](README.ru.md)

# 🧠 Awesome AI Handbook

> A practical guide to AI: from running models locally to building your own agents.
> Not just links — guides, benchmarks, and proven approaches.

> 📖 **Read in:** [Русский →](README.ru.md)

The **full handbook content is in Russian** ([README.ru.md](README.ru.md)); this page is the English navigation hub.

---

## About

**Mission:** help everyone explore AI — from first local model run to a working agent prototype.

---

## Quick start

```bash
brew install ollama          # macOS
ollama run qwen3.5:4b        # chat (~3.4 GB)
ollama run qwen2.5-coder:7b  # coding (~4.7 GB)
```

See [local-models/running-models.md](local-models/running-models.md) (content in Russian).

---

## Repository structure

```
awesome-ai-handbook/
├── README.ru.md              ← main content (Russian)
├── local-models/             ← local inference
│   ├── running-models.md
│   ├── memory-and-context.md
│   ├── catalog.md            ← 50+ models
│   ├── models.md
│   ├── tools.md
│   ├── quantization.md
│   ├── troubleshooting.md
│   └── benchmarks/apple-silicon.md
├── agents/
│   ├── patterns.md
│   ├── frameworks.md
│   ├── ollama-for-agents.md
│   └── tutorials/
├── use-cases/
│   ├── coding.md
│   ├── rag.md
│   ├── automation.md
│   └── writing.md
├── resources/
│   ├── ru.md
│   ├── en.md
│   ├── courses.md
│   └── communities.md
└── CONTRIBUTING.md
```

---

## Sections

| Section | Entry point |
|---------|-------------|
| Local models | [local-models/README.md](local-models/README.md) |
| AI agents | [agents/README.md](agents/README.md) |
| Use cases | [use-cases/README.md](use-cases/README.md) |
| Resources | [resources/README.md](resources/README.md) |

Full Russian handbook: **[README.ru.md](README.ru.md)**

---

## Contributing & license

[CONTRIBUTING.md](CONTRIBUTING.md) · [MIT](LICENSE)
