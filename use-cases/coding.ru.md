# 💻 AI для кодинга

Локальные модели и инструменты для разработки ПО.

[← Практические кейсы](README.ru.md)

---

## Инструменты

| Инструмент | Тип | Описание |
|---|---|---|
| [**Continue**](https://github.com/continuedev/continue) | IDE-плагин | Бесплатный Copilot с локальной моделью |
| [**Cursor**](https://cursor.sh/) | AI-IDE | IDE с AI из коробки |
| [**Aider**](https://github.com/paul-gauthier/aider) | CLI | Агент для правок в git-репозитории |
| [**OpenHands**](https://github.com/All-Hands-AI/OpenHands) | Автономный агент | Software engineer в контейнере |

---

## Связка для M1 16 GB

```
Continue IDE → Ollama (localhost:11434) → Qwen 2.5 Coder 7B
```

**Модели:**
- Автодополнение (tab): `qwen2.5-coder:1.5b` — 30+ tok/s
- Chat / edit: `qwen2.5-coder:7b` — HumanEval 82%, FIM

Каталог кодинг-моделей — в [local-models/catalog.md](../local-models/catalog.md#1-кодинг-модели).

---

## Пример `config.json` для Continue

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

---

## Aider + Ollama

```bash
pip install aider-install
aider --model ollama/qwen2.5-coder:7b
```

Подробнее об Aider — в [local-models/tools.md](../local-models/tools.md#aider--cli-агент-для-кодинга).

---

## Связанные материалы

- [local-models/running-models.md](../local-models/running-models.ru.md) — запуск Ollama
- [agents/ollama-for-agents.md](../agents/ollama-for-agents.ru.md) — API для агентов
- [agents/tutorials/](../agents/tutorials/) — туториалы (скоро)


---

## Что дальше

| Если хотите | Переходите |
|-------------|-----------|
| **Вернуться к списку кейсов** | [README.ru.md](README.ru.md) |
| **Настроить модели** под свою задачу | [../local-models/models.ru.md](../local-models/models.ru.md) |
| **Разобраться с инструментами** | [../local-models/tools.ru.md](../local-models/tools.ru.md) |
| **Построить агента** для этого сценария | [../agents/ollama-for-agents.ru.md](../agents/ollama-for-agents.ru.md) — API, tool calling |
