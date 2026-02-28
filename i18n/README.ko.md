[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


[![LazyingArt banner](https://github.com/lachlanchen/lachlanchen/raw/main/figs/banner.png)](https://github.com/lachlanchen/lachlanchen/blob/main/figs/banner.png)

# ChatGPT Voice Assistant

| 구성 요소 | 상태 |
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

OpenAI ChatGPT 모델과 손을 쓰지 않고 대화할 수 있는 CLI 애플리케이션입니다. 음성을 녹음하고 Whisper로 전사한 뒤 응답을 생성하고, 선택 가능한 TTS 제공자를 통해 합성 음성을 재생합니다.
`chatgpt-voice-assistant`는 런타임 루프를 가볍게 유지하고 아키텍처를 모듈형으로 구성했습니다.

[![Install guide](https://img.shields.io/badge/Install-Quick_Start-0EA5E9?style=flat-square&logo=python&logoColor=white)](#installation)
[![Usage demo](https://img.shields.io/badge/Usage-gptassist-16A34A?style=flat-square&logo=terminal&logoColor=white)](#usage)
[![Architecture](https://img.shields.io/badge/Architecture-Modular-8B5CF6?style=flat-square&logo=github&logoColor=white)](#architecture)
[![Troubleshoot](https://img.shields.io/badge/Troubleshoot-Common_Issues-F59E0B?style=flat-square&logo=bugatti&logoColor=white)](#troubleshooting)

---

## 한눈에 보기

| 계층 | 목적 |
| --- | --- |
| 🎤 음성 루프 | 마이크 입력이 계속 캡처되어 전사됩니다. |
| 🧠 모델 경로 | OpenAI 완성 API로 대화형 응답을 생성합니다. |
| 🔉 음성 경로 | 응답은 플러그형 TTS 제공자로 음성 합성됩니다. |
| 🧩 아키텍처 | Listener → generator → responder 계약으로 구성요소 교체가 쉬움 |

---

| 초점 | 상세 내용 |
| --- | --- |
| 🎙️ 음성 캡처 | 마이크 기반 상호작용 루프 |
| 🧠 모델 출력 | OpenAI 채팅 완성 응답 |
| 🔊 음성 재생 | 교체 가능한 TTS 출력 경로 |
| 🧩 프로젝트 구조 | `chatgpt_voice_assistant/` 모듈과 명시적 계약 |
| 🧩 아키텍처 | 모듈형 listener / generator / responder 계약 |

## 목차

- [언어 옵션](#언어-옵션)
- [개요](#개요)
- [빠른 시작](#빠른-시작)
- [기능](#기능)
- [아키텍처](#아키텍처)
- [프로젝트 구조](#프로젝트-구조)
- [사전 요구 사항](#사전-요구-사항)
- [설치](#설치)
- [사용법](#사용법)
- [설정](#설정)
- [예시](#예시)
- [개발 노트](#개발-노트)
- [문제 해결](#문제-해결)
- [국제화 (i18n)](#국제화-i18n)
- [로드맵](#로드맵)
- [기여](#기여)
- [❤️ Support](#-support)
- [연락](#연락)
- [라이선스](#라이선스)
- [참고](#참고)

## 언어 옵션

🌐 Language options: 한국어 (현재 초안)

## 개요

`chatgpt-voice-assistant`는 OpenAI 채팅 모델과 자유롭게 음성으로 대화할 수 있는 Python CLI 애플리케이션(`gptassist`)입니다.

전체 흐름은 다음과 같습니다.

1. 마이크 입력을 캡처합니다.
2. 음성을 텍스트로 전사합니다.
3. 모델 응답을 생성합니다.
4. 응답 텍스트를 음성으로 변환합니다.
5. 생성된 오디오를 로컬에서 재생합니다.

이 프로젝트는 `poetry`, 명시적 의존성 주입, 타입 힌트 인터페이스를 사용해 음성 제공자와 통합부를 쉽게 교체하도록 설계했습니다.

### 설계 의도

- 런타임 루프를 최소화하고 예측 가능하게 유지합니다.
- 명시적 인터페이스를 통해 제공자 통합을 쉽게 교체합니다.
- CLI 우선 인터페이스와 명시적 인수, 환경 변수 폴백을 유지합니다.

### 빠른 시작

1. 의존성 설치(아래 방식 중 하나 선택)
2. `OPENAI_API_KEY` 설정
3. `gptassist` 실행
4. 말하고, 전사/생성/재생 결과를 기다림
5. 안전어(`exit` / `--safe-word`) 또는 `Ctrl+C`로 종료

## 기능

| 영역 | 상세 내용 |
| --- | --- |
| 🎛️ 음성 루프 | 웨이크 워드와 안전어 제어가 있는 음성 대화 루프 |
| 🎤 Speech-to-text | OpenAI Whisper 전사 (`whisper-1`) |
| 🔉 텍스트-음성 엔진 | `openai` (`tts-1`), `google` (`gtts`), `apple` (`say` CLI) |
| 🧭 웨이크 동작 | `--wake-word` 지원 |
| 🛑 종료 동작 | `--safe-word`를 통한 종료 (기본 동작은 `EXIT`) |
| 🎧 입력 장치 | `--input-device-name` 또는 대화형 프롬프트로 장치 선택 |
| 🎚️ 재생 조정 | `--speech-rate`로 재생 속도 조절 |
| 🧩 억양 조정 | Google TTS 억양 설정용 `--lang`, `--tld` |
| ✅ 품질 도구 | 개발 및 CI에서 Ruff + coverage 검사 |

## 아키텍처

핵심 런타임은 구현체를 최소 결합으로 교체할 수 있도록 계층화되어 있습니다.

1. `main.py`가 런타임 의존성을 연결합니다.
2. `cli_parser.py`가 옵션을 파싱하고 런타임 구성을 구성합니다.
3. `input_devices.py`가 캡처 장치 선택지를 해석하고 유효성을 검증합니다.
4. 모드별로 구체 구현을 선택합니다.
   - `whisper_listener.py`와 `speech_listener.py` (STT)
   - `open_ai_text_generator.py` (생성)
   - `computer_voice_responder.py` (응답 오케스트레이션/재생)
5. `conversation.py`가 웨이크 워드/안전어, 재시도, 응답 흐름을 포함한 루프를 실행합니다.

### 인터페이스 추상화

- Listener 추상화: `whisper_listener.py`, `speech_listener.py`
- 텍스트 생성 추상화: `open_ai_text_generator.py`와 `open_ai_client.py`
- Responder 추상화: `computer_voice_responder.py`와 OpenAI, Google, Apple 클라이언트
- 옵션 파싱 추상화: `cli_parser.py`의 중앙 파서

이 분리는 제공자 변경과 테스트용 모킹을 단순하게 만듭니다.

## 프로젝트 구조

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

### 핵심 흐름 컴포넌트

- `main.py`: `gptassist`의 CLI 진입점
- `cli_parser.py`: 실행 구성의 인수 파싱 및 검증
- `conversation.py`: 녹음 → 전사 → 생성 → 재생 루프 오케스트레이션
- `whisper_listener.py`: OpenAI Whisper 기반 주 리스너 경로
- `speech_listener.py`: 대체 음성 경로
- `open_ai_text_generator.py`: 채팅 완성 통합 및 프롬프트/메시지 흐름
- `computer_voice_responder.py`: TTS 어댑터 및 재생 오케스트레이션
- `input_devices.py`: 장치 검색 및 사용자 선택 처리
- `clients/`, `bases/`, `helpers/`, `models/`, `exceptions/`: 서비스 어댑터, 계약, 공통 헬퍼, 도메인 엔티티, 명시적 오류 타입

## 사전 요구 사항

| 필요 조건 | 비고 |
| --- | --- |
| 🐍 Python | `>=3.9, <4.0` |
| 🔐 OpenAI API 키 | 응답 생성에 필요 |
| 🎙️ 마이크 / 입력 장치 | 음성 캡처에 필요 |
| 🎵 PortAudio 라이브러리 | `pyaudio` 의존성에 필요 |

### macOS 사전 요구 사항

PortAudio 의존성을 설치합니다.

```bash
brew install portaudio
brew link portaudio
```

PortAudio 사용을 위한 `pydistutils` 설정 업데이트:

```bash
echo "[build_ext]" >> $HOME/.pydistutils.cfg
echo "include_dirs="`brew --prefix portaudio`"/include/" >> $HOME/.pydistutils.cfg
echo "library_dirs="`brew --prefix portaudio`"/lib/" >> $HOME/.pydistutils.cfg
```

### 비 macOS 참고사항

CI는 Ubuntu 호환 시스템 패키지로 오디오 의존성을 설치하지만, 플랫폼에 따라 재생 유틸 동작이 달라질 수 있습니다.

필수 환경 입력값은 `.env.example`을 로컬 기준 파일로 사용하세요.

## 설치

### PyPI에서 설치

```bash
pip install chatgpt-voice-assistant
```

### 소스에서 설치

1. 이 저장소를 클론합니다.
2. Poetry를 설치합니다([공식 문서](https://python-poetry.org/docs/#installation) 또는 `pip install poetry`).
3. 의존성을 설치합니다.

```bash
poetry install
```

개발 의존성은 다음으로 설치합니다.

```bash
poetry install --with dev
```

## 사용법

실행 전에 `OPENAI_API_KEY`를 설정하거나 키를 직접 전달하세요.

```bash
export OPENAI_API_KEY=<OPEN_AI_SECRET_KEY>
gptassist

# OR
gptassist --open-ai-key=<OPEN_AI_SECRET_KEY>
```

소스에서 실행할 때:

```bash
poetry run gptassist --open-ai-key=<OPEN_AI_SECRET_KEY>
```

말한 뒤 음성 응답이 나올 때까지 기다립니다.

안전어(`exit`가 기본값이며 `--safe-word`로 변경 가능)를 말하거나 `Ctrl+C`로 종료합니다.

## 설정

### 환경 변수

| 변수 | 필수 | 설명 |
| --- | --- | --- |
| `OPENAI_API_KEY` | 예 (`--open-ai-key` 사용 시 예외) | STT + 채팅 + 선택적 TTS에서 사용하는 OpenAI 인증 정보 |

### CLI 옵션

```text
-h, --help
    이 도움말 메시지를 표시하고 종료합니다.

--log-level LOG_LEVEL
    디버그 레벨로 출력할지 여부를 지정합니다.

--input-device-name INPUT_DEVICE_NAME
    입력 장치 이름입니다.

--lang LANG
    음성 텍스트 변환 대상 언어입니다(예: en 또는 fr).

--max-tokens MAX_TOKENS
    텍스트 생성에 사용할 OpenAI completion 최대 토큰 수입니다.

--tld TLD
    상위 도메인(TLD)입니다(예: com 또는 com.au).

--safe-word SAFE_WORD
    애플리케이션 종료 시 말해야 할 단어입니다.

--wake-word WAKE_WORD
    (선택) 응답을 트리거하는 단어입니다.

--open-ai-key OPEN_AI_KEY
    필수 항목. OpenAI 비밀 키(`OPENAI_API_KEY` 환경 변수로 대체 가능)

--open-ai-model OPEN_AI_MODEL
    사용할 OpenAI 모델입니다. 참고: https://platform.openai.com/docs/models/overview

--tts {apple,google,openai}
    텍스트-음성 엔진을 선택합니다.

--speech-rate SPEECH_RATE
    음성 재생 속도입니다(1.0=기본값)
```

> Tip: 현재 체크아웃한 버전의 `cli_parser.py`에서 문서의 기본값을 동기화하세요.

## 예시

### 기본 사용

```bash
gptassist --open-ai-key=<OPENAI_KEY>
```

### 웨이크 워드 사용

```bash
gptassist --open-ai-key=<OPENAI_KEY> --wake-word="hey assistant"
```

### 사용자 지정 안전어

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

### Google 억양 출력 설정

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=google --lang=en --tld=com
```

`LANGUAGE` / `TOP_LEVEL_DOMAIN`를 설정하면 기본 로케일 동작을 바꿀 수 있습니다.

| 로케일 | 값 |
| --- | --- |
| 프랑스어 (프랑스) | `LANGUAGE=fr TOP_LEVEL_DOMAIN=fr` |

자세한 내용은 gTTS 문서의 지역별 억양 섹션을 확인하세요.

### 통합 예시

```bash
export OPENAI_API_KEY=sk-...
gptassist --open-ai-key=$OPENAI_API_KEY --tts=google --wake-word="hey helper" --safe-word="stop now" --speech-rate=1.1
```

## 개발 노트

`DEVELOPMENT.md`에서 설정과 코딩 규칙을 확인하세요.

공통 명령:

```bash
poetry install --with dev
poetry run pytest
poetry run coverage run --source chatgpt_voice_assistant -m pytest tests
poetry run coverage report --show-missing --fail-under=90
poetry run ruff format --check .
poetry run ruff check .
```

CI 워크플로우 `.github/workflows/test-application.yml`에서 테스트, Ruff 검사, coverage를 실행합니다.

## 문제 해결

### `Open AI Secret Key not specified...`

`OPENAI_API_KEY`를 설정하거나 `--open-ai-key`를 전달하세요.

### 입력 장치를 찾을 수 없거나 마이크가 잘못 선택됨

- 마이크가 연결되어 있고 OS에서 사용 가능한지 확인하세요.
- `--input-device-name`에 정확한 장치명을 지정하세요.
- 미지정이면 앱이 발견된 입력 장치 목록으로부터 선택하도록 요청합니다.

### `pyaudio` 설치/빌드 문제

- PortAudio를 설치하세요(앞의 macOS 사전 요구 사항 참조).
- `.pydistutils.cfg` 설정 후 의존성 재설치를 실행하세요.

### 소리 재생이 되지 않음

- 현재 재생 경로는 `afplay`(macOS 명령)입니다.
- Apple TTS는 `say`를 사용하고 OpenAI/Google은 `afplay`로 오디오 파일을 재생합니다.
- 비 macOS 플랫폼에서는 추가 재생 적응이 필요할 수 있습니다.

### 모델을 사용할 수 없음

기본 또는 선택한 `--open-ai-model`이 유효하지 않으면 접근 가능한 모델을 지정하세요.

```bash
gptassist --open-ai-key=<OPENAI_KEY> --open-ai-model=<YOUR_AVAILABLE_MODEL>
```

### API / 네트워크 오류

- 네트워크 연결과 API 키 권한을 확인하세요.
- 선택한 제공자/모델에서 키가 유효한지 확인하세요.
- 파서와 런타임 흐름 추적이 필요하면 `--log-level DEBUG`로 로깅 레벨을 높이세요.

## 국제화 (i18n)

- `i18n/` 안에는 지역화된 README가 들어 있습니다.
- 이 영어 원문은 기준 버전입니다.
- 각 README 변형의 상단에는 `Language options:` 라인이 정확히 하나만 있어야 하며 중복되지 않아야 합니다.
- 번역은 저장소 문서 흐름에 맞게 언어·로케일별로 그룹화됩니다.

## 로드맵

- 문서에서 플랫폼 간 오디오 재생 동작을 더 상세하게 다룹니다.
- `i18n/` 아래에서 다국어 README 변형을 추가하거나 갱신합니다.
- 기본값과 예시를 현재 권장되는 OpenAI 모델명과 맞춰 유지합니다.
- README/DEVELOPMENT를 넘어 아키텍처와 운영 문서를 개선합니다.

## 기여

1. 저장소를 포크합니다.
2. 기능 브랜치를 생성합니다.
3. 로컬에서 테스트와 검사를 실행합니다.

```bash
poetry run pytest
poetry run ruff format --check .
poetry run ruff check .
```

4. 변경 사항과 테스트 범위를 명확히 기술한 PR을 엽니다.

## 연락

- 버그 신고나 기능 요청: [GitHub Issues](https://github.com/lachlanchen/chatgpt-voice-assistant/issues)
- 설계 또는 아키텍처 질문: [GitHub Discussions](https://github.com/lachlanchen/chatgpt-voice-assistant/discussions)

## 라이선스

현재 이 저장소에는 `LICENSE` 파일이 없습니다.

가정: 사용 및 배포 조건이 아직 명시되지 않았습니다. 사용 범위를 명확히 하려면 `LICENSE` 파일을 추가하세요.

## 참고

- [Speech Recognition library docs](https://pypi.org/project/SpeechRecognition/1.2.3)
- [Google Translate Text-to-Speech API (gTTS)](https://gtts.readthedocs.io/en/latest/module.html#)


## ❤️ Support

| Donate | PayPal | Stripe |
| --- | --- | --- |
| [![Donate](https://camo.githubusercontent.com/24a4914f0b42c6f435f9e101621f1e52535b02c225764b2f6cc99416926004b7/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f446f6e6174652d4c617a79696e674172742d3045413545393f7374796c653d666f722d7468652d6261646765266c6f676f3d6b6f2d6669266c6f676f436f6c6f723d7768697465)](https://chat.lazying.art/donate) | [![PayPal](https://camo.githubusercontent.com/d0f57e8b016517a4b06961b24d0ca87d62fdba16e18bbdb6aba28e978dc0ea21/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f50617950616c2d526f6e677a686f754368656e2d3030343537433f7374796c653d666f722d7468652d6261646765266c6f676f3d70617970616c266c6f676f436f6c6f723d7768697465)](https://paypal.me/RongzhouChen) | [![Stripe](https://camo.githubusercontent.com/1152dfe04b6943afe3a8d2953676749603fb9f95e24088c92c97a01a897b4942/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f5374726970652d446f6e6174652d3633354246463f7374796c653d666f722d7468652d6261646765266c6f676f3d737472697065266c6f676f436f6c6f723d7768697465)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400) |
