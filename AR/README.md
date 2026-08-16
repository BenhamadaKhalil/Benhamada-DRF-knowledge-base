# 🇸🇦 النسخة العربية — قيد الإنشاء

> **Arabic edition — in progress.**
> This folder will mirror [`EN/`](../EN) in Arabic. The English edition is being finished first.

---

## الوضع الحالي · Current status

| | |
| --- | --- |
| **English** | ✅ مكتملة — [`EN/`](../EN) |
| **العربية** | 🚧 قيد الإنشاء |

---

## الخطة · The plan

سيتم بناء النسخة العربية على نفس هيكل النسخة الإنجليزية، قسمًا بقسم:

The Arabic edition will mirror the English structure section by section:

```
AR/
├── _Noetix/        الصفحة الرئيسية · خريطة المحتوى · مسار التعلّم
├── _Concepts/      طبقة المفاهيم
├── 01.تصميم التطبيق
├── 02.إعداد النظام
│   ...
└── 22.أساسيات Django
```

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
