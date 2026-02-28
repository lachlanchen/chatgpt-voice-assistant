[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


# ChatGPT Voice Assistant

![GitHub Actions Build Status](https://github.com/jakecyr/openai-gpt3-chatbot/actions/workflows/test-application.yml/badge.svg)
![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?logo=python&logoColor=white)
![Poetry](https://img.shields.io/badge/Poetry-managed-60A5FA?logo=poetry&logoColor=white)
![Platform](https://img.shields.io/badge/Platform-macOS%20focused-0F172A)
![CLI](https://img.shields.io/badge/Interface-CLI-16A34A)

Một giao diện đơn giản cho mô hình OpenAI ChatGPT với chuyển giọng nói thành văn bản để nhập liệu và chuyển văn bản thành giọng nói để xuất âm thanh.
`chatgpt-voice-assistant` sử dụng OpenAI Whisper để phiên âm giọng nói và hỗ trợ đầu ra text-to-speech từ OpenAI, Google hoặc Apple.

## Tổng quan

`chatgpt-voice-assistant` là một ứng dụng Python CLI (`gptassist`) dành cho hội thoại bằng giọng nói với các mô hình OpenAI.

Luồng xử lý cấp cao:

1. Thu âm đầu vào từ micro.
2. Phiên âm giọng nói thành văn bản.
3. Tạo phản hồi từ mô hình.
4. Chuyển văn bản phản hồi thành giọng nói.
5. Phát âm thanh đã tạo trên máy cục bộ.

Dự án này dùng Poetry, có type hints và được bao phủ bởi unit test trong `tests/`.

## Tính năng

| Hạng mục | Chi tiết |
| --- | --- |
| Vòng lặp thoại | Vòng hội thoại CLI điều khiển bằng giọng nói |
| Speech-to-text | Phiên âm OpenAI Whisper (`whisper-1`) |
| Engine text-to-speech | `openai` (OpenAI TTS), `google` (gTTS), `apple` (`say` CLI) |
| Hành vi đánh thức | Hỗ trợ từ kích hoạt qua `--wake-word` |
| Hành vi thoát | Kết thúc bằng từ an toàn qua `--safe-word` (mặc định thoát khi nói `EXIT`) |
| Thiết bị đầu vào | Chọn rõ ràng bằng `--input-device-name` hoặc quay về prompt tương tác |
| Tinh chỉnh phát âm | Có thể cấu hình tốc độ phát giọng qua `--speech-rate` |
| Tinh chỉnh giọng vùng miền | Có thể cấu hình language/TLD cho accent của Google TTS |

## Cấu trúc dự án

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

## Điều kiện tiên quyết

| Yêu cầu | Ghi chú |
| --- | --- |
| Python | `>=3.9, <4.0` |
| OpenAI API key | Bắt buộc để tạo phản hồi |
| Microphone/thiết bị âm thanh đầu vào | Cần cho việc thu giọng nói |
| Thư viện PortAudio | Bắt buộc cho dependency `pyaudio` |

### Điều kiện tiên quyết trên Mac

Cài đặt các dependency:

```bash
brew install portaudio
brew link portaudio
```

Cập nhật tệp cấu hình `pydistutils` của bạn để dùng PortAudio bằng cách chạy:

```bash
echo "[build_ext]" >> $HOME/.pydistutils.cfg
echo "include_dirs="`brew --prefix portaudio`"/include/" >> $HOME/.pydistutils.cfg
echo "library_dirs="`brew --prefix portaudio`"/lib/" >> $HOME/.pydistutils.cfg
```

## Cài đặt

### Cài từ PyPI

Chạy lệnh sau để cài ứng dụng CLI `gptassist`:

```bash
pip install chatgpt-voice-assistant
```

### Cài từ mã nguồn

1. Cài Poetry ([tài liệu chính thức](https://python-poetry.org/docs/#installation) hoặc dùng `pip install poetry`)
2. Cài các dependency:

```bash
poetry install
```

Để cài dependency cho phát triển:

```bash
poetry install --with dev
```

## Sử dụng

Hoặc thiết lập `OPENAI_API_KEY` trước khi chạy, hoặc truyền trực tiếp secret key của bạn:

```bash
export OPENAI_API_KEY=<OPEN API SECRET KEY HERE>
gptassist

# OR

gptassist --open-ai-key=<OPEN API SECRET KEY HERE>
```

Nếu cài từ mã nguồn với Poetry:

```bash
poetry run gptassist --open-ai-key=<OPEN API SECRET KEY HERE>
```

Bắt đầu nói và tăng âm lượng để nghe trợ lý AI phản hồi.

Nói từ `exit` (hoặc từ an toàn bạn đã cấu hình) hoặc nhấn `Ctrl+C` trong terminal để dừng ứng dụng.

## Cấu hình

### Biến môi trường

| Biến | Bắt buộc | Mô tả |
| --- | --- | --- |
| `OPENAI_API_KEY` | Có (trừ khi ghi đè) | Bắt buộc trừ khi đã cung cấp `--open-ai-key` |

### Tùy chọn CLI

Bên dưới là menu trợ giúp từ CLI `gptassist` mô tả các tùy chọn hiện có:

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

## Ví dụ

### Cơ bản

```bash
gptassist --open-ai-key=<OPENAI_KEY>
```

### Dùng Từ Kích Hoạt

```bash
gptassist --open-ai-key=<OPENAI_KEY> --wake-word="hey assistant"
```

### Từ An Toàn Tùy Chỉnh

```bash
gptassist --open-ai-key=<OPENAI_KEY> --safe-word="goodbye"
```

### Chọn Engine TTS

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=openai
gptassist --open-ai-key=<OPENAI_KEY> --tts=google
gptassist --open-ai-key=<OPENAI_KEY> --tts=apple
```

### Chỉ Định Thiết Bị Đầu Vào Theo Tên

```bash
gptassist --open-ai-key=<OPENAI_KEY> --input-device-name="MacBook Pro Microphone"
```

### Chỉ Định Accent Ngôn Ngữ Đầu Ra (Google TTS)

Chỉ định cả biến `LANGUAGE` và `TOP_LEVEL_DOMAIN` để ghi đè tiếng Anh mặc định (Hoa Kỳ):

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=google --lang=en --tld=com
```

#### Ví Dụ Ngôn Ngữ

| Locale | Value |
| --- | --- |
| French (France) | `LANGUAGE=fr TOP_LEVEL_DOMAIN=fr` |

Xem phần Localized accents trong tài liệu gTTS để biết thêm thông tin.

## Ghi chú phát triển

Xem [DEVELOPMENT.md](./DEVELOPMENT.md) để thiết lập môi trường phát triển cơ bản.

Các lệnh phát triển thường dùng:

```bash
poetry install --with dev
poetry run pytest
poetry run coverage run --source chatgpt_voice_assistant -m pytest tests
poetry run coverage report --show-missing --fail-under=90
poetry run ruff format --check .
poetry run ruff check .
```

Workflow CI (`.github/workflows/test-application.yml`) chạy test, kiểm tra Ruff và coverage.
