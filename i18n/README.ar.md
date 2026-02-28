[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


[![LazyingArt banner](https://github.com/lachlanchen/lachlanchen/raw/main/figs/banner.png)](https://github.com/lachlanchen/lachlanchen/blob/main/figs/banner.png)

# مساعد صوتي لـ ChatGPT

| المكوّن | الحالة |
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

تطبيق CLI لإجراء محادثات مع نماذج دردشة OpenAI بدون استخدام اليدين. يقوم بتسجيل الكلام، تحويله إلى نص عبر Whisper، وتوليد الردود، ثم تشغيل الإجابات الصوتية عبر مزوّدات TTS قابلة للتبديل.
`chatgpt-voice-assistant` يحافظ على حلقة التشغيل بسيطة جدًا ويجعل البنية المعمارية معيارية.

[![Install guide](https://img.shields.io/badge/Install-Quick_Start-0EA5E9?style=flat-square&logo=python&logoColor=white)](#installation)
[![Usage demo](https://img.shields.io/badge/Usage-gptassist-16A34A?style=flat-square&logo=terminal&logoColor=white)](#usage)
[![Architecture](https://img.shields.io/badge/Architecture-Modular-8B5CF6?style=flat-square&logo=github&logoColor=white)](#architecture)
[![Troubleshoot](https://img.shields.io/badge/Troubleshoot-Common_Issues-F59E0B?style=flat-square&logo=bugatti&logoColor=white)](#troubleshooting)

---

## نظرة سريعة

| الطبقة | الهدف |
| --- | --- |
| 🎤 حلقة الصوت | يتم التقاط مدخل الميكروفون وتفريغه بشكل مستمر. |
| 🧠 مسار النموذج | مكالمات OpenAI لتوليد ردود محادثة. |
| 🔉 مسار الصوت | يُحوَّل الرد عبر مزوّدات TTS قابلة للتبديل. |
| 🧩 البنية المعمارية | عقود Listener → generator → responder تجعل المكوّنات قابلة للاستبدال. |

---

| النقاط الرئيسية | التفاصيل |
| --- | --- |
| 🎙️ التقاط الصوت | حلقة تفاعل تعتمد على الميكروفون |
| 🧠 مخرجات النموذج | ردود OpenAI chat completion |
| 🔊 تشغيل الصوت | مسار إخراج TTS قابل للتبديل |
| 🧩 هيكل المشروع | وحدات `chatgpt_voice_assistant/` حول عقود واضحة |
| 🧩 المعمارية | عقود listener / generator / responder معيارية |

## جدول المحتويات

- [خيارات اللغة](#language-options)
- [نظرة عامة](#overview)
- [البدء السريع](#quick-start)
- [المزايا](#features)
- [المعماريات](#architecture)
- [هيكل المشروع](#project-structure)
- [المتطلبات المسبقة](#prerequisites)
- [التثبيت](#installation)
- [الاستخدام](#usage)
- [الإعدادات](#configuration)
- [أمثلة](#examples)
- [ملاحظات التطوير](#development-notes)
- [استكشاف الأخطاء](#troubleshooting)
- [التدويل (i18n)](#internationalization-i18n)
- [خريطة الطريق](#roadmap)
- [المساهمة](#contributing)
- [❤️ Support](#-support)
- [التواصل](#contact)
- [الرخصة](#license)
- [المراجع](#references)

## Language options

🌐 خيارات اللغة: العربية (الصياغة الحالية)

## نظرة عامة

`chatgpt-voice-assistant` هو تطبيق CLI مكتوب بلغة Python (`gptassist`) للمحادثات بدون استخدام اليدين مع نماذج دردشة OpenAI.

مسار العمل العام:

1. التقاط مدخل الميكروفون.
2. تحويل الكلام إلى نص.
3. توليد رد النموذج.
4. تحويل النص إلى كلام.
5. تشغيل الصوت المنتج محليًا.

المشروع يستخدم `poetry`، وحقن التبعيات الصريح، وواجهات مكتوبة بأنواع بيانات ثابتة لجعل مزودي الصوت والتكاملات قابلة للاستبدال.

### نية التصميم

- الحفاظ على حلقة التشغيل بسيطة ومتوقعة.
- جعل تكامل المزوّدات قابلاً للتبديل عبر واجهات صريحة.
- اعتماد تفاعل CLI أولًا مع وسيطات واضحة وبديل قائم على المتغيرات البيئية.

### البداية السريعة

1. تثبيت التبعيات (اختر أحد الطرق).
2. تعيين `OPENAI_API_KEY`.
3. تشغيل `gptassist`.
4. تحدث، ثم انتظر التفريغ/التوليد/التشغيل.
5. إنهاء الجلسة بكلمة الأمان (`exit` / `--safe-word`) أو `Ctrl+C`.

## المزايا

| المجال | التفاصيل |
| --- | --- |
| 🎛️ حلقة الصوت | محادثة بقيادة الصوت مع تحكم wake-word + safe-word |
| 🎤 Speech-to-text | تفريغ OpenAI Whisper (`whisper-1`) |
| 🔉 محركات Text-to-Speech | `openai` (`tts-1`)، `google` (`gtts`)، `apple` (`say` CLI) |
| 🧭 سلوك الاستيقاظ | دعم `--wake-word` |
| 🛑 سلوك الإيقاف | إنهاء آمن عبر `--safe-word` (السلوك الافتراضي يخرج على `EXIT`) |
| 🎧 أجهزة الإدخال | اختيار صريح عبر `--input-device-name` أو مطالبة تفاعلية |
| 🎚️ ضبط التشغيل | معدل تشغيل قابل للضبط عبر `--speech-rate` |
| 🧩 ضبط اللهجة | `--lang` و`--tld` للهجات Google TTS |
| ✅ أدوات الجودة | فحص Ruff + coverage في التطوير وCI |

## المعمارية

تم تصميم زمن تشغيل النواة بشكل طبقي بحيث يمكن استبدال كل تنفيذ بتراكيب ارتباط محدودة.

1. `main.py` يربط تبعيات زمن التشغيل.
2. `cli_parser.py` يحلل الخيارات ويبني إعدادات التشغيل.
3. `input_devices.py` يحدد ويتحقق من خيارات جهاز الالتقاط.
4. يتم اختيار التطبيقات التنفيذية حسب النمط:
   - `whisper_listener.py` و`speech_listener.py` للتحويل الصوتي
   - `open_ai_text_generator.py` للتوليد
   - `computer_voice_responder.py` لتنظيم الرد والتشغيل
5. `conversation.py` يدير منطق الحلقة، بما في ذلك wake word + safe word وإعادة المحاولة وتدفق الاستجابة.

### تجريدات الواجهة

- تجريد Listener: `whisper_listener.py`، `speech_listener.py`
- تجريد مولّد النص: `open_ai_text_generator.py` مع `open_ai_client.py`
- تجريد Responder: `computer_voice_responder.py` مع عملاء OpenAI وGoogle وApple
- تجريد تحليل الخيارات: parser مركزي في `cli_parser.py`

هذا الفصل يجعل ترحيل المزوّدات والاختبارات الوهمية (mocking) مباشرًا.

## هيكل المشروع

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

### مكونات المسار الأساسي

- `main.py`: نقطة دخول CLI لـ `gptassist`.
- `cli_parser.py`: تحليل وتهيئة الخيارات والتحقق من إعدادات زمن التشغيل.
- `conversation.py`: تنسيق حلقة التسجيل → التفريغ → التوليد → النطق.
- `whisper_listener.py`: المسار الرئيسي للـ listener باستخدام OpenAI Whisper.
- `speech_listener.py`: مسار كلام بديل.
- `open_ai_text_generator.py`: تكامل إكمالات المحادثة وتدفق الرسائل.
- `computer_voice_responder.py`: موصل TTS وتنظيم التشغيل.
- `input_devices.py`: يكتشف الأجهزة ويحوّل اختيار المستخدم.
- `clients/`، `bases/`، `helpers/`، `models/`، `exceptions/`: محولات خدمات، عقود، مساعدات مشتركة، كائنات مجال، وأنواع أخطاء صريحة.

## المتطلبات المسبقة

| المتطلب | الملاحظات |
| --- | --- |
| 🐍 Python | `>=3.9, <4.0` |
| 🔐 مفتاح OpenAI API | مطلوب لتوليد الردود |
| 🎙️ الميكروفون / جهاز الإدخال | مطلوب لالتقاط الصوت |
| 🎵 مكتبات PortAudio | مطلوبة لاعتماد `pyaudio` |

### متطلبات macOS

ثبّت اعتمادات PortAudio:

```bash
brew install portaudio
brew link portaudio
```

حدث إعدادات `pydistutils` لاستخدام PortAudio:

```bash
echo "[build_ext]" >> $HOME/.pydistutils.cfg
echo "include_dirs="`brew --prefix portaudio`"/include/" >> $HOME/.pydistutils.cfg
echo "library_dirs="`brew --prefix portaudio`"/lib/" >> $HOME/.pydistutils.cfg
```

### ملاحظات غير macOS

تثبيت CI يقوم بتثبيت حزم Ubuntu المتوافقة مع التبعيات الصوتية، لكن سلوك زمن التشغيل قد يختلف حسب أدوات التشغيل.

استخدم `.env.example` كمرجع محلي لمدخلات البيئة المطلوبة.

## التثبيت

### التثبيت من PyPI

```bash
pip install chatgpt-voice-assistant
```

### التثبيت من المصدر

1. استنِسخ هذا المستودع.
2. ثبّت Poetry ([الوثائق الرسمية](https://python-poetry.org/docs/#installation) أو `pip install poetry`).
3. ثبّت التبعيات:

```bash
poetry install
```

لاستخدام تبعيات التطوير:

```bash
poetry install --with dev
```

## الاستخدام

عيّن `OPENAI_API_KEY` قبل التشغيل، أو أرسل المفتاح مباشرة:

```bash
export OPENAI_API_KEY=<OPEN_AI_SECRET_KEY>
gptassist

# OR
gptassist --open-ai-key=<OPEN_AI_SECRET_KEY>
```

إذا كان التشغيل من المصدر:

```bash
poetry run gptassist --open-ai-key=<OPEN_AI_SECRET_KEY>
```

ابدأ بالكلام واسمَع إلى الردود المنطوقة.

قل كلمة الإنهاء الآمن (`exit` افتراضيًا، يمكن تعديلها عبر `--safe-word`) أو اضغط `Ctrl+C` للتوقف.

## الإعدادات

### متغيرات البيئة

| المتغير | مطلوب | الوصف |
| --- | --- | --- |
| `OPENAI_API_KEY` | نعم (ما لم تُستخدم `--open-ai-key`) | بيانات اعتماد OpenAI المستخدمة في STT + chat + TTS الاختياري |

### خيارات CLI

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

> تلميح: أبقِ القيم الافتراضية الموثقة متزامنة مع `cli_parser.py` في النسخة الحالية.

## أمثلة

### أساسي

```bash
gptassist --open-ai-key=<OPENAI_KEY>
```

### استخدام wake word

```bash
gptassist --open-ai-key=<OPENAI_KEY> --wake-word="hey assistant"
```

### كلمة إنهاء مخصصة

```bash
gptassist --open-ai-key=<OPENAI_KEY> --safe-word="goodbye"
```

### اختيار محرك TTS

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=openai
gptassist --open-ai-key=<OPENAI_KEY> --tts=google
gptassist --open-ai-key=<OPENAI_KEY> --tts=apple
```

### تحديد جهاز الإدخال بالاسم

```bash
gptassist --open-ai-key=<OPENAI_KEY> --input-device-name="MacBook Pro Microphone"
```

### ضبط نبر Google

```bash
gptassist --open-ai-key=<OPENAI_KEY> --tts=google --lang=en --tld=com
```

عيّن `LANGUAGE` / `TOP_LEVEL_DOMAIN` لتغيير سلوك الإعداد المحلي الافتراضي:

| الإعداد المحلي | القيمة |
| --- | --- |
| French (France) | `LANGUAGE=fr TOP_LEVEL_DOMAIN=fr` |

راجع قسم اللهجات المحلية في وثائق gTTS لمزيد من التفاصيل.

### مثال مختلط

```bash
export OPENAI_API_KEY=sk-...
gptassist --open-ai-key=$OPENAI_API_KEY --tts=google --wake-word="hey helper" --safe-word="stop now" --speech-rate=1.1
```

## ملاحظات التطوير

راجع [DEVELOPMENT.md](./DEVELOPMENT.md) لإعدادات المشروع ومعايير البرمجة.

الأوامر الشائعة:

```bash
poetry install --with dev
poetry run pytest
poetry run coverage run --source chatgpt_voice_assistant -m pytest tests
poetry run coverage report --show-missing --fail-under=90
poetry run ruff format --check .
poetry run ruff check .
```

سير العمل في CI `.github/workflows/test-application.yml` يشغّل الاختبارات و فحوصات Ruff والتغطية.

## استكشاف الأخطاء

### `Open AI Secret Key not specified...`

عيّن `OPENAI_API_KEY` أو مرّر `--open-ai-key`.

### لا توجد أجهزة إدخال/ تم اختيار ميكروفون خاطئ

- تأكد أن الميكروفون متصل ومتاح لنظام التشغيل.
- استخدم `--input-device-name` مع اسم دقيق للجهاز.
- إن لم تُمرّر قيمة، فسيطلب التطبيق منك اختيار جهاز من قائمة أجهزة الإدخال المكتشفة.

### مشاكل تثبيت أو بناء `pyaudio`

- ثبّت PortAudio (انظر متطلبات macOS).
- أعد تشغيل تثبيت التبعيات بعد ضبط `.pydistutils.cfg`.

### لا يوجد تشغيل للصوت

- مسار التشغيل الحالي يستخدم `afplay` (أمر macOS).
- Apple TTS يستخدم `say` وOpenAI/Google ينتجان ملفات صوتية تُشغّل عبر `afplay`.
- على المنصات غير macOS قد يلزم تعديل إضافي لمسار التشغيل.

### النموذج غير متاح

إذا كان `--open-ai-model` الافتراضي أو المحدد غير متاح، مرّر نموذجًا يمكنك الوصول إليه:

```bash
gptassist --open-ai-key=<OPENAI_KEY> --open-ai-model=<YOUR_AVAILABLE_MODEL>
```

### أخطاء API / الشبكة

- تأكد من الاتصال وصلاحيات المفتاح.
- تحقق أن المفتاح صالح للمزوّد/النموذج المختار.
- زد مستوى التسجيل باستخدام `--log-level DEBUG` عند تتبع تدفق المحلل/زمن التشغيل.

## Internationalization (i18n)

- `i18n/` يحتوي على ملفات README مترجمة.
- الصيغة الإنجليزية هي النسخة المرجعية الرسمية.
- احتفظ بسطر واحد فقط `Language options:` في أعلى كل نسخة README وتجنب التكرار.
- الترجمات مجمّعة حسب اللغة والمنطقة لتتماشى مع تدفق المستندات في المشروع.

## Roadmap

- توسيع توثيق سلوك تشغيل الصوت متعدد المنصات.
- إضافة أو تحديث نسخ README متعددة اللغات ضمن `i18n/`.
- الحفاظ على تزامن القيم والأمثلة مع أسماء نماذج OpenAI الموصى بها حالياً.
- تحسين وثائق المعمارية والتشغيلية بما يتجاوز أساسيات README/DEVELOPMENT.

## Contributing

1. اعمل fork للمستودع.
2. أنشئ فرع ميزة.
3. شغّل الاختبارات والفحوصات محليًا:

```bash
poetry run pytest
poetry run ruff format --check .
poetry run ruff check .
```

4. افتح pull request مع وصف واضح للتغييرات وتغطية الاختبارات.

## ❤️ Support

| Donate | PayPal | Stripe |
| --- | --- | --- |
| [![Donate](https://camo.githubusercontent.com/24a4914f0b42c6f435f9e101621f1e52535b02c225764b2f6cc99416926004b7/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f446f6e6174652d4c617a79696e674172742d3045413545393f7374796c653d666f722d7468652d6261646765266c6f676f3d6b6f2d6669266c6f676f436f6c6f723d7768697465)](https://chat.lazying.art/donate) | [![PayPal](https://camo.githubusercontent.com/d0f57e8b016517a4b06961b24d0ca87d62fdba16e18bbdb6aba28e978dc0ea21/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f50617950616c2d526f6e677a686f754368656e2d3030343537433f7374796c653d666f722d7468652d6261646765266c6f676f3d70617970616c266c6f676f436f6c6f723d7768697465)](https://paypal.me/RongzhouChen) | [![Stripe](https://camo.githubusercontent.com/1152dfe04b6943afe3a8d2953676749603fb9f95e24088c92c97a01a897b4942/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f5374726970652d446f6e6174652d3633354246463f7374796c653d666f722d7468652d6261646765266c6f676f3d737472697065266c6f676f436f6c6f723d7768697465)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400) |

## Contact

- أبلغ عن الأعطال أو اطلب ميزات جديدة: [GitHub Issues](https://github.com/lachlanchen/chatgpt-voice-assistant/issues)
- اطرح أسئلة التصميم أو المعمارية: [GitHub Discussions](https://github.com/lachlanchen/chatgpt-voice-assistant/discussions)

## License

لا يوجد ملف `LICENSE` حاليًا في هذا المستودع.

الافتراض: شروط الترخيص غير موضّحة صراحة بعد. أضف ملف `LICENSE` لتوضيح شروط الاستخدام والتوزيع.

## References

- [Speech Recognition library docs](https://pypi.org/project/SpeechRecognition/1.2.3)
- [Google Translate Text-to-Speech API (gTTS)](https://gtts.readthedocs.io/en/latest/module.html#)
