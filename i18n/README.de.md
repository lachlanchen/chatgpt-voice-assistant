[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


# ChatGPT Voice Assistant

![GitHub Actions Build Status](https://github.com/jakecyr/openai-gpt3-chatbot/actions/workflows/test-application.yml/badge.svg)
![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?logo=python&logoColor=white)
![Poetry](https://img.shields.io/badge/Poetry-managed-60A5FA?logo=poetry&logoColor=white)
![Platform](https://img.shields.io/badge/Platform-macOS%20focused-0F172A)
![CLI](https://img.shields.io/badge/Interface-CLI-16A34A)

Eine einfache Oberfläche für das OpenAI-ChatGPT-Modell mit Speech-to-Text für die Eingabe und Text-to-Speech für die Ausgabe.
`chatgpt-voice-assistant` verwendet OpenAI Whisper für die Sprachtranskription und unterstützt OpenAI-, Google- oder Apple-Text-to-Speech-Ausgabe.

## Überblick

`chatgpt-voice-assistant` ist eine Python-CLI-Anwendung (`gptassist`) für Sprachkonversationen mit OpenAI-Modellen.

Ablauf auf hoher Ebene:

1. Mikrofoneingabe erfassen.
2. Sprache in Text transkribieren.
3. Eine Modellantwort erzeugen.
4. Antworttext in Sprache umwandeln.
5. Das erzeugte Audio lokal abspielen.

Dieses Projekt basiert auf Poetry, ist typisiert und durch Unit-Tests in `tests/` abgedeckt.

## Funktionen

| Bereich | Details |
| --- | --- |
| Sprachschleife | Sprachgesteuerte CLI-Konversationsschleife |
| Speech-to-Text | OpenAI-Whisper-Transkription (`whisper-1`) |
| Text-to-Speech-Engines | `openai` (OpenAI TTS), `google` (gTTS), `apple` (`say` CLI) |
| Aktivierungsverhalten | Wake-Word-Unterstützung über `--wake-word` |
| Beenden | Beenden per Safe-Word über `--safe-word` (Standardverhalten beendet bei `EXIT`) |
| Eingabegeräte | Explizite Auswahl mit `--input-device-name` oder interaktiver Prompt als Fallback |
| Wiedergabe-Tuning | Konfigurierbare Sprachwiedergabegeschwindigkeit über `--speech-rate` |
| Akzent-Tuning | Konfigurierbare Sprache/TLD für Google-TTS-Akzente |

## Projektstruktur

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

## Voraussetzungen

| Anforderung | Hinweise |
| --- | --- |
| Python | `>=3.9, <4.0` |
| OpenAI API key | Erforderlich zum Generieren von Antworten |
| Microphone/input audio device | Für die Sprachaufnahme erforderlich |
| PortAudio libs | Für die Abhängigkeit `pyaudio` erforderlich |

### Mac-Voraussetzungen

Abhängigkeiten installieren:

```bash
brew install portaudio
brew link portaudio
```

Aktualisieren Sie Ihre `pydistutils`-Konfigurationsdatei für die Verwendung von PortAudio mit:

```bash
echo "[build_ext]" >> $HOME/.pydistutils.cfg
echo "include_dirs="`brew --prefix portaudio`"/include/" >> $HOME/.pydistutils.cfg
echo "library_dirs="`brew --prefix portaudio`"/lib/" >> $HOME/.pydistutils.cfg
```

## Installation

### Von PyPI installieren

Führen Sie Folgendes aus, um die CLI-Anwendung `gptassist` zu installieren:

```bash
pip install chatgpt-voice-assistant
```

### Aus dem Quellcode installieren

1. Installieren Sie Poetry ([offizielle Doku](https://python-poetry.org/docs/#installation) oder mit `pip install poetry`)
2. Abhängigkeiten installieren:

```bash
poetry install
```

Für Entwicklungsabhängigkeiten:

```bash
poetry install --with dev
```

## Verwendung

Setzen Sie entweder `OPENAI_API_KEY` vor dem Ausführen oder übergeben Sie Ihren geheimen Schlüssel direkt:

```bash
export OPENAI_API_KEY=<OPEN API SECRET KEY HERE>
gptassist

# OR

gptassist --open-ai-key=<OPEN API SECRET KEY HERE>
```

Wenn aus dem Quellcode mit Poetry installiert:

```bash
poetry run gptassist --open-ai-key=<OPEN API SECRET KEY HERE>
```

Beginnen Sie zu sprechen und erhöhen Sie Ihre Lautstärke, um die Antwort des KI-Assistenten zu hören.

Sagen Sie `exit` (oder Ihr konfiguriertes Safe-Word) oder drücken Sie `Ctrl+C` im Terminal, um die Anwendung zu beenden.

## Konfiguration

### Umgebungsvariablen

| Variable | Erforderlich | Beschreibung |
| --- | --- | --- |
| `OPENAI_API_KEY` | Ja (außer überschrieben) | Erforderlich, sofern `--open-ai-key` nicht angegeben ist |

### CLI-Optionen

Nachfolgend das Hilfemenü der `gptassist`-CLI mit den verfügbaren Optionen:

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

## Beispiele

### Basis

```bash
gptassist --open-ai-key=<OPENAI_KEY>
```

### Wake Word verwenden

```bash
gptassist --open-ai-key=<OPENAI_KEY> --wake-word="hey assistant"
```

### Benutzerdefiniertes Safe-Word

```bash
gptassist --open-ai-key=<OPENAI_KEY> --safe-word="goodbye"
```

### TTS-Engine auswählen

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=openai
gptassist --open-ai-key=<OPENAI_KEY> --tts=google
gptassist --open-ai-key=<OPENAI_KEY> --tts=apple
```

### Eingabegerät nach Namen angeben

```bash
gptassist --open-ai-key=<OPENAI_KEY> --input-device-name="MacBook Pro Microphone"
```

### Ausgabesprachakzent angeben (Google TTS)

Geben Sie sowohl `LANGUAGE`- als auch `TOP_LEVEL_DOMAIN`-Variablen an, um das Standard-Englisch (Vereinigte Staaten) zu überschreiben:

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=google --lang=en --tld=com
```

#### Sprachbeispiele

| Gebietsschema | Wert |
| --- | --- |
| Englisch (Vereinigte Staaten) _STANDARD_ | `LANGUAGE=en TOP_LEVEL_DOMAIN=com` |
| Englisch (Australien) | `LANGUAGE=en TOP_LEVEL_DOMAIN=com.au` |
| Englisch (Indien) | `LANGUAGE=en TOP_LEVEL_DOMAIN=co.in` |
| Französisch (Frankreich) | `LANGUAGE=fr TOP_LEVEL_DOMAIN=fr` |

Weitere Informationen finden Sie im Abschnitt zu lokalisierten Akzenten in der gTTS-Dokumentation.

## Hinweise zur Entwicklung

Siehe [DEVELOPMENT.md](./DEVELOPMENT.md) für das grundlegende Entwicklungssetup.

Häufige Entwicklungsbefehle:

```bash
poetry install --with dev
poetry run pytest
poetry run coverage run --source chatgpt_voice_assistant -m pytest tests
poetry run coverage report --show-missing --fail-under=90
poetry run ruff format --check .
poetry run ruff check .
```

Der CI-Workflow (`.github/workflows/test-application.yml`) führt Tests, Ruff-Checks und Coverage aus.
