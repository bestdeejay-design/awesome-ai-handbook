# 💾 Память и управление состоянием агентов

> Как агенты помнят контекст, хранят информацию между шагами и не теряют нить разговора.

[← AI-агенты](README.ru.md) · [Промптинг →](prompting.ru.md)

---

## Содержание

1. [Зачем агенту память](#1-зачем-агенту-память)
2. [Типы памяти](#2-типы-памяти)
3. [Short-term memory (рабочая)](#3-short-term-memory-рабочая)
4. [Long-term memory (долговременная)](#4-long-term-memory-долговременная)
5. [Memory в популярных фреймворках](#5-memory-в-популярных-фреймворках)
6. [Проблемы и решения](#6-проблемы-и-решения)
7. [Что дальше](#7-что-дальше)

---

## 1. Зачем агенту память

Без памяти каждый новый запрос для модели — как разговор с незнакомцем. Она не помнит:

- Что уже сделала в этом цикле
- Какие результаты получила
- Что пользователь просил 10 шагов назад
- Какие решения уже приняла

**Для твоей команды агентов** память критична:
- PM-агент должен помнить, какие задачи уже поставлены
- Разработчик — какие изменения уже сделаны
- Тестировщик — какие баги уже найдены

---

## 2. Типы памяти

```
┌────────────────────────────────────────────────────┐
│                   ПАМЯТЬ АГЕНТА                     │
│                                                    │
│  ┌──────────────────────────────────────────┐      │
│  │         SHORT-TERM (рабочая)             │      │
│  │  · Текущий диалог с LLM                  │      │
│  │  · Все сообщения в цикле                 │      │
│  │  · Ограничена контекстным окном          │      │
│  └──────────────────────────────────────────┘      │
│                                                    │
│  ┌──────────────────────────────────────────┐      │
│  │         LONG-TERM (долговременная)       │      │
│  │  · Между сессиями работы                 │      │
│  │  · Векторная БД / файлы / БД             │      │
│  │  · Не ограничена контекстом              │      │
│  └──────────────────────────────────────────┘      │
│                                                    │
│  ┌──────────────────────────────────────────┐      │
│  │         WORKING (состояние задачи)        │      │
│  │  · Текущий план и прогресс               │      │
│  │  · Что сделано, что осталось             │      │
│  │  · Промежуточные результаты              │      │
│  └──────────────────────────────────────────┘      │
└────────────────────────────────────────────────────┘
```

---

## 3. Short-term memory (рабочая)

Это **контекстное окно модели** — все сообщения, которые были отправлены в текущем цикле.

### Как работает

```python
messages = [
    {"role": "system", "content": "Ты — PM-агент..."},
    {"role": "user", "content": "Какие задачи нужно сделать?"},
    {"role": "assistant", "content": "Нужно: 1. Написать API 2. Сделать UI"},
    {"role": "user", "content": "Оцени сроки по каждому пункту"},
    # → каждое новое сообщение добавляется сюда
]
```

### Проблема: переполнение контекста

Каждый шаг цикла агента добавляет ~500-5000 токенов. После 10 шагов контекст может вырасти до 50000 токенов, что превышает лимит модели.

### Решение: компрессия контекста

```python
def compress_messages(messages, max_messages=10, model="qwen3.5:4b"):
    """Сжимает историю, если она слишком длинная."""
    
    if len(messages) <= max_messages:
        return messages
    
    # Берём системный промпт + последние N-1 сообщений
    system = [m for m in messages if m["role"] == "system"][:1]
    recent = messages[-(max_messages-1):]
    
    # Если всё ещё слишком длинно — суммаризируем середину
    total_tokens = sum(len(m.get("content", "")) for m in messages)
    if total_tokens > 10000:
        middle = messages[1:-(max_messages-1)]
        summary = requests.post("http://localhost:11434/api/chat", json={
            "model": model,
            "messages": [
                {"role": "system", "content": "Суммаризируй историю диалога кратко"},
                {"role": "user", "content": json.dumps(middle)}
            ],
            "stream": False
        })
        summary_text = summary.json()["message"]["content"]
        
        return system + [
            {"role": "system", "content": f"Краткая история: {summary_text}"}
        ] + recent
    
    return system + recent
```

### Когда применять компрессию

- ✅ Цикл агента превысил 5 шагов
- ✅ Общий размер сообщений > 70% контекстного окна
- ❌ На 2-3 шагах — избыточно

---

## 4. Long-term memory (долговременная)

Short-term память живёт, пока работает агент. Long-term память сохраняется между запусками.

### Способы хранения

| Метод | Где хранить | Когда использовать |
|-------|-------------|-------------------|
| **Файлы JSON** | `agent_memory.json` | Простые проекты, один пользователь |
| **SQLite** | `memory.db` | Несколько агентов, структурированные данные |
| **Векторная БД** | Chroma, Qdrant | Поиск по семантической памяти |
| **Git** | Коммиты в репозиторий | Для агентов-программистов |

### Пример: файловая память

```python
import json
import os

class FileMemory:
    """Простая долговременная память в JSON-файле."""
    
    def __init__(self, filepath="agent_memory.json"):
        self.filepath = filepath
        self.data = self._load()
    
    def _load(self) -> dict:
        if os.path.exists(self.filepath):
            with open(self.filepath) as f:
                return json.load(f)
        return {"projects": {}, "decisions": [], "facts": []}
    
    def save(self):
        with open(self.filepath, "w") as f:
            json.dump(self.data, f, ensure_ascii=False, indent=2)
    
    def remember_fact(self, fact: str):
        """Запомнить факт о проекте."""
        self.data["facts"].append({
            "fact": fact,
            "timestamp": __import__("datetime").datetime.now().isoformat()
        })
        self.save()
    
    def get_project_state(self, project: str) -> dict:
        """Получить состояние проекта."""
        return self.data["projects"].get(project, {})
    
    def update_project(self, project: str, key: str, value):
        """Обновить состояние проекта."""
        if project not in self.data["projects"]:
            self.data["projects"][project] = {}
        self.data["projects"][project][key] = value
        self.save()


# Использование в агенте
memory = FileMemory()

# PM-агент запоминает решение
memory.remember_fact("Решили использовать FastAPI для бэкенда")
memory.update_project("awesome-ai-handbook", "status", "в разработке")

# Другой агент читает
print(memory.get_project_state("awesome-ai-handbook"))
```

### Пример: векторная память (ChromaDB)

```python
# pip install chromadb
import chromadb
from chromadb.utils import embedding_functions

class VectorMemory:
    """Долговременная память с поиском по смыслу."""
    
    def __init__(self, collection_name="agent_memory"):
        self.client = chromadb.Client()
        self.collection = self.client.get_or_create_collection(
            name=collection_name,
            embedding_function=embedding_functions.DefaultEmbeddingFunction()
        )
    
    def add(self, text: str, metadata: dict = None):
        """Добавить информацию в память."""
        self.collection.add(
            documents=[text],
            metadatas=[metadata or {}],
            ids=[str(hash(text))]
        )
    
    def search(self, query: str, n_results: int = 3) -> list:
        """Найти похожие записи."""
        results = self.collection.query(
            query_texts=[query],
            n_results=n_results
        )
        return results["documents"][0]


# Агент запоминает контекст
memory = VectorMemory()
memory.add(
    "Пользователь попросил сделать TODO-приложение с веб-интерфейсом",
    {"project": "todo-app", "type": "requirement"}
)

# Позже другой агент ищет
relevant = memory.search("Что хотел пользователь?")
print(relevant)
```

---

## 5. Memory в популярных фреймворках

### LangGraph — checkpointing

LangGraph автоматически сохраняет состояние между шагами:

```python
from langgraph.checkpoint.memory import MemorySaver

memory_saver = MemorySaver()

agent = graph.compile(checkpointer=memory_saver)

# При каждом вызове состояние сохраняется
config = {"configurable": {"thread_id": "project-123"}}
agent.invoke({"messages": [("user", "Привет!")]}, config)
agent.invoke({"messages": [("user", "Что я только что сказал?")]}, config)
# Помнит!
```

### CrewAI — встроенная память

```python
from crewai import Crew, Process
from crewai.memory import Memory

crew = Crew(
    agents=[...],
    tasks=[...],
    memory=Memory(
        short_term=True,   # помнит в рамках одной сессии
        long_term=True,    # помнит между сессиями
        entity=True        # помнит сущности (имена, даты)
    )
)
```

### Agno — session state

```python
from agno.agent import Agent
from agno.models.ollama import Ollama

agent = Agent(
    model=Ollama(id="qwen3.5:4b"),
    session_state={}  # произвольные данные
)

# Сохраняем между вызовами
agent.session_state["project"] = "awesome-ai-handbook"
agent.run("Запомни: мы делаем handbook по AI")
```

---

## 6. Проблемы и решения

| Проблема | Описание | Решение |
|----------|----------|---------|
| **Забывание контекста** | Модель «забывает» начало диалога | Суммаризация истории каждые N шагов |
| **Разрастание контекста** | Слишком много сообщений → превышение лимита | Компрессия, удаление устаревших сообщений |
| **Противоречивая память** | Один агент записал одно, другой — другое | Централизованное хранилище, версионирование |
| **Утечка данных** | Чувствительные данные остаются в истории | Scoping, очистка после завершения задачи |
| **Зависимость от embeddings** | Векторный поиск может найти не то | Всегда проверять релевантность результатов |

---

## 7. Что дальше

| Если хотите | Переходите |
|-------------|-----------|
| Научиться писать промпты для агентов | [prompting.ru.md](prompting.ru.md) |
| Понять, как оркестрировать команду агентов | [multi-agent.md](multi-agent.ru.md) |
| Обезопасить агентов | [safety.ru.md](safety.ru.md) |
| Собрать свою команду | [tutorials/02-agent-team.ru.md](tutorials/02-agent-team.ru.md) |
| Вернуться к навигации | [README.md](README.ru.md) |
