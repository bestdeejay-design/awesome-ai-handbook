# 👥 Мульти-агентные системы

> Как несколько агентов работают вместе: архитектуры, коммуникация, координация.  
> **Этот раздел — ключевой для построения команды агентов твоего проекта.**

[← AI-агенты](README.ru.md) · [Архитектура](architecture.ru.md) · [Туториал: команда →](tutorials/02-agent-team.ru.md)

---

## Содержание

1. [Зачем нужны несколько агентов](#1-зачем-нужны-несколько-агентов)
2. [Архитектуры мульти-агентных систем](#2-архитектуры-мульти-агентных-систем)
3. [Supervisor Pattern](#3-supervisor-pattern)
4. [Handoff Pattern](#4-handoff-pattern)
5. [Swarm Pattern](#5-swarm-pattern)
6. [GroupChat Pattern](#6-groupchat-pattern)
7. [Иерархическая команда (для твоего проекта)](#7-иерархическая-команда-для-твоего-проекта)
8. [Проблемы мульти-агентных систем](#8-проблемы-мульти-агентных-систем)
9. [Что дальше](#9-что-дальше)

---

## 1. Зачем нужны несколько агентов

Один агент — это цикл «думай → действуй → наблюдай». Он хорош для простых задач, но упирается в ограничения:

| Проблема одного агента | Решение мульти-агент |
|---|---|
| **Контекстное окно** — быстро заполняется | Каждый агент имеет свой узкий контекст |
| **Одна экспертиза** — модель не может быть экспертом во всём | Каждый агент специализируется на своём |
| **Последовательность** — всё делает по очереди | Агенты работают параллельно |
| **Отсутствие проверки** — модель не замечает свои ошибки | Один агент пишет, другой проверяет |
| **Одна точка отказа** — если зациклился, всё встало | Другие агенты продолжают работу |

**Твой проект** — идеальный сценарий для мульти-агентной системы:
- PM-агент ставит задачи и следит за сроками
- Аналитик собирает требования
- Программист пишет код
- Тестировщик проверяет
- DevOps деплоит
- Дизайнер создаёт интерфейсы

---

## 2. Архитектуры мульти-агентных систем

```
Supervisor (иерархическая):
  ┌──────────────┐
  │  Supervisor  │  ← центральный координатор
  └──┬──┬──┬──┬──┘
     │  │  │  │
  ┌──┘  │  │  └──┐
  ▼     ▼  ▼     ▼
┌────┐┌────┐┌────┐┌────┐
│ A1  ││ A2  ││ A3  ││ A4  │  ← специалисты
└────┘└────┘└────┘└────┘

Handoff (передача управления):
  ┌──────┐   ┌──────┐   ┌──────┐
  │ Triage│──▶│ Sales│──▶│Support│
  └──────┘   └──────┘   └──────┘
  Агент сам решает, кому передать задачу

Swarm (роевая):
  Агенты равны, каждый может взять задачу
  ┌──────┐
  │  A1  │◀───┐
  └──────┘    │
  ┌──────┐    │
  │  A2  │◀───┼─── Общая очередь задач
  └──────┘    │
  ┌──────┐    │
  │  A3  │◀───┘
  └──────┘

GroupChat (дискуссионная):
  Все агенты в одном чате, обсуждают и приходят к решению
  ┌──────┐    ┌──────┐
  │  A1  │◀──▶│  A2  │
  └──────┘    └──────┘
      ▲            ▲
      │    ┌──────┐ │
      └────│  A3  │─┘
           └──────┘
```

---

## 3. Supervisor Pattern

**Один главный агент управляет специалистами.** Supervisor решает, кому какую задачу дать, и собирает результаты.

### Как работает

```
Пользователь: «Сделай исследование рынка AI-агентов»

Supervisor:
  └─▶ Аналитик: собери данные о рынке
  └─▶ Писатель: оформи отчёт на основе данных
  └─▶ Дизайнер: создай диаграммы

Supervisor собирает результаты → финальный ответ
```

### Реализация на Python

```python
import requests
import json

OLLAMA = "http://localhost:11434/api/chat"
MODEL = "qwen3.5:4b"

class Agent:
    """Агент-специалист с ролью."""
    
    def __init__(self, name: str, role: str, instruction: str):
        self.name = name
        self.role = role
        self.instruction = instruction
    
    def run(self, task: str, context: str = "") -> str:
        response = requests.post(OLLAMA, json={
            "model": MODEL,
            "messages": [
                {"role": "system", "content": self.instruction},
                {"role": "user", "content": f"{context}\nЗадача: {task}"}
            ],
            "stream": False
        })
        return response.json()["message"]["content"]


class Supervisor:
    """Supervisor — управляет командой агентов."""
    
    def __init__(self, agents: list[Agent]):
        self.agents = agents
    
    def run(self, task: str) -> str:
        results = {}
        
        for agent in self.agents:
            print(f"  ▶ {agent.name} работает...")
            context = "\n".join([f"{name}: {res}" for name, res in results.items()])
            result = agent.run(task, context)
            results[agent.name] = result
            print(f"    ✓ {agent.name} завершил")
        
        # Supervisor собирает финальный ответ
        summary_prompt = (
            f"Задача: {task}\n\n"
            f"Результаты команды:\n" +
            "\n".join([f"{name}: {res}" for name, res in results.items()]) +
            "\n\nСоставь сводный отчёт на основе работы всех участников."
        )
        
        response = requests.post(OLLAMA, json={
            "model": MODEL,
            "messages": [
                {"role": "system", "content": "Ты — руководитель команды. Собери результаты в единый отчёт."},
                {"role": "user", "content": summary_prompt}
            ],
            "stream": False
        })
        return response.json()["message"]["content"]


# Пример: команда для анализа
analyst = Agent(
    "Аналитик",
    "Исследователь рынка",
    "Ты — аналитик. Ищи данные, строй гипотезы, делай выводы."
)
writer = Agent(
    "Писатель",
    "Технический автор",
    "Ты — писатель. Создавай понятные структурированные тексты."
)
designer = Agent(
    "Дизайнер",
    "Визуализатор данных",
    "Ты — дизайнер. Предлагай, как визуализировать данные."
)

supervisor = Supervisor([analyst, writer, designer])
result = supervisor.run("Проанализируй текущее состояние рынка AI-агентов")
print(f"\n📋 Итог:\n{result}")
```

### Плюсы и минусы

| Плюсы | Минусы |
|-------|--------|
| Чёткое распределение ролей | Supervisor — единая точка отказа |
| Простая логика координации | Supervisor может неверно распределить задачи |
| Легко добавлять новых агентов | Последовательное выполнение (можно сделать параллельным) |

---

## 4. Handoff Pattern

**Агент сам решает, что не может выполнить задачу, и передаёт её другому агенту.**  
Этот паттерн использует OpenAI Agents SDK и подходит для сценариев, где заранее неизвестно, какой агент понадобится.

### Как работает

```
Пользователь: «Хочу заказать пиццу»

Triage-агент:
  → Это про заказ еды? → Передаю агенту заказов
  → Нет, это про возврат → Передаю агенту поддержки
  → Нет, это общий вопрос → Отвечаю сам
```

### Реализация

```python
class HandoffAgent:
    """Агент, который может передать задачу другому."""
    
    def __init__(self, name: str, description: str, system_prompt: str, 
                 can_handoff_to: list = None):
        self.name = name
        self.description = description
        self.system_prompt = system_prompt
        self.can_handoff_to = can_handoff_to or []
    
    def run(self, task: str) -> tuple[str, str]:
        """Возвращает (ответ, следующий_агент_или_None)."""
        
        # Описываем доступные handoff-ы
        handoff_desc = ""
        if self.can_handoff_to:
            agents_desc = "\n".join([
                f"- {a.name}: {a.description}" 
                for a in self.can_handoff_to
            ])
            handoff_desc = (
                f"\n\nЕсли задача НЕ относится к твоей компетенции, "
                f"напиши 'HANDOFF: имя_агента' в начале ответа.\n"
                f"Доступные агенты:\n{agents_desc}"
            )
        
        response = requests.post(OLLAMA, json={
            "model": "qwen3.5:4b",
            "messages": [
                {"role": "system", "content": self.system_prompt + handoff_desc},
                {"role": "user", "content": task}
            ],
            "stream": False
        })
        
        content = response.json()["message"]["content"]
        
        if content.startswith("HANDOFF:"):
            agent_name = content.replace("HANDOFF:", "").strip()
            return None, agent_name
        
        return content, None


class HandoffOrchestrator:
    """Оркестратор для handoff-агентов."""
    
    def __init__(self, triage_agent: HandoffAgent):
        self.triage_agent = triage_agent
        # Строим карту всех агентов
        self.agents_map = {}
        self._build_map(triage_agent)
    
    def _build_map(self, agent: HandoffAgent):
        self.agents_map[agent.name] = agent
        for child in agent.can_handoff_to or []:
            self._build_map(child)
    
    def run(self, task: str) -> str:
        current = self.triage_agent
        history = []
        
        for hop in range(5):  # максимум 5 передач
            result, next_name = current.run(task)
            history.append(f"{current.name}: получил задачу")
            
            if next_name:
                if next_name in self.agents_map:
                    print(f"  ▶ {current.name} → {next_name}")
                    current = self.agents_map[next_name]
                else:
                    return f"Ошибка: агент {next_name} не найден"
            else:
                return result
        
        return "Слишком много передач между агентами"
```

---

## 5. Swarm Pattern

**Агенты работают как «рой» — каждый может взять задачу из общей очереди.**  
Подходит для сценариев, где задачи независимы и могут выполняться параллельно.

```python
import threading
from queue import Queue

class SwarmWorker(threading.Thread):
    """Агент в рое — работает в отдельном потоке."""
    
    def __init__(self, name: str, role: str, task_queue: Queue, result_queue: Queue):
        super().__init__()
        self.name = name
        self.role = role
        self.task_queue = task_queue
        self.result_queue = result_queue
    
    def run(self):
        while True:
            task = self.task_queue.get()
            if task is None:  # сигнал остановки
                break
            
            print(f"  ▶ {self.name} взял задачу: {task[:50]}...")
            
            response = requests.post(OLLAMA, json={
                "model": "qwen3.5:4b",
                "messages": [
                    {"role": "system", "content": f"Ты — {self.name}. {self.role}"},
                    {"role": "user", "content": task}
                ],
                "stream": False
            })
            
            result = response.json()["message"]["content"]
            self.result_queue.put((self.name, task, result))
            self.task_queue.task_done()


class Swarm:
    """Оркестратор роя агентов."""
    
    def __init__(self, workers: list[SwarmWorker]):
        self.workers = workers
    
    def run(self, tasks: list[str]) -> dict:
        task_queue = Queue()
        result_queue = Queue()
        
        # Запускаем workers
        for w in self.workers:
            w.task_queue = task_queue
            w.result_queue = result_queue
            w.start()
        
        # Отправляем задачи
        for task in tasks:
            task_queue.put(task)
        
        # Ждём завершения
        task_queue.join()
        
        # Останавливаем workers
        for _ in self.workers:
            task_queue.put(None)
        
        # Собираем результаты
        results = {}
        while not result_queue.empty():
            name, task, result = result_queue.get()
            results[name] = result
        
        return results
```

---

## 6. GroupChat Pattern

**Все агенты в одном «чате» обмениваются сообщениями, пока не придут к решению.**  
Используется для дискуссий, мозговых штурмов, коллективного принятия решений.

```python
class GroupChat:
    """Групповой чат агентов."""
    
    def __init__(self, agents: list[Agent], max_rounds: int = 3):
        self.agents = agents
        self.max_rounds = max_rounds
        self.messages = []
    
    def run(self, topic: str) -> str:
        self.messages = [
            {"role": "system", "content": (
                f"Групповое обсуждение: {topic}\n"
                f"Участники: {', '.join(a.name for a in self.agents)}.\n"
                f"Каждый участник высказывается по очереди."
            )}
        ]
        
        for round_num in range(self.max_rounds):
            print(f"\n  📢 Раунд {round_num + 1}:")
            
            for agent in self.agents:
                response = requests.post(OLLAMA, json={
                    "model": "qwen3.5:4b",
                    "messages": [
                        {"role": "system", "content": (
                            f"Ты — {agent.name}. {agent.role}. "
                            f"Участвуй в обсуждении. Реагируй на других участников. "
                            f"Если согласен с кем-то — поддержи. Если нет — аргументируй."
                        )},
                        *self.messages[-6:],  # последние 6 сообщений
                        {"role": "user", "content": f"Твой ход, {agent.name}. Что думаешь?"}
                    ],
                    "stream": False
                })
                
                reply = response.json()["message"]["content"]
                self.messages.append({
                    "role": "assistant",
                    "content": f"{agent.name}: {reply}"
                })
                print(f"    {agent.name}: {reply[:80]}...")
        
        # Суммаризация
        summary = requests.post(OLLAMA, json={
            "model": "qwen3.5:4b",
            "messages": [
                {"role": "system", "content": "Подведи итог дискуссии: какие решения приняты, какие аргументы прозвучали."},
                {"role": "user", "content": "\n".join(m["content"] for m in self.messages[-10:])}
            ],
            "stream": False
        })
        return summary.json()["message"]["content"]
```

---

## 7. Иерархическая команда (для твоего проекта)

Этот паттерн объединяет **Supervisor + специалистов** в иерархию, где каждый уровень управляет своими подчинёнными.

```
                     ┌──────────────┐
                     │  Оркестратор  │  ← главный координатор
                     │   (SuperPM)   │
                     └──┬──┬──┬──┬──┘
                        │  │  │  │
         ┌──────────────┘  │  │  └──────────────┐
         ▼                 ▼  ▼                 ▼
   ┌──────────┐    ┌──────────┐    ┌──────────┐
   │  Аналитик │    │  Архитектор│   │   DevOps  │
   └──────────┘    └────┬─────┘    └──────────┘
                        │
          ┌─────────────┼─────────────┐
          ▼             ▼             ▼
    ┌──────────┐  ┌──────────┐  ┌──────────┐
    │  Бэкенд   │  │ Фронтенд │  │  Тестировщик│
    └──────────┘  └──────────┘  └──────────┘
```

### CrewAI реализация для твоей команды

```python
from crewai import Agent, Task, Crew, Process, LLM

llm = LLM(model="ollama/qwen3.5:4b", base_url="http://localhost:11434")

# === Аналитик ===
analyst = Agent(
    role="Системный аналитик",
    goal="Собрать и структурировать требования к проекту",
    backstory="Ты — опытный аналитик. Превращаешь расплывчатые идеи в чёткие задачи.",
    llm=llm
)

# === Архитектор (управляет разработчиками) ===
backend_dev = Agent(
    role="Backend-разработчик",
    goal="Реализовать серверную логику API",
    backstory="Пишешь надёжный Python-код с тестами.",
    llm=llm
)

frontend_dev = Agent(
    role="Frontend-разработчик",
    goal="Создать пользовательский интерфейс",
    backstory="Специалист по React и TypeScript.",
    llm=llm
)

tester = Agent(
    role="QA-инженер",
    goal="Найти баги до того, как их увидят пользователи",
    backstory="Ты — дотошный тестировщик. Проверяешь всё.",
    llm=llm
)

architect = Agent(
    role="Архитектор",
    goal="Координировать разработку: распределять задачи между разработчиками и проверять качество",
    backstory="Ты — технический лидер. Понимаешь и бэкенд, и фронтенд.",
    llm=llm
)

# === DevOps ===
devops = Agent(
    role="DevOps-инженер",
    goal="Настроить инфраструктуру, CI/CD и мониторинг",
    backstory="Автоматизируешь всё, что можно автоматизировать.",
    llm=llm
)

# === Supervisor (PM) ===
pm = Agent(
    role="Project Manager",
    goal=(
        "Координировать всю команду. "
        "После каждого этапа проверять результат и решать, что делать дальше."
    ),
    backstory="Ты — опытный PM. Веди команду к релизу.",
    llm=llm
)

# === Задачи ===
task_analyze = Task(
    description="Проанализируй требования: пользователь хочет сделать TODO-приложение с веб-интерфейсом",
    agent=analyst,
    expected_output="Документ с требованиями: функциональные и нефункциональные"
)

task_architect = Task(
    description="Спроектируй архитектуру: какие компоненты нужны, какие API",
    agent=architect,
    expected_output="Описание архитектуры"
)

task_backend = Task(
    description="Реализуй REST API для TODO: создание, чтение, обновление, удаление задач",
    agent=backend_dev,
    expected_output="Код на Python (FastAPI)"
)

task_frontend = Task(
    description="Создай веб-интерфейс: список задач, форма добавления, кнопка удаления",
    agent=frontend_dev,
    expected_output="Код на React"
)

task_test = Task(
    description="Напиши тесты для API: проверь все CRUD-операции",
    agent=tester,
    expected_output="Тесты на pytest"
)

task_deploy = Task(
    description="Настрой Docker Compose для запуска всего приложения",
    agent=devops,
    expected_output="docker-compose.yml + инструкция"
)

# === Собираем команду ===
crew = Crew(
    agents=[pm, analyst, architect, backend_dev, frontend_dev, tester, devops],
    tasks=[task_analyze, task_architect, task_backend, task_frontend, task_test, task_deploy],
    process=Process.hierarchical,  # PM управляет иерархически
    manager_llm=llm,
    verbose=True
)

result = crew.kickoff()
```

---

## 8. Проблемы мульти-агентных систем

### 8.1 Коммуникационные накладные расходы

Каждое сообщение между агентами — это токены. В групповом чате 5 агентов обмениваются 10+ сообщениями, и контекст быстро растёт.

**Решение:** Ограничивать раунды, использовать короткие сообщения, суммаризировать.

### 8.2 Противоречивые решения

Агенты могут не соглашаться друг с другом, и непонятно, чьё решение правильное.

**Решение:** Supervisor с правом вето, голосование, human-in-the-loop.

### 8.3 Каскадные ошибки

Ошибка одного агента (неправильные данные, неверное решение) распространяется на всех.

**Решение:** Валидация на каждом шаге, retry, fallback-агенты.

### 8.4 Потеря контекста

Каждый агент видит только свою часть. Общая картина может теряться.

**Решение:** Supervisor хранит общее состояние, регулярные синхронизации.

### 8.5 Когда НЕ нужно делать мульти-агент

- ✅ Задача решается одним агентом за 1-2 шага
- ✅ Нет чёткого разделения на разные экспертизы
- ❌ Кажется, что «больше агентов = круче» (нет, больше агентов = больше проблем)

---

## 9. Что дальше

| Если хотите | Переходите |
|-------------|-----------|
| **Собрать свою команду агентов** (твой проект) | [tutorials/02-agent-team.ru.md](tutorials/02-agent-team.ru.md) |
| Понимать, как агенты хранят память | [memory.ru.md](memory.ru.md) |
| Научиться писать промпты для агентов | [prompting.ru.md](prompting.ru.md) |
| Обезопасить агентов | [safety.ru.md](safety.ru.md) |
| Вернуться к навигации | [README.md](README.ru.md) |
