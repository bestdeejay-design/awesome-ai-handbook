# ⚙️ AI + автоматизация

Связка локальных моделей с бизнес-процессами и скриптами.

[← Практические кейсы](README.md)

---

## Связки

| Компонент | Роль |
|-----------|------|
| **Ollama** | Локальная модель (API на :11434) |
| [**n8n**](https://github.com/n8n-io/n8n) | Self-hosted автоматизация, HTTP-ноды к Ollama |
| [**Make**](https://www.make.com/) / [**Zapier**](https://zapier.com/) | No-code интеграции с OpenAI-совместимым API |
| **cron + curl** | Периодическая обработка данных |

---

## Примеры сценариев

### Саммари входящих писем

```
IMAP trigger → n8n → Ollama (qwen3.5:4b) → Telegram / Slack
```

### Обработка CSV / логов

```
cron → script → Ollama structured output (JSON) → база данных
```

Structured output — см. [agents/ollama-for-agents.md](../agents/ollama-for-agents.md#1-structured-output-json-schema).

---

## OpenAI-совместимый API

Любой сервис с поддержкой OpenAI API может использовать Ollama:

```
base_url: http://localhost:11434/v1
api_key: ollama
model: qwen3.5:4b
```

---

## Агенты в автоматизации

Для многошаговых сценариев — LangGraph или CrewAI поверх Ollama:

- [agents/frameworks.md](../agents/frameworks.md)
- [agents/patterns.md](../agents/patterns.md)

---

## Связанные материалы

- [use-cases/rag.md](rag.md) — документы и базы знаний
- [local-models/running-models.md](../local-models/running-models.md) — настройка Ollama
