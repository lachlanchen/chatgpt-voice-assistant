[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)



[![LazyingArt banner](https://github.com/lachlanchen/lachlanchen/raw/main/figs/banner.png)](https://github.com/lachlanchen/lachlanchen/blob/main/figs/banner.png)

# ChatGPT Voice Assistant

| コンポーネント | ステータス |
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

OpenAI ChatGPT モデルでのハンズフリー会話向け CLI アプリケーションです。マイクから音声を録音し、Whisper で文字起こしし、応答を生成して、選択可能な TTS エンジンで音声として再生します。
`chatgpt-voice-assistant` は実行ループをシンプルに保ち、アーキテクチャはモジュール化されています。

[![インストールガイド](https://img.shields.io/badge/Install-Quick_Start-0EA5E9?style=flat-square&logo=python&logoColor=white)](#installation)
[![使用デモ](https://img.shields.io/badge/Usage-gptassist-16A34A?style=flat-square&logo=terminal&logoColor=white)](#usage)
[![アーキテクチャ](https://img.shields.io/badge/Architecture-Modular-8B5CF6?style=flat-square&logo=github&logoColor=white)](#architecture)
[![トラブルシューティング](https://img.shields.io/badge/Troubleshoot-Common_Issues-F59E0B?style=flat-square&logo=bugatti&logoColor=white)](#troubleshooting)

---

<a id="at-a-glance"></a>
## 一目でわかる

| レイヤー | 目的 |
| --- | --- |
| 🎤 音声ループ | マイク入力を収録し、連続して文字起こしを行います。 |
| 🧠 モデル経路 | OpenAI の completion が会話的な返信を生成します。 |
| 🔉 音声経路 | 選択可能な TTS プロバイダーで音声に変換します。 |
| 🧩 アーキテクチャ | Listener → generator → responder の契約で、コンポーネント交換を前提とした設計です。 |

---

| フォーカス | 詳細 |
| --- | --- |
| 🎙️ 音声キャプチャ | マイク主導の対話ループ |
| 🧠 モデル出力 | OpenAI chat completion 応答 |
| 🔊 音声再生 | 交換可能な TTS 出力パス |
| 🧩 プロジェクト構成 | `chatgpt_voice_assistant/` の契約重視モジュール |
| 🧩 アーキテクチャ | モジュラーな listener / generator / responder 契約 |

<a id="language-options"></a>
## 目次

- [言語オプション](#language-options)
- [概要](#overview)
- [クイックスタート](#quick-start)
- [機能](#features)
- [アーキテクチャ](#architecture)
- [プロジェクト構成](#project-structure)
- [前提条件](#prerequisites)
- [インストール](#installation)
- [使い方](#usage)
- [設定](#configuration)
- [使用例](#examples)
- [開発ノート](#development-notes)
- [トラブルシューティング](#troubleshooting)
- [国際化（i18n）](#internationalization-i18n)
- [ロードマップ](#roadmap)
- [コントリビュート](#contributing)
- [❤️ Support](#-support)
- [お問い合わせ](#contact)
- [ライセンス](#license)
- [参考資料](#references)

<a id="language-options"></a>
## 言語オプション


<a id="overview"></a>
## 概要

`chatgpt-voice-assistant` は、OpenAI チャットモデルとハンズフリー対話ができる Python の CLI アプリケーション（`gptassist`）です。

全体の流れ:

1. マイク入力を取得します。
2. 音声を文字起こしします。
3. モデル応答を生成します。
4. 応答テキストを音声に変換します。
5. 生成した音声をローカルで再生します。

このプロジェクトは `poetry`、明示的な依存性注入、型付きインターフェースを使い、音声プロバイダーと統合部分を交換しやすくしています。

### 設計意図

- 実行ループを小さく、予測可能に保つ。
- 明示的なインターフェースを通じて提供元統合を入れ替え可能にする。
- CLI ファーストな操作を維持し、明示的な引数と環境変数ベースのフォールバックを持つ。

<a id="quick-start"></a>
### クイックスタート

1. 依存関係をインストールします（以下のいずれか）。
2. `OPENAI_API_KEY` を設定します。
3. `gptassist` を実行します。
4. 話しかけ、文字起こし / 生成 / 再生の完了を待ちます。
5. セーフワード（`exit` / `--safe-word`）または `Ctrl+C` で終了します。

<a id="features"></a>
## 機能

| 領域 | 詳細 |
| --- | --- |
| 🎛️ 音声ループ | ウェイクワードとセーフワードで制御される音声対話ループ |
| 🎤 音声認識 | OpenAI Whisper の文字起こし（`whisper-1`） |
| 🔉 テキスト読み上げエンジン | `openai`（`tts-1`）、`google`（`gtts`）、`apple`（`say` CLI） |
| 🧭 ウェイク挙動 | `--wake-word` 対応 |
| 🛑 終了挙動 | `--safe-word` による終了（既定では `EXIT`） |
| 🎧 入力デバイス | `--input-device-name` または対話式プロンプトでデバイスを指定 |
| 🎚️ 再生調整 | `--speech-rate` で再生速度を調整 |
| 🧩 アクセント調整 | Google TTS の `--lang` と `--tld` |
| ✅ 品質ツール | 開発と CI で Ruff + カバレッジチェックを実行 |

<a id="architecture"></a>
## アーキテクチャ

コアの実行層は意図的に分離されており、実装は最小限の結合で交換できます。

1. `main.py` がランタイム依存を接続します。
2. `cli_parser.py` がオプションを解析し実行設定を生成します。
3. `input_devices.py` が入力デバイス候補を解決・検証します。
4. 実装の具体選択はモードにより行われます。
   - STT には `whisper_listener.py` と `speech_listener.py`
   - 生成には `open_ai_text_generator.py`
   - 応答の調整と再生には `computer_voice_responder.py`
5. `conversation.py` が wake word / safe word、リトライ、応答フローを含むループを実行します。

### インターフェース抽象

- Listener 抽象: `whisper_listener.py`、`speech_listener.py`
- テキスト生成抽象: `open_ai_text_generator.py` と `open_ai_client.py`
- Responder 抽象: `computer_voice_responder.py`（OpenAI / Google / Apple クライアント）
- オプション解析抽象: `cli_parser.py` の一元パーサー

この分離により、プロバイダー移行やテスト時のモック化が容易になります。

<a id="project-structure"></a>
## プロジェクト構成

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

### コアフローコンポーネント

- `main.py`: `gptassist` の CLI エントリポイント。
- `cli_parser.py`: 実行時設定の引数解析と検証。
- `conversation.py`: 録音 → 文字起こし → 応答生成 → 話すループを制御。
- `whisper_listener.py`: OpenAI Whisper を使う主要リスナー。
- `speech_listener.py`: 代替の音声経路。
- `open_ai_text_generator.py`: chat completion 連携とプロンプト・メッセージフロー。
- `computer_voice_responder.py`: TTS アダプターと再生オーケストレーション。
- `input_devices.py`: デバイス検出とユーザー選択の解決。
- `clients/`、`bases/`、`helpers/`、`models/`、`exceptions/`: サービスアダプター、契約、共通ヘルパー、ドメインエンティティ、明示的エラー型。

<a id="prerequisites"></a>
## 前提条件

| 要件 | 補足 |
| --- | --- |
| 🐍 Python | `>=3.9, <4.0` |
| 🔐 OpenAI API キー | 応答生成に必須 |
| 🎙️ マイク / 入力デバイス | 音声キャプチャに必須 |
| 🎵 PortAudio libs | `pyaudio` 依存のために必要 |

### macOS の前提条件

PortAudio の依存をインストールします。

```bash
brew install portaudio
brew link portaudio
```

PortAudio 利用時は `pydistutils` 設定を更新します。

```bash
echo "[build_ext]" >> $HOME/.pydistutils.cfg
echo "include_dirs="`brew --prefix portaudio`"/include/" >> $HOME/.pydistutils.cfg
echo "library_dirs="`brew --prefix portaudio`"/lib/" >> $HOME/.pydistutils.cfg
```

### 非 macOS の注意点

CI は Ubuntu 互換のシステムパッケージをインストールしますが、実行時の再生ユーティリティ挙動はプラットフォームごとに異なる可能性があります。

必須の環境入力は `.env.example` をローカル参照として利用してください。

<a id="installation"></a>
## インストール

### PyPI からインストール

```bash
pip install chatgpt-voice-assistant
```

### ソースからインストール

1. このリポジトリをクローンします。
2. Poetry をインストールします（[公式ドキュメント](https://python-poetry.org/docs/#installation) または `pip install poetry`）。
3. 依存関係をインストールします。

```bash
poetry install
```

開発依存を入れる場合:

```bash
poetry install --with dev
```

<a id="usage"></a>
## 使い方

起動前に `OPENAI_API_KEY` を設定するか、キーを直接渡します。

```bash
export OPENAI_API_KEY=<OPEN_AI_SECRET_KEY>
gptassist

# OR

gptassist --open-ai-key=<OPEN_AI_SECRET_KEY>
```

ソースから実行する場合:

```bash
poetry run gptassist --open-ai-key=<OPEN_AI_SECRET_KEY>
```

話し始めると、音声応答を聞くまで待機します。

デフォルトの `exit`（`--safe-word` で変更可）を発話して終了するか、`Ctrl+C` で停止します。

<a id="configuration"></a>
## 設定

### 環境変数

| 変数 | 必須 | 説明 |
| --- | --- | --- |
| `OPENAI_API_KEY` | はい（`--open-ai-key` を使う場合は不要） | STT + chat + 任意の TTS で使用する OpenAI 認証情報 |

### CLI オプション

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

> Tip: 現在のチェックアウトで `cli_parser.py` に記載されたデフォルト値とドキュメントの既定値を一致させてください。

<a id="examples"></a>
## 使用例

### 基本

```bash
gptassist --open-ai-key=<OPENAI_KEY>
```

### ウェイクワードを使う

```bash
gptassist --open-ai-key=<OPENAI_KEY> --wake-word="hey assistant"
```

### カスタムセーフワード

```bash
gptassist --open-ai-key=<OPENAI_KEY> --safe-word="goodbye"
```

### TTS エンジンを選択

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=openai
gptassist --open-ai-key=<OPENAI_KEY> --tts=google
gptassist --open-ai-key=<OPENAI_KEY> --tts=apple
```

### デバイス名で入力デバイスを指定

```bash
gptassist --open-ai-key=<OPENAI_KEY> --input-device-name="MacBook Pro Microphone"
```

### Google のアクセント出力を指定

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=google --lang=en --tld=com
```

`LANGUAGE` / `TOP_LEVEL_DOMAIN` を設定すると、ロケール動作の既定を変更できます。

| ロケール | 値 |
| --- | --- |
| French (France) | `LANGUAGE=fr TOP_LEVEL_DOMAIN=fr` |

詳細は gTTS のローカライズ済みアクセント項目をご確認ください。

### 組み合わせ例

```bash
export OPENAI_API_KEY=sk-...
gptassist --open-ai-key=$OPENAI_API_KEY --tts=google --wake-word="hey helper" --safe-word="stop now" --speech-rate=1.1
```

<a id="development-notes"></a>
## 開発ノート

セットアップとコーディング規約は [DEVELOPMENT.md](./DEVELOPMENT.md) を参照してください。

共通コマンド:

```bash
poetry install --with dev
poetry run pytest
poetry run coverage run --source chatgpt_voice_assistant -m pytest tests
poetry run coverage report --show-missing --fail-under=90
poetry run ruff format --check .
poetry run ruff check .
```

CI ワークフロー `.github/workflows/test-application.yml` ではテスト、Ruff チェック、カバレッジを実行します。

<a id="troubleshooting"></a>
## トラブルシューティング

### `Open AI Secret Key not specified...`

`OPENAI_API_KEY` を設定するか `--open-ai-key` を渡してください。

### 入力デバイスが見つからない / 誤ったマイクが選ばれている

- マイクが接続され、OS が利用可能と認識していることを確認してください。
- `--input-device-name` で正確なデバイス名を使用します。
- 未指定の場合、アプリは検出された入力デバイスから選択を促します。

### `pyaudio` のインストール / ビルド問題

- PortAudio をインストールしてください（macOS の前提条件を参照）。
- `.pydistutils.cfg` を設定した後に依存関係インストールを再実行してください。

### 音声が再生されない

- 現在の再生経路は `afplay`（macOS コマンド）を使用します。
- Apple TTS は `say`、OpenAI/Google は `afplay` で生成音声を再生します。
- 非 macOS 環境では、再生周りの追加対応が必要な場合があります。

### モデルが利用できない

既定または選択した `--open-ai-model` が利用不可の場合は、利用可能なモデルを指定してください。

```bash
gptassist --open-ai-key=<OPENAI_KEY> --open-ai-model=<YOUR_AVAILABLE_MODEL>
```

### API / ネットワーク障害

- 通信状況と API キーの権限を確認します。
- 選択したプロバイダー/モデルでキーが有効か確認します。
- パーサーと実行フローを追跡する場合は、`--log-level DEBUG` で詳細ログを有効にします。

<a id="internationalization-i18n"></a>
## 国際化（i18n）

- `i18n/` にはローカライズ済み README が入っています。
- この英語版は基準として扱われます。
- 各 README の先頭付近に `Language options:` 行を1つだけ保持し、重複を避けてください。
- 翻訳は、このリポジトリのドキュメントの流れに合わせて言語とロケールで整理されます。

<a id="roadmap"></a>
## ロードマップ

- クロスプラットフォームの音声再生挙動のドキュメントを拡張する。
- `i18n/` 配下の多言語 README を追加・更新する。
- 推奨 OpenAI モデル名の変更に合わせ、既定値と例を最新状態に保つ。
- README / DEVELOPMENT を超える運用面の設計ドキュメントを改善する。

<a id="contributing"></a>
## コントリビュート

1. リポジトリをフォークします。
2. フィーチャーブランチを作成します。
3. ローカルでテストとチェックを実行します。

```bash
poetry run pytest
poetry run ruff format --check .
poetry run ruff check .
```

4. 変更内容とテストカバレッジを明示したプルリクエストを作成します。

<a id="-support"></a>
## ❤️ Support

| Donate | PayPal | Stripe |
| --- | --- | --- |
| [![Donate](https://camo.githubusercontent.com/24a4914f0b42c6f435f9e101621f1e52535b02c225764b2f6cc99416926004b7/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f446f6e6174652d4c617a79696e674172742d3045413545393f7374796c653d666f722d7468652d6261646765266c6f676f3d6b6f2d6669266c6f676f436f6c6f723d7768697465)](https://chat.lazying.art/donate) | [![PayPal](https://camo.githubusercontent.com/d0f57e8b016517a4b06961b24d0ca87d62fdba16e18bbdb6aba28e978dc0ea21/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f50617950616c2d526f6e677a686f754368656e2d3030343537433f7374796c653d666f722d7468652d6261646765266c6f676f3d70617970616c266c6f676f436f6c6f723d7768697465)](https://paypal.me/RongzhouChen) | [![Stripe](https://camo.githubusercontent.com/1152dfe04b6943afe3a8d2953676749603fb9f95e24088c92c97a01a897b4942/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f5374726970652d446f6e6174652d3633354246463f7374796c653d666f722d7468652d6261646765266c6f676f3d737472697065266c6f676f436f6c6f723d7768697465)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400) |

## Contact

- バグ報告や機能要望: [GitHub Issues](https://github.com/lachlanchen/chatgpt-voice-assistant/issues)
- 設計・アーキテクチャに関する質問: [GitHub Discussions](https://github.com/lachlanchen/chatgpt-voice-assistant/discussions)

<a id="license"></a>
## ライセンス

このリポジトリには現在 `LICENSE` ファイルがありません。

想定: 利用条件はまだ明示されていません。配布・利用条件を明確にするため、`LICENSE` ファイルを追加してください。

<a id="references"></a>
## 参考資料

- [Speech Recognition library docs](https://pypi.org/project/SpeechRecognition/1.2.3)
- [Google Translate Text-to-Speech API (gTTS)](https://gtts.readthedocs.io/en/latest/module.html#)
