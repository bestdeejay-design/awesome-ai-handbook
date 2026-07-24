# 📄 RAG на своих документах

> Retrieval-Augmented Generation: Q&A по вашим файлам без отправки данных в облако.  
> От готовых решений до собственного RAG-пайплайна на Python.

🟢 **Не знакомы с AI?** [basics/](../basics/README.ru.md) — основы: что такое модели, как их запускать.

[← Практические кейсы](README.ru.md) · [Memory агента →](../agents/memory.ru.md)

---

## Содержание

1. [Что такое RAG](#1-что-такое-rag)
2. [Готовые решения](#2-готовые-решения)
3. [Свой RAG на Python + ChromaDB](#3-свой-rag-на-python--chromadb)
4. [Рекомендации по моделям](#4-рекомендации-по-моделям)
5. [Что дальше](#5-что-дальше)

---

## 1. Что такое RAG

RAG = Retrieval-Augmented Generation. Модель не просто отвечает из своих знаний, а сначала ищет релевантные куски в ваших документах и только потом формулирует ответ.

```
Вопрос → Поиск похожих кусков в документах → 
→ Контекст + вопрос → Модель формулирует ответ
```

**Когда нужен RAG:**
- Вопросы по внутренней документации компании
- Поиск по личным заметкам (Obsidian, Notion)
- Анализ юридических / медицинских документов
- Чат-бот по базе знаний

---

## 2. Готовые решения

### Open WebUI + Ollama (рекомендую)

Веб-интерфейс с RAG из коробки. Поддерживает 9 векторных БД.

```bash
docker run -d -p 3000:8080 \
  -e OLLAMA_BASE_URL=http://host.docker.internal:11434 \
  -v open-webui:/app/backend/data \
  --name open-webui \
  --restart always \
  ghcr.io/open-webui/open-webui:main
```

После запуска: откройте `http://localhost:3000` → Workpaces → загрузите документы.

### GPT4All — для одного пользователя

Десктопное приложение с RAG из коробки. Без Docker, без терминала.

```bash
brew install --cask gpt4all
```

Откройте → LocalDocs → укажите папку с документами.

### PrivateGPT — полностью офлайн

```bash
pip install private-gpt
private-gpt
```

Всё работает локально: эмбеддинги, генерация.

---

## 3. Свой RAG на Python + ChromaDB

Когда готовых решений недостаточно или нужна кастомная логика.

### Установка

```bash
pip install chromadb ollama
```

### Код RAG-пайплайна

```python
import chromadb
from chromadb.utils import embedding_functions
import requests
import os

class LocalRAG:
    """RAG-система на локальных моделях через Ollama."""
    
    def __init__(self, collection_name: str = "my_docs"):
        self.client = chromadb.Client()
        
        # Используем Ollama для эмбеддингов
        self.ollama_ef = embedding_functions.OllamaEmbeddingFunction(
            url="http://localhost:11434/api/embeddings",
            model_name="nomic-embed-text"
        )
        
        self.collection = self.client.get_or_create_collection(
            name=collection_name,
            embedding_function=self.ollama_ef
        )
    
    def add_document(self, text: str, metadata: dict = None, doc_id: str = None):
        """Добавить документ в базу."""
        self.collection.add(
            documents=[text],
            metadatas=[metadata or {}],
            ids=[doc_id or str(hash(text))]
        )
        print(f"  ✓ Добавлен документ ({len(text)} символов)")
    
    def add_directory(self, path: str):
        """Добавить все .txt и .md файлы из директории."""
        for filename in os.listdir(path):
            if filename.endswith((".txt", ".md", ".py", ".json")):
                filepath = os.path.join(path, filename)
                with open(filepath, "r") as f:
                    content = f.read()
                self.add_document(
                    content,
                    metadata={"source": filename},
                    doc_id=filename
                )
    
    def query(self, question: str, n_results: int = 3) -> str:
        """Задать вопрос по документам."""
        
        # 1. Ищем похожие куски
        results = self.collection.query(
            query_texts=[question],
            n_results=n_results
        )
        
        if not results["documents"][0]:
            return "Нет релевантных документов в базе"
        
        # 2. Собираем контекст
        context = "\n\n".join([
            f"[{meta.get('source', 'unknown')}]: {doc}"
            for doc, meta in zip(results["documents"][0], results["metadatas"][0])
        ])
        
        # 3. Отправляем модели с контекстом
        response = requests.post("http://localhost:11434/api/chat", json={
            "model": "qwen3.5:4b",
            "messages": [
                {"role": "system", "content": (
                    "Ты — Q&A ассистент. Отвечай на вопросы ТОЛЬКО на основе "
                    "предоставленных документов. Если в документах нет ответа — "
                    "так и скажи. Не придумывай факты."
                )},
                {"role": "user", "content": (
                    f"Контекст из документов:\n{context}\n\n"
                    f"Вопрос: {question}"
                )}
            ],
            "stream": False
        })
        
        answer = response.json()["message"]["content"]
        
        # 4. Показываем источники
        sources = [meta.get('source', 'unknown') 
                   for meta in results["metadatas"][0]]
        
        return f"{answer}\n\n📚 Источники: {', '.join(sources)}"


# === Пример использования ===
if __name__ == "__main__":
    rag = LocalRAG("handbook")
    
    # Индексируем документы
    print("Индексация документов...")
    rag.add_document(
        "Ollama — программа для запуска LLM локально. "
        "Устанавливается через brew install ollama.",
        {"source": "getting-started.md", "topic": "installation"}
    )
    rag.add_document(
        "Для RAG используйте Open WebUI или GPT4All. "
        "Эмбеддинги: nomic-embed-text.",
        {"source": "rag.md", "topic": "rag"}
    )
    
    # Задаём вопрос
    print("\nВопрос: Как установить Ollama?")
    print(rag.query("Как установить Ollama?"))
```

### Запуск

```bash
# Убедитесь, что скачана модель для эмбеддингов
ollama pull nomic-embed-text

# Запустите скрипт
python rag_example.py
```

---

## 4. Рекомендации по моделям

| Задача | Модель | Почему |
|--------|--------|--------|
| Генерация ответов | Qwen 3.5 9B | Баланс качества и RAM |
| Быстрые ответы | Qwen 3.5 4B | 28–35 tok/s |
| Эмбеддинги | `nomic-embed-text` | Оптимизирована для RAG |
| Альтернатива | `all-minilm` | Ещё легче (0.1 GB) |

Память и контекст — [local-models/memory-and-context.ru.md](../local-models/memory-and-context.ru.md).

---

## 5. Что дальше

| Если хотите | Переходите |
|-------------|-----------|
| **Понять, как работает векторная память** | [../agents/memory.ru.md](../agents/memory.ru.md) |
| **Использовать RAG в агенте** (tool calling) | [../agents/ollama-for-agents.ru.md](../agents/ollama-for-agents.ru.md) |
| **Выбрать модель** под RAG | [../local-models/models.ru.md](../local-models/models.ru.md) |
| **Вернуться к списку кейсов** | [README.ru.md](README.ru.md) |
