# 📊 Тестирование AI-агентов

> Как оценивать качество агентов, находить ошибки и улучшать производительность.

[← AI-агенты](README.ru.md) · [Безопасность](safety.ru.md) · [Туториалы →](tutorials/)

---

## Содержание

1. [Что тестировать в агенте](#1-что-тестировать-в-агенте)
2. [Функциональное тестирование](#2-функциональное-тестирование)
3. [Интеграционное тестирование](#3-интеграционное-тестирование)
4. [Бенчмарки для агентов](#4-бенчмарки-для-агентов)
5. [Tracing и observability](#5-tracing-и-observability)
6. [Метрики качества](#6-метрики-качества)
7. [Что дальше](#7-что-дальше)

---

## 1. Что тестировать в агенте

Агент сложнее обычной программы — он не детерминирован. Один и тот же запрос может дать разные ответы. Поэтому тестировать нужно не конкретный вывод, а **поведение**.

| Что тестировать | Как | Почему |
|----------------|-----|--------|
| **Выбор инструмента** | Проверить, что агент вызывает правильный инструмент в правильной ситуации | Самая частая ошибка — не тот инструмент |
| **Правильность аргументов** | Проверить форматы аргументов перед вызовом | Невалидный JSON → ошибка |
| **Количество шагов** | Проверить, что агент укладывается в лимит | Зацикливание = сожжённые токены |
| **Качество ответа** | Оценить релевантность финального ответа | Агент может выполнить всё правильно, но ответить не на тот вопрос |
| **Обработка ошибок** | Что агент делает при сбое инструмента | Должен повторить или сообщить, а не упасть |

---

## 2. Функциональное тестирование

### Тест выбора инструмента

```python
def test_agent_chooses_correct_tool():
    """Агент должен вызывать search_web для вопросов о погоде."""
    
    result = agent_loop("Какая погода в Лондоне?", tools=[search_web, calculate])
    
    assert "tool_calls" in result
    assert result["tool_calls"][0]["function"]["name"] == "search_web"
```

### Тест аргументов

```python
def test_tool_arguments():
    """Агент должен передавать корректные аргументы."""
    
    result = agent_loop("Сколько будет 15 * 3?", tools=[calculate])
    
    assert result["tool_calls"][0]["function"]["name"] == "calculate"
    args = json.loads(result["tool_calls"][0]["function"]["arguments"])
    assert "15" in args["expression"] or 15 in args.values()
```

### Тест лимита шагов

```python
def test_step_limit():
    """Агент не должен превышать лимит шагов."""
    
    start = time.time()
    result = agent_loop("Бесконечная задача", tools=[search_web], max_steps=5)
    duration = time.time() - start
    
    assert result == "Достигнут лимит шагов"
    assert duration < 30  # должен завершиться быстро
```

---

## 3. Интеграционное тестирование

### Тест полного цикла

```python
class AgentTestCase:
    """Тест-кейс для агента: вход → ожидаемое поведение."""
    
    def __init__(self, name: str, input_text: str, 
                 expected_tools: list[str] = None,
                 expected_keywords: list[str] = None,
                 max_steps: int = 10):
        self.name = name
        self.input_text = input_text
        self.expected_tools = expected_tools or []
        self.expected_keywords = expected_keywords or []
        self.max_steps = max_steps


def run_agent_test(test_case: AgentTestCase) -> dict:
    """Запускает тест и возвращает результаты."""
    
    messages = [{"role": "user", "content": test_case.input_text}]
    tools_used = []
    
    for step in range(test_case.max_steps):
        response = requests.post("http://localhost:11434/api/chat", json={
            "model": "qwen3.5:4b",
            "messages": messages,
            "tools": TEST_TOOLS,
            "stream": False
        })
        
        msg = response.json()["message"]
        messages.append(msg)
        
        if msg.get("tool_calls"):
            for tc in msg["tool_calls"]:
                tools_used.append(tc["function"]["name"])
                # выполняем...
                messages.append({
                    "role": "tool",
                    "name": tc["function"]["name"],
                    "content": json.dumps({"result": "ok"})
                })
        else:
            final_answer = msg["content"]
            
            # Проверки
            passed = True
            errors = []
            
            for tool in test_case.expected_tools:
                if tool not in tools_used:
                    passed = False
                    errors.append(f"Ожидался вызов {tool}, но он не был вызван")
            
            for keyword in test_case.expected_keywords:
                if keyword.lower() not in final_answer.lower():
                    passed = False
                    errors.append(f"Ожидалось '{keyword}' в ответе")
            
            return {
                "name": test_case.name,
                "passed": passed,
                "errors": errors,
                "steps": step + 1,
                "tools_used": tools_used,
                "answer": final_answer[:200]
            }
    
    return {
        "name": test_case.name,
        "passed": False,
        "errors": ["Превышен лимит шагов"],
        "steps": test_case.max_steps,
        "tools_used": tools_used
    }


# Набор тестов
test_suite = [
    AgentTestCase(
        name="Погода",
        input_text="Какая погода в Москве?",
        expected_tools=["search_web"],
        expected_keywords=["Москва"]
    ),
    AgentTestCase(
        name="Калькулятор",
        input_text="Сколько будет 2 + 2?",
        expected_tools=["calculate"],
        expected_keywords=["4"]
    ),
    AgentTestCase(
        name="Общий вопрос",
        input_text="Что такое Python?",
        expected_tools=[],  # не должен вызывать инструменты
        expected_keywords=["язык программирования"]
    ),
]

# Запуск
for test in test_suite:
    result = run_agent_test(test)
    status = "✅" if result["passed"] else "❌"
    print(f"{status} {result['name']}: {result['steps']} шагов")
    if result["errors"]:
        for e in result["errors"]:
            print(f"   {e}")
```

---

## 4. Бенчмарки для агентов

Стандартные бенчмарки для измерения качества агентов:

| Бенчмарк | Что измеряет | Подходит для |
|----------|-------------|-------------|
| **SWE-bench** | Решение реальных GitHub issues | Coding-агентов |
| **GAIA** | Мульти-шаговые задачи с инструментами | General-purpose агентов |
| **BFCL** | Качество вызова функций | Tool calling |
| **AgentBench** | Взаимодействие с ОС, БД, вебом | Универсальных агентов |

### Простейший бенчмарк для своего агента

```python
def benchmark_agent(agent_func, test_cases: list[AgentTestCase]) -> dict:
    """Запускает бенчмарк и возвращает метрики."""
    
    results = []
    start = time.time()
    
    for test in test_cases:
        result = run_agent_test(test)
        results.append(result)
    
    total_time = time.time() - start
    
    passed = sum(1 for r in results if r["passed"])
    total = len(results)
    
    return {
        "pass_rate": f"{passed}/{total} ({passed/total*100:.0f}%)",
        "avg_steps": sum(r["steps"] for r in results) / total,
        "total_time": f"{total_time:.1f}с",
        "details": results
    }


# Использование
benchmark = benchmark_agent(agent_loop, test_suite)
print(f"Проход: {benchmark['pass_rate']}")
print(f"Среднее шагов: {benchmark['avg_steps']:.1f}")
print(f"Общее время: {benchmark['total_time']}")
```

---

## 5. Tracing и observability

Чтобы понять, почему агент принял неверное решение, нужно видеть **каждый шаг**.

### Логирование шагов

```python
import logging
import json
from datetime import datetime

class AgentTracer:
    """Трассировка каждого шага агента."""
    
    def __init__(self, agent_name: str):
        self.agent_name = agent_name
        self.steps = []
        self.session_id = datetime.now().strftime("%Y%m%d_%H%M%S")
    
    def log_step(self, step: int, action: str, details: dict):
        """Логирует шаг цикла агента."""
        entry = {
            "time": datetime.now().isoformat(),
            "session": self.session_id,
            "agent": self.agent_name,
            "step": step,
            "action": action,
            "details": details
        }
        self.steps.append(entry)
        
        # Выводим в консоль для отладки
        print(f"  [{step}] {action}: {json.dumps(details, ensure_ascii=False)[:100]}")
    
    def save(self, filepath: str = None):
        """Сохраняет трассировку в файл."""
        path = filepath or f"trace_{self.session_id}.json"
        with open(path, "w") as f:
            json.dump(self.steps, f, ensure_ascii=False, indent=2)
        print(f"📝 Трассировка сохранена: {path}")
    
    def report(self) -> str:
        """Генерирует отчёт о работе агента."""
        lines = [
            f"Агент: {self.agent_name}",
            f"Сессия: {self.session_id}",
            f"Шагов: {len(self.steps)}",
            f"---"
        ]
        for s in self.steps:
            lines.append(f"[{s['step']}] {s['action']}")
        return "\n".join(lines)


# Использование в цикле
tracer = AgentTracer("PM-агент")

for step in range(max_steps):
    tracer.log_step(step, "llm_call", {"model": "qwen3.5:4b"})
    # ... вызов модели ...
    
    if msg.get("tool_calls"):
        tracer.log_step(step, "tool_call", {
            "tool": msg["tool_calls"][0]["function"]["name"],
            "args": msg["tool_calls"][0]["function"]["arguments"]
        })

tracer.save()
print(tracer.report())
```

---

## 6. Метрики качества

Ключевые показатели для оценки агента в production:

```python
class AgentMetrics:
    """Метрики качества агента."""
    
    def __init__(self):
        self.total_tasks = 0
        self.successful_tasks = 0
        self.total_steps = 0
        self.total_tokens = 0
        self.total_tool_calls = 0
        self.errors = []
    
    def record(self, task: str, success: bool, steps: int, 
               tokens: int, tool_calls: int, error: str = None):
        self.total_tasks += 1
        if success:
            self.successful_tasks += 1
        self.total_steps += steps
        self.total_tokens += tokens
        self.total_tool_calls += tool_calls
        if error:
            self.errors.append(error)
    
    def summary(self) -> dict:
        return {
            "success_rate": f"{self.successful_tasks}/{self.total_tasks} ({self.successful_tasks/self.total_tasks*100:.0f}%)",
            "avg_steps": self.total_steps / max(self.total_tasks, 1),
            "avg_tokens": self.total_tokens / max(self.total_tasks, 1),
            "avg_tool_calls": self.total_tool_calls / max(self.total_tasks, 1),
            "errors": self.errors[:5]
        }
```

---

## 7. Что дальше

| Если хотите | Переходите |
|-------------|-----------|
| Написать первого агента и протестировать | [tutorials/01-first-agent.ru.md](tutorials/01-first-agent.ru.md) |
| Собрать команду агентов | [tutorials/02-agent-team.ru.md](tutorials/02-agent-team.ru.md) |
| Вернуться к навигации | [README.md](README.ru.md) |
