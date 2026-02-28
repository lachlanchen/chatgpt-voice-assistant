[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


# ChatGPT Voice Assistant

![GitHub Actions Build Status](https://github.com/jakecyr/openai-gpt3-chatbot/actions/workflows/test-application.yml/badge.svg)
![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?logo=python&logoColor=white)
![Poetry](https://img.shields.io/badge/Poetry-managed-60A5FA?logo=poetry&logoColor=white)
![Platform](https://img.shields.io/badge/Platform-macOS%20focused-0F172A)
![CLI](https://img.shields.io/badge/Interface-CLI-16A34A)

OpenAI ChatGPT モデル向けのシンプルなインターフェースで、入力には音声認識、出力には音声合成を利用します。
`chatgpt-voice-assistant` は音声文字起こしに OpenAI Whisper を使用し、テキスト読み上げ出力として OpenAI、Google、Apple をサポートします。

## 概要

`chatgpt-voice-assistant` は、OpenAI モデルと音声会話を行うための Python CLI アプリケーション（`gptassist`）です。

高レベルの処理フロー:

1. マイク入力を取得する。
2. 音声をテキストに文字起こしする。
3. モデル応答を生成する。
4. 応答テキストを音声に変換する。
5. 生成した音声をローカルで再生する。

このプロジェクトは Poetry ベースで、型付けされており、`tests/` のユニットテストでカバーされています。

## 機能

| 項目 | 詳細 |
| --- | --- |
| 音声ループ | 音声駆動の CLI 会話ループ |
| 音声認識 | OpenAI Whisper 文字起こし（`whisper-1`） |
| 音声合成エンジン | `openai`（OpenAI TTS）、`google`（gTTS）、`apple`（`say` CLI） |
| ウェイク動作 | `--wake-word` によるウェイクワード対応 |
| 終了動作 | `--safe-word` によるセーフワード終了（既定では `EXIT` で終了） |
| 入力デバイス | `--input-device-name` で明示選択、または対話プロンプトにフォールバック |
| 再生チューニング | `--speech-rate` で音声再生速度を設定可能 |
| アクセント調整 | Google TTS の言語/TLD によるアクセント設定 |

## プロジェクト構成

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

## 前提条件

| 要件 | 備考 |
| --- | --- |
| Python | `>=3.9, <4.0` |
| OpenAI API キー | 応答生成に必要 |
| マイク/音声入力デバイス | 音声取得に必要 |
| PortAudio ライブラリ | `pyaudio` 依存のために必要 |

### Mac の前提条件

依存関係をインストール:

```bash
brew install portaudio
brew link portaudio
```

以下を実行して、PortAudio を利用するために `pydistutils` の設定ファイルを更新します:

```bash
echo "[build_ext]" >> $HOME/.pydistutils.cfg
echo "include_dirs="`brew --prefix portaudio`"/include/" >> $HOME/.pydistutils.cfg
echo "library_dirs="`brew --prefix portaudio`"/lib/" >> $HOME/.pydistutils.cfg
```

## インストール

### PyPI からインストール

`gptassist` CLI アプリケーションをインストールするには次を実行します:

```bash
pip install chatgpt-voice-assistant
```

### ソースからインストール

1. Poetry をインストール（[公式ドキュメント](https://python-poetry.org/docs/#installation) または `pip install poetry`）
2. 依存関係をインストール:

```bash
poetry install
```

開発用依存関係を含める場合:

```bash
poetry install --with dev
```

## 使い方

実行前に `OPENAI_API_KEY` を設定するか、シークレットキーを直接渡します:

```bash
export OPENAI_API_KEY=<OPEN API SECRET KEY HERE>
gptassist

# OR

gptassist --open-ai-key=<OPEN API SECRET KEY HERE>
```

Poetry でソースからインストールした場合:

```bash
poetry run gptassist --open-ai-key=<OPEN API SECRET KEY HERE>
```

話し始めると、AI アシスタントの応答音声が再生されます（音量を上げてください）。

アプリケーションを停止するには、`exit`（または設定したセーフワード）を発話するか、ターミナルで `Ctrl+C` を押します。

## 設定

### 環境変数

| 変数 | 必須 | 説明 |
| --- | --- | --- |
| `OPENAI_API_KEY` | Yes (unless overridden) | `--open-ai-key` を指定しない場合に必要 |

### CLI オプション

以下は、利用可能なオプションを示す `gptassist` CLI のヘルプメニューです:

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

## 例

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

### 名前で入力デバイスを指定

```bash
gptassist --open-ai-key=<OPENAI_KEY> --input-device-name="MacBook Pro Microphone"
```

### 出力言語アクセントを指定（Google TTS）

既定の英語（米国）を上書きするには、`LANGUAGE` と `TOP_LEVEL_DOMAIN` の両方を指定します:

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=google --lang=en --tld=com
```

#### 言語例

| Locale | Value |
| --- | --- |
| French (France) | `LANGUAGE=fr TOP_LEVEL_DOMAIN=fr` |

詳細は gTTS ドキュメントの Localized accents セクションを参照してください。

## 開発ノート

基本的な開発セットアップは [DEVELOPMENT.md](./DEVELOPMENT.md) を参照してください。

よく使う開発コマンド:

```bash
poetry install --with dev
poetry run pytest
poetry run coverage run --source chatgpt_voice_assistant -m pytest tests
poetry run coverage report --show-missing --fail-under=90
poetry run ruff format --check .
poetry run ruff check .
```

CI ワークフロー（`.github/workflows/test-application.yml`）は、テスト、Ruff チェック、カバレッジを実行します。

## トラブルシューティング

### `Open AI Secret Key not specified...`

`OPENAI_API_KEY` を設定するか、`--open-ai-key` を渡してください。

### 入力デバイスが見つからない / 間違ったマイクが選ばれる

- マイクが接続され、OS から利用可能であることを確認してください。
- `--input-device-name` にデバイス名を正確に指定してください。
- 未指定の場合、アプリは検出した入力デバイスから選択するプロンプトを表示します。

### `pyaudio` のインストール/ビルド問題

- PortAudio がインストール済みであることを確認してください（Mac の前提条件セクション参照）。
- `.pydistutils.cfg` 設定後に依存関係のインストールを再実行してください。

### 音が再生されない

- 現在の再生経路は `afplay`（macOS コマンド）を使用します。
- Apple TTS は `say` を使い、OpenAI/Google は `afplay` で再生される音声ファイルを生成します。
- 非 macOS で実行する場合、追加の再生適応が必要になる可能性があります。

### モデルが利用できない

既定または指定した `--open-ai-model` がアカウントで利用できない場合は、アクセス可能なモデルを指定してください:

```bash
gptassist --open-ai-key=<OPENAI_KEY> --open-ai-model=<YOUR_AVAILABLE_MODEL>
```

## 国際化（i18n）

- 多言語 README バリアント用に `i18n/` が用意されています。
- このドラフトは意図的に英語のみで、正本（canonical base）の位置付けです。
- 今後の言語ファイルは 1 つずつ追加し、この正本 README の内容と同期を維持してください。
- 各 README バリアントの先頭には `Language options:` 行を必ず 1 行だけ置き、重複を避けてください。

## ロードマップ

- クロスプラットフォームの音声再生動作に関するドキュメントを拡充する。
- `i18n/` 配下に生成/保守された多言語 README バリアントを追加する。
- 既定値と例を、現在推奨される OpenAI モデル名に合わせて維持する。
- README/DEVELOPMENT の基本説明を超えるアーキテクチャ・運用ドキュメントを改善する。

## コントリビュート

1. リポジトリをフォークします。
2. 機能ブランチを作成します。
3. ローカルでテストとチェックを実行します:

```bash
poetry run pytest
poetry run ruff format --check .
poetry run ruff check .
```

4. 変更内容とテストカバレッジを明確に説明した Pull Request を作成します。

## ライセンス

このリポジトリには現在 `LICENSE` ファイルが存在しません。

想定: まだライセンス条件が明示されていません。利用・配布条件を明確にするため、`LICENSE` ファイルを追加してください。

## 参考資料

- [Speech Recognition library docs](https://pypi.org/project/SpeechRecognition/1.2.3)
- [Google Translate Text-to-Speech API (gTTS)](https://gtts.readthedocs.io/en/latest/module.html#)
