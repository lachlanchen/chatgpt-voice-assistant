[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


# ChatGPT Voice Assistant

![GitHub Actions Build Status](https://github.com/jakecyr/openai-gpt3-chatbot/actions/workflows/test-application.yml/badge.svg)
![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?logo=python&logoColor=white)
![Poetry](https://img.shields.io/badge/Poetry-managed-60A5FA?logo=poetry&logoColor=white)
![Platform](https://img.shields.io/badge/Platform-macOS%20focused-0F172A)
![CLI](https://img.shields.io/badge/Interface-CLI-16A34A)

Простой интерфейс к модели OpenAI ChatGPT со Speech-to-Text для ввода и Text-to-Speech для вывода.
`chatgpt-voice-assistant` использует OpenAI Whisper для распознавания речи и поддерживает вывод через OpenAI, Google или Apple Text-to-Speech.

## Обзор

`chatgpt-voice-assistant` — это Python CLI-приложение (`gptassist`) для голосовых диалогов с моделями OpenAI.

Общий сценарий работы:

1. Захват ввода с микрофона.
2. Транскрибация речи в текст.
3. Генерация ответа модели.
4. Преобразование текста ответа в речь.
5. Воспроизведение сгенерированного аудио локально.

Проект использует Poetry, имеет типизацию и покрыт unit-тестами в `tests/`.

## Возможности

| Область | Подробности |
| --- | --- |
| Голосовой цикл | Цикл диалога в CLI, управляемый голосом |
| Speech-to-text | Транскрибация OpenAI Whisper (`whisper-1`) |
| Движки Text-to-Speech | `openai` (OpenAI TTS), `google` (gTTS), `apple` (`say` CLI) |
| Поведение пробуждения | Поддержка wake-word через `--wake-word` |
| Поведение завершения | Завершение по safe-word через `--safe-word` (по умолчанию выход при `EXIT`) |
| Устройства ввода | Явный выбор через `--input-device-name` или интерактивный fallback-подбор |
| Настройка воспроизведения | Настраиваемая скорость воспроизведения речи через `--speech-rate` |
| Настройка акцента | Настраиваемые language/TLD для акцентов Google TTS |

## Структура проекта

```text
chatgpt-voice-assistant/
├── README.md
├── DEVELOPMENT.md
├── pyproject.toml
├── poetry.lock
├── .env.example
├── .github/workflows/
├── chatgpt_voice_assistant/
│   ├── main.py
│   ├── cli_parser.py
│   ├── conversation.py
│   ├── whisper_listener.py
│   ├── open_ai_text_generator.py
│   ├── computer_voice_responder.py
│   ├── input_devices.py
│   ├── clients/
│   ├── bases/
│   ├── helpers/
│   ├── models/
│   └── exceptions/
├── tests/
└── i18n/
```

## Требования

| Требование | Примечания |
| --- | --- |
| Python | `>=3.9, <4.0` |
| OpenAI API key | Обязателен для генерации ответов |
| Microphone/input audio device | Нужен для захвата речи |
| PortAudio libs | Требуются для зависимости `pyaudio` |

### Требования для Mac

Установите зависимости:

```bash
brew install portaudio
brew link portaudio
```

Обновите файл конфигурации `pydistutils` для использования PortAudio, выполнив:

```bash
echo "[build_ext]" >> $HOME/.pydistutils.cfg
echo "include_dirs="`brew --prefix portaudio`"/include/" >> $HOME/.pydistutils.cfg
echo "library_dirs="`brew --prefix portaudio`"/lib/" >> $HOME/.pydistutils.cfg
```

## Установка

### Установка из PyPI

Выполните следующую команду, чтобы установить CLI-приложение `gptassist`:

```bash
pip install chatgpt-voice-assistant
```

### Установка из исходников

1. Установите Poetry ([официальная документация](https://python-poetry.org/docs/#installation) или через `pip install poetry`)
2. Установите зависимости:

```bash
poetry install
```

Для зависимостей разработки:

```bash
poetry install --with dev
```

## Использование

Либо задайте `OPENAI_API_KEY` перед запуском, либо передайте секретный ключ напрямую:

```bash
export OPENAI_API_KEY=<OPEN API SECRET KEY HERE>
gptassist

# OR

gptassist --open-ai-key=<OPEN API SECRET KEY HERE>
```

Если установлено из исходников с Poetry:

```bash
poetry run gptassist --open-ai-key=<OPEN API SECRET KEY HERE>
```

Начните говорить и увеличьте громкость, чтобы услышать ответ AI-ассистента.

Скажите `exit` (или ваше настроенное safe-word) либо нажмите `Ctrl+C` в терминале, чтобы остановить приложение.

## Конфигурация

### Переменные окружения

| Variable | Required | Description |
| --- | --- | --- |
| `OPENAI_API_KEY` | Yes (unless overridden) | Required unless `--open-ai-key` is provided |

### Параметры CLI

Ниже приведено меню помощи CLI `gptassist` с доступными параметрами:

```txt
-h, --help
    show this help message and exit

--log-level LOG_LEVEL
    Whether to print at the debug level or not.

--input-device-name INPUT_DEVICE_NAME
    The input device name.

--lang LANG
    The language to listen for when running speech to text (ex. en or fr).

--max-tokens MAX_TOKENS
    Max OpenAI completion tokens to use for text generation.

--tld TLD
    Top level domain (ex. com or com.au).

--safe-word SAFE_WORD
    Word to speak to exit the application.

--wake-word WAKE_WORD
    (Optional) Word to trigger a response.

--open-ai-key OPEN_AI_KEY
    Required. Open AI Secret Key (or set OPENAI_API_KEY environment variable)

--open-ai-model OPEN_AI_MODEL
    The Open AI model to use. See: https://platform.openai.com/docs/models/overview

--tts {apple,google,openai}
    Choose a text-to-speech engine.

--speech-rate SPEECH_RATE
    The rate at which to play speech. 1.0=normal
```

## Примеры

### Базовый

```bash
gptassist --open-ai-key=<OPENAI_KEY>
```

### Использование Wake Word

```bash
gptassist --open-ai-key=<OPENAI_KEY> --wake-word="hey assistant"
```

### Пользовательское Safe Word

```bash
gptassist --open-ai-key=<OPENAI_KEY> --safe-word="goodbye"
```

### Выбор движка TTS

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=openai
gptassist --open-ai-key=<OPENAI_KEY> --tts=google
gptassist --open-ai-key=<OPENAI_KEY> --tts=apple
```

### Указание устройства ввода по имени

```bash
gptassist --open-ai-key=<OPENAI_KEY> --input-device-name="MacBook Pro Microphone"
```

### Указание акцента языка вывода (Google TTS)

Укажите переменные `LANGUAGE` и `TOP_LEVEL_DOMAIN`, чтобы переопределить английский по умолчанию (США):

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=google --lang=en --tld=com
```

#### Примеры языков

| Locale | Value |
| --- | --- |
| French (France) | `LANGUAGE=fr TOP_LEVEL_DOMAIN=fr` |

Дополнительную информацию смотрите в разделе Localized accents документации gTTS.

## Примечания по разработке

См. [DEVELOPMENT.md](./DEVELOPMENT.md) для базовой настройки среды разработки.

Часто используемые команды разработки:

```bash
poetry install --with dev
poetry run pytest
poetry run coverage run --source chatgpt_voice_assistant -m pytest tests
poetry run coverage report --show-missing --fail-under=90
poetry run ruff format --check .
poetry run ruff check .
```

CI workflow (`.github/workflows/test-application.yml`) запускает тесты, проверки Ruff и coverage.
