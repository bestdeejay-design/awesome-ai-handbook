# <img src="https://lucide.dev/api/icons/brain" alt="" width="20" height="20" style="display:inline;vertical-align:middle"> Awesome AI Handbook

> A practical guide to AI: from running your first local model to building your own agents.  
> **52 files** · **~6 hours reading** · **Levels: <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> beginner → <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-red.svg" alt="" width="12" height="12" style="vertical-align:middle"> advanced**

<p align="center">
  <a href="https://bestdeejay-design.github.io/awesome-ai-handbook/"><img src="https://img.shields.io/badge/GitHub_Pages-site-22272e?style=flat-square&logo=githubpages" alt="GitHub Pages"></a>
  <a href="https://github.com/bestdeejay-design/awesome-ai-handbook"><img src="https://img.shields.io/badge/GitHub-repo-181717?style=flat-square&logo=github" alt="GitHub"></a>
  <a href="https://github.com/bestdeejay-design/awesome-ai-handbook/stargazers"><img src="https://img.shields.io/github/stars/bestdeejay-design/awesome-ai-handbook?style=flat-square&logo=github" alt="Stars"></a>
</p>

**[Русский](README.ru.md)** · **English**

---

## <img src="https://lucide.dev/api/icons/rocket" alt="" width="20" height="20" style="display:inline;vertical-align:middle"> Quick Start

```bash
brew install ollama            # macOS
ollama run qwen3.5:4b          # chat (~3.4 GB) — ready!
```

**New to AI?** → [basics/](basics/README.md) — start from zero, no terminal required.  
**Already experienced?** → [file catalog](catalog.md) — find any topic in 10 seconds.

---

## <img src="https://lucide.dev/api/icons/book-open" alt="" width="20" height="20" style="display:inline;vertical-align:middle"> Sections

| Section | Description | Entry | Files | Level |
|---------|-------------|-------|-------|-------|
| [<img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> Zero Level](basics/README.md) | AI basics: terms, setup, hardware choice | `basics/` | 9 | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> |
| [<img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> Local Models](local-models/README.md) | Running, choosing, tuning and optimizing models | `local-models/` | 11 | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle">→<img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> |
| [<img src="https://lucide.dev/api/icons/bot" alt="" width="20" height="20" style="display:inline;vertical-align:middle"> AI Agents](agents/README.md) | Architecture, patterns, frameworks, tutorials | `agents/` | 17 | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle">→<img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-red.svg" alt="" width="12" height="12" style="vertical-align:middle"> |
| [<img src="https://lucide.dev/api/icons/briefcase" alt="" width="20" height="20" style="display:inline;vertical-align:middle"> Use Cases](use-cases/README.md) | Coding, RAG, automation, writing | `use-cases/` | 5 | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-yellow.svg" alt="" width="12" height="12" style="vertical-align:middle"> |
| [<img src="https://lucide.dev/api/icons/book-open" alt="" width="20" height="20" style="display:inline;vertical-align:middle"> Resources](resources/README.md) | Communities, courses, RU and EN materials | `resources/` | 5 | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle">→<img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-yellow.svg" alt="" width="12" height="12" style="vertical-align:middle"> |

> <img src="https://lucide.dev/api/icons/search" alt="" width="20" height="20" style="display:inline;vertical-align:middle"> **Looking for a specific file?** [Open catalog →](catalog.md) — full list of all 52 files with descriptions.

---

## <img src="https://lucide.dev/api/icons/folder-tree" alt="" width="20" height="20" style="display:inline;vertical-align:middle"> Project Structure

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

## <img src="https://lucide.dev/api/icons/handshake" alt="" width="20" height="20" style="display:inline;vertical-align:middle"> Contributing

Benchmarks, RU resources, tutorials, and fixes are welcome.  
Details: [CONTRIBUTING.md](CONTRIBUTING.md) · [Code of Conduct](CODE_OF_CONDUCT.md)

## <img src="https://lucide.dev/api/icons/file-text" alt="" width="20" height="20" style="display:inline;vertical-align:middle"> License

[MIT](LICENSE)

---

*Awesome AI Handbook — helping everyone explore AI, regardless of skill level or hardware.*
