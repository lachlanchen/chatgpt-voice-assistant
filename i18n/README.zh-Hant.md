[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


[![LazyingArt banner](https://github.com/lachlanchen/lachlanchen/raw/main/figs/banner.png)](https://github.com/lachlanchen/lachlanchen/blob/main/figs/banner.png)

# ChatGPT Voice Assistant

| 元件 | 狀態 |
| --- | --- |
| 🧪 CI | ![CI](https://img.shields.io/badge/CI-configured-0EA5E9?style=flat-square&logo=githubactions&logoColor=white) |
| 🐍 Python | ![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?logo=python&logoColor=white) |
| 📦 打包 | ![Poetry](https://img.shields.io/badge/Poetry-managed-60A5FA?logo=poetry&logoColor=white) |
| 🍎 平台 | ![Platform](https://img.shields.io/badge/Platform-macOS%20first-0F172A) |
| 🖥️ 介面 | ![CLI](https://img.shields.io/badge/Interface-CLI-16A34A) |
| 🤝 貢獻 | ![Contributing](https://img.shields.io/badge/Contributing-Welcome-22C55E?style=flat-square&logo=github&logoColor=white) |

![Workflow status](https://img.shields.io/github/actions/workflow/status/lachlanchen/chatgpt-voice-assistant/test-application.yml?branch=main&style=flat-square&logo=githubactions&logoColor=white)
[![Open Issues](https://img.shields.io/github/issues/lachlanchen/chatgpt-voice-assistant?style=flat-square&logo=github&logoColor=white)](https://github.com/lachlanchen/chatgpt-voice-assistant/issues)
[![Stars](https://img.shields.io/github/stars/lachlanchen/chatgpt-voice-assistant?style=flat-square&logo=github&logoColor=white)](https://github.com/lachlanchen/chatgpt-voice-assistant/stargazers)
[![Last commit](https://img.shields.io/github/last-commit/lachlanchen/chatgpt-voice-assistant?style=flat-square&logo=git&logoColor=white)](https://github.com/lachlanchen/chatgpt-voice-assistant/commits/main)

這是一個讓你可使用 OpenAI 對話模型進行免操作對話的 CLI 應用。它會錄音，透過 Whisper 轉錄語音，產生回覆，並透過可選的 TTS 提供者播放合成語音。
`chatgpt-voice-assistant` 讓執行時循環保持精簡，同時確保架構可模組化、可替換。

[![安裝指南](https://img.shields.io/badge/Install-Quick_Start-0EA5E9?style=flat-square&logo=python&logoColor=white)](#快速開始)
[![使用範例](https://img.shields.io/badge/Usage-gptassist-16A34A?style=flat-square&logo=terminal&logoColor=white)](#使用)
[![架構](https://img.shields.io/badge/Architecture-Modular-8B5CF6?style=flat-square&logo=github&logoColor=white)](#架構)
[![故障排解](https://img.shields.io/badge/Troubleshoot-Common_Issues-F59E0B?style=flat-square&logo=bugatti&logoColor=white)](#故障排解)

---

## 概覽

| 層級 | 用途 |
| --- | --- |
| 🎤 語音迴圈 | 麥克風輸入會被持續擷取並轉錄。 |
| 🧠 模型路徑 | OpenAI 補齊會產生對話式回覆。 |
| 🔉 語音路徑 | 透過可插拔的 TTS 提供者合成回應。 |
| 🧩 架構 | Listener → Generator → Responder 的合約讓各元件可互換。 |

---

| 重點 | 說明 |
| --- | --- |
| 🎙️ 語音擷取 | 以麥克風為驅動的互動迴圈 |
| 🧠 模型輸出 | OpenAI 聊天補齊回覆 |
| 🔊 語音播放 | 可插拔的 TTS 輸出鏈路 |
| 🧩 專案結構 | `chatgpt_voice_assistant/` 模組採用明確合約組織 |
| 🧩 架構 | 模組化 Listener / Generator / Responder 合約 |

## 目錄

- [語言選項](#語言選項)
- [概覽](#概覽)
- [快速開始](#快速開始)
- [功能](#功能)
- [架構](#架構)
- [專案結構](#專案結構)
- [先決條件](#先決條件)
- [安裝](#安裝)
- [使用](#使用)
- [組態](#組態)
- [範例](#範例)
- [開發說明](#開發說明)
- [疑難排解](#疑難排解)
- [國際化（i18n）](#國際化i18n)
- [發展藍圖](#發展藍圖)
- [貢獻](#貢獻)
- [❤️ Support](#-support)
- [聯絡](#聯絡)
- [授權條款](#授權條款)
- [參考資料](#參考資料)

## 語言選項

🌐 語言選項：中文（繁體）（目前草稿）

## 概覽

`chatgpt-voice-assistant` 是一個 Python CLI 應用程式（`gptassist`），可讓你與 OpenAI 對話模型進行免操作對話。

高階流程如下：

1. 擷取麥克風輸入。
2. 將語音轉為文字。
3. 產生模型回覆。
4. 將回覆文字轉為語音。
5. 在本機播放生成的音訊。

專案使用 `poetry`、明確的依賴注入與型別化介面，讓語音提供者與整合可替換。

### 設計意圖

- 讓執行時循環保持精簡且可預測。
- 透過明確介面讓提供者整合可交換。
- 保持 CLI 優先互動，並以明確參數與環境變數作為 fallback。

### 快速開始

1. 安裝相依套件（擇一）。
2. 設定 `OPENAI_API_KEY`。
3. 執行 `gptassist`。
4. 開口說話，等待轉錄 / 生成 / 播放。
5. 以安全詞（預設 `exit`，可用 `--safe-word` 設定）或 `Ctrl+C` 結束。

## 功能

| 領域 | 說明 |
| --- | --- |
| 🎛️ 語音迴圈 | 具備喚醒詞與安全詞控制的語音對話循環 |
| 🎤 文字轉語音 | OpenAI Whisper 轉錄（`whisper-1`） |
| 🔉 TTS 引擎 | `openai`（`tts-1`）、`google`（`gtts`）、`apple`（`say` CLI） |
| 🧭 喚醒行為 | 支援 `--wake-word` |
| 🛑 離開行為 | 透過 `--safe-word` 結束程式（預設行為在 `EXIT` 時停止） |
| 🎧 輸入裝置 | 使用 `--input-device-name` 或互動式提示明確選擇裝置 |
| 🎚️ 播放微調 | 透過 `--speech-rate` 調整播放速率 |
| 🧩 口音調整 | `--lang` 與 `--tld` 用於 Google TTS 口音 |
| ✅ 品質工具 | 開發與 CI 均使用 Ruff 與 coverage 檢查 |

## 架構

核心運行時採用分層設計，讓每個實作都能在低耦合下替換。

1. `main.py` 組裝運行時依賴。
2. `cli_parser.py` 解析參數並建立 runtime 設定。
3. `input_devices.py` 解析並驗證錄音設備。
4. 依模式選取具體實作：
   - `whisper_listener.py` 與 `speech_listener.py`（用於 STT）
   - `open_ai_text_generator.py`（用於生成）
   - `computer_voice_responder.py`（回應協調與播放）
5. `conversation.py` 負責迴圈邏輯，含喚醒詞／安全詞、重試與回應流程。

### 介面抽象

- Listener 抽象：`whisper_listener.py`、`speech_listener.py`
- 文字生成抽象：`open_ai_text_generator.py` 搭配 `open_ai_client.py`
- Responder 抽象：`computer_voice_responder.py` 與 OpenAI、Google、Apple 的客戶端
- 參數解析抽象：集中式解析器 `cli_parser.py`

這種拆分讓提供者切換與測試 mock 都很直接。

## 專案結構

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

### 核心流程元件

- `main.py`：`gptassist` 的 CLI 入口。
- `cli_parser.py`：執行階段設定的參數解析與驗證。
- `conversation.py`：協調「錄音 → 轉錄 → 生成 → 語音回覆」迴圈。
- `whisper_listener.py`：OpenAI Whisper 的主要監聽路徑。
- `speech_listener.py`：備用語音路徑。
- `open_ai_text_generator.py`：聊天補齊整合與提示詞／訊息流程。
- `computer_voice_responder.py`：TTS 適配器與播放流程編排。
- `input_devices.py`：列舉設備並解析使用者選擇。
- `clients/`、`bases/`、`helpers/`、`models/`、`exceptions/`：服務轉接器、契約、共用工具、領域實體與明確錯誤型別。

## 先決條件

| 要求 | 說明 |
| --- | --- |
| 🐍 Python | `>=3.9, <4.0` |
| 🔐 OpenAI API 金鑰 | 產生回覆所必需 |
| 🎙️ 麥克風 / 輸入裝置 | 進行語音擷取所必需 |
| 🎵 PortAudio 函式庫 | `pyaudio` 相依性所需 |

### macOS 先決條件

安裝 PortAudio 相依套件：

```bash
brew install portaudio
brew link portaudio
```

更新 PortAudio 的 `pydistutils` 設定：

```bash
echo "[build_ext]" >> $HOME/.pydistutils.cfg
echo "include_dirs="`brew --prefix portaudio`"/include/" >> $HOME/.pydistutils.cfg
echo "library_dirs="`brew --prefix portaudio`"/lib/" >> $HOME/.pydistutils.cfg
```

### 非 macOS 說明

CI 安裝 Ubuntu 相容的系統套件以處理音訊相依性，但在其他作業系統上播放行為可能受工具差異影響。

請以 `.env.example` 作為本機環境輸入變數參考。

## 安裝

### 從 PyPI 安裝

```bash
pip install chatgpt-voice-assistant
```

### 從原始碼安裝

1. 複製此倉庫。
2. 安裝 Poetry（[官方文件](https://python-poetry.org/docs/#installation)或 `pip install poetry`）。
3. 安裝相依套件：

```bash
poetry install
```

若要安裝開發相依套件：

```bash
poetry install --with dev
```

## 使用

在啟動前先設定 `OPENAI_API_KEY`，或直接傳入你的金鑰：

```bash
export OPENAI_API_KEY=<OPEN_AI_SECRET_KEY>
gptassist

# 或
gptassist --open-ai-key=<OPEN_AI_SECRET_KEY>
```

如果從原始碼執行：

```bash
poetry run gptassist --open-ai-key=<OPEN_AI_SECRET_KEY>
```

開始說話並聆聽語音回應。

說出你的安全詞（預設為 `exit`，可用 `--safe-word` 設定）或按 `Ctrl+C` 停止。

## 組態

### 環境變數

| 變數 | 是否必須 | 說明 |
| --- | --- | --- |
| `OPENAI_API_KEY` | 是（未使用 `--open-ai-key` 時） | OpenAI 憑證，供 STT、聊天與可選 TTS 使用 |

### CLI 選項

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

> 小提醒：請確保目前簽出版本中的 `cli_parser.py` 所記載預設值保持同步。

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

### 設定 Google 口音輸出

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=google --lang=en --tld=com
```

將 `LANGUAGE` / `TOP_LEVEL_DOMAIN` 設定為不同組合以調整預設地區行為：

| 地區 | 值 |
| --- | --- |
| 法文（法國） | `LANGUAGE=fr TOP_LEVEL_DOMAIN=fr` |

更多細節請參考 gTTS 文件中的在地化口音章節。

### 綜合範例

```bash
export OPENAI_API_KEY=sk-...
gptassist --open-ai-key=$OPENAI_API_KEY --tts=google --wake-word="hey helper" --safe-word="stop now" --speech-rate=1.1
```

## 開發說明

請參閱 [DEVELOPMENT.md](./DEVELOPMENT.md) 了解設定與程式碼規範。

常用指令：

```bash
poetry install --with dev
poetry run pytest
poetry run coverage run --source chatgpt_voice_assistant -m pytest tests
poetry run coverage report --show-missing --fail-under=90
poetry run ruff format --check .
poetry run ruff check .
```

CI 工作流程 `.github/workflows/test-application.yml` 會執行測試、Ruff 檢查與 coverage。

## 疑難排解

### `Open AI Secret Key not specified...`

設定 `OPENAI_API_KEY`，或傳入 `--open-ai-key`。

### 找不到輸入設備 / 選錯麥克風

- 確認你的麥克風已連接，且系統可用。
- 使用 `--input-device-name`，並提供完全一致的設備名稱。
- 若未提供，應用會提示你從偵測到的輸入設備中挑選。

### `pyaudio` 安裝與建置問題

- 安裝 PortAudio（請見 macOS 先決條件）。
- 設定 `.pydistutils.cfg` 後，重新執行相依套件安裝。

### 無法播放聲音

- 目前播放流程使用 `afplay`（macOS 指令）。
- Apple TTS 使用 `say`，OpenAI/Google 會先產生音訊檔再透過 `afplay` 播放。
- 在非 macOS 平台上，可能需要額外的播放適配處理。

### 模型不存在

若預設或已選 `--open-ai-model` 無法使用，請改用可存取的模型：

```bash
gptassist --open-ai-key=<OPENAI_KEY> --open-ai-model=<YOUR_AVAILABLE_MODEL>
```

### API / 網路異常

- 檢查網路連線與 API 金鑰權限。
- 確認金鑰可用於所選提供者與模型。
- 需要追蹤解析與執行流程時，請用 `--log-level DEBUG` 提升日誌層級。

## 國際化（i18n）

- `i18n/` 放置本地化的 README。
- 這份英文稿是主要版本。
- 每個 README 變體只保留一行 `Language options:`，並避免重複。
- 翻譯依語言與地區分組，與本專案文件流程一致。

## 發展藍圖

- 擴充跨平台音訊播放行為的文件。
- 在 `i18n/` 下新增或更新多語言 README。
- 讓預設值與範例與目前建議的 OpenAI 模型名稱保持一致。
- 進一步優化 README 與 DEVELOPMENT 之外的架構與營運文件。

## 貢獻

1. Fork 倉庫。
2. 建立功能分支。
3. 在本機執行測試與檢查：

```bash
poetry run pytest
poetry run ruff format --check .
poetry run ruff check .
```

4. 提出 Pull Request，清楚說明修改內容與測試覆蓋。

## 聯絡

- 回報錯誤或提出新需求：[GitHub Issues](https://github.com/lachlanchen/chatgpt-voice-assistant/issues)
- 詢問設計或架構問題：[GitHub Discussions](https://github.com/lachlanchen/chatgpt-voice-assistant/discussions)

## 授權條款

目前倉庫中尚未提供 `LICENSE` 檔案。

目前未明確宣告授權條款。建議新增 `LICENSE` 以釐清使用與發布規範。

## 參考資料

- [Speech Recognition library docs](https://pypi.org/project/SpeechRecognition/1.2.3)
- [Google Translate Text-to-Speech API (gTTS)](https://gtts.readthedocs.io/en/latest/module.html#)


## ❤️ Support

| Donate | PayPal | Stripe |
| --- | --- | --- |
| [![Donate](https://camo.githubusercontent.com/24a4914f0b42c6f435f9e101621f1e52535b02c225764b2f6cc99416926004b7/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f446f6e6174652d4c617a79696e674172742d3045413545393f7374796c653d666f722d7468652d6261646765266c6f676f3d6b6f2d6669266c6f676f436f6c6f723d7768697465)](https://chat.lazying.art/donate) | [![PayPal](https://camo.githubusercontent.com/d0f57e8b016517a4b06961b24d0ca87d62fdba16e18bbdb6aba28e978dc0ea21/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f50617950616c2d526f6e677a686f754368656e2d3030343537433f7374796c653d666f722d7468652d6261646765266c6f676f3d70617970616c266c6f676f436f6c6f723d7768697465)](https://paypal.me/RongzhouChen) | [![Stripe](https://camo.githubusercontent.com/1152dfe04b6943afe3a8d2953676749603fb9f95e24088c92c97a01a897b4942/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f5374726970652d446f6e6174652d3633354246463f7374796c653d666f722d7468652d6261646765266c6f676f3d737472697065266c6f676f436f6c6f723d7768697465)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400) |
