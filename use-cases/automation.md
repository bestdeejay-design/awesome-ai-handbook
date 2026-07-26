# <img src="https://lucide.dev/api/icons/cog" alt="" width="20" height="20" style="vertical-align:middle"> AI + Automation

> Connecting local models with business processes, scripts, and schedules.

<img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> **Need the basics?** [basics/](../basics/README.md) — AI terms, hardware, Ollama installation.

**🇷🇺 Russian version:** [automation.ru.md](automation.ru.md)

[← Use Cases](README.md) · [Multi-Agent Systems →](../agents/multi-agent.md)

---

## Contents

1. [Stacks](#1-stacks)
2. [Basic Script: Processing via Ollama](#2-basic-script-processing-via-ollama)
3. [cron + Ollama: Periodic Processing](#3-cron--ollama-periodic-processing)
4. [n8n + Ollama: Visual Automation](#4-n8n--ollama-visual-automation)
5. [Telegram Bot with Local Model](#5-telegram-bot-with-local-model)
6. [What's Next](#6-whats-next)

---

## 1. Stacks

| Component | Role | Installation |
|-----------|------|--------------|
| **Ollama** | Local model (API on :11434) | `brew install ollama` |
| **Python Script** | Logic, API calls, processing | Built into macOS |
| **cron** | Scheduled execution | Built into macOS/Linux |
| [**n8n**](https://github.com/n8n-io/n8n) | Self-hosted visual automation | `npx n8n` |
| [**Make**](https://www.make.com/) | No-code cloud automation | Sign up on website |

---

## 2. Basic Script: Processing via Ollama

The simplest way to automate — a Python script that calls Ollama via API.

```python
#!/usr/bin/env python3
"""ai_process.py — universal script for text processing via Ollama."""

import requests
import json
import sys

OLLAMA = "http://localhost:11434/api/chat"
MODEL = "qwen3.5:4b"

def ai_process(text: str, instruction: str = "Answer briefly") -> str:
    """Sends text to model and returns response."""
    
    response = requests.post(OLLAMA, json={
        "model": MODEL,
        "messages": [
            {"role": "system", "content": instruction},
            {"role": "user", "content": text}
        ],
        "stream": False
    })
    return response.json()["message"]["content"]

def ai_process_json(text: str, json_schema: dict) -> dict:
    """Sends text and gets structured JSON."""
    
    response = requests.post(OLLAMA, json={
        "model": MODEL,
        "messages": [{"role": "user", "content": text}],
        "format": json_schema,
        "stream": False
    })
    return json.loads(response.json()["message"]["content"])


# Usage examples
if __name__ == "__main__":
    # Simple call
    result = ai_process(
        "Write a brief summary: 'Company released new product version...'",
        "You are an assistant. Write brief summaries (2-3 sentences)."
    )
    print(f"📝 {result}")
    
    # JSON output
    schema = {
        "type": "object",
        "properties": {
            "summary": {"type": "string"},
            "sentiment": {"type": "string", "enum": ["positive", "negative", "neutral"]},
            "keywords": {"type": "array", "items": {"type": "string"}}
        }
    }
    data = ai_process_json(
        "Product is great, but interface is a bit confusing",
        schema
    )
    print(f"📊 {json.dumps(data, ensure_ascii=False, indent=2)}")
```

---

## 3. cron + Ollama: Periodic Processing

Run a script every day at 9 AM to process new data.

```bash
# Create processing script
cat > ~/scripts/daily_report.sh << 'EOF'
#!/bin/bash
cd ~/scripts
# Read incoming data
cat today_notes.txt | \
python3 -c "
import sys, requests
text = sys.stdin.read()
resp = requests.post('http://localhost:11434/api/chat', json={
    'model': 'qwen3.5:4b',
    'messages': [
        {'role': 'system', 'content': 'Create a brief report from notes'},
        {'role': 'user', 'content': text}
    ],
    'stream': False
})
print(resp.json()['message']['content'])
" > daily_report.md

echo "Report saved: daily_report.md"
EOF

# Make executable
chmod +x ~/scripts/daily_report.sh

# Add to crontab
echo "0 9 * * * \$HOME/scripts/daily_report.sh" | crontab -
```

**Important:** cron uses absolute paths. Make sure `ollama serve` is running.

---

## 4. n8n + Ollama: Visual Automation

n8n lets you build automations without code.

### Installation

```bash
npx n8n
```

### Ready Workflow: Email Summaries

```
[IMAP Email] → [HTTP Request (Ollama)] → [Slack/Telegram]
```

**HTTP Node Configuration for Ollama:**
```
Method: POST
URL: http://localhost:11434/api/chat
Body (JSON):

{
  "model": "qwen3.5:4b",
  "messages": [
    {"role": "system", "content": "Write a brief email summary (3-5 sentences)"},
    {"role": "user", "content": "={% raw %}{{ $json.emailBody }}{% endraw %}"
  ],
  "stream": false
}
```

### Workflow Examples

| Scenario | Trigger | Action |
|----------|---------|--------|
| Email summaries | New email (IMAP) | Ollama → Telegram |
| Ticket classification | New ticket (webhook) | Ollama → Jira |
| Document translation | New file in folder | Ollama → save |
| News monitoring | RSS feed | Ollama → database |

---

## 5. Telegram Bot with Local Model


```python
#!/usr/bin/env python3
"""telegram_ai_bot.py — Telegram bot on local model."""

import requests
import logging
from http.server import HTTPServer, BaseHTTPRequestHandler

OLLAMA = "http://localhost:11434/api/chat"
MODEL = "qwen3.5:4b"

class AIBotHandler(BaseHTTPRequestHandler):
    """Simple HTTP server that accepts requests and responds via AI."""
    
    def do_POST(self):
        content_length = int(self.headers['Content-Length'])
        body = self.rfile.read(content_length).decode()
        
        response = requests.post(OLLAMA, json={
            "model": MODEL,
            "messages": [
                {"role": "system", "content": "You are a helpful assistant. Answer briefly."},
                {"role": "user", "content": body}
            ],
            "stream": False
        })
        
        answer = response.json()["message"]["content"]
        
        self.send_response(200)
        self.send_header('Content-type', 'application/json')
        self.end_headers()
        self.wfile.write(f'{% raw %}{{"response": "{answer}"}}{% endraw %}'.encode())

if __name__ == "__main__":
    server = HTTPServer(("localhost", 8888), AIBotHandler)
    print("🤖 AI Bot running on http://localhost:8888")
    server.serve_forever()
```

For a full-featured Telegram bot, use `python-telegram-bot` + Ollama.

---

## 6. What's Next

| If You Want | Go To |
|-------------|-------|
| **Build a multi-agent system** for automation | [../agents/multi-agent.md](../agents/multi-agent.md) |
| **Write an executor agent** with tools | [../agents/tutorials/01-first-agent.md](../agents/tutorials/01-first-agent.md) |
| **Understand Ollama API** | [../agents/ollama-for-agents.md](../agents/ollama-for-agents.md) |
| **Back to use cases** | [README.md](README.md) |

---

**In section:** [coding](coding.md) · [rag](rag.md) · [automation](automation.md) · [writing](writing.md)  
**Related sections:** [Local Models](../local-models/README.md) · [AI Agents](../agents/README.md) · [Zero Level](../basics/README.md)  
**Navigation:** [← Use Cases](README.md) · [↑ Back to main](../README.md) · [🇷🇺 Русский](automation.ru.md)
