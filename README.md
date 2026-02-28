[English](README.md) · [العربية](i18n/README.ar.md) · [Español](i18n/README.es.md) · [Français](i18n/README.fr.md) · [日本語](i18n/README.ja.md) · [한국어](i18n/README.ko.md) · [Tiếng Việt](i18n/README.vi.md) · [中文 (简体)](i18n/README.zh-Hans.md) · [中文（繁體）](i18n/README.zh-Hant.md) · [Deutsch](i18n/README.de.md) · [Русский](i18n/README.ru.md)


[![LazyingArt banner](https://github.com/lachlanchen/lachlanchen/raw/main/figs/banner.png)](https://github.com/lachlanchen/lachlanchen/blob/main/figs/banner.png)

# ChatGPT Voice Assistant

| Component | Status |
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

A CLI application for hands-free conversations with OpenAI chat models. It records speech, transcribes via Whisper, generates replies, and plays synthesized responses through selectable TTS providers.
`chatgpt-voice-assistant` keeps the runtime loop minimal and the architecture modular.

[![Install guide](https://img.shields.io/badge/Install-Quick_Start-0EA5E9?style=flat-square&logo=python&logoColor=white)](#installation)
[![Usage demo](https://img.shields.io/badge/Usage-gptassist-16A34A?style=flat-square&logo=terminal&logoColor=white)](#usage)
[![Architecture](https://img.shields.io/badge/Architecture-Modular-8B5CF6?style=flat-square&logo=github&logoColor=white)](#architecture)
[![Troubleshoot](https://img.shields.io/badge/Troubleshoot-Common_Issues-F59E0B?style=flat-square&logo=bugatti&logoColor=white)](#troubleshooting)

---

## At a glance

| Layer | Purpose |
| --- | --- |
| 🎤 Voice loop | Microphone input is captured and transcribed continuously. |
| 🧠 Model path | OpenAI completions generate conversational responses. |
| 🔉 Voice path | Response is synthesized through pluggable TTS providers. |
| 🧩 Architecture | Listener → generator → responder contracts keep components swappable. |

---

| Focus | Details |
| --- | --- |
| 🎙️ Voice capture | Microphone-driven interaction loop |
| 🧠 Model output | OpenAI chat completion responses |
| 🔊 Speech playback | Swappable TTS output path |
| 🧩 Project structure | `chatgpt_voice_assistant/` modules around explicit contracts |
| 🧩 Architecture | Modular listener / generator / responder contracts |

## Table of Contents

- [Language options](#language-options)
- [Overview](#overview)
- [Quick start](#quick-start)
- [Features](#features)
- [Architecture](#architecture)
- [Project structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
- [Configuration](#configuration)
- [Examples](#examples)
- [Development notes](#development-notes)
- [Troubleshooting](#troubleshooting)
- [Internationalization (i18n)](#internationalization-i18n)
- [Roadmap](#roadmap)
- [Contributing](#contributing)
- [❤️ Support](#-support)
- [Contact](#contact)
- [License](#license)
- [References](#references)

## Language options

🌐 Language options: English (current draft)

## Overview

`chatgpt-voice-assistant` is a Python CLI application (`gptassist`) for hands-free conversations with OpenAI chat models.

High-level flow:

1. Capture microphone input.
2. Transcribe speech to text.
3. Generate a model response.
4. Convert response text to speech.
5. Play the generated audio locally.

The project uses `poetry`, explicit dependency injection, and typed interfaces to keep voice providers and integrations replaceable.

### Design intent

- Keep the runtime loop minimal and predictable.
- Keep provider integrations swap-able via explicit interfaces.
- Keep CLI-first interaction with explicit arguments and environment-based fallback.

### Quick start

1. Install dependencies (choose one method below).
2. Set `OPENAI_API_KEY`.
3. Run `gptassist`.
4. Speak, and wait for transcription/generation/playback.
5. End with safe word (`exit` / `--safe-word`) or `Ctrl+C`.

## Features

| Area | Details |
| --- | --- |
| 🎛️ Voice loop | Voice-driven conversation loop with wake-word + safe-word control |
| 🎤 Speech-to-text | OpenAI Whisper transcription (`whisper-1`) |
| 🔉 Text-to-speech engines | `openai` (`tts-1`), `google` (`gtts`), `apple` (`say` CLI) |
| 🧭 Wake behavior | `--wake-word` support |
| 🛑 Exit behavior | Safe-word termination via `--safe-word` (default behavior exits on `EXIT`) |
| 🎧 Input devices | Explicit device selection via `--input-device-name` or interactive prompt |
| 🎚️ Playback tuning | Configurable playback rate via `--speech-rate` |
| 🧩 Accent tuning | `--lang` and `--tld` for Google TTS accents |
| ✅ Quality tooling | Ruff + coverage checks in development and CI |

## Architecture

The core runtime is intentionally layered so each implementation can be replaced with minimal coupling.

1. `main.py` wires runtime dependencies.
2. `cli_parser.py` parses options and builds runtime configuration.
3. `input_devices.py` resolves and validates capture device choices.
4. Concrete implementations are selected by mode:
   - `whisper_listener.py` and `speech_listener.py` for STT
   - `open_ai_text_generator.py` for generation
   - `computer_voice_responder.py` for response orchestration and playback
5. `conversation.py` runs loop logic, including wake word + safe word, retries, and response flow.

### Interface abstractions

- Listener abstraction: `whisper_listener.py`, `speech_listener.py`
- Text generator abstraction: `open_ai_text_generator.py` with `open_ai_client.py`
- Responder abstraction: `computer_voice_responder.py` with clients for OpenAI, Google, and Apple
- Option parsing abstraction: centralized parser in `cli_parser.py`

This separation keeps provider migration and test mocking straightforward.

## Project Structure

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

### Core flow components

- `main.py`: CLI entrypoint for `gptassist`.
- `cli_parser.py`: argument parsing and validation for runtime configuration.
- `conversation.py`: orchestration for record → transcribe → generate → speak loop.
- `whisper_listener.py`: primary listener path using OpenAI Whisper.
- `speech_listener.py`: alternate speech path.
- `open_ai_text_generator.py`: chat completion integration and prompt/message flow.
- `computer_voice_responder.py`: TTS adapter and playback orchestration.
- `input_devices.py`: discovers devices and resolves user selection.
- `clients/`, `bases/`, `helpers/`, `models/`, `exceptions/`: service adapters, contracts, shared helpers, domain entities, and explicit error types.

## Prerequisites

| Requirement | Notes |
| --- | --- |
| 🐍 Python | `>=3.9, <4.0` |
| 🔐 OpenAI API key | Required to generate responses |
| 🎙️ Microphone / input device | Required for speech capture |
| 🎵 PortAudio libs | Required for `pyaudio` dependency |

### macOS prerequisites

Install PortAudio dependencies:

```bash
brew install portaudio
brew link portaudio
```

Update your `pydistutils` config for PortAudio usage:

```bash
echo "[build_ext]" >> $HOME/.pydistutils.cfg
echo "include_dirs="`brew --prefix portaudio`"/include/" >> $HOME/.pydistutils.cfg
echo "library_dirs="`brew --prefix portaudio`"/lib/" >> $HOME/.pydistutils.cfg
```

### Non-macOS notes

CI installs Ubuntu-compatible system packages for audio dependencies, but runtime behavior may differ for playback utilities.

Use `.env.example` as a local reference for required environment inputs.

## Installation

### Install from PyPI

```bash
pip install chatgpt-voice-assistant
```

### Install from source

1. Clone this repository.
2. Install Poetry ([official docs](https://python-poetry.org/docs/#installation) or `pip install poetry`).
3. Install dependencies:

```bash
poetry install
```

For development dependencies:

```bash
poetry install --with dev
```

## Usage

Set `OPENAI_API_KEY` before launching, or pass your key directly:

```bash
export OPENAI_API_KEY=<OPEN_AI_SECRET_KEY>
gptassist

# OR
gptassist --open-ai-key=<OPEN_AI_SECRET_KEY>
```

If running from source:

```bash
poetry run gptassist --open-ai-key=<OPEN_AI_SECRET_KEY>
```

Start speaking and listen for spoken replies.

Say your safe word (`exit` by default, configurable with `--safe-word`) or press `Ctrl+C` to stop.

## Configuration

### Environment variables

| Variable | Required | Description |
| --- | --- | --- |
| `OPENAI_API_KEY` | Yes (unless `--open-ai-key` is used) | OpenAI credential used by STT + chat + optional TTS |

### CLI options

```text
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

> Tip: Keep the documented defaults in sync with `cli_parser.py` in your current checkout.

## Examples

### Basic

```bash
gptassist --open-ai-key=<OPENAI_KEY>
```

### Use a wake word

```bash
gptassist --open-ai-key=<OPENAI_KEY> --wake-word="hey assistant"
```

### Custom safe word

```bash
gptassist --open-ai-key=<OPENAI_KEY> --safe-word="goodbye"
```

### Select TTS engine

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=openai
gptassist --open-ai-key=<OPENAI_KEY> --tts=google
gptassist --open-ai-key=<OPENAI_KEY> --tts=apple
```

### Specify input device by name

```bash
gptassist --open-ai-key=<OPENAI_KEY> --input-device-name="MacBook Pro Microphone"
```

### Configure Google accent output

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=google --lang=en --tld=com
```

Set `LANGUAGE` / `TOP_LEVEL_DOMAIN` to change default locale behavior:

| Locale | Value |
| --- | --- |
| French (France) | `LANGUAGE=fr TOP_LEVEL_DOMAIN=fr` |

See the localized accents section in gTTS docs for more details.

### Combined example

```bash
export OPENAI_API_KEY=sk-...
gptassist --open-ai-key=$OPENAI_API_KEY --tts=google --wake-word="hey helper" --safe-word="stop now" --speech-rate=1.1
```

## Development Notes

See [DEVELOPMENT.md](./DEVELOPMENT.md) for setup and coding standards.

Common commands:

```bash
poetry install --with dev
poetry run pytest
poetry run coverage run --source chatgpt_voice_assistant -m pytest tests
poetry run coverage report --show-missing --fail-under=90
poetry run ruff format --check .
poetry run ruff check .
```

The CI workflow `.github/workflows/test-application.yml` runs tests, Ruff checks, and coverage.

## Troubleshooting

### `Open AI Secret Key not specified...`

Set `OPENAI_API_KEY` or pass `--open-ai-key`.

### No input devices found / wrong microphone selected

- Ensure your microphone is connected and available to the OS.
- Use `--input-device-name` with an exact device name.
- If not provided, the app will prompt you to choose from discovered input devices.

### `pyaudio` install/build issues

- Install PortAudio (see macOS prerequisites).
- Re-run dependency installation after setting `.pydistutils.cfg`.

### No sound playback

- Current playback path uses `afplay` (macOS command).
- Apple TTS uses `say` and OpenAI/Google generate audio files played through `afplay`.
- On non-macOS platforms, additional playback adaptation may be required.

### Model not available

If the default or selected `--open-ai-model` is unavailable, pass a model you can access:

```bash
gptassist --open-ai-key=<OPENAI_KEY> --open-ai-model=<YOUR_AVAILABLE_MODEL>
```

### API / network failures

- Check connectivity and API key permissions.
- Verify the key is valid for the selected provider/model.
- Increase logging with `--log-level DEBUG` when tracing parser/runtime flow.

## Internationalization (i18n)

- `i18n/` contains localized READMEs.
- This English draft is the canonical base.
- Keep exactly one `Language options:` line near the top of each README variant and avoid duplicates.
- Translations are grouped by language and locale to match repository docs flow.

## Roadmap

- Expand documented cross-platform audio playback behavior.
- Add or update multilingual README variants under `i18n/`.
- Keep defaults and examples aligned with currently recommended OpenAI model names.
- Improve architecture and operational docs beyond README/DEVELOPMENT basics.

## Contributing

1. Fork the repository.
2. Create a feature branch.
3. Run tests and checks locally:

```bash
poetry run pytest
poetry run ruff format --check .
poetry run ruff check .
```

4. Open a pull request with a clear description of changes and test coverage.

## ❤️ Support

| Donate | PayPal | Stripe |
| --- | --- | --- |
| [![Donate](https://camo.githubusercontent.com/24a4914f0b42c6f435f9e101621f1e52535b02c225764b2f6cc99416926004b7/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f446f6e6174652d4c617a79696e674172742d3045413545393f7374796c653d666f722d7468652d6261646765266c6f676f3d6b6f2d6669266c6f676f436f6c6f723d7768697465)](https://chat.lazying.art/donate) | [![PayPal](https://camo.githubusercontent.com/d0f57e8b016517a4b06961b24d0ca87d62fdba16e18bbdb6aba28e978dc0ea21/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f50617950616c2d526f6e677a686f754368656e2d3030343537433f7374796c653d666f722d7468652d6261646765266c6f676f3d70617970616c266c6f676f436f6c6f723d7768697465)](https://paypal.me/RongzhouChen) | [![Stripe](https://camo.githubusercontent.com/1152dfe04b6943afe3a8d2953676749603fb9f95e24088c92c97a01a897b4942/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f5374726970652d446f6e6174652d3633354246463f7374796c653d666f722d7468652d6261646765266c6f676f3d737472697065266c6f676f436f6c6f723d7768697465)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400) |

## Contact

- Report bugs or request features: [GitHub Issues](https://github.com/lachlanchen/chatgpt-voice-assistant/issues)
- Ask design or architecture questions: [GitHub Discussions](https://github.com/lachlanchen/chatgpt-voice-assistant/discussions)

## License

No `LICENSE` file is currently present in this repository.

Assumption: licensing terms are not explicitly declared yet. Add a `LICENSE` file to clarify usage and distribution terms.

## References

- [Speech Recognition library docs](https://pypi.org/project/SpeechRecognition/1.2.3)
- [Google Translate Text-to-Speech API (gTTS)](https://gtts.readthedocs.io/en/latest/module.html#)
