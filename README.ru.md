# 🧠 Awesome AI Handbook

> Практический справочник по AI: от локальных моделей до своих агентов.
> Не просто ссылки, а готовые руководства, бенчмарки и проверенные подходы.

**[English](README.md)** | **Русский**

---

## 📋 Содержание

- [О проекте](#-о-проекте)
- [Быстрый старт](#-быстрый-старт-5-минут)
- [Что влезет в RAM](#-что-влезет-в-вашу-ram)
- [Как найти и запустить модель](#-как-найти-и-запустить-модель-практикум)
- [Каталог моделей с альтернативами](#-каталог-моделей-с-альтернативами)
  - [Кодинг](#-кодинг)
  - [Чат](#-чат--general)
  - [Reasoning](#-рассуждения--математика)
  - [Vision](#-мультимодальные-vision)
  - [8GB модели](#-маленькие-модели-для-8gb-и-cpu)
  - [Рекомендации по железу](#-рекомендации-по-железу)
  - [Бенчмарки Apple Silicon](#-сводная-таблица-производительности-apple-silicon)
- [Инструменты запуска](#-сравнение-инструментов)
  - [Decision Tree](#decision-tree-какой-инструмент-взять)
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
ollama run qwen3.5:4b   # ~3.4 GB — чат
# или для кодинга:
ollama run qwen2.5-coder:7b  # ~4.7 GB

# 3. Готово! Спросите что-нибудь.
```

---

## 💾 Что влезет в вашу RAM

**Реальная формула:** macOS резервирует ~6–10GB. Доступно для модели = Общая RAM − 8GB (в среднем).  
**Правило 60%:** держите размер модели ≤ 60% от общей RAM для стабильной работы без swap.

| RAM | Доступно | Комфортные модели (Q4) | Впритык (будет swap) |
|---|---|---|---|
| **8 GB** | ~3–4 GB | 0.8B–3B | 7B Q4 — **0.4 tok/s** ⚠️ |
| **16 GB** ⭐ | ~8–10 GB | 3B–8B Q4/Q5 | 9B Q4, 14B Q4 — тесно |
| **24 GB** | ~14–17 GB | 7B–14B | 32B Q4 — впритык |
| **32 GB** | ~22–25 GB | 14B–32B Q4 | 70B Q3 (сильные потери) |
| **48 GB** | ~38–42 GB | 20B–40B Q4 | 70B Q4 — тесно |
| **64 GB** | ~54–58 GB | 32B–70B Q4 | 70B Q5/Q6 |
| **128 GB** | ~118–122 GB | 70B+ Q4–Q8 | 120B+ Q4 |

> ⚡ **Что будет при swap:** генерация падает до **0.3–2.2 tok/s**. На M1 8GB 8B-модель выдаёт 0.4 tok/s — в 200× медленнее cloud API.  
> **Проверка:** `ollama ps` показывает занятую память. Если Memory (MB) близко к RAM → закрывайте приложения или берите модель меньше.

### ⚙️ Контекст по умолчанию (тихий пожиратель памяти)

Ollama автоматически выбирает `num_ctx` в зависимости от VRAM вашего Mac:

| VRAM | `num_ctx` | Примеры чипов |
|------|-----------|---------------|
| < 23 GB | **4096** | M1/M2/M3/M4 **16GB** — ваш случай |
| 23–47 GB | **32768** | M1/M2/M3/M4 24–36GB |
| ≥ 47 GB | **262144** | M1/M2/M3/M4 48GB+ |

Если вы не задаёте `num_ctx` явно, на M1 16GB контекст всего **4096 токенов**. Для чата это маловато (≈2–3 страницы текста). Чтобы увеличить:

```bash
# Через API
curl http://localhost:11434/api/chat -d '{
  "model": "qwen3.5:4b",
  "messages": [{"role": "user", "content": "Привет!"}],
  "options": {"num_ctx": 8192}
}'

# Или через Modelfile
ollana create my-model -f - << 'EOF'
FROM qwen3.5:4b
PARAMETER num_ctx 8192
EOF
```

**Правило:** каждый токен контекста ≈ 0.5–2 MB KV cache на модель. Увеличение `num_ctx` с 4096 до 8192 — это ещё ~4–16 MB.

### 🧠 Расход KV cache (скрытый потребитель памяти)

Даже если веса модели влезают, **длинный контекст съедает RAM через KV cache**. Вот сколько памяти уходит сверху:

| Контекст | 8B модель | 32B Q4 | 70B Q4 |
|----------|-----------|--------|--------|
| 4K | +0.6 GB | +1.5 GB | +2.5 GB |
| 32K | +5 GB | +12 GB | +20 GB |
| 128K | +20 GB | +48 GB | +80 GB |

> **Решение:** используйте KV cache квантизацию + flash attention:
> - `OLLAMA_KV_CACHE_TYPE=q8_0` — 2× экономия (хорошее качество)
> - `OLLAMA_KV_CACHE_TYPE=q4_0` — **4× экономия** (небольшая потеря качества, но контекст влезает)
> - `OLLAMA_FLASH_ATTENTION=1` — дополнительная экономия + ускорение (экспериментально)
> - Комбинация `q4_0` + flash attention даёт до **5× меньше памяти** на KV cache

---

## 🎯 Каталог моделей с альтернативами

> **Полный справочник** — в [local-models/catalog.md](local-models/catalog.md) (50+ моделей, все спецификации, Hidden Gems).  
> Ниже — сжатый каталог по задачам с несколькими альтернативами на выбор.

---

### 💻 Кодинг

| Для | Модель | Параметры | Контекст | Скорость на M1 16GB | HumanEval | SWE-bench |
|-----|--------|-----------|----------|---------------|-----------|-----------|
| **Автодополнение (FIM)** | Qwen 2.5 Coder 1.5B | 1.5B | 32K | 30+ tok/s ⚡ | ~45% | — |
| **Автодополнение** | CodeGemma 2B | 2B | 8K | 35+ tok/s ⚡ | ~50% | — |
| **Автодополнение** | Stable Code 3B ⭐ | 2.7B | 16K | 20+ tok/s | ~55% | — |
| **Основной кодер** | Qwen 2.5 Coder 7B ⭐ | 7B | 128K | 22–25 tok/s 🟢 | **82%** | — |
| **Основной кодер** | DeepSeek R1 Distill 7B | 7B | 32K | 18–22 tok/s 🟢 | ~58% | — |
| **Основной кодер** | Yi-Coder 9B | 9B | **128K** | 14–18 tok/s 🟡 | ~70% | — |
| **Продвинутый** | Qwen3-Coder-30B-A3B | 30B/3.3B MoE | 256K | — | — | ~70% |
| **Продвинутый** | Devstral Small 2 | 24B | 256K | — | — | **68.0%** |
| **Продвинутый** | Phi-4-reasoning 14B | 14B | 16K | 12–16 tok/s 🟡 | — | — |
| **FIM + 80 языков** | Codestral 25.01 | 22B | 256K | — | **95.3% (FIM)** | — |

> 🟢 Быстро | 🟡 Нормально | 🔴 Медленно. Все цифры — Q4_K_M на MacBook Air M1 16GB.  
> **FIM** (Fill-in-the-Middle) — автодополнение кода в IDE. **SWE-bench** — решение реальных GitHub issues.

---

## 🚀 Как найти и запустить модель (практикум)

### Способ 1: Ollama (рекомендую для начала)

Ollama — готовые модели одной командой. Список всех моделей: [ollama.com/search](https://ollama.com/search)

```bash
# Найти модель: открыть https://ollama.com/search, найти по имени

# Запустить (автоматически скачает):
ollama run qwen3.5:4b        # Qwen 3.5 4B (3.4GB) — чат
ollama run qwen2.5-coder:7b  # Qwen 2.5 Coder 7B (4.7GB) — кодинг

# Размеры на Ollama указаны на странице модели (колонка Size)
# Все модели уже в оптимальной квантизации (Q4_K_M)
```

**Важно:** разные версии Qwen — **разные библиотеки** на Ollama:
- `ollama run qwen3:4b` — Qwen 3 (Март 2025)
- `ollama run qwen3.5:4b` — Qwen 3.5 (Февраль 2026)
- `ollama run qwen3.6:27b` — Qwen 3.6 (Апрель 2026)

**Не путайте!** Каждая точка-релиз — отдельная страница на Ollama.

#### 🔧 Настройка Ollama (для Mac)

Ключевые переменные окружения для тюнинга производительности:

| Переменная | По умолчанию | Что даёт |
|---|---|---|
| `OLLAMA_KV_CACHE_TYPE=q4_0` | `f16` | **4× меньше памяти** на KV cache (контекст влезает лучше) |
| `OLLAMA_FLASH_ATTENTION=1` | выкл | Экономия памяти + ускорение (experimental) |
| `OLLAMA_NUM_PARALLEL=2` | `1` | Два параллельных запроса к одной модели |
| `OLLAMA_KEEP_ALIVE=10m` | `5m` | Держать модель в памяти 10 минут после последнего запроса |
| `OLLAMA_GPU_OVERHEAD=536870912` | `0` | Резерв 512MB для других приложений |
| `OLLAMA_LOAD_TIMEOUT=10m` | `5m` | Таймаут загрузки модели |
| `OLLAMA_HOST=0.0.0.0:11434` | `127.0.0.1:11434` | Доступ по сети (с осторожностью!) |
| `OLLAMA_MAX_QUEUE=512` | `512` | Макс. очередь запросов |

**Рекомендуемый конфиг для M1 16GB** — добавьте в `~/.zshrc`:

```bash
# Ollama — оптимизация для M1 16GB
export OLLAMA_KV_CACHE_TYPE=q4_0    # 4x экономия KV cache
export OLLAMA_FLASH_ATTENTION=1      # экспериментально, но помогает
export OLLAMA_NUM_PARALLEL=2         # 2 параллельных запроса
export OLLAMA_KEEP_ALIVE=10m         # модель живёт 10 минут
export OLLAMA_GPU_OVERHEAD=536870912 # 512 MB — запас для системы
```

После добавления — перезапустите оболочку (`source ~/.zshrc` или новое окно терминала) и затем `ollama serve`.

#### ⚠️ MLX-модели: важное отличие

Модели, которые Ollama запускает через MLX (safetensors), **не поддерживают truncation**. Если контекст превышает лимит — ошибка, а не обрезание, как у GGUF/llama.cpp. Если видите ошибки при длинных диалогах — переключитесь на GGUF-версию модели.

### Способ 2: LM Studio (для новичков)

1. Скачайте [LM Studio](https://lmstudio.ai)
2. В поиске найдите модель (например «Qwen 3.5 4B»)
3. Нажмите Download → Load → Chat

LM Studio сам покажет размер модели, предложит лучшую квантизацию и настроит бэкенд.

### Способ 3: HuggingFace → Ollama (продвинутый)

Модели на HuggingFace в формате transformers — их **нельзя запустить напрямую**. Нужно:

```
Модель на HuggingFace → Ollama (есть в библиотеке)    → ollama run
                      → GGUF (конвертирована)         → скачать .gguf → запустить через LM Studio / llama.cpp
                      → MLX (конвертирована)           → запустить через mlx-lm
```

**Что проще всего:** если модель есть на [ollama.com/search](https://ollama.com/search) — просто `ollama run <имя>`.  
Если модели нет на Ollama — ищите GGUF-версию на HuggingFace (в поиске добавьте «GGUF»).

---

### 💬 Чат / General

| Для | Модель | Запуск | Параметры | Контекст | Скорость (M1 16GB) | Качество |
|-----|--------|--------|-----------|----------|---------------------|----------|
| **Быстрый чат** | Qwen 3.5 4B | `ollama run qwen3.5:4b` | 4B | 256K | 28–35 tok/s 🟢 | Хорошее |
| **Быстрый чат** | Phi-4-mini | `ollama run phi4-mini` | 3.8B | 128K | 25–30 tok/s 🟢 | Хорошее (MMLU 67.3%) |
| **Быстрый чат** | Llama 3.2 3B | `ollama run llama3.2:3b` | 3B | 128K | 30–40 tok/s 🟢 | Базовое |
| **Сбалансированный** | Qwen 3.5 9B ⭐ | `ollama run qwen3.5:9b` | 9B | **256K** | 10–13 tok/s 🟡 | **Отличное** |
| **Сбалансированный** | Gemma 3 9B | `ollama run gemma3:9b` | 9B | 128K | 12–15 tok/s 🟡 | Отличное |
| **Сбалансированный** | Llama 3.1 8B | `ollama run llama3.1:8b` | 8B | 128K | 14–18 tok/s 🟡 | Отличное |
| **Сбалансированный** | Mistral Small 3 | `ollama run mistral-small:7b` | 7B | 128K | 18–22 tok/s 🟢 | Очень хорошее |
| **Мощный** | Qwen3.6-27B | `ollama run qwen3.6:27b` | 27B | 262K | — | **SWE-bench 77.2%** |
| **Мощный** | Gemma 4 26B A4B MoE | `ollama run gemma4:26b` | 25.2B/3.8B | 256K | — | MMLU-Pro **82.6%** |
| **Мощный** | Llama 3.3 70B | `ollama run llama3.3:70b` | 70B | 128K | — | Для 48GB+ |

---

### 🧮 Reasoning / Математика

| Модель | Параметры | RAM | Когда выбрать |
|--------|-----------|-----|--------------|
| Phi-4-reasoning | 14B | 16GB ✅ | Лучший в классе, на базе o3-mini |
| DeepSeek R1 Distill 7B | 7B | 8GB ✅ | Reasoning в 2-3× лучше обычной 7B |
| DeepSeek R1 Distill 14B | 14B | 16GB ✅ | Сильный mid-range |
| DeepSeek R1 Distill 32B | 32B | 24GB ✅ | Почти как полный R1 |
| Qwen QwQ-32B | 32B | 24GB ✅ | Reasoning-specialized, Apache 2.0 |
| DeepSeek R1 (full) | 671B | Cloud | **MATH-500 97.3%, AIME 79.8%** |

> 💡 **R1 Distills дают chain-of-thought reasoning по цене обычной модели.**  
> ⚠️ Не умеют FIM (автодополнение кода). Для кодинка — Qwen Coder.

#### 🎛️ Управление reasoning через `think` параметр

Reasoning-модели (DeepSeek R1, Phi-4-reasoning и др.) поддерживают параметр `think` в Ollama:

```bash
# Без reasoning (быстрый ответ)
curl http://localhost:11434/api/chat -d '{
  "model": "deepseek-r1:7b",
  "messages": [{"role": "user", "content": "Сколько будет 2+2?"}],
  "options": {"think": false}
}'

# Максимальное reasoning (медленнее, но точнее)
curl http://localhost:11434/api/chat -d '{
  "model": "deepseek-r1:7b",
  "messages": [{"role": "user", "content": "Реши уравнение x² + 3x + 2 = 0"}],
  "options": {"think": "high"}
}'
```

Значения: `false` | `true` | `"low"` | `"medium"` | `"high"` | `"max"`.  
По умолчанию — `true` (средний уровень reasoning). Для быстрых вопросов используйте `false`, для сложных — `"high"`.

---

### 👁️ Мультимодальные (Vision)

| Модель | Параметры | VRAM | Лучше всего для |
|--------|-----------|------|-----------------|
| **Gemma 4 E4B** | ~4.5B eff | ~5 GB | Универсальная, 140+ языков |
| **Qwen3-VL 8B** | 7B | ~6 GB | OCR, мультиязычные документы |
| **MiniCPM-V 4.5 (8B)** | 8B | ~6 GB | **Лучший OCR для документов** |
| **InternVL 2.5 8B** | 8B | ~8 GB | UI, charts, code screenshots |
| **Llama 3.2 Vision 11B** | 11B | ~8 GB | Лучший all-rounder для 8-16GB |
| **LLaVA 1.6 13B** | 13B | ~10 GB | Проверенная классика |
| **Gemma 4 12B Unified** | ~12B | ~8 GB | Text + image + audio |
| **Gemma 4 26B A4B MoE** | 25.2B/3.8B | ~14 GB | Vision + reasoning |
| **Qwen3-VL 72B** | 72B | ~48 GB | Лучший open-source VLM |

---

### ⚡ Маленькие модели (для 8GB и CPU)

| Модель | Параметры | RAM (Q4) | MMLU | HumanEval | tok/s (8GB) |
|--------|-----------|----------|------|-----------|-------------|
| **SmolLM2-1.7B** | 1.7B | 1.2 GB | — | — | 50+ |
| **Llama 3.2 1B** | 1B | 0.8 GB | — | — | 40-50 |
| **TinyLlama 1.1B** | 1.1B | 0.7 GB | — | — | 40+ |
| **Llama 3.2 3B** | 3B | 2.0 GB | 63.4% | — | 30-40 |
| **Qwen3.5-4B** | 4B | 2.8 GB | ~70% | **~74%** | 22-26 |
| **Phi-4-mini** ⭐ | 3.8B | 2.5 GB | **67.3%** | **74.4%** | ~28 |
| **Gemma 3 4B** | 4B | 2.5 GB | 59.6% | 71.3% | ~24 |

> **На 8GB Mac:** Phi-4-mini (reasoning + код) или Qwen3.5-4B (общий чат).

---

### 🎯 Рекомендации по железу

**8GB RAM — только маленькие модели (<4B)**
| # | Модель | Вес (Q4) | Почему |
|---|--------|----------|--------|
| 1 | **Phi-4-mini** | 2.5 GB | Лучший reasoner, MIT, 128K |
| 2 | **Qwen3.5-4B** | 2.8 GB | Лучший coding в этом классе |
| 3 | **Llama 3.2 3B** | 2.0 GB | Самый быстрый, 128K |
| 4 | **Gemma 3 4B** | 2.5 GB | Multimodal (vision!) |

**16GB RAM ⭐ — ваш вариант**
| # | Модель | Запуск | Вес (Q4) | tok/s (M1) | Почему |
|---|--------|--------|----------|-----------|--------|
| 1 | **Qwen 2.5 Coder 7B** | `ollama run qwen2.5-coder:7b` | 4.7 GB | 22–25 | Лучший кодер с FIM |
| 2 | **Qwen 3.5 9B** | `ollama run qwen3.5:9b` | 6.6 GB | 10–13 | Лучший general-purpose |
| 3 | **Phi-4 (14B)** | `ollama run phi4:14b` | 9.1 GB | 8–11 | MMLU 84.8%, тесно но работает |
| 4 | **DeepSeek R1 Distill 14B** | `ollama run deepseek-r1:14b` | 9.0 GB | 8–10 | Reasoning |
| 5 | **Llama 3.1 8B** | `ollama run llama3.1:8b` | 4.9 GB | 14–18 | Проверенная классика |

> **Как узнать точный размер модели:** `ollama run <model>` или откройте страницу модели на [ollama.com/search](https://ollama.com/search) — там колонка Size.

**24-32GB RAM**
| # | Модель | Вес (Q4) | Почему |
|---|--------|----------|--------|
| 1 | **Qwen3.6-27B** | ~17 GB | **Лучшая coding модель**, SWE-bench 77.2% |
| 2 | **Gemma 4 26B A4B MoE** | ~14 GB | 97% качества 31B, Apache 2.0 |
| 3 | **Qwen3-Coder-30B-A3B** | ~19 GB | MoE coding, 256K |
| 4 | **DeepSeek R1 Distill 32B** | ~20 GB | Reasoning |

**48-64GB RAM**
| # | Модель | Вес (Q4) | Почему |
|---|--------|----------|--------|
| 1 | **Llama 3.3 70B** | ~40 GB | Мощный general-purpose |
| 2 | **Qwen3.5-397B-A17B** | ~48 GB | Флагман, Apache 2.0 |
| 3 | **Llama 4 Maverick** | ~48 GB | Multimodal, 1M контекст |

---

### 📊 Сводная таблица производительности (Apple Silicon)

Реальные замеры на Q4_K_M (источники: MLJourney, CraftRigs, MacYou, LLMCheck):

| Чип | 8B tok/s | 14B tok/s | 27-30B tok/s | 70B tok/s |
|-----|----------|-----------|-------------|-----------|
| **M1 8GB** | 12–14 | ❌ | ❌ | ❌ |
| **M1 16GB** | 18–24 | 11–12 | ❌ | ❌ |
| **M2 16GB** | 22–28 | 12–14 | ❌ | ❌ |
| **M2 Pro 32GB** | 30–38 | 18–20 | — | ❌ |
| **M2 Max 64GB** | 50–65 | 30–35 | — | 10–12 |
| **M3 16GB** | 22–33 | 12–15 | ❌ | ❌ |
| **M3 Pro 36GB** | 35–39 | 20–22 | 12–15 | ❌ |
| **M3 Max 128GB** | 65–75 | 40–45 | 25–30 | 10–14 |
| **M4 16GB** | 25–35 | 14–17 | ❌ | ❌ |
| **M4 Pro 48GB** | 38–50 | 22–26 | 18–25 | 7–11 |
| **M4 Max 64GB** | 65–85 | 40–50 | 28–35 | 12–18 |
| **M4 Max 128GB** | 75–95 | 45–55 | 30–40 | 15–22 |
| **M4 Ultra 192GB** | 100–140 | 60–80 | 40–60 | 25–35 |

> ❌ = не влезает в RAM / постоянный swap (0.4–2.2 tok/s).  
> Все цифры — **генерация**, не prefill. Prefill обычно в 10–30× быстрее.  
> Для NVIDIA/Windows/Linux производительность будет существенно выше (RTX 4090 выдаёт 80–120 tok/s на 8B).

---

## 🛠 Сравнение инструментов

### Обзор

| Инструмент | Интерфейс | Open Source | Apple Silicon | Для кого | GitHub |
|---|---|---|---|---|---|
| **Ollama** ⭐ | CLI + API | ✅ MIT | Metal + MLX (с v0.19) | Разработчики, интеграции | [ollama/ollama](https://github.com/ollama/ollama) ⭐ 148K |
| **LM Studio** ⭐ | GUI | ❌ | MLX авто | Начинающие, поиск моделей | [lmstudio-ai/lmstudio](https://github.com/lmstudio-ai/lmstudio) |
| **MLX** (mlx-lm) | Python / CLI | ✅ Apache | Нативно (Apple) | Фреймворк Apple для ML | [ml-explore/mlx](https://github.com/ml-explore/mlx) |
| **llama.cpp** | CLI | ✅ MIT | Metal | Полный контроль | [ggml-org/llama.cpp](https://github.com/ggml-org/llama.cpp) ⭐ 70K+ |
| **GPT4All** | GUI | ✅ ГПЛв3 | Metal | RAG, документы | [nomic-ai/gpt4all](https://github.com/nomic-ai/gpt4all) |
| **Jan** | GUI | ❌ | Metal | Приватность, десктоп | [janhq/jan](https://github.com/janhq/jan) ⭐ 35K |
| **vLLM** | API-сервер | ✅ Apache | ❌ (Linux) | Продакшн, батчинг | [vllm-project/vllm](https://github.com/vllm-project/vllm) |

### Бенчмарки: MLX vs Ollama vs llama.cpp на Apple Silicon

Реальные замеры на одинаковом железе с одинаковой моделью:

**M4 Pro 24GB, Qwen3-Coder-30B MoE** (источник: asiai.dev):

| Метрика | LM Studio (MLX) | Ollama (llama.cpp) | Разница |
|---------|-----------------|-------------------|---------|
| **Throughput** | **102 tok/s** | 70 tok/s | **+46%** |
| TTFT | 291 ms | **175 ms** | Ollama быстрее |
| GPU Power | **12.4 W** | 15.4 W | **-20%** |
| Efficiency | **8.2 tok/s/W** | 4.5 tok/s/W | **+82%** |
| Process Memory | **21.4 GB** | 41.6 GB | **-49%** |

**M5 Max 128GB, Llama 3.1 70B Q4** (источник: CraftRigs):

| Движок | tok/s | Преимущество |
|--------|-------|-------------|
| **MLX** | **18** | 🥇 |
| llama.cpp Metal | 14 | -22% |
| Ollama | 12 | -33% |

**Систематическое сравнение 5 фреймворков** (arXiv:2511.05502, M2 Ultra 192GB):

| Фреймворк | Макс. tok/s | TTFT | Когда выбрать |
|-----------|------------|------|--------------|
| **MLX** | **~230** | Средний | **Максимальная скорость на Mac** |
| MLC-LLM | ~200 | **Низкий** | Минимальная задержка |
| llama.cpp | ~150 | Быстрый | Кроссплатформенность |
| Ollama | ~130 | Медленный | Простота |
| PyTorch MPS | ~7–9 | — | Не для продакшена |

### Важное обновление 2026: Ollama 0.19+ с MLX backend

С марта 2026 Ollama умеет автоматически переключаться на MLX backend на Mac с 32GB+ RAM:
- Qwen 3.5-35B-A3B: 58 → **112 tok/s** на M5 Max (+93%)
- Пока работает не со всеми моделями (Qwen — да, Llama/Mistral — пока нет)

### Как выбрать инструмент

| Сценарий | Рекомендация | Почему |
|----------|-------------|--------|
| **Начинающий / тесты** | LM Studio | Визуальный интерфейс, авто-MLX |
| **Разработка / интеграция** | **Ollama** | Одна команда, HTTP API, Continue.dev |
| **Максимальная скорость** | MLX (mlx-lm) | +20–40% tok/s, -50% памяти |
| **Кроссплатформа** | llama.cpp | GGUF работает везде |
| **Продакшн (Linux)** | vLLM | Continuous batching |
| **Тонкая настройка** | MLX | Обучение + инференс в одном фреймворке |
| **RAG / документы** | GPT4All | Встроенная RAG-система |

> **Полный справочник** — в [local-models/tools.md](local-models/tools.md) (12 инструментов, бенчмарки, гайды по установке, scaling, decision tree).

### Decision Tree: какой инструмент взять

```
Новичок, не хочу терминал        → LM Studio (GUI, авто-MLX)
Одна команда и API                → Ollama (+ Open WebUI для веба)
  + IDE assistant                 → Continue.dev
  + CLI coding agent              → Aider
Mac, нужно максимально быстро     → MLX (через LM Studio или mlx-lm)
Production на Linux               → vLLM
RAG на документах                 → GPT4All (один) / Open WebUI (команда)
Fine-tuning / обучение            → MLX (mlx-lm, LoRA/QLoRA)
Полный контроль / GGUF            → llama.cpp (raw)
Полный OpenAI API (TTS, STT, img) → LocalAI
```

### Связки для разработчика

```
Редактор кода → Continue.dev → Ollama → Qwen 2.5 Coder 7B
    (VS Code /                              (локально,
     JetBrains)                              бесплатно,
                                             приватно)
```

```
CLI → Aider → Ollama → Qwen 2.5 Coder 7B
           (автономный      (локально,
            coding агент)    приватно)
```

**Почему не хватает просто Ollama:** Ollama — это сервер моделей (как локальный OpenAI API). Чтобы реально кодить файлы, нужен «клиент»: **Continue.dev** (IDE-плагин), **Aider** (CLI), или **OpenHands** (автономный агент). Ollama сам по себе только отвечает на промпты.

---

## 📦 Квантизация в двух словах

| Формат | Бит | Размер 8B | Perplexity ↑ | Когда использовать |
|--------|-----|-----------|-------------|-------------------|
| Q4_K_M ⭐ | ~4.8 | ~5.2 GB | 4.7× | **По умолчанию** — лучший баланс |
| Q5_K_M | ~5.5 | ~6.1 GB | 6.0× | Письмо, аналитика |
| Q6_K | ~6.0 | ~6.6 GB | 9.5× | Код, критична точность |
| Q8_0 | 8.0 | ~8.5 GB | ∞ (≈FP16) | Если RAM позволяет |
| IQ2_XXS | ~2.3 | ~2.5 GB | — | **Экстремальная** (70B в 24GB) |
| IQ3_XXS | ~3.3 | ~3.6 GB | — | Сильно сжатая |

> **Perplexity ↑** — во сколько раз качество выше базового 4-bit (чем больше, тем лучше).

### MLX vs GGUF квантизация

- **GGUF** (llama.cpp/Ollama): Q4_K_M, Q5_K_M и т.д. — смешанная точность, лучшая perplexity на бит
- **MLX** (LM Studio/mlx-lm): униформный 4-bit / 8-bit. Чуть хуже perplexity, но **значительно быстрее** на Apple Silicon (+20-40%) и меньше памяти (-50%)
- **Формула:** MLX 4-bit ≈ Q4_K_M по скорости, но уступает по качеству. Для повседневных задач разница незаметна.

### TurboQuant (turbo3/turbo4) — KV cache квантизация

Новый метод (2026) для работы с длинным контекстом. Позволяет сжать KV cache до 3-4 бит вместо 16:

| Контекст | fp16 | q8_0 | turbo3 | turbo4 |
|----------|------|------|--------|--------|
| 32K | 20 GB | 10 GB | 5 GB | 5 GB |
| 128K | 80 GB | 40 GB | 20 GB | 20 GB |
| **1M** | **OOM** | **OOM** | **6.5 tok/s** ✅ | OOM |

**Turbo3 — единственный способ получить 1M контекста на 128GB Mac.**

### Как выбрать

1️⃣ **Q4_K_M** — всегда, если не знаете, что выбрать  
2️⃣ **MLX 4-bit** — если нужна максимальная скорость на Mac  
3️⃣ **Q5_K_M** — если пишете тексты или анализируете  
4️⃣ **IQ2** — если нужно запустить 70B на 24GB (но качество пострадает)

---

## ⚠️ Частые проблемы и решения

| Проблема | Причина | Решение |
|---|---|---|
| **2–5 tok/s (очень медленно)** | Модель не влезает в RAM → swap на SSD | Уменьшите модель/квантизацию. На 8GB Mac 8B модель → **0.4 tok/s** — бесполезно |
| **Metal не используется** | Ollama не переключена на Metal | `ollama ps` — проверьте backend. Должен быть `metal`, не `cpu` |
| **Jetsam убивает Ollama** | macOS защищает память (OOM-killer) | Берите модель меньше и `num_ctx` 2048–4096 |
| **Крашится на длинных ответах** | KV cache съел всю RAM | `OLLAMA_KV_CACHE_TYPE=q8_0` — уменьшит кэш в 2× |
| **Скорость падает после Chrome** | Браузер ест 2–4 GB — модели остаётся меньше | Закройте Chrome. На 16GB Mac Chrome + IDE + модель = гарантированный swap |
| **Долгая загрузка модели** | Первый запуск читает с диска | После `ollama pull` модель кэшируется. Последующие запуски быстрее |
| **Модель «зависает» на длинном промпте** | Prefill (обработка входа) занимает время на CPU | Используйте `mlx-lm` для быстрого prefill на Apple Silicon |
| **Ollama выдаёт `OOM` на запуске** | Модель + KV cache не влезают | Уменьшите `num_ctx` до 4096 или возьмите модель меньше |
| **MLX работает медленнее Ollama** | MLX не оптимизирован для вашей модели | Проверьте, что модель в формате MLX (не GGUF через конвертацию) |

### Диагностика за 10 секунд

```bash
# 1. Сколько памяти ест модель сейчас?
ollama ps

# 2. Какой backend используется?
ollama ps  # смотрите колонку Engine (должен быть metal или mlx)

# 3. Есть ли swap?
memory_pressure | head -5
# "pressure: satisfied" — всё ок
# "pressure: under memory pressure" — ищите процесс-пожиратель

# 4. Кто ест память?
top -l 1 -n 10 -stats pid,command,mem -o mem | head -15
```

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

---

### 🎯 Полезные возможности Ollama для агентов

#### 1. Structured output (JSON Schema)

Ollama умеет возвращать ответ строго по JSON Schema — без `outlines` или `lm-format-enforcer`:

```bash
curl http://localhost:11434/api/chat -d '{
  "model": "qwen3.5:4b",
  "messages": [{"role": "user", "content": "Извлеки: имя, возраст, город из: Иван, 25 лет, Москва"}],
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
# → {"name": "Иван", "age": 25, "city": "Москва"}
```

Идеально для агентов: вызов функций, извлечение данных, AI pipelines.

#### 2. Tool calling (нативная поддержка)

Модель сама решает, какой инструмент вызвать и с какими аргументами:

```bash
curl http://localhost:11434/api/chat -d '{
  "model": "qwen3.5:4b",
  "messages": [{"role": "user", "content": "Какая погода в Токио?"}],
  "tools": [{
    "type": "function",
    "function": {
      "name": "get_weather",
      "description": "Get weather for a city",
      "parameters": {
        "type": "object",
        "properties": {
          "city": {"type": "string"},
          "units": {"type": "string", "enum": ["celsius", "fahrenheit"]}
        },
        "required": ["city"]
      }
    }
  }],
  "stream": false
}'
```

#### 3. OpenAI-совместимые эндпоинты

Любая библиотека для OpenAI работает с Ollama — просто смените `base_url`:

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
```

Поддерживаются: `/v1/chat/completions`, `/v1/completions`, `/v1/embeddings`, `/v1/models`.

#### 4. Быстрые интеграции через `ollama launch`

```bash
ollama launch claude       # Claude Code → использует Ollama
ollama launch codex        # Codex → использует Ollama
ollama launch openclaw     # AI assistant в WhatsApp/Telegram
ollama launch cline        # Cline VS Code integration
```

#### 5. Экспериментальный веб-поиск (без сторонних API)

```bash
curl http://localhost:11434/api/experimental/web_search -d '{
  "query": "последние новости AI 2026"
}'
```

Позволяет агенту искать информацию в интернете без отдельного Search API.

#### 6. Десктопные и мобильные клиенты

Ollama — это сервер (как локальный OpenAI API). Для повседневного общения удобно использовать клиенты:

| Инструмент | Платформа | Фишка |
|---|---|---|
| [**Enchanted**](https://github.com/gluonfield/enchanted) | macOS + iOS | Нативный SwiftUI, подключается к Ollama по URI, можно с iPhone до Mac |
| [**MindWork AI Studio**](https://github.com/MindWorkAI/AI-Studio) | macOS/Win/Linux | Универсальный: локальные (Ollama/llama.cpp) + облачные провайдеры в одном GUI |
| [**Open WebUI**](https://github.com/open-webui/open-webui) | Веб (Docker) | RAG, мультимодальность, управление пользователями |
| **LM Studio** | macOS/Win/Linux | GUI для скачивания и теста моделей |
| [**Continue**](https://github.com/continuedev/continue) | VS Code / JetBrains | AI-автодополнение кода с локальной моделью |

---

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