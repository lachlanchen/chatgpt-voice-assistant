[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


# ChatGPT Voice Assistant

![GitHub Actions Build Status](https://github.com/jakecyr/openai-gpt3-chatbot/actions/workflows/test-application.yml/badge.svg)
![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?logo=python&logoColor=white)
![Poetry](https://img.shields.io/badge/Poetry-managed-60A5FA?logo=poetry&logoColor=white)
![Platform](https://img.shields.io/badge/Platform-macOS%20focused-0F172A)
![CLI](https://img.shields.io/badge/Interface-CLI-16A34A)

一个面向 OpenAI ChatGPT 模型的简洁接口，支持语音转文本输入与文本转语音输出。
`chatgpt-voice-assistant` 使用 OpenAI Whisper 进行语音转写，并支持 OpenAI、Google 或 Apple 的文本转语音输出。

## 概览

`chatgpt-voice-assistant` 是一个用于与 OpenAI 模型进行语音对话的 Python CLI 应用（`gptassist`）。

高层流程：

1. 采集麦克风输入。
2. 将语音转写为文本。
3. 生成模型回复。
4. 将回复文本转换为语音。
5. 在本地播放生成的音频。

该项目基于 Poetry，包含类型标注，并由 `tests/` 中的单元测试覆盖。

## 功能

| 区域 | 详情 |
| --- | --- |
| Voice loop | 语音驱动的 CLI 对话循环 |
| Speech-to-text | OpenAI Whisper 转写（`whisper-1`） |
| Text-to-speech engines | `openai`（OpenAI TTS）、`google`（gTTS）、`apple`（`say` CLI） |
| Wake behavior | 通过 `--wake-word` 支持唤醒词 |
| Exit behavior | 通过 `--safe-word` 安全词结束（默认行为是在说出 `EXIT` 时退出） |
| Input devices | 可通过 `--input-device-name` 显式选择，或回退到交互式提示 |
| Playback tuning | 通过 `--speech-rate` 配置语音播放速度 |
| Accent tuning | 可配置 Google TTS 的语言/TLD 口音参数 |

## 项目结构

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

## 前置要求

| 要求 | 说明 |
| --- | --- |
| Python | `>=3.9, <4.0` |
| OpenAI API key | 生成回复所必需 |
| Microphone/input audio device | 语音采集所必需 |
| PortAudio libs | `pyaudio` 依赖所必需 |

### Mac 前置要求

安装依赖：

```bash
brew install portaudio
brew link portaudio
```

运行以下命令更新你的 `pydistutils` 配置文件以支持 PortAudio：

```bash
echo "[build_ext]" >> $HOME/.pydistutils.cfg
echo "include_dirs="`brew --prefix portaudio`"/include/" >> $HOME/.pydistutils.cfg
echo "library_dirs="`brew --prefix portaudio`"/lib/" >> $HOME/.pydistutils.cfg
```

## 安装

### 从 PyPI 安装

运行以下命令安装 `gptassist` CLI 应用：

```bash
pip install chatgpt-voice-assistant
```

### 从源码安装

1. 安装 Poetry（[官方文档](https://python-poetry.org/docs/#installation) 或使用 `pip install poetry`）
2. 安装依赖：

```bash
poetry install
```

安装开发依赖：

```bash
poetry install --with dev
```

## 使用方法

在运行前设置 `OPENAI_API_KEY`，或直接传入你的密钥：

```bash
export OPENAI_API_KEY=<OPEN API SECRET KEY HERE>
gptassist

# OR

gptassist --open-ai-key=<OPEN API SECRET KEY HERE>
```

如果你是通过源码并使用 Poetry 安装：

```bash
poetry run gptassist --open-ai-key=<OPEN API SECRET KEY HERE>
```

开始说话，并调高音量以听到 AI 助手的回复。

说出 `exit`（或你配置的安全词），或在终端中按 `Ctrl+C` 即可停止应用。

## 配置

### 环境变量

| Variable | Required | Description |
| --- | --- | --- |
| `OPENAI_API_KEY` | Yes（除非被覆盖） | 除非提供了 `--open-ai-key`，否则必填 |

### CLI 选项

以下为 `gptassist` CLI 的帮助菜单，列出了可用选项：

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

## 示例

### 基础

```bash
gptassist --open-ai-key=<OPENAI_KEY>
```

### 使用唤醒词

```bash
gptassist --open-ai-key=<OPENAI_KEY> --wake-word="hey assistant"
```

### 自定义安全词

```bash
gptassist --open-ai-key=<OPENAI_KEY> --safe-word="goodbye"
```

### 选择 TTS 引擎

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=openai
gptassist --open-ai-key=<OPENAI_KEY> --tts=google
gptassist --open-ai-key=<OPENAI_KEY> --tts=apple
```

### 按名称指定输入设备

```bash
gptassist --open-ai-key=<OPENAI_KEY> --input-device-name="MacBook Pro Microphone"
```

### 指定输出语言口音（Google TTS）

同时指定 `LANGUAGE` 和 `TOP_LEVEL_DOMAIN` 变量以覆盖默认英语（美国）：

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=google --lang=en --tld=com
```

#### 语言示例

| Locale | Value |
| --- | --- |
| French (France) | `LANGUAGE=fr TOP_LEVEL_DOMAIN=fr` |

更多信息请参见 gTTS 文档中的 Localized accents 部分。

## 开发说明

基础开发环境设置请参见 [DEVELOPMENT.md](./DEVELOPMENT.md)。

常用开发命令：

```bash
poetry install --with dev
poetry run pytest
poetry run coverage run --source chatgpt_voice_assistant -m pytest tests
poetry run coverage report --show-missing --fail-under=90
poetry run ruff format --check .
poetry run ruff check .
```

CI 工作流（`.github/workflows/test-application.yml`）会运行测试、Ruff 检查和覆盖率校验。
