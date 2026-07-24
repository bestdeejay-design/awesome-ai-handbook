[![Русский](https://img.shields.io/badge/Русский-README-blue.svg)](README.ru.md)
[![English](https://img.shields.io/badge/English-README-blue.svg)](README.md)

# 🧠 Awesome AI Handbook

> Практический справочник по AI: от локальных моделей до своих агентов.  
> Не просто ссылки, а готовые руководства, бенчмарки и проверенные подходы.

**[English](README.md)** | **Русский**

---

## 📌 О проекте

**Миссия:** дать каждому готовый инструмент — от первого запуска модели до своего AI-агента для кодинга.  
**Цель:** чтобы любой человек с любым «более-менее вменяемым» компьютером мог собрать себе локальную AI-систему.

**Для кого:** от людей, впервые открывающих для себя AI, до опытных разработчиков, строящих production-агентов.

**Что здесь есть (51 файл, ~6 часов чтения):**

| Раздел | Файлов | Уровень | Время |
|--------|--------|---------|-------|
| [🟢 Нулевой уровень](#-нулевой-уровень--разобраться-в-основах) | 10 | с нуля | ~1 ч |
| [🔵 Локальные модели](#-локальные-модели--настроить-инференс) | 11 | 🟢→🔵 | ~2 ч |
| [🤖 AI-агенты](#-ai-агенты--построить-своего-помощника) | 15 | 🟢→🔴 | ~3 ч |
| [💼 Use Cases](#-use-cases--применить-на-практике) | 5 | 🟡 | ~30 мин |
| [📖 Ресурсы](#-ресурсы--найти-сообщества-и-курсы) | 5 | 🟢→🟡 | ~15 мин |

---

## 🚀 Мгновенный старт

```bash
brew install ollama            # macOS
# Linux: curl -fsSL https://ollama.com/install.sh | sh
# Windows: скачать с https://ollama.com/download/windows

ollama run qwen3.5:4b          # чат (~3.4 GB) — готово!
```

🟢 **Впервые слышите про локальный AI?** → [Нулевой уровень](#-нулевой-уровень--разобраться-в-основах) — там всё с нуля.  
Уже в теме? → [Поиск по файлам](#-быстрый-поиск-по-файлам).

---

## 🗺️ Оглавление

> Каждый раздел — это шаг на пути от полного новичка до владельца собственной локальной AI-инфраструктуры.  
> Выберите свой уровень и цель.

---

### 🟢 Нулевой уровень — разобраться в основах

**Где:** `basics/` · **Время:** ~50 мин · **Результат:** понимаю термины, выбрал подход (облако / локально), установил Ollama

| Раздел | Что узнаю / сделаю | Время |
|--------|-------------------|-------|
| [Что такое AI, ML и LLM](basics/what-is-ai.ru.md) | Пойму разницу между терминами «искусственный интеллект», «машинное обучение» и «языковая модель» — без математики и формул | 4 мин |
| [Как работают нейросети](basics/how-models-work.ru.md) | Узнаю на аналогиях, как нейросети генерируют текст: трансформеры, токены, температура | 6 мин |
| [Облачные AI vs локальные модели](basics/cloud-vs-local.ru.md) | Сравню ChatGPT, Claude, DeepSeek с локальными моделями и пойму, что выбрать под свои задачи | 4 мин |
| [Какое нужно железо](basics/hardware-guide.ru.md) | Проверю, подходит ли мой компьютер: разберусь с RAM, VRAM, квантованием (мифы о суперкомпьютерах развеяны) | 6 мин |
| [Словарь терминов](basics/glossary.ru.md) | Найду определение любого термина: 35+ понятий от «агента» до «эмбеддинга» с примерами | 3 мин |
| [FAQ](basics/faq.ru.md) | Получу ответы на 15+ частых вопросов новичков | 6 мин |
| [План обучения](basics/learning-path.ru.md) | Построю пошаговый маршрут: 10 шагов от первого чата до собственного AI-агента | 7 мин |
| [Установка на Windows 🪟](basics/setup-windows.ru.md) | Установлю Ollama двумя способами (нативный / WSL2), запущу первую модель | 6 мин |
| [Установка на Linux 🐧](basics/setup-linux.ru.md) | Установлю Ollama на Linux (CPU / CUDA / Docker), настрою сервис | 7 мин |

**Дальше:** Установили Ollama? → [Локальные модели](#-локальные-модели--настроить-инференс) · Хотите сразу кодить? → [AI для кодинга](use-cases/coding.ru.md)

---

### 🔵 Локальные модели — настроить инференс

**Где:** `local-models/` · **Время:** ~1.5 ч · **Результат:** умею запускать, выбирать, настраивать и оптимизировать модели

**Старт здесь:** [Пошаговая установка для Mac](local-models/getting-started.ru.md) — полный гайд для первого запуска

| Раздел | Что узнаю / сделаю | Уровень | Время |
|--------|-------------------|---------|-------|
| [Как найти и запустить модель](local-models/running-models.ru.md) | Освою Ollama, LM Studio и HuggingFace: от одной команды до продвинутых сценариев | 🟢🟡 | 4 мин |
| [Модели для разных задач](local-models/models.ru.md) | Научусь выбирать модель под конкретную задачу: кодинг, чат, RAG, аналитика | 🟡 | 5 мин |
| [Каталог моделей](local-models/catalog.ru.md) | Просмотрю 50+ open-weight LLM с характеристиками для локального запуска | 🟡 | 10 мин |
| [Квантизация](local-models/quantization.ru.md) | Разберусь, как сжать модель в 2–4 раза без потери качества | 🟡 | 4 мин |
| [Память и контекст](local-models/memory-and-context.ru.md) | Пойму, какая модель влезет в мою RAM, и как настроить контекст | 🟡 | 4 мин |
| [Сравнение инструментов](local-models/tools.ru.md) | Выберу движок: Ollama, LM Studio, MLX, llama.cpp — подробное сравнение | 🟡 | 30 мин |
| [Продвинутая настройка Ollama](local-models/advanced-setup.ru.md) | Настрою Modelfile, API, параллельные запросы, несколько моделей | 🔵 | 18 мин |
| [Бенчмарки Apple Silicon](local-models/benchmarks/apple-silicon.ru.md) | Узнаю реальную скорость моделей на M1–M4 (токенов/с) | 🔵 | 3 мин |
| [Частые проблемы](local-models/troubleshooting.ru.md) | Найду решение, если что-то пошло не так | 🟡 | 2 мин |

**Дальше:** Хотите, чтобы модель **сама вызывала инструменты**? → [AI-агенты](#-ai-агенты--построить-своего-помощника)  
Хотите применить к реальной задаче? → [Use Cases](#-use-cases--применить-на-практике)

---

### 🤖 AI-агенты — построить своего помощника

**Где:** `agents/` · **Время:** ~3 ч · **Результат:** создаю автономных AI-агентов от простого ReAct до мульти-агентной команды

| Раздел | Что узнаю / сделаю | Уровень | Время |
|--------|-------------------|---------|-------|
| [Готовые AI-агенты](agents/ready-made.ru.md) | **Выберу готовый инструмент** вместо написания с нуля: Aider, Cline, Cursor, Continue, OpenHands — сравнение и рекомендации | 🟢 | 7 мин |
| [Скиллы для агентов](agents/skills.ru.md) | **Пойму, как расширять агента** скиллами, MCP-серверами и плагинами, напишу свой первый скилл | 🟡 | 6 мин |
| [Туториал 1: первый агент за 30 мин](agents/tutorials/01-first-agent.ru.md) | 🛠️ **Напишу ReAct-агента** на Python + Ollama: сам решает, вызывать ли инструменты | 🟢 | 7 мин |
| [Туториал 2: команда агентов](agents/tutorials/02-agent-team.ru.md) | 🛠️ **Соберу мульти-агентную команду** (PM + аналитик + разработчик + QA) на CrewAI | 🟡 | 9 мин |
| [Туториал 3: агент-программист](agents/tutorials/03-coding-agent.ru.md) | 🛠️ **Создам coding-агента** на LangGraph: читает код, правит баги, пишет тесты, создаёт PR | 🔴 | 7 мин |
| [Архитектура AI-агента](agents/architecture.ru.md) | Пойму, из чего состоит агент: LLM → инструменты → цикл решений → память | 🔵 | 11 мин |
| [Паттерны AI-агентов](agents/patterns.ru.md) | Изучу архитектурные паттерны: ReAct, Plan-and-Execute, Reflection, Multi-Agent | 🔵 | 14 мин |
| [Фреймворки для агентов](agents/frameworks.ru.md) | Сравню LangGraph, CrewAI, Agno и выберу под свою задачу | 🔵 | 12 мин |
| [Мульти-агентные системы](agents/multi-agent.ru.md) | Разберусь с коммуникацией и координацией между агентами | 🔵 | 18 мин |
| [Память агентов](agents/memory.ru.md) | Настрою долговременную память: контекст, хранение, извлечение | 🔵 | 9 мин |
| [Промптинг для агентов](agents/prompting.ru.md) | Научусь писать system prompts, которые управляют поведением агента | 🔵 | 11 мин |
| [Ollama для агентов](agents/ollama-for-agents.ru.md) | Освою tool calling, JSON schema и structured output через Ollama API | 🔵 | 10 мин |
| [Безопасность агентов](agents/safety.ru.md) | Защищу агента: guardrails, лимиты, изоляция, валидация | 🔵 | 16 мин |
| [Тестирование агентов](agents/evaluation.ru.md) | Научусь оценивать качество агента, отлавливать регрессии | 🔵 | 10 мин |

**Дальше:** Построили агента? → [Use Cases](#-use-cases--применить-на-практике) — применить в реальном проекте  
Надо подобрать модель под агента? → [Локальные модели — модели для разных задач](local-models/models.ru.md)

---

### 💼 Use Cases — применить на практике

**Где:** `use-cases/` · **Время:** ~30 мин · **Результат:** применяю AI для кодинга, работы с документами, контента и автоматизации

| Кейс | Что узнаю / сделаю | Инструменты | Время |
|------|-------------------|-------------|-------|
| [AI для кодинга](use-cases/coding.ru.md) | Настрою бесплатный Copilot на локальной модели: Continue.dev + Aider + OpenHands | Qwen 2.5 Coder, Ollama | 5 мин |
| [RAG на своих документах](use-cases/rag.ru.md) | Сделаю Q&A по своим файлам без отправки в облако: от Open WebUI до своего Python-пайплайна | ChromaDB, Ollama, nomic-embed | 7 мин |
| [AI + автоматизация](use-cases/automation.ru.md) | Свяжу локальные модели с бизнес-процессами: cron, n8n, Telegram-бот | Ollama API, Python | 7 мин |
| [Письмо и контент](use-cases/writing.ru.md) | Построю контент-пайплайн: генерация, рефлексия, массовый выпуск текстов | Qwen, Reflection-агент | 6 мин |

**Дальше:** Не хватает возможностей готовых решений? → [AI-агенты](#-ai-агенты--построить-своего-помощника)  
Нужна другая модель под кейс? → [Каталог моделей](local-models/catalog.ru.md)

---

### 📖 Ресурсы — найти сообщества и курсы

**Где:** `resources/` · **Время:** ~15 мин · **Результат:** знаю, где учиться, с кем обсуждать, за кем следить

| Раздел | Что найду | Для кого | Время |
|--------|-----------|----------|-------|
| [Русскоязычные ресурсы](resources/ru.ru.md) | Telegram-каналы, YouTube, блоги по AI на русском | 🟢 Все | 3 мин |
| [Сообщества](resources/communities.ru.md) | Discord-серверы, Reddit, Telegram-чаты для обсуждения AI | 🟢 Все | 3 мин |
| [Курсы](resources/courses.ru.md) | Бесплатные образовательные программы: от новичка до профи | 🟢🟡 | 3 мин |
| [Англоязычные ресурсы](resources/en.ru.md) | Awesome-списки, рассылки, подкасты, блоги лидеров индустрии | 🟡 | 3 мин |

**Дальше:** Нашли интересное? → [Вернуться к оглавлению](#-оглавление) и выбрать раздел

---

## 📂 Структура проекта

Справочно — как файлы лежат на диске. Основная навигация — [оглавление выше](#-оглавление).

```
awesome-ai-handbook/                          # 51 файл
│
├── README.ru.md                              ← вы здесь
├── README.md                                # английская версия (навигация)
│
├── basics/                                   # 🟢 нулевой уровень (10 файлов)
│   ├── README.ru.md                          #   три дорожки: 5 мин / 30 мин / 2 ч
│   ├── what-is-ai.ru.md                      #   AI, ML, LLM — без формул
│   ├── how-models-work.ru.md                 #   нейросети, трансформеры, токены
│   ├── cloud-vs-local.ru.md                  #   ChatGPT vs локально
│   ├── hardware-guide.ru.md                  #   RAM, VRAM, квантование
│   ├── glossary.ru.md                        #   35 терминов (3 уровня)
│   ├── faq.ru.md                             #   15+ частых вопросов
│   ├── learning-path.ru.md                   #   10 шагов от чата до агента
│   ├── setup-windows.ru.md                   #   установка на Windows
│   └── setup-linux.ru.md                     #   установка на Linux
│
├── local-models/                             # 🔵 локальный инференс (11 файлов)
│   ├── README.ru.md                          #   сценарии: новичок → профи
│   ├── getting-started.ru.md                 #   🟢 полный гайд для Mac (400 строк)
│   ├── running-models.ru.md                  #   практикум: от Ollama до HuggingFace
│   ├── models.ru.md                          #   выбор под задачу
│   ├── catalog.ru.md                         #   50+ моделей с характеристиками
│   ├── quantization.ru.md                    #   Q4, Q5, Q8 — что выбрать
│   ├── memory-and-context.ru.md              #   RAM, KV cache, контекст
│   ├── tools.ru.md                           #   сравнение Ollama, LM Studio, MLX, llama.cpp
│   ├── advanced-setup.ru.md                  #   🔵 Modelfile, API, env vars
│   ├── troubleshooting.ru.md                 #   частые проблемы
│   └── benchmarks/
│       └── apple-silicon.ru.md               #   🔵 tok/s на M1–M4
│
├── agents/                                   # 🤖 AI-агенты (15 файлов)
│   ├── README.ru.md                          #   обзор + минимальный код агента
│   ├── ready-made.ru.md                      #   🟢 каталог готовых AI-агентов
│   ├── skills.ru.md                          #   🟡 скиллы, MCP, плагины
│   ├── architecture.ru.md                    #   🔵 из чего состоит агент
│   ├── patterns.ru.md                        #   🔵 ReAct, Multi-Agent, Reflection
│   ├── frameworks.ru.md                      #   🔵 LangGraph, CrewAI, Agno
│   ├── multi-agent.ru.md                     #   🔵 коммуникация, координация
│   ├── memory.ru.md                          #   🔵 долговременная память
│   ├── prompting.ru.md                       #   🔵 system prompts для агентов
│   ├── ollama-for-agents.ru.md              #   🔵 tool calling, JSON schema
│   ├── safety.ru.md                          #   🔵 безопасность, guardrails
│   ├── evaluation.ru.md                      #   🔵 тестирование, регрессии
│   └── tutorials/
│       ├── README.ru.md                      #   индекс туториалов
│       ├── 01-first-agent.ru.md              #   🟢 первый агент за 30 мин
│       ├── 02-agent-team.ru.md               #   🟡 команда агентов
│       └── 03-coding-agent.ru.md             #   🔴 агент-программист
│
├── use-cases/                                # 💼 применение (5 файлов)
│   ├── README.ru.md                          #   кейсы: кодинг, RAG, автоматизация, контент
│   ├── coding.ru.md                          #   🟡 Continue, Aider, OpenHands
│   ├── rag.ru.md                             #   🟡 Open WebUI, ChromaDB, PrivateGPT
│   ├── automation.ru.md                      #   🟡 n8n, cron, Telegram-бот
│   └── writing.ru.md                         #   🟡 контент-пайплайн, Reflection
│
├── resources/                                # 📖 сообщества и курсы (5 файлов)
│   ├── README.ru.md                          #   индекс
│   ├── ru.ru.md                              #   🟢 русскоязычные ресурсы
│   ├── communities.ru.md                     #   🟢 Discord, Reddit, Telegram
│   ├── courses.ru.md                         #   🟢🟡 бесплатные курсы
│   └── en.ru.md                              #   🟡 англоязычные ресурсы
│
├── CONTRIBUTING.ru.md                        # руководство контрибьютора
├── CODE_OF_CONDUCT.ru.md                     # кодекс поведения
└── SECURITY.ru.md                            # политика безопасности
```

---

## 📄 Быстрый поиск по файлам

Компактная таблица всех файлов. Знаете, что ищете? — нашли за 10 секунд.

### basics/ — нулевой уровень
| Файл | О чём | Ур. |
|------|-------|:---:|
| [README](basics/README.ru.md) | Три дорожки обучения | — |
| [what-is-ai](basics/what-is-ai.ru.md) | AI, ML, LLM — без математики | 🟢 |
| [how-models-work](basics/how-models-work.ru.md) | Нейросети на аналогиях | 🟢 |
| [cloud-vs-local](basics/cloud-vs-local.ru.md) | ChatGPT vs локально | 🟢 |
| [hardware-guide](basics/hardware-guide.ru.md) | Какое железо нужно | 🟢 |
| [glossary](basics/glossary.ru.md) | Словарь: 35 терминов | 🟢 |
| [faq](basics/faq.ru.md) | 15+ вопросов новичков | 🟢 |
| [learning-path](basics/learning-path.ru.md) | План: от чата до агента | 🟢 |
| [setup-windows](basics/setup-windows.ru.md) | Установка на Windows | 🟢 |
| [setup-linux](basics/setup-linux.ru.md) | Установка на Linux | 🟢 |

### local-models/ — локальный инференс
| Файл | О чём | Ур. |
|------|-------|:---:|
| [README](local-models/README.ru.md) | Сценарии: новичок → профи | — |
| [getting-started](local-models/getting-started.ru.md) | Полный гайд установки (Mac) | 🟢 |
| [running-models](local-models/running-models.ru.md) | Запуск: Ollama, LM Studio, HF | 🟢🟡 |
| [models](local-models/models.ru.md) | Выбор модели под задачу | 🟡 |
| [catalog](local-models/catalog.ru.md) | 50+ моделей с характеристиками | 🟡 |
| [quantization](local-models/quantization.ru.md) | Q4, Q5, Q8 — сжатие моделей | 🟡 |
| [memory-and-context](local-models/memory-and-context.ru.md) | RAM, KV cache, контекст | 🟡 |
| [tools](local-models/tools.ru.md) | Сравнение движков инференса | 🟡 |
| [advanced-setup](local-models/advanced-setup.ru.md) | Modelfile, API, env vars | 🔵 |
| [troubleshooting](local-models/troubleshooting.ru.md) | Частые проблемы | 🟡 |
| [apple-silicon](local-models/benchmarks/apple-silicon.ru.md) | Бенчмарки M1–M4 | 🔵 |

### agents/ — AI-агенты
| Файл | О чём | Ур. |
|------|-------|:---:|
| [README](agents/README.ru.md) | Обзор + минимальный код агента | — |
| [ready-made](agents/ready-made.ru.md) | 🟢 Каталог готовых AI-агентов: Aider, Cursor, OpenHands | 🟢 |
| [skills](agents/skills.ru.md) | 🟡 Скиллы, MCP-серверы, плагины, как создать свой | 🟡 |
| [architecture](agents/architecture.ru.md) | Из чего состоит агент | 🔵 |
| [patterns](agents/patterns.ru.md) | ReAct, Multi-Agent, Reflection | 🔵 |
| [frameworks](agents/frameworks.ru.md) | LangGraph, CrewAI, Agno | 🔵 |
| [multi-agent](agents/multi-agent.ru.md) | Мульти-агентные системы | 🔵 |
| [memory](agents/memory.ru.md) | Память и состояние агента | 🔵 |
| [prompting](agents/prompting.ru.md) | System prompts для агентов | 🔵 |
| [ollama-for-agents](agents/ollama-for-agents.ru.md) | Tool calling, JSON schema | 🔵 |
| [safety](agents/safety.ru.md) | Безопасность, guardrails | 🔵 |
| [evaluation](agents/evaluation.ru.md) | Тестирование, регрессии | 🔵 |
| [01-first-agent](agents/tutorials/01-first-agent.ru.md) | 🛠️ ReAct-агент на Python | 🟢 |
| [02-agent-team](agents/tutorials/02-agent-team.ru.md) | 🛠️ Команда агентов (CrewAI) | 🟡 |
| [03-coding-agent](agents/tutorials/03-coding-agent.ru.md) | 🛠️ Coding-агент (LangGraph) | 🔴 |

### use-cases/ — применение
| Файл | О чём | Ур. |
|------|-------|:---:|
| [README](use-cases/README.ru.md) | Кейсы: кодинг, RAG, автоматизация | — |
| [coding](use-cases/coding.ru.md) | Continue, Aider, OpenHands | 🟡 |
| [rag](use-cases/rag.ru.md) | RAG на своих документах | 🟡 |
| [automation](use-cases/automation.ru.md) | n8n, cron, Telegram-бот | 🟡 |
| [writing](use-cases/writing.ru.md) | Контент-пайплайн, Reflection | 🟡 |

### resources/ — сообщества и курсы
| Файл | О чём | Ур. |
|------|-------|:---:|
| [README](resources/README.ru.md) | Индекс ресурсов | — |
| [ru](resources/ru.ru.md) | Русскоязычные: Telegram, YouTube, блоги | 🟢 |
| [communities](resources/communities.ru.md) | Discord, Reddit, Telegram-чаты | 🟢 |
| [courses](resources/courses.ru.md) | Бесплатные курсы по AI | 🟢🟡 |
| [en](resources/en.ru.md) | Англоязычные: awesome-списки, подкасты | 🟡 |

### Корневые файлы
| Файл | О чём |
|------|-------|
| [CONTRIBUTING](CONTRIBUTING.ru.md) | Как помочь проекту |
| [CODE_OF_CONDUCT](CODE_OF_CONDUCT.ru.md) | Правила сообщества |
| [SECURITY](SECURITY.ru.md) | Политика безопасности |

---

## 🤝 Контрибьютинг

Приветствуются бенчмарки, RU-ресурсы, туториалы и исправления.  
Подробности — в [CONTRIBUTING.md](CONTRIBUTING.md).

---

## 📄 Лицензия

[MIT](LICENSE)

---

*Этот проект создан для того, чтобы каждый мог расширить свой кругозор в мире AI — независимо от технического уровня и железа.*
