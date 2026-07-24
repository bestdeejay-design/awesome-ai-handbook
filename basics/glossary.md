# 📖 Glossary

> All key concepts you'll encounter in the local AI world.  
> Levels: 🟢 survival → 🟡 comfortable reading → 🔵 advanced beginner.

**[🇷🇺 Russian version: [glossary.ru.md](glossary.ru.md)]**

[← Level Zero](README.md) · [FAQ →](faq.md)

---

## 🟢 Level 1: Survival (10 Terms)

Must know to understand 90% of articles and conversations.

| # | Term | Explanation | Related Section |
|---|------|-------------|-----------------|
| 1 | **LLM (Large Language Model)** | Large Language Model — a program that generates and understands text. ChatGPT, Qwen, Llama — all are LLMs. | [what-is-ai.md](what-is-ai.md) |
| 2 | **Prompt** | Your request to the model. What you type in chat. "Write a Python function" — that's a prompt. | [../agents/prompting.md](../agents/prompting.md) |
| 3 | **Token** | "LEGO brick of text". A word can be 1-3 tokens. Model works with tokens, not letters. | [how-models-work.md](how-models-work.md#2-tokens-what-the-model-builds-text-from) |
| 4 | **Parameters (B)** | Model's "neurons". 7B = 7 billion parameters. More = smarter, but heavier. | [what-is-ai.md](what-is-ai.md#4-parameters-what-do-7b-14b-70b-mean) |
| 5 | **Inference** | Process of getting answer from model. You asked → model answered — that's inference. | [what-is-ai.md](what-is-ai.md#3-where-does-the-model-get-its-answers) |
| 6 | **Open-source / Open Model** | Model whose files you can download and run locally. Qwen, Llama, Mistral — open-source. | [what-is-ai.md](what-is-ai.md#5-open-source-vs-proprietary-models) |
| 7 | **Ollama** | Main program for running models locally. One command — and model works. | [../local-models/getting-started.md](../local-models/getting-started.md) |
| 8 | **Quantization** | Model compression to save memory. Q4 = model weighs 4x less, quality almost same. | [../local-models/quantization.md](../local-models/quantization.md) |
| 9 | **GGUF** | Model file format for local running. All models on Ollama — in GGUF. | [../local-models/running-models.md](../local-models/running-models.md) |
| 10 | **Local** | Everything runs on your computer, no internet, data never leaves. | [cloud-vs-local.md](cloud-vs-local.md) |

---

## 🟡 Level 2: Comfortable Reading (15 Terms)

Needed to understand technical articles and documentation.

| # | Term | Explanation | Related Section |
|---|------|-------------|-----------------|
| 11 | **Context Window** | How many tokens model "remembers" at once. 8K = ~5000 words. More = better, but needs more memory. | [how-models-work.md](how-models-work.md#5-context-window-what-the-model-remembers) |
| 12 | **Temperature** | Model "creativity" setting. 0 = conservative (for code), 1 = creative (for writing). | [how-models-work.md](how-models-work.md#6-temperature-and-creativity) |
| 13 | **System Prompt** | Instruction for model on how to behave. E.g.: "You are an experienced Python developer. Answer concisely and to the point." | [../agents/prompting.md](../agents/prompting.md) |
| 14 | **Fine-tuning** | Additional training of ready model on your data. Cheaper than training from scratch. | Rare locally, mentioned in `what-is-ai.md` |
| 15 | **GPU / Video Card** | Hardware accelerating model work. Optional but desirable. Apple Silicon — also GPU. | [hardware-guide.md](hardware-guide.md#4-windows-and-linux-cpu-vs-gpu) |
| 16 | **VRAM** | Video card memory. Main resource for large models. More VRAM = larger model you can run. | [hardware-guide.md](hardware-guide.md) |
| 17 | **Hallucination** | When model confidently outputs incorrect fact. Not maliciously — just "imagined" most probable continuation. | [how-models-work.md](how-models-work.md#7-why-models-make-mistakes-hallucinations) |
| 18 | **API** | Programmatic interface — way to call model from code. Ollama compatible with OpenAI API. | [../local-models/advanced-setup.md](../local-models/advanced-setup.md) |
| 19 | **RAG (Retrieval-Augmented Generation)** | "Search + answer": model first searches info in your documents, then answers based on it. | [../use-cases/rag.md](../use-cases/rag.md) |
| 20 | **Embedding** | Numerical representation of text meaning. Allows searching similar texts by meaning, not words. | [../use-cases/rag.md](../use-cases/rag.md) |
| 21 | **FIM (Fill-in-the-Middle)** | Code autocomplete. Model looks at code before and after cursor and inserts what should be in middle. | [../use-cases/coding.md](../use-cases/coding.md) |
| 22 | **Tool Calling (Function Calling)** | Model's ability to call functions. Allows model to run code, search web, work with files. | [../agents/ollama-for-agents.md](../agents/ollama-for-agents.md) |
| 23 | **Self-attention / Attention** | Mechanism allowing model to connect words in sentence. Thanks to it, model understands "it" refers to "animal". | [how-models-work.md](how-models-work.md#3-attention-how-the-model-connects-words) |
| 24 | **Transformer** | Neural network architecture all modern LLMs are built on. Invented by Google in 2017. | [how-models-work.md](how-models-work.md#4-transformer-the-architecture-that-changed-everything) |
| 25 | **num_ctx** | Ollama parameter setting context window size. `num_ctx: 8192` = 8K context. | [../local-models/memory-and-context.md](../local-models/memory-and-context.md) |

---

## 🔵 Level 3: Advanced Beginner (10 Terms)

Will be needed when you start configuring agents and digging deeper.

| # | Term | Explanation | Related Section |
|---|------|-------------|-----------------|
| 26 | **Prompt Engineering** | Art of crafting requests properly to get best answers from model. | [../agents/prompting.md](../agents/prompting.md) |
| 27 | **Tokens per Second (t/s)** | Model speed. 10+ t/s — comfortable for chat, 30+ — for autocomplete. | [../local-models/benchmarks/apple-silicon.md](../local-models/benchmarks/apple-silicon.md) |
| 28 | **ReAct (Reasoning + Acting)** | Agent pattern: "think → act → observe result → think again". | [../agents/patterns.md](../agents/patterns.md) |
| 29 | **AI Agent** | Model that decides which tools to call and in what order to complete a task. | [../agents/README.md](../agents/README.md) |
| 30 | **Multi-agent** | Multiple agents working together: one plans, second writes code, third tests. | [../agents/multi-agent.md](../agents/multi-agent.md) |
| 31 | **Top-k, Top-p** | Additional parameters affecting model creativity (like temperature). | [../agents/prompting.md](../agents/prompting.md) |
| 32 | **KV Cache** | Cache model uses to speed up responses. Takes lots of memory with large context. | [../local-models/memory-and-context.md](../local-models/memory-and-context.md) |
| 33 | **MoE (Mixture of Experts)** | Model architecture where part of "experts" activates only for relevant task. Saves resources. | [../local-models/catalog.md](../local-models/catalog.md) |
| 34 | **Vector Database** | Embedding storage for fast semantic search. Used in RAG. | [../use-cases/rag.md](../use-cases/rag.md) |
| 35 | **Sliding Window Attention** | Old context window type (limited length, Infinity). Rarely used in modern models. | — |

---

## How to Remember All Terms

Don't try to memorize everything at once. Read handbook sections in order — each term will appear in context and stick naturally.

**Tip:** when you encounter an unfamiliar term in an article — come back here. Over time, all will be memorized.

---

## What's Next

| If You Want To | Go To |
|----------------|-------|
| **Frequently Asked Questions** | [faq.md](faq.md) |
| **Step-by-step Learning Plan** | [learning-path.md](learning-path.md) |
| **Install Ollama** | [`../local-models/getting-started.md`](../local-models/getting-started.md) |
| **Back to Navigation** | [README.md](README.md) |