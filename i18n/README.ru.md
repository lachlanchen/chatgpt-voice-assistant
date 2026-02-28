[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


[![LazyingArt banner](https://github.com/lachlanchen/lachlanchen/raw/main/figs/banner.png)](https://github.com/lachlanchen/lachlanchen/blob/main/figs/banner.png)

# Чат-ассистент ChatGPT

| Компонент | Статус |
| --- | --- |
| 🧪 CI | ![CI](https://img.shields.io/badge/CI-configured-0EA5E9?style=flat-square&logo=githubactions&logoColor=white) |
| 🐍 Python | ![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?logo=python&logoColor=white) |
| 📦 Packaging | ![Poetry](https://img.shields.io/badge/Poetry-managed-60A5FA?logo=poetry&logoColor=white) |
| 🍎 Platform | ![Platform](https://img.shields.io/badge/Platform-macOS%20first-0F172A) |
| 🖥️ Interface | ![CLI](https://img.shields.io/badge/Interface-CLI-16A34A) |
| 🤝 Contributions | ![Contributing](https://img.shields.io/badge/Contributing-Welcome-22C55E?style=flat-square&logo=github&logoColor=white) |

![Workflow status](https://img.shields.io/github/actions/workflow/status/lachlanchen/chatgpt-voice-assistant/test-application.yml?branch=main&style=flat-square&logo=githubactions&logoColor=white)
[![Open Issues](https://img.shields.io/github/issues/lachlanchen/chatgpt-voice-assistant?style=flat-square&logo=github&logoColor=white)](https://github.com/lachlanchen/chatgpt-voice-assistant/issues)
[![Stars](https://img.shields.io/github/stars/lachlanchen/chatgpt-voice-assistant?style=flat-square&logo=github&logoColor=white)](https://github.com/lachlanchen/chatgpt-voice-assistant/stargazers)
[![Last commit](https://img.shields.io/github/last-commit/lachlanchen/chatgpt-voice-assistant?style=flat-square&logo=git&logoColor=white)](https://github.com/lachlanchen/chatgpt-voice-assistant/commits/main)

CLI-приложение для разговоров с моделями OpenAI Chat в режиме hands-free. Оно записывает речь, расшифровывает её через Whisper, генерирует ответы и воспроизводит синтезированную речь через выбранные провайдеры TTS.
`chatgpt-voice-assistant` держит цикл выполнения коротким, а архитектуру — модульной.

[![Руководство по установке](https://img.shields.io/badge/Install-Quick_Start-0EA5E9?style=flat-square&logo=python&logoColor=white)](#быстрый-старт)
[![Использование](https://img.shields.io/badge/Usage-gptassist-16A34A?style=flat-square&logo=terminal&logoColor=white)](#использование)
[![Архитектура](https://img.shields.io/badge/Architecture-Modular-8B5CF6?style=flat-square&logo=github&logoColor=white)](#архитектура)
[![Устранение неполадок](https://img.shields.io/badge/Troubleshoot-Common_Issues-F59E0B?style=flat-square&logo=bugatti&logoColor=white)](#устранение-неполадок)

---

## Взгляд сверху

| Слой | Назначение |
| --- | --- |
| 🎤 Голосовой цикл | Ввод с микрофона постоянно захватывается и транскрибируется. |
| 🧠 Путь модели | OpenAI completions генерирует разговорные ответы. |
| 🔉 Речевой путь | Ответ синтезируется через подключаемые провайдеры TTS. |
| 🧩 Архитектура | Контракты listener → generator → responder позволяют подменять компоненты. |

---

| Фокус | Детали |
| --- | --- |
| 🎙️ Захват голоса | Цикл взаимодействия через микрофон |
| 🧠 Ответ модели | Chat completion OpenAI |
| 🔊 Воспроизведение речи | Свободно заменяемый путь вывода TTS |
| 🧩 Структура проекта | Модули `chatgpt_voice_assistant/` вокруг явных контрактов |
| 🧩 Архитектура | Модульные контракты listener / generator / responder |

## Содержимое

- [Языковые опции](#языковые-опции)
- [Обзор](#обзор)
- [Быстрый старт](#быстрый-старт)
- [Возможности](#возможности)
- [Архитектура](#архитектура)
- [Структура проекта](#структура-проекта)
- [Требования](#требования)
- [Установка](#установка)
- [Использование](#использование)
- [Настройка](#настройка)
- [Примеры](#примеры)
- [Заметки по разработке](#заметки-по-разработке)
- [Устранение неполадок](#устранение-неполадок)
- [Интернационализация (i18n)](#интернационализация-i18n)
- [Дорожная карта](#дорожная-карта)
- [Вклад в проект](#вклад-в-проект)
- [❤️ Support](#-support)
- [Контакты](#контакты)
- [Лицензия](#лицензия)
- [Ссылки](#ссылки)

## Языковые опции

🌐 Языковые опции: русский (текущий черновик)

## Обзор

`chatgpt-voice-assistant` — это Python CLI-приложение (`gptassist`) для голосового общения с моделями OpenAI Chat.

Общий сценарий:

1. Захват звукового входа с микрофона.
2. Расшифровка речи в текст.
3. Генерация ответа моделью.
4. Преобразование ответа в речь.
5. Локальное воспроизведение сгенерированного аудио.

Проект использует `poetry`, явное внедрение зависимостей и типизированные интерфейсы, чтобы провайдеры и интеграции оставались заменяемыми.

### Цели проектирования

- Поддерживать минимальный и предсказуемый цикл выполнения.
- Делать интеграции провайдеров переключаемыми через явные интерфейсы.
- Держать CLI в режиме первого контакта с явными аргументами и fallback через переменные окружения.

### Быстрый старт

1. Установите зависимости (выберите один из вариантов ниже).
2. Задайте `OPENAI_API_KEY`.
3. Запустите `gptassist`.
4. Говорите, дождитесь этапов распознавания/генерации/воспроизведения.
5. Завершите по безопасному слову (`exit` / `--safe-word`) или сочетанием `Ctrl+C`.

## Возможности

| Область | Детали |
| --- | --- |
| 🎛️ Голосовой цикл | Разговорный цикл с управлением через wake-word + safe-word |
| 🎤 Распознавание речи | Транскрипция OpenAI Whisper (`whisper-1`) |
| 🔉 Модули текст-в-речь | `openai` (`tts-1`), `google` (`gtts`), `apple` (`say` CLI) |
| 🧭 Поведение пробуждения | Поддержка `--wake-word` |
| 🛑 Поведение выхода | Завершение через `--safe-word` (по умолчанию выход по `EXIT`) |
| 🎧 Устройства ввода | Явный выбор устройства через `--input-device-name` или интерактивный запрос |
| 🎚️ Подстройка воспроизведения | Настраиваемая скорость воспроизведения через `--speech-rate` |
| 🧩 Подстройка акцента | `--lang` и `--tld` для акцентов Google TTS |
| ✅ Контроль качества | Ruff + проверка покрытия в разработке и CI |

## Архитектура

Основной runtime-слой сделан слоистым, поэтому любую реализацию можно заменить с минимальной связанностью.

1. `main.py` связывает зависимости во время исполнения.
2. `cli_parser.py` разбирает параметры и строит runtime-конфигурацию.
3. `input_devices.py` определяет и проверяет варианты устройств захвата.
4. Конкретные реализации выбираются по режиму:
   - `whisper_listener.py` и `speech_listener.py` для STT
   - `open_ai_text_generator.py` для генерации
   - `computer_voice_responder.py` для оркестрации ответа и воспроизведения
5. `conversation.py` выполняет логику цикла (wake word + safe word, ретраи и поток ответа).

### Абстракции интерфейсов

- Абстракция слушателя: `whisper_listener.py`, `speech_listener.py`
- Абстракция генератора текста: `open_ai_text_generator.py` с `open_ai_client.py`
- Абстракция реагирования: `computer_voice_responder.py` с клиентами OpenAI, Google и Apple
- Абстракция разбора опций: централизованный парсер в `cli_parser.py`

Такое разделение делает миграцию провайдеров и мокирование в тестах предсказуемым.

## Структура проекта

```text
chatgpt-voice-assistant/
├── README.md
├── DEVELOPMENT.md
├── pyproject.toml
├── poetry.lock
├── .env.example
├── .github/
│   └── workflows/
│       ├── set-status.yml
│       └── test-application.yml
├── chatgpt_voice_assistant/
│   ├── main.py
│   ├── cli_parser.py
│   ├── conversation.py
│   ├── whisper_listener.py
│   ├── speech_listener.py
│   ├── open_ai_text_generator.py
│   ├── computer_voice_responder.py
│   ├── input_devices.py
│   ├── clients/
│   ├── bases/
│   ├── helpers/
│   ├── models/
│   └── exceptions/
└── tests/
    └── ...
```

### Ключевые компоненты потока

- `main.py`: точка входа CLI для `gptassist`.
- `cli_parser.py`: разбор параметров и валидация runtime-конфигурации.
- `conversation.py`: оркестрация цикла запись → расшифровка → генерация → речь.
- `whisper_listener.py`: основной путь слушателя на OpenAI Whisper.
- `speech_listener.py`: альтернативный путь распознавания речи.
- `open_ai_text_generator.py`: интеграция чат-комплишена и потоков prompt/message.
- `computer_voice_responder.py`: адаптер TTS и управление воспроизведением.
- `input_devices.py`: обнаружение устройств и разрешение выбора пользователя.
- `clients/`, `bases/`, `helpers/`, `models/`, `exceptions/`: адаптеры сервисов, контракты, общие утилиты, доменные сущности и явные типы ошибок.

## Требования

| Требование | Примечания |
| --- | --- |
| 🐍 Python | `>=3.9, <4.0` |
| 🔐 Ключ OpenAI API | Обязателен для генерации ответов |
| 🎙️ Микрофон / входное устройство | Нужен для захвата речи |
| 🎵 Библиотеки PortAudio | Нужны для зависимости `pyaudio` |

### Требования для macOS

Установите зависимости PortAudio:

```bash
brew install portaudio
brew link portaudio
```

Обновите конфигурацию `pydistutils` для работы с PortAudio:

```bash
echo "[build_ext]" >> $HOME/.pydistutils.cfg
echo "include_dirs="`brew --prefix portaudio`"/include/" >> $HOME/.pydistutils.cfg
echo "library_dirs="`brew --prefix portaudio`"/lib/" >> $HOME/.pydistutils.cfg
```

### Примечания для non-macOS

CI устанавливает системные пакеты для audio-зависимостей Ubuntu, но runtime-реализация может отличаться для утилит воспроизведения.

Используйте `.env.example` как локальную справку по обязательным переменным окружения.

## Установка

### Установка из PyPI

```bash
pip install chatgpt-voice-assistant
```

### Установка из исходников

1. Клонируйте репозиторий.
2. Установите Poetry ([официальная документация](https://python-poetry.org/docs/#installation) или `pip install poetry`).
3. Установите зависимости:

```bash
poetry install
```

Для зависимостей разработки:

```bash
poetry install --with dev
```

## Использование

Задайте `OPENAI_API_KEY` до запуска или передайте ключ напрямую:

```bash
export OPENAI_API_KEY=<OPEN_AI_SECRET_KEY>
gptassist

# OR
gptassist --open-ai-key=<OPEN_AI_SECRET_KEY>
```

Если вы запускаете из исходников:

```bash
poetry run gptassist --open-ai-key=<OPEN_AI_SECRET_KEY>
```

Начните говорить и дождитесь голосового ответа.

Произнесите безопасное слово (`exit` по умолчанию, настраивается через `--safe-word`) или нажмите `Ctrl+C` для остановки.

## Настройка

### Переменные окружения

| Переменная | Обязательно | Описание |
| --- | --- | --- |
| `OPENAI_API_KEY` | Да (если не используется `--open-ai-key`) | Учетные данные OpenAI, используемые STT + chat + опциональный TTS |

### Опции CLI

```text
-h, --help
    show this help message and exit

--log-level LOG_LEVEL
    Выводить логи в debug-уровне или не выводить.

--input-device-name INPUT_DEVICE_NAME
    Имя устройства ввода.

--lang LANG
    Язык распознавания речи (например: en или fr).

--max-tokens MAX_TOKENS
    Максимальное число токенов OpenAI для генерации текста.

--tld TLD
    Top level domain (например: com или com.au).

--safe-word SAFE_WORD
    Слово, произнесение которого завершает приложение.

--wake-word WAKE_WORD
    (Опционально) Слово для запуска ответа.

--open-ai-key OPEN_AI_KEY
    Обязательно. Секретный ключ Open AI (или через переменную окружения OPENAI_API_KEY)

--open-ai-model OPEN_AI_MODEL
    Модель Open AI для использования. Подробнее: https://platform.openai.com/docs/models/overview

--tts {apple,google,openai}
    Выбрать движок text-to-speech.

--speech-rate SPEECH_RATE
    Скорость воспроизведения речи. 1.0=нормально
```

> Совет: Синхронизируйте перечисленные значения по умолчанию с `cli_parser.py` в вашей текущей версии.

## Примеры

### Базовый

```bash
gptassist --open-ai-key=<OPENAI_KEY>
```

### Использовать wake word

```bash
gptassist --open-ai-key=<OPENAI_KEY> --wake-word="hey assistant"
```

### Пользовательское safe word

```bash
gptassist --open-ai-key=<OPENAI_KEY> --safe-word="goodbye"
```

### Выбрать TTS-движок

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=openai
gptassist --open-ai-key=<OPENAI_KEY> --tts=google
gptassist --open-ai-key=<OPENAI_KEY> --tts=apple
```

### Указать устройство ввода по имени

```bash
gptassist --open-ai-key=<OPENAI_KEY> --input-device-name="MacBook Pro Microphone"
```

### Настроить произношение акцента Google

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=google --lang=en --tld=com
```

Установите `LANGUAGE` / `TOP_LEVEL_DOMAIN`, чтобы изменить поведение локали по умолчанию:

| Локаль | Значение |
| --- | --- |
| Французский (France) | `LANGUAGE=fr TOP_LEVEL_DOMAIN=fr` |

Дополнительные сведения в разделе локализованных акцентов документации gTTS.

### Комбинированный пример

```bash
export OPENAI_API_KEY=sk-...
gptassist --open-ai-key=$OPENAI_API_KEY --tts=google --wake-word="hey helper" --safe-word="stop now" --speech-rate=1.1
```

## Заметки по разработке

Смотрите [DEVELOPMENT.md](./DEVELOPMENT.md) для настройки и стандартов разработки.

Часто используемые команды:

```bash
poetry install --with dev
poetry run pytest
poetry run coverage run --source chatgpt_voice_assistant -m pytest tests
poetry run coverage report --show-missing --fail-under=90
poetry run ruff format --check .
poetry run ruff check .
```

В CI (`.github/workflows/test-application.yml`) выполняются тесты, проверки Ruff и проверка покрытия.

## Устранение неполадок

### `Open AI Secret Key not specified...`

Установите `OPENAI_API_KEY` или передайте `--open-ai-key`.

### Не найдено устройств ввода / выбран неверный микрофон

- Убедитесь, что микрофон подключен и доступен в ОС.
- Используйте `--input-device-name` с точным именем устройства.
- Если имя не указано, приложение предложит выбрать его из обнаруженных устройств ввода.

### Ошибки установки/сборки `pyaudio`

- Установите PortAudio (см. требования для macOS).
- Повторно установите зависимости после настройки `.pydistutils.cfg`.

### Нет воспроизведения звука

- Текущий путь воспроизведения использует `afplay` (команда macOS).
- Apple TTS использует `say`, а OpenAI/Google генерируют аудиофайлы, которые воспроизводятся через `afplay`.
- На non-macOS-платформах может потребоваться дополнительная адаптация пути воспроизведения.

### Модель недоступна

Если модель по умолчанию или выбранная через `--open-ai-model` недоступна, задайте модель, которой вы можете пользоваться:

```bash
gptassist --open-ai-key=<OPENAI_KEY> --open-ai-model=<YOUR_AVAILABLE_MODEL>
```

### Ошибки API / сети

- Проверьте подключение и права ключа API.
- Убедитесь, что ключ валиден для выбранного провайдера/модели.
- Увеличьте уровень логов с `--log-level DEBUG`, чтобы проследить поток парсинга и выполнения.

## Интернационализация (i18n)

- `i18n/` содержит локализованные README.
- Английский вариант в корне остаётся канонической базой.
- В каждой локализованной версии держите ровно одну строку `Language options:` в верхней части и не дублируйте её.
- Переводы группируются по языку и локали в соответствии с потоком документации репозитория.

## Дорожная карта

- Расширить документацию по кроссплатформенному воспроизведению звука.
- Добавить/обновить многоязычные версии README в `i18n/`.
- Поддерживать актуальность значений по умолчанию и примеров для рекомендованных на данный момент моделей OpenAI.
- Улучшить архитектурную и эксплуатационную документацию поверх базовых материалов README/DEVELOPMENT.

## Вклад в проект

1. Сделайте форк репозитория.
2. Создайте feature branch.
3. Запустите тесты и проверки локально:

```bash
poetry run pytest
poetry run ruff format --check .
poetry run ruff check .
```

4. Откройте pull request с понятным описанием изменений и покрытием тестами.

## Контакты

- Сообщите об ошибках или предложите новые функции: [GitHub Issues](https://github.com/lachlanchen/chatgpt-voice-assistant/issues)
- Задавайте вопросы по дизайну или архитектуре: [GitHub Discussions](https://github.com/lachlanchen/chatgpt-voice-assistant/discussions)

## Лицензия

В текущий момент в репозитории отсутствует файл `LICENSE`.

Предположение: юридические условия использования пока не заданы явно. Добавьте `LICENSE`, чтобы чётко зафиксировать условия использования и распространения.

## Ссылки

- [Speech Recognition library docs](https://pypi.org/project/SpeechRecognition/1.2.3)
- [Google Translate Text-to-Speech API (gTTS)](https://gtts.readthedocs.io/en/latest/module.html#)


## ❤️ Support

| Donate | PayPal | Stripe |
| --- | --- | --- |
| [![Donate](https://camo.githubusercontent.com/24a4914f0b42c6f435f9e101621f1e52535b02c225764b2f6cc99416926004b7/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f446f6e6174652d4c617a79696e674172742d3045413545393f7374796c653d666f722d7468652d6261646765266c6f676f3d6b6f2d6669266c6f676f436f6c6f723d7768697465)](https://chat.lazying.art/donate) | [![PayPal](https://camo.githubusercontent.com/d0f57e8b016517a4b06961b24d0ca87d62fdba16e18bbdb6aba28e978dc0ea21/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f50617950616c2d526f6e677a686f754368656e2d3030343537433f7374796c653d666f722d7468652d6261646765266c6f676f3d70617970616c266c6f676f436f6c6f723d7768697465)](https://paypal.me/RongzhouChen) | [![Stripe](https://camo.githubusercontent.com/1152dfe04b6943afe3a8d2953676749603fb9f95e24088c92c97a01a897b4942/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f5374726970652d446f6e6174652d3633354246463f7374796c653d666f722d7468652d6261646765266c6f676f3d737472697065266c6f676f436f6c6f723d7768697465)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400) |
