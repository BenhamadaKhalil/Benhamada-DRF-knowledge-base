---
title: ترويسة Authorization
lang: ar
type: concept
tags: [concept, http, authentication, headers, arabic]
updated: 2026-08-16
---

# 🔖 ترويسة `Authorization`

> 🇬🇧 النسخة الإنجليزية: [[EN/_Concepts/Authorization Header|Authorization Header]]

---

## 🎯 البنية

الترويسة القياسية لحمل بيانات الاعتماد. قيمتها **مخطّط** ثم **بيانات**:

```http
Authorization: <scheme> <credentials>
```

| المخطّط | يستخدمه |
| --- | --- |
| `Token` | **`TokenAuthentication` في DRF** ← هذا المشروع |
| `Bearer` | [[EN/_Concepts/JWT\|JWT]] و OAuth 2.0 |
| `Basic` | `user:pass` بترميز base64 — يُرسَل مع كل طلب |

```http
Authorization: Token 9c1f2e4a8b3d6e0f1a2b3c4d5e6f7a8b9c0d1e2f
```

---

## ⚠️ الخطأ الذي يُكلّف ساعة

```http
Authorization: Bearer 9c1f...   ❌ DRF يتجاهلها تمامًا
Authorization: Token 9c1f...    ✅
```

**لماذا يقع فيه الجميع؟**

1. كل درس عن JWT يستخدم `Bearer`
2. Postman ومعظم عملاء HTTP تختارها افتراضيًا
3. رسالة خطأ DRF — *"Authentication credentials were not provided."* — **لا تذكر الكلمة إطلاقًا**

فتبدو المشكلة وكأن التوكن غير صالح، بينما التوكن سليم تمامًا ولم يُقرأ أصلًا.

---

## 📤 كيف تُرسلها

```bash
curl -H "Authorization: Token 9c1f..." https://api.example.com/api/recipe/recipes/
```

```python
self.client.credentials(HTTP_AUTHORIZATION="Token " + token.key)
```

```js
fetch(url, { headers: { Authorization: `Token ${token}` } })
```

لاحظ الصيغة في اختبارات Django: `HTTP_AUTHORIZATION`. يُحوّل Django ترويسات الطلب إلى `request.META` برفعها إلى الأحرف الكبيرة، واستبدال `-` بـ `_`، وإضافة البادئة `HTTP_`.

---

## 🚫 لماذا ترويسة لا معامل استعلام؟

```text
GET /api/recipes/?token=9c1f...     ❌ لا تفعل هذا أبدًا
```

معاملات الاستعلام تُسجَّل في:

- سجلّات الوصول على الخادم
- تاريخ المتصفّح
- ترويسة `Referer` المُرسَلة إلى مواقع الطرف الثالث

الترويسات لا تُسجَّل في أيٍّ من هذه افتراضيًا.

> [!CAUTION] الترويسة سرٌّ أثناء النقل
> استخدم HTTPS دائمًا. ولا تُسجّل الترويسة الخام في السجلّات أبدًا — ذلك يضع بيانات اعتماد حيّة في نظام تجميع السجلّات لديك.

---

## 🔗 روابط ذات صلة

- [[مصادقة التوكن]] · [[الصلاحيات]]
- [[EN/18.All Fixes/2.Errors You Will Actually Hit|الأخطاء التي ستواجهها فعلًا]] 🇬🇧
