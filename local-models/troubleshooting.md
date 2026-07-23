# ⚠️ Частые проблемы и решения

Диагностика и типичные ошибки при локальном запуске LLM.

[← Локальные модели](README.md) · [Память и контекст](memory-and-context.md)

---

## Таблица проблем

| Проблема | Причина | Решение |
|---|---|---|
| **2–5 tok/s (очень медленно)** | Модель не влезает в RAM → swap на SSD | Уменьшите модель/квантизацию. На 8 GB Mac 8B модель → **0.4 tok/s** — бесполезно |
| **Metal не используется** | Ollama не переключена на Metal | `ollama ps` — проверьте backend. Должен быть `metal`, не `cpu` |
| **Jetsam убивает Ollama** | macOS защищает память (OOM-killer) | Берите модель меньше и `num_ctx` 2048–4096 |
| **Крашится на длинных ответах** | KV cache съел всю RAM | `OLLAMA_KV_CACHE_TYPE=q8_0` — уменьшит кэш в 2× |
| **Скорость падает после Chrome** | Браузер ест 2–4 GB — модели остаётся меньше | Закройте Chrome. На 16 GB Mac Chrome + IDE + модель = гарантированный swap |
| **Долгая загрузка модели** | Первый запуск читает с диска | После `ollama pull` модель кэшируется. Последующие запуски быстрее |
| **Модель «зависает» на длинном промпте** | Prefill (обработка входа) занимает время на CPU | Используйте `mlx-lm` для быстрого prefill на Apple Silicon |
| **Ollama выдаёт `OOM` на запуске** | Модель + KV cache не влезают | Уменьшите `num_ctx` до 4096 или возьмите модель меньше |
| **MLX работает медленнее Ollama** | MLX не оптимизирован для вашей модели | Проверьте, что модель в формате MLX (не GGUF через конвертацию) |

---

## Диагностика за 10 секунд

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

## Связанные материалы

- [memory-and-context.md](memory-and-context.md) — формулы RAM, KV cache, `num_ctx`
- [quantization.md](quantization.md) — выбор Q4/Q5/IQ2
- [running-models.md](running-models.md) — настройка Ollama на Mac
