---
title: مجموعات الاستعلام QuerySet
lang: ar
type: concept
tags: [concept, django, orm, queryset, performance, arabic]
updated: 2026-08-16
---

# 🔬 مجموعات الاستعلام — QuerySet

> 🇬🇧 النسخة الإنجليزية: [[EN/22.Django Core/4.QuerySets in Depth|QuerySets in Depth]]

---

## 🎯 مجموعة الاستعلام وعدٌ، لا نتيجة

```python
qs = Recipe.objects.filter(user=user)     # صفر استعلامات
qs = qs.filter(price__lt=10)              # صفر استعلامات
qs = qs.order_by("-id")                   # صفر استعلامات
qs = qs[:10]                              # صفر استعلامات
results = list(qs)                        # استعلام واحد — كل شيء دفعة واحدة
```

التسلسل يبني شجرة تعبير. لا شيء يمسّ قاعدة البيانات حتى **يُجبَر التقييم**.

| يُطلق استعلامًا | لا يُطلق |
| --- | --- |
| `list(qs)` | `qs.filter(...)` |
| `for r in qs:` | `qs.exclude(...)` |
| `len(qs)` | `qs.order_by(...)` |
| `bool(qs)` / `if qs:` | `qs[5:10]` *(تقطيع)* |
| `qs[0]` *(فهرسة)* | `.only()` / `.defer()` |
| `repr(qs)` | الإسناد |

> [!TIP] لماذا تُضلّلك الطرفية؟
> طباعة مجموعة استعلام في الـ shell تُقيّمها، فيصبح الكسل غير مرئي هناك. في الكود الحقيقي ينطلق الاستعلام عند أول تكرار — غالبًا داخل مُسلسِل، بعيدًا عن المكان الذي بنيت فيه المجموعة.

---

## 💾 ذاكرة النتائج

```python
qs = Recipe.objects.all()
for r in qs: ...        # استعلام ١ — النتيجة تُخزَّن على الكائن
for r in qs: ...        # صفر استعلامات — من الذاكرة

for r in Recipe.objects.all(): ...   # مجموعة جديدة ← استعلام ٢
```

```python
# ❌ استعلامان
if Recipe.objects.filter(user=user).exists():
    for r in Recipe.objects.filter(user=user): ...

# ✅ استعلام واحد
recipes = list(Recipe.objects.filter(user=user))
if recipes:
    for r in recipes: ...
```

قاعدة مفيدة: `exists()` أفضل من `count()` أفضل من `len()` حين تريد فقط أن تعرف «هل يوجد شيء؟».

---

## 🐌 مشكلة N+1

أهمّ مشكلة أداء في أي Django API:

```text
استعلام ١     SELECT * FROM core_recipe WHERE user_id = 7
١٠٠ استعلام   SELECT * FROM core_recipe_tags WHERE recipe_id = ?
١٠٠ استعلام   SELECT * FROM core_recipe_ingredients WHERE recipe_id = ?
─────────────────────────────────────────────────────────────
٢٠١ استعلام لطلب HTTP واحد
```

الحلّ أربعة أسطر:

```python
def get_queryset(self):
    return (
        self.queryset
        .filter(user=self.request.user)
        .select_related("user")                    # ForeignKey ← JOIN
        .prefetch_related("tags", "ingredients")   # ManyToMany ← استعلام ثانٍ
        .order_by("-id")
    )
```

٢٠١ استعلام ← **٣**.

| | `select_related` | `prefetch_related` |
| --- | --- | --- |
| للعلاقات | ForeignKey، OneToOne | ManyToMany، العكسية |
| الآلية | `JOIN` في SQL | استعلام ثانٍ + `IN` |
| الدمج يحدث في | قاعدة البيانات | بايثون |

**لماذا لا يصلح JOIN للـ ManyToMany؟** لأن ربط ١٠٠ وصفة بـ ٥ وسوم لكل منها يُعيد ٥٠٠ صفًا لإعادة بناء ١٠٠ كائن — التضخّم أغلى من استعلام ثانٍ.

---

## ⭐ الجديد في Django 6.1: `fetch_mode()`

```python
from django.db import models

Recipe.objects.fetch_mode(models.FETCH_PEERS)   # حمّل للجميع عند أول وصول
Recipe.objects.fetch_mode(models.FETCH_RAISE)   # ارفض التحميل الكسول تمامًا
```

| الوضع | عند الوصول إلى علاقة غير محمّلة |
| --- | --- |
| `FETCH_ONE` | استعلام لهذا الكائن فقط *(الافتراضي — مصدر N+1)* |
| `FETCH_PEERS` | استعلام واحد يغطّي كل كائنات المجموعة نفسها |
| `FETCH_RAISE` | يرمي `FieldFetchBlocked` |

الفرق الجوهري: `prefetch_related` يتطلّب أن **تتنبّأ** بما سيلمسه المُسلسِل، بينما `FETCH_PEERS` **يتفاعل** مع ما يُلمَس فعلًا.

> [!TIP] `FETCH_RAISE` في إعدادات الاختبار
> يُحوّل أي N+1 عرضي إلى فشل صريح في CI، بدل نقطة نهاية بطيئة لا ينتبه إليها أحد.

---

## 🔍 كيف ترى المشكلة؟

```python
with self.assertNumQueries(4):
    self.client.get(RECIPES_URL)
```

`assertNumQueries` يُحوّل «أظنّه سريعًا» إلى اختبار يفشل حين يُعيد أحدهم المشكلة. أضف واحدًا لكل نقطة نهاية قائمة تهتمّ بها.

```python
print(Recipe.objects.filter(user=user).query)     # الـ SQL الفعلي
```

---

## ⚠️ أخطاء شائعة

- **`.filter(x=1, y=2)` تختلف عن `.filter(x=1).filter(y=2)`** في علاقات ManyToMany — كل `filter` يُنشئ `JOIN` خاصًّا به.
- **`Cannot filter a query once a slice has been taken`** ← رشِّح أولًا، وقطِّع أخيرًا.
- **`prefetch_related` لم يُفِد** ← استدعيت `.filter()` على العلاقة داخل الحلقة، فأهدرت التحميل المسبق.
- **`count()` بطيء على جدول ضخم** ← `SELECT COUNT(*)` مسح كامل في Postgres.
- **`update()` و`bulk_create()` تتجاوز `save()` والإشارات** — وتتجاوز `auto_now`.

---

## 🧠 اختبر نفسك

> [!QUESTION]
> ١. لماذا لا يُكلّف تكرار المتغيّر نفسه شيئًا، بينما استدعاء `objects.all()` من جديد يُكلّف استعلامًا؟
> ٢. لماذا لا يصلح `select_related` لعلاقة ManyToMany؟
> ٣. متى يكون `FETCH_PEERS` أفضل من `prefetch_related`، ومتى يكون أسوأ؟

---

## 🔗 روابط ذات صلة

- [[مجموعات العروض ViewSet]] · [[الدالة get_or_create]]
- [[1.الجديد في Django 6.1]]
- [[EN/19.Production DRF/4.The N+1 Problem|مشكلة N+1 بالتفصيل]] 🇬🇧
