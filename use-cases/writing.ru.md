# ✍️ Письмо и контент

> Генерация текстов, копирайтинг, переводы и контент-пайплайны на локальных моделях.

[← Практические кейсы](README.ru.md) · [Reflection-паттерн →](../agents/patterns.ru.md#3-reflection--self-correction)

---

## Содержание

1. [Рекомендуемые модели](#1-рекомендуемые-модели)
2. [Квантизация для текстов](#2-квантизация-для-текстов)
3. [Workflow с Reflection-агентом](#3-workflow-с-reflection-агентом)
4. [Массовая генерация: скрипт для контента](#4-массовая-генерация-скрипт-для-контента)
5. [RAG для контента](#5-rag-для-контента)
6. [Что дальше](#6-что-дальше)

---

## 1. Рекомендуемые модели

| Задача | Модель | Запуск | tok/s | Почему |
|--------|--------|--------|-------|--------|
| **Быстрые черновики** | Qwen 3.5 4B | `ollama run qwen3.5:4b` | 28–35 | Хороший русский, быстрый |
| **Качественные тексты** | Qwen 3.5 9B | `ollama run qwen3.5:9b` | 10–13 | Лучший баланс на 16 GB |
| **Классика** | Llama 3.1 8B | `ollama run llama3.1:8b` | 14–18 | Стабильный стиль |
| **Аналитика / отчёты** | Phi-4-mini | `ollama run phi4-mini` | 25–30 | Лучший для логики |

---

## 2. Квантизация для текстов

Для письма и аналитики имеет смысл **Q5_K_M** вместо Q4_K_M — если модель влезает в RAM:

| Формат | Когда использовать | Разница |
|--------|-------------------|---------|
| Q4_K_M | По умолчанию, черновики | Быстро, 3.5% потери качества |
| Q5_K_M | Финальные тексты, аналитика | 2.5% потери, чуть больше памяти |
| Q6_K | Юридические / медицинские формулировки | 1.6% потери, заметно лучше |

```bash
# Пример: запуск модели с Q5_K_M
ollama pull qwen3.5:9b:q5_k_m
ollama run qwen3.5:9b:q5_k_m
```

Подробнее — [local-models/quantization.ru.md](../local-models/quantization.ru.md).

---

## 3. Workflow с Reflection-агентом

Один проход модели даёт «сырой» результат. Reflection добавляет самопроверку:

```
Черновик (Qwen 4B, быстро) → Редактор-агент (Qwen 9B + Reflection) → Финал
```

### Реализация

```python
import requests

OLLAMA = "http://localhost:11434/api/chat"

def draft(text: str) -> str:
    """Быстрый черновик."""
    r = requests.post(OLLAMA, json={
        "model": "qwen3.5:4b",
        "messages": [{"role": "user", "content": text}],
        "stream": False
    })
    return r.json()["message"]["content"]

def review(text: str) -> str:
    """Критический разбор."""
    r = requests.post(OLLAMA, json={
        "model": "qwen3.5:9b",
        "messages": [
            {"role": "system", "content": (
                "Ты — редактор. Найди ошибки, неточности, слабые места в тексте. "
                "Проверь: факты, грамматику, стиль, структуру."
            )},
            {"role": "user", "content": text}
        ],
        "stream": False
    })
    return r.json()["message"]["content"]

def improve(text: str, critique: str) -> str:
    """Улучшение на основе критики."""
    r = requests.post(OLLAMA, json={
        "model": "qwen3.5:9b",
        "messages": [
            {"role": "system", "content": "Улучши текст на основе критики. Верни только финальную версию."},
            {"role": "user", "content": f"Оригинал:\n{text}\n\nКритика:\n{critique}\n\nУлучшенный текст:"}
        ],
        "stream": False
    })
    return r.json()["message"]["content"]

# Полный пайплайн
def write_with_reflection(topic: str) -> str:
    """Пишет текст с самопроверкой."""
    print("  ✏️ Черновик...")
    raw = draft(f"Напиши краткую статью на тему: {topic}")
    print("  🔍 Рецензия...")
    critique = review(raw)
    print("  ✨ Финальная версия...")
    final = improve(raw, critique)
    return final

# Пример
article = write_with_reflection("Преимущества локальных AI-моделей")
print(article)
```

**Результат:** текст проходит три стадии — генерация → критика → улучшение. Качество заметно выше, чем за один проход.

---

## 4. Массовая генерация: скрипт для контента

```python
#!/usr/bin/env python3
"""content_generator.py — массовая генерация текстов через Ollama."""

import requests
import json
import csv
from pathlib import Path

OLLAMA = "http://localhost:11434/api/chat"

def generate(topic: str, tone: str = "neutral") -> str:
    """Генерирует текст по теме с заданным тоном."""
    
    tones = {
        "neutral": "Напиши информативный текст.",
        "professional": "Напиши деловой текст. Используй профессиональную лексику.",
        "friendly": "Напиши дружелюбный, разговорный текст.",
        "persuasive": "Напиши убедительный текст с призывом к действию."
    }
    
    response = requests.post(OLLAMA, json={
        "model": "qwen3.5:4b",
        "messages": [
            {"role": "system", "content": tones.get(tone, tones["neutral"])},
            {"role": "user", "content": f"Тема: {topic}"}
        ],
        "stream": False
    })
    return response.json()["message"]["content"]


# Генерация из CSV-файла
def generate_from_csv(csv_path: str, output_dir: str = "output"):
    """Генерирует тексты для каждой строки CSV."""
    
    Path(output_dir).mkdir(exist_ok=True)
    
    with open(csv_path) as f:
        reader = csv.DictReader(f)
        for row in reader:
            text = generate(row["topic"], row.get("tone", "neutral"))
            
            filename = row.get("filename", f"{row['topic'][:30]}.md")
            filepath = Path(output_dir) / filename
            filepath.write_text(text)
            print(f"  ✓ {filename}")


if __name__ == "__main__":
    # Пример: массовая генерация описаний
    topics = [
        {"topic": "Что такое RAG в контексте LLM", "tone": "professional"},
        {"topic": "Как выбрать модель для кодинга", "tone": "friendly"},
        {"topic": "Преимущества локального AI перед облачным", "tone": "persuasive"},
    ]
    
    for item in topics:
        text = generate(item["topic"], item["tone"])
        print(f"\n📄 {item['topic']}")
        print(text[:200] + "...")
```

---

## 5. RAG для контента

Для текстов на основе ваших материалов (документация, база знаний) — используйте RAG:

```
Тема → Поиск похожих материалов в базе → Контекст + запрос → Текст
```

Подробнее — [rag.ru.md](rag.ru.md).

---

## 6. Что дальше

| Если хотите | Переходите |
|-------------|-----------|
| **Использовать Reflection-агента** для контента | [../agents/patterns.ru.md](../agents/patterns.ru.md#3-reflection--self-correction) |
| **Собрать контент-команду** из агентов | [../agents/tutorials/02-agent-team.ru.md](../agents/tutorials/02-agent-team.ru.md) |
| **Выбрать модели** для текстов | [../local-models/catalog.ru.md](../local-models/catalog.ru.md#2-общие-чатинструкционные-модели) |
| **Вернуться к списку кейсов** | [README.ru.md](README.ru.md) |
