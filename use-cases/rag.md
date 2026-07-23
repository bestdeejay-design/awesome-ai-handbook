# 📄 RAG на своих документах

Retrieval-Augmented Generation: Q&A по вашим файлам без отправки данных в облако.

[← Практические кейсы](README.md)

---

## Стек

### Вариант 1: Open WebUI + Ollama

```
Документы → Open WebUI (индексация) → Ollama (эмбеддинги + генерация)
```

- [Open WebUI](https://github.com/open-webui/open-webui) — веб-интерфейс с RAG из коробки
- Модель: `qwen3.5:4b` или `nomic-embed-text` для эмбеддингов

```bash
# Docker
docker run -d -p 3000:8080 \
  -e OLLAMA_BASE_URL=http://host.docker.internal:11434 \
  ghcr.io/open-webui/open-webui:main
```

### Вариант 2: PrivateGPT

- [**PrivateGPT**](https://github.com/zylon-ai/private-gpt) — всё в одном, полностью оффлайн

### Вариант 3: GPT4All

- [**GPT4All**](https://github.com/nomic-ai/gpt4all) — встроенная RAG-система, GUI

---

## Применение

- Q&A по внутренней документации компании
- Поиск по личным заметкам (Obsidian, Notion экспорт)
- Анализ юридических / медицинских документов

---

## Рекомендации по моделям

| Задача | Модель | Почему |
|--------|--------|--------|
| Генерация ответов | Qwen 3.5 9B | Баланс качества и RAM |
| Быстрые ответы | Qwen 3.5 4B | Меньше памяти |
| Эмбеддинги | `nomic-embed-text` | Оптимизированы для RAG |

Память и контекст — [local-models/memory-and-context.md](../local-models/memory-and-context.md).

---

## Связанные материалы

- [local-models/tools.md](../local-models/tools.md) — Open WebUI, GPT4All
- [agents/ollama-for-agents.md](../agents/ollama-for-agents.md) — `/v1/embeddings`
