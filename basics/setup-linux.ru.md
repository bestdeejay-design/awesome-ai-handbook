# 🐧 Установка Ollama на Linux

> Пошаговое руководство для пользователей Linux: от CPU до мульти-GPU.

[← Установка на Windows](setup-windows.ru.md) · [Пошаговый план обучения →](learning-path.ru.md)

---

## Содержание

1. [Простая установка (CPU)](#1-простая-установка-cpu)
2. [Установка с GPU (NVIDIA CUDA)](#2-установка-с-gpu-nvidia-cuda)
3. [Установка через Docker](#3-установка-через-docker)
4. [Серверная настройка (systemd)](#4-серверная-настройка-systemd)
5. [Запуск первой модели](#5-запуск-первой-модели)
6. [Настройка для кодинга](#6-настройка-для-кодинга)
7. [Частые проблемы](#7-частые-проблемы)
8. [Что дальше](#8-что-дальше)

---

## 1. Простая установка (CPU)

Если у вас нет дискретной видеокарты NVIDIA — Ollama будет работать на процессоре. Медленнее, чем с GPU, но для маленьких моделей (до 7B) вполне юзабельно.

### Установка (одна команда)

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

После установки Ollama запускается как системный сервис:

```bash
# Проверить статус
systemctl status ollama

# Посмотреть логи
journalctl -u ollama -f
```

### Проверка

```bash
ollama --version
curl http://localhost:11434/api/version
```

---

## 2. Установка с GPU (NVIDIA CUDA)

### Шаг 1. Убедитесь, что NVIDIA драйверы установлены

```bash
nvidia-smi
```

Должны показать вашу видеокарту, версию драйвера и объём VRAM.

### Шаг 2. Установите Ollama

Та же команда:
```bash
curl -fsSL https://ollama.com/install.sh | sh
```

Установщик автоматически обнаружит CUDA и настроит GPU-ускорение.

### Шаг 3. Проверьте, что GPU используется

```bash
ollama ps
```

Должен показать `100% GPU` при запущенной модели. Если показывает `0% GPU` — что-то пошло не так.

### Если GPU не определился

Установите вручную:
```bash
# NVIDIA Container Toolkit (требуется для Docker)
sudo apt install nvidia-container-toolkit   # Debian/Ubuntu
sudo dnf install nvidia-container-toolkit   # Fedora

# Или проверьте переменные окружения
export OLLAMA_CUDA=1
ollama serve
```

---

## 3. Установка через Docker

Рекомендую для серверов и тонкой настройки:

```bash
# С GPU (NVIDIA)
docker run -d --gpus all -p 11434:11434 \
  -v ollama:/root/.ollama \
  --name ollama ollama/ollama

# Без GPU (CPU only)
docker run -d -p 11434:11434 \
  -v ollama:/root/.ollama \
  --name ollama ollama/ollama
```

**Переменные окружения для Docker:**
```bash
docker run -d --gpus all -p 11434:11434 \
  -v ollama:/root/.ollama \
  -e OLLAMA_CONTEXT_LENGTH=32768 \
  -e OLLAMA_FLASH_ATTENTION=1 \
  -e OLLAMA_KV_CACHE_TYPE=q8_0 \
  --name ollama ollama/ollama
```

---

## 4. Серверная настройка (systemd)

Если Ollama стоит не через Docker, а нативно:

```bash
# Редактируем конфиг сервиса
sudo systemctl edit ollama.service
```

Добавьте:
```
[Service]
Environment="OLLAMA_HOST=0.0.0.0:11434"
Environment="OLLAMA_CONTEXT_LENGTH=32768"
Environment="OLLAMA_FLASH_ATTENTION=1"
Environment="OLLAMA_KV_CACHE_TYPE=q8_0"
```

Перезапустите:
```bash
sudo systemctl daemon-reload
sudo systemctl restart ollama
```

> **Важно:** `OLLAMA_HOST=0.0.0.0` открывает доступ к Ollama по сети. Если это ваш личный компьютер — можно оставить `127.0.0.1` (только локальный доступ).

---

## 5. Запуск первой модели

```bash
ollama run qwen3.5:4b
```

После скачивания (~3.4 GB) появится приглашение `>>>` — модель готова.

**Что попробовать:**
```
>>> Напиши bash-скрипт, который находит все файлы > 100 MB
>>> Объясни разницу между soft и hard link в Linux
```

---

## 6. Настройка для кодинга

### VS Code + Continue.dev

Установите VS Code, затем расширение **Continue**. Настройка:

```json
{
  "models": [{
    "title": "Local Qwen",
    "provider": "ollama",
    "model": "qwen2.5-coder:7b",
    "apiBase": "http://localhost:11434"
  }]
}
```

### Aider

```bash
pip install aider-chat
export OLLAMA_API_BASE=http://127.0.0.1:11434
aider --model ollama_chat/qwen2.5-coder:7b
```

Подробнее: [`../use-cases/coding.ru.md`](../use-cases/coding.ru.md)

---

## 7. Частые проблемы

### Не хватает прав
```bash
# Добавьте пользователя в группу docker (если используете Docker)
sudo usermod -aG docker $USER

# Или запускайте Ollama от текущего пользователя
ollama serve
```

### GPU не используется в Docker
```bash
# Установите nvidia-container-toolkit
sudo apt install nvidia-container-toolkit
sudo systemctl restart docker

# И используйте флаг --gpus all
docker run -d --gpus all ...
```

### Модель падает с ошибкой памяти
- Уменьшите контекст: `OLLAMA_CONTEXT_LENGTH=4096`
- Используйте более сильное квантование: `qwen3.5:4b:q3_k_m`
- Закройте другие программы, потребляющие RAM

### Ошибка «libcuda.so not found»
```bash
# Установите CUDA Toolkit
sudo apt install nvidia-cuda-toolkit   # Ubuntu/Debian
sudo dnf install cuda                  # Fedora
```

### Как остановить сервер
```bash
sudo systemctl stop ollama     # если установлен как сервис
docker stop ollama             # если в Docker
```

---

## 8. Что дальше

| Если хотите | Переходите |
|-------------|-----------|
| **Выбрать модель под задачу** | [`../local-models/models.ru.md`](../local-models/models.ru.md) |
| **Настроить кодинг с AI** | [`../use-cases/coding.ru.md`](../use-cases/coding.ru.md) |
| **Сравнить инструменты** | [`../local-models/tools.ru.md`](../local-models/tools.ru.md) |
| **Разобраться с квантизацией** | [`../local-models/quantization.ru.md`](../local-models/quantization.ru.md) |
| **Вернуться к плану обучения** | [learning-path.ru.md](learning-path.ru.md) |
| **Вернуться к навигации** | [README.ru.md](README.ru.md) |
