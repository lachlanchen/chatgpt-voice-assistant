[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


[![LazyingArt banner](https://github.com/lachlanchen/lachlanchen/raw/main/figs/banner.png)](https://github.com/lachlanchen/lachlanchen/blob/main/figs/banner.png)

# ChatGPT Sprachassistent

| Komponente | Status |
| --- | --- |
| 🧪 CI | ![CI](https://img.shields.io/badge/CI-configured-0EA5E9?style=flat-square&logo=githubactions&logoColor=white) |
| 🐍 Python | ![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?logo=python&logoColor=white) |
| 📦 Packaging | ![Poetry](https://img.shields.io/badge/Poetry-managed-60A5FA?logo=poetry&logoColor=white) |
| 🍎 Plattform | ![Platform](https://img.shields.io/badge/Platform-macOS%20first-0F172A) |
| 🖥️ Oberfläche | ![CLI](https://img.shields.io/badge/Interface-CLI-16A34A) |
| 🤝 Beiträge | ![Contributing](https://img.shields.io/badge/Contributing-Welcome-22C55E?style=flat-square&logo=github&logoColor=white) |

![Workflow-Status](https://img.shields.io/github/actions/workflow/status/lachlanchen/chatgpt-voice-assistant/test-application.yml?branch=main&style=flat-square&logo=githubactions&logoColor=white)
[![Offene Probleme](https://img.shields.io/github/issues/lachlanchen/chatgpt-voice-assistant?style=flat-square&logo=github&logoColor=white)](https://github.com/lachlanchen/chatgpt-voice-assistant/issues)
[![Sterne](https://img.shields.io/github/stars/lachlanchen/chatgpt-voice-assistant?style=flat-square&logo=github&logoColor=white)](https://github.com/lachlanchen/chatgpt-voice-assistant/stargazers)
[![Letzter Commit](https://img.shields.io/github/last-commit/lachlanchen/chatgpt-voice-assistant?style=flat-square&logo=git&logoColor=white)](https://github.com/lachlanchen/chatgpt-voice-assistant/commits/main)

Eine CLI-Anwendung für freihändige Gespräche mit OpenAI-Chat-Modellen. Sie zeichnet Sprache auf, transkribiert sie über Whisper, erzeugt Antworten und gibt synthetisierte Antworten über auswählbare TTS-Anbieter wieder.
`chatgpt-voice-assistant` hält die Laufzeit-Schleife schlank und die Architektur modular.

[![Installationsanleitung](https://img.shields.io/badge/Install-Quick_Start-0EA5E9?style=flat-square&logo=python&logoColor=white)](#schnellstart)
[![Nutzungsdemo](https://img.shields.io/badge/Usage-gptassist-16A34A?style=flat-square&logo=terminal&logoColor=white)](#nutzung)
[![Architektur](https://img.shields.io/badge/Architecture-Modular-8B5CF6?style=flat-square&logo=github&logoColor=white)](#architektur)
[![Fehlersuche](https://img.shields.io/badge/Troubleshoot-Common_Issues-F59E0B?style=flat-square&logo=bugatti&logoColor=white)](#fehlerbehebung)

---

## Ueberblick

| Schicht | Zweck |
| --- | --- |
| 🎤 Sprachschleife | Mikrofoneingaben werden kontinuierlich aufgenommen und transkribiert. |
| 🧠 Modellpfad | OpenAI-Completions erzeugen konversationelle Antworten. |
| 🔉 Sprachpfad | Antwort wird durch austauschbare TTS-Anbieter synthetisiert. |
| 🧩 Architektur | Listener → Generator → Responder-Verträge machen Komponenten austauschbar. |

---

| Fokus | Details |
| --- | --- |
| 🎙️ Spracherfassung | Mikrofongesteuerte Interaktionsschleife |
| 🧠 Modellantwort | OpenAI Chat Completion Antworten |
| 🔊 Sprachausgabe | Austauschbarer TTS-Ausgabepfad |
| 🧩 Projektstruktur | `chatgpt_voice_assistant/`-Module mit expliziten Verträgen |
| 🧩 Architektur | Modulare Listener-/Generator-/Responder-Verträge |

## Inhaltsverzeichnis

- [Sprachoptionen](#sprachoptionen)
- [Ueberblick](#ueberblick)
- [Schnellstart](#schnellstart)
- [Funktionen](#funktionen)
- [Architektur](#architektur)
- [Projektstruktur](#projektstruktur)
- [Voraussetzungen](#voraussetzungen)
- [Installation](#installation)
- [Nutzung](#nutzung)
- [Konfiguration](#konfiguration)
- [Beispiele](#beispiele)
- [Entwicklungsnotizen](#entwicklungsnotizen)
- [Fehlerbehebung](#fehlerbehebung)
- [Internationalisierung (i18n)](#internationalisierung-i18n)
- [Roadmap](#roadmap)
- [Mitmachen](#mitmachen)
- [❤️ Support](#-support)
- [Kontakt](#kontakt)
- [Lizenz](#lizenz)
- [Referenzen](#referenzen)

## Sprachoptionen

🌐 Sprachoptionen: Deutsch (aktuelle Fassung)

## Ueberblick

`chatgpt-voice-assistant` ist eine Python-CLI-Anwendung (`gptassist`) für freihändige Gespräche mit OpenAI-Chat-Modellen.

Ablauf im Großen:

1. Mikrofoneingabe erfassen.
2. Gesprochenen Text transkribieren.
3. Eine Modellantwort erzeugen.
4. Antworttext in Sprache umwandeln.
5. Die erzeugte Audiodatei lokal wiedergeben.

Das Projekt nutzt `poetry`, explizite Dependency Injection und typisierte Schnittstellen, um Sprachanbieter und Integrationen austauschbar zu halten.

### Entwurfsabsicht

- Die Laufzeit-Schleife schlank und vorhersagbar halten.
- Anbieter-Integrationen über explizite Schnittstellen austauschbar halten.
- CLI-zentrierte Interaktion mit expliziten Argumenten und Umgebungs-Fallbacks beibehalten.

### Schnellstart

1. Abhängigkeiten installieren (eine der Methoden unten wählen).
2. `OPENAI_API_KEY` setzen.
3. `gptassist` starten.
4. Sprechen und auf Transkription/Generierung/Wiedergabe warten.
5. Mit dem Sicherheitswort (`exit` / `--safe-word`) oder mit `Ctrl+C` beenden.

## Funktionen

| Bereich | Details |
| --- | --- |
| 🎛️ Sprachschleife | Sprachgesteuerte Gesprächsschleife mit Wake-Word- und Safe-Word-Steuerung |
| 🎤 Spracherkennung | OpenAI Whisper Transkription (`whisper-1`) |
| 🔉 Text-to-Speech-Engines | `openai` (`tts-1`), `google` (`gtts`), `apple` (`say` CLI) |
| 🧭 Wake-Verhalten | Unterstützung für `--wake-word` |
| 🛑 Beendigungsverhalten | Beenden über Safe-Word mit `--safe-word` (Standard beendet bei `EXIT`) |
| 🎧 Eingabegeräte | Explizite Gerätauswahl über `--input-device-name` oder interaktiver Eingabeaufforderung |
| 🎚️ Wiedergabe-Tuning | Konfigurierbare Wiedergabegeschwindigkeit über `--speech-rate` |
| 🧩 Akzentanpassung | `--lang` und `--tld` für Google-TTS-Akzente |
| ✅ Qualitätswerkzeuge | Ruff + Coverage-Prüfungen in Entwicklung und CI |

## Architektur

Der Kernlaufzeit ist absichtlich schichtbasiert, damit jede Implementierung mit minimaler Kopplung ersetzt werden kann.

1. `main.py` verdrahtet Laufzeitabhängigkeiten.
2. `cli_parser.py` analysiert Optionen und baut die Laufzeitkonfiguration.
3. `input_devices.py` löst und validiert die Auswahl von Aufzeichnungsgeräten.
4. Konkrete Implementierungen werden per Modus gewählt:
   - `whisper_listener.py` und `speech_listener.py` für STT
   - `open_ai_text_generator.py` für die Generierung
   - `computer_voice_responder.py` für Antwort-Orchestrierung und Wiedergabe
5. `conversation.py` führt die Schleifenlogik aus, inklusive Wake-Word + Safe-Word, Wiederholungen und Antwortfluss.

### Schnittstellen-Abstraktionen

- Listener-Abstraktion: `whisper_listener.py`, `speech_listener.py`
- Textgenerator-Abstraktion: `open_ai_text_generator.py` mit `open_ai_client.py`
- Responder-Abstraktion: `computer_voice_responder.py` mit Clients für OpenAI, Google und Apple
- Option-Parsing-Abstraktion: zentraler Parser in `cli_parser.py`

Diese Trennung macht Anbieterwechsel und Test-Mocking unkompliziert.

## Projektstruktur

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

### Kernfluss-Komponenten

- `main.py`: CLI-Einstiegspunkt für `gptassist`.
- `cli_parser.py`: Argument-Parsing und Validierung der Laufzeitkonfiguration.
- `conversation.py`: Orchestrierung der Schleife Aufzeichnen → Transkribieren → Generieren → Sprechen.
- `whisper_listener.py`: Primärer Listener-Pfad über OpenAI Whisper.
- `speech_listener.py`: Alternativer Sprachpfad.
- `open_ai_text_generator.py`: Chat-Completion-Integration und Prompt/Message-Flow.
- `computer_voice_responder.py`: TTS-Adapter und Wiedergabe-Orchestrierung.
- `input_devices.py`: Findet Geräte und löst die Benutzerauswahl auf.
- `clients/`, `bases/`, `helpers/`, `models/`, `exceptions/`: Service-Adapter, Verträge, geteilte Hilfsfunktionen, Domänenentitäten und explizite Fehlerarten.

## Voraussetzungen

| Voraussetzung | Hinweise |
| --- | --- |
| 🐍 Python | `>=3.9, <4.0` |
| 🔐 OpenAI API-Schlüssel | Erforderlich für die Antwortgenerierung |
| 🎙️ Mikrofon / Eingabegerät | Erforderlich für die Sprachaufnahme |
| 🎵 PortAudio-Bibliotheken | Erforderlich für die `pyaudio`-Abhängigkeit |

### macOS-Voraussetzungen

PortAudio-Abhängigkeiten installieren:

```bash
brew install portaudio
brew link portaudio
```

Aktualisieren Sie Ihre `pydistutils`-Konfiguration für die PortAudio-Nutzung:

```bash
echo "[build_ext]" >> $HOME/.pydistutils.cfg
echo "include_dirs="`brew --prefix portaudio`"/include/" >> $HOME/.pydistutils.cfg
echo "library_dirs="`brew --prefix portaudio`"/lib/" >> $HOME/.pydistutils.cfg
```

### Hinweise für Nicht-macOS-Systeme

Die CI installiert Ubuntu-kompatible Systempakete für Audio-Abhängigkeiten, aber das Laufzeitverhalten kann je nach Wiedergabe-Utilities variieren.

Nutzen Sie `.env.example` als lokale Referenz für benötigte Umgebungsvariablen.

## Installation

### Installation von PyPI

```bash
pip install chatgpt-voice-assistant
```

### Installation aus dem Quellcode

1. Dieses Repository klonen.
2. Poetry installieren ([offizielle Doku](https://python-poetry.org/docs/#installation) oder `pip install poetry`).
3. Abhängigkeiten installieren:

```bash
poetry install
```

Für Entwicklungsabhängigkeiten:

```bash
poetry install --with dev
```

## Nutzung

Setzen Sie `OPENAI_API_KEY` vor dem Start, oder geben Sie Ihren Schlüssel direkt weiter:

```bash
export OPENAI_API_KEY=<OPEN_AI_SECRET_KEY>
gptassist

# ODER
gptassist --open-ai-key=<OPEN_AI_SECRET_KEY>
```

Wenn Sie aus dem Quellcode ausführen:

```bash
poetry run gptassist --open-ai-key=<OPEN_AI_SECRET_KEY>
```

Fangen Sie an zu sprechen und lauschen Sie auf die gesprochene Rückmeldung.

Sagen Sie Ihr Sicherheitswort (`exit` standardmäßig, konfigurierbar mit `--safe-word`) oder drücken Sie `Ctrl+C`, um zu stoppen.

## Konfiguration

### Umgebungsvariablen

| Variable | Erforderlich | Beschreibung |
| --- | --- | --- |
| `OPENAI_API_KEY` | Ja (außer wenn `--open-ai-key` verwendet wird) | OpenAI-Anmeldeinformationen für STT + Chat + optionales TTS |

### CLI-Optionen

```text
-h, --help
    Zeigt diese Hilfenachricht an und beendet das Programm.

--log-level LOG_LEVEL
    Gibt an, ob im Debug-Modus protokolliert werden soll.

--input-device-name INPUT_DEVICE_NAME
    Der Name des Eingabegeräts.

--lang LANG
    Die Sprache, die für die Spracherkennung verwendet werden soll (z. B. en oder fr).

--max-tokens MAX_TOKENS
    Maximale OpenAI-Completion-Tokens für die Texterzeugung.

--tld TLD
    Top-Level-Domain (z. B. com oder com.au).

--safe-word SAFE_WORD
    Wort, das gesprochen wird, um die Anwendung zu beenden.

--wake-word WAKE_WORD
    (Optional) Wort, das eine Antwort auslöst.

--open-ai-key OPEN_AI_KEY
    Erforderlich. OpenAI-Geheimschlüssel (oder setzen Sie die Umgebungsvariable OPENAI_API_KEY)

--open-ai-model OPEN_AI_MODEL
    Das zu verwendende OpenAI-Modell. Siehe: https://platform.openai.com/docs/models/overview

--tts {apple,google,openai}
    Wählen Sie eine Text-to-Speech-Engine aus.

--speech-rate SPEECH_RATE
    Die Geschwindigkeit, mit der die Sprache wiedergegeben wird. 1.0 = normal
```

> Tipp: Halten Sie die dokumentierten Standardwerte in Ihrem aktuellen Checkout mit `cli_parser.py` synchron.

## Beispiele

### Grundlegend

```bash
gptassist --open-ai-key=<OPENAI_KEY>
```

### Wake-Word verwenden

```bash
gptassist --open-ai-key=<OPENAI_KEY> --wake-word="hey assistant"
```

### Eigenes Safe-Word

```bash
gptassist --open-ai-key=<OPENAI_KEY> --safe-word="goodbye"
```

### TTS-Engine auswählen

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=openai
gptassist --open-ai-key=<OPENAI_KEY> --tts=google
gptassist --open-ai-key=<OPENAI_KEY> --tts=apple
```

### Eingabegerät per Name auswählen

```bash
gptassist --open-ai-key=<OPENAI_KEY> --input-device-name="MacBook Pro Microphone"
```

### Google-Akzentausgabe konfigurieren

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=google --lang=en --tld=com
```

Setzen Sie `LANGUAGE` / `TOP_LEVEL_DOMAIN`, um das Standard-Lokalverhalten zu ändern:

| Locale | Wert |
| --- | --- |
| Französisch (Frankreich) | `LANGUAGE=fr TOP_LEVEL_DOMAIN=fr` |

Weitere Details finden Sie im Abschnitt zu lokalisierten Akzenten in der gTTS-Dokumentation.

### Kombiniertes Beispiel

```bash
export OPENAI_API_KEY=sk-...
gptassist --open-ai-key=$OPENAI_API_KEY --tts=google --wake-word="hey helper" --safe-word="stop now" --speech-rate=1.1
```

## Entwicklungsnotizen

Siehe [DEVELOPMENT.md](./DEVELOPMENT.md) für Einrichtung und Coding-Standards.

Häufige Befehle:

```bash
poetry install --with dev
poetry run pytest
poetry run coverage run --source chatgpt_voice_assistant -m pytest tests
poetry run coverage report --show-missing --fail-under=90
poetry run ruff format --check .
poetry run ruff check .
```

Der CI-Workflow `.github/workflows/test-application.yml` führt Tests, Ruff-Prüfungen und Coverage durch.

## Fehlerbehebung

### `Open AI Secret Key not specified...`

Setzen Sie `OPENAI_API_KEY` oder übergeben Sie `--open-ai-key`.

### Keine Eingabegeräte gefunden / falsches Mikrofon ausgewählt

- Stellen Sie sicher, dass Ihr Mikrofon angeschlossen und für das Betriebssystem verfügbar ist.
- Verwenden Sie `--input-device-name` mit einem exakten Gerätenamen.
- Falls nicht angegeben, fordert die App Sie auf, aus den erkannten Eingabegeräten zu wählen.

### Probleme bei Installation / Build von `pyaudio`

- Installieren Sie PortAudio (siehe macOS-Voraussetzungen).
- Wiederholen Sie die Abhängigkeitsinstallation nach dem Setzen von `.pydistutils.cfg`.

### Keine Tonwiedergabe

- Der aktuelle Wiedergabeweg verwendet `afplay` (macOS-Kommando).
- Apple TTS verwendet `say` und OpenAI/Google erzeugen Audiodateien, die mit `afplay` abgespielt werden.
- Auf Nicht-macOS-Plattformen kann eine zusätzliche Anpassung der Wiedergabe erforderlich sein.

### Modell nicht verfügbar

Wenn das Standard- oder ausgewählte `--open-ai-model` nicht verfügbar ist, übergeben Sie ein zugängliches Modell:

```bash
gptassist --open-ai-key=<OPENAI_KEY> --open-ai-model=<YOUR_AVAILABLE_MODEL>
```

### API-/Netzwerkfehler

- Überprüfen Sie die Konnektivität und die Berechtigungen des API-Schlüssels.
- Überprüfen Sie, ob der Schlüssel für den gewählten Anbieter/das Modell gültig ist.
- Erhöhen Sie das Logging mit `--log-level DEBUG`, um den Parser-/Laufzeitfluss nachzuvollziehen.

## Internationalisierung (i18n)

- `i18n/` enthält lokalisierte READMEs.
- Diese englische Fassung ist die kanonische Basis.
- Behalten Sie genau eine Zeile `Sprachoptionen:` nahe am Anfang jeder README-Variante und vermeiden Sie Duplikate.
- Übersetzungen sind nach Sprache und Locale gruppiert, um zum Dokumentationsfluss des Repos zu passen.

## Roadmap

- Das dokumentierte plattformübergreifende Verhalten der Audio-Wiedergabe erweitern.
- Weitere oder aktualisierte mehrsprachige README-Varianten unter `i18n/` hinzufügen.
- Standardwerte und Beispiele auf aktuell empfohlene OpenAI-Modellnamen abstimmen.
- Architektur- und Betriebsdokumentation über README/DEVELOPMENT hinaus verbessern.

## Mitmachen

1. Forken Sie das Repository.
2. Erstellen Sie einen Feature-Branch.
3. Führen Sie lokal Tests und Prüfungen aus:

```bash
poetry run pytest
poetry run ruff format --check .
poetry run ruff check .
```

4. Öffnen Sie eine Pull-Request mit klarer Beschreibung der Änderungen und Testabdeckung.

## Kontakt

- Fehler melden oder Funktionen anfragen: [GitHub Issues](https://github.com/lachlanchen/chatgpt-voice-assistant/issues)
- Design- oder Architekturfragen stellen: [GitHub Discussions](https://github.com/lachlanchen/chatgpt-voice-assistant/discussions)

## Lizenz

In diesem Repository ist derzeit keine `LICENSE`-Datei vorhanden.

Annahme: Nutzungs- und Verteilungsbedingungen sind noch nicht explizit festgelegt. Bitte ergänzen Sie eine `LICENSE`-Datei, um die Nutzung zu klären.

## Referenzen

- [Speech Recognition Bibliotheksdokumentation](https://pypi.org/project/SpeechRecognition/1.2.3)
- [Google Translate Text-to-Speech API (gTTS)](https://gtts.readthedocs.io/en/latest/module.html#)


## ❤️ Support

| Donate | PayPal | Stripe |
| --- | --- | --- |
| [![Donate](https://camo.githubusercontent.com/24a4914f0b42c6f435f9e101621f1e52535b02c225764b2f6cc99416926004b7/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f446f6e6174652d4c617a79696e674172742d3045413545393f7374796c653d666f722d7468652d6261646765266c6f676f3d6b6f2d6669266c6f676f436f6c6f723d7768697465)](https://chat.lazying.art/donate) | [![PayPal](https://camo.githubusercontent.com/d0f57e8b016517a4b06961b24d0ca87d62fdba16e18bbdb6aba28e978dc0ea21/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f50617950616c2d526f6e677a686f754368656e2d3030343537433f7374796c653d666f722d7468652d6261646765266c6f676f3d70617970616c266c6f676f436f6c6f723d7768697465)](https://paypal.me/RongzhouChen) | [![Stripe](https://camo.githubusercontent.com/1152dfe04b6943afe3a8d2953676749603fb9f95e24088c92c97a01a897b4942/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f5374726970652d446f6e6174652d3633354246463f7374796c653d666f722d7468652d6261646765266c6f676f3d737472697065266c6f676f436f6c6f723d7768697465)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400) |
