# 📖 File Catalog

> Full list of all 52 handbook files.  
> Press `Cmd+F` / `Ctrl+F` and type a keyword to find any topic.

[← Awesome AI Handbook](README.md) · [🇷🇺 Русский](catalog.ru.md)

---

## 🟢 Zero Level — `basics/`

| File | Description | Time |
|------|-------------|------|
| [README](basics/README.md) | Three learning paths (5 min / 30 min / 2 h) | — |
| [what-is-ai](basics/what-is-ai.md) | AI, ML, LLM — without math | 5 min |
| [how-models-work](basics/how-models-work.md) | How neural networks work (by analogy) | 10 min |
| [cloud-vs-local](basics/cloud-vs-local.md) | ChatGPT vs local models | 5 min |
| [hardware-guide](basics/hardware-guide.md) | Hardware requirements: RAM, VRAM, quantization | 5 min |
| [glossary](basics/glossary.md) | Glossary: 35+ terms from agent to embeddings | ∞ |
| [faq](basics/faq.md) | 15+ common beginner questions | 5 min |
| [learning-path](basics/learning-path.md) | 10 steps: from first chat to AI agent | 10 min |
| [setup-windows](basics/setup-windows.md) | Installing Ollama on Windows (native / WSL2) | 10 min |
| [setup-linux](basics/setup-linux.md) | Installing Ollama on Linux (CPU / CUDA / Docker) | 10 min |

## 🔵 Local Models — `local-models/`

| File | Description | Level | Time |
|------|-------------|-------|------|
| [README](local-models/README.md) | Scenarios: beginner → advanced | — | — |
| [getting-started](local-models/getting-started.md) | 🟢 Full Mac guide: Homebrew, Ollama, first model | 🟢 | 10 min |
| [running-models](local-models/running-models.md) | Finding and running models: Ollama, LM Studio, HuggingFace | 🟢🟡 | 15 min |
| [models](local-models/models.md) | Choosing a model for your task: coding, chat, RAG | 🟡 | 10 min |
| [catalog](local-models/catalog.md) | 50+ open-weight LLMs with specs | 🟡 | — |
| [quantization](local-models/quantization.md) | Quantization: Q4, Q5, Q8 — compression without quality loss | 🟡 | 10 min |
| [memory-and-context](local-models/memory-and-context.md) | RAM, KV cache, context — which model fits your hardware | 🟡 | 10 min |
| [tools](local-models/tools.md) | Engine comparison: Ollama, LM Studio, MLX, llama.cpp | 🟡 | 20 min |
| [advanced-setup](local-models/advanced-setup.md) | Modelfile, API, parallel requests, env vars | 🔵 | 15 min |
| [troubleshooting](local-models/troubleshooting.md) | Common issues and solutions | 🟡 | 5 min |
| [apple-silicon](local-models/benchmarks/apple-silicon.md) | Apple Silicon benchmarks: tok/s on M1–M4 | 🔵 | 5 min |

## 🤖 AI Agents — `agents/`

| File | Description | Level | Time |
|------|-------------|-------|------|
| [README](agents/README.md) | Section overview + minimal agent code | — | — |
| [ready-made](agents/ready-made.md) | 🟢 Ready-made AI agents: Aider, Cline, Cursor, OpenHands | 🟢 | 7 min |
| [orchestrators](agents/orchestrators.md) | Sub-agent orchestrators: Sisyphus, Atlas, Prometheus | 🔵 | 6 min |
| [skills](agents/skills.md) | Skills, MCP servers, plugins — extending agents | 🟡 | 6 min |
| [architecture](agents/architecture.md) | Agent architecture: LLM → tools → loop → memory | 🔵 | 11 min |
| [patterns](agents/patterns.md) | Architectural patterns: ReAct, Plan-and-Execute, Reflection | 🔵 | 14 min |
| [frameworks](agents/frameworks.md) | Framework comparison: LangGraph, CrewAI, Agno | 🔵 | 12 min |
| [multi-agent](agents/multi-agent.md) | Multi-agent communication and coordination | 🔵 | 18 min |
| [memory](agents/memory.md) | Long-term memory: context, storage, retrieval | 🔵 | 9 min |
| [prompting](agents/prompting.md) | System prompts for agent behavior control | 🔵 | 11 min |
| [ollama-for-agents](agents/ollama-for-agents.md) | Tool calling, JSON schema, structured output via Ollama | 🔵 | 10 min |
| [safety](agents/safety.md) | Safety: guardrails, limits, isolation, validation | 🔵 | 16 min |
| [evaluation](agents/evaluation.md) | Agent quality evaluation, regression testing | 🔵 | 10 min |

### Tutorials — `agents/tutorials/`

| File | Description | Level | Time |
|------|-------------|-------|------|
| [README](agents/tutorials/README.md) | Tutorial index | — | — |
| [01-first-agent](agents/tutorials/01-first-agent.md) | 🛠️ ReAct agent in Python + Ollama | 🟢 | 30 min |
| [02-agent-team](agents/tutorials/02-agent-team.md) | 🛠️ Multi-agent team (CrewAI) | 🟡 | 60 min |
| [03-coding-agent](agents/tutorials/03-coding-agent.md) | 🛠️ Coding agent (LangGraph) | 🔴 | 45 min |

## 💼 Use Cases — `use-cases/`

| File | Description | Stack | Time |
|------|-------------|-------|------|
| [README](use-cases/README.md) | Cases overview | — | — |
| [coding](use-cases/coding.md) | Free Copilot on a local model | Continue, Aider, OpenHands | 5 min |
| [rag](use-cases/rag.md) | Q&A on your files without the cloud | Open WebUI, ChromaDB | 7 min |
| [automation](use-cases/automation.md) | AI + business processes | n8n, cron, Telegram bot | 7 min |
| [writing](use-cases/writing.md) | Content pipeline: generation, reflection | Qwen, Reflection | 6 min |

## 📖 Resources — `resources/`

| File | Description | Audience | Time |
|------|-------------|----------|------|
| [README](resources/README.md) | Resource index | — | — |
| [ru](resources/ru.md) | Russian AI resources | 🟢 All | 3 min |
| [en](resources/en.md) | English resources | 🟡 | 3 min |
| [courses](resources/courses.md) | Free AI courses | 🟢🟡 | 3 min |
| [communities](resources/communities.md) | Discord, Reddit, Telegram chats | 🟢 All | 3 min |

## Root Files

| File | Description |
|------|-------------|
| [CONTRIBUTING](CONTRIBUTING.md) | How to contribute |
| [CODE_OF_CONDUCT](CODE_OF_CONDUCT.md) | Code of conduct |
| [SECURITY](SECURITY.md) | Security policy |

---

**Navigation:** [← Awesome AI Handbook](README.md) · [🇷🇺 Русский](catalog.ru.md)
