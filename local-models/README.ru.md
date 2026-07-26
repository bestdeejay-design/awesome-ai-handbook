# 🖥 Локальные модели

> Запуск LLM на своём оборудовании: от первой установки до продвинутой настройки.

[← Awesome AI Handbook](../README.ru.md) · [🇬🇧 English](README.md)

---

## С чего начать — выберите сценарий

### <img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> Абсолютный новичок
> Никогда не запускал модели, не знаком с терминалом.

**Сначала:** [basics/README.ru.md](../basics/README.ru.md) — что такое AI, как работают модели, какое нужно железо.

**Затем установка:** [<img src="https://lucide.dev/api/icons/book-open" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> Подготовка окружения и установка](getting-started.ru.md) — Homebrew, Ollama, первая модель за 10 минут.

Или вообще без терминала: **LM Studio** → описание в [running-models.md](running-models.ru.md).

### <img src="assets/icons/level-yellow.svg" alt="" width="12" height="12" style="vertical-align:middle"> Хочу разобраться
> Уже установил Ollama, запускал модель, но хочет понимать что и как.

1. [<img src="https://lucide.dev/api/icons/play" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> Как найти и запустить модель](running-models.ru.md) — практикум
2. [<img src="https://lucide.dev/api/icons/target" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> Выбор модели под задачу](models.ru.md) — что выбрать для кодинга / чата / RAG
3. [💾 Память и контекст](memory-and-context.ru.md) — почему модель не влезает и как это исправить

### <img src="assets/icons/level-blue.svg" alt="" width="12" height="12" style="vertical-align:middle"> Продвинутый пользователь
> Хочу выжать максимум, настроить API, сравнить инструменты.

1. [<img src="https://lucide.dev/api/icons/cog" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> Продвинутая настройка Ollama](advanced-setup.ru.md) — Modelfile, API, env vars
2. [<img src="https://lucide.dev/api/icons/wrench" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> Сравнение инструментов](tools.ru.md) — Ollama vs LM Studio vs MLX vs llama.cpp
3. [<img src="https://lucide.dev/api/icons/scale" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> Квантизация](quantization.ru.md) — Q4, Q5, Q8 — что выбрать
4. [<img src="https://lucide.dev/api/icons/bar-chart-3" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> Бенчмарки Apple Silicon](benchmarks/apple-silicon.ru.md) — tok/s на M1–M4

### <img src="https://lucide.dev/api/icons/book-open" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> Справочники (для всех)

| Раздел | О чём |
|--------|-------|
| [<img src="https://lucide.dev/api/icons/book-open" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> Каталог моделей](catalog.ru.md) | 50+ моделей с характеристиками |
| [<img src="https://lucide.dev/api/icons/alert-triangle" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> Частые проблемы](troubleshooting.ru.md) | Диагностика и решения |

---

## Файлы раздела

| # | Файл | Для кого | Время |
|---|------|----------|-------|
| 1 | [<img src="https://lucide.dev/api/icons/rocket" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> getting-started.md](getting-started.ru.md) | Новички | 10 мин |
| 2 | [<img src="https://lucide.dev/api/icons/play" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> running-models.md](running-models.ru.md) | Все | 15 мин |
| 3 | [<img src="https://lucide.dev/api/icons/target" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> models.md](models.ru.md) | Все | 10 мин |
| 4 | [💾 memory-and-context.md](memory-and-context.ru.md) | Все | 10 мин |
| 5 | [<img src="https://lucide.dev/api/icons/cog" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> advanced-setup.md](advanced-setup.ru.md) | Продвинутые | 15 мин |
| 6 | [<img src="https://lucide.dev/api/icons/wrench" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> tools.md](tools.ru.md) | Все | 20 мин |
| 7 | [<img src="https://lucide.dev/api/icons/scale" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> quantization.md](quantization.ru.md) | Продвинутые | 10 мин |
| 8 | [<img src="https://lucide.dev/api/icons/bar-chart-3" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> benchmarks/apple-silicon.md](benchmarks/apple-silicon.ru.md) | Все | 5 мин |
| 9 | [<img src="https://lucide.dev/api/icons/alert-triangle" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> troubleshooting.md](troubleshooting.ru.md) | При проблемах | 5 мин |
| 10 | [<img src="https://lucide.dev/api/icons/book-open" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> catalog.md](catalog.ru.md) | Справочник | — |

---

## Связанные разделы

- [<img src="assets/icons/level-green.svg" alt="" width="12" height="12" style="vertical-align:middle"> Нулевой уровень](../basics/README.ru.md) — основы AI, установка
- [<img src="https://lucide.dev/api/icons/bot" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> AI-агенты](../agents/README.ru.md) — агенты на локальных моделях
- [<img src="https://lucide.dev/api/icons/briefcase" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> Use Cases](../use-cases/README.ru.md) — кодинг, RAG, автоматизация

---

**Навигация:** [← На главную](../README.ru.md) · [<img src="https://lucide.dev/api/icons/book-open" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> Каталог](../catalog.ru.md)
