# 🔴 Туториал 3: Агент-программист

> Coding-агент: читает код, находит баги, пишет тесты и делает PR.  
> **LangGraph + Ollama** — для максимального контроля.

[← Туториалы](README.ru.md) · [Команда агентов](02-agent-team.ru.md)

---

## Что вы сделаете

Агента-программиста, который:
1. Читает файлы проекта
2. Анализирует код на ошибки
3. Пишет тесты
4. Исправляет баги
5. Создаёт коммит с изменениями

---

## Шаг 1: Установка

```bash
pip install langgraph langchain-ollama
```

---

## Шаг 2: Код агента

Создайте файл `coding_agent.py`:

```python
import os
import json
import requests
from typing import TypedDict, Annotated
from langgraph.graph import StateGraph, START, END
from langgraph.graph.message import add_messages
from langchain_ollama import ChatOllama
from langchain_core.tools import tool

OLLAMA = "http://localhost:11434"
MODEL = "qwen2.5-coder:7b"  # используем coding-модель

# =============================================
# 1. ИНСТРУМЕНТЫ
# =============================================

@tool
def read_file(path: str) -> str:
    """Читает содержимое файла по указанному пути."""
    try:
        with open(path, "r") as f:
            return f.read()
    except Exception as e:
        return f"Ошибка чтения: {e}"

@tool
def write_file(path: str, content: str) -> str:
    """Записывает содержимое в файл."""
    try:
        with open(path, "w") as f:
            f.write(content)
        return f"Файл {path} сохранён"
    except Exception as e:
        return f"Ошибка записи: {e}"

@tool
def list_files(path: str = ".") -> str:
    """Показывает список файлов в директории."""
    try:
        files = []
        for f in os.listdir(path):
            if f.startswith("."):
                continue
            files.append(f)
        return "\n".join(files)
    except Exception as e:
        return f"Ошибка: {e}"

@tool
def run_tests() -> str:
    """Запускает тесты через pytest."""
    result = os.popen("python -m pytest --tb=short 2>&1").read()
    return result[:2000]  # ограничиваем вывод

tools = [read_file, write_file, list_files, run_tests]

# =============================================
# 2. СОСТОЯНИЕ АГЕНТА
# =============================================

class AgentState(TypedDict):
    messages: Annotated[list, add_messages]
    task: str

# =============================================
# 3. МОДЕЛЬ
# =============================================

llm = ChatOllama(model=MODEL, base_url=OLLAMA)
llm_with_tools = llm.bind_tools(tools)

# =============================================
# 4. УЗЛЫ ГРАФА
# =============================================

def agent_node(state: AgentState):
    """Узел агента: вызывает LLM с инструментами."""
    result = llm_with_tools.invoke(state["messages"])
    return {"messages": [result]}

def should_continue(state: AgentState) -> str:
    """Решает, продолжать или завершить."""
    last_message = state["messages"][-1]
    if last_message.tool_calls:
        return "tools"
    return END

# =============================================
# 5. ГРАФ
# =============================================

from langgraph.prebuilt import ToolNode

graph = StateGraph(AgentState)

graph.add_node("agent", agent_node)
graph.add_node("tools", ToolNode(tools))

graph.add_edge(START, "agent")
graph.add_conditional_edges("agent", should_continue, {
    "tools": "tools",
    END: END
})
graph.add_edge("tools", "agent")

agent = graph.compile()

# =============================================
# 6. ЗАПУСК
# =============================================

def run_coding_agent(task: str):
    """Запускает coding-агента с задачей."""
    
    system_prompt = (
        "Ты — Senior Python-разработчик. Твои принципы:\n"
        "1. Сначала прочитай код, потом меняй\n"
        "2. Пиши тесты до изменений (TDD)\n"
        "3. Следи, чтобы старые тесты проходили\n"
        "4. Не удаляй существующий функционал\n"
        "5. Добавляй type hints и docstrings\n\n"
        "План работы:\n"
        "1. Прочитай файлы проекта (list_files, read_file)\n"
        "2. Запусти существующие тесты (run_tests)\n"
        "3. Внеси изменения (write_file)\n"
        "4. Запусти тесты снова\n"
        "5. Сообщи результат"
    )
    
    result = agent.invoke({
        "messages": [
            {"role": "system", "content": system_prompt},
            {"role": "user", "content": task}
        ],
        "task": task
    })
    
    return result["messages"][-1].content


# === Пример использования ===
if __name__ == "__main__":
    # Создаём тестовый проект
    os.makedirs("test_project", exist_ok=True)
    
    with open("test_project/calculator.py", "w") as f:
        f.write("""
def add(a, b):
    return a + b

def divide(a, b):
    return a / b  # баг: нет проверки на 0
""")
    
    with open("test_project/test_calculator.py", "w") as f:
        f.write("""
from calculator import add, divide

def test_add():
    assert add(2, 3) == 5

def test_divide():
    assert divide(10, 2) == 5
""")
    
    # Запускаем агента
    result = run_coding_agent(
        "Найди баги в test_project/calculator.py, "
        "исправь их и добавь тесты для граничных случаев"
    )
    
    print("=" * 50)
    print("🤖 CODING AGENT RESULT:")
    print(result)
    
    # Проверяем, что агент сделал
    print("\n" + "=" * 50)
    print("📁 Итоговый код calculator.py:")
    print(open("test_project/calculator.py").read())
```

---

## Шаг 3: Запуск

```bash
python coding_agent.py
```

---

## Как это работает

```
1. Агент получает задачу + system prompt
2. Агент: list_files → видит calculator.py и test_calculator.py
3. Агент: read_file("calculator.py") → видит баг (деление на 0)
4. Агент: write_file → исправляет баг
5. Агент: read_file("test_calculator.py") → видит, что нет теста на деление на 0
6. Агент: write_file → добавляет тест
7. Агент: run_tests → все тесты проходят
8. Агент: сообщает результат
```

---

## Что дальше

| Если хотите | Переходите |
|-------------|-----------|
| Подключить агента к GitHub | использовать `git` как инструмент |
| Добавить Code Review | Reflection-паттерн из [patterns.md](../patterns.ru.md) |
| Запускать агента по расписанию | cron + скрипт |
| Вернуться к туториалам | [README.ru.md](README.ru.md) |
