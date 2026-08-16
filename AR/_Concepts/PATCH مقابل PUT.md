---
title: PATCH مقابل PUT
lang: ar
type: concept
tags: [concept, http, rest, api-design, arabic]
updated: 2026-08-16
---

# 🔧 `PATCH` مقابل `PUT`

> 🇬🇧 النسخة الإنجليزية: [[EN/_Concepts/PATCH vs PUT|PATCH vs PUT]]

---

## 🎯 الفرق

| | `PUT` | `PATCH` |
| --- | --- | --- |
| المعنى | **استبدل** المورد كاملًا | **عدّل** الحقول المُرسَلة فقط |
| الحقول المحذوفة | تُعاد إلى الافتراضي أو تُرفض كمطلوبة | تبقى كما هي |
| مُتكافئ التكرار | ✅ | غالبًا، وليس مضمونًا |
| العملية في DRF | `update` | `partial_update` |
| الراية في DRF | `partial=False` | `partial=True` |

```python
# الوصفة الحالية: {"title": "كاري", "time_minutes": 45, "price": "12.00"}

PUT   /recipes/1/  {"title": "كاري تايلندي"}
# ❌ 400 — time_minutes و price مطلوبان ولم تُرسلهما

PATCH /recipes/1/  {"title": "كاري تايلندي"}
# ✅ 200 — العنوان فقط يتغيّر
```

**الآلية كلها في راية واحدة:** `partial=True` تجعل كل حقول المُسلسِل `required=False` لهذا الطلب وحده.

---

## ⚖️ أيّهما تستخدم؟

**`PATCH` في معظم الحالات.** العميل الذي يُعدّل حقلًا واحدًا لا ينبغي أن يُعيد إرسال الكائن كاملًا — وإن فعل، فقد خلقتَ **سباق تحديثات ضائعة** بين عميلين يُعدّلان حقلين مختلفين في الوقت نفسه.

**`PUT` حين يكون الاستبدال هو المقصود فعلًا**، والعميل يملك المورد كاملًا بشكل مشروع.

---

## 🧪 الاختبار الذي يهمّ

```python
def test_partial_update(self):
    """PATCH لا يمسّ بقيّة الحقول."""
    recipe = create_recipe(user=self.user, title="قديم", link="http://x.com")

    res = self.client.patch(detail_url(recipe.id), {"title": "جديد"})

    self.assertEqual(res.status_code, status.HTTP_200_OK)
    recipe.refresh_from_db()
    self.assertEqual(recipe.title, "جديد")
    self.assertEqual(recipe.link, "http://x.com")     # ← هذا هو التأكيد الحقيقي
```

السطر الأخير هو الاختبار الفعلي. التحقّق من أن الحقل المُرسَل تغيّر سهل؛ التحقّق من أن **البقيّة لم تتغيّر** هو ما يُثبت أن `PATCH` يعمل.

> [!WARNING] `refresh_from_db()` ليست اختيارية
> الكائن `recipe` في بايثون لقطة من الصف قبل الطلب. الـ `PATCH` غيّر **الصف** لا كائنك. بدون التحديث يُقارن التأكيد القيمة القديمة ويفشل، فتقضي عشرين دقيقة مقتنعًا أن العرض معطوب.

---

## ⚠️ فخّ القائمة الفارغة

```python
{"tags": []}      # يعني: احذف كل الوسوم
```

على دالّة `update()` أن تُميّز بين «قائمة فارغة» و«لم يُذكَر الحقل أصلًا»:

```python
if tags is not None:   # ✅
if tags:               # ❌ يُعامل [] كأنها «لا تلمس الوسوم»
```

خطأ حقيقي وصامت في طلبات `PATCH`.

---

## 🔗 روابط ذات صلة

- [[تصميم REST API]] · [[المُسلسِلات]]
- [[EN/13.Build Tag API/6.Implement update recipe tags feature Follow Along|تعديل وسوم الوصفة]] 🇬🇧
