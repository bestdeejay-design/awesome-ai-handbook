# <img src="https://lucide.dev/api/icons/brain" alt="" width="20" height="20" style="display:inline;vertical-align:middle"> Awesome AI Handbook

> Практический справочник по AI: от запуска первой локальной модели до своего агента.  
> **52 файла** · **~6 часов чтения** · **Уровни: <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> новичок → <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-red.svg" alt="" width="12" height="12" style="vertical-align:middle"> профи**

**[English](README.md)** · **Русский**

---

## <img src="https://lucide.dev/api/icons/rocket" alt="" width="20" height="20" style="display:inline;vertical-align:middle"> Быстрый старт

```bash
brew install ollama            # macOS
ollama run qwen3.5:4b          # чат (~3.4 GB) — готово!
```

**Никогда не пробовали AI?** → [basics/](basics/README.ru.md) — всё с нуля, без терминала.  
**Уже в теме?** → [каталог файлов](catalog.ru.md) — найти конкретную тему за 10 секунд.

---

## <img src="https://lucide.dev/api/icons/book-open" alt="" width="20" height="20" style="display:inline;vertical-align:middle"> Разделы

| Раздел | Описание | Вход | Файлов | Уровень |
|--------|----------|------|--------|---------|
| [<img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> Нулевой уровень](basics/README.ru.md) | Основы AI: термины, установка, выбор железа | `basics/` | 9 | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> |
| [<img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> Локальные модели](local-models/README.ru.md) | Запуск, выбор, настройка и оптимизация моделей | `local-models/` | 11 | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle">→<img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> |
| [<img src="https://lucide.dev/api/icons/bot" alt="" width="20" height="20" style="display:inline;vertical-align:middle"> AI-агенты](agents/README.ru.md) | Архитектура, паттерны, фреймворки, туториалы | `agents/` | 17 | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle">→<img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-red.svg" alt="" width="12" height="12" style="vertical-align:middle"> |
| [<img src="https://lucide.dev/api/icons/briefcase" alt="" width="20" height="20" style="display:inline;vertical-align:middle"> Use Cases](use-cases/README.ru.md) | Кодинг, RAG, автоматизация, контент | `use-cases/` | 5 | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-yellow.svg" alt="" width="12" height="12" style="vertical-align:middle"> |
| [<img src="https://lucide.dev/api/icons/book-open" alt="" width="20" height="20" style="display:inline;vertical-align:middle"> Ресурсы](resources/README.ru.md) | Сообщества, курсы, русско- и англоязычные материалы | `resources/` | 5 | <img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle">→<img src="https://bestdeejay-design.github.io/awesome-ai-handbook/assets/icons/level-yellow.svg" alt="" width="12" height="12" style="vertical-align:middle"> |

> <img src="https://lucide.dev/api/icons/search" alt="" width="20" height="20" style="display:inline;vertical-align:middle"> **Ищете конкретный файл?** [Открыть каталог →](catalog.ru.md) — полный список всех 52 файлов с описаниями.

---

## <img src="https://lucide.dev/api/icons/folder-tree" alt="" width="20" height="20" style="display:inline;vertical-align:middle"> Структура проекта

```
awesome-ai-handbook/
│
├── README.md, README.ru.md      ← навигация (RU + EN)
├── catalog.md, catalog.ru.md    ← полный каталог файлов
│
├── basics/                      # 🟢 нулевой уровень
│   ├── README.ru.md             #   три дорожки обучения
│   ├── what-is-ai.ru.md         #   AI, ML, LLM — без формул
│   ├── how-models-work.ru.md    #   нейросети на аналогиях
│   ├── cloud-vs-local.ru.md     #   ChatGPT vs локально
│   ├── hardware-guide.ru.md     #   RAM, VRAM, квантование
│   ├── glossary.ru.md           #   35 терминов
│   ├── faq.ru.md                #   15+ частых вопросов
│   ├── learning-path.ru.md      #   10 шагов: чат → агент
│   ├── setup-windows.ru.md      #   установка на Windows
│   └── setup-linux.ru.md        #   установка на Linux
│
├── local-models/                # 🔵 локальный инференс
│   ├── README.ru.md             #   сценарии: новичок → профи
│   ├── getting-started.ru.md    #   🟢 полный гайд для Mac
│   ├── running-models.ru.md     #   от Ollama до HuggingFace
│   ├── models.ru.md             #   выбор модели под задачу
│   ├── catalog.ru.md            #   50+ моделей с характеристиками
│   ├── quantization.ru.md       #   Q4, Q5, Q8 — сжатие
│   ├── memory-and-context.ru.md #   RAM, KV cache, контекст
│   ├── tools.ru.md              #   сравнение движков
│   ├── advanced-setup.ru.md     #   🔵 Modelfile, API, env
│   ├── troubleshooting.ru.md    #   частые проблемы
│   └── benchmarks/
│       └── apple-silicon.ru.md  #   🔵 tok/s на M1–M4
│
├── agents/                      # 🤖 AI-агенты
│   ├── README.ru.md             #   обзор + минимальный код
│   ├── ready-made.ru.md         #   🟢 каталог готовых агентов
│   ├── orchestrators.ru.md      #   🔵 оркестраторы
│   ├── skills.ru.md             #   🟡 скиллы, MCP, плагины
│   ├── architecture.ru.md       #   🔵 из чего состоит агент
│   ├── patterns.ru.md           #   🔵 ReAct, Multi-Agent
│   ├── frameworks.ru.md         #   🔵 LangGraph, CrewAI
│   ├── multi-agent.ru.md        #   🔵 коммуникация
│   ├── memory.ru.md             #   🔵 долговременная память
│   ├── prompting.ru.md          #   🔵 system prompts
│   ├── ollama-for-agents.ru.md  #   🔵 tool calling
│   ├── safety.ru.md             #   🔵 guardrails
│   ├── evaluation.ru.md         #   🔵 тестирование
│   └── tutorials/
│       ├── README.ru.md         #   индекс туториалов
│       ├── 01-first-agent.ru.md #   🟢 первый агент
│       ├── 02-agent-team.ru.md  #   🟡 команда агентов
│       └── 03-coding-agent.ru.md#   🔴 агент-программист
│
├── use-cases/                   # 💼 применение
│   ├── README.ru.md             #   кейсы
│   ├── coding.ru.md             #   🟡 Continue, Aider
│   ├── rag.ru.md                #   🟡 Open WebUI, ChromaDB
│   ├── automation.ru.md         #   🟡 n8n, Telegram-бот
│   └── writing.ru.md            #   🟡 контент-пайплайн
│
├── resources/                   # 📖 сообщества и курсы
│   ├── README.ru.md             #   индекс
│   ├── ru.ru.md                 #   🟢 русскоязычные
│   ├── en.ru.md                 #   🟡 англоязычные
│   ├── courses.ru.md            #   🟢🟡 курсы
│   └── communities.ru.md        #   🟢 Discord, Reddit
│
├── CONTRIBUTING.md              # руководство контрибьютора
├── CODE_OF_CONDUCT.md           # кодекс поведения
└── SECURITY.md                  # политика безопасности
```

---

## <img src="https://lucide.dev/api/icons/handshake" alt="" width="20" height="20" style="display:inline;vertical-align:middle"> Контрибьютинг

Приветствуются бенчмарки, RU-ресурсы, туториалы и исправления.  
Подробности — [CONTRIBUTING.md](CONTRIBUTING.md) · [Кодекс поведения](CODE_OF_CONDUCT.md)

## <img src="https://lucide.dev/api/icons/file-text" alt="" width="20" height="20" style="display:inline;vertical-align:middle"> Лицензия

[MIT](LICENSE)

---

*Awesome AI Handbook — чтобы каждый мог освоить AI, независимо от уровня и железа.*
