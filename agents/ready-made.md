# <img src="https://lucide.dev/api/icons/package" alt="" width="20" height="20" style="vertical-align:middle"> Ready-Made AI Agents: Catalog & Comparison

> Not every agent needs to be built from scratch. Dozens of ready-made tools already edit code, execute tasks autonomously, and integrate with IDEs.  
> This section is a guide to existing AI agents: how they differ, which to choose, and how to connect them to local models.

[← AI agents](README.md) · [Skills for agents →](skills.md) · [🇷🇺 Русский](ready-made.ru.md)

---

## Contents

1. [How to Read This Table](#1-how-to-read-this-table)
2. [Comparison Table](#2-comparison-table)
3. [CLI Agents](#3-cli-agents)
4. [IDE Plugins](#4-ide-plugins)
5. [AI-native IDEs](#5-ai-native-ides)
6. [Autonomous Agents](#6-autonomous-agents)
7. [Orchestrator Agents](#7-orchestrator-agents)
8. [Which Agent to Choose](#8-which-agent-to-choose)
9. [What's Next](#9-whats-next)

---

## 1. How to Read This Table

All agents are divided into **5 categories** by usage style:

| Category | Examples | For Whom |
|----------|----------|----------|
| **CLI Agents** | Aider, Codex CLI, Claude Code | Those who work in the terminal |
| **IDE Plugins** | Continue, Cline, Cody | Those who work in VS Code / JetBrains |
| **AI-native IDEs** | Cursor, Windsurf | Those who want an IDE with AI "out of the box" |
| **Autonomous Agents** | OpenHands, AutoCodeRover | Those who want "set a task — get a result" |
| **Orchestrator Agents** | Sisyphus, CrewAI, AutoGen, LangGraph, Dify | Those building multi-agent teams "out of the box" |

**Local Model Integration Levels:**
- <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> **Full** — agent works with any model via Ollama / OpenAI-compatible API
- <img src="assets/icons/level-yellow.svg" alt="" width="12" height="12" style="vertical-align:middle"> **Limited** — local models connect, but not all features available
- <img src="assets/icons/level-red.svg" alt="" width="12" height="12" style="vertical-align:middle"> **Cloud Only** — agent requires a proprietary model

---

## 2. Comparison Table

| Agent | Type | Local | Install | Stars <img src="https://lucide.dev/api/icons/star" alt="" width="20" height="20" style="vertical-align:middle"> |
|-------|------|-------|---------|:-------:|
| [**Aider**](https://github.com/paul-gauthier/aider) | CLI <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> | <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> Full | `pip install aider-install` | 25K+ |
| [**Codex CLI**](https://github.com/openai/codex) | CLI <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> | <img src="assets/icons/level-red.svg" alt="" width="12" height="12" style="vertical-align:middle"> Cloud | `npm install -g @openai/codex` | 15K+ |
| [**Claude Code**](https://github.com/anthropics/claude-code) | CLI <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> | <img src="assets/icons/level-red.svg" alt="" width="12" height="12" style="vertical-align:middle"> Cloud | `npm install -g @anthropic-ai/claude-code` | 10K+ |
| [**Codebuff**](https://github.com/richardschneider/codebuff) | CLI <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> | <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> Full | `brew install codebuff` | 1K+ |
| [**Continue**](https://github.com/continuedev/continue) | IDE <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> | <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> Full | Install from VS Code Marketplace | 25K+ |
| [**Cline**](https://github.com/cline/cline) | IDE <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> | <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> Full | Install from VS Code Marketplace | 15K+ |
| [**Cody**](https://github.com/sourcegraph/cody) | IDE <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> | <img src="assets/icons/level-yellow.svg" alt="" width="12" height="12" style="vertical-align:middle"> Limited | Install from VS Code Marketplace | 5K+ |
| [**Cursor**](https://cursor.sh/) | IDE <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> | <img src="assets/icons/level-yellow.svg" alt="" width="12" height="12" style="vertical-align:middle"> Limited | Download from website | — |
| [**Windsurf**](https://codeium.com/windsurf) | IDE <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> | <img src="assets/icons/level-yellow.svg" alt="" width="12" height="12" style="vertical-align:middle"> Limited | Download from website | — |
| [**OpenHands**](https://github.com/All-Hands-AI/OpenHands) | Autonomous <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> | <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> Full | `docker pull` | 40K+ |
| [**AutoCodeRover**](https://github.com/nus-apr/auto-code-rover) | Autonomous <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> | <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> Full | `pip install auto-code-rover` | 3K+ |
| [**Sisyphus / OhMyOpenAgent**](https://github.com/code-yeongyu/oh-my-openagent) | Orchestrator <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> | <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> Full | `bunx oh-my-openagent install` | 66K+ |
| [**Copilot**](https://github.com/features/copilot) | IDE <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> | <img src="assets/icons/level-red.svg" alt="" width="12" height="12" style="vertical-align:middle"> Cloud | Install from Marketplace | — |

---

## 3. CLI Agents

Work in the terminal, edit files, work with git. Perfect for those who don't want to leave the console.

### Aider <img src="https://lucide.dev/api/icons/medal" alt="" width="20" height="20" style="vertical-align:middle">

[GitHub →](https://github.com/paul-gauthier/aider) · <img src="https://lucide.dev/api/icons/star" alt="" width="20" height="20" style="vertical-align:middle"> 25K+ · <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> Full local model support

**What it does:** Edits code in a git repository via text commands. Makes edits itself, runs tests, creates commits.

**Key feature:** Works with a pair of models — one architect (plans), one editor (writes code). Supports "lazy" mode for cheap models.

```bash
pip install aider-install
aider-install

# Run with local model
aider --model ollama/qwen2.5-coder:7b

# Architect mode (2 models)
aider --model ollama/qwen2.5-coder:7b --editor-model ollama/qwen3:4b
```

**When to choose:** Daily terminal development. Refactoring. Multi-file changes.

### Codex CLI

[GitHub →](https://github.com/openai/codex) · <img src="https://lucide.dev/api/icons/star" alt="" width="20" height="20" style="vertical-align:middle"> 15K+ · <img src="assets/icons/level-red.svg" alt="" width="12" height="12" style="vertical-align:middle"> Cloud only (OpenAI)

**What it does:** Official CLI agent from OpenAI. Runs code in sandbox environment, edits files.

**Key feature:** Native OpenAI API integration, sandbox isolation, extension support.

```bash
npm install -g @openai/codex
codex "add sorting to task list"
```

### Claude Code

[GitHub →](https://github.com/anthropics/claude-code) · <img src="https://lucide.dev/api/icons/star" alt="" width="20" height="20" style="vertical-align:middle"> 10K+ · <img src="assets/icons/level-red.svg" alt="" width="12" height="12" style="vertical-align:middle"> Cloud only (Anthropic)

**What it does:** CLI agent from Anthropic. Works with Claude.

**Key feature:** Strong safety, CLAUDE.md instruction support, large context handling.

### Codebuff

[GitHub →](https://github.com/richardschneider/codebuff) · <img src="https://lucide.dev/api/icons/star" alt="" width="20" height="20" style="vertical-align:middle"> 1K+ · <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> Full local model support

**What it does:** Lightweight CLI agent for file operations.

**Key feature:** Simplicity. One command — one change. No sandboxes.

---

## 4. IDE Plugins

Extensions for VS Code, JetBrains, and other editors. Add AI features directly to the interface.

### Continue <img src="https://lucide.dev/api/icons/medal" alt="" width="20" height="20" style="vertical-align:middle">

[GitHub →](https://github.com/continuedev/continue) · <img src="https://lucide.dev/api/icons/star" alt="" width="20" height="20" style="vertical-align:middle"> 25K+ · <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> Full local model support

**What it does:** Free Copilot with local model. Tab autocomplete, chat, inline editing, @codebase.

**Key feature:** Only IDE plugin with full Ollama + MCP server + custom rules support. Configured via JSON.

```json
{
  "models": [{
    "title": "Local Coder",
    "provider": "ollama",
    "model": "qwen2.5-coder:7b",
    "apiBase": "http://localhost:11434"
  }],
  "tabAutocompleteModel": {
    "provider": "ollama",
    "model": "qwen2.5-coder:1.5b"
  }
}
```

**When to choose:** If you're in VS Code and want a free Copilot alternative with local models.

### Cline

[GitHub →](https://github.com/cline/cline) · <img src="https://lucide.dev/api/icons/star" alt="" width="20" height="20" style="vertical-align:middle"> 15K+ · <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> Full local model support

**What it does:** Autonomous agent inside VS Code. Reads files, writes code, runs commands itself.

**Key feature:** Can create and execute plans autonomously. Supports MCP servers.

**When to choose:** If you need an autonomous agent inside the editor, not just autocomplete.

### Cody

[GitHub →](https://github.com/sourcegraph/cody) · <img src="https://lucide.dev/api/icons/star" alt="" width="20" height="20" style="vertical-align:middle"> 5K+ · <img src="assets/icons/level-yellow.svg" alt="" width="12" height="12" style="vertical-align:middle"> Limited local model support

**What it does:** AI assistant from Sourcegraph. Chat, autocomplete, commands.

**Key feature:** Sourcegraph integration for code search. Strong contextual awareness.

---

## 5. AI-native IDEs

Full-featured IDEs with AI built into every corner of the interface.

### Cursor

[Website →](https://cursor.sh/) · <img src="assets/icons/level-yellow.svg" alt="" width="12" height="12" style="vertical-align:middle"> Limited local model support

**What it does:** VS Code fork with built-in AI at every level. Tab, chat, agent, terminal.

**Key feature:** Most polished AI experience. Agent mode plans and executes multi-step tasks autonomously.

> **With local models:** Cursor can connect custom models via OpenAI-compatible API. Not all features work — some are tied to Cursor's proprietary models.

### Windsurf

[Website →](https://codeium.com/windsurf) · <img src="assets/icons/level-yellow.svg" alt="" width="12" height="12" style="vertical-align:middle"> Limited local model support

**What it does:** IDE from Codeium. Cascade — mode where AI sees the entire project.

**Key feature:** "Cascade" — agent that understands the whole codebase and can execute tasks autonomously.

---

## 6. Autonomous Agents

Run in isolated environments and execute tasks from start to finish without human involvement.

### OpenHands <img src="https://lucide.dev/api/icons/medal" alt="" width="20" height="20" style="vertical-align:middle">

[GitHub →](https://github.com/All-Hands-AI/OpenHands) · <img src="https://lucide.dev/api/icons/star" alt="" width="20" height="20" style="vertical-align:middle"> 40K+ · <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> Full local model support

**What it does:** Autonomous software engineer in a Docker container. Creates plan → writes code → runs → fixes bugs → makes PR.

**Key feature:** Full isolation (Docker), web browser support, local model support.

```bash
docker run -it --rm \
  -e LLM_MODEL=qwen2.5-coder:7b \
  -e LLM_API_KEY=ollama \
  -e LLM_BASE_URL=http://host.docker.internal:11434 \
  -v $(pwd)/workspace:/workspace \
  ghcr.io/all-hands-ai/openhands:latest
```

**When to choose:** If you need "set it and forget it". Automatic task execution, bug fixing, PR creation.

### AutoCodeRover

[GitHub →](https://github.com/nus-apr/auto-code-rover) · <img src="https://lucide.dev/api/icons/star" alt="" width="20" height="20" style="vertical-align:middle"> 3K+ · <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> Full local model support

**What it does:** Autonomous agent for bug fixing and feature implementation.

**Key feature:** Focused on code repair. Can explore code, localize bugs, propose fixes.

---

## 7. Orchestrator Agents

Manage teams of specialized subagents — don't write code themselves, but coordinate those who do.

### Sisyphus (OhMyOpenAgent)

[GitHub →](https://github.com/code-yeongyu/oh-my-openagent) · <img src="https://lucide.dev/api/icons/star" alt="" width="20" height="20" style="vertical-align:middle"> 66K+ · <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> Full local model support

**What it does:** Plugin for OpenCode / Codex CLI adding multi-agent architecture. **11 agents**, 54+ lifecycle hooks, 5 built-in MCPs, Team Mode. Exists in two editions:

| Edition | For | Install | Agents |
|---------|-----|---------|--------|
| **Ultimate** | OpenCode | `bunx oh-my-openagent install` | All 11 |
| **Light** | Codex CLI | `npx lazycodex-ai install` | 8 components (no orchestration) |

**Key feature:** Only system where each agent uses its own model, optimal for its task, while the orchestrator (Atlas) manages their work like a conductor.

#### Agent Team

| Agent | Role | Default Model |
|-------|------|---------------|
| **Sisyphus** (me) | Top-level orchestrator | `claude-opus-4-8` / `kimi-k3` |
| **Hephaestus** | Deep executor | `gpt-5.6-sol` |
| **Prometheus** | Strategic planner | `claude-opus-4-8` / `gpt-5.6-sol` |
| **Atlas** | Plan execution conductor | `claude-sonnet-4-6` / `kimi-k3` |
| **Metis** | Requirements analysis, hidden intent discovery | `claude-sonnet-4-6` |
| **Momus** | QA, plan review | `gpt-5.6-terra` |
| **Oracle** | Architecture, debugging | `gpt-5.6-sol` |
| **Sisyphus-Junior** | Task executor | `claude-sonnet-4-6` / `kimi-k3` |
| **Explore** | Code search (grep) | `gpt-5.4-mini-fast` |
| **Librarian** | Documentation, OSS | `gpt-5.4-mini-fast` |
| **Multimodal-Looker** | Image, PDF analysis | — |

**How it works (real architecture from repo):**

```
User
    │
    ▼
┌─────────────────────────────────────────────────────┐
│  Planning Layer (Prometheus + Metis + Momus + Oracle)│
│  ┌──────────┐   ┌──────────┐   ┌──────┐  ┌──────┐  │
│  │Prometheus├──►│  Metis   │   │Momus │  │Oracle│  │
│  │(plans)   │   │(analyzes)│   │(review)│  │(review)│  │
│  └────┬─────┘   └──────────┘   └──────┘  └──────┘  │
│       │          ▲               ▲         ▲        │
│       └──────────┴───────┬───────┘         │        │
│                          │                 │        │
│                     ┌────┴─────┐            │        │
│                     │ .omo/plans/*.md       │        │
│                     └──────────┘            │        │
└─────────────────────────────────────────────┼────────┘
                                              │
    /start-work                               │
    ▼                                         │
┌─────────────────────────────────────────────┼────────┐
│  Execution Layer (Atlas)                    │        │
│  ┌────────────────────────────────────────┐ │        │
│  │  Atlas (conductor) reads plan          │ │        │
│  │  and delegates tasks:                  │ │        │
│  │                                        │ │        │
│  │  task(category="deep") ───► Junior     │ │        │
│  │  task(subagent_type="oracle") ──► Oracle │        │
│  │  task(category="visual-engineering")   │ │        │
│  │      ───► Frontend                     │ │        │
│  │  explore ───► Explore agent            │ │        │
│  └────────────────────────────────────────┘ │        │
└─────────────────────────────────────────────┼────────┘
                                              │
    Result ◄──────────────────────────────────┘
```

**When to choose:** If you want a ready-made multi-agent team in your editor — 11 agents with different roles and models, no manual setup needed.

> **This handbook was written by this same team.**

#### How to Install

```bash
# Ultimate Edition (OpenCode)
bunx oh-my-openagent install

# Light Edition (Codex CLI)
npx lazycodex-ai install

# Both editions at once
bunx oh-my-openagent install --platform=both
```

**Tip:** Open the installation guide via an AI agent:
```
Install and configure oh-my-openagent by following the instructions here:
https://raw.githubusercontent.com/code-yeongyu/oh-my-openagent/refs/heads/dev/docs/guide/installation.md
```

More details — in the [separate orchestrators guide](orchestrators.md).

---

## 8. Which Agent to Choose

| Your Scenario | Recommendation |
|--------------|----------------|
| **"Want autocomplete in VS Code + chat with local model"** | [Continue](#continue-) |
| **"Refactoring code in terminal, need git integration"** | [Aider](#aider-) |
| **"Want an autonomous agent that does everything itself"** | [OpenHands](#openhands-) |
| **"Need an agent inside editor that writes code itself"** | [Cline](#cline) or [Cursor](#cursor) |
| **"Want IDE with built-in AI, no setup"** | [Cursor](#cursor) or [Windsurf](#windsurf) |
| **"Want a multi-agent team 'out of the box' in editor"** | [OhMyOpenAgent (Sisyphus)](orchestrators.md) — detailed guide |
| **"Trying for the first time, just want to look"** | [Continue](#continue-) — free and easy |
| **"Have a custom local model, want full control"** | [Aider](#aider-) or [Continue](#continue-) |
| **"Making a PR from an issue task"** | [OpenHands](#openhands-) or [AutoCodeRover](#autocoderover) |

---

## 9. What's Next

| If you want to | Go to |
|---------------|-------|
| **Understand how orchestrators with subagents work** (Sisyphus, Atlas, Prometheus) | [Orchestrators with Subagents →](orchestrators.md) |
| **Understand how to extend agent capabilities** (skills, MCP, plugins) | [Skills for Agents →](skills.md) |
| **Build your own agent from scratch** | [AI Agent Architecture](architecture.md) |
| **Choose a framework** for a custom agent | [Frameworks](frameworks.md) |
| **Set up a local model** for an agent | [Ollama for Agents](ollama-for-agents.md) |
| **Return to the agents section** | [README.md](README.md) |
---

---

**In section:** [architecture](architecture.md) · [evaluation](evaluation.md) · [frameworks](frameworks.md) · [memory](memory.md) · [multi-agent](multi-agent.md) · [ollama-for-agents](ollama-for-agents.md) · [orchestrators](orchestrators.md) · [patterns](patterns.md) · [prompting](prompting.md) · [ready-made](ready-made.md) · [safety](safety.md) · [skills](skills.md)  
**Related sections:** [Zero Level](../basics/README.md) · [Local Models](../local-models/README.md) · [Use Cases](../use-cases/README.md) · [Resources](../resources/README.md)  
**Navigation:** [← AI Agents](README.md) · [↑ Back to main](../README.md) · [🇷🇺 Русский](ready-made.ru.md)