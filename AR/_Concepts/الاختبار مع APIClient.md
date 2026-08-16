---
title: الاختبار مع APIClient
lang: ar
type: concept
tags: [concept, drf, testing, tdd, arabic]
updated: 2026-08-16
---

# 🧪 الاختبار مع `APIClient`

> 🇬🇧 النسخة الإنجليزية: [[EN/_Concepts/APIClient|APIClient]] · [[EN/20.Reference/5.Testing Patterns|Testing Patterns]]

---

## 🎯 عميل الاختبار في DRF

مثل عميل Django، لكنه يتحدّث JSON ويفهم مصادقة DRF.

```python
from rest_framework.test import APIClient

self.client = APIClient()
```

```python
self.client.get(URL)
self.client.get(URL, {"tags": "1,2"})                    # معاملات استعلام
self.client.post(URL, payload)
self.client.post(URL, payload, format="json")            # بيانات متداخلة
self.client.post(URL, {"image": f}, format="multipart")  # رفع ملف
self.client.patch(url, {"title": "جديد"})
self.client.delete(url)
```

> [!WARNING] `format="json"` للبيانات المتداخلة
> الترميز الافتراضي يُسطّح البُنى المتداخلة. `{"tags": [{"name": "نباتي"}]}` يصل مشوّهًا، فيفشل الاختبار لسبب لا علاقة له بكودك إطلاقًا.

---

## 🎭 نمط البيت: عام وخاصّ

كل ملف اختبار API في هذا المشروع ينقسم إلى صنفين:

```python
class PublicRecipeApiTests(TestCase):
    """طلبات غير مُصادَقة — الاختبار الذي يتخطّاه الجميع."""

    def setUp(self):
        self.client = APIClient()

    def test_auth_required(self):
        res = self.client.get(RECIPES_URL)
        self.assertEqual(res.status_code, status.HTTP_401_UNAUTHORIZED)


class PrivateRecipeApiTests(TestCase):
    """طلبات مُصادَقة."""

    def setUp(self):
        self.client = APIClient()
        self.user = create_user()
        self.client.force_authenticate(self.user)
```

`test_auth_required` هو الاختبار الذي يُهمَل، ثم تُنشَر نقطة نهاية مفتوحة للجميع.

---

## 🎫 `force_authenticate`

```python
self.client.force_authenticate(self.user)                          # يتخطّى التوكن
self.client.credentials(HTTP_AUTHORIZATION="Token " + token.key)   # يستخدم الترويسة الحقيقية
```

**استخدم `force_authenticate` حين تختبر نقطة نهاية**، لأنك تختبر النقطة لا آلية المصادقة. واختبر آلية المصادقة نفسها **مرة واحدة**، في اختبارها الخاصّ.

> [!INFO] يتخطّى المصادقة، لا الصلاحيات
> أصناف الصلاحيات تعمل كالمعتاد. `force_authenticate` يُمرّرك من الخطوة الرابعة فقط — وهذا بالضبط ما تريده حين تختبر إن كان المستخدم «أ» يصل إلى بيانات المستخدم «ب».

---

## ✅ أقوى تأكيد للقوائم

```python
recipes = Recipe.objects.all().order_by("-id")
serializer = RecipeSerializer(recipes, many=True)
self.assertEqual(res.data, serializer.data)
```

يفحص الحقول والقيم **والشكل** في سطر واحد.

> [!WARNING] الترتيب يجب أن يتطابق
> على الاختبار أن يستخدم `order_by()` نفسه الذي يستخدمه العرض. PostgreSQL لا يضمن ترتيبًا بدون `ORDER BY` — وهذا السبب المعتاد لاختبار ينجح محلّيًا ويفشل في CI.

---

## 🔄 `refresh_from_db()`

```python
res = self.client.patch(url, {"title": "جديد"})

recipe.refresh_from_db()                       # ← بدونها يبقى "قديم"
self.assertEqual(recipe.title, "جديد")
```

كائنك في بايثون لقطة من الصف وقت جلبه. الطلب غيّر **الصف**.

---

## 🏃 تشغيل الاختبارات

```bash
docker compose run --rm app sh -c "python manage.py test"
```

```bash
docker compose run --rm app sh -c "python manage.py test --shuffle"
```

| الراية | الأثر |
| --- | --- |
| `--shuffle` | ترتيب عشوائي — **يكشف الاختبارات المتعلّقة ببعضها** |
| `--keepdb` | يُعيد استخدام قاعدة الاختبار؛ أسرع بكثير |
| `--failfast` | يتوقّف عند أول فشل |
| `-v 2` | يطبع اسم كل اختبار |

---

## ⚠️ أخطاء شائعة

- **«Ran 0 tests»** ← ينقص `tests/__init__.py`، أو الملفات ليست بصيغة `test_*.py`. يخرج بحالة نجاح دون أن يختبر شيئًا.
- **ينجح منفردًا ويفشل ضمن المجموعة** ← حالة مشتركة. `--shuffle` يُثبت ذلك.
- **التأكيد يرى القيمة القديمة** ← ينقص `refresh_from_db()`.
- **المحاكاة (mock) بلا أثر** ← رقّعت مكان **التعريف** لا مكان **الاستخدام**.

---

## 🎯 ما الذي تختبره؟

| دائمًا | نادرًا |
| --- | --- |
| رفض الطلب غير المُصادَق | الـ ORM نفسه |
| أن المستخدم «أ» لا يرى بيانات «ب» | الـ getters والـ setters |
| رفض المُدخَلات الخاطئة | تفاصيل مكتبات الطرف الثالث |
| المسار السعيد يُعيد الشكل الصحيح | كل تباديل الحقل الواحد |

**اختبارات الصلاحيات هي الأجدر بالكتابة والأكثر إهمالًا.**

---

## 🔗 روابط ذات صلة

- [[الصلاحيات]] · [[مجموعات الاستعلام QuerySet]]
- [[EN/20.Reference/5.Testing Patterns|أنماط الاختبار الكاملة]] 🇬🇧
