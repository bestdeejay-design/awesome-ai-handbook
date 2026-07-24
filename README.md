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
**51 files** · **~6 hours of reading** · **Levels: 🟢 beginner → 🔵 advanced**

---

## Quick start

```bash
brew install ollama          # macOS
ollama run qwen3.5:4b        # chat (~3.4 GB)
ollama run qwen2.5-coder:7b  # coding (~4.7 GB)
```

**New to AI?** Start with [basics/](basics/README.ru.md) — no prior knowledge needed.  
Full Russian handbook with navigation: **[README.ru.md →](README.ru.md)**

---

## Repository structure

```
awesome-ai-handbook/                          # 51 files
├── README.ru.md                              ← main Russian handbook
├── basics/                                   # 🟢 zero level (10 files)
│   ├── what-is-ai.ru.md                      #   AI/ML/LLM explained
│   ├── how-models-work.ru.md                 #   neural networks without math
│   ├── cloud-vs-local.ru.md                  #   ChatGPT vs local models
│   ├── hardware-guide.ru.md                  #   hardware requirements
│   ├── glossary.ru.md                        #   35 terms
│   ├── faq.ru.md                             #   FAQ
│   ├── learning-path.ru.md                   #   10 steps: chat → agent
│   ├── setup-windows.ru.md                   #   Windows installation
│   └── setup-linux.ru.md                     #   Linux installation
├── local-models/                             # 🔵 local inference (11 files)
│   ├── getting-started.ru.md                 #   🟢 Mac setup guide
│   ├── running-models.ru.md                  #   Ollama, LM Studio, HF
│   ├── models.ru.md                          #   choosing by task
│   ├── catalog.ru.md                         #   50+ models
│   ├── tools.ru.md                           #   engine comparison
│   ├── advanced-setup.ru.md                  #   🔵 Modelfile, API
│   └── benchmarks/apple-silicon.ru.md        #   tok/s on M1–M4
├── agents/                                   # 🤖 AI agents (15 files)
│   ├── ready-made.ru.md                      #   🟢 catalog of existing agents
│   ├── skills.ru.md                          #   🟡 skills, MCP, plugins
│   ├── tutorials/
│   │   ├── 01-first-agent.ru.md              #   🟢 ReAct agent in 30 min
│   │   ├── 02-agent-team.ru.md               #   🟡 multi-agent team
│   │   └── 03-coding-agent.ru.md             #   🔴 coding agent
│   └── ...                                   #   architecture, patterns, frameworks
├── use-cases/                                # 💼 applications (5 files)
│   ├── coding.ru.md                          #   Continue, Aider, OpenHands
│   ├── rag.ru.md                             #   Open WebUI, ChromaDB
│   ├── automation.ru.md                      #   n8n, cron, Telegram bot
│   └── writing.ru.md                         #   content pipeline
├── resources/                                # 📖 communities & courses (5 files)
│   ├── ru.ru.md                              #   🇷🇺 Russian resources
│   ├── en.ru.md                              #   EN newsletters, podcasts
│   ├── courses.ru.md                         #   free AI courses
│   └── communities.ru.md                     #   Discord, Reddit, Telegram
└── CONTRIBUTING.md
```

---

## Sections

| Section | Entry point | Level | Files |
|---------|-------------|-------|-------|
| Zero level | [basics/](basics/README.ru.md) | 🟢 | 10 |
| Local models | [local-models/](local-models/README.ru.md) | 🟢→🔵 | 11 |
| AI agents | [agents/](agents/README.ru.md) | 🟢→🔴 | 15 |
| Use cases | [use-cases/](use-cases/README.ru.md) | 🟡 | 5 |
| Resources | [resources/](resources/README.ru.md) | 🟢→🟡 | 5 |

Full Russian handbook with complete navigation: **[README.ru.md →](README.ru.md)**

---

## Contributing & license

[CONTRIBUTING.md](CONTRIBUTING.md) · [MIT](LICENSE)
