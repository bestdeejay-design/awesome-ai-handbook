# 🖥 Локальные модели

Запуск LLM на своём оборудовании: выбор модели, инструментов и настройка под ваше железо.

[← Главная](../README.ru.md)

---

## Содержание

| Документ | Описание |
|----------|----------|
| [running-models.md](running-models.md) | Ollama, LM Studio, HuggingFace — практикум |
| [memory-and-context.md](memory-and-context.md) | RAM, KV cache, `num_ctx`, рекомендации по железу |
| [catalog.md](catalog.md) | Полный каталог 50+ open-weight моделей |
| [models.md](models.md) | Выбор модели по задаче и объёму памяти |
| [tools.md](tools.md) | Ollama, MLX, llama.cpp, LM Studio и др. |
| [quantization.md](quantization.md) | GGUF, MLX, TurboQuant |
| [benchmarks/apple-silicon.md](benchmarks/apple-silicon.md) | tok/s на M1–M4 |
| [troubleshooting.md](troubleshooting.md) | Частые проблемы и диагностика |

---

## Быстрый старт

```bash
brew install ollama    # macOS
# curl -fsSL https://ollama.com/install.sh | sh  # Linux

ollama run qwen3.5:4b        # чат
ollama run qwen2.5-coder:7b  # кодинг
```

Подробнее — [running-models.md](running-models.md).

---

## С чего начать

1. **Новичок** → [running-models.md](running-models.md) → [models.md](models.md)
2. **Выбор модели** → [memory-and-context.md](memory-and-context.md) → [catalog.md](catalog.md)
3. **Медленно / падает** → [troubleshooting.md](troubleshooting.md)
4. **Нужна скорость** → [tools.md](tools.md) + [benchmarks/apple-silicon.md](benchmarks/apple-silicon.md)
