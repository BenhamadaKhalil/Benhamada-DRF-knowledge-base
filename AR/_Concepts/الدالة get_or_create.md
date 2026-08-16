---
title: الدالة get_or_create
lang: ar
type: concept
tags: [concept, django, orm, nested-write, arabic]
updated: 2026-08-16
---

# 🎣 الدالة `get_or_create`

> 🇬🇧 النسخة الإنجليزية: [[EN/_Concepts/get_or_create|get_or_create]]

---

## 🎯 الفكرة

اجلب الصف، أو أنشئه إن لم يكن موجودًا. تُعيد **زوجًا (tuple)**:

```python
tag_obj, created = Tag.objects.get_or_create(user=auth_user, name="نباتي")
```

| القيمة المُعادة | المعنى |
| --- | --- |
| `(obj, True)` | لم يكن موجودًا، فأنشأناه |
| `(obj, False)` | كان موجودًا |

حين لا يهمّك العَلَم:

```python
tag_obj, _ = Tag.objects.get_or_create(user=auth_user, **tag)
```

> [!WARNING] تُعيد زوجًا دائمًا
> ```python
> tag = Tag.objects.get_or_create(...)     # ❌ tag أصبح (Tag, bool)
> tag, _ = Tag.objects.get_or_create(...)  # ✅
> ```
> نسيان فكّ الزوج يُعطي `AttributeError: 'tuple' object has no attribute 'name'`.

---

## 🪆 لماذا هي قلب الكتابة المتداخلة؟

هذه هي **القرار الذي رفض DRF أن يتّخذه نيابةً عنك**.

حين يصل `{"tags": [{"name": "نباتي"}]}`، لا يعرف DRF إن كنت تقصد: أنشئ وسمًا جديدًا، أم استخدم الموجود، أم ارفض التكرار. فيرفض التخمين. و`get_or_create` هي إجابتك: **أعِد استخدام وسم المستخدم الموجود، أو أنشئ واحدًا.** بلا تكرار، وبلا بحث من جهة العميل، وبلا طلب إضافي.

```python
def _get_or_create_tags(self, tags, recipe):
    auth_user = self.context["request"].user

    for tag in tags:
        tag_obj, _ = Tag.objects.get_or_create(user=auth_user, **tag)
        recipe.tags.add(tag_obj)
```

> [!CAUTION] `user=auth_user` ليست تفصيلًا
> بدونها، «نباتي» الخاصّ بك و«نباتي» الخاصّ بي يندمجان في صفّ واحد. الوسوم مملوكة لمستخدم، لا للعالم.

---

## 🎛️ المعامل `defaults`

```python
Tag.objects.get_or_create(user=user, name="نباتي", defaults={"color": "green"})
```

كل ما هو **خارج** `defaults` يُستخدَم في البحث؛ ما بداخلها يُطبَّق عند الإنشاء فقط.

---

## 🏁 التزامن — الحالة التي تنساها

تحت الحِمل، قد يصل طلبان في اللحظة نفسها، فلا يجد كلاهما شيئًا، فيُدرج كلاهما صفًّا. النتيجة: وسمان متطابقان.

الحلّ قيد على مستوى قاعدة البيانات، فيفشل الثاني بوضوح بدل أن يُكرِّر:

```python
class Meta:
    constraints = [
        models.UniqueConstraint(
            fields=["user", "name"],
            name="unique_tag_per_user",
        ),
    ]
```

هذا مثال عامّ على قاعدة أوسع: **التحقّق في المُسلسِل يُعطي رسالة جيّدة، والقيد في قاعدة البيانات يضمن الصحّة.** الأول يمكن تجاوزه من الـ shell أو لوحة الإدارة أو أمر إداري؛ الثاني لا.

---

## 🔁 الأخوات

```python
obj, created = Tag.objects.update_or_create(
    user=user,
    name="نباتي",
    defaults={"color": "green"},      # يُحدَّث إن وُجد، ويُضبط إن أُنشئ
)
```

---

## ⚠️ أخطاء شائعة

- **`'tuple' object has no attribute ...`** ← نسيت فكّ الزوج.
- **وسوم مكرّرة لمستخدمين مختلفين تندمج** ← نسيت `user=` في معاملات البحث.
- **`IntegrityError` تحت الحِمل** ← سباق تزامن؛ أضف `UniqueConstraint` والتقط الخطأ.
- **`TypeError` عند `Model.objects.create(**validated_data)`** ← لم تُخرِج المفاتيح المتداخلة بـ `pop` أولًا.

---

## 🧠 اختبر نفسك

> [!QUESTION]
> ١. أيّ قرارٍ تُجسّده `get_or_create` ورفض DRF أن يتّخذه؟
> ٢. ماذا يحدث إن حذفت `user=auth_user` من معاملات البحث؟
> ٣. لماذا لا يكفي التحقّق في المُسلسِل لمنع التكرار؟

---

## 🔗 روابط ذات صلة

- [[المُسلسِلات]] · [[مجموعات الاستعلام QuerySet]]
- [[EN/_Concepts/get_or_create|النسخة الإنجليزية]] 🇬🇧
- [[EN/13.Build Tag API/4.Implementing Tag Creation|تنفيذ إنشاء الوسوم]] 🇬🇧
