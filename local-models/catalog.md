# 📚 Полный каталог open-weight LLM для локального запуска

> Дата данных: 23 июля 2026. Все модели — open-weight, доступные для скачивания и локального запуска.

---

## Содержание

1. [Кодинг (Coding)](#1-кодинг-модели)
2. [Общие чат-модели](#2-общие-чатинструкционные-модели)
3. [Рассуждения / математика (Reasoning)](#3-модели-рассужденийматематики)
4. [Мультимодальные / Vision](#4-мультимодальные-vision-модели)
5. [Маленькие модели (до 4B)](#5-маленькие-быстрые-модели-до-4b)
6. [MoE (эффективные архитектуры)](#6-moe-mixture-of-experts-модели)
7. [Модели с длинным контекстом](#7-модели-с-длинным-контекстом)
8. [Скрытые жемчужины (Hidden Gems)](#8-скрытые-жемчужины-hidden-gems)

---

## 1. Кодинг-модели

### Маленькие (до 7B) — для 8-16GB RAM

| Модель | Параметры | Контекст | FIM | HumanEval | Где запускать | Ollama |
|--------|-----------|----------|-----|-----------|--------------|--------|
| **Stable Code 3B** | 2.7B | 16K | ✅ | ~55% | 4GB VRAM, CPU | `ollama run stable-code:3b` |
| **CodeGemma 2B** | 2B | 8K | ✅ | ~50% | 2GB VRAM | `ollama run codegemma:2b` |
| **Qwen 2.5 Coder 1.5B** | 1.5B | 32K | ✅ | ~45% | 2GB, CPU | `ollama run qwen2.5-coder:1.5b` |
| **Yi-Coder 1.5B** | 1.5B | 128K | ✅ | ~48% | 2GB, CPU | `ollama run yi-coder:1.5b` |
| **Granite Code 3B** | 3B | 4K | ✅ | ~52% | 4GB | Community |
| **Phi-4-mini (3.8B)** | 3.8B | 128K | ❌ | **74.4%** | 3GB | `ollama run phi4-mini` |
| **Qwen 2.5 Coder 7B** | 7B | 128K | ✅ | **82%** | 6GB | `ollama run qwen2.5-coder:7b` |
| **CodeGemma 7B** | 7B | 8K | ✅ | ~65% | 8GB | `ollama run codegemma:7b` |
| **StarCoder2 7B** | 7B | 16K | ✅ | ~60% | 7GB | Community |
| **DeepSeek R1 Distill Qwen 7B** | 7B | 32K | ❌ | ~58% | 6GB | `ollama run deepseek-r1:7b` |
| **Mistral Small 3 (7B)** | 7B | 128K | ❌ | ~60% | 6GB | `ollama run mistral-small:7b` |
| **Yi-Coder 9B** | 9B | 128K | ✅ | ~70% | 8GB | `ollama run yi-coder:9b` |

**Вывод:** Для кодинга на 16GB Mac — **Qwen 2.5 Coder 7B** (FIM, 82% HumanEval). Если влезает — **Yi-Coder 9B** (128K контекст). Для автодополнения (FIM) — **CodeGemma 2B** или **Qwen 2.5 Coder 1.5B** (молниеносно).

### Средние (7B-30B) — для 24-32GB RAM

| Модель | Параметры | Контекст | SWE-bench | LiveCodeBench | Когда выбирать |
|--------|-----------|----------|-----------|---------------|----------------|
| **Qwen3-Coder-30B-A3B** (MoE) | 30B / 3.3B active | 256K | ~70% | — | Лучший MoE-кодер, 256K ctx |
| **Devstral Small 2 (24B)** | 24B dense | 256K | **68.0%** | — | Агентный кодинг, мультифайловые правки |
| **Codestral 25.01 (22B)** | 22B dense | 256K | — | — | **Лучший FIM** (95.3%), 80+ языков |
| **gpt-oss-20b** | 20B MoE | 128K | — | — | ~o3-mini, Apache 2.0 |
| **Granite Code 20B** | 20B | 4K | — | — | Enterprise, Apache 2.0 |
| **Phi-4 (14B)** | 14B | 16K | — | — | MMLU 84.8%, кодинг + математика |
| **Phi-4-reasoning (14B)** | 14B | 16K | — | — | Reasoning + код |
| **DeepSeek R1 Distill Qwen 14B** | 14B | 32K | — | — | Reasoning для сложных задач |
| **Qwen 2.5 Coder 14B** | 14B | 128K | — | — | FIM, 128K ctx |
| **StarCoder2 15B** | 15B | 16K | — | — | 600+ языков |

### Крупные (30B+) — для 48-128GB RAM или cloud

| Модель | Параметры | SWE-bench | LiveCodeBench | Лицензия |
|--------|-----------|-----------|---------------|----------|
| **Qwen3.6-27B dense** | 27B | **77.2%** | **83.9%** | Apache 2.0 ⭐ **лучшая consumer coding model 2026** |
| **DeepSeek-V4-Flash** | 284B / 13B active | ~78% | ~80% | MIT |
| **Devstral 2 (123B)** | 123B dense | 72.2% | — | Modified MIT |
| **Qwen3-Coder-480B** | 480B | 69.6% | — | Apache 2.0 |
| **Kimi K2.6** | 1T / 38B active | **80.2%** | 72.4% | Modified MIT |
| **Kimi K2.7 Code** | 1T+ / ~38B active | — | — | Modified MIT |
| **Kimi K3** | **2.8T / ~50B active** | — | — | Modified MIT (веса 27 июля) |
| **GLM-5.1** | 744B / 40B active | — | 73.9% | MIT |
| **DeepSeek-V4-Pro** | 1.6T / 49B active | **80.6%** | **93.5%** | MIT ⭐ **лучший open-weight coder** |

---

## 2. Общие чат/инструкционные модели

### 2.1 Qwen 3.5 (февраль 2026) — все Apache 2.0

| Модель | Параметры | Контекст | Особенность |
|--------|-----------|----------|-------------|
| Qwen3.5-0.8B | 0.8B | 256K | Edge, CPU inference |
| Qwen3.5-2B | 2B | 256K | Mobile |
| Qwen3.5-4B | 4B | 256K | Ноутбуки |
| **Qwen3.5-9B** | **9B** | **256K** | **Sweet spot для 16GB** |
| Qwen3.5-27B | 27B | 256K | 24GB GPU |
| Qwen3.5-35B-A3B | 35B / 3B active (MoE) | 262K | Эффективный MoE |
| Qwen3.5-122B-A10B | 122B / 10B active (MoE) | 262K | Mid-range frontier |
| **Qwen3.5-397B-A17B** | 397B / 17B active | 262K | Флагман (Hybrid DeltaNet) |

### 2.2 Qwen 3.6 (апрель 2026) — Apache 2.0

| Модель | Параметры | Контекст | Ключевое |
|--------|-----------|----------|----------|
| **Qwen3.6-27B** | 27B dense | 262K | **SWE-bench 77.2%, нативный multimodal** |
| Qwen3.6-35B-A3B | 35B / 3B active (MoE) | 262K | MoE версия |

**Важно:** Qwen3.6-27B — лучшая локальная модель 2026 для кодинга. В Q4_K_M ~17GB, помещается в 24GB.

### 2.3 Meta Llama 4 (апрель 2026)

| Модель | Параметры | Контекст | MMLU-Pro | Особенность |
|--------|-----------|----------|----------|-------------|
| **Llama 4 Scout** | 109B / 17B active (MoE) | **10M** | — | **Самый длинный контекст**, 1x H100 |
| **Llama 4 Maverick** | 400B / 17B active (MoE) | 1M | 80.5% | Multimodal |

### 2.4 Llama 3.x (проверенная классика)

| Модель | Параметры | Контекст | Когда выбирать |
|--------|-----------|----------|----------------|
| Llama 3.2 1B | 1B | 128K | Edge / CPU |
| Llama 3.2 3B | 3B | 128K | 8GB RAM, быстрый |
| Llama 3.1 8B | 8B | 128K | Проверенный baseline |
| Llama 3.3 8B | 8B | 128K | Улучшенный 3.1 8B |
| Llama 3.3 70B | 70B | 128K | 32GB+, мощный general |

### 2.5 Google Gemma 4 (апрель 2026) — все Apache 2.0

| Модель | Параметры | Контекст | MMLU-Pro | AIME | Особенность |
|--------|-----------|----------|----------|------|-------------|
| **Gemma 4 E2B** | ~2.3B eff (5.1B total) | 128K | — | — | Телефоны, IoT, аудио |
| **Gemma 4 E4B** | ~4.5B eff (8B total) | 128K | 69.4% | 42.5% | Mobile + vision |
| **Gemma 4 12B Unified** | ~12B | 256K | — | — | Text + image + audio |
| **Gemma 4 26B A4B MoE** | 25.2B / 3.8B active | 256K | **82.6%** | **88.3%** | **97% качества 31B при цене 4B** |
| **Gemma 4 31B Dense** | 30.7B | 256K | **85.2%** | **89.2%** | **Лучшая single-GPU модель** |

### 2.6 Другие

| Модель | Параметры | Контекст | Особенность |
|--------|-----------|----------|-------------|
| **gpt-oss-20b** (OpenAI) | 20B MoE | 128K | Оpen-weight от OpenAI, Apache 2.0 |
| **gpt-oss-120b** (OpenAI) | 117B / 5.1B active | 128K | Codeforces Elo 2622, Apache 2.0 |
| **GLM-5.2** | 753B / ~40B active | 1M | SWE-Bench Pro 62.1%, MIT |
| **Mistral Large 2** | ~123B MoE | 128K | Фронтовый general-purpose |
| **Command R+** | 104B | 128K | RAG-специализированный |
| **Yi-1.5** | 6B / 9B / 34B | 200K | Длинный контекст |

---

## 3. Модели рассуждений/математики

| Модель | Параметры | Контекст | MATH-500 | AIME 2024 | Где запускать |
|--------|-----------|----------|----------|-----------|---------------|
| **Phi-4-reasoning** | 14B | 16K | — | — | 16GB ✅ |
| **Qwen QwQ-32B** | 32B | 32K | — | — | 24GB ✅ |
| **DeepSeek-R1-Distill-Qwen-1.5B** | 1.5B | 32K | — | — | 4GB ✅ |
| **DeepSeek-R1-Distill-Qwen-7B** | 7B | 32K | — | — | 8GB ✅ |
| **DeepSeek-R1-Distill-Llama-8B** | 8B | 32K | — | — | 8GB ✅ |
| **DeepSeek-R1-Distill-Qwen-14B** | 14B | 32K | — | — | 16GB ✅ |
| **DeepSeek-R1-Distill-Qwen-32B** | 32B | 32K | — | — | 24GB ✅ |
| **DeepSeek-R1-Distill-Llama-70B** | 70B | 32K | — | — | 48GB ✅ |
| **DeepSeek-R1** (full) | 671B / 37B active | 128K | **97.3%** | **79.8%** | Cloud / кластер |

**Как выбрать:**
- 8GB RAM → R1-Distill-7B (reasoning в 2-3 раза лучше обычной 7B)
- 16GB RAM → R1-Distill-14B или Phi-4-reasoning
- 24-32GB RAM → R1-Distill-32B или QwQ-32B
- **Важно:** R1 distills **не умеют FIM** (автодополнение кода). Для кодинга берите Qwen Coder.

---

## 4. Мультимодальные (Vision) модели

| Модель | Параметры | VRAM (Q4) | Контекст | Сильные стороны |
|--------|-----------|-----------|----------|-----------------|
| **Moondream 2** | 1.9B | ~2 GB | — | Ультра-лёгкий, фото |
| **PaliGemma 2** | 3B | ~3 GB | — | Google, фото + документы |
| **SmolVLM 2** | 2.2B | ~2 GB | — | HuggingFace, компактный |
| **LLaVA 1.6 7B** | 7B | ~6 GB | — | Классика, фото/документы |
| **MiniCPM-V 4.5 (8B)** | 8B | ~6 GB | — | **Лучший OCR для документов** |
| **Qwen3-VL 8B** | 7B | ~6 GB | 256K | Мультиязычный OCR, dynamic resolution |
| **InternVL 2.5 8B** | 8B | ~8 GB | — | **Лучший для UI/charts/code screenshots** |
| **Llama 3.2 Vision 11B** | 11B | ~8 GB | 128K | Лучший all-rounder для 8-16GB |
| **Gemma 4 12B Unified** | ~12B | ~8 GB | 256K | Text + image + audio |
| **Gemma 4 26B A4B MoE** | 25.2B / 3.8B active | ~14 GB | 256K | Vision + 140+ языков |
| **Gemma 4 31B Dense** | 30.7B | ~20 GB | 256K | **Лучшая single-GPU vision** |
| **Qwen3-VL 72B** | 72B | ~48 GB | 256K | Лучший open-source VLM |
| **Llama 3.2 Vision 90B** | 90B | ~64 GB | 128K | Лучший локальный VLM |

---

## 5. Маленькие / быстрые модели (до 4B)

Для 8GB RAM, CPU-инференса, edge-устройств.

| Модель | Параметры | RAM (Q4) | MMLU | HumanEval | tok/s (M1 8GB) |
|--------|-----------|----------|------|-----------|-----------------|
| **SmolLM2-135M** | 135M | ~0.2 GB | — | — | 100+ |
| **SmolLM2-1.7B** | 1.7B | ~1.2 GB | — | — | 50+ |
| **Qwen3.5-0.8B** | 0.8B | ~0.5 GB | — | — | 60+ |
| **Llama 3.2 1B** | 1B | ~0.8 GB | — | — | 40-50 |
| **TinyLlama 1.1B** | 1.1B | ~0.7 GB | — | — | 40+ |
| **Llama 3.2 3B** | 3B | ~2.0 GB | 63.4% | — | 30-40 |
| **Qwen3.5-2B** | 2B | ~1.5 GB | — | ~55% | 35+ |
| **Gemma 4 E2B** | ~2.3B eff | ~1.5 GB | ~60% | — | 30+ |
| **Qwen3-4B** | 4B | ~2.8 GB | ~70% | **~74%** | 22-26 |
| **Gemma 3 4B** | 4B | ~2.5 GB | 59.6% | 71.3% | ~24 |
| **Phi-4-mini** | 3.8B | ~2.5 GB | **67.3%** | **74.4%** | ~28 |
| **Gemma 4 E4B** | ~4.5B eff | ~5.0 GB | **69.4%** | 52% | ~20 |
| **SmolLM3-3B** | 3B | ~2.0 GB | — | — | — |
| **H2O-Danube 3B** | 3B | ~2.0 GB | — | — | — |
| **Stable LM 2 3B** | 3B | ~2.0 GB | — | — | — |

**Вывод для 8GB Mac:**
- Быстрый чат → **Llama 3.2 3B** (2GB, 30-40 tok/s)
- Кодинг → **Qwen3 4B** (2.8GB, лучший coding в этом классе)
- Math/reasoning → **Phi-4-mini** (2.5GB, MMLU 67.3%)
- Multimodal → **Gemma 3 4B** (2.5GB, vision!)

---

## 6. MoE (Mixture-of-Experts) модели

MoE активирует только часть параметров на каждый токен — выше качество при том же compute.

| Модель | Total / Active | Эксперты | Контекст | Качество | Где запускать |
|--------|---------------|----------|----------|----------|---------------|
| **Qwen3.5-35B-A3B** | 35B / 3B | — | 262K | ~9B level | 16-24GB ✅ |
| **Qwen3-Coder-30B-A3B** | 30B / 3.3B | — | 256K | coding leader | 24GB ✅ |
| **Gemma 4 26B A4B** | 25.2B / 3.8B | 128 (8+1) | 256K | 97% of 31B | **24GB ✅** |
| **gpt-oss-20b** | 20B / ~5B | — | 128K | ~o3-mini | 16GB ✅ |
| **Mixtral 8x7B** | 47B / 13B | 8 | 32K | solid | 32GB ✅ |
| **Mixtral 8x22B** | 141B / 39B | 8 | 65K | мощный | 64GB ✅ |
| **Qwen3.5-122B-A10B** | 122B / 10B | — | 262K | frontier | 48GB+ |
| **gpt-oss-120b** | 117B / 5.1B | 128 | 128K | Codeforces 2622 | 48GB+ |
| **Nemotron 3 Super** | 120B / 12B | 512 (top-22) | 1M | hybrid Mamba | 64GB+ |
| **Llama 4 Scout** | 109B / 17B | — | **10M** | solid | 48GB+ |
| **Llama 4 Maverick** | 400B / 17B | 128 | 1M | multimodal | 48GB+ |
| **DeepSeek-V4-Flash** | 284B / 13B | — | 1M | budget frontier | 64GB+ |
| **DeepSeek-V4-Pro** | 1.6T / 49B | 384+1 | 1M | **best coder** | 8xH200 |
| **Qwen3.5-397B-A17B** | 397B / 17B | 512 | 262K | hybrid DeltaNet | 64GB+ |
| **Kimi K3** | **2.8T / 50B** | **896 (16 active)** | **1M** | largest open-weight | Cluster |
| **GLM-5.2** | 753B / 40B | 256 (8 active) | 1M | MIT, SWE-Bench Pro 62.1% | Cluster |

**Вывод:** Для 24GB Mac — **Gemma 4 26B A4B** или **Qwen3-Coder-30B-A3B**. Для 16GB — **Qwen3.5-35B-A3B** (3B active, 20GB в Q4 — чуть тесно, но работает).

---

## 7. Модели с длинным контекстом

| Модель | Контекст | Параметры | Реальная работа на длинном |
|--------|----------|-----------|---------------------------|
| **Llama 4 Scout** | **10M** | 109B / 17B | ✅ RULER strong |
| **Kimi K2.6** | **2M** | ~1.2T / 38B | ✅ Agent swarm |
| **Kimi K3** | **1M** | 2.8T / 50B | ✅ Frontier |
| **DeepSeek-V4-Pro** | **1M** | 1.6T / 49B | ✅ MIT |
| **DeepSeek-V4-Flash** | **1M** | 284B / 13B | ✅ MIT, budget |
| **GLM-5.2** | **1M** | 753B / 40B | ✅ MIT |
| **Nemotron 3 Ultra** | **1M** | 550B / 55B | ✅ Hybrid Mamba |
| **Qwen3.5-397B-A17B** | 262K (ext. ~1M YaRN) | 397B / 17B | ✅ Apache 2.0 |
| **Qwen3.6-27B** | 262K | 27B dense | ✅ Apache 2.0, 24GB |
| **Gemma 4 31B** | 256K | 31B | ✅ RULER 66.4% @128K |
| **Llama 4 Maverick** | 1M | 400B / 17B | ✅ Multimodal |

**Важно:** Длинный контекст ≠ качество на длинном контексте. Gemma 4 получила RULER 66.4% на 128K — в 5× лучше Gemma 3. Qwen 3.5/3.6 поддерживают YaRN для расширения контекста.

---

## 8. Скрытые жемчужины (Hidden Gems)

Модели, которые незаслуженно обходят вниманием.

### Для кодинга

| Модель | Размер | Почему стоит попробовать |
|--------|--------|-------------------------|
| **Stable Code 3B** | 2.7B | Качество CodeLlama 7B при 3B размере. Лучший sub-3B кодер |
| **Granite Code 3B/8B** | 3-8B | Apache 2.0, встроенная фильтрация PII, 116 языков |
| **Yi-Coder 9B** | 9B | 128K контекст, бьёт CodeLlama 34B при 9B |
| **WaveCoder** | 7B | Специализация на code repair/refactoring |

### Для русского языка

| Модель | Размер | Почему |
|--------|--------|--------|
| **Vikhr-7B** | 7B | Лучшая open-source русская модель. Бьёт Saiga и ruadapt на NLU |
| **Saiga-YandexGPT-8B** | 8B | Тюнинг на YandexGPT 5 Lite, 4.98/5 беглость |
| **ruGPT-13B** | 13B | Обучена с нуля на русском корпусе |

### Специализированные домены

| Модель | Домен | Размер | Почему |
|--------|-------|--------|--------|
| **BioMistral 7B** | Медицина | 7B | Лучший open-source medical LLM под 10B |
| **ChatLaw 7B** | Юриспруденция | 7B | MoE + multi-agent, снижает галлюцинации |
| **FinGPT 7B** | Финансы | 7B | LoRA fine-tune за <$300 |

### Экспериментальные архитектуры

| Модель | Архитектура | Размер | Особенность |
|--------|------------|--------|-------------|
| **Falcon Mamba 7B** | Pure SSM | 7B | Постоянная память на 128K, 85 tok/s |
| **Codestral Mamba 7B** | SSM | 7B | Бесконечный контекст через рекуррентность |
| **Zamba 2 7B** | Mamba + Attention | 7B | 16K, гибрид |
| **RWKV 7 G1** | Recurrent | 1.5-14B | Бесконечный контекст, CPU-friendly |
| **Jamba 1.5 Mini** | Hybrid | 52B / 12B active | 256K, production hybrid |

### Community Favorites (r/LocalLLaMA)

| Модель | Вердикт сообщества |
|--------|-------------------|
| **Phi-3 Mini (3.8B)** | «Работает на чём угодно, удивительно способный» |
| **Gemma 3 9B** | «Apple Silicon sweet spot» |
| **Llama 4 Scout** | «Недооценён — 100% accuracy в batch-тестах» |
| **DeepSeek R1 Distills** | «Reasoning качество конкурентно с моделями в 10× больше» |
| **Stable Code 3B** | ⭐ **Лучшая hidden gem** — качество 7B при 3B |

---

## Быстрая таблица: какая модель на каком Mac?

| RAM Mac | Что запускать | tok/s | Качество |
|---------|--------------|-------|----------|
| **8GB** | Phi-4-mini (3.8B) или Qwen3.5-4B | 25-35 🟢 | Базовое-хорошее |
| **16GB** | Qwen 2.5 Coder 7B или Gemma 3 9B | 15-25 🟡 | Хорошее |
| **24GB** | Qwen3.6-27B (Q4) или Gemma 4 26B MoE | 18-28 🟡 | Отличное |
| **32GB** | Qwen3.6-35B-A3B MoE или Llama 3.1 70B (Q3) | 20-40 🟢 | Отличное |
| **48GB** | Llama 3.3 70B (Q4) или Qwen3.5-397B MoE (Q4) | 10-20 🟡 | Превосходное |
| **64GB** | Llama 3.3 70B (Q6) или Gemma 4 31B (Q8) | 8-15 🟠 | Почти FP16 |
| **128GB** | DeepSeek-V4-Flash или Qwen3.5-397B (Q8) | 15-30 🟢 | Максимальное |

> 🟢 Быстро | 🟡 Нормально | 🟠 Медленно, но терпимо

---

## Лицензии: что можно использовать коммерчески

| Лицензия | Модели | Коммерция |
|----------|--------|-----------|
| **Apache 2.0** | Qwen 3/3.5/3.6, Gemma 4, gpt-oss, Granite Code | ✅ Без ограничений |
| **MIT** | DeepSeek V3/V4/R1, GLM-5, Phi-4, Stable Code | ✅ Без ограничений |
| **Llama Community** | Llama 3.x/4 | ✅ >700M MAU — плати |
| **Modified MIT** | Kimi K2/K3, Devstral 2 (123B) | ⚠️ Читайте карту модели |
| **Mistral Non-Prod** | Codestral | ❌ Только некоммерческое |
| **Gemma Terms** | Gemma 3 | ⚠️ Не OSI-совместимая |

---

> **Последнее обновление:** 23 июля 2026
> **Источники:** HuggingFace model cards, Ollama library, r/LocalLLaMA, CraftRigs, MLJourney, arXiv, официальные анонсы
