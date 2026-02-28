[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


# ChatGPT Voice Assistant

![GitHub Actions Build Status](https://github.com/jakecyr/openai-gpt3-chatbot/actions/workflows/test-application.yml/badge.svg)
![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?logo=python&logoColor=white)
![Poetry](https://img.shields.io/badge/Poetry-managed-60A5FA?logo=poetry&logoColor=white)
![Platform](https://img.shields.io/badge/Platform-macOS%20focused-0F172A)
![CLI](https://img.shields.io/badge/Interface-CLI-16A34A)

입력에는 음성-텍스트 변환을, 출력에는 텍스트-음성 변환을 사용하는 OpenAI ChatGPT 모델용 간단한 인터페이스입니다.
`chatgpt-voice-assistant`는 음성 전사를 위해 OpenAI Whisper를 사용하며, 텍스트-음성 출력으로 OpenAI, Google, Apple을 지원합니다.

## 개요

`chatgpt-voice-assistant`는 OpenAI 모델과 음성 대화를 할 수 있는 Python CLI 애플리케이션(`gptassist`)입니다.

상위 수준 동작 흐름:

1. 마이크 입력을 수집합니다.
2. 음성을 텍스트로 전사합니다.
3. 모델 응답을 생성합니다.
4. 응답 텍스트를 음성으로 변환합니다.
5. 생성된 오디오를 로컬에서 재생합니다.

이 프로젝트는 Poetry 기반이며, 타입 힌트를 사용하고 `tests/`의 단위 테스트로 검증됩니다.

## 기능

| 영역 | 세부 내용 |
| --- | --- |
| Voice loop | 음성 기반 CLI 대화 루프 |
| Speech-to-text | OpenAI Whisper 전사 (`whisper-1`) |
| Text-to-speech engines | `openai` (OpenAI TTS), `google` (gTTS), `apple` (`say` CLI) |
| Wake behavior | `--wake-word`를 통한 웨이크 워드 지원 |
| Exit behavior | `--safe-word`를 통한 세이프 워드 종료 (기본 동작은 `EXIT`에서 종료) |
| Input devices | `--input-device-name`으로 명시 선택 또는 대화형 프롬프트 대체 |
| Playback tuning | `--speech-rate`를 통한 음성 재생 속도 설정 |
| Accent tuning | Google TTS 억양용 언어/TLD 설정 가능 |

## 프로젝트 구조

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

## 사전 요구사항

| 요구사항 | 참고 |
| --- | --- |
| Python | `>=3.9, <4.0` |
| OpenAI API key | 응답 생성에 필요 |
| Microphone/input audio device | 음성 캡처에 필요 |
| PortAudio libs | `pyaudio` 의존성에 필요 |

### macOS 사전 요구사항

의존성을 설치합니다:

```bash
brew install portaudio
brew link portaudio
```

다음 명령으로 PortAudio 사용을 위한 `pydistutils` 설정 파일을 업데이트합니다:

```bash
echo "[build_ext]" >> $HOME/.pydistutils.cfg
echo "include_dirs="`brew --prefix portaudio`"/include/" >> $HOME/.pydistutils.cfg
echo "library_dirs="`brew --prefix portaudio`"/lib/" >> $HOME/.pydistutils.cfg
```

## 설치

### PyPI에서 설치

`gptassist` CLI 애플리케이션을 설치하려면 다음을 실행하세요:

```bash
pip install chatgpt-voice-assistant
```

### 소스에서 설치

1. Poetry 설치([공식 문서](https://python-poetry.org/docs/#installation) 또는 `pip install poetry` 사용)
2. 의존성 설치:

```bash
poetry install
```

개발 의존성까지 설치하려면:

```bash
poetry install --with dev
```

## 사용법

실행 전에 `OPENAI_API_KEY`를 설정하거나, 비밀 키를 직접 전달하세요:

```bash
export OPENAI_API_KEY=<OPEN API SECRET KEY HERE>
gptassist

# OR

gptassist --open-ai-key=<OPEN API SECRET KEY HERE>
```

Poetry로 소스 설치한 경우:

```bash
poetry run gptassist --open-ai-key=<OPEN API SECRET KEY HERE>
```

말하기를 시작하고 볼륨을 올리면 AI 어시스턴트의 응답을 들을 수 있습니다.

애플리케이션을 종료하려면 `exit`(또는 설정한 세이프 워드)를 말하거나 터미널에서 `Ctrl+C`를 누르세요.

## 설정

### 환경 변수

| 변수 | 필수 | 설명 |
| --- | --- | --- |
| `OPENAI_API_KEY` | 예 (재정의되지 않은 경우) | `--open-ai-key`를 제공하지 않는 한 필수 |

### CLI 옵션

아래는 사용 가능한 옵션을 보여주는 `gptassist` CLI의 도움말 메뉴입니다:

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

## 예시

### 기본

```bash
gptassist --open-ai-key=<OPENAI_KEY>
```

### 웨이크 워드 사용

```bash
gptassist --open-ai-key=<OPENAI_KEY> --wake-word="hey assistant"
```

### 사용자 지정 세이프 워드

```bash
gptassist --open-ai-key=<OPENAI_KEY> --safe-word="goodbye"
```

### TTS 엔진 선택

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=openai
gptassist --open-ai-key=<OPENAI_KEY> --tts=google
gptassist --open-ai-key=<OPENAI_KEY> --tts=apple
```

### 이름으로 입력 장치 지정

```bash
gptassist --open-ai-key=<OPENAI_KEY> --input-device-name="MacBook Pro Microphone"
```

### 출력 언어 억양 지정 (Google TTS)

기본 영어(미국)를 덮어쓰려면 `LANGUAGE`와 `TOP_LEVEL_DOMAIN` 변수를 모두 지정하세요:

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=google --lang=en --tld=com
```

#### 언어 예시

| 로케일 | 값 |
| --- | --- |
| 영어 (미국) _DEFAULT_ | `LANGUAGE=en TOP_LEVEL_DOMAIN=com` |
| 영어 (호주) | `LANGUAGE=en TOP_LEVEL_DOMAIN=com.au` |
| 영어 (인도) | `LANGUAGE=en TOP_LEVEL_DOMAIN=co.in` |
| 프랑스어 (프랑스) | `LANGUAGE=fr TOP_LEVEL_DOMAIN=fr` |

자세한 내용은 gTTS 문서의 Localized accents 섹션을 참고하세요.

## 개발 노트

기본 개발 설정은 [DEVELOPMENT.md](./DEVELOPMENT.md)를 참고하세요.

자주 사용하는 개발 명령어:

```bash
poetry install --with dev
poetry run pytest
poetry run coverage run --source chatgpt_voice_assistant -m pytest tests
poetry run coverage report --show-missing --fail-under=90
poetry run ruff format --check .
poetry run ruff check .
```

CI 워크플로(`.github/workflows/test-application.yml`)는 테스트, Ruff 검사, 커버리지를 실행합니다.
