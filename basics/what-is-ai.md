# 🤖 What is AI, ML, and LLM?

> Explained in plain language, without jargon or formulas.

**[🇷🇺 Russian version: [what-is-ai.ru.md](what-is-ai.ru.md)]**

[← Level Zero](README.md) · [How Neural Networks Work →](how-models-work.md)

---

## Contents

1. [AI, ML, LLM — What's the Difference?](#1-ai-ml-llm--whats-the-difference)
2. [What is a Language Model (LLM)?](#2-what-is-a-language-model-llm)
3. [Where Does the Model Get Its Answers?](#3-where-does-the-model-get-its-answers)
4. [Parameters: What Do 7B, 14B, 70B Mean?](#4-parameters-what-do-7b-14b-70b-mean)
5. [Open-Source vs Proprietary Models](#5-open-source-vs-proprietary-models)
6. [What's Next](#6-whats-next)

---

## 1. AI, ML, LLM — What's the Difference?

These three acronyms are often confused. Let's break them down.

```
┌──────────────────────────────────────┐
│  AI (Artificial Intelligence)        │
│  Artificial Intelligence — general   │
│  concept: a machine that "thinks"    │
│                                      │
│  ┌──────────────────────────────┐    │
│  │  ML (Machine Learning)       │    │
│  │  Machine Learning — AI that  │    │
│  │  learns from data            │    │
│  │                              │    │
│  │  ┌──────────────────────┐    │    │
│  │  │  LLM (Large Language │    │    │
│  │  │  Model)              │    │    │
│  │  │  Large Language      │    │    │
│  │  │  Model — what you    │    │    │
│  │  │  chat with           │    │    │
│  │  └──────────────────────┘    │    │
│  └──────────────────────────────┘    │
└──────────────────────────────────────┘
```

**AI** — the broadest concept. Includes chess programs, face recognition on your phone, and voice assistants.

**ML** — a way to create AI: instead of manually programming rules, you "feed" the program examples and let it find patterns itself.

**LLM** — a specific type of ML that works with text. ChatGPT, Claude, Qwen, DeepSeek — these are all LLMs.

**For this handbook:** we talk almost exclusively about LLMs — language models you can run on your own computer.

---

## 2. What is a Language Model (LLM)?

Imagine a **"smart autocomplete"** that works not with words, but with entire texts.

You start a phrase — the model continues. You ask a question — the model "completes" the answer. Everything it does is predict the next word (technically, token) based on previous ones.

> **Analogy:** An LLM is a person who has read almost the entire internet and can now finish your thought. Not because they "understand," but because they've seen similar texts millions of times.

**Important:** the model doesn't "think" or "understand" in the human sense. It's an extremely complex probability calculator: sequence of words in → most probable continuation out. But due to complexity (billions of parameters), the result looks like the model actually understands.

---

## 3. Where Does the Model Get Its Answers?

The model **does not have internet access** when answering. All its "knowledge" is what it memorized during training.

The process looks like this:

### Stage 1: Training
The model is "fed" massive amounts of text — books, articles, websites, code. Trillions of words. It learns to predict the next word again and again until it gets good enough at it.

> **Analogy:** imagine you've never seen chess. You're shown a million games, and you start guessing what move is usually made in a given position. You don't know the rules — you've just "played out" the statistics.

Training a large model takes **months** and costs **millions of dollars** (electricity, server rental). This is done by big companies: Meta, Google, Alibaba, DeepSeek.

### Stage 2: Inference
This is what you do when typing a question in chat. The model is **already trained** — it just applies its knowledge. This is fast and **free** (only electricity).

> Training → model became smart. Inference → it answers questions.

### Stage 3: Fine-tuning
If you need the model to understand your narrow domain — you can take a ready model and "fine-tune" it on your data.

> **Analogy:** the model graduated regular school. You send it to professional development courses in your specialty. This is much faster and cheaper than teaching from scratch.

---

## 4. Parameters: What Do 7B, 14B, 70B Mean?

When looking at a model, you see: `Qwen 3.5 7B`, `Llama 3.1 70B`. The number with B — number of **parameters** (B = billion).

**Parameters** are the model's "neurons". More parameters = potentially smarter model, but also more resources needed.

| Parameters | Analogy | Where to Run | Examples |
|-----------|---------|--------------|----------|
| **1–3B** | 🐭 mouse brain | Any laptop, 8 GB RAM | Phi-3-mini, Qwen 2.5 1.5B |
| **7–9B** | 🐕 sweet spot | MacBook / PC, 16 GB RAM | Qwen 3.5 7B, Llama 3.1 8B |
| **14–30B** | 🐒 needs hardware | 32 GB RAM or GPU | Qwen 3.5 14B, DeepSeek-R1 14B |
| **70B+** | 🧠 human | Server, 64 GB+ RAM | Llama 3 70B, DeepSeek-R1 671B |

> **Golden rule:** more parameters = smarter model, but proportionally more memory needed. On MacBook Air 16 GB — your ceiling is 7–9B. On MacBook Pro 48 GB — you can run 30B.

Important nuance: 70B model is **not 10x smarter** than 7B. It's 20–30% smarter but needs 10x more resources. For most everyday tasks, 7–9B models are more than enough.

---

## 5. Open-Source vs Proprietary Models

| | Proprietary (Closed) | Open-Source (Open) |
|---|---|---|
| **Examples** | GPT-4, Claude, Gemini | Llama 3, Qwen, Mistral, DeepSeek |
| **Who Creates** | OpenAI, Anthropic, Google | Meta, Alibaba, Mistral, community |
| **Where It Runs** | Only on company servers | Can download and run locally |
| **Cost** | $10–200/mo or per token | Free |
| **Can Customize** | No | Yes (fine-tune, modify) |
| **Privacy** | Your data goes to server | Everything stays with you |

**For local running** we only care about open-source models. Their weights are open — you can download the model file and run it anywhere.

Leading open-source models as of July 2026:
- **Qwen 3.5** (Alibaba) — best for Russian language
- **Llama 3.1 / 4** (Meta) — benchmark, good English
- **DeepSeek-R1 / V3** — excellent reasoning
- **Mistral** — efficient models
- **Gemma 4** (Google) — gaining popularity
- **Phi-4** (Microsoft) — small and smart

---

## 6. What's Next

| If You Want To | Go To |
|----------------|-------|
| **Understand how neural networks work** (by analogy) | [how-models-work.md](how-models-work.md) |
| **Choose: cloud or local** | [cloud-vs-local.md](cloud-vs-local.md) |
| **Check what hardware you need** | [hardware-guide.md](hardware-guide.md) |
| **Skip theory and install now** | [`../local-models/getting-started.md`](../local-models/getting-started.md) |
| **Back to navigation** | [README.md](README.md) |

---

**In section:** [what-is-ai](what-is-ai.md) · [how-models-work](how-models-work.md) · [cloud-vs-local](cloud-vs-local.md) · [hardware-guide](hardware-guide.md) · [glossary](glossary.md) · [faq](faq.md) · [learning-path](learning-path.md) · [setup-windows](setup-windows.md) · [setup-linux](setup-linux.md)  
**Related sections:** [Local Models](../local-models/README.md) · [AI Agents](../agents/README.md) · [Use Cases](../use-cases/README.md)  
**Navigation:** [← Zero Level](README.md) · [↑ Back to main](../README.md) · [🇷🇺 Русский](what-is-ai.ru.md)