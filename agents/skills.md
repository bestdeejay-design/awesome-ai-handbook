# <img src="https://lucide.dev/api/icons/puzzle" alt="" width="20" height="20" style="vertical-align:middle"> Skills for AI Agents: What They Are, Where to Get Them, How to Create Them

> Skills (plugins, MCP servers) are modular extensions that give an agent new capabilities: file operations, web search, database access, image generation, and much more.
>
> Without skills, an agent is just a chatbot. With skills — it's a tool that solves real tasks.

[← Ready-made AI Agents](ready-made.md) · [AI Agent Architecture →](architecture.md) · [🇷🇺 Русский](skills.ru.md)

---

## Contents

1. [What Are Skills](#1-what-are-skills)
2. [Platform Comparison](#2-platform-comparison)
3. [MCP — The Universal Standard](#3-mcp--the-universal-standard)
4. [Where to Get Ready-made Skills](#4-where-to-get-ready-made-skills)
5. [How to Create Your Own Skill](#5-how-to-create-your-own-skill)
6. [Example: test-graphics — Image Generation via Third-party API](#6-example-test-graphics--image-generation-via-third-party-api)
7. [Skill Progression: From Simple to Complex](#7-skill-progression-from-simple-to-complex)
8. [Skills in This Handbook](#8-skills-in-this-handbook)
9. [What's Next](#9-whats-next)

---

## 1. What Are Skills

A skill is an additional capability you give to an agent. The agent decides when to use it based on the task.

**Example:** you ask the agent to "draw an icon for an app". The agent realizes it can't draw, finds a suitable image generation skill, calls it — and returns the result.

Different platforms name skills differently:

| Platform | Name | Format |
|----------|------|--------|
| OpenCode / Codex CLI | Skills | Markdown + instructions |
| Continue.dev | Context Providers + MCP | JSON + JS/TS |
| Cline | MCP Servers | JSON |
| Cursor | Rules | Markdown (.cursorrules) |
| Aider | Conventions + Instructions | Markdown |
| Claude Code | CLAUDE.md | Markdown |
| OpenHands | Microagents | Markdown |
| GitHub Copilot | Custom Instructions | Markdown |
| **Any MCP client** | **MCP Server** | **JS/Python/Go/…** |

Despite different names, the principle is the same: **you describe what the skill does, and the agent calls it when needed**.

---

## 2. Platform Comparison

### OpenCode / Codex CLI — Skills

**Format:** Markdown files with instructions + shell scripts.  
**Location:** In `.opencode/skills/` folder or in `opencode.json` config.  
**How it works:** The agent reads the skill description and decides whether to apply it.

#### Simplest Skill

```markdown
# test-graphics

Generate test images, photos, icons, placeholders for projects.
Uses Python + free APIs (loremflickr, placehold.co).

Command: python test-graphics.py --width 800 --height 600 --output icon.png
```

Skill is loaded via config:
```json
{
  "skills": ["test-graphics"]
}
```

### Continue.dev — Context Providers + MCP

**Format:** JSON config + context provider code.  
**Location:** `~/.continue/config.json` + `~/.continue/plugins/`.  
**How it works:** Providers load context (files, terminal, git), MCP servers add tools.

```json
{
  "contextProviders": [
    {"name": "file"},
    {"name": "terminal"}
  ],
  "experimental": {
    "mcpServers": [
      {"name": "playwright", "command": "npx", "args": ["@playwright/mcp"]}
    ]
  }
}
```

### Cline — MCP Servers

**Format:** JSON.  
**Location:** `~/.vscode/extensions/cline/` or in extension settings.  
**How it works:** MCP servers connect as external tools.

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/allowed/dir"]
    }
  }
}
```

### Cursor — Rules

**Format:** Markdown files.  
**Location:** `.cursorrules` (global) or `.cursor/rules/` (project-specific).  
**How it works:** The agent reads rules and follows them when generating code.

### Aider — Conventions

**Format:** Markdown + YAML.  
**Location:** `CONVENTIONS.md` (repo root) or `.aider.conf.yml`.  
**How it works:** Aider reads conventions and follows code style.

### Claude Code — CLAUDE.md

**Format:** Markdown.  
**Location:** `CLAUDE.md` in project root.  
**How it works:** Claude Code reads this file on startup and follows instructions.

### OpenHands — Microagents

**Format:** Markdown.  
**Location:** `.openhands/microagents/`.  
**How it works:** Microagent is a file with instructions for the agent: what it should know about the project, which commands to run.

### GitHub Copilot — Custom Instructions

**Format:** Markdown.  
**Location:** `.github/copilot-instructions.md`.  
**How it works:** Copilot reads instructions when generating code.

---

## 3. MCP — The Universal Standard

**Model Context Protocol (MCP)** — an open protocol from Anthropic that allows agents to connect any external tools through a unified interface.

**How it works:**
```
Agent (client) ←→ MCP Server ←→ External Service (DB, API, files, browser, …)
```

MCP is supported by:

| Platform | MCP Status |
|----------|------------|
| Claude Desktop | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Native |
| Claude Code | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Native |
| Cursor | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Native |
| Continue.dev | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Experimental |
| Cline | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Native |
| VS Code (GitHub Copilot) | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Via extension |
| OpenCode / Codex CLI | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Via MCP config |
| Windsurf | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> Native |

**Where to get MCP servers:**

| Resource | Servers | Link |
|----------|--------:|------|
| MCP Directory | 2,300+ | [mcp.directory](https://mcp.directory) |
| MCP Servers | 9,800+ | [mcpservers.org](https://mcpservers.org) |
| Awesome MCP Servers | 2,300+ | [github.com/punkpeye/awesome-mcp-servers](https://github.com/punkpeye/awesome-mcp-servers) |
| MCP Trove | 520+ | [mcptrove.com](https://mcptrove.com) |
| findarepo MCP | 256+ | [findarepo.com/skills/mcp/](https://findarepo.com/skills/mcp/) |

**MCP Server Categories:**
- <img src="https://lucide.dev/api/icons/archive" alt="" width="20" height="20" style="vertical-align:middle"> **Databases** — Postgres, SQLite, MySQL, Turso
- <img src="https://lucide.dev/api/icons/globe" alt="" width="20" height="20" style="vertical-align:middle"> **Search & Web** — DuckDuckGo, Brave, Jina, Context7
- <img src="https://lucide.dev/api/icons/folder-tree" alt="" width="20" height="20" style="vertical-align:middle"> **Dev Tools** — GitHub, Git, Docker, Kubernetes
- <img src="https://lucide.dev/api/icons/bot" alt="" width="20" height="20" style="vertical-align:middle"> **AI/ML** — Ollama, embeddings, RAG
- <img src="https://lucide.dev/api/icons/folder-tree" alt="" width="20" height="20" style="vertical-align:middle"> **Files & Storage** — Local filesystem, Google Drive, S3
- <img src="https://lucide.dev/api/icons/globe" alt="" width="20" height="20" style="vertical-align:middle"> **Browsers** — Playwright, Puppeteer
- <img src="https://lucide.dev/api/icons/message-circle" alt="" width="20" height="20" style="vertical-align:middle"> **Communications** — Slack, Telegram, Email
- <img src="https://lucide.dev/api/icons/bar-chart-3" alt="" width="20" height="20" style="vertical-align:middle"> **Analytics** — BigQuery, Prometheus, Datadog

---

## 4. Where to Get Ready-made Skills

### MCP Server Catalogs

Largest selection: [mcpservers.org](https://mcpservers.org) — 9,800+ servers.  
By category: [mcp.directory](https://mcp.directory) — 2,300+ servers.  
With ratings: [findarepo.com/skills/mcp/](https://findarepo.com/skills/mcp/) — 256+ best.

### Skill Repositories

| Platform | Where to Look | Format |
|----------|---------------|--------|
| OpenCode / Codex CLI | `.opencode/skills/` in projects | Markdown |
| Continue.dev | [continue.dev/plugins](https://continue.dev/plugins) | JSON |
| Aider | [aider.chat/docs/usage/conventions.html](https://aider.chat/docs/usage/conventions.html) | Markdown |
| Claude Code | `CLAUDE.md` in open-source projects | Markdown |
| Cursor | [cursor.directory](https://cursor.directory) — community rules | Markdown |

### Awesome Lists

- [awesome-mcp-servers](https://github.com/punkpeye/awesome-mcp-servers) — curated list
- [awesome-cursorrules](https://github.com/PatrickJS/awesome-cursorrules) — rules for Cursor
- [awesome-continue](https://github.com/continuedev/awesome-continue) — plugins for Continue

---

## 5. How to Create Your Own Skill

The process is the same for any platform:

1. **Define what the skill does** — one specific thing
2. **Write the instruction** — describe when the agent should call the skill
3. **Connect to the agent** — via config or file in the appropriate directory

---

## 6. Example: test-graphics — Image Generation via Third-party API

> **Our real use case:** the handbook needed to quickly fill demo pages with screenshots, icons, photos — without hiring a designer and without generating via neural network. Solution: a skill that calls free services `placehold.co` and `loremflickr.com` via HTTP.

### What It Does
- Generates placeholders (`placehold.co`) with specified dimensions and text
- Gets random photos (`loremflickr.com`) for realistic placeholders
- Creates icons of specified size
- Works **without API keys**, **without limits**, **instantly**

### Skill Structure

```
.opencode/skills/test-graphics/
├── SKILL.md          # Agent instructions (Markdown)
├── test-graphics.py  # Executable code (Python)
└── requirements.txt  # Dependencies (requests)
```

### Agent Instructions (`.opencode/skills/test-graphics/SKILL.md`)

```markdown
# test-graphics

Generate test images, photos, icons, placeholders for projects.
No quality claims — just fill data.
Uses Python + free APIs (loremflickr, placehold.co).

## When to Use
- Need a test image for a mockup
- Need a placeholder icon
- Need to fill a page with images for demonstration

## Command
python test-graphics.py --width <W> --height <H> --output <file> [--type icon|photo|placeholder]

## Examples
- python test-graphics.py --width 800 --height 600 --output hero.png --type photo
- python test-graphics.py --width 64 --height 64 --output icon.png --type icon

## Dependencies
- Python 3
- requests
```

### Skill Code (`test-graphics.py`)

```python
#!/usr/bin/env python3
"""Generate test images via free APIs."""
import argparse, requests

def generate_placeholder(width, height, text=""):
    url = f"https://placehold.co/{width}x{height}?text={text or f'{width}x{height}'}"
    return url

def generate_photo(width, height):
    return f"https://loremflickr.com/{width}/{height}"

if __name__ == "__main__":
    parser = argparse.ArgumentParser()
    parser.add_argument("--width", type=int, default=800)
    parser.add_argument("--height", type=int, default=600)
    parser.add_argument("--type", choices=["photo", "icon", "placeholder"], default="placeholder")
    parser.add_argument("--output", required=True)
    args = parser.parse_args()

    if args.type == "photo":
        url = generate_photo(args.width, args.height)
    else:
        url = generate_placeholder(args.width, args.height, args.type)

    img = requests.get(url).content
    with open(args.output, "wb") as f:
        f.write(img)
    print(f"✓ {args.output} ({args.width}x{args.height})")
```

### Dependencies (`requirements.txt`)

```
requests>=2.31.0
```

### Connecting in OpenCode

In `opencode.json`:
```json
{
  "skills": ["test-graphics"]
}
```

### How the Agent Uses It

You ask: *"generate a 64×64 icon for testing"*

Agent:
1. Sees `test-graphics` in available skills
2. Reads `SKILL.md` — understands parameters
3. Runs: `python test-graphics.py --width 64 --height 64 --output icon.png --type icon`
4. Returns ready file `icon.png`

### Key Pattern: Third-party Services as "Free Backend"

| Service | What It Provides | Limits |
|---------|------------------|--------|
| **placehold.co** | SVG/PNG placeholders with text, colors, formats | No limits, works over HTTPS |
| **loremflickr.com** | Random photos from Flickr by size | No limits, random images |
| **picsum.photos** | Alternative for photos | No limits |

> **Pattern:** when a task can be solved by an HTTP request to a public API — don't write your own generation, wrap the call in a skill. This works for: QR codes, barcodes, URL shortening, email validation, geocoding, currency rates, etc.

---

## 7. Skill Progression: From Simple to Complex

Skills can evolve through complexity levels:

### Level 1: HTTP API Wrapper (like test-graphics)
- Single public endpoint call
- No secrets
- Examples: QR codes, placeholders, random photos, validators

### Level 2: Local Logic + File Operations
- Work with files on disk
- Simple data processing
- Examples: format conversion, image resize (Pillow), CSV/JSON parsing

### Level 3: MCP Server (Model Context Protocol)
- Persistent process
- Two-way communication with agent
- Standard interface for all MCP clients
- Examples: file access, code search, database work, browser

### Level 4: Complex Pipeline with State
- Multi-step processes
- Caching, retries, rate limiting
- Own API or task queue
- Examples: RAG pipeline, code generation with tests, CI/CD integration

### Level 5: Skill Pack / Plugin Ecosystem
- Set of related skills for a domain
- Versioned (data-science, web-dev, devops)
- Marketplace, versioning, dependencies
- Examples: `.opencode/skills/data-science/`, `.opencode/skills/frontend/`

### Skill Maturity Checklist

| Criterion | Level 1 | Level 2 | Level 3 | Level 4 | Level 5 |
|-----------|---------|---------|---------|---------|---------|
| Needs API key | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/alert-triangle" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> |
| State between calls | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/alert-triangle" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> |
| Works offline | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/alert-triangle" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> |
| MCP standard | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> |
| Versioning | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/alert-triangle" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> |
| Dependencies on other skills | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/x" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/alert-triangle" alt="" width="20" height="20" style="vertical-align:middle"> | <img src="https://lucide.dev/api/icons/check" alt="" width="20" height="20" style="vertical-align:middle"> |

### Recommendation: Start with Level 1

1. Find a routine task you do manually
2. Is there a public API? → wrap in Level 1 skill
3. No API? → write local logic (Level 2)
4. Need context/memory/two-way dialog? → MCP (Level 3)

---

## 8. Skills in This Handbook

As Awesome AI Handbook evolves, we add skills for different platforms.  
Watch the folder:

```
awesome-ai-handbook/
└── .opencode/
    └── skills/
        └── test-graphics/    ← example above
```

**Planned:**
- `handbook-qa` — search handbook content
- `model-benchmark` — run benchmarks on local hardware
- `agent-scaffold` — generate agent code for chosen framework

---

## 9. What's Next

| If You Want To | Go To |
|----------------|-------|
| **Pick a ready-made agent** for your tasks | [Ready-made AI Agents](ready-made.md) |
| **Build your own agent** from scratch | [AI Agent Architecture](architecture.md) |
| **Connect a local model** to an agent | [Ollama for Agents](ollama-for-agents.md) |
| **Find an MCP server** for your task | [mcp.directory](https://mcp.directory) (external) |
| **Return to agents section** | [README.md](README.md) |

---

---

**In section:** [architecture](architecture.md) · [evaluation](evaluation.md) · [frameworks](frameworks.md) · [memory](memory.md) · [multi-agent](multi-agent.md) · [ollama-for-agents](ollama-for-agents.md) · [orchestrators](orchestrators.md) · [patterns](patterns.md) · [prompting](prompting.md) · [ready-made](ready-made.md) · [safety](safety.md) · [skills](skills.md)  
**Related sections:** [Zero Level](../basics/README.md) · [Local Models](../local-models/README.md) · [Use Cases](../use-cases/README.md) · [Resources](../resources/README.md)  
**Navigation:** [← AI Agents](README.md) · [↑ Back to main](../README.md) · [🇷🇺 Русский](skills.ru.md)