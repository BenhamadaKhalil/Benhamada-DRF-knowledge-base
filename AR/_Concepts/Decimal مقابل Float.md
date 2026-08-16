---
title: Decimal مقابل Float
lang: ar
type: concept
tags: [concept, python, django, models, money, arabic]
updated: 2026-08-16
---

# 💰 `Decimal` مقابل `Float`

> 🇬🇧 النسخة الإنجليزية: [[EN/_Concepts/Decimal vs Float|Decimal vs Float]]

---

## 🎯 المشكلة في سطرين

```python
>>> 0.1 + 0.2
0.30000000000000004
```

هذا **ليس خطأً في بايثون**. الفاصلة العائمة الثنائية لا تستطيع تمثيل `0.1` بدقّة، تمامًا كما لا يستطيع النظام العشري تمثيل ⅓ بدقّة. كل عملية حسابية تحمل خطأً ضئيلًا، والأخطاء **تتراكم**.

في محاكاة فيزيائية هذا مقبول. في المال هو **عيب**.

```python
price = models.FloatField()                                   # ❌
price = models.DecimalField(max_digits=5, decimal_places=2)    # ✅
```

`DecimalField` يُقابل نوع `numeric` في PostgreSQL — حساب عشري دقيق بلا خطأ تقريب. و`max_digits=5, decimal_places=2` تعني حتى `999.99`.

---

## ⚠️ أنشئ `Decimal` من **نصّ**

```python
from decimal import Decimal

Decimal("0.1") + Decimal("0.2")     # Decimal('0.3') ✅
Decimal(0.1)                        # Decimal('0.1000000000000000055511151231257827') ❌
```

تمرير `float` يُدخِل الخطأ **قبل أن تبدأ**. في الاختبارات: `create_recipe(price=Decimal("5.25"))` لا `Decimal(5.25)`.

---

## 📤 يُسلسَل كنصّ

```json
{ "price": "5.25" }
```

**وهذا مقصود.** تحويله إلى رقم JSON يُعيد المشكلة نفسها إلى العميل. لتغيير السلوك:

```python
REST_FRAMEWORK = {"COERCE_DECIMAL_TO_STRING": False}
```

افعل ذلك عن **علم**: هو تغيير كاسر للتوافق مع العملاء الحاليين.

---

## 💡 البديل: خزّن السنتات كعدد صحيح

بعض الفرق تستخدم `PositiveIntegerField` للسنتات (`525` = ٥٫٢٥) وتُنسّق عند الحافة. دقيق وسريع ويستحيل إساءة استخدامه — لكنه يتطلّب تحويلًا في كل مكان.

`DecimalField` هو الافتراضي الأفضل؛ اعرف أن هذا البديل موجود.

---

## 🔗 روابط ذات صلة

- [[نماذج Django]] · [[المُسلسِلات]]
- [[EN/20.Reference/7.Model Field Reference|مرجع حقول النموذج]] 🇬🇧
