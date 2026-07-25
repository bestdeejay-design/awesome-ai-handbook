# 💻 AI for Coding

> Local models and tools for software development: from autocomplete to autonomous agents.

🟢 **Need the basics first?** Start with [basics/](../basics/README.md) — what AI is, how to pick a model, and install Ollama.

**🇷🇺 Russian version:** [coding.ru.md](coding.ru.md)

[← Use Cases](README.md) · [Step-by-step Tutorial →](../agents/tutorials/03-coding-agent.md)

---

## Contents

1. [Tools](#1-tools)
2. [Stack for M1 16 GB](#2-stack-for-m1-16-gb)
3. [Continue.dev — Free Copilot](#3-continuedev--free-copilot)
4. [Aider — CLI Agent for Code](#4-aider--cli-agent-for-code)
5. [OpenHands — Autonomous Software Engineer](#5-openhands--autonomous-software-engineer)
6. [What's Next](#6-whats-next)

---

## 1. Tools

| Tool | Type | Description | When to Use |
|------|------|-------------|-------------|
| [**Continue**](https://github.com/continuedev/continue) | IDE Plugin | Free Copilot with local model | Daily development in IDE |
| [**Cursor**](https://cursor.sh/) | AI IDE | IDE with AI built-in, Tab+Chat+Agent | Want everything in one |
| [**Aider**](https://github.com/paul-gauthier/aider) | CLI | Agent for edits in git repo | Refactoring, multi-file changes |
| [**OpenHands**](https://github.com/All-Hands-AI/OpenHands) | Autonomous Agent | Software engineer in a container | Autonomous task execution |

---

## 2. Stack for M1 16 GB

```
Continue IDE → Ollama (localhost:11434) → Qwen 2.5 Coder 7B
```

**Models for coding:**

| Model | Size | tok/s (M1 16GB) | Use Case |
|-------|------|-----------------|----------|
| `qwen2.5-coder:1.5b` | ~1.1 GB | 30+ | Tab autocomplete |
| `qwen2.5-coder:7b` | ~4.7 GB | 22–25 | Chat, code editing |
| `qwen3.5:4b` | ~3.4 GB | 28–35 | Fast chat, code explanation |
| `phi4-mini` | ~2.5 GB | 25–30 | Refactoring, logic |

Coding model catalog — [local-models/catalog.md](../local-models/catalog.md#1-coding-models).

---

## 3. Continue.dev — Free Copilot

Continue is a plugin for VS Code and JetBrains that connects to Ollama.

### Installation

```bash
# Install Continue plugin in VS Code
# Configure config.json:
```

### Config for Local Models

```json
{
  "models": [
    {
      "title": "Qwen 2.5 Coder 7B (main)",
      "provider": "ollama",
      "model": "qwen2.5-coder:7b",
      "apiBase": "http://localhost:11434"
    }
  ],
  "tabAutocompleteModel": {
    "title": "Qwen 2.5 Coder 1.5B (fast)",
    "provider": "ollama",
    "model": "qwen2.5-coder:1.5b",
    "apiBase": "http://localhost:11434"
  },
  "experimental": {
    "roles": [
      {
        "name": "coder",
        "model": "qwen2.5-coder:7b"
      },
      {
        "name": "architect",
        "model": "qwen3.5:4b"
      }
    ]
  }
}
```

### What You Can Do

- **Tab autocomplete** — model suggests code as you type
- **Chat** — select code → Ctrl+L → ask "what does this do?"
- **Inline edit** — select code → Ctrl+I → "rewrite in TypeScript"
- **@codebase** — questions about entire repo (via embeddings)

---

## 4. Aider — CLI Agent for Code

Aider runs in the terminal and edits files in a git repository itself.

### Installation

```bash
pip install aider-install
aider-install
```

### Running with Ollama

```bash
# Simple mode
aider --model ollama/qwen2.5-coder:7b

# Architect mode (2 models: one plans, one writes code)
aider --model ollama/qwen2.5-coder:7b \
  --editor-model ollama/qwen3:4b

# Work modes
aider --chat-mode ask       # questions about code
aider --chat-mode code      # writing code
aider --chat-mode architect # architect + editor
```

### Example Session

```bash
$ aider --model ollama/qwen2.5-coder:7b

> Add error handling to parse_config() function

Aider reads the file, adds try/except,
verifies the code works — and commits.
```

More on Aider — [local-models/tools.md](../local-models/tools.md#aider--cli-agent-for-coding).

---

## 5. OpenHands — Autonomous Software Engineer

OpenHands (formerly OpenDevin) runs in Docker and can execute tasks autonomously.

```bash
docker run -it --rm \
  -e LLM_MODEL=qwen2.5-coder:7b \
  -e LLM_API_KEY=ollama \
  -e LLM_BASE_URL=http://host.docker.internal:11434 \
  -v $(pwd)/workspace:/workspace \
  ghcr.io/all-hands-ai/openhands:latest
```

OpenHands creates a plan → writes code → runs tests → fixes errors — fully autonomously.

---

## 6. What's Next

| If You Want | Go To |
|-------------|-------|
| **Try a coding agent** from scratch | [03-coding-agent.md](../agents/tutorials/03-coding-agent.md) — step-by-step tutorial |
| **Build a team** of developer agents | [02-agent-team.md](../agents/tutorials/02-agent-team.md) |
| **Pick models** for coding | [../local-models/models.md](../local-models/models.md) |
| **Configure Ollama** for coding | [../local-models/advanced-setup.md](../local-models/advanced-setup.md) |
| **Back to use cases** | [README.md](README.md) |

---

**In section:** [coding](coding.md) · [rag](rag.md) · [automation](automation.md) · [writing](writing.md)  
**Related sections:** [Local Models](../local-models/README.md) · [AI Agents](../agents/README.md) · [Zero Level](../basics/README.md)  
**Navigation:** [← Use Cases](README.md) · [↑ Back to main](../README.md) · [🇷🇺 Русский](coding.ru.md)
