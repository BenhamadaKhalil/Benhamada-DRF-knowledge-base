---
title: ملكيّة البيانات — request.user و perform_create
lang: ar
type: concept
tags: [concept, drf, security, ownership, mass-assignment, arabic]
updated: 2026-08-16
---

# 👤 ملكيّة البيانات — `request.user` و `perform_create`

> 🇬🇧 النسخة الإنجليزية: [[EN/_Concepts/request.user|request.user]] · [[EN/_Concepts/perform_create|perform_create]]

---

## 🎯 القاعدة الذهبية

**الملكيّة تأتي من التوكن، لا من جسم الطلب. أبدًا.**

```python
# ١ · من يستطيع أن يرى
def get_queryset(self):
    return self.queryset.filter(user=self.request.user)

# ٢ · من يملك ما يُنشَأ
def perform_create(self, serializer):
    serializer.save(user=self.request.user)

# ٣ · من هو «أنا»
def get_object(self):
    return self.request.user
```

اضبط هذه الثلاثة بشكل صحيح، وتكون قد ضمنت معظم أمان الـ API.

---

## 🔐 لماذا `perform_create` إجراء أمني لا رفاهية؟

```python
# ❌ لو كان user حقلًا قابلًا للكتابة في المُسلسِل
POST /api/recipe/recipes/  {"title": "س", "user": 3}
# ← وصفة يملكها المستخدم رقم ٣، أنشأها أيّ شخص
```

هذه ثغرة **الإسناد الجماعي (mass assignment)** — الثغرة رقم ٦ في تصنيف OWASP للـ APIs. الدفاع طبقتان:

1. `user` **غير موجود** في `Meta.fields` — فلا يستطيع العميل إرساله أصلًا
2. `perform_create` يضبطه من `request.user` — فيأتي من التوكن

`perform_create(serializer)` تعمل **بعد التحقّق وقبل الحفظ مباشرةً**، والمعامل الذي تُمرّره ينتهي داخل `validated_data`.

> [!TIP] لماذا لا تتجاوز `create()` بدلًا منها؟
> لأن `create()` مسؤولة أيضًا عن التحقّق وجسم الاستجابة ورمز الحالة. `perform_create` تُغيّر الجزء الذي تحتاجه فقط.

---

## 👥 `request.user` — من يضبطه؟

يضبطه **صنف المصادقة** في الخطوة الرابعة من دورة حياة الطلب.

```python
request.user                    # كائن User، أو AnonymousUser
request.user.is_authenticated   # ← تحقّق من هذا دائمًا أولًا
request.auth                    # كائن التوكن نفسه، إن وُجد
```

> [!WARNING] `AnonymousUser` قيمته **صحيحة منطقيًا**
> ```python
> if request.user:                   # ❌ True حتى حين لا أحد مسجّل
> if request.user.is_authenticated:  # ✅
> ```
> و`request.user.id` تُعيد `None` للمستخدم المجهول — فالمقارنة `obj.user == request.user` آمنة وتُعيد `False`، لكن الوصول إلى خصائص النتيجة ليس كذلك.

---

## ⏱️ الآثار الجانبية بعد الحفظ

```python
def perform_create(self, serializer):
    recipe = serializer.save(user=self.request.user)
    transaction.on_commit(
        lambda: notify.delay(recipe.id)      # ← بعد تثبيت المعاملة
    )
```

استدعاء `.delay()` خارج `on_commit` قد يلتقطه العامل **قبل تثبيت المعاملة**، فيبحث عن صفّ غير موجود بعد ويرمي `DoesNotExist`. خطأ متقطّع لا يظهر محلّيًا ويظهر باستمرار تحت الحِمل.

القاعدة: **كل ما لا تستطيع قاعدة البيانات التراجع عنه** — البريد، الـ webhooks، إبطال الذاكرة المؤقّتة، مهام Celery — مكانه `on_commit`.

---

## 🪝 الأخوات

| الخطّاف | يعمل عند |
| --- | --- |
| `perform_create(serializer)` | POST |
| `perform_update(serializer)` | PUT / PATCH — ومكان إبطال الذاكرة المؤقّتة |
| `perform_destroy(instance)` | DELETE — الحذف الناعم، تنظيف الملفات |

---

## 🔗 روابط ذات صلة

- [[الصلاحيات]] · [[مجموعات العروض ViewSet]] · [[المُسلسِلات]]
- [[EN/19.Production DRF/8.Transactions|المعاملات و on_commit]] 🇬🇧
