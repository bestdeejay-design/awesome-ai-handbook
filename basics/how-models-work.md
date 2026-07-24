# 🧠 How Neural Networks Work (Explained Without Formulas)

> Inside LLM architecture — by analogy, without a single formula.

**[🇷🇺 Russian version: [how-models-work.ru.md](how-models-work.ru.md)]**

[← What is AI](what-is-ai.md) · [Cloud or Local →](cloud-vs-local.md)

---

## Contents

1. [Neural Network is a Prediction Pipeline](#1-neural-network-is-a-prediction-pipeline)
2. [Tokens: What the Model Builds Text From](#2-tokens-what-the-model-builds-text-from)
3. [Attention: How the Model Connects Words](#3-attention-how-the-model-connects-words)
4. [Transformer: The Architecture That Changed Everything](#4-transformer-the-architecture-that-changed-everything)
5. [Context Window: What the Model "Remembers"](#5-context-window-what-the-model-remembers)
6. [Temperature and Creativity](#6-temperature-and-creativity)
7. [Why Models Make Mistakes (Hallucinations)](#7-why-models-make-mistakes-hallucinations)
8. [What's Next](#8-whats-next)

---

## 1. Neural Network is a Prediction Pipeline

The most important thing to understand about modern language models:

> **LLM is a "smart autocomplete".**

It doesn't "think", doesn't "understand", doesn't have consciousness. It does one simple thing: **receives a sequence of words and predicts the next word**. Again and again, word by word.

```
Input: "The capital of France is..."  →  Model computes  →  "Paris"
Now input: "The capital of France is Paris. It is known..."  →  Model computes  →  "for"
And so on, until a full text emerges.
```

> **Analogy:** imagine autocomplete on your smartphone that predicts the next word. LLM is the same thing, but trained on an unimaginably larger volume of text and with billions of "neurons".

---

## 2. Tokens: What the Model Builds Text From

The model works **not with words**, but with **tokens**.

**Token** — a piece of text, usually 2-4 characters. A word can be one token, or can be split into several.

> **Analogy:** tokens are LEGO bricks. From them you can build anything: the word "house", a sentence, an entire book. The model operates with bricks, not ready-made words.

Example (how the model sees text):
```
"Hello, how are you?" → [Hel) (lo) (,) (how) ( are) ( you) (?)]
```

**Why this matters:**
- The model has a limit on tokens per request — this is the **context window**
- More tokens in request = longer processing time
- Russian requires more tokens per word than English (Russian words are longer)

**Real numbers:**
- 1 token ≈ 0.7 words for Russian (roughly 3-4 characters)
- 1000 tokens ≈ 700 Russian words (about a page of text)
- Model with 8K context ≈ 5000 words (several pages)
- Model with 128K context ≈ 90,000 words (a whole book)

---

## 3. Attention: How the Model Connects Words

The main mechanism that makes modern LLMs smart is **Self-Attention**.

### The Problem

Look at this sentence:
> "The animal didn't cross the street because **it** was too tired."

Who does **it** refer to — the animal or the street? A human immediately understands: tired can be an animal, not a street.

Early models couldn't handle this. Self-attention solved this problem.

### How It Works (Analogy)

> **Filing cabinet analogy:**
> The model has a "filing cabinet" of all words in the sentence. For each word, it searches this cabinet for "folders" that help understand this word.
>
> - **Query** — a sticky note with what we're looking for
> - **Keys** — labels on folders
> - **Values** — folder contents
>
> The model matches query with keys, determines which folders are most important, and mixes their contents. As a result, the word "it" gets information from "animal", not from "street".

> **Spotlight analogy:**
> Imagine the model shines a spotlight across words in a sentence. For each word, it highlights other words that help understand it. For "it" the spotlight shines on "animal". For "translated" — on "programmer" and "code".

Thanks to attention, the model understands context: knows that `ice cream` is ice cream, not "ice + cream", and that in the word "key" it means a door key, not a spring.

---

## 4. Transformer: The Architecture That Changed Everything

In 2017, Google published the paper "Attention Is All You Need", proposing the **Transformer** architecture.

Before transformers, models processed text sequentially (word by word), which was slow and inefficient. Transformer processes **all words at once**, in parallel.

### Simplified Diagram

```
Input: "Cat on the rug"
         ↓
Each word becomes numbers (embeddings)
         ↓
Attention layer: all words "look" at all words
         ↓
Neural network draws conclusions from these connections
         ↓
Again attention layer → again conclusions (6-96 times in a row)
         ↓
Output: most probable next word
```

Each layer makes the model slightly "smarter". Small models have 6-12 layers, large ones — 60-96.

> **Important:** all modern language models (GPT, Llama, Qwen, DeepSeek, Mistral) are transformers. The difference is in number of layers, parameters, and training data.

---

## 5. Context Window: What the Model "Remembers"

**Context window** — the number of tokens the model can process at once.

> **Analogy:** this is the size of the model's desk. All tools and documents that fit on the desk — the model sees. What doesn't fit — the model "forgot".

### Why This Matters

When you chat with a model, the entire dialog (your questions + model answers) must fit in the context window. If the dialog is too long — the model forgets the beginning of the conversation.

**Practical implications:**

| Window Size | How Much Text | Good For |
|-------------|---------------|----------|
| 2K (2000 tokens) | ~1400 words | Simple Q&A |
| 4K | ~2800 words | Small dialog |
| 8K | ~5600 words | Medium document |
| 32K | ~22000 words | Codebase, large article |
| 128K | ~90000 words | Novel "War and Peace" (partially) |
| 1M+ | ~700000 words | Entire project codebase |

**In practice:** for local models, 8K context is the norm. For coding with Aider you need at least 16-32K. More context = more memory needed.

---

## 6. Temperature and Creativity

**Temperature** — a parameter controlling model "creativity".

> **Analogy:** imagine the model picks the next word on a roulette wheel. At temperature 0 the wheel always stops on the most probable sector. Higher temperature = more often picks less probable variants.

| Temperature | Behavior | When to Use |
|-------------|----------|-------------|
| 0 | Conservative, always picks most probable word | Facts, translation, summarization |
| 0.3–0.5 | Some variety | Code, business correspondence |
| 0.7–0.9 | Creative | Writing, brainstorming |
| 1.0+ | Chaotic, may "fly off into space" | Poetry, idea generation |

For coding, **temperature 0.1–0.3** is recommended — you want predictable, correct code, not creativity.

---

## 7. Why Models Make Mistakes (Hallucinations)

**Hallucination** — when the model confidently outputs an incorrect fact.

### Causes

1. **Model doesn't know the boundaries of its knowledge.** It can't say "I don't know" — it always outputs the most probable continuation. If the question is about something not in training, it still answers — but the answer may be wrong.

2. **Model doesn't "remember" facts.** It doesn't store a database. It predicts words based on statistical patterns. Sometimes statistics fail.

3. **Model confuses similar concepts.** If Africa and America were often mentioned together in training, the model may mix up facts about them.

### How to Fight It

- Always verify facts, especially important ones
- Use RAG (search in your documents) — then model answers based on your data
- Lower temperature (0 → minimum hallucinations)
- Give model tools: if it can "search the web" or "run code", fewer errors

---

## 8. What's Next

| If You Want To | Go To |
|----------------|-------|
| **Choose: cloud or local** | [cloud-vs-local.md](cloud-vs-local.md) |
| **Check what hardware you need** | [hardware-guide.md](hardware-guide.md) |
| **Install Ollama and run a model** | [`../local-models/getting-started.md`](../local-models/getting-started.md) |
| **Check the glossary** | [glossary.md](glossary.md) |
| **Back to navigation** | [README.md](README.md) |