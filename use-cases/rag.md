# <img src="https://lucide.dev/api/icons/file-text" alt="" width="20" height="20" style="vertical-align:middle"> RAG on Your Documents

> Retrieval-Augmented Generation: Q&A over your files without cloud upload.

**🇷🇺 Russian version:** [rag.ru.md](rag.ru.md)

---

[← Use cases](README.md)

---

## Contents

1. [How RAG works](#1-how-rag-works)
2. [Quick start with Open WebUI](#2-quick-start-with-open-webui)
3. [DIY RAG in Python](#3-diy-rag-in-python)
4. [Choosing a vector DB](#4-choosing-a-vector-db)
5. [Whats next](#5-whats-next)

---

## 1. How RAG works

```
Documents → Chunks → Embeddings → Vector DB
                                          ↑
User question ────────────────────────────┘
    → Find similar chunks → LLM generates answer from context
```

RAG solves the hallucination problem: the model answers based on YOUR documents, not its training data.

---

## 2. Quick start with Open WebUI

```bash
docker run -d -p 3000:8080 \
  -v open-webui:/app/backend/data \
  -e OLLAMA_BASE_URL=http://host.docker.internal:11434 \
  ghcr.io/open-webui/open-webui:main
```

1. Open `http://localhost:3000`
2. Go to Workspace → Knowledge
3. Upload PDFs, Word, or text files
4. Chat with your documents

---

## 3. DIY RAG in Python

```bash
pip install chromadb sentence-transformers ollama
```

```python
import chromadb
from chromadb.utils import embedding_functions

# Load documents
with open("my_document.txt") as f:
    text = f.read()

# Chunk
chunk_size = 500
chunks = [text[i:i+chunk_size] for i in range(0, len(text), chunk_size)]

# Store
client = chromadb.Client()
collection = client.create_collection("my_docs")
collection.add(documents=chunks, ids=[f"chunk_{i}" for i in range(len(chunks))])

# Search
query = "What does the document say about AI?"
results = collection.query(query_texts=[query], n_results=3)
context = "\n".join(results["documents"][0])

# Generate
import requests
r = requests.post("http://localhost:11434/api/chat", json={
    "model": "qwen3.5:4b",
    "messages": [
        {"role": "system", "content": f"Answer based on this context:\n{context}"},
        {"role": "user", "content": query}
    ]
})
print(r.json()["message"]["content"])
```

---

## 4. Choosing a vector DB

| DB | Best for | Setup |
|----|----------|-------|
| **ChromaDB** | Prototypes, single user | pip install |
| **Qdrant** | Production, many users | Docker |
| **Milvus** | Billion-scale | Kubernetes |
| **FAISS** | Pure speed | pip install |

---

## 5. Whats next

| If you want | Go to |
|-------------|-------|
| Automate RAG pipelines | [automation.md](automation.md) |
| Agent memory with vectors | [memory.md](../agents/memory.md) |
| Model selection for embeddings | [models.md](../local-models/models.md) |
| Back | [README.md](README.md) |

---

**In section:** [coding](coding.md) · [rag](rag.md) · [automation](automation.md) · [writing](writing.md)  
**Related sections:** [Local Models](../local-models/README.md) · [AI Agents](../agents/README.md) · [Zero Level](../basics/README.md)  
**Navigation:** [← Use Cases](README.md) · [↑ Back to main](../README.md) · [🇷🇺 Русский](rag.ru.md)
