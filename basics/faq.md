# ❓ FAQ: Frequently Asked Questions

> Answers to the most common questions about local AI models, hardware, coding, and safety.

**🇷🇺 Russian version:** [faq.ru.md](faq.ru.md)

---

[← Glossary](glossary.md) · [Learning path →](learning-path.md)

---

## Contents

1. [General questions](#1-general-questions)
2. [About hardware](#2-about-hardware)
3. [About models](#3-about-models)
4. [About coding](#4-about-coding)
5. [About safety and legality](#5-about-safety-and-legality)
6. [Whats next](#6-whats-next)

---

## 1. General questions

### Is AI dangerous?

**Analogy:** imagine you saw a car for the first time. It is fast, powerful, can kill — but you are not afraid of every car on the road. The danger is not that the model "wants" to harm you, but that a person might blindly trust its answers. Treat AI as a helper that can make mistakes — and you will be fine.

### Is ChatGPT the only AI?

ChatGPT is one specific service from OpenAI. But there are dozens of other models: **Llama** (Meta), **Qwen** (Alibaba), **Mistral**, **DeepSeek**, **Gemma** (Google). They can be just as good as ChatGPT — and run entirely on your computer, for free, without internet.

ChatGPT is the tip of the iceberg. Below the surface lies an ocean of open models you can run locally.

### Will AI replace my job?

Just like Excel did not replace accountants but made them more effective — AI is becoming a tool of augmentation, not replacement. Especially in many professions that require local knowledge and human contact — AI is still weak here.

### Why run locally if ChatGPT exists?

| Argument | ChatGPT | Local model |
|----------|---------|-------------|
| Privacy | Everything goes to OpenAI servers | Nothing leaves your computer |
| Price | $20/month or usage limits | Free (just electricity) |
| Availability | Needs internet | Works offline anywhere |
| Control | Model can be changed or "fixed" | Your model, your rules |
| Customization | Cannot fine-tune | Can fine-tune for your tasks |

More details: [cloud-vs-local.md](cloud-vs-local.md)

---

## 2. About hardware

### I have 8 GB RAM — can I run anything?

Yes. Run small models (1-3B parameters): Phi-3-mini, Qwen 2.5 1.5B. They work on 8 GB, though speed will be modest (~10-15 tok/s). Enough for simple chat and code autocomplete.

If budget allows — a used MacBook Air M1 16 GB will give a much better experience.

### Do I need a GPU?

Not necessarily. Modern tools (Ollama, LM Studio, llama.cpp) work well on CPU. GPU accelerates by 2-5x but is not required.

**Exception:** for 14B+ models GPU is recommended. For models up to 7B, CPU is sufficient.

### Is Mac bad for AI?

**Outdated myth.** Apple Silicon (M1/M2/M3/M4) is one of the best platforms for local AI:
- Unified Memory: all RAM is available to the model
- MLX Framework: often faster than CUDA on comparable hardware
- Energy efficiency: MacBook runs on battery, PC with GPU consumes 300-450W

Intel Mac is not suitable. But Apple Silicon is an ideal starting point.

### My computer is 5 years old — can it run?

Depends on configuration:
- 5-year-old MacBook Pro (Intel) 16 GB — can run models up to 7B, slowly
- 5-year-old Windows laptop 8 GB without GPU — only 1-3B models
- **Tip:** use Q4 quantization and small models. Start with Qwen 2.5 1.5B.

---

## 3. About models

### How many models can I install?

As many as you want. Each model is a file (4-40 GB). Ollama stores them in `~/.ollama/models/`.

```bash
ollama rm qwen3.5:4b    # remove a model
ollama list              # see installed models
```

### Models give wrong answers (hallucinations). What to do?

- Verify important facts
- Use RAG (search your documents) — the model answers based on your data
- Lower temperature (0 = fewer hallucinations)

### Which model is best for Russian?

**Qwen 3.5** from Alibaba is the best for Russian among open-source models. Also good: **Llama 3.1** (worse, but can be improved with system prompts) and **DeepSeek-R1** (excellent reasoning, average Russian).

### How to update Ollama?

```bash
brew upgrade ollama  # Mac
# Or reinstall from ollama.com for Windows/Linux
```

### Can I run multiple models at the same time?

Ollama keeps a model in RAM while you work with it. If you start a second one, the first gets unloaded.

**Practical tip:** use one model for chat (Qwen 3.5 7B) and one small one for autocomplete (CodeGemma 2B).

---

## 4. About coding

### Can I use local models for coding?

Yes. This is one of the main use cases. Install Ollama, download a coding model (Qwen 2.5 Coder 7B), and connect to VS Code via Continue.dev or Aider.

Step-by-step: [`../use-cases/coding.md`](../use-cases/coding.md)

### What is vibecoding?

Vibecoding is an approach where you describe what you want, and AI writes the code. You review, give feedback, AI fixes.

### What is Aider?

Aider is a terminal-based coding tool. You give it a task, it reads your codebase and makes changes. Works with local models via Ollama.

More: [`../use-cases/coding.md`](../use-cases/coding.md)

### OpenCode / Cursor do not work with local models — what to do?

For local models use:
1. **VS Code + Continue.dev** — plugin, works with Ollama
2. **Aider** — terminal coding agent
3. **Cline** — VS Code extension with local models
4. **Void** — open-source Cursor alternative

More: [`../use-cases/coding.md`](../use-cases/coding.md)

---

## 5. About safety and legality

### Is it legal to use open-source models for commercial work?

**Yes, in most cases.** Major open-source models have commercial licenses.

**Nuances:**
- Copyright on generated code is debatable
- **Practical tip:** check each models license on its page
- For content — do not pass generated material off as your own without editing

More: [`../agents/safety.md`](../agents/safety.md)

---

## 6. Whats next

| If you want | Go to |
|-------------|-------|
| **Step-by-step plan: from chat to agent** | [learning-path.md](learning-path.md) |
| **Install Ollama** | [`../local-models/getting-started.md`](../local-models/getting-started.md) |
| **Start coding** | [`../use-cases/coding.md`](../use-cases/coding.md) |
| **Back to navigation** | [README.md](README.md) |

---

**In section:** [what-is-ai](what-is-ai.md) · [how-models-work](how-models-work.md) · [cloud-vs-local](cloud-vs-local.md) · [hardware-guide](hardware-guide.md) · [glossary](glossary.md) · [faq](faq.md) · [learning-path](learning-path.md) · [setup-windows](setup-windows.md) · [setup-linux](setup-linux.md)  
**Related sections:** [Local Models](../local-models/README.md) · [AI Agents](../agents/README.md) · [Use Cases](../use-cases/README.md)  
**Navigation:** [← Zero Level](README.md) · [↑ Back to main](../README.md) · [🇷🇺 Русский](faq.ru.md)
