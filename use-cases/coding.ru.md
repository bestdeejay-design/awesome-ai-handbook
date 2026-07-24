# 💻 AI для кодинга

> Локальные модели и инструменты для разработки ПО: от автодополнения до автономных агентов.

[← Практические кейсы](README.ru.md) · [Пошаговый туториал →](../agents/tutorials/03-coding-agent.ru.md)

---

## Содержание

1. [Инструменты](#1-инструменты)
2. [Связка для M1 16 GB](#2-связка-для-m1-16-gb)
3. [Continue.dev — бесплатный Copilot](#3-continuedev--бесплатный-copilot)
4. [Aider — CLI-агент для кода](#4-aider--cli-агент-для-кода)
5. [OpenHands — автономный software engineer](#5-openhands--автономный-software-engineer)
6. [Что дальше](#6-что-дальше)

---

## 1. Инструменты

| Инструмент | Тип | Описание | Когда использовать |
|-----------|-----|----------|-------------------|
| [**Continue**](https://github.com/continuedev/continue) | IDE-плагин | Бесплатный Copilot с локальной моделью | Повседневная разработка в IDE |
| [**Cursor**](https://cursor.sh/) | AI-IDE | IDE с AI из коробки, Tab+Chat+Agent | Если хотите всё в одном |
| [**Aider**](https://github.com/paul-gauthier/aider) | CLI | Агент для правок в git-репозитории | Рефакторинг, многофайловые изменения |
| [**OpenHands**](https://github.com/All-Hands-AI/OpenHands) | Автономный агент | Software engineer в контейнере | Автономное выполнение задач |

---

## 2. Связка для M1 16 GB

```
Continue IDE → Ollama (localhost:11434) → Qwen 2.5 Coder 7B
```

**Модели для кодинга:**

| Модель | Размер | tok/s (M1 16GB) | Для чего |
|--------|--------|-----------------|----------|
| `qwen2.5-coder:1.5b` | ~1.1 GB | 30+ | Автодополнение (tab) |
| `qwen2.5-coder:7b` | ~4.7 GB | 22–25 | Чат, редактирование кода |
| `qwen3.5:4b` | ~3.4 GB | 28–35 | Быстрый чат, объяснение кода |
| `phi4-mini` | ~2.5 GB | 25–30 | Рефакторинг, логика |

Каталог кодинг-моделей — [local-models/catalog.ru.md](../local-models/catalog.ru.md#1-кодинг-модели).

---

## 3. Continue.dev — бесплатный Copilot

Continue — плагин для VS Code и JetBrains, который подключается к Ollama.

### Установка

```bash
# Установите плагин Continue в VS Code
# Настройте config.json:
```

### Конфиг для локальных моделей

```json
{
  "models": [
    {
      "title": "Qwen 2.5 Coder 7B (основной)",
      "provider": "ollama",
      "model": "qwen2.5-coder:7b",
      "apiBase": "http://localhost:11434"
    }
  ],
  "tabAutocompleteModel": {
    "title": "Qwen 2.5 Coder 1.5B (быстрый)",
    "provider": "ollama",
    "model": "qwen2.5-coder:1.5b",
    "apiBase": "http://localhost:11434"
  },
  "experimental": {
    "roles": [
      {
        "name": "coder",
        "model": "qwen2.5-coder:7b"
      },
      {
        "name": "architect",
        "model": "qwen3.5:4b"
      }
    ]
  }
}
```

### Что можно делать

- **Tab autocomplete** — модель предлагает код пока печатаете
- **Chat** — выделили код → Ctrl+L → спросили «что это?»
- **Inline edit** — выделили код → Ctrl+I → «перепиши на TypeScript»
- **@codebase** — вопросы по всему репозиторию (через embeddings)

---

## 4. Aider — CLI-агент для кода

Aider работает в терминале и сам правит файлы в git-репозитории.

### Установка

```bash
pip install aider-install
aider-install
```

### Запуск с Ollama

```bash
# Простой режим
aider --model ollama/qwen2.5-coder:7b

# Режим архитектора (2 модели: одна планирует, вторая пишет код)
aider --model ollama/qwen2.5-coder:7b \
  --editor-model ollama/qwen3:4b

# Режимы работы
aider --chat-mode ask       # вопросы о коде
aider --chat-mode code      # написание кода
aider --chat-mode architect # архитектор + редактор
```

### Пример сессии

```bash
$ aider --model ollama/qwen2.5-coder:7b

> Добавь обработку ошибок в функцию parse_config()

Aider прочитает файл, добавит try/except,
проверит что код работает — и сделает коммит.
```

Подробнее об Aider — [local-models/tools.ru.md](../local-models/tools.ru.md#aider--cli-агент-для-кодинга).

---

## 5. OpenHands — автономный software engineer

OpenHands (бывший OpenDevin) запускается в Docker и может автономно выполнять задачи.

```bash
docker run -it --rm \
  -e LLM_MODEL=qwen2.5-coder:7b \
  -e LLM_API_KEY=ollama \
  -e LLM_BASE_URL=http://host.docker.internal:11434 \
  -v $(pwd)/workspace:/workspace \
  ghcr.io/all-hands-ai/openhands:latest
```

OpenHands создаёт план → пишет код → запускает тесты → исправляет ошибки — полностью автономно.

---

## 6. Что дальше

| Если хотите | Переходите |
|-------------|-----------|
| **Попробовать coding-агента** с нуля | [03-coding-agent.ru.md](../agents/tutorials/03-coding-agent.ru.md) — пошаговый туториал |
| **Собрать команду** разработчиков-агентов | [02-agent-team.ru.md](../agents/tutorials/02-agent-team.ru.md) |
| **Выбрать модели** для кодинга | [../local-models/models.ru.md](../local-models/models.ru.md) |
| **Настроить Ollama** под кодинг | [../local-models/advanced-setup.ru.md](../local-models/advanced-setup.ru.md) |
| **Вернуться к списку кейсов** | [README.ru.md](README.ru.md) |
