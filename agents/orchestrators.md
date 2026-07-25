# 🎼 Sub-Agent Orchestrators: Catalog & Installation

> A sub-agent orchestrator is a system where one "lead" agent manages a team of specialized agents, each with its own model and role.  
> This is not a framework (LangGraph, CrewAI), but a **ready-to-use configuration** that works "out of the box".

[← Ready-made AI Agents](ready-made.md) · [AI Agent Architecture →](architecture.md) · [🇷🇺 Русский](orchestrators.ru.md)

---

## Contents

1. [What Is a Sub-Agent Orchestrator](#1-what-is-a-sub-agent-orchestrator)
2. [OhMyOpenAgent (Sisyphus)](#2-ohmyopenagent-sisyphus)
3. [Other Systems Approaching This](#3-other-systems-approaching-this)
4. [Comparison Table](#4-comparison-table)
5. [Installing OhMyOpenAgent](#5-installing-ohmyopenagent)
6. [What's Next](#6-whats-next)

---

## 1. What Is a Sub-Agent Orchestrator

**Regular AI agent:** one model → one task → one answer.

**Sub-agent orchestrator:** one task → planner → multiple specialized agents with different models → result synthesis.

```
Task
  │
  ▼
┌──────────────┐
│  Orchestrator │  ← decides who does what
└──────┬───────┘
       │
   ┌───┼───┬───┬───┐
   ▼   ▼   ▼   ▼   ▼
  A1  A2  A3  A4  A5   ← different agents, different models
  │   │   │   │   │
  └───┴───┴───┴───┘
       │
       ▼
    Result
```

**Key principle:** each agent uses the model optimal for its role. Cheap models — for grep and search, expensive — for architecture and planning.

As of writing (July 2026), the **only** system with this architecture "out of the box" is OhMyOpenAgent.

---

## 2. OhMyOpenAgent (Sisyphus)

[GitHub →](https://github.com/code-yeongyu/oh-my-openagent) · ⭐ 66K+ · TypeScript

**Author:** [@code-yeongyu](https://github.com/code-yeongyu)  
**Company:** [Sisyphus Labs](https://sisyphuslabs.ai)  
**License:** SUL-1.0  
**Releases:** 222+ · **Contributors:** 300

### Architecture (11 Agents)

OhMyOpenAgent contains **11 built-in agents** in three layers:

#### Planning Layer
| Agent | Role | Models |
|-------|------|--------|
| **Prometheus** | Strategic planner. Interview, research, plan creation | `claude-opus-4-8`, `gpt-5.6-sol`, `glm-5.2` |
| **Metis** | Requirements analysis. Finds hidden intents, ambiguities, AI-slop | `claude-sonnet-4-6`, `claude-opus-4-8` |
| **Momus** | Plan QA review. Checks completeness, feasibility, consistency | `gpt-5.6-terra`, `gpt-5.6-sol` |

#### Execution Layer
| Agent | Role | Models |
|-------|------|--------|
| **Atlas** 🏆 | Execution conductor. Reads plan, delegates tasks to sub-agents, accumulates knowledge | `claude-sonnet-4-6`, `kimi-k3` |
| **Sisyphus** | Main orchestrator. `ultrawork` — entry into full autonomous work mode | `claude-opus-4-8`, `kimi-k3` |

#### Worker Layer
| Agent | Role | Models |
|-------|------|--------|
| **Hephaestus** 🏆 | Deep executor. Receives goal → self-researches, plans, writes code | `gpt-5.6-sol` |
| **Sisyphus-Junior** | Focused executor. Receives detailed prompt → executes | `claude-sonnet-4-6`, `kimi-k3` |
| **Oracle** | Architecture and debugging. Read-only consultant | `gpt-5.6-sol`, `gemini-3.1-pro` |
| **Explore** | Code researcher. Contextual grep | `gpt-5.4-mini-fast` |
| **Librarian** | Documentation and OSS. External reference search | `gpt-5.4-mini-fast` |
| **Multimodal-Looker** | Image, PDF, media analysis | — |

### How It Works in Practice

**Simple path:** write `ulw` or `ultrawork` in chat → Sisyphus decides which agents are needed and executes the task.

**Precise path:** `@plan "do X"` → Prometheus interviews, creates plan in `.omo/plans/` → Momus + Oracle review → `/start-work` → Atlas reads plan and delegates tasks to sub-agents.

**Deep path:** switch to Hephaestus → Hephaestus self-researches, plans, and executes as an autonomous deep worker.

### Work Modes

| Mode | Command | What It Does |
|------|---------|--------------|
| `ultrawork` / `ulw` | `ulw do X` | Full autonomous work, doesn't stop until done |
| `@plan` | `@plan do X` | Prometheus plans, Atlas executes |
| `/start-work` | After plan | Atlas executes plan from `.omo/plans/` |
| Team Mode | `team_create(...)` | Up to 8 parallel team members |
| `hyperplan` | `hyperplan X` | 5 hostile critics plan |
| `search` | `search X` | Code and documentation search |
| `analyze` | `analyze X` | Code analysis |

### Two Editions

| | Ultimate (OpenCode) | Light (Codex CLI) |
|---|---|---|
| **Agents** | 11 agents with orchestration | 8 components, no orchestration |
| **Models** | Own model per agent | Codex CLI model |
| **Team Mode** | ✅ Up to 8 participants | ❌ |
| **Lifecycle hooks** | 54+ | ✅ (limited) |
| **MCP** | 5 built-in + custom | Plugin-scoped MCP |
| **Installation** | `bunx oh-my-openagent install` | `npx lazycodex-ai install` |
| **Requirements** | Bun + OpenCode | Node.js + Codex CLI |

### Name History

The project has been renamed several times:
1. **oh-my-opencode** — original name (npm package still named this)
2. **oh-my-openagent** — current name (since March 2026)
3. **lazycodex** — Codex CLI version
4. **omo** — internal abbreviation (don't confuse with npm package `omo`, that's a different project!)

---

## 3. Other Systems Approaching This

No full analogs of OhMyOpenAgent exist, but some systems have individual multi-agent elements.

### Claude Code (sub-agents)

[GitHub →](https://github.com/anthropics/claude-code) · ⭐ 139K+

**What it has:** Claude Code supports sub-agents — can spawn child agents for parallel task execution. Has `CLAUDE.md` for instructions, MCP servers.

**What it lacks:** No role separation (architect vs executor vs researcher). One model for everything. No planner separate from executor.

### Cline — Kanban mode

[GitHub →](https://github.com/cline/cline) · ⭐ 65K+

**What it has:** Kanban board (`npx kanban`) for parallel launch of multiple agents. Each agent works on its task independently. MCP server support.

**What it lacks:** No orchestrator to distribute tasks. No different models for different roles. No planner.

### Aider — Architect/Editor mode

[GitHub →](https://github.com/paul-gauthier/aider) · ⭐ 48K+

**What it has:** Two-model mode: one (architect) plans changes, second (editor) writes code. Ollama support.

**What it lacks:** Only two agents, no role specialization. No sub-agents for research/documentation.

### OpenHands — Agent Canvas

[GitHub →](https://github.com/All-Hands-AI/OpenHands) · ⭐ 82K+

**What it has:** Web UI for launching multiple agents. ACP protocol for coordination. Different model support.

**What it lacks:** Each agent is a copy of the same pipeline, no role separation. No planner.

---

## 4. Comparison Table

| System | Agents | Role Separation | Different Models | Planner | Editions | ⭐ |
|--------|:------:|:---------------:|:----------------:|:-------:|:--------:|:--:|
| **OhMyOpenAgent** | **11** | ✅ Yes | ✅ Yes | ✅ Prometheus | 2 (OpenCode/Codex CLI) | 66K |
| Claude Code | sub-agents | ❌ No | ❌ No | ❌ No | 1 | 139K |
| Cline | N (kanban) | ❌ No | ❌ No | ❌ No | 1 | 65K |
| Aider | 2 | ⚠️ Two roles | ⚠️ Two models | ❌ No | 1 | 48K |
| OpenHands | N (UI) | ❌ No | ✅ Different | ❌ No | 1 | 82K |
| CrewAI* | N (code) | ✅ Roles | ✅ Different | ⚠️ Via code | Framework | 25K |
| AutoGen* | N (code) | ✅ Roles | ✅ Different | ⚠️ Via code | Framework | 38K |

> *CrewAI and AutoGen are frameworks, not ready-to-use systems. They require writing code to configure the team.

---

## 5. Installing OhMyOpenAgent

### Method 1: Ultimate Edition (OpenCode) — Recommended

```bash
# Install via Bun
bunx oh-my-openagent install
```

Interactive TUI (Terminal UI) will guide you through:
1. Subscription selection (ChatGPT, Kimi Code, GLM Coding Plan)
2. Provider setup (Anthropic, Gemini, Copilot, Z.ai)
3. Model assignment to 11 agents
4. Team Mode configuration

**Recommended subscriptions:**
- ChatGPT ($20/mo) — for GPT-5.6 Sol
- Kimi Code ($19/mo) — for Kimi K3 (excellent Claude alternative)
- GLM Coding Plan ($10/mo) — for GLM-5

### Method 2: Light Edition (Codex CLI)

```bash
# Install via npm (Node.js, no Bun required)
npx lazycodex-ai install

# Non-TUI mode with autonomous permissions
npx lazycodex-ai install --no-tui --codex-autonomous
```

Installs 8 components to `~/.codex/plugins/cache/sisyphuslabs/omo/`.

### Method 3: Both Editions at Once

```bash
bunx oh-my-openagent install --platform=both
```

### Installation via AI Agent (Recommended by Authors)

Copy this prompt into Claude Code, Cursor, AmpCode, or any other agent:

```
Install and configure oh-my-openagent by following the instructions here:
https://raw.githubusercontent.com/code-yeongyu/oh-my-openagent/refs/heads/dev/docs/guide/installation.md
```

The agent will read the full guide, execute each step, and configure everything automatically.

### Important Notes

1. **Package name:** npm package is still called `oh-my-opencode` (dual-published as `oh-my-openagent`). Command `bunx oh-my-openagent install` works.
2. **Don't confuse `omo`:** package uses `omo` as bin alias, but DO NOT use `npx omo` — that's a different npm package.
3. **OpenCode vs Codex CLI:** Ultimate edition requires OpenCode. Light edition works with official Codex CLI from OpenAI.
4. **Telemetry:** anonymous telemetry enabled by default (1 request/day). Disable: `"telemetry": false` in config.

### After Installation

```bash
# Check status
bunx oh-my-openagent doctor

# Basic command — just write:
ulw <your task>

# Or precise path:
@plan <task description>   # Prometheus plans
/start-work               # Atlas executes
```

---

## 6. What's Next

| If You Want To | Go To |
|----------------|-------|
| **Compare with other agents** (Aider, Cline, OpenHands) | [Ready-made AI Agents](ready-made.md) |
| **Understand agent architecture** deeper | [AI Agent Architecture](architecture.md) |
| **Choose a framework** for your orchestrator | [Agent Frameworks](frameworks.md) |
| **Configure skills and MCP** | [Skills for Agents](skills.md) |
| **Configure local model** for agent | [Ollama for Agents](ollama-for-agents.md) |
| **Return to agents section** | [README.md](README.md) |

---

---

**In section:** [architecture](architecture.md) · [evaluation](evaluation.md) · [frameworks](frameworks.md) · [memory](memory.md) · [multi-agent](multi-agent.md) · [ollama-for-agents](ollama-for-agents.md) · [orchestrators](orchestrators.md) · [patterns](patterns.md) · [prompting](prompting.md) · [ready-made](ready-made.md) · [safety](safety.md) · [skills](skills.md)  
**Related sections:** [Zero Level](../basics/README.md) · [Local Models](../local-models/README.md) · [Use Cases](../use-cases/README.md) · [Resources](../resources/README.md)  
**Navigation:** [← AI Agents](README.md) · [↑ Back to main](../README.md) · [🇷🇺 Русский](orchestrators.ru.md)