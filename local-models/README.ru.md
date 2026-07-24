# 🖥 Локальные модели

Запуск LLM на своём оборудовании: от первой установки до продвинутой настройки.

[← Awesome AI Handbook (русская версия)](../README.ru.md) · [English version](README.md)

---

## 🤔 С чего начать — выберите свой сценарий

### 🟢 Абсолютный новичок
> Никогда не запускал модели, не знаком с терминалом, хочет просто попробовать.

**Сначала основы:** [basics/README.ru.md](../basics/README.ru.md) — что такое AI, как работают модели, какое нужно железо.

**Затем установка:** [📖 Подготовка окружения и установка](getting-started.ru.md)
→ установка Homebrew, Ollama, первая модель за 10 минут

Или вообще без терминала: **LM Studio** → описание в [running-models.md](running-models.ru.md)

### 🟡 Хочу разобраться
> Уже установил Ollama, запускал модель, но хочет понимать что и как.

**Маршрут:**
1. [▶️ Как найти и запустить модель](running-models.ru.md) — практикум
2. [🎯 Выбор модели под задачу](models.ru.md) — что выбрать для кодинга / чата / RAG
3. [💾 Память и контекст](memory-and-context.ru.md) — почему модель не влезает и как это исправить

### 🔵 Продвинутый пользователь
> Хочу выжать максимум, настроить API, сравнить инструменты.

**Маршрут:**
1. [⚙️ Продвинутая настройка Ollama](advanced-setup.ru.md) — Modelfile, API, env vars
2. [🛠 Сравнение инструментов](tools.ru.md) — Ollama vs LM Studio vs MLX vs llama.cpp
3. [⚖️ Квантизация](quantization.ru.md) — Q4, Q5, Q8 — что выбрать
4. [📊 Бенчмарки Apple Silicon](benchmarks/apple-silicon.ru.md) — tok/s на M1–M4

### 📚 Справочники (для всех)
| Раздел | О чём |
|--------|-------|
| [📚 Каталог моделей](catalog.ru.md) | 50+ моделей с характеристиками |
| [⚠️ Частые проблемы](troubleshooting.ru.md) | Диагностика и решения |

---

## 🗺 Все разделы

| # | Файл | Для кого | Время чтения |
|---|------|----------|-------------|
| 1 | [🚀 getting-started.md](getting-started.ru.md) | Новички | 10 мин |
| 2 | [▶️ running-models.md](running-models.ru.md) | Все | 15 мин |
| 3 | [🎯 models.md](models.ru.md) | Все | 10 мин |
| 4 | [💾 memory-and-context.md](memory-and-context.ru.md) | Все | 10 мин |
| 5 | [⚙️ advanced-setup.md](advanced-setup.ru.md) | Продвинутые | 15 мин |
| 6 | [🛠 tools.md](tools.ru.md) | Все | 20 мин |
| 7 | [⚖️ quantization.md](quantization.ru.md) | Продвинутые | 10 мин |
| 8 | [📊 benchmarks/apple-silicon.md](benchmarks/apple-silicon.ru.md) | Все | 5 мин |
| 9 | [⚠️ troubleshooting.md](troubleshooting.ru.md) | При проблемах | 5 мин |
| 10 | [📚 catalog.md](catalog.ru.md) | Справочник | — |

---

## 📋 План развития раздела

- [x] Базовая структура файлов
- [x] Связность: ссылки «дальше» в каждом файле
- [ ] **Детализация:** пошаговые инструкции в getting-started.md
- [ ] **Детализация:** примеры API и Modelfile в advanced-setup.md
- [ ] **Детализация:** проверка и уточнение всех cross-reference ссылок
- [ ] **Английская версия:** перевод всех файлов раздела
