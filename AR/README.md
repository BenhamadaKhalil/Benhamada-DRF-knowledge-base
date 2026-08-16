# 🇸🇦 النسخة العربية

> **Arabic edition.** Mirrors [`EN/`](../EN). The concept and reference layers are being translated first — they're what you reopen and search.

## ▶️ ابدأ من هنا · Start here

**[[الصفحة الرئيسية]]** — لوحة التحكّم · [`_Noetix/الصفحة الرئيسية.md`](_Noetix/الصفحة%20الرئيسية.md)

---

## الوضع الحالي · Current status

| | | |
| --- | --- | --- |
| **English** | ✅ مكتملة | [`EN/`](../EN) |
| **العربية** | 🚧 قيد الإنشاء — ٣٢ ملاحظة | راجع [خريطة المحتوى](_Noetix/خريطة%20المحتوى.md) |

### ✅ ما تُرجم حتى الآن

| | |
| --- | --- |
| **طبقة Noetix** | [الصفحة الرئيسية](_Noetix/الصفحة%20الرئيسية.md) · [مسار التعلّم](_Noetix/مسار%20التعلّم.md) · [خريطة المحتوى](_Noetix/خريطة%20المحتوى.md) |
| **المفاهيم — ١٩** | [فهرس المفاهيم](_Concepts/0.فهرس%20المفاهيم.md) · الإطار · هيكل المشروع · تصميم REST · ViewSet · APIView · الملكيّة · المُسلسِلات · `get_or_create` · PATCH/PUT · مصادقة التوكن · ترويسة Authorization · الصلاحيات · نموذج المستخدم · تجزئة كلمات المرور · النماذج · QuerySet · الترحيلات · Decimal/Float · APIClient |
| **المرجع — ٩** | [فهرس المرجع](20.المرجع/0.فهرس%20المرجع.md) · رموز حالة HTTP · مصفوفة حقول المُسلسِل · ورقة ViewSet والـ Router · كتاب طبخ الـ ORM · أنماط الاختبار · أوامر Docker · مرجع حقول النموذج · [المسرد](20.المرجع/8.المسرد.md) |
| **أساسيات Django** | [الجديد في Django 6.1](22.أساسيات%20Django/1.الجديد%20في%20Django%206.1.md) |

**طبقتا المفاهيم والمرجع مكتملتان** — وهما ما يُفتَح ويُبحَث فيه مرارًا. المفاهيم المتبقّية بالإنجليزية ثانوية، ومُدرَجة في فهرس المفاهيم مع روابطها.

### ⬜ الترتيب القادم

1. DRF في الإنتاج (القسم ١٩)
2. أقسام الدورة ١–١٦

**لماذا هذا الترتيب؟** المفاهيم والمراجع هي ما يُفتَح مرارًا ويُبحَث فيه — أعلى عائد للترجمة. ملاحظات الدورة تُقرأ مرة واحدة بالتوازي مع الفيديو.

---

## الهيكل · Structure

```
AR/
├── _Noetix/                الصفحة الرئيسية · مسار التعلّم · خريطة المحتوى
├── _Concepts/              طبقة المفاهيم
└── 22.أساسيات Django/      الجديد في Django 6.1
```

الأقسام تُضاف تباعًا بنفس ترقيم النسخة الإنجليزية.

### قواعد الترجمة · Translation rules

1. **الشرح بالعربية، الكود بالإنجليزية.** أسماء الدوال والمتغيّرات ورسائل الأخطاء تبقى كما هي — لأنك ستراها هكذا في الطرفية.
   *Prose in Arabic, code in English.* Function names, variables and error messages stay verbatim — that's how you'll meet them in the terminal.

2. **المصطلحات التقنية تبقى لاتينية** مع شرح عربي عند أول ذكر:
   *Technical terms stay in Latin script* with an Arabic gloss on first mention:
   `serializer` (المُسلسِل) · `queryset` (مجموعة الاستعلام) · `middleware` (الوسيطة)

3. **الروابط متبادلة.** كل ملاحظة عربية تشير إلى مقابلها الإنجليزي والعكس.
   *Cross-linked.* Every Arabic note links to its English twin and back.

4. **الرسوم مشتركة.** المخططات في [`assets/`](../assets) مشتركة بين اللغتين؛ المخططات التي تحتوي نصًا كثيرًا سيكون لها نسخة عربية.
   *Shared diagrams.* The SVGs in `assets/` serve both editions; text-heavy ones get an Arabic variant.

---

## ملاحظة حول الاتجاه · A note on direction

Obsidian يدعم الكتابة من اليمين إلى اليسار. فعّلها من:
Obsidian supports right-to-left. Enable it at:

**Settings → Editor → Right-to-left (RTL)**

كتل الكود تبقى من اليسار إلى اليمين تلقائيًا.
Code blocks stay left-to-right automatically.

---

*Noetix by KB · المعرفة، مُهندَسة.*
