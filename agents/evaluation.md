# <img src="https://lucide.dev/api/icons/bar-chart-3" alt="" width="20" height="20" style="vertical-align:middle"> Testing AI Agents

> How to evaluate agent quality, find bugs, and improve performance.

**🇷🇺 Russian version:** [evaluation.ru.md](evaluation.ru.md)

[← AI Agents](README.md) · [Safety](safety.md) · [Tutorials →](tutorials/)

---

## Table of Contents

1. [What to Test in an Agent](#1-what-to-test-in-an-agent)
2. [Functional Testing](#2-functional-testing)
3. [Integration Testing](#3-integration-testing)
4. [Benchmarks for Agents](#4-benchmarks-for-agents)
5. [Tracing and Observability](#5-tracing-and-observability)
6. [Quality Metrics](#6-quality-metrics)
7. [What's Next](#7-whats-next)

---

## 1. What to Test in an Agent

An agent is more complex than a regular program — it's non-deterministic. The same prompt can yield different answers. Therefore, you test not a specific output, but **behavior**.

| What to Test | How | Why |
|--------------|-----|-----|
| **Tool Selection** | Verify agent calls the right tool in the right situation | Most common error — wrong tool |
| **Argument Correctness** | Validate argument formats before calling | Invalid JSON → error |
| **Step Count** | Verify agent stays within step limit | Infinite loops = burned tokens |
| **Answer Quality** | Evaluate relevance of final answer | Agent may do everything right but answer the wrong question |
| **Error Handling** | What agent does when a tool fails | Should retry or report, not crash |

---

## 2. Functional Testing

### Tool Selection Test

```python
def test_agent_chooses_correct_tool():
    """Agent should call search_web for weather questions."""
    
    result = agent_loop("What's the weather in London?", tools=[search_web, calculate])
    
    assert "tool_calls" in result
    assert result["tool_calls"][0]["function"]["name"] == "search_web"
```

### Argument Test

```python
def test_tool_arguments():
    """Agent should pass correct arguments."""
    
    result = agent_loop("What is 15 * 3?", tools=[calculate])
    
    assert result["tool_calls"][0]["function"]["name"] == "calculate"
    args = json.loads(result["tool_calls"][0]["function"]["arguments"])
    assert "15" in args["expression"] or 15 in args.values()
```

### Step Limit Test

```python
def test_step_limit():
    """Agent should not exceed step limit."""
    
    start = time.time()
    result = agent_loop("Infinite task", tools=[search_web], max_steps=5)
    duration = time.time() - start
    
    assert result == "Step limit reached"
    assert duration < 30  # should finish quickly
```

---

## 3. Integration Testing

### Full Cycle Test

```python
class AgentTestCase:
    """Test case for agent: input → expected behavior."""
    
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
    """Runs test and returns results."""
    
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
                # execute...
                messages.append({
                    "role": "tool",
                    "name": tc["function"]["name"],
                    "content": json.dumps({"result": "ok"})
                })
        else:
            final_answer = msg["content"]
            
            # Checks
            passed = True
            errors = []
            
            for tool in test_case.expected_tools:
                if tool not in tools_used:
                    passed = False
                    errors.append(f"Expected call to {tool}, but it wasn't called")
            
            for keyword in test_case.expected_keywords:
                if keyword.lower() not in final_answer.lower():
                    passed = False
                    errors.append(f"Expected '{keyword}' in answer")
            
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
        "errors": ["Step limit exceeded"],
        "steps": test_case.max_steps,
        "tools_used": tools_used
    }


# Test suite
test_suite = [
    AgentTestCase(
        name="Weather",
        input_text="What's the weather in Moscow?",
        expected_tools=["search_web"],
        expected_keywords=["Moscow"]
    ),
    AgentTestCase(
        name="Calculator",
        input_text="What is 2 + 2?",
        expected_tools=["calculate"],
        expected_keywords=["4"]
    ),
    AgentTestCase(
        name="General Question",
        input_text="What is Python?",
        expected_tools=[],  # should not call tools
        expected_keywords=["programming language"]
    ),
]

# Run
for test in test_suite:
    result = run_agent_test(test)
    status = "✅" if result["passed"] else "❌"
    print(f"{status} {result['name']}: {result['steps']} steps")
    if result["errors"]:
        for e in result["errors"]:
            print(f"   {e}")
```

---

## 4. Benchmarks for Agents

Standard benchmarks for measuring agent quality:

| Benchmark | What It Measures | Best For |
|-----------|------------------|----------|
| **SWE-bench** | Solving real GitHub issues | Coding agents |
| **GAIA** | Multi-step tasks with tools | General-purpose agents |
| **BFCL** | Function calling quality | Tool calling |
| **AgentBench** | Interaction with OS, DB, web | Universal agents |

### Simple Benchmark for Your Agent

```python
def benchmark_agent(agent_func, test_cases: list[AgentTestCase]) -> dict:
    """Runs benchmark and returns metrics."""
    
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
        "total_time": f"{total_time:.1f}s",
        "details": results
    }


# Usage
benchmark = benchmark_agent(agent_loop, test_suite)
print(f"Pass rate: {benchmark['pass_rate']}")
print(f"Avg steps: {benchmark['avg_steps']:.1f}")
print(f"Total time: {benchmark['total_time']}")
```

---

## 5. Tracing and Observability

To understand why an agent made a wrong decision, you need to see **every step**.

### Step Logging

```python
import logging
import json
from datetime import datetime

class AgentTracer:
    """Traces every agent step."""
    
    def __init__(self, agent_name: str):
        self.agent_name = agent_name
        self.steps = []
        self.session_id = datetime.now().strftime("%Y%m%d_%H%M%S")
    
    def log_step(self, step: int, action: str, details: dict):
        """Logs an agent cycle step."""
        entry = {
            "time": datetime.now().isoformat(),
            "session": self.session_id,
            "agent": self.agent_name,
            "step": step,
            "action": action,
            "details": details
        }
        self.steps.append(entry)
        
        # Print to console for debugging
        print(f"  [{step}] {action}: {json.dumps(details, ensure_ascii=False)[:100]}")
    
    def save(self, filepath: str = None):
        """Saves trace to file."""
        path = filepath or f"trace_{self.session_id}.json"
        with open(path, "w") as f:
            json.dump(self.steps, f, ensure_ascii=False, indent=2)
        print(f"📝 Trace saved: {path}")
    
    def report(self) -> str:
        """Generates agent run report."""
        lines = [
            f"Agent: {self.agent_name}",
            f"Session: {self.session_id}",
            f"Steps: {len(self.steps)}",
            f"---"
        ]
        for s in self.steps:
            lines.append(f"[{s['step']}] {s['action']}")
        return "\n".join(lines)


# Usage in loop
tracer = AgentTracer("PM-agent")

for step in range(max_steps):
    tracer.log_step(step, "llm_call", {"model": "qwen3.5:4b"})
    # ... model call ...
    
    if msg.get("tool_calls"):
        tracer.log_step(step, "tool_call", {
            "tool": msg["tool_calls"][0]["function"]["name"],
            "args": msg["tool_calls"][0]["function"]["arguments"]
        })

tracer.save()
print(tracer.report())
```

---

## 6. Quality Metrics

Key indicators for evaluating an agent in production:

```python
class AgentMetrics:
    """Agent quality metrics."""
    
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

## 7. What's Next

| If You Want | Go To |
|-------------|-------|
| Write your first agent and test it | [tutorials/01-first-agent.md](tutorials/01-first-agent.md) |
| Build an agent team | [tutorials/02-agent-team.md](tutorials/02-agent-team.md) |
| Back to navigation | [README.md](README.md) |
---

---

**In section:** [architecture](architecture.md) · [evaluation](evaluation.md) · [frameworks](frameworks.md) · [memory](memory.md) · [multi-agent](multi-agent.md) · [ollama-for-agents](ollama-for-agents.md) · [orchestrators](orchestrators.md) · [patterns](patterns.md) · [prompting](prompting.md) · [ready-made](ready-made.md) · [safety](safety.md) · [skills](skills.md)  
**Related sections:** [Zero Level](../basics/README.md) · [Local Models](../local-models/README.md) · [Use Cases](../use-cases/README.md) · [Resources](../resources/README.md)  
**Navigation:** [← AI Agents](README.md) · [↑ Back to main](../README.md) · [🇷🇺 Русский](evaluation.ru.md)
