# 🧠 Awesome AI Handbook

> Практический справочник по AI: от локальных моделей до своих агентов.
> Не просто ссылки, а готовые руководства, бенчмарки и проверенные подходы.

**[English](README.md)** | **Русский**

---

## 📋 Содержание

- [О проекте](#-о-проекте)
- [Быстрый старт](#-быстрый-старт-5-минут)
- [Что влезет в RAM](#-что-влезет-в-вашу-ram)
- [Лучшие модели по задачам](#-лучшие-модели-по-задачам-ориентир-на-m1-16gb)
- [Инструменты запуска](#-сравнение-инструментов)
- [Квантизация](#-квантизация-в-двух-словах)
- [Частые проблемы](#-частые-проблемы-и-решения)
- [AI-агенты](#-ai-агенты)
- [Практические кейсы](#-практические-кейсы)
- [Ресурсы](#-ресурсы)
- [Контрибьютинг](#-как-контрибьютить)

---

## 📌 О проекте

**Миссия**: помочь каждому расширить кругозор в мире AI — от первого запуска модели локально до создания собственного агента.

**Чем отличается от других awesome-списков:**
- 🔧 **Практика, а не ссылки** — пошаговые гайды с кодом
- 💻 **Реальные бенчмарки** — цифры для конкретного железа (MacBook Air M1 16GB и не только)
- 🤖 **Создание агентов** — туториалы «от идеи до работающего прототипа» *(скоро)*
- 🌍 **Два языка** — английский + русский, заполняем вакуум для RU-сообщества

**Для кого:**
- Разработчики, которые хотят запускать AI локально (приватность, оффлайн, бесплатно)
- Те, кто хочет создать своего AI-агента, а не только использовать готовый
- Все, кто хочет разобраться в AI без глубокого погружения в математику

---

## 🚀 Быстрый старт (5 минут)

```bash
# 1. Установка Ollama
brew install ollama    # macOS
# Linux: curl -fsSL https://ollama.com/install.sh | sh

# 2. Запуск модели (автоматически скачает)
ollama run qwen3:8b

# 3. Готово! Спросите что-нибудь.
```

---

## 💾 Что влезет в вашу RAM

| RAM | Доступно для модели | Комфортные модели | Максимум (впритык) |
|---|---|---|---|
| **8 GB** | ~3–4 GB | 0.8B–3B Q4 | 7B Q4 (впритык, swap) |
| **16 GB** ⭐ | ~10–11 GB | 3B–8B Q4/Q5 | 9B Q4, 14B Q4 (впритык) |
| **24 GB** | ~17 GB | 8B–14B | 32B Q4 (впритык) |
| **32 GB** | ~24 GB | 14B–32B Q4 | 70B Q3 (с потерями) |
| **64 GB** | ~56 GB | 32B–70B Q4 | 70B Q5/Q6 |

> **Правило**: вычтите 5–6 GB из общего объёма RAM на macOS + системные процессы.

---

## 🎯 Лучшие модели по задачам (ориентир на M1 16GB)

| Задача | Модель | Квант. | Размер | Скорость | Ссылка |
|---|---|---|---|---|---|
| **Повседневный чат** ⭐ | Qwen 3.5 9B | Q4_K_M | ~7.0 GB | 10–13 tok/s | [HuggingFace](https://huggingface.co/Qwen/Qwen3-9B) / [Ollama](https://ollama.com/library/qwen3:9b) |
| **Максимальная скорость** ⭐ | Qwen 3.5 4B | Q4_K_M | ~3.5 GB | 28–35 tok/s | [HuggingFace](https://huggingface.co/Qwen/Qwen3-4B) / [Ollama](https://ollama.com/library/qwen3:4b) |
| **Кодинг** ⭐ | Qwen 2.5 Coder 7B | Q4_K_M | ~4.7 GB | 22–25 tok/s | [HuggingFace](https://huggingface.co/Qwen/Qwen2.5-Coder-7B) / [Ollama](https://ollama.com/library/qwen2.5-coder:7b) |
| **Письмо / контент** ⭐ | Llama 3.3 8B | Q4_K_M | ~4.9 GB | 14–18 tok/s | [HuggingFace](https://huggingface.co/meta-llama/Llama-3.3-8B-Instruct) / [Ollama](https://ollama.com/library/llama3.3:8b) |
| **Математика / reasoning** ⭐ | Phi-4 Mini | Q4_K_M | ~2.3 GB | 25–30 tok/s | [HuggingFace](https://huggingface.co/microsoft/Phi-4-mini-instruct) / [Ollama](https://ollama.com/library/phi4-mini) |
| **Мультимодальная (зрение)** ⭐ | Gemma 4 E4B | Q4_K_M | ~2.5 GB | 30–38 tok/s | [HuggingFace](https://huggingface.co/google/gemma-3-4b-it) / [Ollama](https://ollama.com/library/gemma3:4b) |
| **Tool calling / агенты** ⭐ | LFM2.5 8B-A1B | Q4_K_M | ~5.5 GB | 35–45 tok/s | [HuggingFace](https://huggingface.co/LiquidAI/LFM2-8B-A1B) |
| **Длинный контекст** ⭐ | Qwen 3.5 9B | Q4_K_M | ~7.0 GB | 10–13 tok/s | [HuggingFace](https://huggingface.co/Qwen/Qwen3-9B) / [Ollama](https://ollama.com/library/qwen3:9b) |

> 💡 **Примечание**: Бенчмарки сняты на MacBook Air M1 16GB. Для NVIDIA GPU / Windows / Linux производительность будет другой — см. сравнение чипов ниже.

---

## 🛠 Сравнение инструментов

| Инструмент | Интерфейс | Open Source | Apple Silicon | Для кого | GitHub |
|---|---|---|---|---|---|
| **Ollama** ⭐ | CLI + API | ✅ MIT | Metal | Разработчики, интеграции | [ollama/ollama](https://github.com/ollama/ollama) ⭐ 148K |
| **LM Studio** ⭐ | GUI + CLI | ❌ | MLX авто | Начинающие, поиск моделей | [lmstudio-ai/lmstudio](https://github.com/lmstudio-ai/lmstudio) |
| **mlx-lm** | Python / CLI | ✅ Apache | Нативно | Макс. скорость на Mac | [ml-explore/mlx](https://github.com/ml-explore/mlx) |
| **llama.cpp** | CLI | ✅ MIT | Metal | Полный контроль | [ggml-org/llama.cpp](https://github.com/ggml-org/llama.cpp) ⭐ 70K+ |
| **GPT4All** | GUI | ✅ | Metal | RAG, документы | [nomic-ai/gpt4all](https://github.com/nomic-ai/gpt4all) |
| **Jan** | GUI | ❌ | Metal | Приватность, десктоп | [janhq/jan](https://github.com/janhq/jan) ⭐ 35K |
| **Enchanted** | macOS GUI | ❌ | MLX нативно | Нативное macOS‑приложение | [AugustDev/Enchanted](https://github.com/AugustDev/Enchanted) |

> **Рекомендация для M1 16GB**: LM Studio (для поиска/тестов) + Ollama (для работы и интеграций).

---

## 📦 Квантизация в двух словах

| Формат | Бит | Размер 8B | Качество | Когда использовать |
|---|---|---|---|---|
| Q4_K_M ⭐ | ~4.8 | ~5.2 GB | Отличное | **По умолчанию** |
| Q5_K_M | ~5.5 | ~6.1 GB | Отличное+ | Письмо, аналитика |
| Q6_K | ~6.0 | ~6.6 GB | Почти FP16 | Код, важна точность |
| Q8_0 | 8.0 | ~8.5 GB | ≈ FP16 | Если RAM позволяет |

> **Q4_K_M ≠ MLX 4-bit**: Q4_K_M использует смешанную точность (4.83 бит среднее) и даёт perplexity в 4.7x лучше, чем униформный 4-bit. Подробнее — в [блоге llama.cpp](https://github.com/ggml-org/llama.cpp/discussions/2544).

---

## ⚠️ Частые проблемы и решения

| Проблема | Причина | Решение |
|---|---|---|
| 2–5 tok/s (очень медленно) | Модель не влезает в RAM → swap | Уменьшите модель/квантизацию |
| Metal не используется | Ollama не на Metal | `ollama ps` — проверьте backend |
| Jetsam убивает Ollama | macOS защищает память | Меньше модель, меньше контекст |
| Крашится на длинных ответах | Нехватка памяти KV cache | Уменьшите `num_ctx` (4096 достаточно) |
| Скорость падает после Chrome | Браузер ест 2–4 GB | Закройте браузер |

---

## 🤖 AI-агенты

### Что такое AI-агент

AI-агент = LLM + память + инструменты + цикл принятия решений.
В отличие от обычного чата, агент может:
- Планировать действия
- Использовать инструменты (поиск, код, API)
- Помнить контекст между шагами
- Анализировать и улучшать свои результаты

### Паттерны агентов

| Паттерн | Описание | Когда использовать |
|---|---|---|
| **ReAct** | Reasoning + Acting по очереди | Простые задачи с инструментами |
| **Plan-and-Execute** | Сначала план, потом выполнение | Сложные многошаговые задачи |
| **Multi-Agent** | Несколько агентов с разными ролями | Большие задачи, разделение ответственности |
| **Reflection** | Агент проверяет и улучшает свой вывод | Когда важно качество |
| **Tool Use** | Агент выбирает и вызывает инструменты | Когда нужны внешние данные/действия |

### Фреймворки для создания агентов

| Фреймворк | ⭐ | Язык | Ключевая особенность | Для чего лучше всего | GitHub |
|---|---|---|---|---|---|
| **LangGraph** | 10K+ | Python | Графовые агенты, stateful | Сложные workflow, циклы | [langchain-ai/langgraph](https://github.com/langchain-ai/langgraph) |
| **CrewAI** | 25K+ | Python | Ролевые мульти-агенты | Команды агентов с ролями | [joaomdmoura/crewAI](https://github.com/joaomdmoura/crewAI) |
| **AutoGen** (Microsoft) | 48K+ | Python | Мульти-агентный диалог | Исследования, сложные задачи | [microsoft/autogen](https://github.com/microsoft/autogen) |
| **Agno** | 15K+ | Python | Быстрый старт, простой API | Начинающие, прототипы | [agno-ai/agno](https://github.com/agno-ai/agno) |
| **OpenAI Agents SDK** | — | Python | Официальный, Responses API | Экосистема OpenAI | [openai/agents-sdk](https://github.com/openai/agents-sdk) |
| **Claude Agent SDK** | — | TypeScript | Официальный Anthropic | Экосистема Claude | [anthropics/claude-agent-sdk](https://github.com/anthropics/claude-agent-sdk) |
| **Mastra** | 10K+ | TypeScript | TypeScript-first | JS/TS разработчики | [mastra-ai/mastra](https://github.com/mastra-ai/mastra) |
| **Agency Swarm** | 3.7K | Python | AI-агентства | Автоматизация агентств | [VRSEN/agency-swarm](https://github.com/VRSEN/agency-swarm) |

**Как выбрать:**
- **Первый агент / прототип** → Agno или OpenAI Agents SDK
- **Сложный workflow с циклами** → LangGraph
- **Команда агентов с ролями** → CrewAI
- **Исследования / мульти-агентные диалоги** → AutoGen
- **TypeScript‑проект** → Mastra или Claude Agent SDK

### Туториалы

> 🚧 **В разработке** — раздел наполняется.
> Планируемые туториалы:
> - Первый агент за 30 минут
> - Агент для кодинга (чтение репозитория, правки, тесты)
> - Мульти-агентная система (писатель + редактор)

---

## 💼 Практические кейсы

### AI для кодинга

**Инструменты:**
- [**Continue**](https://github.com/continuedev/continue) (IDE‑плагин) + локальная модель = бесплатный Copilot
- [**Cursor**](https://cursor.sh/) — AI‑IDE из коробки
- [**Aider**](https://github.com/paul-gauthier/aider) — CLI‑агент для кодинга
- [**OpenHands**](https://github.com/All-Hands-AI/OpenHands) — автономный software engineer

**Связка для M1 16GB:**
```
Continue IDE → Ollama (localhost:11434) → Qwen 2.5 Coder 7B
```

**Пример `config.json` для Continue:**
```json
{
  "models": [
    {
      "title": "Qwen 2.5 Coder 7B (local)",
      "provider": "ollama",
      "model": "qwen2.5-coder:7b",
      "apiBase": "http://localhost:11434"
    }
  ],
  "tabAutocompleteModel": {
    "title": "Qwen 2.5 Coder 1.5B (fast)",
    "provider": "ollama",
    "model": "qwen2.5-coder:1.5b",
    "apiBase": "http://localhost:11434"
  }
}
```

### RAG на своих документах

**Стек:**
- Ollama (модель) + [**Open WebUI**](https://github.com/open-webui/open-webui) (интерфейс) + коллекция документов
- Или: [**PrivateGPT**](https://github.com/zylon-ai/private-gpt) — всё в одном

**Применение:**
- Q&A по внутренней документации компании
- Поиск по личным заметкам (Obsidian, Notion экспорт)
- Анализ юридических / медицинских документов

### AI + автоматизация

**Связки:**
- AI‑агент + [**n8n**](https://github.com/n8n-io/n8n) / [**Make**](https://www.make.com/) / [**Zapier**](https://zapier.com/) = автоматизация бизнес‑процессов
- AI + cron‑скрипты = регулярная обработка данных
- AI + email = автоматические ответы и саммари

---

## 📖 Ресурсы

### Русскоязычные ресурсы

> ⚠️ **Голубой океан** — курируемых списков русскоязычных AI‑ресурсов на GitHub практически нет.
> Мы собираем этот раздел с нуля. Знаете хорошие RU‑ресурсы — [контрибутьте!](#как-контрибьютить)

**Планируемые категории:**
- Telegram‑каналы и чаты
- Статьи на Хабре
- YouTube‑каналы
- Подкасты
- Конференции и митапы

> 🚧 **Скоро** — собираем лучшие русскоязычные AI‑ресурсы.

### Англоязычные ресурсы

**Reddit:**
- [r/LocalLLaMA](https://reddit.com/r/LocalLLaMA) — локальный запуск моделей (200K+ участников)
- [r/MachineLearning](https://reddit.com/r/MachineLearning) — исследования
- [r/artificial](https://reddit.com/r/artificial) — новости AI

**Сообщества:**
- [Hugging Face Discord](https://discord.gg/hugging-face)
- [Ollama Discord](https://discord.gg/ollama)
- [LangChain Discord](https://discord.gg/langchain)

**Курсы (бесплатные):**
- [DeepLearning.AI](https://www.deeplearning.ai/) — короткие курсы от Andrew Ng
- [Hugging Face NLP Course](https://huggingface.co/learn/nlp-course)
- [FatAI](https://www.fast.ai/) — практико‑ориентированный подход

**Рассылки:**
- [The Batch](https://www.deeplearning.ai/the-batch/) (DeepLearning.AI) — еженедельно
- [Import AI](https://importai.substack.com/) (Jack Clark) — аналитика
- [The Gradient](https://thegradient.pub/) — глубокие разборы

---

## 🤝 Как контрибьютить

Приветствуются:
- 🇷🇺 **Русскоязычные ресурсы** (каналы, статьи, курсы, подкасты)
- 📊 **Бенчмарки** на вашем железе (любой чип / GPU)
- 📝 **Черновики туториалов** (Markdown + код)
- 🐛 **Исправления и обновления** (ссылки устарели, версии изменились)

**Правила:**
1. Каждая ссылка — с кратким описанием «зачем это полезно»
2. Минимум: проект должен быть активным (коммиты за последние 3–6 месяцев)
3. Категоризация: PR в соответствующий файл раздела
4. Пометка `last-verified: YYYY-MM-DD` в описании ссылки

> Подробнее — в [CONTRIBUTING.md](CONTRIBUTING.md) (будет создан при запуске репо).

---

## 📄 Лицензия

Проект распространяется под лицензией **MIT** — свободно используйте, модифицируйте, делитесь.
Файл лицензии будет добавлен при создании репозитория.

---

*Этот проект создан для того, чтобы каждый мог расширить свой кругозор в мире AI — независимо от технического уровня и железа.*