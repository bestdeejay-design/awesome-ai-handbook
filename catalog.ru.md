# 📖 Каталог файлов

> Полный список всех 52 файлов справочника.  
> Чтобы найти конкретную тему — нажмите `Cmd+F` / `Ctrl+F` и введите ключевое слово.

[← Awesome AI Handbook](README.ru.md) · [🇬🇧 English](catalog.md)

---

## 🟢 Нулевой уровень — `basics/`

| Файл | Описание | Время |
|------|----------|-------|
| [README](basics/README.ru.md) | Три дорожки обучения (5 мин / 30 мин / 2 ч) | — |
| [what-is-ai](basics/what-is-ai.ru.md) | AI, ML, LLM — без формул и математики | 5 мин |
| [how-models-work](basics/how-models-work.ru.md) | Как работают нейросети (аналогиями) | 10 мин |
| [cloud-vs-local](basics/cloud-vs-local.ru.md) | ChatGPT vs локальные модели — что и когда выбирать | 5 мин |
| [hardware-guide](basics/hardware-guide.ru.md) | Какое железо нужно: RAM, VRAM, квантование | 5 мин |
| [glossary](basics/glossary.ru.md) | Словарь: 35+ терминов от «агента» до «эмбеддинга» | ∞ |
| [faq](basics/faq.ru.md) | 15+ частых вопросов новичков | 5 мин |
| [learning-path](basics/learning-path.ru.md) | 10 шагов: от первого чата до AI-агента | 10 мин |
| [setup-windows](basics/setup-windows.ru.md) | Установка Ollama на Windows (нативный / WSL2) | 10 мин |
| [setup-linux](basics/setup-linux.ru.md) | Установка Ollama на Linux (CPU / CUDA / Docker) | 10 мин |

## 🔵 Локальные модели — `local-models/`

| Файл | Описание | Уровень | Время |
|------|----------|---------|-------|
| [README](local-models/README.ru.md) | Сценарии: новичок → продвинутый | — | — |
| [getting-started](local-models/getting-started.ru.md) | 🟢 Полный гайд для Mac: Homebrew, Ollama, первая модель | 🟢 | 10 мин |
| [running-models](local-models/running-models.ru.md) | Как искать и запускать модели: Ollama, LM Studio, HuggingFace | 🟢🟡 | 15 мин |
| [models](local-models/models.ru.md) | Выбор модели под задачу: кодинг, чат, RAG, аналитика | 🟡 | 10 мин |
| [catalog](local-models/catalog.ru.md) | 50+ open-weight LLM с характеристиками для запуска | 🟡 | — |
| [quantization](local-models/quantization.ru.md) | Квантизация: Q4, Q5, Q8 — сжатие без потери качества | 🟡 | 10 мин |
| [memory-and-context](local-models/memory-and-context.ru.md) | RAM, KV cache, контекст — какая модель влезет в ваше железо | 🟡 | 10 мин |
| [tools](local-models/tools.ru.md) | Сравнение движков: Ollama, LM Studio, MLX, llama.cpp | 🟡 | 20 мин |
| [advanced-setup](local-models/advanced-setup.ru.md) | Modelfile, API, параллельные запросы, env vars | 🔵 | 15 мин |
| [troubleshooting](local-models/troubleshooting.ru.md) | Частые проблемы и их решения | 🟡 | 5 мин |
| [apple-silicon](local-models/benchmarks/apple-silicon.ru.md) | Бенчмарки Apple Silicon: tok/s на M1–M4 | 🔵 | 5 мин |

## 🤖 AI-агенты — `agents/`

| Файл | Описание | Уровень | Время |
|------|----------|---------|-------|
| [README](agents/README.ru.md) | Обзор раздела + минимальный код агента | — | — |
| [ready-made](agents/ready-made.ru.md) | 🟢 Каталог готовых AI-агентов: Aider, Cline, Cursor, OpenHands | 🟢 | 7 мин |
| [orchestrators](agents/orchestrators.ru.md) | Оркестраторы с субагентами: Sisyphus, Atlas, Prometheus | 🔵 | 6 мин |
| [skills](agents/skills.ru.md) | Скиллы, MCP-серверы, плагины — как расширять агента | 🟡 | 6 мин |
| [architecture](agents/architecture.ru.md) | Из чего состоит агент: LLM → инструменты → цикл → память | 🔵 | 11 мин |
| [patterns](agents/patterns.ru.md) | Архитектурные паттерны: ReAct, Plan-and-Execute, Reflection | 🔵 | 14 мин |
| [frameworks](agents/frameworks.ru.md) | Сравнение LangGraph, CrewAI, Agno | 🔵 | 12 мин |
| [multi-agent](agents/multi-agent.ru.md) | Коммуникация и координация между агентами | 🔵 | 18 мин |
| [memory](agents/memory.ru.md) | Долговременная память: контекст, хранение, извлечение | 🔵 | 9 мин |
| [prompting](agents/prompting.ru.md) | System prompts для управления поведением агента | 🔵 | 11 мин |
| [ollama-for-agents](agents/ollama-for-agents.ru.md) | Tool calling, JSON schema, structured output через Ollama | 🔵 | 10 мин |
| [safety](agents/safety.ru.md) | Безопасность: guardrails, лимиты, изоляция, валидация | 🔵 | 16 мин |
| [evaluation](agents/evaluation.ru.md) | Тестирование качества агента, отлов регрессий | 🔵 | 10 мин |

### Туториалы — `agents/tutorials/`

| Файл | Описание | Уровень | Время |
|------|----------|---------|-------|
| [README](agents/tutorials/README.ru.md) | Индекс туториалов | — | — |
| [01-first-agent](agents/tutorials/01-first-agent.ru.md) | 🛠️ ReAct-агент на Python + Ollama | 🟢 | 30 мин |
| [02-agent-team](agents/tutorials/02-agent-team.ru.md) | 🛠️ Мульти-агентная команда (CrewAI) | 🟡 | 60 мин |
| [03-coding-agent](agents/tutorials/03-coding-agent.ru.md) | 🛠️ Coding-агент (LangGraph) | 🔴 | 45 мин |

## 💼 Use Cases — `use-cases/`

| Файл | Описание | Стек | Время |
|------|----------|------|-------|
| [README](use-cases/README.ru.md) | Обзор кейсов | — | — |
| [coding](use-cases/coding.ru.md) | Бесплатный Copilot на локальной модели | Continue, Aider, OpenHands | 5 мин |
| [rag](use-cases/rag.ru.md) | Q&A по своим файлам без облака | Open WebUI, ChromaDB | 7 мин |
| [automation](use-cases/automation.ru.md) | AI + бизнес-процессы | n8n, cron, Telegram-бот | 7 мин |
| [writing](use-cases/writing.ru.md) | Контент-пайплайн: генерация, рефлексия | Qwen, Reflection | 6 мин |

## 📖 Ресурсы — `resources/`

| Файл | Описание | Для кого | Время |
|------|----------|----------|-------|
| [README](resources/README.ru.md) | Индекс ресурсов | — | — |
| [ru](resources/ru.ru.md) | Русскоязычные AI-ресурсы | 🟢 Все | 3 мин |
| [en](resources/en.ru.md) | Англоязычные ресурсы | 🟡 | 3 мин |
| [courses](resources/courses.ru.md) | Бесплатные курсы по AI | 🟢🟡 | 3 мин |
| [communities](resources/communities.ru.md) | Discord, Reddit, Telegram-чаты | 🟢 Все | 3 мин |

## Корневые файлы

| Файл | Описание |
|------|----------|
| [CONTRIBUTING](CONTRIBUTING.ru.md) | Как помочь проекту |
| [CODE_OF_CONDUCT](CODE_OF_CONDUCT.ru.md) | Кодекс поведения |
| [SECURITY](SECURITY.ru.md) | Политика безопасности |

---

**Навигация:** [← Awesome AI Handbook](README.ru.md) · [🇬🇧 English](catalog.md)
