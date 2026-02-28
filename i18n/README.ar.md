[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


# مساعد ChatGPT الصوتي

![GitHub Actions Build Status](https://github.com/jakecyr/openai-gpt3-chatbot/actions/workflows/test-application.yml/badge.svg)
![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?logo=python&logoColor=white)
![Poetry](https://img.shields.io/badge/Poetry-managed-60A5FA?logo=poetry&logoColor=white)
![Platform](https://img.shields.io/badge/Platform-macOS%20focused-0F172A)
![CLI](https://img.shields.io/badge/Interface-CLI-16A34A)

واجهة بسيطة لنموذج OpenAI ChatGPT مع تحويل الكلام إلى نص للإدخال وتحويل النص إلى كلام للإخراج.
يستخدم `chatgpt-voice-assistant` نموذج OpenAI Whisper لنسخ الكلام ويدعم إخراج تحويل النص إلى كلام عبر OpenAI أو Google أو Apple.

## نظرة عامة

`chatgpt-voice-assistant` هو تطبيق Python لسطر الأوامر (`gptassist`) لإجراء محادثات صوتية مع نماذج OpenAI.

التدفق العام:

1. التقاط إدخال الميكروفون.
2. تحويل الكلام إلى نص.
3. توليد رد من النموذج.
4. تحويل نص الرد إلى كلام.
5. تشغيل الصوت المُولَّد محليًا.

هذا المشروع مبني باستخدام Poetry، ومكتوب بأنماط typed، ومغطى باختبارات وحدات ضمن `tests/`.

## الميزات

| المجال | التفاصيل |
| --- | --- |
| حلقة صوتية | حلقة محادثة عبر CLI تعتمد على الصوت |
| تحويل الكلام إلى نص | نسخ الكلام عبر OpenAI Whisper (`whisper-1`) |
| محركات تحويل النص إلى كلام | `openai` (OpenAI TTS)، `google` (gTTS)، `apple` (`say` CLI) |
| سلوك التنبيه | دعم كلمة تنبيه عبر `--wake-word` |
| سلوك الإنهاء | الإنهاء بكلمة أمان عبر `--safe-word` (السلوك الافتراضي يُنهي عند قول `EXIT`) |
| أجهزة الإدخال | اختيار صريح عبر `--input-device-name` أو الرجوع إلى موجه تفاعلي |
| ضبط التشغيل | سرعة تشغيل كلام قابلة للتهيئة عبر `--speech-rate` |
| ضبط اللكنة | لغة/TLD قابلة للتهيئة لكنات Google TTS |

## هيكل المشروع

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

## المتطلبات المسبقة

| المتطلب | ملاحظات |
| --- | --- |
| Python | `>=3.9, <4.0` |
| مفتاح OpenAI API | مطلوب لتوليد الردود |
| ميكروفون/جهاز إدخال صوتي | مطلوب لالتقاط الكلام |
| مكتبات PortAudio | مطلوبة لاعتمادية `pyaudio` |

### متطلبات macOS

ثبّت الاعتماديات:

```bash
brew install portaudio
brew link portaudio
```

حدّث ملف إعداد `pydistutils` لاستخدام PortAudio عبر تشغيل:

```bash
echo "[build_ext]" >> $HOME/.pydistutils.cfg
echo "include_dirs="`brew --prefix portaudio`"/include/" >> $HOME/.pydistutils.cfg
echo "library_dirs="`brew --prefix portaudio`"/lib/" >> $HOME/.pydistutils.cfg
```

## التثبيت

### التثبيت من PyPI

شغّل الأمر التالي لتثبيت تطبيق CLI `gptassist`:

```bash
pip install chatgpt-voice-assistant
```

### التثبيت من المصدر

1. ثبّت Poetry ([التوثيق الرسمي](https://python-poetry.org/docs/#installation) أو عبر `pip install poetry`)
2. ثبّت الاعتماديات:

```bash
poetry install
```

لاعتماديات التطوير:

```bash
poetry install --with dev
```

## الاستخدام

إما أن تضبط `OPENAI_API_KEY` قبل التشغيل، أو تمرّر مفتاحك السري مباشرة:

```bash
export OPENAI_API_KEY=<OPEN API SECRET KEY HERE>
gptassist

# OR

gptassist --open-ai-key=<OPEN API SECRET KEY HERE>
```

إذا ثبّتَّ المشروع من المصدر باستخدام Poetry:

```bash
poetry run gptassist --open-ai-key=<OPEN API SECRET KEY HERE>
```

ابدأ بالتحدث وارفع مستوى الصوت لسماع رد المساعد الذكي.

قل كلمة `exit` (أو كلمة الأمان التي ضبطتها) أو اضغط `Ctrl+C` في الطرفية لإيقاف التطبيق.

## الإعداد

### متغيرات البيئة

| المتغير | مطلوب | الوصف |
| --- | --- | --- |
| `OPENAI_API_KEY` | نعم (ما لم يتم تجاوزه) | مطلوب ما لم يتم تمرير `--open-ai-key` |

### خيارات CLI

فيما يلي قائمة المساعدة من CLI الخاص بـ `gptassist` التي توضح الخيارات المتاحة:

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

## أمثلة

### أساسي

```bash
gptassist --open-ai-key=<OPENAI_KEY>
```

### استخدام كلمة تنبيه

```bash
gptassist --open-ai-key=<OPENAI_KEY> --wake-word="hey assistant"
```

### كلمة أمان مخصّصة

```bash
gptassist --open-ai-key=<OPENAI_KEY> --safe-word="goodbye"
```

### اختيار محرك تحويل النص إلى كلام

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=openai
gptassist --open-ai-key=<OPENAI_KEY> --tts=google
gptassist --open-ai-key=<OPENAI_KEY> --tts=apple
```

### تحديد جهاز الإدخال بالاسم

```bash
gptassist --open-ai-key=<OPENAI_KEY> --input-device-name="MacBook Pro Microphone"
```

### تحديد لكنة لغة الإخراج (Google TTS)

حدّد كلًا من المتغيرين `LANGUAGE` و `TOP_LEVEL_DOMAIN` لتجاوز الإنجليزية الافتراضية (الولايات المتحدة):

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=google --lang=en --tld=com
```

#### أمثلة اللغات

| الإعداد المحلي | القيمة |
| --- | --- |
| الإنجليزية (الولايات المتحدة) _افتراضي_ | `LANGUAGE=en TOP_LEVEL_DOMAIN=com` |
| الإنجليزية (أستراليا) | `LANGUAGE=en TOP_LEVEL_DOMAIN=com.au` |
| الإنجليزية (الهند) | `LANGUAGE=en TOP_LEVEL_DOMAIN=co.in` |
| الفرنسية (فرنسا) | `LANGUAGE=fr TOP_LEVEL_DOMAIN=fr` |

راجع قسم اللكنات المحلية في توثيق gTTS لمزيد من المعلومات.

## ملاحظات التطوير

راجع [DEVELOPMENT.md](../DEVELOPMENT.md) لإعداد التطوير الأساسي.

أوامر تطوير شائعة:

```bash
poetry install --with dev
poetry run pytest
poetry run coverage run --source chatgpt_voice_assistant -m pytest tests
poetry run coverage report --show-missing --fail-under=90
poetry run ruff format --check .
poetry run ruff check .
```

يشغّل مسار CI (`.github/workflows/test-application.yml`) الاختبارات، وفحوصات Ruff، وتغطية الاختبارات.
