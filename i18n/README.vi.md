[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


[![LazyingArt banner](https://github.com/lachlanchen/lachlanchen/raw/main/figs/banner.png)](https://github.com/lachlanchen/lachlanchen/blob/main/figs/banner.png)

# Trợ lý giọng nói ChatGPT

| Thành phần | Trạng thái |
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

Ứng dụng CLI cho trò chuyện không cần dùng tay với các mô hình chat của OpenAI. Nó ghi âm giọng nói, phiên âm qua Whisper, tạo phản hồi và phát lại bằng các nhà cung cấp TTS có thể chọn.
`chatgpt-voice-assistant` giữ vòng lặp runtime gọn nhẹ và kiến trúc mô-đun.

[![Hướng dẫn cài đặt](https://img.shields.io/badge/Install-Quick_Start-0EA5E9?style=flat-square&logo=python&logoColor=white)](#bắt-đầu-nhanh)
[![Demo sử dụng](https://img.shields.io/badge/Usage-gptassist-16A34A?style=flat-square&logo=terminal&logoColor=white)](#sử-dụng)
[![Kiến trúc](https://img.shields.io/badge/Architecture-Modular-8B5CF6?style=flat-square&logo=github&logoColor=white)](#kiến-trúc)
[![Khắc phục sự cố](https://img.shields.io/badge/Troubleshoot-Common_Issues-F59E0B?style=flat-square&logo=bugatti&logoColor=white)](#khắc-phục-sự-cố)

---

## Tóm tắt nhanh

| Lớp | Mục đích |
| --- | --- |
| 🎤 Vòng lặp thoại | Dữ liệu micro được ghi nhận và phiên âm liên tục. |
| 🧠 Đường đi mô hình | OpenAI completion tạo phản hồi hội thoại. |
| 🔉 Đường đi âm thanh | Phản hồi được tổng hợp qua các nhà cung cấp TTS có thể gắn thay. |
| 🧩 Kiến trúc | Các hợp đồng Listener → generator → responder giúp thành phần dễ thay thế. |

---

| Trọng tâm | Chi tiết |
| --- | --- |
| 🎙️ Ghi âm | Chu trình tương tác điều khiển bằng micro |
| 🧠 Đầu ra mô hình | Phản hồi chat completion của OpenAI |
| 🔊 Phát giọng | Đường đi TTS đầu ra có thể thay thế |
| 🧩 Cấu trúc dự án | Các module `chatgpt_voice_assistant/` xoay quanh contracts rõ ràng |
| 🧩 Kiến trúc | Các contract listener / generator / responder theo kiểu mô-đun |

## Mục lục

- [Tùy chọn ngôn ngữ](#tùy-chọn-ngôn-ngữ)
- [Tổng quan](#tổng-quan)
- [Bắt đầu nhanh](#bắt-đầu-nhanh)
- [Tính năng](#tính-năng)
- [Kiến trúc](#kiến-trúc)
- [Cấu trúc dự án](#cấu-trúc-dự-án)
- [Yêu cầu](#yêu-cầu)
- [Cài đặt](#cài-đặt)
- [Sử dụng](#sử-dụng)
- [Cấu hình](#cấu-hình)
- [Ví dụ](#ví-dụ)
- [Ghi chú phát triển](#ghi-chú-phát-triển)
- [Khắc phục sự cố](#khắc-phục-sự-cố)
- [Quốc tế hóa (i18n)](#quốc-tế-hóa-i18n)
- [Lộ trình](#lộ-trình)
- [Đóng góp](#đóng-góp)
- [❤️ Support](#-support)
- [Contact](#contact)
- [License](#license)
- [Tham khảo](#tham-khảo)

## Tùy chọn ngôn ngữ

🌐 Language options: Tiếng Việt (bản nháp hiện tại)

## Tổng quan

`chatgpt-voice-assistant` là một ứng dụng CLI Python (`gptassist`) cho trò chuyện tay trắng với các mô hình chat của OpenAI.

Luồng tổng quan:

1. Ghi nhận đầu vào micro.
2. Phiên âm tiếng nói thành văn bản.
3. Tạo phản hồi từ mô hình.
4. Chuyển văn bản phản hồi thành giọng nói.
5. Phát âm thanh đã tạo cục bộ.

Dự án sử dụng `poetry`, dependency injection rõ ràng, và các interface có kiểu dữ liệu để giữ cho trình cung cấp giọng nói và tích hợp luôn có thể thay đổi.

### Ý đồ thiết kế

- Giữ vòng lặp runtime tối giản và dễ dự đoán.
- Giữ các tích hợp provider có thể thay thế thông qua interface rõ ràng.
- Giữ tương tác theo kiểu CLI-first với đối số tường minh và fallback dựa vào biến môi trường.

### Bắt đầu nhanh

1. Cài dependencies (chọn một phương pháp bên dưới).
2. Thiết lập `OPENAI_API_KEY`.
3. Chạy `gptassist`.
4. Nói vào micro rồi chờ quá trình phiên âm/generat/ phát lại.
5. Kết thúc bằng từ khóa an toàn (`exit` / `--safe-word`) hoặc `Ctrl+C`.

## Tính năng

| Khu vực | Chi tiết |
| --- | --- |
| 🎛️ Vòng lặp thoại | Chu trình hội thoại bằng giọng nói với kiểm soát wake-word + safe-word |
| 🎤 Nhận diện giọng nói | Phiên âm bằng OpenAI Whisper (`whisper-1`) |
| 🔉 Công cụ text-to-speech | `openai` (`tts-1`), `google` (`gtts`), `apple` (`say` CLI) |
| 🧭 Hành vi wake | Hỗ trợ `--wake-word` |
| 🛑 Hành vi thoát | Kết thúc an toàn qua `--safe-word` (mặc định thoát khi nói `EXIT`) |
| 🎧 Thiết bị đầu vào | Chọn thiết bị rõ ràng qua `--input-device-name` hoặc prompt tương tác |
| 🎚️ Điều chỉnh phát | Tinh chỉnh tốc độ phát bằng `--speech-rate` |
| 🧩 Tinh chỉnh âm sắc | `--lang` và `--tld` cho giọng Google TTS |
| ✅ Công cụ chất lượng | Ruff + coverage trong development và CI |

## Kiến trúc

Runtime lõi được thiết kế theo tầng để mỗi implementation có thể thay thế với mức ràng buộc tối thiểu.

1. `main.py` kết nối các phụ thuộc runtime.
2. `cli_parser.py` phân tích tuỳ chọn và xây dựng cấu hình runtime.
3. `input_devices.py` giải quyết và xác thực lựa chọn thiết bị thu.
4. Các implementation cụ thể được chọn theo chế độ:
   - `whisper_listener.py` và `speech_listener.py` cho STT
   - `open_ai_text_generator.py` cho generation
   - `computer_voice_responder.py` cho orchestration phản hồi và phát âm
5. `conversation.py` chạy logic vòng lặp, gồm wake word + safe word, retry, và flow phản hồi.

### Trừu tượng interface

- Listener abstraction: `whisper_listener.py`, `speech_listener.py`
- Text generator abstraction: `open_ai_text_generator.py` với `open_ai_client.py`
- Responder abstraction: `computer_voice_responder.py` với các client OpenAI, Google và Apple
- Option parsing abstraction: parser tập trung trong `cli_parser.py`

Việc tách lớp này giúp migrate provider và mock test trở nên trực tiếp.

## Cấu trúc dự án

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

### Thành phần luồng chính

- `main.py`: điểm vào CLI cho `gptassist`.
- `cli_parser.py`: parse đối số và validate cấu hình runtime.
- `conversation.py`: orchestration cho vòng `record → transcribe → generate → speak`.
- `whisper_listener.py`: đường dẫn listener chính dùng OpenAI Whisper.
- `speech_listener.py`: đường dẫn speech dự phòng.
- `open_ai_text_generator.py`: tích hợp chat completion và luồng prompt/message.
- `computer_voice_responder.py`: adapter TTS và orchestration phát âm.
- `input_devices.py`: phát hiện thiết bị và resolve lựa chọn người dùng.
- `clients/`, `bases/`, `helpers/`, `models/`, `exceptions/`: adapters dịch vụ, contracts, shared helpers, domain entities và loại lỗi rõ ràng.

## Yêu cầu

| Yêu cầu | Ghi chú |
| --- | --- |
| 🐍 Python | `>=3.9, <4.0` |
| 🔐 OpenAI API key | Bắt buộc để sinh phản hồi |
| 🎙️ Microphone / thiết bị đầu vào | Bắt buộc để thu giọng nói |
| 🎵 Thư viện PortAudio | Bắt buộc cho dependency `pyaudio` |

### Yêu cầu macOS

Cài đặt PortAudio:

```bash
brew install portaudio
brew link portaudio
```

Cập nhật cấu hình `pydistutils` để dùng PortAudio:

```bash
echo "[build_ext]" >> $HOME/.pydistutils.cfg
echo "include_dirs="`brew --prefix portaudio`"/include/" >> $HOME/.pydistutils.cfg
echo "library_dirs="`brew --prefix portaudio`"/lib/" >> $HOME/.pydistutils.cfg
```

### Lưu ý ngoài macOS

CI cài các package hệ thống tương thích Ubuntu cho dependency âm thanh, nhưng hành vi runtime có thể khác tùy tiện ích phát.

Dùng `.env.example` làm tham chiếu cục bộ cho các biến môi trường bắt buộc.

## Cài đặt

### Cài đặt từ PyPI

```bash
pip install chatgpt-voice-assistant
```

### Cài đặt từ source

1. Clone repository.
2. Cài Poetry ([tài liệu chính thức](https://python-poetry.org/docs/#installation) hoặc `pip install poetry`).
3. Cài dependencies:

```bash
poetry install
```

Với dependencies phát triển:

```bash
poetry install --with dev
```

## Sử dụng

Thiết lập `OPENAI_API_KEY` trước khi khởi chạy, hoặc truyền trực tiếp khóa của bạn:

```bash
export OPENAI_API_KEY=<OPEN_AI_SECRET_KEY>
gptassist

# OR
gptassist --open-ai-key=<OPEN_AI_SECRET_KEY>
```

Nếu chạy từ source:

```bash
poetry run gptassist --open-ai-key=<OPEN_AI_SECRET_KEY>
```

Bắt đầu nói và lắng nghe phản hồi được phát ra.

Nói từ khóa an toàn (`exit` mặc định, có thể thay bằng `--safe-word`) hoặc nhấn `Ctrl+C` để dừng.

## Cấu hình

### Biến môi trường

| Biến | Bắt buộc | Mô tả |
| --- | --- | --- |
| `OPENAI_API_KEY` | Có (trừ khi dùng `--open-ai-key`) | Chứng thực OpenAI dùng cho STT + chat + TTS tùy chọn |

### Tùy chọn CLI

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

> Mẹo: Giữ mặc định đã ghi trong tài liệu đồng bộ với `cli_parser.py` trong bản checkout hiện tại.

## Ví dụ

### Cơ bản

```bash
gptassist --open-ai-key=<OPENAI_KEY>
```

### Dùng wake word

```bash
gptassist --open-ai-key=<OPENAI_KEY> --wake-word="hey assistant"
```

### Tùy chỉnh safe word

```bash
gptassist --open-ai-key=<OPENAI_KEY> --safe-word="goodbye"
```

### Chọn engine TTS

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=openai
gptassist --open-ai-key=<OPENAI_KEY> --tts=google
gptassist --open-ai-key=<OPENAI_KEY> --tts=apple
```

### Chỉ định thiết bị đầu vào theo tên

```bash
gptassist --open-ai-key=<OPENAI_KEY> --input-device-name="MacBook Pro Microphone"
```

### Cấu hình giọng Google

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=google --lang=en --tld=com
```

Đặt `LANGUAGE` / `TOP_LEVEL_DOMAIN` để thay đổi ngôn ngữ mặc định locale:

| Locale | Giá trị |
| --- | --- |
| Pháp (Pháp) | `LANGUAGE=fr TOP_LEVEL_DOMAIN=fr` |

Xem phần giọng địa phương hóa trong tài liệu gTTS để biết chi tiết.

### Ví dụ kết hợp

```bash
export OPENAI_API_KEY=sk-...
gptassist --open-ai-key=$OPENAI_API_KEY --tts=google --wake-word="hey helper" --safe-word="stop now" --speech-rate=1.1
```

## Ghi chú phát triển

Xem [DEVELOPMENT.md](./DEVELOPMENT.md) để thiết lập môi trường và chuẩn code.

Các lệnh thường dùng:

```bash
poetry install --with dev
poetry run pytest
poetry run coverage run --source chatgpt_voice_assistant -m pytest tests
poetry run coverage report --show-missing --fail-under=90
poetry run ruff format --check .
poetry run ruff check .
```

Workflow CI `.github/workflows/test-application.yml` chạy tests, Ruff check, và coverage.

## Khắc phục sự cố

### `Open AI Secret Key not specified...`

Thiết lập `OPENAI_API_KEY` hoặc truyền `--open-ai-key`.

### Không tìm thấy thiết bị đầu vào / chọn nhầm micro

- Đảm bảo micro đã kết nối và sẵn sàng cho OS.
- Dùng `--input-device-name` với đúng tên thiết bị.
- Nếu không được truyền vào, ứng dụng sẽ hiển thị prompt để bạn chọn từ danh sách thiết bị đã phát hiện.

### Lỗi cài đặt/xây dựng `pyaudio`

- Cài PortAudio (xem phần yêu cầu macOS).
- Cài lại dependencies sau khi cấu hình `.pydistutils.cfg`.

### Không có âm thanh phát

- Đường phát hiện tại dùng `afplay` (lệnh macOS).
- Apple TTS dùng `say`, OpenAI/Google tạo file âm thanh rồi phát qua `afplay`.
- Trên nền tảng không phải macOS có thể cần điều chỉnh thêm phần playback.

### Mô hình không khả dụng

Nếu mặc định hoặc `--open-ai-model` đã chọn không có sẵn, truyền một mô hình bạn có quyền truy cập:

```bash
gptassist --open-ai-key=<OPENAI_KEY> --open-ai-model=<YOUR_AVAILABLE_MODEL>
```

### Lỗi API / mạng

- Kiểm tra kết nối và quyền của API key.
- Xác minh key hợp lệ cho provider/model đã chọn.
- Tăng mức log bằng `--log-level DEBUG` khi theo dõi luồng parser/runtime.

## Quốc tế hóa (i18n)

- Thư mục `i18n/` chứa các README đã bản địa hóa.
- English draft là bản gốc chuẩn.
- Giữ đúng **một** dòng `Language options:` ở phần đầu mỗi bản dịch và tránh trùng lặp.
- Các bản dịch được gom theo ngôn ngữ và locale để khớp luồng tài liệu repo.

## Lộ trình

- Mở rộng tài liệu về hành vi phát âm thanh đa nền tảng.
- Thêm hoặc cập nhật bản README đa ngôn ngữ trong `i18n/`.
- Giữ giá trị mặc định và ví dụ đồng bộ với tên model OpenAI đang khuyến nghị.
- Cải thiện tài liệu kiến trúc và vận hành vượt khỏi phạm vi README/DEVELOPMENT.

## Đóng góp

1. Fork repository.
2. Tạo branch tính năng.
3. Chạy tests và kiểm tra cục bộ:

```bash
poetry run pytest
poetry run ruff format --check .
poetry run ruff check .
```

4. Mở pull request với mô tả rõ ràng về thay đổi và độ bao phủ test.

## ❤️ Support

| Donate | PayPal | Stripe |
| --- | --- | --- |
| [![Donate](https://camo.githubusercontent.com/24a4914f0b42c6f435f9e101621f1e52535b02c225764b2f6cc99416926004b7/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f446f6e6174652d4c617a79696e674172742d3045413545393f7374796c653d666f722d7468652d6261646765266c6f676f3d6b6f2d6669266c6f676f436f6c6f723d7768697465)](https://chat.lazying.art/donate) | [![PayPal](https://camo.githubusercontent.com/d0f57e8b016517a4b06961b24d0ca87d62fdba16e18bbdb6aba28e978dc0ea21/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f50617950616c2d526f6e677a686f754368656e2d3030343537433f7374796c653d666f722d7468652d6261646765266c6f676f3d70617970616c266c6f676f436f6c6f723d7768697465)](https://paypal.me/RongzhouChen) | [![Stripe](https://camo.githubusercontent.com/1152dfe04b6943afe3a8d2953676749603fb9f95e24088c92c97a01a897b4942/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f5374726970652d446f6e6174652d3633354246463f7374796c653d666f722d7468652d6261646765266c6f676f3d737472697065266c6f676f436f6c6f723d7768697465)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400) |

## Contact

- Báo lỗi hoặc đề xuất tính năng: [GitHub Issues](https://github.com/lachlanchen/chatgpt-voice-assistant/issues)
- Hỏi về thiết kế hoặc kiến trúc: [GitHub Discussions](https://github.com/lachlanchen/chatgpt-voice-assistant/discussions)

## License

Không có file `LICENSE` hiện đang có trong repository này.

Giả định: điều khoản cấp phép chưa được nêu rõ ràng. Hãy thêm file `LICENSE` để làm rõ điều kiện sử dụng và phân phối.

## Tham khảo

- [Speech Recognition library docs](https://pypi.org/project/SpeechRecognition/1.2.3)
- [Google Translate Text-to-Speech API (gTTS)](https://gtts.readthedocs.io/en/latest/module.html#)
