# 🛠 Инструменты запуска

Сравнение популярных инструментов для запуска LLM локально.

## Таблица сравнения

| Инструмент | Интерфейс | Open Source | Apple Silicon | Лучше всего для | GitHub |
|---|---|---|---|---|---|
| **Ollama** | CLI + API | ✅ MIT | Metal | Разработчики, интеграции | [ollama/ollama](https://github.com/ollama/ollama) ⭐ 148K |
| **LM Studio** | GUI + CLI | ❌ | MLX авто | Начинающие, поиск моделей | [lmstudio-ai/lmstudio](https://github.com/lmstudio-ai/lmstudio) |
| **mlx-lm** | Python / CLI | ✅ Apache | Нативно | Макс. скорость на Mac | [ml-explore/mlx](https://github.com/ml-explore/mlx) |
| **llama.cpp** | CLI | ✅ MIT | Metal | Полный контроль | [ggml-org/llama.cpp](https://github.com/ggml-org/llama.cpp) ⭐ 70K+ |
| **GPT4All** | GUI | ✅ | Metal | RAG, работа с документами | [nomic-ai/gpt4all](https://github.com/nomic-ai/gpt4all) |
| **Jan** | GUI | ❌ | Metal | Приватность, десктопный чат | [janhq/jan](https://github.com/janhq/jan) ⭐ 35K |
| **Enchanted** | macOS GUI | ❌ | MLX нативно | Нативное macOS приложение | [AugustDev/Enchanted](https://github.com/AugustDev/Enchanted) |

## Подробности по инструментам

### Ollama - выбор для разработчиков
Ollama предоставляет простой CLI и OpenAI-совместимый API, что делает его идеальным для интеграции с IDE (Continue, Cursor) и собственными приложениями.

**Установка:**
```bash
brew install ollama  # macOS
curl -fsSL https://ollama.com/install.sh | sh  # Linux
```

**Основные команды:**
```bash
ollama list              # список установленных моделей
ollama run model_name    # запустить модель в интерактивном режиме
ollama pull model_name   # загрузить модель без запуска
ollama ps                # список запущенных моделей
```

### LM Studio - лучший для начала
Графический интерфейс с встроенным поиском моделей на HuggingFace. Автоматически определяет лучшие настройки для вашего железа.

**Особенности:**
- Перетаскивание GGUF файлов
- Встроенный чат-интерфейс
- API сервер в один клик
- Регулярные обновления с улучшениями производительности

### mlx-lm - для максимальной скорости на Mac
Библиотека от Apple, использующая Metal Performance Shaders для ускоренной работы на Apple Silicon.

**Пример использования:**
```bash
pip install mlx-lm
python -m mlx_lm.generate --model mlx-community/Qwen3-8B-4bit --prompt "Привет, как дела?"
```

### llama.cpp - для тех, кто хочет полного контроля
Оригинальная реализация Georgi Gerganova. Требует ручной настройки, но дает максимальную гибкость.

**Пример запуска:**
```bash
./main -m ./models/qwen3-8b.Q4_K_M.gguf -p "Привет! Как дела?" -n 128
```

### Выбор инструмента
- **Для разработчиков и интеграций:** Ollama
- **Для начинающих и визуальной работы:** LM Studio
- **Для максимальной производительности на Mac:** mlx-lm
- **Для полного контроля и экспериментов:** llama.cpp
- **Для работы с документами:** GPT4All или PrivateGPT