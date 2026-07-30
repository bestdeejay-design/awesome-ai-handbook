# <img src="https://lucide.dev/api/icons/file-text" alt="" width="20" height="20" style="vertical-align:middle"> RAG on Your Documents

> Retrieval-Augmented Generation: Q&A over your files without cloud upload.  
> From ready-made solutions to your own RAG pipeline in Python.

**🇷🇺 Russian version:** [rag.ru.md](rag.ru.md)

---

[← Use cases](README.md) · [Agent memory →](../agents/memory.md)

---

## Contents

1. [How RAG works](#1-how-rag-works)
2. [Ready-made solutions](#2-ready-made-solutions)
3. [DIY RAG in Python + ChromaDB](#3-diy-rag-in-python--chromadb)
4. [Choosing a vector DB](#4-choosing-a-vector-db)
5. [Model recommendations](#5-model-recommendations)
6. [What's next](#6-whats-next)

---

## 1. How RAG works

RAG = Retrieval-Augmented Generation. The model doesn't just answer from its training data — it first searches for relevant chunks in your documents and only then formulates an answer.

```
Documents → Chunks → Embeddings → Vector DB
                                          ↑
User question ────────────────────────────┘
    → Find similar chunks → LLM generates answer from context
```

RAG solves the hallucination problem: the model answers based on YOUR documents, not its training data.

**When you need RAG:**
- Questions about internal company documentation
- Searching personal notes (Obsidian, Notion)
- Analyzing legal / medical documents
- Chatbot over a knowledge base

---

## 2. Ready-made solutions

### Open WebUI + Ollama (recommended)

Web interface with built-in RAG. Supports 9 vector databases.

```bash
docker run -d -p 3000:8080 \
  -e OLLAMA_BASE_URL=http://host.docker.internal:11434 \
  -v open-webui:/app/backend/data \
  --name open-webui \
  --restart always \
  ghcr.io/open-webui/open-webui:main
```

After launch: open `http://localhost:3000` → Workspace → Knowledge → upload your documents.

### GPT4All — for a single user

Desktop app with built-in RAG. No Docker, no terminal.

```bash
brew install --cask gpt4all
```

Open it → LocalDocs → point to a folder with your documents.

### PrivateGPT — fully offline

```bash
pip install private-gpt
private-gpt
```

Everything runs locally: embeddings, generation.

---

## 3. DIY RAG in Python + ChromaDB

When ready-made solutions aren't enough or you need custom logic.

### Installation

```bash
pip install chromadb ollama
```

### RAG pipeline code

```python
import chromadb
from chromadb.utils import embedding_functions
import requests
import os

class LocalRAG:
    """RAG system on local models via Ollama."""

    def __init__(self, collection_name: str = "my_docs"):
        self.client = chromadb.Client()

        # Use Ollama for embeddings
        self.ollama_ef = embedding_functions.OllamaEmbeddingFunction(
            url="http://localhost:11434/api/embeddings",
            model_name="nomic-embed-text"
        )

        self.collection = self.client.get_or_create_collection(
            name=collection_name,
            embedding_function=self.ollama_ef
        )

    def add_document(self, text: str, metadata: dict = None, doc_id: str = None):
        """Add a document to the database."""
        self.collection.add(
            documents=[text],
            metadatas=[metadata or {}],
            ids=[doc_id or str(hash(text))]
        )
        print(f"  ✓ Document added ({len(text)} chars)")

    def add_directory(self, path: str):
        """Add all .txt and .md files from a directory."""
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
        """Ask a question about the documents."""

        # 1. Find similar chunks
        results = self.collection.query(
            query_texts=[question],
            n_results=n_results
        )

        if not results["documents"][0]:
            return "No relevant documents in the database"

        # 2. Build context
        context = "\n\n".join([
            f"[{meta.get('source', 'unknown')}]: {doc}"
            for doc, meta in zip(results["documents"][0], results["metadatas"][0])
        ])

        # 3. Send to model with context
        response = requests.post("http://localhost:11434/api/chat", json={
            "model": "qwen3.5:4b",
            "messages": [
                {"role": "system", "content": (
                    "You are a Q&A assistant. Answer questions ONLY based on "
                    "the provided documents. If the documents don't contain "
                    "an answer, say so. Don't make up facts."
                )},
                {"role": "user", "content": (
                    f"Document context:\n{context}\n\n"
                    f"Question: {question}"
                )}
            ],
            "stream": False
        })

        answer = response.json()["message"]["content"]

        # 4. Show sources
        sources = [meta.get('source', 'unknown')
                   for meta in results["metadatas"][0]]

        return f"{answer}\n\n📚 Sources: {', '.join(sources)}"


# === Usage example ===
if __name__ == "__main__":
    rag = LocalRAG("handbook")

    # Index documents
    print("Indexing documents...")
    rag.add_document(
        "Ollama is a program for running LLMs locally. "
        "Install via brew install ollama.",
        {"source": "getting-started.md", "topic": "installation"}
    )
    rag.add_document(
        "For RAG use Open WebUI or GPT4All. "
        "Embeddings: nomic-embed-text.",
        {"source": "rag.md", "topic": "rag"}
    )

    # Ask a question
    print("\nQuestion: How to install Ollama?")
    print(rag.query("How to install Ollama?"))
```

### Running

```bash
# Make sure the embedding model is downloaded
ollama pull nomic-embed-text

# Run the script
python rag_example.py
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

## 5. Model recommendations

| Task | Model | Why |
|------|-------|-----|
| Answer generation | Qwen 3.5 9B | Balance of quality and RAM |
| Fast answers | Qwen 3.5 4B | 28–35 tok/s |
| Embeddings | `nomic-embed-text` | Optimized for RAG |
| Alternative | `all-minilm` | Even lighter (0.1 GB) |

Memory and context — [local-models/memory-and-context.md](../local-models/memory-and-context.md).

---

## 6. What's next

| If you want | Go to |
|-------------|-------|
| **Understand how vector memory works** | [../agents/memory.md](../agents/memory.md) |
| **Use RAG in an agent** (tool calling) | [../agents/ollama-for-agents.md](../agents/ollama-for-agents.md) |
| **Choose a model** for RAG | [../local-models/models.md](../local-models/models.md) |
| **Automate RAG pipelines** | [automation.md](automation.md) |
| **Back to use cases** | [README.md](README.md) |

---

**In section:** [coding](coding.md) · [rag](rag.md) · [automation](automation.md) · [writing](writing.md)  
**Related sections:** [Local Models](../local-models/README.md) · [AI Agents](../agents/README.md) · [Zero Level](../basics/README.md)  
**Navigation:** [← Use Cases](README.md) · [↑ Back to main](../README.md) · [🇷🇺 Русский](rag.ru.md)
