# <img src="https://lucide.dev/api/icons/book-open" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> Туториалы по AI-агентам

> Пошаговые гайды: от первого агента до production-команды.

[← AI-агенты](../README.ru.md) · [🇬🇧 English](README.md)

---

## Туториалы

| # | Туториал | Описание | Сложность | Время |
|---|----------|----------|-----------|-------|
| 1 | [Первый агент за 30 минут](01-first-agent.ru.md) | ReAct агент с поиском, без фреймворков | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#22c55e" stroke="#16a34a" stroke-width="0.5"/></svg> Начальный | 30 мин |
| 2 | [Команда агентов для проекта](02-agent-team.ru.md) | Multi-agent система: PM, аналитик, разработчик, тестировщик | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#eab308" stroke="#ca8a04" stroke-width="0.5"/></svg> Средний | 60 мин |
| 3 | [Агент-программист](03-coding-agent.ru.md) | Coding-агент: читает код, пишет тесты, делает PR | <svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#ef4444" stroke="#dc2626" stroke-width="0.5"/></svg> Продвинутый | 45 мин |

---

## Перед началом

Убедитесь, что:
1. Установлена Ollama — [local-models/getting-started.ru.md](../../local-models/getting-started.ru.md)
2. Запущена модель `qwen3.5:4b` — `ollama pull qwen3.5:4b`
3. Установлен Python 3.10+ с `pip install requests`

## Теория

Если хотите понять, как работают агенты, перед туториалами прочитайте:

- [Архитектура агента](../architecture.ru.md) — из чего состоит агент
- [Паттерны](../patterns.ru.md) — ReAct, Plan-and-Execute, Reflection
- [Промптинг](../prompting.ru.md) — как писать system prompt для агентов

---

## Связанные разделы

- [<svg width="0.8em" height="0.8em" viewBox="0 0 12 12" style="display:inline;vertical-align:middle"><circle cx="6" cy="6" r="5" fill="#3b82f6" stroke="#2563eb" stroke-width="0.5"/></svg> Локальные модели](../../local-models/README.ru.md) — установка Ollama, модели
- [<img src="https://lucide.dev/api/icons/briefcase" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> Use Cases](../../use-cases/README.ru.md) — применение агентов на практике

---

**Навигация:** [← AI-агенты](../README.ru.md) · [↑ На главную](../../README.ru.md) · [<img src="https://lucide.dev/api/icons/book-open" alt="" width="1em" height="1em" style="display:inline;vertical-align:middle"> Каталог](../../catalog.ru.md)
