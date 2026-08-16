---
title: هيكل مشروع Django
lang: ar
type: concept
tags: [concept, django, architecture, project-structure, arabic]
updated: 2026-08-16
---

# 🏗️ هيكل مشروع Django

> 🇬🇧 النسخة الإنجليزية: [[EN/_Concepts/Django Project Structure|Django Project Structure]]

---

## 🎯 مشروع أم تطبيق؟

**المشروع (project)** هو الكلّ، يُنشَأ مرة واحدة.
**التطبيق (app)** وحدة مُركّزة بداخله، تُنشئ واحدًا كلما احتجت وظيفة جديدة.

```bash
django-admin startproject app .
```

```bash
docker compose run --rm app sh -c "python manage.py startapp recipe"
```

> [!WARNING] التطبيق لا يفعل شيئًا حتى تُسجّله
> ```python
> INSTALLED_APPS = [..., "recipe"]
> ```
> هذا هو الشيء الذي ينساه الجميع، والخطأ الناتج لا يُشير إليه.

---

## 📁 الملفات الخمسة، بترتيب تدفّق البيانات

| الملف | وظيفته | التشبيه |
| --- | --- | --- |
| `models.py` | شكل بياناتك كأصناف بايثون | عناوين أعمدة الجدول |
| `migrations/` | فروق مُولَّدة تتحوّل إلى SQL حقيقي | commits في git، لكن للمخطّط |
| `serializers.py` | JSON ↔ نموذج، مع التحقّق | المُترجِم عند الحدود |
| `views.py` | ما يعمل حين يُطلَب مسار | من يردّ على الهاتف |
| `urls.py` | يربط المسار بالعرض | رقم الهاتف ← أيّ مكتب يرنّ |

`startapp` يُنشئ `models.py` و`views.py` و`admin.py` و`apps.py` و`tests.py` و`migrations/`.
**لا يُنشئ `urls.py` ولا `serializers.py`** — تُضيفهما بنفسك.

---

## 🧩 تطبيقات هذا المشروع

| التطبيق | يملك |
| --- | --- |
| `app/` | المشروع: `settings.py`، `urls.py` الجذري، `wsgi.py` |
| `core` | النماذج المشتركة (User, Recipe, Tag, Ingredient)، لوحة الإدارة، `wait_for_db` |
| `user` | `/api/user/` — التسجيل، التوكن، الملف الشخصي |
| `recipe` | `/api/recipe/` — الوصفات، الوسوم، المكوّنات |

> [!INFO] لماذا النماذج في `core` لا في كل تطبيق؟
> لإبقاء الترحيلات في مكان واحد، ولتجنّب الاستيراد الدائري بين `user` و`recipe`. هذا **تبسيط مقصود** يناسب حجم هذا المشروع؛ قاعدة كود أكبر تدفع النماذج إلى تطبيقاتها الخاصّة.

---

## ➡️ قاعدة الاعتماد

```text
urls.py ← views.py (رفيعة) ← serializers.py (تحقّق) ← models.py (الحقيقة) ← migrations ← SQL
```

**أبقِ العروض رفيعة.** منطق العمل مكانه المُسلسِلات أو النماذج أو وحدة خدمات مخصّصة — لا داخل دالّة عرض.

![[project-architecture.svg]]

---

## ⚡ الأوامر التي ستُكرّرها

```bash
docker compose run --rm app sh -c "python manage.py startapp <name>"
```

```bash
docker compose run --rm app sh -c "python manage.py makemigrations && python manage.py migrate"
```

النمط الذي تُعيده إلى الأبد: **عدِّل `models.py` ← `makemigrations` ← `migrate`.** نسيان الخطوة الأخيرة هو السبب الأول لسؤال «لماذا لا يظهر تعديلي؟» — للجميع، دائمًا.

---

## 🔗 روابط ذات صلة

- [[نماذج Django]] · [[الترحيلات]] · [[إطار Django REST]]
- [[EN/18.All Fixes/1.Django Refresher - Projects, Apps and the Five Files|مراجعة Django السريعة]] 🇬🇧
