# <img src="https://lucide.dev/api/icons/pen-tool" alt="" width="20" height="20" style="vertical-align:middle"> Writing and Content

> Text generation, copywriting, and translation with local models.

**🇷🇺 Russian version:** [writing.ru.md](writing.ru.md)

---

[← Use cases](README.md)

---

1. [General text generation](#1-general-text-generation)
2. [Reflection pattern](#2-reflection-pattern)
3. [Mass production](#3-mass-production)
4. [Whats next](#4-whats-next)

---

## 1. General text generation

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

## 2. Reflection pattern

Generate → critique → improve:

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

---

## 3. Mass production

```python
topics = ["What is RAG", "Local LLM setup", "AI agents explained"]
for topic in topics:
    article = reflection_article(topic)
    filename = topic.lower().replace(" ", "_") + ".md"
    with open(filename, "w") as f:
        f.write(f"# {topic}\n\n{article}")
```

---

## 4. Whats next

| If you want | Go to |
|-------------|-------|
| Automate content pipelines | [automation.md](automation.md) |
| Reflection agent pattern | [patterns.md](../agents/patterns.md) |
| Back | [README.md](README.md) |

---

**In section:** [coding](coding.md) · [rag](rag.md) · [automation](automation.md) · [writing](writing.md)  
**Related sections:** [Local Models](../local-models/README.md) · [AI Agents](../agents/README.md) · [Zero Level](../basics/README.md)  
**Navigation:** [← Use Cases](README.md) · [↑ Back to main](../README.md) · [🇷🇺 Русский](writing.ru.md)
