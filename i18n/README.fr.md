[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


# Assistant Vocal ChatGPT

![GitHub Actions Build Status](https://github.com/jakecyr/openai-gpt3-chatbot/actions/workflows/test-application.yml/badge.svg)
![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?logo=python&logoColor=white)
![Poetry](https://img.shields.io/badge/Poetry-managed-60A5FA?logo=poetry&logoColor=white)
![Platform](https://img.shields.io/badge/Platform-macOS%20focused-0F172A)
![CLI](https://img.shields.io/badge/Interface-CLI-16A34A)

Une interface simple vers le modèle ChatGPT d'OpenAI, avec conversion de la parole en texte en entrée et du texte en parole en sortie.
`chatgpt-voice-assistant` utilise OpenAI Whisper pour la transcription vocale et prend en charge la synthèse vocale OpenAI, Google ou Apple.

## Vue d'ensemble

`chatgpt-voice-assistant` est une application CLI Python (`gptassist`) pour des conversations vocales avec les modèles OpenAI.

Flux général :

1. Capturer l'entrée du microphone.
2. Transcrire la parole en texte.
3. Générer une réponse du modèle.
4. Convertir le texte de réponse en parole.
5. Lire l'audio généré en local.

Ce projet est basé sur Poetry, typé, et couvert par des tests unitaires dans `tests/`.

## Fonctionnalités

| Domaine | Détails |
| --- | --- |
| Boucle vocale | Boucle de conversation CLI pilotée par la voix |
| Parole vers texte | Transcription OpenAI Whisper (`whisper-1`) |
| Moteurs de synthèse vocale | `openai` (OpenAI TTS), `google` (gTTS), `apple` (CLI `say`) |
| Comportement d'activation | Prise en charge d'un mot d'activation via `--wake-word` |
| Comportement de sortie | Arrêt via mot de sécurité avec `--safe-word` (par défaut, arrêt sur `EXIT`) |
| Périphériques d'entrée | Sélection explicite avec `--input-device-name` ou repli vers invite interactive |
| Réglage de la lecture | Vitesse de lecture vocale configurable via `--speech-rate` |
| Réglage d'accent | Langue/TLD configurables pour les accents Google TTS |

## Structure du projet

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

## Prérequis

| Exigence | Notes |
| --- | --- |
| Python | `>=3.9, <4.0` |
| Clé API OpenAI | Requise pour générer les réponses |
| Microphone/périphérique audio d'entrée | Nécessaire pour la capture vocale |
| Bibliothèques PortAudio | Requises pour la dépendance `pyaudio` |

### Prérequis Mac

Installez les dépendances :

```bash
brew install portaudio
brew link portaudio
```

Mettez à jour votre fichier de configuration `pydistutils` pour l'utilisation de PortAudio en exécutant :

```bash
echo "[build_ext]" >> $HOME/.pydistutils.cfg
echo "include_dirs="`brew --prefix portaudio`"/include/" >> $HOME/.pydistutils.cfg
echo "library_dirs="`brew --prefix portaudio`"/lib/" >> $HOME/.pydistutils.cfg
```

## Installation

### Installation depuis PyPI

Exécutez la commande suivante pour installer l'application CLI `gptassist` :

```bash
pip install chatgpt-voice-assistant
```

### Installation depuis les sources

1. Installez Poetry ([documentation officielle](https://python-poetry.org/docs/#installation) ou avec `pip install poetry`)
2. Installez les dépendances :

```bash
poetry install
```

Pour les dépendances de développement :

```bash
poetry install --with dev
```

## Utilisation

Définissez `OPENAI_API_KEY` avant l'exécution, ou passez directement votre clé secrète :

```bash
export OPENAI_API_KEY=<OPEN API SECRET KEY HERE>
gptassist

# OR

gptassist --open-ai-key=<OPEN API SECRET KEY HERE>
```

Si installé depuis les sources avec Poetry :

```bash
poetry run gptassist --open-ai-key=<OPEN API SECRET KEY HERE>
```

Commencez à parler et augmentez le volume pour entendre la réponse de l'assistant IA.

Dites le mot `exit` (ou votre mot de sécurité configuré) ou appuyez sur `Ctrl+C` dans votre terminal pour arrêter l'application.

## Configuration

### Variables d'environnement

| Variable | Requise | Description |
| --- | --- | --- |
| `OPENAI_API_KEY` | Oui (sauf surcharge) | Requise sauf si `--open-ai-key` est fourni |

### Options CLI

Ci-dessous, le menu d'aide du CLI `gptassist` détaillant les options disponibles :

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

## Exemples

### Basique

```bash
gptassist --open-ai-key=<OPENAI_KEY>
```

### Utiliser un mot d'activation

```bash
gptassist --open-ai-key=<OPENAI_KEY> --wake-word="hey assistant"
```

### Mot de sécurité personnalisé

```bash
gptassist --open-ai-key=<OPENAI_KEY> --safe-word="goodbye"
```

### Sélectionner un moteur TTS

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=openai
gptassist --open-ai-key=<OPENAI_KEY> --tts=google
gptassist --open-ai-key=<OPENAI_KEY> --tts=apple
```

### Spécifier un périphérique d'entrée par nom

```bash
gptassist --open-ai-key=<OPENAI_KEY> --input-device-name="MacBook Pro Microphone"
```

### Spécifier un accent de langue de sortie (Google TTS)

Spécifiez les variables `LANGUAGE` et `TOP_LEVEL_DOMAIN` pour remplacer l'anglais (États-Unis) par défaut :

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=google --lang=en --tld=com
```

#### Exemples de langue

| Locale | Value |
| --- | --- |
| French (France) | `LANGUAGE=fr TOP_LEVEL_DOMAIN=fr` |

Consultez la section des accents localisés dans la documentation gTTS pour plus d'informations.

## Notes de développement

Voir [DEVELOPMENT.md](./DEVELOPMENT.md) pour la configuration de base du développement.

Commandes de développement courantes :

```bash
poetry install --with dev
poetry run pytest
poetry run coverage run --source chatgpt_voice_assistant -m pytest tests
poetry run coverage report --show-missing --fail-under=90
poetry run ruff format --check .
poetry run ruff check .
```

Le workflow CI (`.github/workflows/test-application.yml`) exécute les tests, les vérifications Ruff et la couverture.

## Dépannage

### `Open AI Secret Key not specified...`

Définissez `OPENAI_API_KEY` ou passez `--open-ai-key`.

### Aucun périphérique d'entrée trouvé / mauvais microphone sélectionné

- Vérifiez que votre microphone est connecté et disponible pour l'OS.
- Utilisez `--input-device-name` avec un nom de périphérique exact.
- Si non fourni, l'application vous proposera de choisir parmi les périphériques d'entrée détectés.

### Problèmes d'installation/build de `pyaudio`

- Vérifiez que PortAudio est installé (voir la section des prérequis Mac).
- Relancez l'installation des dépendances après avoir configuré `.pydistutils.cfg`.

### Pas de lecture audio

- Le chemin de lecture actuel utilise `afplay` (commande macOS).
- Apple TTS utilise `say` et OpenAI/Google produisent des fichiers audio lus via `afplay`.
- Sur un OS non macOS, une adaptation supplémentaire de la lecture peut être nécessaire.

### Modèle indisponible

Si le modèle par défaut ou celui spécifié via `--open-ai-model` n'est pas disponible pour votre compte, passez un modèle auquel vous avez accès :

```bash
gptassist --open-ai-key=<OPENAI_KEY> --open-ai-model=<YOUR_AVAILABLE_MODEL>
```

## Internationalisation (i18n)

- `i18n/` existe pour les variantes multilingues du README.
- Cette version anglaise est intentionnellement conservée comme base canonique.
- Les futurs fichiers de langue doivent être ajoutés un par un et rester synchronisés avec le contenu canonique de ce README.
- Conservez exactement une seule ligne `Language options:` en haut de chaque variante de README et évitez les doublons.

## Feuille de route

- Étendre la documentation sur le comportement de lecture audio multiplateforme.
- Ajouter des variantes README multilingues générées/maintenues sous `i18n/`.
- Garder les valeurs par défaut et les exemples alignés sur les noms de modèles OpenAI actuellement recommandés.
- Améliorer la documentation d'architecture et d'exploitation au-delà des bases README/DEVELOPMENT.

## Contribution

1. Forkez le dépôt.
2. Créez une branche de fonctionnalité.
3. Exécutez les tests et vérifications en local :

```bash
poetry run pytest
poetry run ruff format --check .
poetry run ruff check .
```

4. Ouvrez une pull request avec une description claire des changements et de la couverture de tests.

## Licence

Aucun fichier `LICENSE` n'est actuellement présent dans ce dépôt.

Hypothèse : les conditions de licence ne sont pas encore explicitement déclarées. Ajoutez un fichier `LICENSE` pour clarifier les conditions d'utilisation et de distribution.

## Références

- [Speech Recognition library docs](https://pypi.org/project/SpeechRecognition/1.2.3)
- [Google Translate Text-to-Speech API (gTTS)](https://gtts.readthedocs.io/en/latest/module.html#)
