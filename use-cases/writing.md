# <img src="https://lucide.dev/api/icons/pen-tool" alt="" width="20" height="20" style="vertical-align:middle"> Writing and Content

> Text generation, copywriting, translation, and content pipelines with local models.

<img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> **New to AI?** [basics/](../basics/README.md) — what models are, how to choose and set them up.

**🇷🇺 Russian version:** [writing.ru.md](writing.ru.md)

---

[← Use cases](README.md) · [Reflection pattern →](../agents/patterns.md#3-reflection--self-correction)

---

1. [Recommended models](#1-recommended-models)
2. [Quantization for text](#2-quantization-for-text)
3. [General text generation](#3-general-text-generation)
4. [Reflection pattern](#4-reflection-pattern)
5. [Mass production](#5-mass-production)
6. [RAG for content](#6-rag-for-content)
7. [Whats next](#7-whats-next)

---

## 1. Recommended models

| Task | Model | Run | tok/s | Why |
|------|-------|-----|-------|-----|
| **Quick drafts** | Qwen 3.5 4B | `ollama run qwen3.5:4b` | 28–35 | Good quality, fast |
| **Quality texts** | Qwen 3.5 9B | `ollama run qwen3.5:9b` | 10–13 | Best balance on 16 GB |
| **Classic** | Llama 3.1 8B | `ollama run llama3.1:8b` | 14–18 | Stable style |
| **Analytics / reports** | Phi-4-mini | `ollama run phi4-mini` | 25–30 | Best for reasoning |

---

## 2. Quantization for text

For writing and analytics, **Q5_K_M** makes sense over Q4_K_M — if the model fits in RAM:

| Format | When to use | Difference |
|--------|-------------|------------|
| Q4_K_M | Default, drafts | Fast, 3.5% quality loss |
| Q5_K_M | Final texts, analytics | 2.5% loss, slightly more memory |
| Q6_K | Legal / medical wording | 1.6% loss, noticeably better |

```bash
# Example: run model with Q5_K_M
ollama pull qwen3.5:9b:q5_k_m
ollama run qwen3.5:9b:q5_k_m
```

More details — [local-models/quantization.md](../local-models/quantization.md).

---

## 3. General text generation

```python
import requests

def generate(model, prompt):
    r = requests.post("http://localhost:11434/api/generate", json={
        "model": model, "prompt": prompt, "stream": False
    })
    return r.json()["response"]

for task, model in [
    ("Write a tweet about AI", "qwen3.5:4b"),
    ("Write a technical article about RAG", "qwen3.5:9b"),
]:
    print(f"Using {model}...")
```

---

## 4. Reflection pattern

Generate → critique → improve. A single pass gives a "raw" result; reflection adds self-review:

```
Draft (Qwen 4B, fast) → Reviewer agent (Qwen 9B + Reflection) → Final
```

Here is a compact single-function implementation:

```python
def reflection_article(topic):
    r1 = requests.post("http://localhost:11434/api/chat", json={
        "model": "qwen3.5:9b",
        "messages": [{"role": "user", "content": f"Write an article about: {topic}"}]
    })
    draft = r1.json()["message"]["content"]

    r2 = requests.post("http://localhost:11434/api/chat", json={
        "model": "qwen3.5:4b",
        "messages": [
            {"role": "system", "content": "You are a strict editor. Find issues."},
            {"role": "user", "content": draft}
        ]
    })
    feedback = r2.json()["message"]["content"]

    r3 = requests.post("http://localhost:11434/api/chat", json={
        "model": "qwen3.5:9b",
        "messages": [
            {"role": "system", "content": "Improve based on the feedback."},
            {"role": "user", "content": f"Draft:\n{draft}\n\nFeedback:\n{feedback}\n\nImproved:"}
        ]
    })
    return r3.json()["message"]["content"]
```

Alternatively, a modular approach with separate functions:

```python
import requests

OLLAMA = "http://localhost:11434/api/chat"

def draft(text: str) -> str:
    """Quick draft."""
    r = requests.post(OLLAMA, json={
        "model": "qwen3.5:4b",
        "messages": [{"role": "user", "content": text}],
        "stream": False
    })
    return r.json()["message"]["content"]

def review(text: str) -> str:
    """Critical review."""
    r = requests.post(OLLAMA, json={
        "model": "qwen3.5:9b",
        "messages": [
            {"role": "system", "content": (
                "You are an editor. Find errors, inaccuracies, weak spots in the text. "
                "Check: facts, grammar, style, structure."
            )},
            {"role": "user", "content": text}
        ],
        "stream": False
    })
    return r.json()["message"]["content"]

def improve(text: str, critique: str) -> str:
    """Improve based on critique."""
    r = requests.post(OLLAMA, json={
        "model": "qwen3.5:9b",
        "messages": [
            {"role": "system", "content": "Improve the text based on the critique. Return only the final version."},
            {"role": "user", "content": f"Original:\n{text}\n\nCritique:\n{critique}\n\nImproved text:"}
        ],
        "stream": False
    })
    return r.json()["message"]["content"]

# Full pipeline
def write_with_reflection(topic: str) -> str:
    """Writes text with self-review."""
    print("  ✏️ Draft...")
    raw = draft(f"Write a short article on: {topic}")
    print("  🔍 Review...")
    critique = review(raw)
    print("  ✨ Final version...")
    final = improve(raw, critique)
    return final

# Example
article = write_with_reflection("Advantages of local AI models")
print(article)
```

**Result:** the text goes through three stages — generation → critique → improvement. Quality is noticeably higher than a single pass.

---

## 5. Mass production

Generate from CSV with configurable tone:

```python
#!/usr/bin/env python3
"""content_generator.py — mass text generation through Ollama."""

import requests
import csv
from pathlib import Path

OLLAMA = "http://localhost:11434/api/chat"

def generate(topic: str, tone: str = "neutral") -> str:
    """Generates text by topic with given tone."""
    
    tones = {
        "neutral": "Write an informative text.",
        "professional": "Write a business text. Use professional vocabulary.",
        "friendly": "Write a friendly, conversational text.",
        "persuasive": "Write a persuasive text with a call to action."
    }
    
    response = requests.post(OLLAMA, json={
        "model": "qwen3.5:4b",
        "messages": [
            {"role": "system", "content": tones.get(tone, tones["neutral"])},
            {"role": "user", "content": f"Topic: {topic}"}
        ],
        "stream": False
    })
    return response.json()["message"]["content"]


# Generate from CSV file
def generate_from_csv(csv_path: str, output_dir: str = "output"):
    """Generates texts for each CSV row."""
    
    Path(output_dir).mkdir(exist_ok=True)
    
    with open(csv_path) as f:
        reader = csv.DictReader(f)
        for row in reader:
            text = generate(row["topic"], row.get("tone", "neutral"))
            
            filename = row.get("filename", f"{row['topic'][:30]}.md")
            filepath = Path(output_dir) / filename
            filepath.write_text(text)
            print(f"  ✓ {filename}")


if __name__ == "__main__":
    # Example: mass generate descriptions
    topics = [
        {"topic": "What is RAG in the context of LLM", "tone": "professional"},
        {"topic": "How to choose a model for coding", "tone": "friendly"},
        {"topic": "Advantages of local AI over cloud", "tone": "persuasive"},
    ]
    
    for item in topics:
        text = generate(item["topic"], item["tone"])
        print(f"\n📄 {item['topic']}")
        print(text[:200] + "...")
```

Also keep the simpler loop approach:

```python
topics = ["What is RAG", "Local LLM setup", "AI agents explained"]
for topic in topics:
    article = reflection_article(topic)
    filename = topic.lower().replace(" ", "_") + ".md"
    with open(filename, "w") as f:
        f.write(f"# {topic}\n\n{article}")
```

---

## 6. RAG for content

For texts based on your own materials (documentation, knowledge base) — use RAG:

```
Topic → Search similar materials in database → Context + query → Text
```

More details — [rag.md](rag.md).

---

## 7. Whats next

| If you want | Go to |
|-------------|-------|
| **Use the Reflection agent** for content | [patterns.md](../agents/patterns.md#3-reflection--self-correction) |
| **Build a content team** of agents | [tutorials/02-agent-team.md](../agents/tutorials/02-agent-team.md) |
| **Choose models** for text | [local-models/catalog.md](../local-models/catalog.md#2-general-chatinstruction-models) |
| Automate content pipelines | [automation.md](automation.md) |
| Back to use cases | [README.md](README.md) |

---

**In section:** [coding](coding.md) · [rag](rag.md) · [automation](automation.md) · [writing](writing.md)  
**Related sections:** [Local Models](../local-models/README.md) · [AI Agents](../agents/README.md) · [Zero Level](../basics/README.md)  
**Navigation:** [← Use Cases](README.md) · [↑ Back to main](../README.md) · [🇷🇺 Русский](writing.ru.md)
