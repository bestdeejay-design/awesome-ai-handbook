# <img src="https://lucide.dev/api/icons/rocket" alt="" width="20" height="20" style="vertical-align:middle"> Getting Started

> Step-by-step guide for first-time local AI users. No special knowledge required — everything explained from scratch.

<img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> **Complete beginner?** Start with [basics/](../basics/README.md) — what AI is, how models work, what hardware you need.

**🇷🇺 Russian version:** [getting-started.ru.md](getting-started.ru.md)

---

[← Local models](README.md) · [Running models →](running-models.md)

---

## Contents

1. [What are local models and why use them](#1-what-are-local-models-and-why-use-them)
2. [How to open the terminal](#2-how-to-open-the-terminal)
3. [Check your Mac](#3-check-your-mac)
4. [Install Homebrew](#4-install-homebrew)
5. [Install Ollama](#5-install-ollama)
6. [Run your first model](#6-run-your-first-model)
7. [Install LM Studio (no terminal alternative)](#7-install-lm-studio-no-terminal-alternative)
8. [Whats next](#8-whats-next)

---

## 1. What are local models and why use them

A **local AI model** is a program that runs directly on your computer. You download it once, and it works fully offline, for free, with no limits.

### Local vs Cloud

| ChatGPT / Claude | Local model |
|---|---|
| Needs internet | Works offline |
| Pay per request | Free (just electricity) |
| Your data goes to servers | Everything stays on your Mac |
| Message limits | No limits |
| Cannot customize | Configurable |

**What you will be able to do after this guide:**
- Ask questions to the model directly in the terminal (like a chat)
- Connect the model to a code editor for autocompletion
- Use the model for translation, text analysis, writing code
- All of this — for free and without internet

---

## 2. How to open the terminal

The terminal is a window where you type commands as text instead of clicking buttons. Many commands in this guide need to be entered in the terminal.

### Finding Terminal on Mac

**Method 1 — Spotlight (fast):**
1. Press `Cmd (⌘) + Space` — search appears
2. Type "Terminal"
3. Press `Enter`

**Method 2 — Finder:**
1. Open Finder
2. Go → Utilities
3. Double-click "Terminal"

### What the terminal looks like

You will see something like:
```
MacBook-Air:~ user$
```
or
```
user@MacBook-Air ~ %
```

This is the **command prompt**. It means the terminal is ready for your command.

### Your first command

Type or paste this command into the terminal (right-click → Paste, or `Cmd+V`):

```bash
echo "Hello, Im running an AI model!"
```

Press `Enter`. The terminal should respond with:

```
Hello, Im running an AI model!
```

> **Important:** Commands starting with `$` in guides should be typed without the `$`.
> It just represents the prompt. E.g., `$ brew install ollama` → type `brew install ollama`.

---

## 3. Check your Mac

### What chip do you have?

```bash
uname -m
```

- `arm64` → Apple Silicon (M1/M2/M3/M4) — great, models run fast
- `x86_64` → Intel Mac — still works, slower on large models

### How much RAM?

```bash
sysctl hw.memsize | awk '{print "RAM: " $2 / 1073741824 " GB"}'
```

Example output: `RAM: 16 GB`

### Detailed info

```bash
system_profiler SPHardwareDataType | grep -E "Chip|Memory|Processor"
```

Example output:
```
Chip: Apple M1
Memory: 16 GB
```

### RAM guide

| Your RAM | Suitable models |
|----------|----------------|
| 8 GB | Small models (up to 4B params) — Qwen 3.5 4B |
| **16 GB** <img src="https://lucide.dev/api/icons/star" alt="" width="20" height="20" style="vertical-align:middle"> | **Mid-size (7B–9B)** — universal choice |
| 24–32 GB | Large models (14B–32B) |
| 48+ GB | Very large (70B+) |

> Most base-model MacBook Air/Pro have **16 GB RAM** — optimal for local models.

---

## 4. Install Homebrew

**Homebrew** is the "app store" for the terminal. It installs software not found in the App Store.

### Check if already installed

```bash
brew --version
```

- If you see `Homebrew 4.x.x` → skip to [Install Ollama](#5-install-ollama)
- If you see `command not found: brew` → install it with the command below.

### Install Homebrew

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

After pressing Enter:
1. Terminal may ask for your Mac password — type it (characters wont show, thats normal). Press Enter.
2. Installation takes 1–5 minutes.
3. Wait for `Installation successful!`

### Verify installation

```bash
brew --version
```

Should show:
```
Homebrew 4.x.x
```

### Apple Silicon PATH fix

On M1/M2/M3/M4 you might see: `Warning: /opt/homebrew/bin is not in your PATH...`

```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zshrc
source ~/.zshrc
```

Then verify again:
```bash
brew --version
```

---

## 5. Install Ollama

**Ollama** is the simplest program for running AI models. It can:
- Download models on demand
- Run them with one command
- Work as a server for other programs

```bash
brew install ollama
```

Verify:
```bash
ollama --version
```

Should show: `ollama version 0.x.x`

### Start the Ollama server

After installation, start the background server:

```bash
ollama serve
```

You should see:
```
[GIN] ... Listening on 127.0.0.1:11434
```

**Keep this terminal window open.** Open a **new** terminal window (`Cmd+N`) for the next steps.

Verify the server is running:
```bash
curl http://localhost:11434/api/version
```

You should see: `{"version":"0.x.x"}`

### Auto-start (convenient)

After installation, the **Ollama** app appears in your Applications folder. Search for "Ollama" via Spotlight or in Applications and launch it once — it will automatically start the server on every boot. You will see the Ollama icon in the menu bar.

---

## 6. Run your first model

In your **new** terminal (not the one running `ollama serve`):

```bash
ollama run qwen3.5:4b
```

**What happens:**
1. If the model is not downloaded — Ollama starts downloading (shows a progress bar)
2. Model size: ~3.4 GB (like one HD movie)
3. Download time: 1–10 minutes depending on your internet
4. After download, you will see the `>>>` prompt — the model is ready for conversation.

### How to chat with the model

Simply type your questions and press Enter. The model will generate a response. After it finishes, the `>>>` prompt appears again — you can ask the next question.

### What to try asking

```
>>> Write a poem about a programmer
>>> Explain recursion in simple terms
>>> Write a Python function to check if a number is prime
>>> Translate this to English: "Мне нужно запустить AI-модель локально"
>>> Write a study plan for learning Python for a beginner
>>> /bye  # exit the chat
```

### Chat commands

- `/bye` — exit (model unloads from RAM)
- `/clear` — clear history (start fresh)
- `/show info` — model info
- `Ctrl+C` — stop generation

### Try different models

```bash
ollama run qwen2.5-coder:7b    # coding model
ollama run llama3.3:8b         # Meta model
ollama run phi4-mini           # small but smart
```

### Model comparison

| Model | Size | Best for |
|-------|------|----------|
| `qwen3.5:4b` | 3.4 GB | Universal chat, fast |
| `qwen2.5-coder:7b` | 4.7 GB | Code generation |
| `llama3.3:8b` | 4.9 GB | Reasoning, English |
| `phi4-mini` | 2.5 GB | Math, logic |

> On a MacBook with **16 GB RAM** — all fit.
> With **8 GB** — use only `qwen3.5:4b` or `phi4-mini`.

### Where to find other models

Browse the full model catalog: [ollama.com/search](https://ollama.com/search)

Each model page shows its size (in the Size column) — use this to check if it fits your RAM.

### Download without running

```bash
ollama pull qwen3.5:9b    # download only, dont start chat
ollama list               # see all downloaded models
```

---

## 7. Install LM Studio (no terminal alternative)

If you prefer a GUI over the command line, use **LM Studio** — a desktop app with buttons and menus where everything is done with a mouse.

### Installation

1. Open [lmstudio.ai](https://lmstudio.ai) in your browser
2. Click "Download for macOS" — downloads a `.dmg` file
3. Open the downloaded file (usually in Downloads folder)
4. Drag LM Studio to your Applications folder
5. Open LM Studio via Launchpad or Spotlight

### First run

1. Search for "Qwen 3.5 4B"
2. Click Download
3. Wait for download (shows progress)
4. Click Load — model loads into memory
5. Click Chat — chat window opens

### When to use which

| If you… | Use |
|----------|-----|
| Just want chat, like ChatGPT | LM Studio |
| Plan to connect models to programs | Ollama |
| Want both | Ollama (server) + Open WebUI (web chat on top) |

---

## 8. Whats next

| If you want | Go to |
|-------------|-------|
| **Run models via terminal** with full control | [running-models.md](running-models.md) |
| **Choose the right model** for coding, chat, RAG | [models.md](models.md) |
| **Fix slow performance** | [memory-and-context.md](memory-and-context.md) |
| **Customize models** (Modelfile, API) | [advanced-setup.md](advanced-setup.md) |
| **Something went wrong** | [troubleshooting.md](troubleshooting.md) |
| **Browse all models** | [catalog.md](catalog.md) |
| **Back** | [README.md](README.md) |

---

> **Tip:** Bookmark this page or save it in your browser — you will come back to it when setting up new models.

---

**In section:** [getting-started](getting-started.md) · [running-models](running-models.md) · [models](models.md) · [catalog](catalog.md) · [quantization](quantization.md) · [memory-and-context](memory-and-context.md) · [tools](tools.md) · [advanced-setup](advanced-setup.md) · [troubleshooting](troubleshooting.md) · [apple-silicon](benchmarks/apple-silicon.md)  
**Related sections:** [Zero Level](../basics/README.md) · [AI Agents](../agents/README.md) · [Use Cases](../use-cases/README.md)  
**Navigation:** [← Local Models](README.md) · [↑ Back to main](../README.md) · [🇷🇺 Русский](getting-started.ru.md)
