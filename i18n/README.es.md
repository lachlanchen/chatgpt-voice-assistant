[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


[![LazyingArt banner](https://github.com/lachlanchen/lachlanchen/raw/main/figs/banner.png)](https://github.com/lachlanchen/lachlanchen/blob/main/figs/banner.png)

# Asistente de voz ChatGPT

| Componente | Estado |
| --- | --- |
| 🧪 CI | ![CI](https://img.shields.io/badge/CI-configured-0EA5E9?style=flat-square&logo=githubactions&logoColor=white) |
| 🐍 Python | ![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?logo=python&logoColor=white) |
| 📦 Packaging | ![Poetry](https://img.shields.io/badge/Poetry-managed-60A5FA?logo=poetry&logoColor=white) |
| 🍎 Platform | ![Platform-macOS%20first-0F172A](https://img.shields.io/badge/Platform-macOS%20first-0F172A) |
| 🖥️ Interface | ![CLI](https://img.shields.io/badge/Interface-CLI-16A34A) |
| 🤝 Contribuciones | ![Contributing-Welcome-22C55E?style=flat-square&logo=github&logoColor=white](https://img.shields.io/badge/Contributing-Welcome-22C55E?style=flat-square&logo=github&logoColor=white) |

![Workflow status](https://img.shields.io/github/actions/workflow/status/lachlanchen/chatgpt-voice-assistant/test-application.yml?branch=main&style=flat-square&logo=githubactions&logoColor=white)
[![Open Issues](https://img.shields.io/github/issues/lachlanchen/chatgpt-voice-assistant?style=flat-square&logo=github&logoColor=white)](https://github.com/lachlanchen/chatgpt-voice-assistant/issues)
[![Stars](https://img.shields.io/github/stars/lachlanchen/chatgpt-voice-assistant?style=flat-square&logo=github&logoColor=white)](https://github.com/lachlanchen/chatgpt-voice-assistant/stargazers)
[![Last commit](https://img.shields.io/github/last-commit/lachlanchen/chatgpt-voice-assistant?style=flat-square&logo=git&logoColor=white)](https://github.com/lachlanchen/chatgpt-voice-assistant/commits/main)

Una aplicación CLI para mantener conversaciones manos libres con modelos de chat de OpenAI. Graba el habla, la transcribe con Whisper, genera respuestas y reproduce respuestas sintetizadas mediante proveedores de TTS seleccionables.
`chatgpt-voice-assistant` mantiene el bucle de ejecución mínimo y una arquitectura modular.

[![Guía de instalación](https://img.shields.io/badge/Install-Quick_Start-0EA5E9?style=flat-square&logo=python&logoColor=white)](#inicio-rápido)
[![Demo de uso](https://img.shields.io/badge/Usage-gptassist-16A34A?style=flat-square&logo=terminal&logoColor=white)](#uso)
[![Arquitectura](https://img.shields.io/badge/Architecture-Modular-8B5CF6?style=flat-square&logo=github&logoColor=white)](#arquitectura)
[![Solución de problemas](https://img.shields.io/badge/Troubleshoot-Common_Issues-F59E0B?style=flat-square&logo=bugatti&logoColor=white)](#solución-de-problemas)

---

## A primera vista

| Capa | Propósito |
| --- | --- |
| 🎤 Bucle de voz | La entrada del micrófono se captura y transcribe continuamente. |
| 🧠 Ruta del modelo | Las completions de OpenAI generan respuestas conversacionales. |
| 🔉 Ruta de voz | La respuesta se sintetiza a través de proveedores TTS intercambiables. |
| 🧩 Arquitectura | Los contratos Listener → Generator → Responder mantienen componentes intercambiables. |

---

| Enfoque | Detalles |
| --- | --- |
| 🎙️ Captura de voz | Bucle de interacción dirigido por micrófono |
| 🧠 Salida del modelo | Respuestas de chat completion de OpenAI |
| 🔊 Reproducción de voz | Ruta de salida TTS intercambiable |
| 🧩 Estructura del proyecto | Módulos de `chatgpt_voice_assistant/` alrededor de contratos explícitos |
| 🧩 Arquitectura | Contratos modulares de listener / generator / responder |

## Tabla de contenidos

- [Opciones de idioma](#opciones-de-idioma)
- [Resumen](#resumen)
- [Inicio rápido](#inicio-rápido)
- [Características](#características)
- [Arquitectura](#arquitectura)
- [Estructura del proyecto](#estructura-del-proyecto)
- [Requisitos previos](#requisitos-previos)
- [Instalación](#instalación)
- [Uso](#uso)
- [Configuración](#configuración)
- [Ejemplos](#ejemplos)
- [Notas de desarrollo](#notas-de-desarrollo)
- [Solución de problemas](#solución-de-problemas)
- [Internacionalización (i18n)](#internacionalización-i18n)
- [Roadmap](#roadmap)
- [Contribución](#contribución)
- [❤️ Support](#-support)
- [Contact](#contact)
- [Licencia](#licencia)
- [Referencias](#referencias)

## Opciones de idioma

🌐 Opciones de idioma: Español (borrador actual)

## Resumen

`chatgpt-voice-assistant` es una aplicación CLI en Python (`gptassist`) para mantener conversaciones manos libres con modelos de chat de OpenAI.

Flujo general:

1. Captura la entrada del micrófono.
2. Transcribe el habla a texto.
3. Genera una respuesta del modelo.
4. Convierte el texto de respuesta a voz.
5. Reproduce el audio generado de forma local.

El proyecto usa `poetry`, inyección de dependencias explícita e interfaces tipadas para mantener reemplazables a los proveedores de voz y demás integraciones.

### Objetivo de diseño

- Mantener el bucle de ejecución mínimo y predecible.
- Mantener intercambiables las integraciones de proveedores mediante interfaces explícitas.
- Mantener interacción CLI-first con argumentos explícitos y respaldo mediante variables de entorno.

### Inicio rápido

1. Instala las dependencias (elige uno de los métodos).
2. Configura `OPENAI_API_KEY`.
3. Ejecuta `gptassist`.
4. Habla y espera a la transcripción, generación y reproducción.
5. Finaliza con la palabra de seguridad (`exit` / `--safe-word`) o con `Ctrl+C`.

## Características

| Área | Detalles |
| --- | --- |
| 🎛️ Bucle de voz | Bucle conversacional por voz con control de wake-word y safe-word |
| 🎤 Speech-to-text | Transcripción con OpenAI Whisper (`whisper-1`) |
| 🔉 Motores text-to-speech | `openai` (`tts-1`), `google` (`gtts`), `apple` (`say` CLI) |
| 🧭 Comportamiento de activación | Soporte para `--wake-word` |
| 🛑 Comportamiento de salida | Finalización mediante safe-word con `--safe-word` (por defecto se sale con `EXIT`) |
| 🎧 Dispositivos de entrada | Selección explícita mediante `--input-device-name` o el selector interactivo |
| 🎚️ Ajuste de reproducción | Tasa de reproducción configurable con `--speech-rate` |
| 🧩 Ajuste de acento | `--lang` y `--tld` para acentos de Google TTS |
| ✅ Calidad | Controles Ruff + coverage en desarrollo y CI |

## Arquitectura

La ejecución principal está diseñada en capas para que cada implementación pueda reemplazarse con acoplamiento mínimo.

1. `main.py` conecta las dependencias de tiempo de ejecución.
2. `cli_parser.py` analiza opciones y construye la configuración runtime.
3. `input_devices.py` resuelve y valida las opciones de dispositivos de captura.
4. Las implementaciones concretas se seleccionan por modo:
   - `whisper_listener.py` y `speech_listener.py` para STT
   - `open_ai_text_generator.py` para generación
   - `computer_voice_responder.py` para la orquestación de respuestas y reproducción
5. `conversation.py` ejecuta la lógica del bucle, incluido wake word + safe word, reintentos y flujo de respuesta.

### Abstracciones de interfaz

- Abstracción de listener: `whisper_listener.py`, `speech_listener.py`
- Abstracción de generador de texto: `open_ai_text_generator.py` con `open_ai_client.py`
- Abstracción de responder: `computer_voice_responder.py` con clientes de OpenAI, Google y Apple
- Abstracción del parseo de opciones: parser centralizado en `cli_parser.py`

Esta separación hace que la migración de proveedor y el mocking en pruebas sea directa.

## Estructura del proyecto

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

### Componentes del flujo principal

- `main.py`: punto de entrada CLI para `gptassist`.
- `cli_parser.py`: parseo de argumentos y validación para configuración runtime.
- `conversation.py`: orquestación del ciclo grabar → transcribir → generar → hablar.
- `whisper_listener.py`: ruta principal de escucha con OpenAI Whisper.
- `speech_listener.py`: ruta de escucha alternativa.
- `open_ai_text_generator.py`: integración de chat completion y flujo de prompts/mensajes.
- `computer_voice_responder.py`: adaptador TTS y orquestación de reproducción.
- `input_devices.py`: detecta dispositivos y resuelve la selección del usuario.
- `clients/`, `bases/`, `helpers/`, `models/`, `exceptions/`: adaptadores de servicio, contratos, helpers compartidos, entidades de dominio y tipos de error explícitos.

## Requisitos previos

| Requisito | Notas |
| --- | --- |
| 🐍 Python | `>=3.9, <4.0` |
| 🔐 Clave API de OpenAI | Requerida para generar respuestas |
| 🎙️ Micrófono / dispositivo de entrada | Requerido para la captura de voz |
| 🎵 Librerías PortAudio | Requeridas para la dependencia `pyaudio` |

### Requisitos previos de macOS

Instala dependencias de PortAudio:

```bash
brew install portaudio
brew link portaudio
```

Actualiza la configuración de `pydistutils` para uso de PortAudio:

```bash
echo "[build_ext]" >> $HOME/.pydistutils.cfg
echo "include_dirs="`brew --prefix portaudio`"/include/" >> $HOME/.pydistutils.cfg
echo "library_dirs="`brew --prefix portaudio`"/lib/" >> $HOME/.pydistutils.cfg
```

### Notas para plataformas no macOS

El CI instala paquetes de sistema compatibles con Ubuntu para dependencias de audio, pero el comportamiento en tiempo real puede variar según utilidades de reproducción.

Usa `.env.example` como referencia local para las entradas de entorno requeridas.

## Instalación

### Instalar desde PyPI

```bash
pip install chatgpt-voice-assistant
```

### Instalar desde código fuente

1. Clona este repositorio.
2. Instala Poetry ([documentación oficial](https://python-poetry.org/docs/#installation) o `pip install poetry`).
3. Instala dependencias:

```bash
poetry install
```

Para dependencias de desarrollo:

```bash
poetry install --with dev
```

## Uso

Configura `OPENAI_API_KEY` antes de lanzar, o pasa la clave directamente:

```bash
export OPENAI_API_KEY=<OPEN_AI_SECRET_KEY>
gptassist

# O
gptassist --open-ai-key=<OPEN_AI_SECRET_KEY>
```

Si ejecutas desde el código fuente:

```bash
poetry run gptassist --open-ai-key=<OPEN_AI_SECRET_KEY>
```

Comienza a hablar y escucha las respuestas habladas.

Di tu palabra de seguridad (`exit` por defecto, configurable con `--safe-word`) o pulsa `Ctrl+C` para detener.

## Configuración

### Variables de entorno

| Variable | Requerida | Descripción |
| --- | --- | --- |
| `OPENAI_API_KEY` | Sí (salvo que se use `--open-ai-key`) | Credencial OpenAI usada por STT + chat + TTS opcional |

### Opciones de CLI

```text
-h, --help
    muestra este mensaje de ayuda y sale

--log-level LOG_LEVEL
    Indica si se debe registrar en nivel debug o no.

--input-device-name INPUT_DEVICE_NAME
    Nombre del dispositivo de entrada.

--lang LANG
    Idioma para escuchar al ejecutar speech to text (por ejemplo, en o fr).

--max-tokens MAX_TOKENS
    Máximo de tokens de completado OpenAI para usar en generación de texto.

--tld TLD
    Dominio de nivel superior (por ejemplo, com o com.au).

--safe-word SAFE_WORD
    Palabra que se debe pronunciar para salir de la aplicación.

--wake-word WAKE_WORD
    (Opcional) Palabra para activar una respuesta.

--open-ai-key OPEN_AI_KEY
    Requerido. Clave secreta de OpenAI (o define la variable de entorno OPENAI_API_KEY)

--open-ai-model OPEN_AI_MODEL
    Modelo de OpenAI a usar. Ver: https://platform.openai.com/docs/models/overview

--tts {apple,google,openai}
    Elige un motor de text-to-speech.

--speech-rate SPEECH_RATE
    La velocidad a la que se reproduce el habla. 1.0=normal
```

> Consejo: Mantén los valores por defecto documentados sincronizados con `cli_parser.py` en tu versión actual.

## Ejemplos

### Básico

```bash
gptassist --open-ai-key=<OPENAI_KEY>
```

### Usar una wake word

```bash
gptassist --open-ai-key=<OPENAI_KEY> --wake-word="hey assistant"
```

### Palabra de salida personalizada

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

### Configurar acento de Google

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=google --lang=en --tld=com
```

Establece `LANGUAGE` / `TOP_LEVEL_DOMAIN` para cambiar el comportamiento de configuración regional por defecto:

| Idioma | Valor |
| --- | --- |
| Francés (Francia) | `LANGUAGE=fr TOP_LEVEL_DOMAIN=fr` |

Consulta la sección de acentos localizados en la documentación de gTTS para más detalles.

### Ejemplo combinado

```bash
export OPENAI_API_KEY=sk-...
gptassist --open-ai-key=$OPENAI_API_KEY --tts=google --wake-word="hey helper" --safe-word="stop now" --speech-rate=1.1
```

## Notas de desarrollo

Consulta [DEVELOPMENT.md](./DEVELOPMENT.md) para la configuración y estándares de código.

Comandos comunes:

```bash
poetry install --with dev
poetry run pytest
poetry run coverage run --source chatgpt_voice_assistant -m pytest tests
poetry run coverage report --show-missing --fail-under=90
poetry run ruff format --check .
poetry run ruff check .
```

El flujo de CI `.github/workflows/test-application.yml` ejecuta pruebas, comprobaciones Ruff y coverage.

## Solución de problemas

### `Open AI Secret Key not specified...`

Define `OPENAI_API_KEY` o pasa `--open-ai-key`.

### No se encontraron dispositivos de entrada / se seleccionó un micrófono incorrecto

- Asegúrate de que tu micrófono esté conectado y disponible para el sistema operativo.
- Usa `--input-device-name` con el nombre exacto del dispositivo.
- Si no se indica, la aplicación te pedirá elegir entre los dispositivos de entrada detectados.

### Problemas de instalación/compilación de `pyaudio`

- Instala PortAudio (ve la sección de requisitos previos de macOS).
- Vuelve a instalar dependencias tras configurar `.pydistutils.cfg`.

### No hay reproducción de audio

- La ruta de reproducción actual usa `afplay` (comando de macOS).
- Apple TTS usa `say` y OpenAI/Google generan archivos de audio que se reproducen mediante `afplay`.
- En plataformas distintas de macOS puede requerirse adaptación adicional de reproducción.

### Modelo no disponible

Si el `--open-ai-model` predeterminado o seleccionado no está disponible, usa un modelo al que tengas acceso:

```bash
gptassist --open-ai-key=<OPENAI_KEY> --open-ai-model=<YOUR_AVAILABLE_MODEL>
```

### Fallos de API / red

- Verifica conectividad y permisos de clave API.
- Verifica que la clave sea válida para el proveedor/modelo seleccionado.
- Aumenta el nivel de logs con `--log-level DEBUG` al trazar el flujo de parser/runtime.

## Internacionalización (i18n)

- `i18n/` contiene READMEs localizados.
- Esta versión en inglés es la base canónica.
- Mantén exactamente una línea `Opciones de idioma:` cerca de la parte superior de cada variante README y evita duplicados.
- Las traducciones se agrupan por idioma y localización para coincidir con el flujo documental del repositorio.

## Roadmap

- Ampliar la documentación de comportamiento de reproducción de audio entre plataformas.
- Agregar o actualizar variantes de README multilingües en `i18n/`.
- Mantener defaults y ejemplos alineados con los nombres de modelos OpenAI recomendados actualmente.
- Mejorar la arquitectura y documentación operativa más allá de README/DEVELOPMENT.

## Contribución

1. Haz un fork del repositorio.
2. Crea una rama de funcionalidad.
3. Ejecuta pruebas y verificaciones en local:

```bash
poetry run pytest
poetry run ruff format --check .
poetry run ruff check .
```

4. Abre un pull request con una descripción clara de los cambios y cobertura de pruebas.

## ❤️ Support

| Donate | PayPal | Stripe |
| --- | --- | --- |
| [![Donate](https://camo.githubusercontent.com/24a4914f0b42c6f435f9e101621f1e52535b02c225764b2f6cc99416926004b7/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f446f6e6174652d4c617a79696e674172742d3045413545393f7374796c653d666f722d7468652d6261646765266c6f676f3d6b6f2d6669266c6f676f436f6c6f723d7768697465)](https://chat.lazying.art/donate) | [![PayPal](https://camo.githubusercontent.com/d0f57e8b016517a4b06961b24d0ca87d62fdba16e18bbdb6aba28e978dc0ea21/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f50617950616c2d526f6e677a686f754368656e2d3030343537433f7374796c653d666f722d7468652d6261646765266c6f676f3d70617970616c266c6f676f436f6c6f723d7768697465)](https://paypal.me/RongzhouChen) | [![Stripe](https://camo.githubusercontent.com/1152dfe04b6943afe3a8d2953676749603fb9f95e24088c92c97a01a897b4942/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f5374726970652d446f6e6174652d3633354246463f7374796c653d666f722d7468652d6261646765266c6f676f3d737472697065266c6f676f436f6c6f723d7768697465)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400) |

## Contact

- Reporta errores o solicita funciones: [GitHub Issues](https://github.com/lachlanchen/chatgpt-voice-assistant/issues)
- Pregunta sobre diseño o arquitectura: [GitHub Discussions](https://github.com/lachlanchen/chatgpt-voice-assistant/discussions)

## Licencia

Actualmente no hay un archivo `LICENSE` en este repositorio.

Suposición: los términos de licencia no están explícitamente declarados todavía. Añade un archivo `LICENSE` para aclarar términos de uso y distribución.

## Referencias

- [Speech Recognition library docs](https://pypi.org/project/SpeechRecognition/1.2.3)
- [Google Translate Text-to-Speech API (gTTS)](https://gtts.readthedocs.io/en/latest/module.html#)
