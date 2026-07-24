# 🗺 Learning Path: From Chat to Agent

> A step-by-step plan from first model run to a working multi-agent team.

**🇷🇺 Russian version:** [learning-path.ru.md](learning-path.ru.md)

---

[← FAQ](faq.md) · [Install on Windows →](setup-windows.md)

---

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

## Step 1. LM Studio — just try it

**Time:** 5 minutes
**Result:** you talk to a local AI model for the first time.

1. Download [LM Studio](https://lmstudio.ai) — GUI app, no terminal needed
2. Install and open
3. Search for "Qwen 3.5 4B", click Download
4. After download click Load Chat
5. Type: "Hello! What can you do?"

---

## Step 2. Ollama — install and run your first model

- **Mac:** [`../local-models/getting-started.md`](../local-models/getting-started.md)
- **Windows:** [setup-windows.md](setup-windows.md)
- **Linux:** [setup-linux.md](setup-linux.md)

```bash
ollama run qwen3.5:4b
```

---

## Step 3. Understand model types

Read:
- [`../local-models/running-models.md`](../local-models/running-models.md)
- [what-is-ai.md](what-is-ai.md)

Run two different models and compare:
```bash
ollama run phi4-mini        # small (2.5 GB)
ollama run llama3.3:8b      # larger (4.9 GB)
```

---

## Step 4. Context and quantization

Read:
- [`../local-models/memory-and-context.md`](../local-models/memory-and-context.md)
- [`../local-models/quantization.md`](../local-models/quantization.md)

Try:
```bash
OLLAMA_CONTEXT_LENGTH=16384 ollama run qwen3.5:4b
```

---

## Step 5. Choose a model for your task

Read [`../local-models/models.md`](../local-models/models.md). Download a coding model:
```bash
ollama pull qwen2.5-coder:7b
```

---

## Step 6. Set up local coding assistant

Follow [`../use-cases/coding.md`](../use-cases/coding.md):
- Install VS Code + Continue.dev
- Connect to Ollama
- Start coding with AI

---

## Step 7. Aider — terminal coding agent

```bash
pip install aider-chat
export OLLAMA_API_BASE=http://127.0.0.1:11434
aider --model ollama_chat/qwen2.5-coder:7b
```

More: [`../use-cases/coding.md`](../use-cases/coding.md)

---

## Step 8. Create your first AI agent

Follow the tutorial: [`../agents/tutorials/01-first-agent.md`](../agents/tutorials/01-first-agent.md)

---

## Step 9. Open WebUI — web chat interface

```bash
docker run -d -p 3000:8080 \
  -v open-webui:/app/backend/data \
  ghcr.io/open-webui/open-webui:main
```

Open `http://localhost:3000`

---

## Step 10. Multi-agent team

Follow tutorials:
1. [`../agents/tutorials/02-agent-team.md`](../agents/tutorials/02-agent-team.md) — agent team
2. [`../agents/multi-agent.md`](../agents/multi-agent.md) — multi-agent systems

---

Congratulations! You now have:
- A working local AI on your computer
- AI assistant in your code editor
- Terminal coding agent (Aider)
- Your first AI agent
- Understanding of how it all works

| Topic | Section |
|-------|---------|
| **RAG** — AI answers from your documents | [`../use-cases/rag.md`](../use-cases/rag.md) |
| **Automation** — AI runs on schedule | [`../use-cases/automation.md`](../use-cases/automation.md) |
| **Advanced agents** | [`../agents/README.md`](../agents/README.md) |
| **Safety** — how to protect your agent | [`../agents/safety.md`](../agents/safety.md) |
| **Agent memory** | [`../agents/memory.md`](../agents/memory.md) |
| **All 50+ models catalog** | [`../local-models/catalog.md`](../local-models/catalog.md) |

---

| If you want | Go to |
|-------------|-------|
| **Install Ollama on Mac** | [`../local-models/getting-started.md`](../local-models/getting-started.md) |
| **Install on Windows** | [setup-windows.md](setup-windows.md) |
| **Install on Linux** | [setup-linux.md](setup-linux.md) |
| **Start coding** | [`../use-cases/coding.md`](../use-cases/coding.md) |
| **Back to navigation** | [README.md](README.md) |
