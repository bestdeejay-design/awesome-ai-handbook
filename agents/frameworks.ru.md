# 🏗 Фреймворки для AI-агентов

> Сравнение и примеры кода для популярных фреймворков — с фокусом на локальные модели через Ollama.

[← AI-агенты](README.ru.md) · [Паттерны](patterns.ru.md) · [Ollama для агентов →](ollama-for-agents.ru.md)

---

## Содержание

1. [Сравнительная таблица](#1-сравнительная-таблица)
2. [Agno — быстрый старт](#2-agno--быстрый-старт)
3. [CrewAI — команда агентов](#3-crewai--команда-агентов)
4. [LangGraph — максимальный контроль](#4-langgraph--максимальный-контроль)
5. [Другие фреймворки](#5-другие-фреймворки)
6. [Как выбирать](#6-как-выбирать)
7. [Что дальше](#7-что-дальше)

---

## 1. Сравнительная таблица

| Фреймворк | ⭐ | Уровень | Multi-agent | Ollama | Когда брать |
|-----------|-----|---------|-------------|--------|-------------|
| **Agno** | 15K+ | Начальный | Team | ✅ | Быстрый прототип, первый агент |
| **CrewAI** | 25K+ | Средний | Crew | ✅ | Команда агентов с ролями |
| **LangGraph** | 10K+ | Продвинутый | Графы | ✅ | Максимальный контроль |
| **OpenAI Agents SDK** | — | Начальный | Handoff | ✅ | Если знаете OpenAI API |
| **AutoGen** | 48K+ | Средний | GroupChat | ⚠️ | Мульти-агентные диалоги |
| **Mastra** | 10K+ | Средний | Да | ✅ | TypeScript-проекты |

> Все примеры ниже используют Ollama с моделью `qwen3.5:4b`. Убедитесь, что Ollama запущена (`ollama serve`).

---

## 2. Agno — быстрый старт

Agno — самый простой способ создать агента. Минимум кода, понятный API, встроенные инструменты.

**Установка:**
```bash
pip install agno
```

### Агент с веб-поиском

```python
from agno.agent import Agent
from agno.models.ollama import Ollama
from agno.tools.duckduckgo import DuckDuckGoTools

agent = Agent(
    model=Ollama(id="qwen3.5:4b"),
    tools=[DuckDuckGoTools()],
    description="Ты — полезный ассистент с доступом к интернету",
    markdown=True
)

agent.run("Какие тренды в AI-агентах в 2026 году?")
```

### Агент с кастомными инструментами

```python
from agno.agent import Agent
from agno.models.ollama import Ollama

def get_server_status(host: str) -> str:
    """Проверить статус сервера"""
    import subprocess
    result = subprocess.run(["ping", "-c", "1", host], capture_output=True, text=True)
    return "доступен" if result.returncode == 0 else "недоступен"

agent = Agent(
    model=Ollama(id="qwen3.5:4b"),
    tools=[get_server_status],
    show_tool_calls=True  # 👈 видно, какие инструменты вызывает
)

agent.run("Проверь статус сервера google.com")
```

### Team из двух агентов

```python
from agno.agent import Agent
from agno.models.ollama import Ollama
from agno.team import Team

researcher = Agent(
    name="Исследователь",
    model=Ollama(id="qwen3.5:4b"),
    instructions="Находи информацию и передавай её редактору",
)

writer = Agent(
    name="Редактор",
    model=Ollama(id="qwen3.5:4b"),
    instructions="На основе полученных данных пиши краткий отчёт",
)

team = Team(
    name="Контент-команда",
    members=[researcher, writer],
    model=Ollama(id="qwen3.5:4b"),
    instructions="Координируй работу команды для создания контента",
)

team.run("Напиши краткий обзор последних новостей про AI")
```

**Когда Agno:**
- ✅ Первый агент за 5 минут
- ✅ Быстрое прототипирование
- ✅ Простые сценарии с инструментами
- ❌ Сложные графы и workflow

---

## 3. CrewAI — команда агентов

CrewAI строится вокруг **ролей**: каждый агент имеет роль, цель и историю (backstory). Агенты объединяются в Crew и выполняют Tasks.

**Установка:**
```bash
pip install crewai
```

### Два агента: исследователь + писатель

```python
from crewai import Agent, Task, Crew, Process, LLM

# 1. Создаём LLM с Ollama
llm = LLM(
    model="ollama/qwen3.5:4b",
    base_url="http://localhost:11434"
)

# 2. Агенты с ролями
researcher = Agent(
    role="Исследователь рынка",
    goal="Найти актуальные данные о трендах в AI",
    backstory="Опытный аналитик с 10-летним стажем",
    llm=llm,
    allow_delegation=False
)

writer = Agent(
    role="Технический писатель",
    goal="Создать понятный отчёт на основе данных",
    backstory="Пишет документацию для сложных технологий",
    llm=llm,
    allow_delegation=False
)

# 3. Задачи
research_task = Task(
    description="Найди 5 ключевых трендов в AI-агентах за 2026 год",
    agent=researcher,
    expected_output="Список из 5 трендов с кратким описанием каждого"
)

write_task = Task(
    description="На основе трендов напиши краткий отчёт (3 абзаца)",
    agent=writer,
    expected_output="Текст отчёта в markdown"
)

# 4. Создаём команду
crew = Crew(
    agents=[researcher, writer],
    tasks=[research_task, write_task],
    process=Process.sequential,  # 👈 по очереди
    verbose=True
)

result = crew.kickoff()
print(result)
```

### Иерархическая команда (Manager + агенты)

Этот паттерн ближе всего к твоей цели — команда с менеджером.

```python
crew = Crew(
    agents=[researcher, writer],
    tasks=[research_task, write_task],
    process=Process.hierarchical,  # 👈 менеджер управляет
    manager_llm=llm,
    verbose=True
)
```

При `Process.hierarchical` менеджер сам решает, кому какую задачу дать и в каком порядке.

### Когда CrewAI

- ✅ Команда агентов с чёткими ролями
- ✅ Последовательные или иерархические процессы
- ✅ Быстрое создание multi-agent систем
- ❌ Нужен полный контроль над графом (тогда LangGraph)

---

## 4. LangGraph — максимальный контроль

LangGraph даёт **полный контроль** над графом переходов агента. Вы сами описываете узлы (что делает агент) и рёбра (когда переходить).

**Установка:**
```bash
pip install langgraph langchain-ollama
```

### ReAct-агент на графе

```python
from typing import TypedDict, Annotated
from langgraph.graph import StateGraph, START, END
from langgraph.graph.message import add_messages
from langgraph.prebuilt import ToolNode, tools_condition
from langchain_ollama import ChatOllama
from langchain_core.tools import tool

# 1. Состояние агента
class AgentState(TypedDict):
    messages: Annotated[list, add_messages]

# 2. Инструменты
@tool
def search_web(query: str) -> str:
    """Поиск информации в интернете"""
    return f"Результаты поиска по запросу: {query}"

tools = [search_web]

# 3. Модель
llm = ChatOllama(model="qwen3.5:4b", base_url="http://localhost:11434")
llm_with_tools = llm.bind_tools(tools)

# 4. Узел агента
def agent_node(state: AgentState):
    return {"messages": [llm_with_tools.invoke(state["messages"])]}

# 5. Строим граф
graph = StateGraph(AgentState)
graph.add_node("agent", agent_node)
graph.add_node("tools", ToolNode(tools))
graph.add_edge(START, "agent")
graph.add_conditional_edges("agent", tools_condition, {"tools": "tools", END: END})
graph.add_edge("tools", "agent")

# 6. Компилируем
agent = graph.compile()

# 7. Запускаем
result = agent.invoke({
    "messages": [("user", "Что нового в мире AI в 2026 году?")]
})
print(result["messages"][-1].content)
```

### Мульти-агент с Supervisor (LangGraph)

```python
from langgraph.graph import StateGraph, START, END, MessagesState
from langchain_ollama import ChatOllama

llm = ChatOllama(model="qwen3.5:4b", base_url="http://localhost:11434")

# Агент-исследователь
def researcher(state: MessagesState):
    response = llm.invoke([
        {"role": "system", "content": "Ты — исследователь. Ищи факты и данные."},
        *state["messages"]
    ])
    return {"messages": [response]}

# Агент-писатель
def writer(state: MessagesState):
    response = llm.invoke([
        {"role": "system", "content": "Ты — писатель. Создавай тексты на основе фактов."},
        *state["messages"]
    ])
    return {"messages": [response]}

# Supervisor — решает, кто следующий
def supervisor(state: MessagesState):
    response = llm.invoke([
        {"role": "system", "content": (
            "Ты — supervisor. Выбери, кто будет работать дальше: "
            "'researcher' для поиска данных, 'writer' для написания текста, "
            "'FINISH' если задача выполнена."
        )},
        *state["messages"]
    ])
    return {"messages": [response]}

# Строим граф
graph = StateGraph(MessagesState)
graph.add_node("supervisor", supervisor)
graph.add_node("researcher", researcher)
graph.add_node("writer", writer)

graph.add_edge(START, "supervisor")
graph.add_conditional_edges("supervisor", lambda s: s["messages"][-1].content)
graph.add_edge("researcher", "supervisor")
graph.add_edge("writer", "supervisor")
```

**Когда LangGraph:**
- ✅ Сложные workflow с условиями и циклами
- ✅ Нужен checkpointing (сохранение состояния между запусками)
- ✅ Human-in-the-loop
- ❌ Простые задачи (Agno или CrewAI проще)

---

## 5. Другие фреймворки

### OpenAI Agents SDK

```python
from agents import Agent, Runner, function_tool

@function_tool
def get_weather(city: str) -> str:
    return f"Погода в {city}: +22°C"

agent = Agent(
    name="Weather assistant",
    instructions="Помогай с прогнозом погоды",
    tools=[get_weather],
    model="qwen3.5:4b"  # через Ollama, если настроен proxy
)
```

Подходит, если вы уже знаете OpenAI API. С локальными моделями работает через [forge](https://github.com/antoinezambelli/forge) как proxy.

### Mastra (TypeScript)

```typescript
import { Agent } from '@mastra/core';

const agent = new Agent({
  name: 'my-agent',
  model: {
    provider: 'OLLAMA',
    name: 'qwen3.5:4b',
  },
  instructions: 'Ты — полезный ассистент',
});
```

Для TypeScript-проектов. Хорошая интеграция с Ollama.

---

## 6. Как выбирать

```
Новичок, первый агент
  → Agno (5 строк кода, всё понятно)

Нужна команда агентов с ролями
  → CrewAI (роли, задачи, процессы)

Максимальный контроль над логикой
  → LangGraph (графы, состояния, циклы)

Знаю OpenAI API, хочу локально
  → OpenAI Agents SDK + forge proxy

Пишу на TypeScript
  → Mastra

Мульти-агентные диалоги
  → AutoGen (GroupChat)
```

### Для твоей цели (команда агентов проекта)

Рекомендуемый стек:

```
CrewAI (Process.hierarchical)
  → PM-агент
    → Аналитик
    → Разработчик
    → Тестировщик
    → DevOps
```

Каждый агент — роль CrewAI. Manager распределяет задачи. Подробнее — в туториале [02-agent-team.md](tutorials/02-agent-team.ru.md).

---

## 7. Что дальше

| Если хотите | Переходите |
|-------------|-----------|
| Написать первого агента без фреймворков | [tutorials/01-first-agent.ru.md](tutorials/01-first-agent.ru.md) |
| Собрать команду агентов (твой сценарий) | [tutorials/02-agent-team.ru.md](tutorials/02-agent-team.ru.md) |
| Подключить Ollama к любому фреймворку | [ollama-for-agents.md](ollama-for-agents.ru.md) |
| Разобраться с архитектурой агента | [architecture.md](architecture.ru.md) |
| Вернуться к навигации | [README.md](README.ru.md) |
