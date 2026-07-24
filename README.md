[![English](https://img.shields.io/badge/English-README-blue.svg)](README.md)
[![Русский](https://img.shields.io/badge/Русский-README-blue.svg)](README.ru.md)

# 🧠 Awesome AI Handbook

> A practical guide to AI: from running models locally to building your own agents.
> Not just links — guides, benchmarks, and proven approaches.

> 📖 **Read in:** [Русский](README.ru.md) · [English](README.md)

The handbook is available in **English** (`.md`) and **Russian** (`.ru.md`).  
This page is the English navigation hub — all content is fully translated.

---

## About

**Mission:** help everyone explore AI — from first local model run to a working agent prototype.  
**52 files** · **~6 hours of reading** · **Levels: 🟢 beginner → 🔵 advanced**

---

## Quick start

```bash
brew install ollama          # macOS
ollama run qwen3.5:4b        # chat (~3.4 GB)
ollama run qwen2.5-coder:7b  # coding (~4.7 GB)
```

**New to AI?** Start with [basics/](basics/README.md) — no prior knowledge needed.  
Full Russian handbook with navigation: **[README.ru.md →](README.ru.md)**

---

## Repository structure

```
awesome-ai-handbook/                          # 52 topics (EN + RU)
├── README.md, README.ru.md                   ← bilingual handbooks
├── basics/                                   # 🟢 zero level (10 files)
│   ├── what-is-ai.md / .ru.md                #   AI/ML/LLM explained
│   ├── how-models-work.md / .ru.md           #   neural networks without math
│   ├── cloud-vs-local.md / .ru.md            #   ChatGPT vs local models
│   ├── hardware-guide.md / .ru.md            #   hardware requirements
│   ├── glossary.md / .ru.md                  #   35 terms
│   ├── faq.md / .ru.md                       #   FAQ
│   ├── learning-path.md / .ru.md             #   10 steps: chat → agent
│   ├── setup-windows.md / .ru.md             #   Windows installation
│   └── setup-linux.md / .ru.md              #   Linux installation
├── local-models/                             # 🔵 local inference (11 files)
│   ├── getting-started.md / .ru.md           #   🟢 Mac setup guide
│   ├── running-models.md / .ru.md            #   Ollama, LM Studio, HF
│   ├── models.md / .ru.md                    #   choosing by task
│   ├── catalog.md / .ru.md                   #   50+ models
│   ├── tools.md / .ru.md                     #   engine comparison
│   ├── advanced-setup.md / .ru.md            #   🔵 Modelfile, API
│   └── benchmarks/apple-silicon.md / .ru.md  #   tok/s on M1–M4
├── agents/                                   # 🤖 AI agents (17 files)
│   ├── ready-made.md / .ru.md                #   🟢 catalog of existing agents
│   ├── orchestrators.md / .ru.md             #   🔵 sub-agent orchestrators
│   ├── skills.md / .ru.md                    #   🟡 skills, MCP, plugins
│   ├── tutorials/                            #   step-by-step tutorials
│   │   ├── 01-first-agent.md / .ru.md        #   🟢 ReAct agent in 30 min
│   │   ├── 02-agent-team.md / .ru.md         #   🟡 multi-agent team
│   │   └── 03-coding-agent.md / .ru.md       #   🔴 coding agent
│   └── ...                                   #   architecture, patterns, etc.
├── use-cases/                                # 💼 applications (5 files)
│   ├── coding.md / .ru.md                    #   Continue, Aider, OpenHands
│   ├── rag.md / .ru.md                       #   Open WebUI, ChromaDB
│   ├── automation.md / .ru.md                #   n8n, cron, Telegram bot
│   └── writing.md / .ru.md                   #   content pipeline
├── resources/                                # 📖 communities & courses (5 files)
│   ├── ru.md / .ru.md                        #   🇷🇺 Russian resources
│   ├── en.md / .ru.md                        #   EN newsletters, podcasts
│   ├── courses.md / .ru.md                   #   free AI courses
│   └── communities.md / .ru.md              #   Discord, Reddit, Telegram
└── CONTRIBUTING.md
```

---

## Sections

| Section | Entry point | Level | Files |
|---------|-------------|-------|-------|
| Zero level | [basics/](basics/README.md) | 🟢 | 10 |
| Local models | [local-models/](local-models/README.md) | 🟢→🔵 | 11 |
| AI agents | [agents/](agents/README.md) | 🟢→🔴 | 17 |
| Use cases | [use-cases/](use-cases/README.md) | 🟡 | 5 |
| Resources | [resources/](resources/README.md) | 🟢→🟡 | 5 |

Full Russian handbook with complete navigation: **[README.ru.md →](README.ru.md)**

---

## Contributing & license

[CONTRIBUTING.md](CONTRIBUTING.md) · [MIT](LICENSE)
