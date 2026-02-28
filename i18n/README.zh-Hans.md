[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


[![LazyingArt banner](https://github.com/lachlanchen/lachlanchen/raw/main/figs/banner.png)](https://github.com/lachlanchen/lachlanchen/blob/main/figs/banner.png)

# ChatGPT Voice Assistant

| 组件 | 状态 |
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

一个用于与 OpenAI 对话模型进行免手操作对话的 CLI 应用。它会录制语音，通过 Whisper 转写，生成回复，并通过可选的 TTS 提供商播放合成语音。
`chatgpt-voice-assistant` 保持运行时循环轻量，同时保持架构可插拔。

[![安装指南](https://img.shields.io/badge/Install-Quick_Start-0EA5E9?style=flat-square&logo=python&logoColor=white)](#快速开始)
[![使用示例](https://img.shields.io/badge/Usage-gptassist-16A34A?style=flat-square&logo=terminal&logoColor=white)](#使用)
[![架构](https://img.shields.io/badge/Architecture-Modular-8B5CF6?style=flat-square&logo=github&logoColor=white)](#架构)
[![故障排查](https://img.shields.io/badge/Troubleshoot-Common_Issues-F59E0B?style=flat-square&logo=bugatti&logoColor=white)](#故障排查)

---

## 概览

| 层级 | 用途 |
| --- | --- |
| 🎤 语音循环 | 麦克风输入被持续捕获并转写。 |
| 🧠 模型路径 | OpenAI 补全生成对话式回复。 |
| 🔉 语音路径 | 通过可插拔的 TTS 提供商合成回复语音。 |
| 🧩 架构 | Listener → generator → responder 的契约让各组件可替换。 |

---

| 关注点 | 说明 |
| --- | --- |
| 🎙️ 语音采集 | 麦克风驱动的交互循环 |
| 🧠 模型输出 | OpenAI 聊天补全返回文本 |
| 🔊 语音播放 | 可插拔的 TTS 输出链路 |
| 🧩 项目结构 | `chatgpt_voice_assistant/` 模块围绕显式契约组织 |
| 🧩 架构 | Listener / generator / responder 的模块化契约 |

## 目录

- [语言选项](#语言选项)
- [概览](#概览)
- [快速开始](#快速开始)
- [功能](#功能)
- [架构](#架构)
- [项目结构](#项目结构)
- [先决条件](#先决条件)
- [安装](#安装)
- [使用](#使用)
- [配置](#配置)
- [示例](#示例)
- [开发说明](#开发说明)
- [故障排查](#故障排查)
- [国际化（i18n）](#国际化i18n)
- [路线图](#路线图)
- [❤️ Support](#-support)
- [联系方式](#联系方式)
- [许可证](#许可证)
- [参考资料](#参考资料)

## 语言选项

🌐 语言选项：中文（简体）（当前草稿）

## 概览

`chatgpt-voice-assistant` 是一个用于与 OpenAI 对话模型进行免手操作交流的 Python CLI 工具（`gptassist`）。

高级流程：

1. 捕获麦克风输入。
2. 将语音转为文本。
3. 生成模型回复。
4. 将回复文本转为语音。
5. 在本地播放生成音频。

本项目使用 `poetry`、显式依赖注入和类型化接口来保持语音提供商与集成可替换。

### 设计意图

- 让运行时循环保持最小且可预测。
- 通过显式接口让提供商集成可交换。
- 保持 CLI 为主的交互方式，支持显式参数与环境变量回退。

### 快速开始

1. 安装依赖（选择以下任一方式）。
2. 设置 `OPENAI_API_KEY`。
3. 运行 `gptassist`。
4. 开始说话，等待转写/生成/播放。
5. 使用安全词（默认 `exit`，可通过 `--safe-word` 配置）或按 `Ctrl+C` 结束。

## 功能

| 领域 | 说明 |
| --- | --- |
| 🎛️ 语音循环 | 带唤醒词和安全词控制的语音对话循环 |
| 🎤 语音转文本 | OpenAI Whisper 转写（`whisper-1`） |
| 🔉 语音合成引擎 | `openai`（`tts-1`）、`google`（`gtts`）、`apple`（`say` CLI） |
| 🧭 唤醒行为 | 支持 `--wake-word` |
| 🛑 退出行为 | 通过 `--safe-word` 终止应用（默认行为在 `EXIT` 时退出） |
| 🎧 输入设备 | 通过 `--input-device-name` 或交互提示显式选择设备 |
| 🎚️ 播放调节 | 通过 `--speech-rate` 配置播放速率 |
| 🧩 语音口音调整 | `--lang` 与 `--tld` 用于 Google TTS 口音 |
| ✅ 质量工具 | 开发与 CI 使用 Ruff 与 coverage 校验 |

## 架构

核心运行时经过分层设计，每个实现都可在低耦合下替换。

1. `main.py` 组装运行时依赖。
2. `cli_parser.py` 解析参数并构建运行时配置。
3. `input_devices.py` 解析并校验采集设备。
4. 按模式选择具体实现：
   - `whisper_listener.py` 与 `speech_listener.py`（用于 STT）
   - `open_ai_text_generator.py` 用于生成
   - `computer_voice_responder.py` 用于响应编排和播放
5. `conversation.py` 运行循环逻辑，包括唤醒词 + 安全词、重试与响应流转。

### 接口抽象

- Listener 抽象：`whisper_listener.py`、`speech_listener.py`
- 文本生成抽象：`open_ai_text_generator.py` 与 `open_ai_client.py`
- Responder 抽象：`computer_voice_responder.py` 与 OpenAI、Google、Apple 客户端
- 选项解析抽象：集中式解析器 `cli_parser.py`

这种分离让提供商迁移和测试 mock 变得直接清晰。

## 项目结构

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

### 核心流程组件

- `main.py`：`gptassist` 的 CLI 入口。
- `cli_parser.py`：运行时配置的参数解析与校验。
- `conversation.py`：编排录音 → 转写 → 生成 → 语音回复循环。
- `whisper_listener.py`：基于 OpenAI Whisper 的主监听路径。
- `speech_listener.py`：备用语音路径。
- `open_ai_text_generator.py`：聊天补全集成及提示词/消息流。
- `computer_voice_responder.py`：TTS 适配器与播放编排。
- `input_devices.py`：枚举设备并解析用户选择。
- `clients/`、`bases/`、`helpers/`、`models/`、`exceptions/`：服务适配器、契约、共享工具、领域实体与显式错误类型。

## 先决条件

| 要求 | 说明 |
| --- | --- |
| 🐍 Python | `>=3.9, <4.0` |
| 🔐 OpenAI API 密钥 | 生成回复所必需 |
| 🎙️ 麦克风 / 输入设备 | 进行语音采集所必需 |
| 🎵 PortAudio 库 | `pyaudio` 依赖所需 |

### macOS 先决条件

安装 PortAudio 依赖：

```bash
brew install portaudio
brew link portaudio
```

更新 `pydistutils` 配置用于 PortAudio：

```bash
echo "[build_ext]" >> $HOME/.pydistutils.cfg
echo "include_dirs="`brew --prefix portaudio`"/include/" >> $HOME/.pydistutils.cfg
echo "library_dirs="`brew --prefix portaudio`"/lib/" >> $HOME/.pydistutils.cfg
```

### 非 macOS 说明

CI 会安装 Ubuntu 兼容的系统包来处理音频依赖，但在非 macOS 平台上，播放工具的运行行为可能因平台命令而异。

将 `.env.example` 作为本地环境变量输入参考。

## 安装

### 从 PyPI 安装

```bash
pip install chatgpt-voice-assistant
```

### 从源码安装

1. 克隆此仓库。
2. 安装 Poetry（[官方文档](https://python-poetry.org/docs/#installation) 或 `pip install poetry`）。
3. 安装依赖：

```bash
poetry install
```

开发依赖：

```bash
poetry install --with dev
```

## 使用

启动前设置 `OPENAI_API_KEY`，或直接传递你的密钥：

```bash
export OPENAI_API_KEY=<OPEN_AI_SECRET_KEY>
gptassist

# 或
gptassist --open-ai-key=<OPEN_AI_SECRET_KEY>
```

如果通过源码运行：

```bash
poetry run gptassist --open-ai-key=<OPEN_AI_SECRET_KEY>
```

开始说话，然后听取语音回复。

说出你的安全词（默认 `exit`，可通过 `--safe-word` 配置）或按 `Ctrl+C` 停止。

## 配置

### 环境变量

| 变量 | 是否必需 | 说明 |
| --- | --- | --- |
| `OPENAI_API_KEY` | 是（除非使用 `--open-ai-key`） | OpenAI 凭据，供 STT、聊天和可选的 TTS 使用 |

### CLI 选项

```text
-h, --help
    show this help message and exit

--log-level LOG_LEVEL
    是否输出 debug 级日志。

--input-device-name INPUT_DEVICE_NAME
    输入设备名称。

--lang LANG
    语音转文本时监听的语言（例如 en 或 fr）。

--max-tokens MAX_TOKENS
    用于文本生成的最大 OpenAI token 数。

--tld TLD
    顶级域名（例如 com 或 com.au）。

--safe-word SAFE_WORD
    用于退出应用的口令。

--wake-word WAKE_WORD
    （可选）触发回复的词。

--open-ai-key OPEN_AI_KEY
    必需。OpenAI 秘钥（或设置 `OPENAI_API_KEY` 环境变量）

--open-ai-model OPEN_AI_MODEL
    要使用的 OpenAI 模型。见：https://platform.openai.com/docs/models/overview

--tts {apple,google,openai}
    选择文本到语音引擎。

--speech-rate SPEECH_RATE
    语音播放速率。1.0=正常
```

> 提示：请确保当前检出的 `cli_parser.py` 中的文档默认值与实际默认值保持同步。

## 示例

### 基本

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

### 配置 Google 口音输出

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=google --lang=en --tld=com
```

设置 `LANGUAGE` / `TOP_LEVEL_DOMAIN` 可改变默认语言行为：

| 地区 | 值 |
| --- | --- |
| 法语（法国） | `LANGUAGE=fr TOP_LEVEL_DOMAIN=fr` |

更多细节可查看 gTTS 文档中的本地化口音说明。

### 组合示例

```bash
export OPENAI_API_KEY=sk-...
gptassist --open-ai-key=$OPENAI_API_KEY --tts=google --wake-word="hey helper" --safe-word="stop now" --speech-rate=1.1
```

## 开发说明

参阅 [DEVELOPMENT.md](./DEVELOPMENT.md) 了解配置和代码规范。

常用命令：

```bash
poetry install --with dev
poetry run pytest
poetry run coverage run --source chatgpt_voice_assistant -m pytest tests
poetry run coverage report --show-missing --fail-under=90
poetry run ruff format --check .
poetry run ruff check .
```

CI 工作流 `.github/workflows/test-application.yml` 会执行测试、Ruff 检查与 coverage。

## 故障排查

### `Open AI Secret Key not specified...`

设置 `OPENAI_API_KEY` 或传入 `--open-ai-key`。

### 未找到输入设备 / 选择了错误麦克风

- 确认麦克风已连接且可被系统识别。
- 使用 `--input-device-name` 并给出精确设备名称。
- 未提供时，应用会提示你从发现的输入设备中选择。

### `pyaudio` 安装/构建问题

- 安装 PortAudio（参见 macOS 先决条件）。
- 设置 `.pydistutils.cfg` 后重新安装依赖。

### 无法播放声音

- 当前播放链路使用 `afplay`（macOS 命令）。
- Apple TTS 使用 `say`，OpenAI/Google 先生成音频文件再通过 `afplay` 播放。
- 在非 macOS 平台上可能需要额外的播放适配。

### 模型不可用

如果默认或选定的 `--open-ai-model` 不可用，请切换到你可访问的模型：

```bash
gptassist --open-ai-key=<OPENAI_KEY> --open-ai-model=<YOUR_AVAILABLE_MODEL>
```

### API / 网络故障

- 检查网络连通性和 API key 权限。
- 核实该密钥对所选提供商/模型是否有效。
- 追踪解析器与运行时流程时，可用 `--log-level DEBUG` 提高日志。

## 国际化（i18n）

- `i18n/` 存放本地化 README。
- 本英文草稿是规范的基准。
- 每个 README 变体顶部保留且仅保留一行 `Language options:`，避免重复。
- 翻译按语言与区域分组，以匹配仓库文档流。

## 路线图

- 扩展跨平台音频播放行为文档说明。
- 在 `i18n/` 下新增或更新更多多语言 README。
- 保持默认值和示例与当前推荐的 OpenAI 模型名称一致。
- 在 README/DEVELOPMENT 基础上，完善架构与运行文档。

## 贡献

1. Fork 仓库。
2. 创建功能分支。
3. 在本地运行测试与检查：

```bash
poetry run pytest
poetry run ruff format --check .
poetry run ruff check .
```

4. 提交 Pull Request，清晰说明改动内容与测试覆盖。

## 联系方式

- 报告缺陷或提交功能请求：[GitHub Issues](https://github.com/lachlanchen/chatgpt-voice-assistant/issues)
- 进行设计/架构咨询：[GitHub Discussions](https://github.com/lachlanchen/chatgpt-voice-assistant/discussions)

## 许可证

当前仓库中还没有 `LICENSE` 文件。

当前未明确声明授权条款。建议添加 `LICENSE` 文件以说明使用和分发规则。

## 参考资料

- [Speech Recognition library docs](https://pypi.org/project/SpeechRecognition/1.2.3)
- [Google Translate Text-to-Speech API (gTTS)](https://gtts.readthedocs.io/en/latest/module.html#)


## ❤️ Support

| Donate | PayPal | Stripe |
| --- | --- | --- |
| [![Donate](https://camo.githubusercontent.com/24a4914f0b42c6f435f9e101621f1e52535b02c225764b2f6cc99416926004b7/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f446f6e6174652d4c617a79696e674172742d3045413545393f7374796c653d666f722d7468652d6261646765266c6f676f3d6b6f2d6669266c6f676f436f6c6f723d7768697465)](https://chat.lazying.art/donate) | [![PayPal](https://camo.githubusercontent.com/d0f57e8b016517a4b06961b24d0ca87d62fdba16e18bbdb6aba28e978dc0ea21/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f50617950616c2d526f6e677a686f754368656e2d3030343537433f7374796c653d666f722d7468652d6261646765266c6f676f3d70617970616c266c6f676f436f6c6f723d7768697465)](https://paypal.me/RongzhouChen) | [![Stripe](https://camo.githubusercontent.com/1152dfe04b6943afe3a8d2953676749603fb9f95e24088c92c97a01a897b4942/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f5374726970652d446f6e6174652d3633354246463f7374796c653d666f722d7468652d6261646765266c6f676f3d737472697065266c6f676f436f6c6f723d7768697465)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400) |
