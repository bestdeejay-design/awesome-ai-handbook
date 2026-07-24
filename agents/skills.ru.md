# 🧩 Скиллы для AI-агентов: что это, где взять, как создать

> Скиллы (skills, плагины, MCP-серверы) — это модульные расширения, которые дают агенту новые возможности: работу с файлами, поиск в интернете, доступ к базам данных, генерацию изображений и многое другое.
>
> Без скиллов агент — просто болталка. Со скиллами — инструмент, который решает реальные задачи.

[← Готовые AI-агенты](ready-made.ru.md) · [Архитектура AI-агента →](architecture.ru.md)

---

## Содержание

1. [Что такое скиллы](#1-что-такое-скиллы)
2. [Сравнение платформ](#2-сравнение-платформ)
3. [MCP — универсальный стандарт](#3-mcp--универсальный-стандарт)
4. [Где брать готовые скиллы](#4-где-брать-готовые-скиллы)
5. [Как создать свой скилл](#5-как-создать-свой-скилл)
6. [Скиллы этого хендбука](#6-скиллы-этого-хендбука)
7. [Что дальше](#7-что-дальше)

---

## 1. Что такое скиллы

Скилл — это дополнительная способность, которую вы даёте агенту. Агент сам решает, когда её использовать, в зависимости от задачи.

**Пример:** вы просите агента «нарисуй иконку для приложения». Агент понимает, что не умеет рисовать, находит подходящий скилл генерации изображений, вызывает его — и возвращает результат.

Разные платформы называют скиллы по-разному:

| Платформа | Название | Формат |
|-----------|----------|--------|
| OpenCode / Codex CLI | Skills | Markdown + инструкции |
| Continue.dev | Context Providers + MCP | JSON + JS/TS |
| Cline | MCP Servers | JSON |
| Cursor | Rules | Markdown (.cursorrules) |
| Aider | Conventions + Instructions | Markdown |
| Claude Code | CLAUDE.md | Markdown |
| OpenHands | Microagents | Markdown |
| GitHub Copilot | Custom Instructions | Markdown |
| **Любой MCP-клиент** | **MCP-сервер** | **JS/Python/Go/…** |

Несмотря на разные названия, принцип один: **вы описываете, что умеет скилл, и агент вызывает его когда нужно**.

---

## 2. Сравнение платформ

### OpenCode / Codex CLI — Skills

**Формат:** Markdown-файлы с инструкциями + shell-скрипты.  
**Где лежат:** В папке `.opencode/skills/` или в конфиге `opencode.json`.  
**Как работают:** Агент читает описание скилла и решает, нужно ли его применить.

#### Простейший скилл

```markdown
# test-graphics

Генерация тестовых изображений, фото, иконок, placeholders для проектов.
Использует Python + бесплатные API (loremflickr, placehold.co).

Команда: python test-graphics.py --width 800 --height 600 --output icon.png
```

Скилл загружается через конфиг:
```json
{
  "skills": ["test-graphics"]
}
```

### Continue.dev — Context Providers + MCP

**Формат:** JSON-конфиг + код контекст-провайдера.  
**Где лежат:** `~/.continue/config.json` + `~/.continue/plugins/`.  
**Как работают:** Провайдеры подгружают контекст (файлы, терминал, git), MCP-серверы добавляют инструменты.

```json
{
  "contextProviders": [
    {"name": "file"},
    {"name": "terminal"}
  ],
  "experimental": {
    "mcpServers": [
      {"name": "playwright", "command": "npx", "args": ["@playwright/mcp"]}
    ]
  }
}
```

### Cline — MCP Servers

**Формат:** JSON.  
**Где лежат:** `~/.vscode/extensions/cline/` или в настройках расширения.  
**Как работают:** MCP-серверы подключаются как внешние инструменты.

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/allowed/dir"]
    }
  }
}
```

### Cursor — Rules

**Формат:** Markdown-файлы.  
**Где лежат:** `.cursorrules` (глобальные) или `.cursor/rules/` (проектные).  
**Как работают:** Агент читает правила и следует им при генерации кода.

### Aider — Conventions

**Формат:** Markdown + YAML.  
**Где лежат:** `CONVENTIONS.md` (в корне репозитория) или `.aider.conf.yml`.  
**Как работают:** Aider читает conventions и следует стилю кода.

### Claude Code — CLAUDE.md

**Формат:** Markdown.  
**Где лежат:** `CLAUDE.md` в корне проекта.  
**Как работают:** Claude Code читает этот файл при старте и следует инструкциям.

### OpenHands — Microagents

**Формат:** Markdown.  
**Где лежат:** `.openhands/microagents/`.  
**Как работают:** Microagent — это файл с инструкцией для агента: что он должен знать о проекте, какие команды выполнять.

### GitHub Copilot — Custom Instructions

**Формат:** Markdown.  
**Где лежат:** `.github/copilot-instructions.md`.  
**Как работают:** Copilot читает инструкции при генерации кода.

---

## 3. MCP — универсальный стандарт

**Model Context Protocol (MCP)** — открытый протокол от Anthropic, который позволяет агентам подключать любые внешние инструменты через единый интерфейс.

**Как работает:**
```
Агент (клиент) ←→ MCP-сервер ←→ Внешний сервис (БД, API, файлы, браузер, …)
```

MCP поддерживают:

| Платформа | Статус MCP |
|-----------|------------|
| Claude Desktop | ✅ Нативно |
| Claude Code | ✅ Нативно |
| Cursor | ✅ Нативно |
| Continue.dev | ✅ Experimental |
| Cline | ✅ Нативно |
| VS Code (GitHub Copilot) | ✅ Через расширение |
| OpenCode / Codex CLI | ✅ Через MCP-конфиг |
| Windsurf | ✅ Нативно |

**Где брать MCP-серверы:**

| Ресурс | Серверов | Ссылка |
|--------|:--------:|--------|
| MCP Directory | 2,300+ | [mcp.directory](https://mcp.directory) |
| MCP Servers | 9,800+ | [mcpservers.org](https://mcpservers.org) |
| Awesome MCP Servers | 2,300+ | [github.com/punkpeye/awesome-mcp-servers](https://github.com/punkpeye/awesome-mcp-servers) |
| MCP Trove | 520+ | [mcptrove.com](https://mcptrove.com) |
| findarepo MCP | 256+ | [findarepo.com/skills/mcp/](https://findarepo.com/skills/mcp/) |

**Категории MCP-серверов:**
- 🗄️ **Базы данных** — Postgres, SQLite, MySQL, Turso
- 🌐 **Поиск и Web** — DuckDuckGo, Brave, Jina, Context7
- 🧰 **Инструменты разработчика** — GitHub, Git, Docker, Kubernetes
- 🤖 **AI/ML** — Ollama, embeddings, RAG
- 🗂️ **Файлы и хранилища** — Local filesystem, Google Drive, S3
- 🌍 **Браузеры** — Playwright, Puppeteer
- 💬 **Коммуникации** — Slack, Telegram, Email
- 📊 **Аналитика** — BigQuery, Prometheus, Datadog

---

## 4. Где брать готовые скиллы

### Каталоги MCP-серверов

Самый большой выбор: [mcpservers.org](https://mcpservers.org) — 9,800+ серверов.  
По категориям: [mcp.directory](https://mcp.directory) — 2,300+ серверов.  
С рейтингом: [findarepo.com/skills/mcp/](https://findarepo.com/skills/mcp/) — 256+ лучших.

### Репозитории со скиллами

| Платформа | Где искать | Формат |
|-----------|------------|--------|
| OpenCode / Codex CLI | `.opencode/skills/` в проектах | Markdown |
| Continue.dev | [continue.dev/plugins](https://continue.dev/plugins) | JSON |
| Aider | [aider.chat/docs/usage/conventions.html](https://aider.chat/docs/usage/conventions.html) | Markdown |
| Claude Code | `CLAUDE.md` в open-source проектах | Markdown |
| Cursor | [cursor.directory](https://cursor.directory) — правила от сообщества | Markdown |

### Awesome-списки

- [awesome-mcp-servers](https://github.com/punkpeye/awesome-mcp-servers) — курируемый список
- [awesome-cursorrules](https://github.com/PatrickJS/awesome-cursorrules) — правила для Cursor
- [awesome-continue](https://github.com/continuedev/awesome-continue) — плагины для Continue

---

## 5. Как создать свой скилл

Процесс одинаков для любой платформы:

1. **Определите, что делает скилл** — одну конкретную вещь
2. **Напишите инструкцию** — опишите, когда агенту вызывать скилл
3. **Подключите к агенту** — через конфиг или файл в нужной директории

### Пример: скилл генерации тестовых изображений

Разберём на примере скилла `test-graphics`, который мы создали для этого проекта.

**Что делает:** Генерирует тестовые изображения, иконки, фото-заглушки через бесплатные API.

**Инструкция для агента (`.opencode/skills/test-graphics/SKILL.md`):**

```markdown
# test-graphics

Генерация тестовых изображений, фото, иконок, placeholders для проектов.
Без претензий к качеству — только заполнить данные.

## Когда использовать
- Нужно тестовое изображение для макета
- Нужна иконка-заглушка
- Нужно заполнить страницу картинками для демонстрации

## Команда
python test-graphics.py --width <W> --height <H> --output <file> [--type icon|photo|placeholder]

## Примеры
- python test-graphics.py --width 800 --height 600 --output hero.png --type photo
- python test-graphics.py --width 64 --height 64 --output icon.png --type icon

## Зависимости
- Python 3
- requests
```

**Код скилла (`test-graphics.py`):**

```python
#!/usr/bin/env python3
"""Генерация тестовых изображений через бесплатные API."""
import argparse, requests

def generate_placeholder(width, height, text=""):
    url = f"https://placehold.co/{width}x{height}?text={text or f'{width}x{height}'}"
    return url

def generate_photo(width, height):
    return f"https://loremflickr.com/{width}/{height}"

if __name__ == "__main__":
    parser = argparse.ArgumentParser()
    parser.add_argument("--width", type=int, default=800)
    parser.add_argument("--height", type=int, default=600)
    parser.add_argument("--type", choices=["photo", "icon", "placeholder"], default="placeholder")
    parser.add_argument("--output", required=True)
    args = parser.parse_args()

    if args.type == "photo":
        url = generate_photo(args.width, args.height)
    else:
        url = generate_placeholder(args.width, args.height, args.type)

    img = requests.get(url).content
    with open(args.output, "wb") as f:
        f.write(img)
    print(f"✓ {args.output} ({args.width}x{args.height})")
```

**Подключение:** В `opencode.json`:

```json
{
  "skills": ["test-graphics"]
}
```

**Результат:** Теперь, когда вы попросите агента «сгенерируй иконку 64×64 для теста», он:
1. Поймёт, что это задача для скилла `test-graphics`
2. Запустит скрипт с правильными параметрами
3. Вернёт готовый файл

Этот же скилл можно адаптировать под любую платформу:
- Для **Continue** — оформить как MCP-сервер
- Для **Cline** — добавить в `mcpServers`
- Для **Cursor** — описать в `.cursorrules`

---

## 6. Скиллы этого хендбука

По мере развития Awesome AI Handbook мы добавляем скиллы для разных платформ.  
Следите за папкой:

```
awesome-ai-handbook/
└── .opencode/
    └── skills/
        └── test-graphics/    ← пример выше
```

**Планируется:**
- `handbook-qa` — поиск по содержанию хендбука
- `model-benchmark` — запуск бенчмарков на локальном железе
- `agent-scaffold` — генерация кода агента под выбранный фреймворк

---

## 7. Что дальше

| Если хотите | Переходите |
|-------------|-----------|
| **Выбрать готового агента** для своих задач | [Готовые AI-агенты](ready-made.ru.md) |
| **Построить своего агента** с нуля | [Архитектура AI-агента](architecture.ru.md) |
| **Подключить локальную модель** к агенту | [Ollama для агентов](ollama-for-agents.ru.md) |
| **Найти MCP-сервер** под свою задачу | [mcp.directory](https://mcp.directory) (внешний) |
| **Вернуться в агентский раздел** | [README.ru.md](README.ru.md) |
