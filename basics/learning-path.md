# <img src="https://lucide.dev/api/icons/map" alt="" width="20" height="20" style="vertical-align:middle"> Learning Path: From Chat to Agent

> A step-by-step plan from first model run to a working multi-agent team.

**🇷🇺 Russian version:** [learning-path.ru.md](learning-path.ru.md)

---

[← FAQ](faq.md) · [Install on Windows →](setup-windows.md)

---

## Route overview (2–3 hours)

This plan takes you from installing your first program to building your own AI agent.
Each step takes **10–30 minutes**. Follow them in order.

```
Step 1  LM Studio — just try it
Step 2  Install Ollama and run a model
Step 3  Understand model types and differences
Step 4  Learn about context and quantization
Step 5  Choose a model for your task
Step 6  Set up a local coding assistant
Step 7  Try Aider — terminal coding agent
Step 8  Create your first AI agent
Step 9  Set up Open WebUI — web chat interface
Step 10 Build an agent team for your project
```

---

## <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> Step 1. LM Studio — just try it

**Time:** 5 minutes
**Result:** you talk to a local AI model for the first time.

1. Download [LM Studio](https://lmstudio.ai) — GUI app, no terminal needed
2. Install and open
3. Search for "Qwen 3.5 4B", click Download
4. After download click Load Chat
5. Type: "Hello! What can you do?"

Done. You just ran an AI model on your computer. Free, offline, no registration.

---

## <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> Step 2. Ollama — install and run your first model

**Time:** 10 minutes  
**Result:** you install Ollama and run models via the terminal.

Choose your platform:

- **Mac:** [`../local-models/getting-started.md`](../local-models/getting-started.md)
- **Windows:** [setup-windows.md](setup-windows.md)
- **Linux:** [setup-linux.md](setup-linux.md)

```bash
ollama run qwen3.5:4b
```
Chat with the model. Try `/bye` to exit.

---

## <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-yellow.svg" alt="" width="12" height="12" style="vertical-align:middle"> Step 3. Understand model types

**Time:** 15 minutes  
**Result:** you understand how models differ and which to choose.

Read:
- [`../local-models/running-models.md`](../local-models/running-models.md)
- [what-is-ai.md](what-is-ai.md)

Run two different models and compare:
```bash
ollama run phi4-mini        # small (2.5 GB)
ollama run llama3.3:8b      # larger (4.9 GB)
```

---

## <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-yellow.svg" alt="" width="12" height="12" style="vertical-align:middle"> Step 4. Context and quantization

**Time:** 15 minutes  
**Result:** you understand why models are slow and how to fix it.

Read:
- [`../local-models/memory-and-context.md`](../local-models/memory-and-context.md)
- [`../local-models/quantization.md`](../local-models/quantization.md)

Try:
```bash
OLLAMA_CONTEXT_LENGTH=16384 ollama run qwen3.5:4b
```

---

## <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-yellow.svg" alt="" width="12" height="12" style="vertical-align:middle"> Step 5. Choose a model for your task

**Time:** 10 minutes  
**Result:** you know which model to use for coding, chat, translation, RAG.

Read [`../local-models/models.md`](../local-models/models.md). Download a coding model:
```bash
ollama pull qwen2.5-coder:7b
```

---

## <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> Step 6. Set up local coding assistant

**Time:** 20 minutes  
**Result:** AI helps you write code right in your editor.

Follow [`../use-cases/coding.md`](../use-cases/coding.md):
- Install VS Code + Continue.dev
- Connect to Ollama
- Start coding with AI

**What to try:**
- `Cmd+I` — open AI chat in your editor
- Select code and ask "explain this function" or "find bugs"
- Ask it to write a function: "write a function that sorts a list in descending order"

---

## <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> Step 7. Aider — terminal coding agent

**Time:** 15 minutes  
**Result:** you can give AI coding tasks and it modifies code on its own.

```bash
# Install
pip install aider-chat

# Run with a local model
export OLLAMA_API_BASE=http://127.0.0.1:11434
aider --model ollama_chat/qwen2.5-coder:7b
```

**What to try:**
1. Open any Python file
2. Type: "add docstrings to all functions in this file"
3. Aider reads the file, makes changes, and shows you the diff

More: [`../use-cases/coding.md`](../use-cases/coding.md)

---

## <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> Step 8. Create your first AI agent

**Time:** 30 minutes  
**Result:** you wrote a program where the model decides when to call a tool on its own.

Follow the tutorial: [`../agents/tutorials/01-first-agent.md`](../agents/tutorials/01-first-agent.md)

You'll build an agent that:
- Receives a task
- Decides whether it needs to call a tool (calculator, search)
- Executes and returns the result
- Repeats until the task is solved

---

## <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> Step 9. Open WebUI — web chat interface

**Time:** 15 minutes  
**Result:** you get a web interface for models, like ChatGPT, but local.

Via Docker:
```bash
docker run -d -p 3000:8080 \
  -v open-webui:/app/backend/data \
  ghcr.io/open-webui/open-webui:main
```

Or without Docker, via pip:
```bash
pip install open-webui
open-webui serve
```

Open `http://localhost:3000`

---

## <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> Step 10. Multi-agent team

**Time:** 30–60 minutes  
**Result:** you have several AI agents working together — one plans, another codes, a third tests.

Follow tutorials:
1. [`../agents/tutorials/02-agent-team.md`](../agents/tutorials/02-agent-team.md) — agent team
2. [`../agents/multi-agent.md`](../agents/multi-agent.md) — multi-agent systems

---

## After 10 steps

Congratulations! You now have:
- A working local AI on your computer
- AI assistant in your code editor
- Terminal coding agent (Aider)
- Your first AI agent
- Understanding of how it all works

**Where to grow further:**

| Topic | Section |
|-------|---------|
| **RAG** — AI answers from your documents | [`../use-cases/rag.md`](../use-cases/rag.md) |
| **Automation** — AI runs on schedule | [`../use-cases/automation.md`](../use-cases/automation.md) |
| **Advanced agents** | [`../agents/README.md`](../agents/README.md) |
| **Safety** — how to protect your agent | [`../agents/safety.md`](../agents/safety.md) |
| **Agent memory** | [`../agents/memory.md`](../agents/memory.md) |
| **All 50+ models catalog** | [`../local-models/catalog.md`](../local-models/catalog.md) |

---

## Shortcut route (1 hour)

If you're really short on time:

```
Step 1 → LM Studio (5 min)
Step 2 → Ollama + model (10 min)
Step 6 → Continue.dev + VS Code (20 min)
Step 8 → First agent (30 min)
```

---

## What's next

| If you want | Go to |
|-------------|-------|
| **Install Ollama on Mac** | [`../local-models/getting-started.md`](../local-models/getting-started.md) |
| **Install on Windows** | [setup-windows.md](setup-windows.md) |
| **Install on Linux** | [setup-linux.md](setup-linux.md) |
| **Start coding** | [`../use-cases/coding.md`](../use-cases/coding.md) |
| **Back to navigation** | [README.md](README.md) |

---

**In section:** [what-is-ai](what-is-ai.md) · [how-models-work](how-models-work.md) · [cloud-vs-local](cloud-vs-local.md) · [hardware-guide](hardware-guide.md) · [glossary](glossary.md) · [faq](faq.md) · [learning-path](learning-path.md) · [setup-windows](setup-windows.md) · [setup-linux](setup-linux.md)  
**Related sections:** [Local Models](../local-models/README.md) · [AI Agents](../agents/README.md) · [Use Cases](../use-cases/README.md)  
**Navigation:** [← Zero Level](README.md) · [↑ Back to main](../README.md) · [🇷🇺 Русский](learning-path.ru.md)
