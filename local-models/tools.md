# 🛠 Полное сравнение инструментов локального инференса

> Дата данных: 23 июля 2026. Все инструменты актуальны, версии указаны.

---

## Содержание

1. [Каталог инструментов](#1-каталог-инструментов)
2. [Сценарии: какой инструмент выбрать](#2-сценарии-какой-инструмент-выбрать)
3. [Бенчмарки на Apple Silicon](#3-бенчмарки-на-apple-silicon)
4. [Масштабирование и production](#4-масштабирование)
5. [Гайды по установке](#5-гайды-по-установке)
6. [Decision Tree](#6-decision-tree)

---

## 1. Каталог инструментов

### 1.1 Ollama ⭐ — стандарт для разработчиков

| Параметр | Значение |
|----------|----------|
| **Интерфейс** | CLI + HTTP API |
| **Open Source** | ✅ MIT |
| **Звёзд** | 148K+ |
| **Apple Silicon** | Metal (native) + MLX (c v0.19, 32GB+ Mac) |
| **GPU support** | Metal (Mac), CUDA, ROCm, Vulkan |
| **GitHub** | [ollama/ollama](https://github.com/ollama/ollama) |

**Как работает под капотом:**
- Обёртка над llama.cpp с автоматической загрузкой моделей
- Модели хранятся в `~/.ollama/models/blobs/` в формате GGUF
- При запуске: проверяет модель локально → если нет, скачивает → загружает в RAM → запускает llama.cpp backend
- HTTP API на порту 11434, совместимый с OpenAI API

**GPU Support Matrix:**

| Платформа | Backend | Статус |
|-----------|---------|--------|
| Apple Silicon | Metal | ✅ Нативный |
| Apple Silicon | MLX | ✅ (v0.19+, только 32GB+ RAM, не все модели) |
| NVIDIA Linux | CUDA | ✅ |
| NVIDIA Windows | CUDA | ✅ |
| AMD Linux | ROCm | ✅ |
| AMD Windows | ROCm | ⚠️ Экспериментальный |
| Intel GPU | Vulkan | ⚠️ Ограниченно |
| CPU only | llama.cpp | ✅ Всегда fallback |

**Ключевые возможности:**
- `Modelfile` — кастомные модели (температура, системный промпт, template)
- OpenAI API совместимость (`/api/chat`, `/api/generate`, `/api/embeddings`)
- Multimodal (поддерживает vision-модели)
- Ollama 0.19+ — автоматический MLX backend для совместимых моделей
- `OLLAMA_KV_CACHE_TYPE` — управление квантизацией KV cache
- `OLLAMA_NUM_PARALLEL` — параллельные запросы
- `OLLAMA_MAX_LOADED_MODELS` — несколько моделей в памяти

**Когда выбрать Ollama:**
- ✅ Нужен простой API для интеграции (Continue.dev, Aider, Open WebUI)
- ✅ Хотите одну команду `ollama run <model>` без конфигов
- ✅ Нужна кроссплатформенность (Mac + Linux + Windows)
- ❌ Нужна максимальная скорость (MLX быстрее на 20-40%)
- ❌ Нужна тонкая настройка инференса (llama.cpp даёт больше контроля)

#### 📡 API Endpoints

Ollama предоставляет HTTP API на порту 11434. Все запросы — POST, ответы в JSON.

| Endpoint | Метод | Описание |
|---|---|---|
| `/api/chat` | POST | Чат-комплейшен (streaming) |
| `/api/generate` | POST | Генерация текста (completion) |
| `/api/embed` | POST | Эмбеддинги (новый API, заменяет `/api/embeddings`) |
| `/api/embeddings` | POST | Эмбеддинги (deprecated) |
| `/api/tags` | GET | Список локальных моделей |
| `/api/ps` | GET | Список загруженных моделей |
| `/api/show` | POST | Информация о модели |
| `/api/create` | POST | Создание из Modelfile |
| `/api/pull` | POST | Скачивание модели |
| `/api/push` | POST | Загрузка в реестр |
| `/api/copy` | POST | Копирование модели |
| `/api/delete` | DELETE | Удаление модели |
| `/api/version` | GET | Версия Ollama |
| `/api/blobs/:digest` | HEAD/POST | Проверка/создание blob'а |
| `/api/experimental/web_search` | POST | Веб-поиск (экспериментально) |
| `/api/experimental/web_fetch` | POST | Получение страниц (экспериментально) |

**Базовые примеры:**

```bash
# Generate (без streaming)
curl http://localhost:11434/api/generate -d '{
  "model": "qwen3.5:4b",
  "prompt": "What is the meaning of life?",
  "stream": false
}'

# Chat
curl http://localhost:11434/api/chat -d '{
  "model": "qwen3.5:4b",
  "messages": [{"role": "user", "content": "Привет!"}],
  "stream": false
}'

# JSON mode
curl http://localhost:11434/api/chat -d '{
  "model": "qwen3.5:4b",
  "messages": [{"role": "user", "content": "Извлеки имя из: Меня зовут Иван"}],
  "format": "json",
  "stream": false
}'

# Список установленных моделей
curl http://localhost:11434/api/tags

# Загруженные модели (память, процессор)
curl http://localhost:11434/api/ps

# Выгрузить модель из памяти
curl http://localhost:11434/api/generate -d '{
  "model": "qwen3.5:4b",
  "keep_alive": 0
}'

# Эмбеддинги
curl http://localhost:11434/api/embed -d '{
  "model": "all-minilm",
  "input": ["текст для векторизации"]
}'
```

**Structured output (JSON Schema):**

```bash
curl http://localhost:11434/api/chat -d '{
  "model": "qwen3.5:4b",
  "messages": [{"role": "user", "content": "Извлеки данные: Иван, 25, Москва"}],
  "format": {
    "type": "object",
    "properties": {
      "name": {"type": "string"},
      "age": {"type": "integer"},
      "city": {"type": "string"}
    },
    "required": ["name", "age", "city"]
  },
  "stream": false
}'
```

#### 🤖 OpenAI-совместимые эндпоинты

Любая библиотека для OpenAI подключается к Ollama сменой `base_url`:

| Endpoint | Описание |
|---|---|
| `/v1/chat/completions` | Chat (OpenAI формат) |
| `/v1/completions` | Completion (OpenAI формат) |
| `/v1/embeddings` | Embeddings (OpenAI формат) |
| `/v1/models` | Список моделей |

```python
from openai import OpenAI

client = OpenAI(
    base_url="http://localhost:11434/v1",
    api_key="ollama"  # любое значение
)

response = client.chat.completions.create(
    model="qwen3.5:4b",
    messages=[{"role": "user", "content": "Привет!"}]
)
print(response.choices[0].message.content)
```

```bash
# curl-версия
curl http://localhost:11434/v1/chat/completions -d '{
  "model": "qwen3.5:4b",
  "messages": [{"role": "user", "content": "Привет!"}]
}'
```

#### 📝 Modelfile Parameters

Modelfile — конфигурация модели в Ollama (аналог Dockerfile для LLM):

```dockerfile
FROM qwen3.5:4b
PARAMETER temperature 0.7
PARAMETER num_ctx 8192
SYSTEM "Ты профессиональный Python-разработчик."
```

**Все параметры PARAMETER:**

| Параметр | Дефолт | Описание |
|---|---|---|
| `num_ctx` | 2048* | Размер контекстного окна |
| `num_predict` | -1 (∞) | Макс. токенов генерации |
| `temperature` | 0.8 | «Креативность» ответов |
| `top_k` | 40 | Top-K сэмплирование |
| `top_p` | 0.9 | Nucleus сэмплирование |
| `min_p` | 0.0 | Min-P сэмплирование |
| `seed` | 0 | Seed (для воспроизводимости) |
| `stop` | — | Стоп-последовательности |
| `repeat_penalty` | 1.1 | Штраф повторов |
| `repeat_last_n` | 64 | Окно штрафа (0=выкл) |
| `num_batch` | auto | Размер batch |
| `num_thread` | auto | Количество потоков CPU |
| `numa` | false | NUMA (Linux, multi-socket) |
| `use_mmap` | true | Memory-mapped I/O |

> *`num_ctx` в Modelfile = 2048, но через API авто-выбирается по VRAM. На M1 16GB → 4096 (см. README).

**Другие инструкции Modelfile:**

| Инструкция | Описание |
|---|---|
| `FROM` | Базовая модель (обязательная) |
| `TEMPLATE` | Prompt-шаблон (Go template) |
| `SYSTEM` | Системное сообщение |
| `ADAPTER` | LoRA-адаптер |
| `LICENSE` | Лицензия |
| `MESSAGE` | Few-shot примеры |
| `DRAFT` | Draft-модель (speculative decoding) |
| `REQUIRES` | Минимальная версия Ollama |

#### 🔧 Environment Variables

Ключевые `OLLAMA_*` переменные для настройки:

| Переменная | Дефолт | Описание |
|---|---|---|
| `OLLAMA_HOST` | `127.0.0.1:11434` | IP и порт сервера |
| `OLLAMA_MODELS` | `~/.ollama/models` | Путь к моделям |
| `OLLAMA_KEEP_ALIVE` | `5m` | Время жизни модели в памяти |
| `OLLAMA_NUM_PARALLEL` | `1` | Параллельных запросов |
| `OLLAMA_MAX_LOADED_MODELS` | auto | Макс. моделей на GPU |
| `OLLAMA_LOAD_TIMEOUT` | `5m` | Таймаут загрузки |
| `OLLAMA_KV_CACHE_TYPE` | `f16` | Квантизация KV cache (`q8_0`, `q4_0`) |
| `OLLAMA_FLASH_ATTENTION` | false | Flash attention (экономия памяти) |
| `OLLAMA_GPU_OVERHEAD` | `0` | Резерв VRAM (байты) |
| `OLLAMA_MAX_QUEUE` | `512` | Макс. очередь запросов |
| `OLLAMA_DEBUG` | false | Режим отладки |
| `OLLAMA_NOHISTORY` | false | Отключить историю |
| `OLLAMA_NO_CLOUD` | false | Без облачных функций |

---

### 1.2 LM Studio — лучший GUI

| Параметр | Значение |
|----------|----------|
| **Интерфейс** | GUI + Headless daemon |
| **Open Source** | ❌ Закрытый исходник |
| **Apple Silicon** | MLX (автоопределение) + llama.cpp |
| **OS** | macOS, Windows, Linux |
| **Сайт** | [lmstudio.ai](https://lmstudio.ai) |

**Особенности:**
- Встроенный поиск моделей на HuggingFace (по категориям, размеру)
- Автоматический выбор бэкенда: MLX для Mac, CUDA для NVIDIA
- Возможность перетаскивать GGUF-файлы
- Встроенный чат с system prompt, template, параметрами
- Server mode: OpenAI-совместимый API
- Headless daemon с версии 0.3 (работает в фоне)

**LM Studio vs Ollama на M4 Pro 24GB (Qwen3-Coder-30B MoE):**

| Метрика | LM Studio (MLX) | Ollama (llama.cpp) | Разница |
|---------|----------------|-------------------|---------|
| Throughput | **102 tok/s** | 70 tok/s | **+46%** |
| TTFT | 291 ms | **175 ms** | Ollama быстрее |
| GPU Power | **12.4 W** | 15.4 W | **-20%** |
| Efficiency | **8.2 tok/s/W** | 4.5 tok/s/W | **+82%** |
| Process Memory | **21.4 GB** | 41.6 GB | **-49%** |

*(Источник: asiai.dev)*

**Когда выбрать LM Studio:**
- ✅ Только начинаете — не хотите CLI
- ✅ Хотите быстро тестировать разные модели
- ✅ Нужна максимальная скорость на Mac (MLX)
- ❌ Нужна автоматизация / CI (нет CLI)
- ❌ Нужен полный open source

---

### 1.3 MLX (mlx-lm) — фреймворк Apple

| Параметр | Значение |
|----------|----------|
| **Интерфейс** | Python API + CLI |
| **Open Source** | ✅ Apache 2.0 |
| **Apple Silicon** | Нативный (создан Apple) |
| **OS** | macOS только |
| **GitHub** | [ml-explore/mlx](https://github.com/ml-explore/mlx) ⭐ 18K+ |

**Что такое MLX:**
MLX — это **фреймворк машинного обучения от Apple** для Apple Silicon. Он включает:
- **mlx** — ядро (массивы, автоград, оптимизаторы)
- **mlx-lm** — надстройка для языковых моделей (инференс + fine-tuning)
- **mlx-examples** — примеры (LoRA, QLoRA, адаптеры)

**В отличие от llama.cpp:**
- Не обёртка, а нативный фреймворк для Apple Silicon
- Поддерживает **обучение** (fine-tuning, LoRA, QLoRA), а не только инференс
- В 2 раза быстрее llama.cpp на Mac (нативная оптимизация)
- Использует униформные квантизации (4-bit, 8-bit), не K-quants

**Производительность (M5 Max 128GB, Llama 3.1 70B Q4):**

| Движок | tok/s | Память |
|--------|-------|--------|
| **MLX** | **18** | ~39 GB |
| llama.cpp Metal | 14 | ~41 GB |
| Ollama (CPU) | 12 | ~41 GB |

**Fine-tuning на MLX:**
```python
from mlx_lm import load, generate
model, tokenizer = load("mlx-community/Qwen3-8B-4bit")

# LoRA fine-tuning
from mlx_lm import lora
lora.train_lora(
    model=model,
    tokenizer=tokenizer,
    train_set="data.jsonl",
    num_lora_layers=16,
    lora_rank=8,
)
```

**Когда выбрать MLX:**
- ✅ Максимальная скорость на Mac
- ✅ Нужен fine-tuning / LoRA / QLoRA
- ✅ Python-экосистема
- ❌ Только Mac (не кроссплатформенный)
- ❌ Нужен GUI

---

### 1.4 llama.cpp — полный контроль

| Параметр | Значение |
|----------|----------|
| **Интерфейс** | CLI + Server |
| **Open Source** | ✅ MIT |
| **Apple Silicon** | Metal (нативный) |
| **OS** | Все платформы |
| **GitHub** | [ggml-org/llama.cpp](https://github.com/ggml-org/llama.cpp) ⭐ 70K+ |

**Уникальные возможности:**
- GGUF формат: K-quants (Q4_K_M, Q5_K_M) и I-quants (IQ2_XXS, IQ3_XXS)
- Speculative decoding (в 1.5-2× быстрее)
- KV cache квантизация (q8_0, q4_0)
- Встроенный embedding endpoint
- Batch processing
- LoRA адаптеры на лету

**Speculative decoding (M3 Ultra 192GB, Llama 3.1 70B Q4):**

| Режим | tok/s | Ускорение |
|-------|-------|-----------|
| Direct | 9.4 | 1× |
| **Speculative (70B+70B)** | **11.3** | **1.2×** |
| **Speculative (70B+8B)** | **15.1** | **1.6×** |

**Когда выбрать llama.cpp:**
- ✅ Нужна кроссплатформенность (везде GGUF)
- ✅ Хотите полный контроль над инференсом
- ✅ Используете нестандартные квантизации
- ❌ Хотите простоту «одной команды»

---

### 1.5 vLLM — production serving

| Параметр | Значение |
|----------|----------|
| **Интерфейс** | HTTP API (OpenAI-совместимый) |
| **Open Source** | ✅ Apache 2.0 |
| **Apple Silicon** | ❌ Не поддерживает (только NVIDIA/AMD) |
| **GitHub** | [vllm-project/vllm](https://github.com/vllm-project/vllm) ⭐ 45K+ |

**Ключевые технологии:**
- **PagedAttention** — эффективное управление KV cache (избавляет от фрагментации)
- **Continuous batching** — динамическое добавление/удаление запросов из батча
- **Prefix caching** — кэширование общих префиксов промптов
- **Tensor parallelism** — распределение модели на несколько GPU
- **Speculative decoding** — встроенный

**Когда нужен vLLM:**
- Production-сервер с высокой нагрузкой (10+ concurrent users)
- Batch-обработка тысяч промптов
- Multi-GPU кластер
- ❌ **Не для Mac** (только NVIDIA/AMD Linux)

---

### 1.6 GPT4All — RAG и документы

| Параметр | Значение |
|----------|----------|
| **Интерфейс** | GUI + Python API + CLI |
| **Open Source** | ✅ MIT |
| **Apple Silicon** | Metal |
| **GitHub** | [nomic-ai/gpt4all](https://github.com/nomic-ai/gpt4all) ⭐ 72K+ |

**LocalDocs (RAG):**
- Встроенная индексация документов (PDF, TXT, MD, DOCX)
- Векторная БД (своя реализация на базе Nomic)
- Поиск по локальным файлам без отправки в облако
- Привязка к папкам (до 10 коллекций)

**GPT4All vs Open WebUI для RAG:**

| Критерий | GPT4All | Open WebUI |
|----------|---------|------------|
| Установка | Один `.dmg` | Docker / pip |
| Документы | LocalDocs | 9 векторных БД |
| Мультипользователь | ❌ | ✅ |
| API | Local | OpenAI-совместимый |
| Ресурсы | Лёгкий | Тяжелее (Python) |
| Форматы | PDF, TXT, MD, DOCX | Любые через доки |

**Когда выбрать GPT4All:**
- ✅ Только RAG на локальных документах
- ✅ Минимальная установка (один клик)
- ❌ Нужен API сервер / интеграции
- ❌ Мультипользовательский доступ

---

### 1.7 Jan — десктопный AI

| Параметр | Значение |
|----------|----------|
| **Интерфейс** | GUI (Electron) |
| **Open Source** | ❌ AGPL (ядро), GUI закрыт |
| **Apple Silicon** | Metal |
| **GitHub** | [janhq/jan](https://github.com/janhq/jan) ⭐ 35K+ |

**Особенности:**
- Model Hub — поиск и скачивание моделей
- Поддержка vision-моделей (через llama.cpp)
- Remote model support (подключение к OpenAI, Anthropic, локальным серверам)
- Thread management (история диалогов)
- Local server mode

**Когда выбрать Jan:**
- ✅ Нужен красивый десктопный клиент
- ✅ Хотите совмещать локальные и cloud-модели
- ❌ Нужна лёгкость (Electron жрёт RAM)

---

### 1.8 Open WebUI — веб-интерфейс для Ollama

| Параметр | Значение |
|----------|----------|
| **Интерфейс** | Веб (Python + Svelte) |
| **Open Source** | ✅ MIT |
| **Установка** | `docker run` |
| **GitHub** | [open-webui/open-webui](https://github.com/open-webui/open-webui) ⭐ 70K+ |

**Возможности:**
- Подключение к Ollama, OpenAI, Anthropic, Google, AWS Bedrock
- **9 векторных БД:** Chroma, Milvus, Qdrant, Weaviate, PGVector, Elastic, Meilisearch, Pinecone, Supabase
- Agentic RAG: агент сам решает, когда и как искать документы
- Мультипользовательский режим (команды)
- Web search, image generation, audio input
- Темы, кастомизация

**Когда выбрать Open WebUI:**
- ✅ Нужен веб-интерфейс для команды
- ✅ Продвинутый RAG с агентским поиском
- ✅ Нужна поддержка 9+ векторных БД
- ❌ Только для одного пользователя (избыточно)

---

### 1.9 Aider — CLI-агент для кодинга

| Параметр | Значение |
|----------|----------|
| **Интерфейс** | CLI |
| **Open Source** | ✅ Apache 2.0 |
| **Локальные модели** | Да (через Ollama) |
| **GitHub** | [paul-gauthier/aider](https://github.com/paul-gauthier/aider) ⭐ 25K+ |

**Архитектура:**
- **Архитектор/Редактор (architect/editor)** — split: один планирует, второй пишет код
- **Repo map** — автоматическая карта репозитория для контекста
- **Map-refine** — уточнение карты при изменениях
- **Lint & test integration** — автоматическая проверка кода

**Качество с локальными моделями:**
- Qwen 2.5 Coder 7B — ~50% от качества GPT-4o на задачax Aider
- DeepSeek R1 7B — хорошо для рефакторинга
- Qwen3-Coder-30B — ~70% от GPT-4o

**Установка:**
```bash
python -m pip install aider-chat
export OLLAMA_API_BASE=http://localhost:11434
aider --model ollama/qwen2.5-coder:7b
```

**Когда выбрать Aider:**
- ✅ Нужен автономный кодинг-агент в CLI
- ✅ Рефакторинг через промпты
- ❌ Нужен GUI / визуальный интерфейс

---

### 1.10 Continue.dev — IDE-плагин

| Параметр | Значение |
|----------|----------|
| **Интерфейс** | IDE плагин (VS Code, JetBrains) |
| **Open Source** | ✅ Apache 2.0 |
| **Локальные модели** | Да (через Ollama) |
| **GitHub** | [continuedev/continue](https://github.com/continuedev/continue) ⭐ 25K+ |

**Как работает:**
- Подключается к любому OpenAI-совместимому API (Ollama, LM Studio, vLLM)
- **Chat** — диалог с моделью прямо в IDE
- **Inline editing** — выделили код → промпт → модель меняет
- **Tab autocomplete** — автодополнение при печати (через отдельную модель)
- **@codebase** — контекст всего репозитория через embeddings

**Per-role модели:**
```json
{
  "models": [
    {
      "title": "Быстрый чат",
      "provider": "ollama",
      "model": "qwen3:4b"
    },
    {
      "title": "Автодополнение",
      "provider": "ollama",
      "model": "qwen2.5-coder:1.5b"
    },
    {
      "title": "Сложный рефакторинг",
      "provider": "ollama",
      "model": "qwen2.5-coder:14b"
    }
  ]
}
```

**Когда выбрать Continue.dev:**
- ✅ Работаете в VS Code / JetBrains
- ✅ Хотите AI-ассистента без подписки Copilot
- ✅ Нужен tab autocomplete с локальной моделью
- ❌ Не используете IDE

---

### 1.11 Enchanted — клиент для macOS и iOS

| Параметр | Значение |
|----------|----------|
| **Интерфейс** | macOS + iOS GUI (SwiftUI) |
| **Open Source** | ✅ MIT |
| **Apple Silicon** | Нативный, подключается к Ollama |
| **GitHub** | [gluonfield/enchanted](https://github.com/gluonfield/enchanted) |

**Особенности:**
- Нативные приложения для macOS и iOS (SwiftUI)
- Подключается к любому Ollama-серверу по URI (`http://localhost:11434`)
- Можно стучаться с iPhone до Mac в локальной сети
- Минималистичный интерфейс в стиле Apple
- Markdown-рендеринг, поддержка изображений

**Когда выбрать:**
- ✅ Нужен клиент для iPhone/iPad в дополнение к Mac
- ✅ Нравится минималистичный дизайн
- ❌ Не нужна iOS-версия — Ollama CLI или Open WebUI проще

---

### 1.12 LocalAI — OpenAI API drop-in

| Параметр | Значение |
|----------|----------|
| **Интерфейс** | HTTP API (OpenAI-совместимый) |
| **Open Source** | ✅ MIT |
| **Установка** | Docker, binaries |
| **GitHub** | [mudler/LocalAI](https://github.com/mudler/LocalAI) ⭐ 30K+ |

**Поддержка бэкендов (60+):**
- llama.cpp, transformers, diffusers, whisper, piper-tts, stable-audio
- Image generation, text-to-speech, speech-to-text
- Embeddings, reranking
- Gallery моделей (YAML-конфиги)

**Docker:**
```bash
docker run -p 8080:8080 \
  -v $PWD/models:/models \
  localai/localai:latest
```

**Когда выбрать LocalAI:**
- ✅ Нужен полный OpenAI API (включая TTS, STT, images)
- ✅ Docker-инфраструктура
- ❌ Просто LLM (избыточно)

---

### 1.13 MindWork AI Studio — универсальный мульти-провайдер

| Параметр | Значение |
|----------|----------|
| **Интерфейс** | GUI (macOS, Windows, Linux) |
| **Open Source** | ✅ FSL-1.1-MIT (→ MIT через 2 года) |
| **Apple Silicon** | Нативный |
| **GitHub** | [MindWorkAI/AI-Studio](https://github.com/MindWorkAI/AI-Studio) ⭐ 529 |

**Поддержка провайдеров:**

| Тип | Провайдеры |
|---|---|
| **Локальные** | Ollama, LM Studio, llama.cpp, vLLM |
| **Облачные** | OpenAI, Anthropic, Google Gemini, Mistral, Perplexity, xAI (Grok), DeepSeek, OpenRouter, Groq |
| **HuggingFace** | Cerebras, Nebius, Together AI, Fireworks и др. |

**Особенности:**
- Единый интерфейс для локальных и облачных моделей — переключение без перезагрузки
- Ассистенты для типовых задач (перевод, анализ документов, генерация слайдов)
- Поддержка image generation (DALL-E, Stable Diffusion)
- RAG (Qdrant, внешние данные через ERI)
- Плагины на Lua, i18n, enterprise-конфигурации
- Бесплатно, даже для коммерческого использования

**Когда выбрать:**
- ✅ Нужен один GUI для всех провайдеров (локальных и облачных)
- ✅ Хотите быстро переключаться между моделями без настройки
- ✅ Нужны ассистенты для бизнес-задач
- ❌ Нужен только локальный запуск — Ollama или LM Studio проще
- ❌ Нужен CLI/API для автоматизации — не замена Ollama

---

## 2. Сценарии: какой инструмент выбрать

| Сценарий | Инструмент | Почему |
|----------|-----------|--------|
| **Хочу скачать и тестировать модели** (discovery) | **LM Studio** | Встроенный поиск HF по категориям, визуальный |
| **Нужен API для моего приложения** (integration) | **Ollama** | 3 команды, OpenAI API, любой язык |
| **Максимальная скорость на Mac** (performance) | **MLX** (mlx-lm) или LM Studio | +20-40% tok/s, -50% RAM |
| **Нужен fine-tuning модели** (training) | **MLX** (mlx-lm) | LoRA/QLoRA, нативный Apple, Python API |
| **Приватный RAG на документах** (RAG) | **GPT4All** (один пользователь) или **Open WebUI** (команда) | LocalDocs / 9 векторных БД |
| **AI-ассистент в VS Code** (coding-IDE) | **Continue.dev + Ollama** | Tab autocomplete, inline editing, @codebase |
| **Автономный кодинг-агент в CLI** (coding-agent) | **Aider + Ollama** | architect/editor split, repo map |
| **Production server** (deployment) | **vLLM** (Linux) или **LM Studio** (Mac) | Continuous batching, PagedAttention |
| **Полный новичок** (beginner) | **LM Studio** | GUI, ни одной команды в терминале |
| **Максимальная приватность** (privacy) | **Ollama + GPT4All** | Всё локально, 0 соединений наружу |
| **Длинный контекст 100K+** (context) | **llama.cpp** (raw) | Speculative decoding, KV cache контроль |
| **Batch-обработка тысяч промптов** (batch) | **vLLM** (Linux) или **llama.cpp** (Mac) | Continuous batching |

---

## 3. Бенчмарки на Apple Silicon

### 3.1 MLX vs llama.cpp vs Ollama (M5 Max 128GB, Llama 3.1 70B Q4)

| Бэкенд | tok/s | Память | Отставание |
|--------|-------|--------|-----------|
| **MLX** | **18** | ~39 GB | 🥇 |
| llama.cpp Metal | 14 | ~41 GB | -22% |
| Ollama (CPU) | 12 | ~41 GB | -33% |

*(Источник: CraftRigs)*

### 3.2 MLX vs llama.cpp (M4 Max 36GB, Llama 3 8B Q4)

| Бэкенд | Prefill (tok/s) | Генерация (tok/s) | Память |
|--------|----------------|-------------------|--------|
| llama.cpp Metal | 1420 | 71.3 | 5.8 GB |
| MLX 4-bit | 1180 | 65.8 | 6.1 GB |

*(Источник: Contra Collective)*

### 3.3 MLX vs llama.cpp (M3 Ultra 192GB, Llama 3.1 70B Q4)

| Бэкенд | Prefill (tok/s) | Генерация (tok/s) | Память |
|--------|----------------|-------------------|--------|
| llama.cpp Metal | 380 | 9.4 | 41 GB |
| MLX 4-bit | 470 | 11.1 | 39 GB |

*(Источник: Contra Collective)*

### 3.4 Systematic comparison (arXiv, M2 Ultra 192GB, Qwen-2.5)

| Фреймворк | Макс. tok/s | TTFT | Пропускная способность |
|-----------|------------|------|----------------------|
| **MLX** | **~230** | Средний | Максимальная |
| MLC-LLM | ~200 | **Низкий** | Лучший TTFT |
| llama.cpp | ~150 | Быстрый | Лёгкий |
| Ollama | ~130 | Медленный | Простота |
| PyTorch MPS | ~7-9 | — | Не для продакшена |

*(Источник: arXiv:2511.05502)*

### 3.5 Speculative decoding ускорение

| Инструмент | Модель | Без SD (tok/s) | Со SD (tok/s) | Ускорение |
|-----------|--------|---------------|--------------|-----------|
| **mlx-lm** | Llama 3.3 70B | 11.2 | **23.5** | **2.1×** |
| llama.cpp | Llama 3.3 70B + 8B draft | 9.4 | **15.1** | **1.6×** |

### 3.6 Влияние контекста на скорость

Gemma 4 26B MoE на M3 Max 128GB:

| Контекст | Prefill (tok/s) | Генерация (tok/s) |
|----------|----------------|-------------------|
| 1K | 937 | 41.5 |
| 16K | 1015 | 30.8 |
| 64K | 754 | 15.5 |
| **128K** | **534** | **5.6** |

*(Источник: PubliVault)*

---

## 4. Масштабирование

### 4.1 Concurrent users

| Инструмент | Макс. concurrent | Зависит от | Механизм |
|-----------|-----------------|-----------|----------|
| **vLLM** | 100+ | GPU memory | Continuous batching |
| **Ollama** | 1-4 | RAM, `OLLAMA_NUM_PARALLEL` | Sequential (до v0.5), parallel (v0.5+) |
| **LM Studio** | 1-2 | RAM | Server mode |
| **llama.cpp** | 1-8 | RAM, batch size | Server mode |
| **MLX** | 1-2 | RAM | Нет серверного режима |
| **GPT4All** | 1 | — | Только локально |

### 4.2 Docker support

| Инструмент | Docker | Известный образ |
|-----------|--------|----------------|
| **Ollama** | ✅ | `ollama/ollama` |
| **LM Studio** | ❌ | — |
| **MLX** | ⚠️ (только Mac) | `mlx-community` |
| **llama.cpp** | ✅ | `ghcr.io/ggml-org/llama.cpp` |
| **vLLM** | ✅ | `vllm/vllm-openai` |
| **GPT4All** | ❌ | — |
| **Jan** | ❌ | — |
| **Open WebUI** | ✅ | `ghcr.io/open-webui/open-webui` |
| **LocalAI** | ✅ | `localai/localai` |
| **Aider** | ⚠️ | Community |

### 4.3 GPU memory management

| Инструмент | Unload | Offload | KV cache control | Multi-GPU |
|-----------|--------|---------|-----------------|-----------|
| **Ollama** | ✅ При бездействии | `OLLAMA_GPU_LAYERS` | `OLLAMA_KV_CACHE_TYPE` | ❌ |
| **MLX** | ❌ (вручную) | N/A (всегда GPU) | ❌ | ❌ |
| **llama.cpp** | ✅ | `-ngl` | `--cache-type-k`, `--cache-type-v` | ✅ |
| **vLLM** | ✅ | `--gpu-memory-utilization` | `--kv-cache-dtype` | ✅ |

---

## 5. Гайды по установке

### Ollama

```bash
# macOS
brew install ollama

# Linux
curl -fsSL https://ollama.com/install.sh | sh

# Запуск модели
ollama run qwen3:8b

# API
curl http://localhost:11434/api/generate -d '{
  "model": "qwen3:8b",
  "prompt": "Привет!",
  "stream": false
}'

# Modelfile
cat > Modelfile << 'EOF'
FROM qwen3:8b
PARAMETER temperature 0.7
PARAMETER num_ctx 8192
SYSTEM "Ты профессиональный Python-разработчик."
EOF
ollama create my-coder -f Modelfile
```

### LM Studio

```bash
# 1. Скачайте .dmg с lmstudio.ai
# 2. Откройте → Search → найдите модель
# 3. Download → Load → Chat
# Server mode:
# Settings → Server → Enable → Port 1234
```

### MLX

```bash
# Установка
pip install mlx-lm

# Инференс
python -m mlx_lm.generate \
  --model mlx-community/Qwen3-8B-4bit \
  --prompt "Привет, как дела?" \
  --max-tokens 256

# Чат
python -m mlx_lm.chat \
  --model mlx-community/Qwen3-8B-4bit

# HTTP сервер
python -m mlx_lm.server \
  --model mlx-community/Qwen3-8B-4bit

# Fine-tuning
python -m mlx_lm.lora \
  --model mlx-community/Qwen3-8B-4bit \
  --data data.jsonl \
  --num-layers 16 \
  --lora-rank 8
```

### llama.cpp

```bash
# Сборка
git clone https://github.com/ggml-org/llama.cpp
cd llama.cpp && make -j

# Скачать GGUF
wget https://huggingface.co/Qwen/Qwen3-8B-GGUF/resolve/main/qwen3-8b-q4_k_m.gguf

# Запуск
./main -m qwen3-8b-q4_k_m.gguf \
  -p "Привет!" \
  -n 256 \
  -ngl 99  # все слои на GPU

# Сервер
./server -m qwen3-8b-q4_k_m.gguf \
  --port 8080 \
  -ngl 99

# Embeddings
./embedding -m qwen3-8b-q4_k_m.gguf \
  -p "Какой-то текст для эмбеддинга"
```

### vLLM

```bash
# Установка (Linux only)
pip install vllm

# Сервер
python -m vllm.entrypoints.openai.api_server \
  --model Qwen/Qwen3-8B \
  --dtype auto \
  --gpu-memory-utilization 0.9

# API
curl http://localhost:8000/v1/chat/completions -d '{
  "model": "Qwen/Qwen3-8B",
  "messages": [{"role": "user", "content": "Привет!"}]
}'
```

### Aider + Ollama

```bash
# Установка
python -m pip install aider-chat

# Запуск с локальной моделью
export OLLAMA_API_BASE=http://localhost:11434
aider --model ollama/qwen2.5-coder:7b

# Architect mode (2 модели)
aider --model ollama/qwen2.5-coder:7b \
  --editor-model ollama/qwen3:4b

# Режимы
aider --chat-mode ask      # вопросы о коде
aider --chat-mode code     # написание кода
aider --chat-mode architect # архитектор + редактор
```

### Open WebUI

```bash
# Docker
docker run -d -p 3000:8080 \
  -v open-webui:/app/backend/data \
  --name open-webui \
  --restart always \
  ghcr.io/open-webui/open-webui:main

# Подключение к Ollama
# Настройки → Подключения → Ollama API URL: http://host.docker.internal:11434
```

### GPT4All

```bash
# macOS
brew install --cask gpt4all

# Или скачать с https://gpt4all.io/

# Python API
pip install gpt4all
```

### LocalAI

```bash
# Docker
docker run -p 8080:8080 \
  -v $PWD/models:/models \
  localai/localai:latest

# LLM
curl http://localhost:8080/v1/chat/completions -d '{
  "model": "gpt-4",
  "messages": [{"role": "user", "content": "Привет!"}]
}'

# TTS
curl http://localhost:8080/v1/audio/speech -d '{
  "model": "tts-1",
  "input": "Привет, мир!",
  "voice": "en_US-amy-medium"
}'
```

---

## 6. Decision Tree

```
Хочу запускать LLM локально
│
├─ Я полный новичок, не хочу терминал
│  └─ LM Studio ─────────────────────────────── GUI, авто-MLX, поиск моделей
│
├─ Нужна одна команда и API
│  └─ Ollama ─────────────────────────────────── brew install + ollama run
│     │
│     ├─ Хочу GUI сверху → Open WebUI ───────── Docker, RAG, multi-user
│     ├─ Хочу IDE assistant → Continue.dev ──── VS Code, tab autocomplete
│     └─ Хочу CLI agent → Aider ─────────────── architect/editor, repo map
│
├─ Mac, нужно максимально быстро
│  └─ MLX (через LM Studio или mlx-lm) ─────── +20-40%, -50% RAM
│
├─ Нужен production server на Linux
│  └─ vLLM ───────────────────────────────────── Continuous batching, multi-GPU
│
├─ Нужен RAG на документах
│  ├─ Одному пользователю → GPT4All
│  └─ Команде → Open WebUI
│
├─ Нужно обучение / fine-tuning
│  └─ MLX (mlx-lm) ──────────────────────────── LoRA/QLoRA на Apple Silicon
│
├─ Нужен полный контроль и гибкость
│  └─ llama.cpp (raw) ──────────────────────── GGUF, speculative decoding
│
└─ Нужен полный OpenAI API (TTS, STT, images)
   └─ LocalAI ───────────────────────────────── 60+ бэкендов, Docker
```

---

## Терминология

| Термин | Значение |
|--------|---------|
| **TTFT** (Time To First Token) | Задержка до первого токена ответа |
| **Continuous batching** | Динамическое добавление запросов в батч |
| **PagedAttention** | Эффективное управление KV cache без фрагментации |
| **Speculative decoding** | Маленькая модель «черновик» → большая проверяет |
| **KV cache** | Кэш ключей/значений внимания — главный потребитель RAM при длинном контексте |
| **GGUF** | Формат модели для llama.cpp с встроенной квантизацией |
| **Modelfile** | Конфиг Ollama для создания кастомных моделей |

---

> **Последнее обновление:** 23 июля 2026
> **Источники:** CraftRigs, Contra Collective, arXiv:2511.05502, asiai.dev, MLJourney, MacYou, GitHub, официальные сайты инструментов
