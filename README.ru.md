[![Русский](https://img.shields.io/badge/Русский-README-blue.svg)](README.ru.md)
[![English](https://img.shields.io/badge/English-README-blue.svg)](README.md)

# Awesome AI Handbook (русская версия) 
...

# 🧠 Awesome AI Handbook

> Практический справочник по AI: от локальных моделей до своих агентов.
> Не просто ссылки, а готовые руководства, бенчмарки и проверенные подходы.

**[English](README.md)** | **Русский**

---

## 📌 О проекте

**Миссия:** дать каждому готовый инструмент — от первого запуска модели до своего AI-агента для кодинга.  
**Цель:** чтобы любой человек с «более-менее вменяемым железом» мог собрать себе локальную AI-систему.

**Чем отличается от других awesome-списков:**
- 🔧 **Практика, а не ссылки** — пошаговые гайды с кодом
- 💻 **Реальные бенчмарки** — цифры для конкретного железа (MacBook Air M1 16 GB и не только)
- 🤖 **Создание агентов** — туториалы «от идеи до работающего прототипа» ✅
- 🌍 **Два языка** — английский + русский, заполняем вакуум для RU-сообщества
- 🟢 **Нулевой уровень** — раздел для тех, кто «даже не пробовал, но хочет узнать»

**Для кого:** от людей, впервые открывающих для себя AI, до опытных разработчиков, строящих production-агентов.

---

## 🚀 Быстрый старт (5 минут)

```bash
# 1. Установка Ollama
brew install ollama    # macOS
# Linux: curl -fsSL https://ollama.com/install.sh | sh
# Windows: скачать с https://ollama.com/download/windows

# 2. Запуск модели (автоматически скачает)
ollama run qwen3.5:4b   # ~3.4 GB — чат
# или для кодинга:
ollama run qwen2.5-coder:7b  # ~4.7 GB

# 3. Готово! Спросите что-нибудь.
```

**Полный новичок?** Начните с раздела [`basics/`](basics/README.ru.md) — там всё с нуля.  
Уже в теме: [запуск моделей](local-models/running-models.ru.md) · [что влезет в RAM](local-models/memory-and-context.ru.md) · [выбор модели](local-models/models.ru.md)

---

## 📂 Структура репозитория

```
awesome-ai-handbook/
├── README.ru.md              ← вы здесь
├── basics/                   ← 🟢 нулевой уровень (для новичков)
│   ├── README.ru.md          ← три дорожки обучения
│   ├── what-is-ai.ru.md      ← AI, ML, LLM — без математики
│   ├── how-models-work.ru.md ← нейросети, трансформеры, токены
│   ├── cloud-vs-local.ru.md  ← когда облако, когда локально
│   ├── hardware-guide.ru.md  ← рекомендации по железу
│   ├── glossary.ru.md        ← словарь терминов (3 уровня)
│   ├── faq.ru.md             ← частые вопросы
│   ├── learning-path.ru.md   ← 10 шагов от чата до агента
│   ├── setup-windows.ru.md   ← установка на Windows
│   └── setup-linux.ru.md     ← установка на Linux
├── local-models/             ← локальный инференс
│   ├── README.md             ← навигация и сценарии
│   ├── getting-started.md    ← [NEW] подготовка окружения для новичков
│   ├── running-models.md     ← Ollama, LM Studio, HuggingFace
│   ├── models.md             ← выбор по задачам
│   ├── memory-and-context.md ← RAM, KV cache, num_ctx
│   ├── advanced-setup.md     ← [NEW] Modelfile, API, env vars
│   ├── tools.md              ← Ollama, MLX, llama.cpp…
│   ├── quantization.md       ← Q4, MLX, TurboQuant
│   ├── troubleshooting.md    ← частые проблемы
│   ├── catalog.md            ← 50+ моделей
│   └── benchmarks/
│       └── apple-silicon.md  ← tok/s на M1–M4
├── agents/                   ← AI-агенты
│   ├── patterns.md           ← ReAct, Multi-Agent…
│   ├── frameworks.md         ← LangGraph, CrewAI…
│   ├── ollama-for-agents.md  ← tool calling, JSON schema
│   └── tutorials/            ← гайды ✅
├── use-cases/                ← практика
│   ├── coding.md
│   ├── rag.md
│   ├── automation.md
│   └── writing.md
├── resources/                ← сообщества и курсы
│   ├── ru.md                 ← RU-ресурсы (🚧)
│   ├── en.md
│   ├── courses.md
│   └── communities.md
└── CONTRIBUTING.md
```

---

## 🗺 Навигация по темам

### 🟢 Нулевой уровень (для новичков)

| Тема | Файл |
|------|------|
| **С чего начать (навигатор)** | [basics/README.ru.md](basics/README.ru.md) |
| Что такое AI, ML, LLM | [what-is-ai.ru.md](basics/what-is-ai.ru.md) |
| Как работают нейросети | [how-models-work.ru.md](basics/how-models-work.ru.md) |
| Когда облако, когда локально | [cloud-vs-local.ru.md](basics/cloud-vs-local.ru.md) |
| Какое нужно железо | [hardware-guide.ru.md](basics/hardware-guide.ru.md) |
| Словарь терминов | [glossary.ru.md](basics/glossary.ru.md) |
| FAQ | [faq.ru.md](basics/faq.ru.md) |
| План обучения: от чата до агента | [learning-path.ru.md](basics/learning-path.ru.md) |
| Установка на Windows 🪟 | [setup-windows.ru.md](basics/setup-windows.ru.md) |
| Установка на Linux 🐧 | [setup-linux.ru.md](basics/setup-linux.ru.md) |

### 🔵 Локальные модели

| Тема | Файл |
|------|------|
| **Начать с нуля** 🟢 | [getting-started.md](local-models/getting-started.ru.md) |
| Запуск моделей | [running-models.md](local-models/running-models.ru.md) |
| Выбор по задаче | [models.md](local-models/models.ru.md) |
| Память и контекст | [memory-and-context.md](local-models/memory-and-context.ru.md) |
| Продвинутая настройка 🔵 | [advanced-setup.md](local-models/advanced-setup.ru.md) |
| Инструменты | [tools.md](local-models/tools.ru.md) |
| Квантизация | [quantization.md](local-models/quantization.ru.md) |
| Бенчмарки Mac | [benchmarks/apple-silicon.md](local-models/benchmarks/apple-silicon.ru.md) |
| Проблемы | [troubleshooting.md](local-models/troubleshooting.ru.md) |
| Каталог моделей | [catalog.md](local-models/catalog.ru.md) |

### AI-агенты

| Тема | Файл |
|------|------|
| Обзор | [agents/README.ru.md](agents/README.ru.md) |
| Архитектура агента | [architecture.md](agents/architecture.ru.md) |
| Паттерны | [patterns.md](agents/patterns.ru.md) |
| Фреймворки | [frameworks.md](agents/frameworks.ru.md) |
| Ollama для агентов | [ollama-for-agents.md](agents/ollama-for-agents.ru.md) |
| Мульти-агентные системы | [multi-agent.md](agents/multi-agent.ru.md) |
| Память | [memory.md](agents/memory.ru.md) |
| Промптинг | [prompting.md](agents/prompting.ru.md) |
| Безопасность | [safety.md](agents/safety.ru.md) |
| Тестирование | [evaluation.md](agents/evaluation.ru.md) |
| Туториалы | [tutorials/](agents/tutorials/) ✅ |

### Практические кейсы

| Кейс | Файл |
|------|------|
| Кодинг | [coding.md](use-cases/coding.ru.md) |
| RAG | [rag.md](use-cases/rag.ru.md) |
| Автоматизация | [automation.md](use-cases/automation.ru.md) |
| Письмо | [writing.md](use-cases/writing.ru.md) |

### Ресурсы

| Раздел | Файл |
|--------|------|
| RU-ресурсы | [ru.md](resources/ru.ru.md) |
| EN-ресурсы | [en.md](resources/en.ru.md) |
| Курсы | [courses.md](resources/courses.ru.md) |
| Сообщества | [communities.md](resources/communities.ru.md) |

---

## 🤝 Контрибьютинг

Приветствуются бенчмарки, RU-ресурсы, туториалы и исправления. Подробности — в [CONTRIBUTING.md](CONTRIBUTING.md).

---

## 📄 Лицензия

[MIT](LICENSE)

---

*Этот проект создан для того, чтобы каждый мог расширить свой кругозор в мире AI — независимо от технического уровня и железа.*
