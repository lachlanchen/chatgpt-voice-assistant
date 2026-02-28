[English](README.md) · [العربية](i18n/README.ar.md) · [Español](i18n/README.es.md) · [Français](i18n/README.fr.md) · [日本語](i18n/README.ja.md) · [한국어](i18n/README.ko.md) · [Tiếng Việt](i18n/README.vi.md) · [中文 (简体)](i18n/README.zh-Hans.md) · [中文（繁體）](i18n/README.zh-Hant.md) · [Deutsch](i18n/README.de.md) · [Русский](i18n/README.ru.md)

# ChatGPT Voice Assistant

Language options: English (current draft)

![GitHub Actions Build Status](https://github.com/jakecyr/openai-gpt3-chatbot/actions/workflows/test-application.yml/badge.svg)
![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?logo=python&logoColor=white)
![Poetry](https://img.shields.io/badge/Poetry-managed-60A5FA?logo=poetry&logoColor=white)
![Platform](https://img.shields.io/badge/Platform-macOS%20focused-0F172A)
![CLI](https://img.shields.io/badge/Interface-CLI-16A34A)

A simple interface to the OpenAI ChatGPT model with speech to text for input and text to speech for output.
`chatgpt-voice-assistant` uses OpenAI Whisper for speech transcription and supports OpenAI, Google, or Apple text-to-speech output.

## Overview

`chatgpt-voice-assistant` is a Python CLI application (`gptassist`) for voice conversations with OpenAI models.

High-level flow:

1. Capture microphone input.
2. Transcribe speech to text.
3. Generate a model response.
4. Convert response text to speech.
5. Play the generated audio locally.

This project is Poetry-based, typed, and covered by unit tests in `tests/`.

## Features

| Area | Details |
| --- | --- |
| Voice loop | Voice-driven CLI conversation loop |
| Speech-to-text | OpenAI Whisper transcription (`whisper-1`) |
| Text-to-speech engines | `openai` (OpenAI TTS), `google` (gTTS), `apple` (`say` CLI) |
| Wake behavior | Wake-word support via `--wake-word` |
| Exit behavior | Safe-word termination via `--safe-word` (default behavior exits on `EXIT`) |
| Input devices | Explicit selection with `--input-device-name` or interactive prompt fallback |
| Playback tuning | Configurable speech playback rate via `--speech-rate` |
| Accent tuning | Configurable language/TLD for Google TTS accents |

## Project Structure

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

## Prerequisites

| Requirement | Notes |
| --- | --- |
| Python | `>=3.9, <4.0` |
| OpenAI API key | Required to generate responses |
| Microphone/input audio device | Needed for speech capture |
| PortAudio libs | Required for `pyaudio` dependency |

### Mac Prerequisites

Install dependencies:

```bash
brew install portaudio
brew link portaudio
```

Update your `pydistutils` config file for PortAudio usage by running:

```bash
echo "[build_ext]" >> $HOME/.pydistutils.cfg
echo "include_dirs="`brew --prefix portaudio`"/include/" >> $HOME/.pydistutils.cfg
echo "library_dirs="`brew --prefix portaudio`"/lib/" >> $HOME/.pydistutils.cfg
```

## Installation

### Install from PyPI

Run the following to install the `gptassist` CLI application:

```bash
pip install chatgpt-voice-assistant
```

### Install from Source

1. Install Poetry ([official docs](https://python-poetry.org/docs/#installation) or with `pip install poetry`)
2. Install dependencies:

```bash
poetry install
```

For development dependencies:

```bash
poetry install --with dev
```

## Usage

Either set `OPENAI_API_KEY` before running, or pass your secret key directly:

```bash
export OPENAI_API_KEY=<OPEN API SECRET KEY HERE>
gptassist

# OR

gptassist --open-ai-key=<OPEN API SECRET KEY HERE>
```

If installed from source with Poetry:

```bash
poetry run gptassist --open-ai-key=<OPEN API SECRET KEY HERE>
```

Start speaking and turn up your volume to hear the AI assistant respond.

Say the word `exit` (or your configured safe word) or hit `Ctrl+C` in your terminal to stop the application.

## Configuration

### Environment Variables

| Variable | Required | Description |
| --- | --- | --- |
| `OPENAI_API_KEY` | Yes (unless overridden) | Required unless `--open-ai-key` is provided |

### CLI Options

Below is the help menu from the `gptassist` CLI detailing available options:

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

## Examples

### Basic

```bash
gptassist --open-ai-key=<OPENAI_KEY>
```

### Use a Wake Word

```bash
gptassist --open-ai-key=<OPENAI_KEY> --wake-word="hey assistant"
```

### Custom Safe Word

```bash
gptassist --open-ai-key=<OPENAI_KEY> --safe-word="goodbye"
```

### Select TTS Engine

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=openai
gptassist --open-ai-key=<OPENAI_KEY> --tts=google
gptassist --open-ai-key=<OPENAI_KEY> --tts=apple
```

### Specify Input Device by Name

```bash
gptassist --open-ai-key=<OPENAI_KEY> --input-device-name="MacBook Pro Microphone"
```

### Specify an Output Language Accent (Google TTS)

Specify both `LANGUAGE` and `TOP_LEVEL_DOMAIN` vars to override default English (United States):

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=google --lang=en --tld=com
```

#### Language Examples

| Locale | Value |
| --- | --- |
| French (France) | `LANGUAGE=fr TOP_LEVEL_DOMAIN=fr` |

See Localized accents section in gTTS docs for more information.

## Development Notes

See [DEVELOPMENT.md](./DEVELOPMENT.md) for the base development setup.

Common development commands:

```bash
poetry install --with dev
poetry run pytest
poetry run coverage run --source chatgpt_voice_assistant -m pytest tests
poetry run coverage report --show-missing --fail-under=90
poetry run ruff format --check .
poetry run ruff check .
```

CI workflow (`.github/workflows/test-application.yml`) runs tests, Ruff checks, and coverage.

## Troubleshooting

### `Open AI Secret Key not specified...`

Set `OPENAI_API_KEY` or pass `--open-ai-key`.

### No input devices found / wrong microphone selected

- Ensure your microphone is connected and available to the OS.
- Use `--input-device-name` with an exact device name.
- If not provided, the app will prompt you to choose from discovered input devices.

### `pyaudio` install/build issues

- Ensure PortAudio is installed (see Mac prerequisite section).
- Re-run dependency installation after setting `.pydistutils.cfg`.

### No sound playback

- Current playback path uses `afplay` (macOS command).
- Apple TTS uses `say` and OpenAI/Google produce audio files played through `afplay`.
- If running on non-macOS, additional playback adaptation may be required.

### Model not available

If the default or specified `--open-ai-model` is unavailable for your account, pass a model you can access:

```bash
gptassist --open-ai-key=<OPENAI_KEY> --open-ai-model=<YOUR_AVAILABLE_MODEL>
```

## Internationalization (i18n)

- `i18n/` exists for multilingual README variants.
- This draft is intentionally English-only and remains the canonical base.
- Future language files should be added one-by-one and kept in sync with this canonical README content.
- Keep exactly one `Language options:` line at the top of each README variant and avoid duplicate lines.

## Roadmap

- Expand documented cross-platform audio playback behavior.
- Add generated/maintained multilingual README variants under `i18n/`.
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

## License

No `LICENSE` file is currently present in this repository.

Assumption: licensing terms are not explicitly declared yet. Add a `LICENSE` file to make usage and distribution terms clear.

## References

- [Speech Recognition library docs](https://pypi.org/project/SpeechRecognition/1.2.3)
- [Google Translate Text-to-Speech API (gTTS)](https://gtts.readthedocs.io/en/latest/module.html#)
