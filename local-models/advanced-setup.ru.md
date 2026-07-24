# ⚙️ Продвинутая настройка Ollama

> Modelfile, переменные окружения, API, несколько моделей — всё, что нужно,
> чтобы выжать максимум из локальных моделей.

[← Локальные модели](README.ru.md) · [Начать с начала](getting-started.ru.md)

---

## Содержание

1. [Переменные окружения Ollama](#1-переменные-окружения-ollama)
2. [Modelfile — кастомные модели](#2-modelfile--кастомные-модели)
3. [HTTP API — доступ из программ](#3-http-api--доступ-из-программ)
4. [OpenAI-совместимые эндпоинты](#4-openai-совместимые-эндпоинты)
5. [Параллельные запросы и несколько моделей](#5-параллельные-запросы-и-несколько-моделей)
6. [Speculative decoding (ускорение)](#6-speculative-decoding-ускорение)
7. [Что дальше](#7-что-дальше)

---

## 1. Переменные окружения Ollama

Переменные окружения — самый простой способ повлиять на то, как Ollama использует
память, процессор и сеть. Они задаются **до** запуска сервера.

### Как задать переменную

**Способ 1 — временно (на один запуск):**

```bash
OLLAMA_KV_CACHE_TYPE=q4_0 ollama serve
```

**Способ 2 — на всю сессию терминала:**

```bash
export OLLAMA_KV_CACHE_TYPE=q4_0
ollama serve
```

**Способ 3 — навсегда (рекомендую):**

Добавьте строки в файл `~/.zshrc` (этот файл выполняется при каждом открытии терминала):

```bash
echo 'export OLLAMA_KV_CACHE_TYPE=q4_0' >> ~/.zshrc
echo 'export OLLAMA_FLASH_ATTENTION=1' >> ~/.zshrc
```

Затем примените изменения (или откройте новый терминал):

```bash
source ~/.zshrc
```

### Полная таблица переменных

| Переменная | По умолчанию | Что делает | Когда менять |
|---|---|---|---|
| `OLLAMA_KV_CACHE_TYPE` | `f16` | Тип квантизации KV cache. `q4_0` = в **4× меньше памяти**, `q8_0` = в 2× меньше | Всегда на Mac с ≤16 GB RAM. `q4_0` — безопасный выбор |
| `OLLAMA_FLASH_ATTENTION` | `false` | Flash attention — экономит память + немного ускоряет | Включите (`1`), если модели не влезают |
| `OLLAMA_NUM_PARALLEL` | `1` | Сколько запросов к одной модели можно обрабатывать одновременно | Если несколько приложений стучатся к одной модели (например, Continue.dev + чат) |
| `OLLAMA_MAX_LOADED_MODELS` | `auto` | Сколько разных моделей держать в памяти одновременно | Если часто переключаетесь между моделями |
| `OLLAMA_KEEP_ALIVE` | `5m` | Сколько минут держать модель в памяти после последнего запроса | Увеличьте, если модель часто выгружается и перезагружается |
| `OLLAMA_HOST` | `127.0.0.1:11434` | Какой IP и порт слушает сервер | `0.0.0.0:11434` — если нужно стучаться с других устройств |
| `OLLAMA_GPU_OVERHEAD` | `0` | Резерв памяти (в байтах), который Ollama не трогает | Если Mac тормозит из-за нехватки памяти для системы |
| `OLLAMA_LOAD_TIMEOUT` | `5m` | Таймаут загрузки модели | Если модель большая и долго грузится |
| `OLLAMA_NOHISTORY` | `false` | Не сохранять историю запросов | Для приватности |
| `OLLAMA_DEBUG` | `false` | Подробные логи | Если что-то не работает и нужно разобраться |
| `OLLAMA_MAX_QUEUE` | `512` | Максимальная очередь запросов | При высокой нагрузке |

### Рекомендуемый конфиг для Mac 16 GB

Добавьте всё это в `~/.zshrc` одной командой:

```bash
cat >> ~/.zshrc << 'EOF'

# Ollama — оптимизация для Mac 16 GB
export OLLAMA_KV_CACHE_TYPE=q4_0    # 4× экономия памяти KV cache
export OLLAMA_FLASH_ATTENTION=1      # экономия + ускорение
export OLLAMA_NUM_PARALLEL=2         # 2 параллельных запроса
export OLLAMA_KEEP_ALIVE=10m         # держать модель 10 минут
export OLLAMA_GPU_OVERHEAD=536870912 # 512 MB — запас для системы
EOF
```

Затем:

```bash
source ~/.zshrc
```

### Как проверить, что переменные применились

```bash
# Показать все установленные переменные Ollama
env | grep OLLAMA
```

Перезапустите `ollama serve` или приложение Ollama после изменения переменных.

---

## 2. Modelfile — кастомные модели

**Modelfile** — это аналог Dockerfile, только для AI-моделей. С его помощью можно:

- Сменить системный промпт (как модель себя ведёт)
- Настроить параметры генерации (температуру, контекст)
- Создать специализированную версию модели под свою задачу

### Простой Modelfile

Создайте файл `Modelfile` (без расширения) в любой папке:

```dockerfile
FROM qwen3.5:4b
PARAMETER temperature 0.7
PARAMETER num_ctx 8192
SYSTEM "Ты — дружелюбный AI-ассистент. Отвечай кратко и по делу."
```

**Что здесь происходит:**
- `FROM` — берём за основу модель `qwen3.5:4b`
- `PARAMETER` — меняем настройки:
  - `temperature 0.7` — умеренная креативность (0 = всегда одинаково, 2 = хаос)
  - `num_ctx 8192` — контекст 8K токенов (вместо стандартных 2048)
- `SYSTEM` — задаём системное сообщение (как модель должна себя вести)

### Сборка и запуск

```bash
# Собрать кастомную модель
ollama create my-assistant -f ./Modelfile

# Запустить
ollama run my-assistant
```

### Все параметры PARAMETER

| Параметр | По умолч. | Что делает | Пример |
|---|---|---|---|
| `temperature` | 0.8 | Креативность ответов (0.0 — строго, 2.0 — хаос) | `0.3` для фактов, `1.2` для идей |
| `num_ctx` | 2048 | Размер контекстного окна (токенов) | `8192` для длинных диалогов |
| `num_predict` | -1 (∞) | Максимум токенов в ответе | `512` для коротких ответов |
| `top_k` | 40 | Сколько лучших вариантов рассматривать | `10` для более точных ответов |
| `top_p` | 0.9 | Nucleus sampling (0-1) | `0.5` для более уверенных ответов |
| `min_p` | 0.0 | Минимальная вероятность токена | `0.05` для фильтрации мусора |
| `seed` | 0 | Зерно рандома (для повторяемости) | `42` — всегда одинаковый ответ |
| `repeat_penalty` | 1.1 | Штраф за повторение фраз | `1.2` если модель зацикливается |
| `stop` | — | Стоп-последовательности | `"\\n"` — остановиться на переносе строки |

### Примеры готовых Modelfile

**Для перевода:**
```dockerfile
FROM qwen3.5:4b
PARAMETER temperature 0.3
SYSTEM "Ты — профессиональный переводчик. Переводи текст с русского на английский и обратно. Отвечай только переводом, без комментариев."
```

**Для написания кода:**
```dockerfile
FROM qwen2.5-coder:7b
PARAMETER temperature 0.2
PARAMETER num_ctx 16384
SYSTEM "Ты — senior Python-разработчик. Пиши код с type hints, докстрингами и обработкой ошибок. Используй современные практики."
```

**Для мозгового штурма:**
```dockerfile
FROM qwen3.5:4b
PARAMETER temperature 1.5
PARAMETER top_p 0.95
SYSTEM "Ты — креативный копирайтер. Генерируй необычные идеи, мысли нестандартно."
```

### TEMPLATE — формат промпта

Некоторым моделям нужен специальный формат сообщений:

```dockerfile
FROM llama3.3:8b
TEMPLATE """<|begin_of_text|><|start_header_id|>system<|end_header_id|>

{{ .System }}<|eot_id|><|start_header_id|>user<|end_header_id|>

{{ .Prompt }}<|eot_id|><|start_header_id|>assistant<|end_header_id|>"""
```

Обычно template не нужно менять — он уже встроен в модель. Но если модель ведёт себя странно,
можно переопределить.

### Просмотр информации о модели

```bash
# Показать Modelfile установленной модели
ollama show qwen3.5:4b --modelfile

# Показать параметры модели
ollama show qwen3.5:4b --parameters

# Показать системный промпт
ollama show qwen3.5:4b --system
```

---

## 3. HTTP API — доступ из программ

Ollama запускает HTTP-сервер на порту 11434. К нему можно обращаться из любой программы,
которая умеет делать HTTP-запросы — Python, JavaScript, curl и т.д.

### Базовые запросы через curl

**Chat (диалог):**

```bash
curl http://localhost:11434/api/chat -d '{
  "model": "qwen3.5:4b",
  "messages": [
    {"role": "user", "content": "Привет! Как дела?"}
  ],
  "stream": false
}'
```

Параметр `"stream": false` — ждём полный ответ. Без него ответ будет приходить по частям (streaming).

**Generate (однострочный запрос):**

```bash
curl http://localhost:11434/api/generate -d '{
  "model": "qwen3.5:4b",
  "prompt": "Напиши хайку про программиста",
  "stream": false
}'
```

### JSON mode (получить ответ в формате JSON)

```bash
curl http://localhost:11434/api/chat -d '{
  "model": "qwen3.5:4b",
  "messages": [
    {"role": "user", "content": "Извлеки имя, возраст и город из: Меня зовут Иван, мне 25, я из Москвы"}
  ],
  "format": "json",
  "stream": false
}'
```

**Результат:** модель сама догадается вернуть JSON:
```json
{"name": "Иван", "age": 25, "city": "Москва"}
```

### Structured output (JSON Schema) — точный контроль

Если нужно гарантировать структуру ответа, опишите её через JSON Schema:

```bash
curl http://localhost:11434/api/chat -d '{
  "model": "qwen3.5:4b",
  "messages": [
    {"role": "user", "content": "Извлеки данные из: Вася, 30 лет, инженер"}
  ],
  "format": {
    "type": "object",
    "properties": {
      "name": {"type": "string"},
      "age": {"type": "integer"},
      "occupation": {"type": "string"}
    },
    "required": ["name", "age"]
  },
  "stream": false
}'
```

### Эмбеддинги (векторизация текста)

```bash
curl http://localhost:11434/api/embed -d '{
  "model": "all-minilm",
  "input": "Текст для векторизации"
}'
```

### Управление памятью

**Посмотреть загруженные модели:**

```bash
curl http://localhost:11434/api/ps
```

**Выгрузить модель из памяти:**

```bash
curl http://localhost:11434/api/generate -d '{
  "model": "qwen3.5:4b",
  "keep_alive": 0
}'
```

Значение `keep_alive: 0` означает «выгрузить сразу». По умолчанию модель живёт в памяти 5 минут.

### Streaming (поточный ответ)

Если не указывать `"stream": false`, Ollama будет возвращать ответ по частям — токен за токеном.
Это даёт эффект «живого печатания» в чате:

```bash
curl http://localhost:11434/api/chat -d '{
  "model": "qwen3.5:4b",
  "messages": [
    {"role": "user", "content": "Расскажи историю про робота"}
  ]
}'
```

Каждая строка ответа — отдельный JSON с одним токеном. Последняя строка содержит `"done": true`.

### Список установленных моделей

```bash
curl http://localhost:11434/api/tags
```

### Python (библиотека requests)

```python
import requests
import json

response = requests.post(
    "http://localhost:11434/api/chat",
    json={
        "model": "qwen3.5:4b",
        "messages": [{"role": "user", "content": "Привет!"}],
        "stream": False
    }
)
print(response.json()["message"]["content"])
```

---

## 4. OpenAI-совместимые эндпоинты

Ollama поддерживает API, совместимый с OpenAI. Это значит, что **любая программа,
которая работает с OpenAI**, может работать с Ollama — достаточно сменить адрес сервера.

Это работает для: Continue.dev, Aider, Open Interpreter, Cursor, и любых кастомных скриптов.

### Endpoints

| OpenAI endpoint | Ollama endpoint | Описание |
|---|---|---|
| `https://api.openai.com/v1/chat/completions` | `http://localhost:11434/v1/chat/completions` | Чат |
| `https://api.openai.com/v1/completions` | `http://localhost:11434/v1/completions` | Генерация |
| `https://api.openai.com/v1/embeddings` | `http://localhost:11434/v1/embeddings` | Эмбеддинги |
| `https://api.openai.com/v1/models` | `http://localhost:11434/v1/models` | Список моделей |

### curl

```bash
curl http://localhost:11434/v1/chat/completions -d '{
  "model": "qwen3.5:4b",
  "messages": [{"role": "user", "content": "Привет!"}]
}'
```

### Python (openai SDK)

```python
from openai import OpenAI

client = OpenAI(
    base_url="http://localhost:11434/v1",
    api_key="ollama"  # любое значение, Ollama всё равно не проверяет
)

response = client.chat.completions.create(
    model="qwen3.5:4b",
    messages=[{"role": "user", "content": "Привет!"}]
)

print(response.choices[0].message.content)
```

### JavaScript / Node.js

```javascript
import OpenAI from 'openai';

const client = new OpenAI({
  baseURL: 'http://localhost:11434/v1',
  apiKey: 'ollama',
});

const response = await client.chat.completions.create({
  model: 'qwen3.5:4b',
  messages: [{ role: 'user', content: 'Привет!' }],
});

console.log(response.choices[0].message.content);
```

### Как это использовать в Continue.dev

В `~/.continue/config.json`:

```json
{
  "models": [
    {
      "title": "Локальный чат",
      "provider": "openai",
      "model": "qwen3.5:4b",
      "apiBase": "http://localhost:11434/v1",
      "apiKey": "ollama"
    },
    {
      "title": "Автодополнение",
      "provider": "openai",
      "model": "qwen2.5-coder:1.5b",
      "apiBase": "http://localhost:11434/v1",
      "apiKey": "ollama"
    }
  ]
}
```

### Как это использовать в Aider

```bash
export OLLAMA_API_BASE=http://localhost:11434
aider --model ollama/qwen2.5-coder:7b
```

### Важные отличия от настоящего OpenAI API

| Особенность | OpenAI | Ollama |
|---|---|---|
| API key | Реальный ключ | Любая строка (или `ollama`) |
| Streaming | По умолчанию | По умолчанию |
| `max_tokens` | Работает | Работает |
| `temperature` | Работает | Работает |
| `tools` / `functions` | Работает | Поддерживается (tool calling) |
| `response_format` | `json_object` | `format: "json"` или JSON Schema |
| GPT-4 модели | Есть | Нет (только локальные) |

---

## 5. Параллельные запросы и несколько моделей

По умолчанию Ollama обрабатывает запросы последовательно: один запрос — один ответ.
Но можно включить параллельную обработку.

### Параллельные запросы к одной модели

Установите переменную `OLLAMA_NUM_PARALLEL` перед запуском сервера:

```bash
export OLLAMA_NUM_PARALLEL=2
ollama serve
```

Теперь к одной модели можно обращаться из двух приложений одновременно —
например, Continue.dev в VS Code и чат в Open WebUI.

**Сколько можно поставить:** зависит от вашей RAM. Если модель весит 4 GB и у вас 16 GB,
можно ставить 2–3. Если модель весит 8 GB на 16 GB — только 1.

**Как проверить, сколько запросов сейчас обрабатывается:**

```bash
curl http://localhost:11434/api/ps
```

### Несколько моделей в памяти

```bash
export OLLAMA_MAX_LOADED_MODELS=3
ollama serve
```

Теперь можно держать в памяти до 3 разных моделей одновременно.
Переключение между ними будет мгновенным (без повторной загрузки).

**Пример сценария:**
1. `qwen3.5:4b` — быстрый чат (3.4 GB)
2. `qwen2.5-coder:7b` — кодинг (4.7 GB)
3. `all-minilm` — эмбеддинги (0.1 GB)

Итого: ~8 GB занято, что нормально для 16 GB Mac.

### Как выгрузить модель из памяти вручную

```bash
# Через API
curl http://localhost:11434/api/generate -d '{
  "model": "qwen3.5:4b",
  "keep_alive": 0
}'
```

Или через `ollama ps`, чтобы сначала посмотреть, что загружено:

```bash
ollama ps
```

### Как понять, что модель выгрузилась

Запустите `ollama ps` — он покажет пустой список, если ни одна модель не загружена.

**Важно:** Если модель не используется, Ollama выгрузит её автоматически через
`OLLAMA_KEEP_ALIVE` (по умолчанию 5 минут). Можно уменьшить:

```bash
export OLLAMA_KEEP_ALIVE=30s   # выгружать через 30 секунд бездействия
```

---

## 6. Speculative decoding (ускорение)

Speculative decoding — техника, при которой **маленькая и быстрая модель** (draft model)
набрасывает черновик ответа, а **большая модель** его проверяет и исправляет.

**Результат:** генерация ускоряется в 1.5–2× без потери качества.

### Как это работает

1. Draft-модель (маленькая) быстро предполагает следующие 5–10 токенов
2. Большая модель проверяет их все сразу (параллельно)
3. Если draft угадал — принимаем, если ошибся — большая модель исправляет

### Настройка в Ollama через Modelfile

```dockerfile
FROM qwen2.5-coder:7b
DRAFT qwen2.5-coder:1.5b
```

Соберите модель:

```bash
ollama create my-coder-sd -f ./Modelfile
ollama run my-coder-sd
```

Модель `qwen2.5-coder:7b` будет использовать `qwen2.5-coder:1.5b` как draft.

### Другой способ — через API

```bash
curl http://localhost:11434/api/generate -d '{
  "model": "qwen2.5-coder:7b",
  "prompt": "Напиши функцию сортировки на Python",
  "options": {
    "draft": "qwen2.5-coder:1.5b"
  },
  "stream": false
}'
```

### Какую draft-модель выбрать

| Для большой модели | Хорошая draft-модель | Ускорение |
|---|---|---|
| 7B–9B | 1.5B из того же семейства | ×1.3–1.8 |
| 14B | 1.5B–4B | ×1.5–2.0 |
| 30B–70B | 7B–8B | ×1.5–2.5 |

**Правило:** draft-модель должна быть из того же семейства (например, Qwen + Qwen,
Llama + Llama) и занимать примерно в 4–10× меньше RAM.

**Где взять draft-модель:**

```bash
# Qwen 2.5 Coder — 1.5B для кодинга
ollama pull qwen2.5-coder:1.5b

# Llama 3.2 — 3B (для большой Llama)
ollama pull llama3.2:3b
```

### Практические цифры

На M4 Pro 24 GB, Qwen3-Coder-30B:

| Режим | tok/s | Ускорение |
|---|---|---|
| Без speculative decoding | 70 | 1× |
| С draft (Qwen 2.5 Coder 1.5B) | 110–120 | **×1.6** |

Speculative decoding даёт наибольший прирост на больших моделях (30B+). На моделях
до 7B ускорение меньше — 10–30%.

### Когда НЕ использовать

- Если у вас мало RAM (draft-модель занимает дополнительное место)
- Если модель уже отвечает быстро (>50 tok/s) — ускорение незначительно
- Если draft-модель из другого семейства — качество угадывания падает

---

## 7. Что дальше

После настройки производительности и API:

| Если хотите | Переходите |
|-------------|-----------|
| Сравнить все инструменты (MLX, llama.cpp, vLLM) | [tools.md](tools.ru.md) |
| Выбрать квантизацию (Q4, Q5, Q8) | [quantization.md](quantization.ru.md) |
| Посмотреть бенчмарки на Mac | [benchmarks/apple-silicon.md](benchmarks/apple-silicon.ru.md) |
| Полный каталог всех моделей | [catalog.md](catalog.ru.md) |
| Решить проблему | [troubleshooting.md](troubleshooting.ru.md) |
