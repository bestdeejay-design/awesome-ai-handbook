# 🧠 Awesome AI Handbook

> A practical guide to AI: from running your first local model to building your own agents.  
> **52 files** · **~6 hours reading** · **Levels: 🟢 beginner → 🔴 advanced**

**[Русский](README.ru.md)** · **English**

---

## 🚀 Quick Start

```bash
brew install ollama            # macOS
ollama run qwen3.5:4b          # chat (~3.4 GB) — ready!
```

**New to AI?** → [basics/](basics/README.md) — start from zero, no terminal required.  
**Already experienced?** → [file catalog](catalog.md) — find any topic in 10 seconds.

---

## 📚 Sections

| Section | Description | Entry | Files | Level |
|---------|-------------|-------|-------|-------|
| [🟢 Zero Level](basics/README.md) | AI basics: terms, setup, hardware choice | `basics/` | 9 | 🟢 |
| [🔵 Local Models](local-models/README.md) | Running, choosing, tuning and optimizing models | `local-models/` | 11 | 🟢→🔵 |
| [🤖 AI Agents](agents/README.md) | Architecture, patterns, frameworks, tutorials | `agents/` | 17 | 🟢→🔴 |
| [💼 Use Cases](use-cases/README.md) | Coding, RAG, automation, writing | `use-cases/` | 5 | 🟡 |
| [📖 Resources](resources/README.md) | Communities, courses, RU and EN materials | `resources/` | 5 | 🟢→🟡 |

> 🔍 **Looking for a specific file?** [Open catalog →](catalog.md) — full list of all 52 files with descriptions.

---

## 🗂️ Project Structure

```
awesome-ai-handbook/
│
├── README.md, README.ru.md      ← navigation (EN + RU)
├── catalog.md, catalog.ru.md    ← full file catalog
│
├── basics/                      # 🟢 zero level
│   ├── README.md                #   learning paths
│   ├── what-is-ai.md            #   AI, ML, LLM — without math
│   ├── how-models-work.md       #   neural networks by analogy
│   ├── cloud-vs-local.md        #   ChatGPT vs local
│   ├── hardware-guide.md        #   RAM, VRAM, quantization
│   ├── glossary.md              #   35 terms
│   ├── faq.md                   #   15+ common questions
│   ├── learning-path.md         #   10 steps: chat → agent
│   ├── setup-windows.md         #   Windows installation
│   └── setup-linux.md           #   Linux installation
│
├── local-models/                # 🔵 local inference
│   ├── README.md                #   scenarios: beginner → pro
│   ├── getting-started.md       #   🟢 full Mac setup guide
│   ├── running-models.md        #   from Ollama to HuggingFace
│   ├── models.md                #   choosing by task
│   ├── catalog.md               #   50+ models with specs
│   ├── quantization.md          #   Q4, Q5, Q8 — compression
│   ├── memory-and-context.md    #   RAM, KV cache, context
│   ├── tools.md                 #   engine comparison
│   ├── advanced-setup.md        #   🔵 Modelfile, API, env
│   ├── troubleshooting.md       #   common issues
│   └── benchmarks/
│       └── apple-silicon.md     #   🔵 tok/s on M1–M4
│
├── agents/                      # 🤖 AI agents
│   ├── README.md                #   overview + minimal code
│   ├── ready-made.md            #   🟢 existing agents catalog
│   ├── orchestrators.md         #   🔵 orchestrators
│   ├── skills.md                #   🟡 skills, MCP, plugins
│   ├── architecture.md          #   🔵 agent architecture
│   ├── patterns.md              #   🔵 ReAct, Multi-Agent
│   ├── frameworks.md            #   🔵 LangGraph, CrewAI
│   ├── multi-agent.md           #   🔵 communication
│   ├── memory.md                #   🔵 long-term memory
│   ├── prompting.md             #   🔵 system prompts
│   ├── ollama-for-agents.md     #   🔵 tool calling
│   ├── safety.md                #   🔵 guardrails
│   ├── evaluation.md            #   🔵 testing
│   └── tutorials/
│       ├── README.md            #   tutorial index
│       ├── 01-first-agent.md    #   🟢 first agent
│       ├── 02-agent-team.md     #   🟡 agent team
│       └── 03-coding-agent.md   #   🔴 coding agent
│
├── use-cases/                   # 💼 applications
│   ├── README.md                #   cases overview
│   ├── coding.md                #   🟡 Continue, Aider
│   ├── rag.md                   #   🟡 Open WebUI, ChromaDB
│   ├── automation.md            #   🟡 n8n, Telegram bot
│   └── writing.md               #   🟡 content pipeline
│
├── resources/                   # 📖 communities & courses
│   ├── README.md                #   index
│   ├── ru.md                    #   🟢 Russian resources
│   ├── en.md                    #   🟡 English resources
│   ├── courses.md               #   🟢🟡 free courses
│   └── communities.md           #   🟢 Discord, Reddit
│
├── CONTRIBUTING.md              # contributing guide
├── CODE_OF_CONDUCT.md           # code of conduct
└── SECURITY.md                  # security policy
```

---

## 🤝 Contributing

Benchmarks, RU resources, tutorials, and fixes are welcome.  
Details: [CONTRIBUTING.md](CONTRIBUTING.md) · [Code of Conduct](CODE_OF_CONDUCT.md)

## 📄 License

[MIT](LICENSE)

---

*Awesome AI Handbook — helping everyone explore AI, regardless of skill level or hardware.*
