# 🟡 Туториал 2: Команда агентов для проекта

> Строим мульти-агентную систему: PM, аналитик, разработчик и тестировщик работают вместе.  
> **CrewAI + Ollama** — идеально для твоей цели.

[← Туториалы](README.ru.md) · [Первый агент](01-first-agent.ru.md) · [Агент-программист →](03-coding-agent.ru.md)

---

## Что вы сделаете

Команду из 4 агентов, которые работают над проектом:
1. **PM-агент** — ставит задачи, назначает исполнителей, проверяет результаты
2. **Аналитик** — собирает требования и пишет спецификации
3. **Разработчик** — пишет код по спецификации
4. **Тестировщик** — проверяет код и ищет баги

Агенты работают последовательно: каждый получает результат предыдущего и делает свою часть.

---

## Шаг 1: Установка

```bash
pip install crewai
```

---

## Шаг 2: Код команды

Создайте файл `agent_team.py`:

```python
import json
import requests
from typing import List, Dict

OLLAMA = "http://localhost:11434/api/chat"
MODEL = "qwen3.5:4b"


class SimpleAgent:
    """Базовый агент с ролью и задачей."""
    
    def __init__(self, name: str, role: str, goal: str, system_prompt: str):
        self.name = name
        self.role = role
        self.goal = goal
        self.system_prompt = system_prompt
    
    def run(self, task: str, context: str = "") -> str:
        """Выполняет задачу с учётом контекста."""
        
        full_prompt = (
            f"Твоя роль: {self.role}\n"
            f"Твоя цель: {self.goal}\n\n"
            f"Контекст проекта:\n{context}\n\n"
            f"Задача: {task}"
        )
        
        response = requests.post(OLLAMA, json={
            "model": MODEL,
            "messages": [
                {"role": "system", "content": self.system_prompt},
                {"role": "user", "content": full_prompt}
            ],
            "stream": False
        })
        
        return response.json()["message"]["content"]


class ProjectTeam:
    """Оркестратор команды агентов."""
    
    def __init__(self):
        self.pm = SimpleAgent(
            name="PM",
            role="Project Manager",
            goal="Координировать команду, ставить задачи, проверять результаты",
            system_prompt=(
                "Ты — опытный Project Manager. Твоя задача — разбить проект "
                "на чёткие задачи, назначить их команде и проверить результат. "
                "После каждого этапа пиши краткое резюме: что сделано, что дальше."
            )
        )
        
        self.analyst = SimpleAgent(
            name="Аналитик",
            role="System Analyst",
            goal="Собрать требования и написать спецификацию",
            system_prompt=(
                "Ты — системный аналитик. Превращаешь расплывчатые идеи "
                "в чёткие технические требования. Пиши спецификацию в формате:\n"
                "## Функциональные требования\n## Нефункциональные требования\n## User Stories"
            )
        )
        
        self.developer = SimpleAgent(
            name="Разработчик",
            role="Software Developer",
            goal="Реализовать код по спецификации",
            system_prompt=(
                "Ты — Python-разработчик. Пиши код с type hints и docstrings. "
                "Используй современные практики. "
                "Выводи ТОЛЬКО код, без лишних комментариев."
            )
        )
        
        self.tester = SimpleAgent(
            name="Тестировщик",
            role="QA Engineer",
            goal="Проверить код, найти баги, написать тесты",
            system_prompt=(
                "Ты — QA-инженер. Проверяй код на:\n"
                "- Ошибки и баги\n"
                "- Граничные случаи\n"
                "- Безопасность\n"
                "- Соответствие спецификации\n\n"
                "Если нашёл баг — опиши его. Если всё хорошо — подтверди."
            )
        )
    
    def run(self, project_description: str) -> str:
        """Запускает полный цикл работы команды."""
        
        print("\n" + "=" * 50)
        print(f"🚀 ПРОЕКТ: {project_description[:50]}...")
        print("=" * 50)
        
        # Этап 1: PM ставит задачу
        print("\n📋 Этап 1: PM → Аналитик")
        task = self.pm.run(
            f"Сформулируй задачу для аналитика по проекту: {project_description}"
        )
        print(f"  PM: {task[:200]}...")
        
        # Этап 2: Аналитик пишет спецификацию
        print("\n📋 Этап 2: Аналитик пишет спецификацию")
        spec = self.analyst.run(
            f"Напиши спецификацию для проекта: {project_description}",
            context=f"Задача от PM: {task}"
        )
        print(f"  Аналитик: {spec[:300]}...")
        
        # Этап 3: Разработчик пишет код
        print("\n📋 Этап 3: Разработчик пишет код")
        code = self.developer.run(
            f"Реализуй код по спецификации",
            context=f"Спецификация:\n{spec}"
        )
        print(f"  Разработчик: {code[:300]}...")
        
        # Этап 4: Тестировщик проверяет
        print("\n📋 Этап 4: Тестировщик проверяет код")
        review = self.tester.run(
            f"Проверь код и спецификацию",
            context=f"Спецификация:\n{spec}\n\nКод:\n{code}"
        )
        print(f"  Тестировщик: {review[:300]}...")
        
        # Этап 5: PM подводит итог
        print("\n📋 Этап 5: PM подводит итог")
        summary = self.pm.run(
            "Подведи итог работы команды по проекту",
            context=(
                f"Спецификация:\n{spec}\n\n"
                f"Код:\n{code}\n\n"
                f"Рецензия:\n{review}"
            )
        )
        
        return summary


# === Точка входа ===
if __name__ == "__main__":
    team = ProjectTeam()
    
    project = (
        "TODO-приложение: Flask API для управления задачами. "
        "Нужны CRUD-операции, SQLite, REST-эндпоинты."
    )
    
    result = team.run(project)
    
    print("\n" + "=" * 50)
    print("📌 ИТОГ ПРОЕКТА:")
    print(result)
    print("=" * 50)
```

---

## Шаг 3: Запуск

```bash
python agent_team.py
```

---

## Шаг 4: Что вы увидите

```
==================================================
🚀 ПРОЕКТ: TODO-приложение: Flask API для управления...
==================================================

📋 Этап 1: PM → Аналитик
  PM: Аналитик, необходимо разработать TODO-приложение...

📋 Этап 2: Аналитик пишет спецификацию
  Аналитик: ## Функциональные требования
  1. Создание задачи...

📋 Этап 3: Разработчик пишет код
  Разработчик: from flask import Flask...

📋 Этап 4: Тестировщик проверяет код
  Тестировщик: Код соответствует спецификации...

📋 Этап 5: PM подводит итог
  PM: Проект завершён. Создано TODO-приложение...
```

---

## CrewAI-версия (для реальных проектов)

```python
from crewai import Agent, Task, Crew, Process, LLM

llm = LLM(model="ollama/qwen3.5:4b", base_url="http://localhost:11434")

# Агенты
pm = Agent(
    role="Project Manager",
    goal="Координировать команду",
    backstory="Опытный PM с 10-летним стажем",
    llm=llm
)

analyst = Agent(
    role="System Analyst",
    goal="Написать спецификацию",
    backstory="Технический писатель с опытом в разработке",
    llm=llm
)

developer = Agent(
    role="Python Developer",
    goal="Реализовать код",
    backstory="Senior Python-разработчик",
    llm=llm
)

tester = Agent(
    role="QA Engineer",
    goal="Проверить код",
    backstory="Дотошный тестировщик",
    llm=llm
)

# Задачи
task_spec = Task(description="Создать спецификацию TODO-приложения", agent=analyst)
task_code = Task(description="Реализовать API по спецификации", agent=developer)
task_test = Task(description="Проверить код и написать тесты", agent=tester)

# Иерархическая команда
crew = Crew(
    agents=[pm, analyst, developer, tester],
    tasks=[task_spec, task_code, task_test],
    process=Process.hierarchical,
    manager_llm=llm,
    verbose=True
)

result = crew.kickoff()
```

---

## Как расширить команду

| Роль | Инструменты | Что даёт |
|------|------------|----------|
| **DevOps** | Docker, CI/CD | Деплой и инфраструктура |
| **Дизайнер** | Figma API | UI/UX-макеты |
| **Frontend** | React, HTML/CSS | Веб-интерфейс |
| **Security** | SAST, DAST | Аудит безопасности |

---

## Что дальше

| Если хотите | Переходите |
|-------------|-----------|
| Создать coding-агента | [03-coding-agent.ru.md](03-coding-agent.ru.md) |
| Понять архитектуру мульти-агентных систем | [../multi-agent.ru.md](../multi-agent.ru.md) |
| Настроить память для команды | [../memory.ru.md](../memory.ru.md) |
| Вернуться к туториалам | [README.ru.md](README.ru.md) |
