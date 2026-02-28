[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


[![LazyingArt banner](https://github.com/lachlanchen/lachlanchen/raw/main/figs/banner.png)](https://github.com/lachlanchen/lachlanchen/blob/main/figs/banner.png)

# Assistant vocal ChatGPT

| Composant | Statut |
| --- | --- |
| 🧪 CI | ![CI](https://img.shields.io/badge/CI-configured-0EA5E9?style=flat-square&logo=githubactions&logoColor=white) |
| 🐍 Python | ![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?logo=python&logoColor=white) |
| 📦 Packaging | ![Poetry](https://img.shields.io/badge/Poetry-managed-60A5FA?logo=poetry&logoColor=white) |
| 🍎 Plateforme | ![Platform](https://img.shields.io/badge/Platform-macOS%20first-0F172A) |
| 🖥️ Interface | ![CLI](https://img.shields.io/badge/Interface-CLI-16A34A) |
| 🤝 Contributions | ![Contributing](https://img.shields.io/badge/Contributing-Welcome-22C55E?style=flat-square&logo=github&logoColor=white) |

![Statut du workflow](https://img.shields.io/github/actions/workflow/status/lachlanchen/chatgpt-voice-assistant/test-application.yml?branch=main&style=flat-square&logo=githubactions&logoColor=white)
[![Issues ouvertes](https://img.shields.io/github/issues/lachlanchen/chatgpt-voice-assistant?style=flat-square&logo=github&logoColor=white)](https://github.com/lachlanchen/chatgpt-voice-assistant/issues)
[![Étoiles](https://img.shields.io/github/stars/lachlanchen/chatgpt-voice-assistant?style=flat-square&logo=github&logoColor=white)](https://github.com/lachlanchen/chatgpt-voice-assistant/stargazers)
[![Dernier commit](https://img.shields.io/github/last-commit/lachlanchen/chatgpt-voice-assistant?style=flat-square&logo=git&logoColor=white)](https://github.com/lachlanchen/chatgpt-voice-assistant/commits/main)

Une application CLI pour des conversations mains libres avec les modèles de chat d'OpenAI. Elle enregistre la voix, transcrit via Whisper, génère des réponses et lit les réponses synthétisées via des fournisseurs TTS sélectionnables.
`chatgpt-voice-assistant` garde la boucle d'exécution légère et l'architecture modulaire.

[![Guide d'installation](https://img.shields.io/badge/Install-Quick_Start-0EA5E9?style=flat-square&logo=python&logoColor=white)](#demarrage-rapide)
[![Exemple d'usage](https://img.shields.io/badge/Usage-gptassist-16A34A?style=flat-square&logo=terminal&logoColor=white)](#utilisation)
[![Architecture](https://img.shields.io/badge/Architecture-Modulaire-8B5CF6?style=flat-square&logo=github&logoColor=white)](#architecture)
[![Résolution de problèmes](https://img.shields.io/badge/Troubleshoot-Common_Issues-F59E0B?style=flat-square&logo=bugatti&logoColor=white)](#depannage)

---

## En un coup d'œil

| Couche | Rôle |
| --- | --- |
| 🎤 Boucle vocale | L'entrée micro est capturée et transcrite en continu. |
| 🧠 Chemin du modèle | Les complétions OpenAI génèrent des réponses conversationnelles. |
| 🔉 Chemin vocal | La réponse est synthétisée via des fournisseurs TTS interchangeables. |
| 🧩 Architecture | Les contrats Listener → générateur → responder gardent les composants remplaçables. |

---

| Focus | Détails |
| --- | --- |
| 🎙️ Capture vocale | Boucle d'interaction pilotée par micro |
| 🧠 Sortie du modèle | Réponses de chat completions OpenAI |
| 🔊 Lecture audio | Parcours de sortie TTS remplaçable |
| 🧩 Structure du projet | Modules `chatgpt_voice_assistant/` centrés sur des contrats explicites |
| 🧩 Architecture | Contrats listener / generateur / responder modulaires |

## Table des matières

- [Options linguistiques](#options-linguistiques)
- [Aperçu](#apercu)
- [Démarrage rapide](#demarrage-rapide)
- [Fonctionnalités](#fonctionnalites)
- [Architecture](#architecture)
- [Structure du projet](#structure-du-projet)
- [Prérequis](#pre-requis)
- [Installation](#installation)
- [Utilisation](#utilisation)
- [Configuration](#configuration)
- [Exemples](#exemples)
- [Notes de développement](#notes-de-developpement)
- [Dépannage](#depannage)
- [Internationalisation (i18n)](#internationalisation-i18n)
- [Feuille de route](#feuille-de-route)
- [Contribution](#contribution)
- [❤️ Support](#-support)
- [Contact](#contact)
- [Licence](#licence)
- [Références](#references)

## Options linguistiques

🌐 Options linguistiques : Français (version actuelle)

## Aperçu

`chatgpt-voice-assistant` est une application CLI Python (`gptassist`) pour des conversations mains libres avec les modèles de chat OpenAI.

Flux de haut niveau :

1. Capturer l'entrée du microphone.
2. Transcrire la parole en texte.
3. Générer une réponse du modèle.
4. Convertir la réponse texte en parole.
5. Lire l'audio généré localement.

Le projet utilise `poetry`, l'injection de dépendances explicite et des interfaces typées pour garder les fournisseurs vocaux et les intégrations remplaçables.

### Intention de conception

- Garder la boucle d'exécution minimale et prévisible.
- Garder les intégrations de fournisseurs échangeables via des interfaces explicites.
- Garder une interaction d'abord CLI avec arguments explicites et repli basé sur l'environnement.

### Démarrage rapide

1. Installez les dépendances (choisissez une méthode ci-dessous).
2. Définissez `OPENAI_API_KEY`.
3. Lancez `gptassist`.
4. Parlez, puis attendez la transcription / génération / lecture.
5. Terminez avec le mot de sortie (`exit` / `--safe-word`) ou `Ctrl+C`.

## Fonctionnalités

| Domaine | Détails |
| --- | --- |
| 🎛️ Boucle vocale | Boucle conversationnelle pilotée par la voix avec gestion wake-word + safe-word |
| 🎤 Reconnaissance vocale | Transcription OpenAI Whisper (`whisper-1`) |
| 🔉 Moteurs de synthèse vocale | `openai` (`tts-1`), `google` (`gtts`), `apple` (`say` CLI) |
| 🧭 Gestion du réveil | Prise en charge de `--wake-word` |
| 🛑 Gestion de sortie | Arrêt par mot-clé sûr via `--safe-word` (comportement par défaut : sortie sur `EXIT`) |
| 🎧 Périphériques d'entrée | Sélection explicite du périphérique via `--input-device-name` ou invite interactive |
| 🎚️ Réglage de lecture | Vitesse de lecture configurable via `--speech-rate` |
| 🧩 Ajustement d'accent | `--lang` et `--tld` pour les accents Google TTS |
| ✅ Outils de qualité | Contrôles Ruff + couverture en développement et CI |

## Architecture

Le cœur d'exécution est volontairement en couches afin que chaque implémentation puisse être remplacée avec un couplage minimal.

1. `main.py` assemble les dépendances d'exécution.
2. `cli_parser.py` analyse les options et construit la configuration d'exécution.
3. `input_devices.py` résout et valide les choix de périphériques de capture.
4. Les implémentations concrètes sont sélectionnées par mode :
   - `whisper_listener.py` et `speech_listener.py` pour STT
   - `open_ai_text_generator.py` pour la génération
   - `computer_voice_responder.py` pour l'orchestration des réponses et la lecture
5. `conversation.py` exécute la logique de boucle, y compris wake word + safe word, relances et flux de réponse.

### Abstractions d'interface

- Abstraction de l'écouteur : `whisper_listener.py`, `speech_listener.py`
- Abstraction du générateur de texte : `open_ai_text_generator.py` avec `open_ai_client.py`
- Abstraction du répondant : `computer_voice_responder.py` avec clients pour OpenAI, Google et Apple
- Abstraction de l'analyse des options : parser centralisé dans `cli_parser.py`

Cette séparation rend la migration de fournisseurs et la simulation de tests beaucoup plus simple.

## Structure du projet

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

### Composants du flux principal

- `main.py` : point d'entrée CLI de `gptassist`.
- `cli_parser.py` : analyse des arguments et validation de la configuration d'exécution.
- `conversation.py` : orchestration de la boucle enregistrement → transcription → génération → lecture.
- `whisper_listener.py` : chemin d'écoute principal via OpenAI Whisper.
- `speech_listener.py` : chemin vocal alternatif.
- `open_ai_text_generator.py` : intégration de chat completion et logique prompt/message.
- `computer_voice_responder.py` : adaptateur TTS et orchestration de la lecture.
- `input_devices.py` : découvre les périphériques et résout la sélection utilisateur.
- `clients/`, `bases/`, `helpers/`, `models/`, `exceptions/` : adaptateurs de service, contrats, helpers partagés, entités du domaine et types d'erreur explicites.

## Prérequis

| Exigence | Remarques |
| --- | --- |
| 🐍 Python | `>=3.9, <4.0` |
| 🔐 Clé API OpenAI | Requise pour générer les réponses |
| 🎙️ Microphone / périphérique d'entrée | Nécessaire pour la capture vocale |
| 🎵 Bibliothèques PortAudio | Requises pour la dépendance `pyaudio` |

### Prérequis macOS

Installez les dépendances PortAudio :

```bash
brew install portaudio
brew link portaudio
```

Mettez à jour votre configuration `pydistutils` pour l'utilisation de PortAudio :

```bash
echo "[build_ext]" >> $HOME/.pydistutils.cfg
echo "include_dirs="`brew --prefix portaudio`"/include/" >> $HOME/.pydistutils.cfg
echo "library_dirs="`brew --prefix portaudio`"/lib/" >> $HOME/.pydistutils.cfg
```

### Notes non-macOS

La CI installe des paquets système compatibles Ubuntu pour les dépendances audio, mais le comportement d'exécution peut différer selon les utilitaires de lecture audio.

Utilisez `.env.example` comme référence locale pour les entrées d'environnement requises.

## Installation

### Installation depuis PyPI

```bash
pip install chatgpt-voice-assistant
```

### Installation depuis la source

1. Clonez ce dépôt.
2. Installez Poetry ([documentation officielle](https://python-poetry.org/docs/#installation) ou `pip install poetry`).
3. Installez les dépendances :

```bash
poetry install
```

Pour les dépendances de développement :

```bash
poetry install --with dev
```

## Utilisation

Définissez `OPENAI_API_KEY` avant de lancer, ou passez votre clé directement :

```bash
export OPENAI_API_KEY=<OPEN_AI_SECRET_KEY>
gptassist

# OU
gptassist --open-ai-key=<OPEN_AI_SECRET_KEY>
```

Si vous exécutez depuis la source :

```bash
poetry run gptassist --open-ai-key=<OPEN_AI_SECRET_KEY>
```

Commencez à parler et écoutez les réponses parlées.

Prononcez votre mot sûr (`exit` par défaut, configurable via `--safe-word`) ou appuyez sur `Ctrl+C` pour arrêter.

## Configuration

### Variables d'environnement

| Variable | Requise | Description |
| --- | --- | --- |
| `OPENAI_API_KEY` | Oui (sauf si `--open-ai-key` est utilisé) | Identifiants OpenAI utilisés par STT + chat + TTS optionnel |

### Options CLI

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

> Astuce : Gardez les valeurs par défaut documentées synchronisées avec `cli_parser.py` de votre version actuelle.

## Exemples

### De base

```bash
gptassist --open-ai-key=<OPENAI_KEY>
```

### Utiliser un mot de réveil

```bash
gptassist --open-ai-key=<OPENAI_KEY> --wake-word="hey assistant"
```

### Mot sûr personnalisé

```bash
gptassist --open-ai-key=<OPENAI_KEY> --safe-word="goodbye"
```

### Sélectionner le moteur TTS

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=openai
gptassist --open-ai-key=<OPENAI_KEY> --tts=google
gptassist --open-ai-key=<OPENAI_KEY> --tts=apple
```

### Spécifier le périphérique d'entrée par nom

```bash
gptassist --open-ai-key=<OPENAI_KEY> --input-device-name="MacBook Pro Microphone"
```

### Configurer l'accent Google

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=google --lang=en --tld=com
```

Définissez `LANGUAGE` / `TOP_LEVEL_DOMAIN` pour modifier le comportement local par défaut :

| Paramètres régionaux | Valeur |
| --- | --- |

Consultez la section accents localisés dans la documentation de gTTS pour plus de détails.

### Exemple combiné

```bash
export OPENAI_API_KEY=sk-...
gptassist --open-ai-key=$OPENAI_API_KEY --tts=google --wake-word="hey helper" --safe-word="stop now" --speech-rate=1.1
```

## Notes de développement

Voir [DEVELOPMENT.md](./DEVELOPMENT.md) pour la configuration et les standards de code.

Commandes courantes :

```bash
poetry install --with dev
poetry run pytest
poetry run coverage run --source chatgpt_voice_assistant -m pytest tests
poetry run coverage report --show-missing --fail-under=90
poetry run ruff format --check .
poetry run ruff check .
```

Le workflow CI `.github/workflows/test-application.yml` exécute les tests, les vérifications Ruff et la couverture.

## Dépannage

### `Open AI Secret Key not specified...`

Définissez `OPENAI_API_KEY` ou passez `--open-ai-key`.

### Aucun périphérique d'entrée trouvé / mauvais microphone sélectionné

- Vérifiez que votre microphone est connecté et disponible pour l'OS.
- Utilisez `--input-device-name` avec un nom de périphérique exact.
- Si non fourni, l'application vous demandera de choisir parmi les périphériques détectés.

### Problèmes d'installation / compilation de `pyaudio`

- Installez PortAudio (voir les prérequis macOS).
- Relancez l'installation des dépendances après avoir configuré `.pydistutils.cfg`.

### Pas de lecture audio

- Le chemin de lecture actuel utilise `afplay` (commande macOS).
- Apple TTS utilise `say` et OpenAI/Google génèrent des fichiers audio lus via `afplay`.
- Sur les plateformes non macOS, une adaptation supplémentaire de la lecture peut être nécessaire.

### Modèle indisponible

Si le `--open-ai-model` par défaut ou sélectionné est indisponible, indiquez un modèle accessible :

```bash
gptassist --open-ai-key=<OPENAI_KEY> --open-ai-model=<YOUR_AVAILABLE_MODEL>
```

### Erreurs API / réseau

- Vérifiez la connectivité et les permissions de la clé API.
- Vérifiez que la clé est valide pour le fournisseur/modèle sélectionné.
- Augmentez la verbosité avec `--log-level DEBUG` pour tracer le flux parser/runtime.

## Internationalisation (i18n)

- `i18n/` contient les READMEs localisées.
- Cette version anglaise est la base canonique.
- Conservez exactement une ligne `Options linguistiques :` en haut de chaque variante README et évitez les doublons.
- Les traductions sont groupées par langue et locale pour correspondre au flux documentaire du dépôt.

## Feuille de route

- Étendre la documentation du comportement audio multiplateforme.
- Ajouter ou mettre à jour des variantes README multilingues sous `i18n/`.
- Maintenir les valeurs par défaut et les exemples alignés avec les noms de modèle OpenAI recommandés.
- Améliorer les documents d'architecture et d'exploitation au-delà des bases README/DEVELOPMENT.

## Contribution

1. Forkez le dépôt.
2. Créez une branche de fonctionnalité.
3. Exécutez les tests et contrôles localement :

```bash
poetry run pytest
poetry run ruff format --check .
poetry run ruff check .
```

4. Ouvrez une pull request avec une description claire des changements et de la couverture des tests.

## ❤️ Support

| Donate | PayPal | Stripe |
| --- | --- | --- |
| [![Donate](https://camo.githubusercontent.com/24a4914f0b42c6f435f9e101621f1e52535b02c225764b2f6cc99416926004b7/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f446f6e6174652d4c617a79696e674172742d3045413545393f7374796c653d666f722d7468652d6261646765266c6f676f3d6b6f2d6669266c6f676f436f6c6f723d7768697465)](https://chat.lazying.art/donate) | [![PayPal](https://camo.githubusercontent.com/d0f57e8b016517a4b06961b24d0ca87d62fdba16e18bbdb6aba28e978dc0ea21/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f50617950616c2d526f6e677a686f754368656e2d3030343537433f7374796c653d666f722d7468652d6261646765266c6f676f3d70617970616c266c6f676f436f6c6f723d7768697465)](https://paypal.me/RongzhouChen) | [![Stripe](https://camo.githubusercontent.com/1152dfe04b6943afe3a8d2953676749603fb9f95e24088c92c97a01a897b4942/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f5374726970652d446f6e6174652d3633354246463f7374796c653d666f722d7468652d6261646765266c6f676f3d737472697065266c6f676f436f6c6f723d7768697465)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400) |

## Contact

- Signaler des bogues ou demander des fonctionnalités : [GitHub Issues](https://github.com/lachlanchen/chatgpt-voice-assistant/issues)
- Poser des questions de conception ou d'architecture : [GitHub Discussions](https://github.com/lachlanchen/chatgpt-voice-assistant/discussions)

## Licence

Aucun fichier `LICENSE` n'est actuellement présent dans ce dépôt.

Hypothèse : les termes de licence ne sont pas encore explicitement déclarés. Ajoutez un fichier `LICENSE` pour clarifier les conditions d'utilisation et de distribution.

## Références

- [Documentation de la bibliothèque de reconnaissance vocale](https://pypi.org/project/SpeechRecognition/1.2.3)
- [API Google Translate Text-to-Speech (gTTS)](https://gtts.readthedocs.io/en/latest/module.html#)
