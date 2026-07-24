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

**Миссия:** помочь каждому расширить кругозор в мире AI — от первого запуска модели локально до создания собственного агента.

**Чем отличается от других awesome-списков:**
- 🔧 **Практика, а не ссылки** — пошаговые гайды с кодом
- 💻 **Реальные бенчмарки** — цифры для конкретного железа (MacBook Air M1 16 GB и не только)
- 🤖 **Создание агентов** — туториалы «от идеи до работающего прототипа» *(в разработке)*
- 🌍 **Два языка** — английский + русский, заполняем вакуум для RU-сообщества

**Для кого:** разработчики, запускающие AI локально; те, кто строит своих агентов; все, кто хочет разобраться без глубокого погружения в математику.

---

## 🚀 Быстрый старт (5 минут)

```bash
# 1. Установка Ollama
brew install ollama    # macOS
# Linux: curl -fsSL https://ollama.com/install.sh | sh

# 2. Запуск модели (автоматически скачает)
ollama run qwen3.5:4b   # ~3.4 GB — чат
# или для кодинга:
ollama run qwen2.5-coder:7b  # ~4.7 GB

# 3. Готово! Спросите что-нибудь.
```

Новичок? Начните с **[пошаговой установки](local-models/getting-started.ru.md)**.  
Уже установлено: [как запускать модели](local-models/running-models.ru.md) · [что влезет в RAM](local-models/memory-and-context.ru.md)

---

## 📂 Структура репозитория

```
awesome-ai-handbook/
├── README.ru.md              ← вы здесь
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
│   └── tutorials/            ← гайды (🚧)
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

### Локальные модели

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
| RU-ресурсы 🚧 | [ru.md](resources/ru.ru.md) |
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
