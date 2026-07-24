# ⚙️ AI + автоматизация

> Связка локальных моделей с бизнес-процессами, скриптами и расписаниями.

[← Практические кейсы](README.ru.md) · [Мульти-агентные системы →](../agents/multi-agent.ru.md)

---

## Содержание

1. [Связки](#1-связки)
2. [Базовый скрипт: обработка через Ollama](#2-базовый-скрипт-обработка-через-ollama)
3. [cron + Ollama: периодическая обработка](#3-cron--ollama-периодическая-обработка)
4. [n8n + Ollama: визуальная автоматизация](#4-n8n--ollama-визуальная-автоматизация)
5. [Telegram-бот с локальной моделью](#5-telegram-бот-с-локальной-моделью)
6. [Что дальше](#6-что-дальше)

---

## 1. Связки

| Компонент | Роль | Установка |
|-----------|------|-----------|
| **Ollama** | Локальная модель (API на :11434) | `brew install ollama` |
| **Python скрипт** | Логика, вызов API, обработка | Встроен в macOS |
| **cron** | Запуск по расписанию | Встроен в macOS/Linux |
| [**n8n**](https://github.com/n8n-io/n8n) | Self-hosted визуальная автоматизация | `npx n8n` |
| [**Make**](https://www.make.com/) | No-code облачная автоматизация | Регистрация на сайте |

---

## 2. Базовый скрипт: обработка через Ollama

Самый простой способ автоматизации — Python-скрипт, который вызывает Ollama по API.

```python
#!/usr/bin/env python3
"""ai_process.py — универсальный скрипт для обработки текста через Ollama."""

import requests
import json
import sys

OLLAMA = "http://localhost:11434/api/chat"
MODEL = "qwen3.5:4b"

def ai_process(text: str, instruction: str = "Ответь кратко") -> str:
    """Отправляет текст модели и возвращает ответ."""
    
    response = requests.post(OLLAMA, json={
        "model": MODEL,
        "messages": [
            {"role": "system", "content": instruction},
            {"role": "user", "content": text}
        ],
        "stream": False
    })
    return response.json()["message"]["content"]

def ai_process_json(text: str, json_schema: dict) -> dict:
    """Отправляет текст и получает структурированный JSON."""
    
    response = requests.post(OLLAMA, json={
        "model": MODEL,
        "messages": [{"role": "user", "content": text}],
        "format": json_schema,
        "stream": False
    })
    return json.loads(response.json()["message"]["content"])


# Примеры использования
if __name__ == "__main__":
    # Простой вызов
    result = ai_process(
        "Напиши краткое саммари: 'Компания выпустила новую версию продукта...'",
        "Ты — ассистент. Составляй краткие саммари (2-3 предложения)."
    )
    print(f"📝 {result}")
    
    # JSON-вывод
    schema = {
        "type": "object",
        "properties": {
            "summary": {"type": "string"},
            "sentiment": {"type": "string", "enum": ["positive", "negative", "neutral"]},
            "keywords": {"type": "array", "items": {"type": "string"}}
        }
    }
    data = ai_process_json(
        "Продукт отличный, но интерфейс немного запутанный",
        schema
    )
    print(f"📊 {json.dumps(data, ensure_ascii=False, indent=2)}")
```

---

## 3. cron + Ollama: периодическая обработка

Запуск скрипта каждый день в 9 утра для обработки новых данных.

```bash
# Создайте скрипт обработки
cat > ~/scripts/daily_report.sh << 'EOF'
#!/bin/bash
cd ~/scripts
# Прочитать входящие данные
cat today_notes.txt | \
python3 -c "
import sys, requests
text = sys.stdin.read()
resp = requests.post('http://localhost:11434/api/chat', json={
    'model': 'qwen3.5:4b',
    'messages': [
        {'role': 'system', 'content': 'Составь краткий отчёт по заметкам'},
        {'role': 'user', 'content': text}
    ],
    'stream': False
})
print(resp.json()['message']['content'])
" > daily_report.md

echo "Отчёт сохранён: daily_report.md"
EOF

# Сделайте исполняемым
chmod +x ~/scripts/daily_report.sh

# Добавьте в crontab
echo "0 9 * * * $HOME/scripts/daily_report.sh" | crontab -
```

**Важно:** cron использует绝对 пути. Убедитесь, что `ollama serve` запущен.

---

## 4. n8n + Ollama: визуальная автоматизация

n8n позволяет строить автоматизации без кода.

### Установка

```bash
npx n8n
```

### Готовый workflow: саммари входящих писем

```
[IMAP Email] → [HTTP Request (Ollama)] → [Slack/Telegram]
```

**Настройка HTTP-ноды для Ollama:**
```
Method: POST
URL: http://localhost:11434/api/chat
Body (JSON):
{
  "model": "qwen3.5:4b",
  "messages": [
    {"role": "system", "content": "Составь краткое саммари письма (3-5 предложений)"},
    {"role": "user", "content": "={{ $json.emailBody }}"
  ],
  "stream": false
}
```

### Примеры workflow

| Сценарий | Триггер | Действие |
|----------|---------|----------|
| Саммари писем | Новое письмо (IMAP) | Ollama → Telegram |
| Классификация тикетов | Новый тикет (webhook) | Ollama → Jira |
| Перевод документов | Новый файл в папке | Ollama → сохранение |
| Мониторинг новостей | RSS фид | Ollama → база данных |

---

## 5. Telegram-бот с локальной моделью

```python
#!/usr/bin/env python3
"""telegram_ai_bot.py — Telegram-бот на локальной модели."""

import requests
import logging
from http.server import HTTPServer, BaseHTTPRequestHandler

OLLAMA = "http://localhost:11434/api/chat"
MODEL = "qwen3.5:4b"

class AIBotHandler(BaseHTTPRequestHandler):
    """Простой HTTP-сервер, который принимает запросы и отвечает через AI."""
    
    def do_POST(self):
        content_length = int(self.headers['Content-Length'])
        body = self.rfile.read(content_length).decode()
        
        # Отправляем в Ollama
        response = requests.post(OLLAMA, json={
            "model": MODEL,
            "messages": [
                {"role": "system", "content": "Ты — полезный ассистент. Отвечай кратко."},
                {"role": "user", "content": body}
            ],
            "stream": False
        })
        
        answer = response.json()["message"]["content"]
        
        self.send_response(200)
        self.send_header('Content-type', 'application/json')
        self.end_headers()
        self.wfile.write(f'{{"response": "{answer}"}}'.encode())

# Запуск сервера на порту 8888
if __name__ == "__main__":
    server = HTTPServer(("localhost", 8888), AIBotHandler)
    print("🤖 AI Bot running on http://localhost:8888")
    server.serve_forever()
```

Для полноценного Telegram-бота используйте `python-telegram-bot` + Ollama.

---

## 6. Что дальше

| Если хотите | Переходите |
|-------------|-----------|
| **Создать multi-agent систему** для автоматизации | [../agents/multi-agent.ru.md](../agents/multi-agent.ru.md) |
| **Написать агента-исполнителя** с инструментами | [../agents/tutorials/01-first-agent.ru.md](../agents/tutorials/01-first-agent.ru.md) |
| **Разобраться с API Ollama** | [../agents/ollama-for-agents.ru.md](../agents/ollama-for-agents.ru.md) |
| **Вернуться к списку кейсов** | [README.ru.md](README.ru.md) |
