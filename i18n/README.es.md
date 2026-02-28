[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


# Asistente de Voz para ChatGPT

![GitHub Actions Build Status](https://github.com/jakecyr/openai-gpt3-chatbot/actions/workflows/test-application.yml/badge.svg)
![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?logo=python&logoColor=white)
![Poetry](https://img.shields.io/badge/Poetry-managed-60A5FA?logo=poetry&logoColor=white)
![Platform](https://img.shields.io/badge/Platform-macOS%20focused-0F172A)
![CLI](https://img.shields.io/badge/Interface-CLI-16A34A)

Una interfaz sencilla para el modelo ChatGPT de OpenAI con conversión de voz a texto para la entrada y de texto a voz para la salida.
`chatgpt-voice-assistant` usa OpenAI Whisper para transcribir voz y admite salida de texto a voz con OpenAI, Google o Apple.

## Descripción general

`chatgpt-voice-assistant` es una aplicación CLI de Python (`gptassist`) para conversaciones por voz con modelos de OpenAI.

Flujo de alto nivel:

1. Capturar entrada del micrófono.
2. Transcribir voz a texto.
3. Generar una respuesta del modelo.
4. Convertir el texto de respuesta a voz.
5. Reproducir el audio generado localmente.

Este proyecto está basado en Poetry, usa tipado y está cubierto por pruebas unitarias en `tests/`.

## Funcionalidades

| Área | Detalles |
| --- | --- |
| Bucle de voz | Bucle de conversación en CLI controlado por voz |
| Voz a texto | Transcripción con OpenAI Whisper (`whisper-1`) |
| Motores de texto a voz | `openai` (OpenAI TTS), `google` (gTTS), `apple` (`say` CLI) |
| Comportamiento de activación | Soporte de palabra de activación mediante `--wake-word` |
| Comportamiento de salida | Finalización con palabra de seguridad mediante `--safe-word` (el comportamiento predeterminado sale con `EXIT`) |
| Dispositivos de entrada | Selección explícita con `--input-device-name` o fallback a un prompt interactivo |
| Ajuste de reproducción | Velocidad de reproducción de voz configurable mediante `--speech-rate` |
| Ajuste de acento | Idioma/TLD configurable para acentos de Google TTS |

## Estructura del proyecto

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

## Requisitos previos

| Requisito | Notas |
| --- | --- |
| Python | `>=3.9, <4.0` |
| Clave de API de OpenAI | Obligatoria para generar respuestas |
| Micrófono/dispositivo de entrada de audio | Necesario para capturar voz |
| Librerías de PortAudio | Obligatorias para la dependencia `pyaudio` |

### Requisitos en Mac

Instala las dependencias:

```bash
brew install portaudio
brew link portaudio
```

Actualiza tu archivo de configuración `pydistutils` para usar PortAudio ejecutando:

```bash
echo "[build_ext]" >> $HOME/.pydistutils.cfg
echo "include_dirs="`brew --prefix portaudio`"/include/" >> $HOME/.pydistutils.cfg
echo "library_dirs="`brew --prefix portaudio`"/lib/" >> $HOME/.pydistutils.cfg
```

## Instalación

### Instalar desde PyPI

Ejecuta lo siguiente para instalar la aplicación CLI `gptassist`:

```bash
pip install chatgpt-voice-assistant
```

### Instalar desde el código fuente

1. Instala Poetry ([documentación oficial](https://python-poetry.org/docs/#installation) o con `pip install poetry`)
2. Instala las dependencias:

```bash
poetry install
```

Para dependencias de desarrollo:

```bash
poetry install --with dev
```

## Uso

Puedes definir `OPENAI_API_KEY` antes de ejecutar, o pasar tu clave secreta directamente:

```bash
export OPENAI_API_KEY=<OPEN API SECRET KEY HERE>
gptassist

# OR

gptassist --open-ai-key=<OPEN API SECRET KEY HERE>
```

Si lo instalaste desde el código fuente con Poetry:

```bash
poetry run gptassist --open-ai-key=<OPEN API SECRET KEY HERE>
```

Comienza a hablar y sube el volumen para escuchar la respuesta del asistente de IA.

Di la palabra `exit` (o tu palabra de seguridad configurada) o presiona `Ctrl+C` en la terminal para detener la aplicación.

## Configuración

### Variables de entorno

| Variable | Obligatoria | Descripción |
| --- | --- | --- |
| `OPENAI_API_KEY` | Sí (a menos que se sobrescriba) | Obligatoria a menos que se proporcione `--open-ai-key` |

### Opciones de CLI

A continuación se muestra el menú de ayuda de la CLI `gptassist` con las opciones disponibles:

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

## Ejemplos

### Básico

```bash
gptassist --open-ai-key=<OPENAI_KEY>
```

### Usar una palabra de activación

```bash
gptassist --open-ai-key=<OPENAI_KEY> --wake-word="hey assistant"
```

### Palabra de seguridad personalizada

```bash
gptassist --open-ai-key=<OPENAI_KEY> --safe-word="goodbye"
```

### Seleccionar motor TTS

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=openai
gptassist --open-ai-key=<OPENAI_KEY> --tts=google
gptassist --open-ai-key=<OPENAI_KEY> --tts=apple
```

### Especificar dispositivo de entrada por nombre

```bash
gptassist --open-ai-key=<OPENAI_KEY> --input-device-name="MacBook Pro Microphone"
```

### Especificar un acento de idioma de salida (Google TTS)

Especifica tanto las variables `LANGUAGE` como `TOP_LEVEL_DOMAIN` para sobrescribir el inglés predeterminado (Estados Unidos):

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=google --lang=en --tld=com
```

#### Ejemplos de idioma

| Configuración regional | Valor |
| --- | --- |
| Inglés (Estados Unidos) _PREDETERMINADO_ | `LANGUAGE=en TOP_LEVEL_DOMAIN=com` |
| Inglés (Australia) | `LANGUAGE=en TOP_LEVEL_DOMAIN=com.au` |
| Inglés (India) | `LANGUAGE=en TOP_LEVEL_DOMAIN=co.in` |
| Francés (Francia) | `LANGUAGE=fr TOP_LEVEL_DOMAIN=fr` |

Consulta la sección de acentos localizados en la documentación de gTTS para más información.

## Notas de desarrollo

Consulta [DEVELOPMENT.md](../DEVELOPMENT.md) para la configuración base de desarrollo.

Comandos comunes de desarrollo:

```bash
poetry install --with dev
poetry run pytest
poetry run coverage run --source chatgpt_voice_assistant -m pytest tests
poetry run coverage report --show-missing --fail-under=90
poetry run ruff format --check .
poetry run ruff check .
```

El workflow de CI (`.github/workflows/test-application.yml`) ejecuta pruebas, comprobaciones de Ruff y cobertura.
