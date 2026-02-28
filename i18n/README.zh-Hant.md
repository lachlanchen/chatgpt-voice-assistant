[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


# ChatGPT Voice Assistant

語言選項：繁體中文（目前草稿）

![GitHub Actions Build Status](https://github.com/jakecyr/openai-gpt3-chatbot/actions/workflows/test-application.yml/badge.svg)
![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?logo=python&logoColor=white)
![Poetry](https://img.shields.io/badge/Poetry-managed-60A5FA?logo=poetry&logoColor=white)
![Platform](https://img.shields.io/badge/Platform-macOS%20focused-0F172A)
![CLI](https://img.shields.io/badge/Interface-CLI-16A34A)

一個連接 OpenAI ChatGPT 模型的簡易介面，提供語音轉文字輸入與文字轉語音輸出。
`chatgpt-voice-assistant` 使用 OpenAI Whisper 進行語音轉錄，並支援 OpenAI、Google 或 Apple 的文字轉語音輸出。

## 概覽

`chatgpt-voice-assistant` 是一個 Python CLI 應用程式（`gptassist`），可用於與 OpenAI 模型進行語音對話。

高階流程：

1. 擷取麥克風輸入。
2. 將語音轉錄為文字。
3. 產生模型回覆。
4. 將回覆文字轉為語音。
5. 在本機播放產生的音訊。

此專案採用 Poetry 管理，使用型別標註，並在 `tests/` 中提供單元測試覆蓋。

## 功能

| 區域 | 說明 |
| --- | --- |
| 語音迴圈 | 以語音驅動的 CLI 對話循環 |
| 語音轉文字 | OpenAI Whisper 轉錄（`whisper-1`） |
| 文字轉語音引擎 | `openai`（OpenAI TTS）、`google`（gTTS）、`apple`（`say` CLI） |
| 喚醒行為 | 透過 `--wake-word` 支援喚醒詞 |
| 結束行為 | 透過 `--safe-word` 安全詞終止（預設在 `EXIT` 時結束） |
| 輸入裝置 | 可用 `--input-device-name` 明確指定，或退回互動式提示 |
| 播放調整 | 可用 `--speech-rate` 設定語音播放速率 |
| 口音調整 | 可設定 Google TTS 的語言與 TLD 口音 |

## 專案結構

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

## 先決條件

| 需求 | 備註 |
| --- | --- |
| Python | `>=3.9, <4.0` |
| OpenAI API key | 產生回覆所需 |
| 麥克風／音訊輸入裝置 | 語音擷取所需 |
| PortAudio 函式庫 | `pyaudio` 相依套件所需 |

### macOS 先決條件

安裝相依套件：

```bash
brew install portaudio
brew link portaudio
```

執行下列指令更新 `pydistutils` 設定檔，以便使用 PortAudio：

```bash
echo "[build_ext]" >> $HOME/.pydistutils.cfg
echo "include_dirs="`brew --prefix portaudio`"/include/" >> $HOME/.pydistutils.cfg
echo "library_dirs="`brew --prefix portaudio`"/lib/" >> $HOME/.pydistutils.cfg
```

## 安裝

### 從 PyPI 安裝

執行以下指令安裝 `gptassist` CLI 應用程式：

```bash
pip install chatgpt-voice-assistant
```

### 從原始碼安裝

1. 安裝 Poetry（[官方文件](https://python-poetry.org/docs/#installation) 或使用 `pip install poetry`）
2. 安裝相依套件：

```bash
poetry install
```

若需開發相依套件：

```bash
poetry install --with dev
```

## 使用方式

可在執行前先設定 `OPENAI_API_KEY`，或直接傳入你的密鑰：

```bash
export OPENAI_API_KEY=<OPEN API SECRET KEY HERE>
gptassist

# OR

gptassist --open-ai-key=<OPEN API SECRET KEY HERE>
```

若是透過 Poetry 從原始碼安裝：

```bash
poetry run gptassist --open-ai-key=<OPEN API SECRET KEY HERE>
```

開始說話並調高音量，即可聽到 AI 助理回應。

說出 `exit`（或你設定的安全詞），或在終端機按下 `Ctrl+C` 即可停止應用程式。

## 設定

### 環境變數

| 變數 | 必填 | 說明 |
| --- | --- | --- |
| `OPENAI_API_KEY` | 是（除非覆寫） | 除非提供 `--open-ai-key`，否則必填 |

### CLI 選項

以下為 `gptassist` CLI 的說明選單，列出可用選項：

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

## 範例

### 基本

```bash
gptassist --open-ai-key=<OPENAI_KEY>
```

### 使用喚醒詞

```bash
gptassist --open-ai-key=<OPENAI_KEY> --wake-word="hey assistant"
```

### 自訂安全詞

```bash
gptassist --open-ai-key=<OPENAI_KEY> --safe-word="goodbye"
```

### 選擇 TTS 引擎

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=openai
gptassist --open-ai-key=<OPENAI_KEY> --tts=google
gptassist --open-ai-key=<OPENAI_KEY> --tts=apple
```

### 依名稱指定輸入裝置

```bash
gptassist --open-ai-key=<OPENAI_KEY> --input-device-name="MacBook Pro Microphone"
```

### 指定輸出語言口音（Google TTS）

同時指定 `LANGUAGE` 與 `TOP_LEVEL_DOMAIN` 變數，以覆寫預設英文（美國）：

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=google --lang=en --tld=com
```

#### 語言範例

| 地區語系 | 值 |
| --- | --- |
| French (France) | `LANGUAGE=fr TOP_LEVEL_DOMAIN=fr` |

更多資訊請參考 gTTS 文件中的 Localized accents 章節。

## 開發備註

基礎開發設定請參閱 [DEVELOPMENT.md](./DEVELOPMENT.md)。

常用開發指令：

```bash
poetry install --with dev
poetry run pytest
poetry run coverage run --source chatgpt_voice_assistant -m pytest tests
poetry run coverage report --show-missing --fail-under=90
poetry run ruff format --check .
poetry run ruff check .
```

CI 工作流程（`.github/workflows/test-application.yml`）會執行測試、Ruff 檢查與 coverage。
