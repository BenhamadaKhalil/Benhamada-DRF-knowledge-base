---
title: APIView وأصناف العروض
lang: ar
type: concept
tags: [concept, drf, views, apiview, generic-views, arabic]
updated: 2026-08-16
---

# 🧱 `APIView` وأصناف العروض

> 🇬🇧 النسخة الإنجليزية: [[EN/_Concepts/APIView|APIView]] · [[EN/_Concepts/Generic Views|Generic Views]]

---

## 🪜 سُلَّم التجريد

![[drf-view-hierarchy.svg]]

**كل درجة أعلى تشتري كودًا أقلّ وتُكلّفك تحكّمًا أقلّ.** اصعد بقدر حاجتك، ولا تُقاوم الإطار حين يتوقّف شكله عن مطابقة مشكلتك — انزل درجة بدل ذلك.

---

## 🧱 `APIView` — الأساس

تكتب `get()` و`post()` بنفسك، ويمنحك DRF مع ذلك كائن `Request` و`Response` والتفاوض على المحتوى والمصادقة والصلاحيات والتحديد.

```python
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status


class HealthView(APIView):
    permission_classes = [AllowAny]

    def get(self, request):
        return Response({"status": "ok"}, status=status.HTTP_200_OK)
```

**متى تستخدمه؟** حين لا تُقابل نقطة النهاية عمليات CRUD على نموذج:

- `/api/user/token/` — تسجيل الدخول
- `/export/` — توليد تقرير
- مُستقبِل webhook
- فحص صحّة الخدمة

**متى لا تستخدمه؟** لأي مورد له قائمة وتفصيل. ذلك عمل [[مجموعات العروض ViewSet|ViewSet]] أو عرض عامّ، وكتابته يدويًا مجرّد كود إضافي يحتمل الخطأ.

---

## 📦 العروض العامّة — Generic Views

أصناف جاهزة تُغطّي التركيبات القياسية. **صنف واحد = مسار واحد.**

| الصنف | الطرق | المسار |
| --- | --- | --- |
| `CreateAPIView` | POST | `/things/` |
| `ListAPIView` | GET | `/things/` |
| `ListCreateAPIView` | GET, POST | `/things/` |
| `RetrieveAPIView` | GET | `/things/{id}/` |
| `RetrieveUpdateAPIView` | GET, PUT, PATCH | `/things/{id}/` |
| `RetrieveUpdateDestroyAPIView` | + DELETE | `/things/{id}/` |

```python
# app/user/views.py
class CreateUserView(generics.CreateAPIView):
    """التسجيل — نقطة النهاية العامّة الوحيدة."""
    serializer_class = UserSerializer
    permission_classes = [AllowAny]


class ManageUserView(generics.RetrieveUpdateAPIView):
    """قراءة وتعديل الملف الشخصي للمستخدم الحالي."""
    serializer_class = UserSerializer
    authentication_classes = [TokenAuthentication]
    permission_classes = [IsAuthenticated]

    def get_object(self):
        return self.request.user      # لا يوجد {id} في المسار
```

> [!TIP] الحيلة كلها في `get_object()`
> مسار `/me/` لا يحمل أي مُعرِّف، والكائن المُعاد هو **صاحب التوكن**. لا يستطيع العميل أن يطلب ملف شخص آخر ببساطة لأنه **لا يوجد مكان يضع فيه الطلب**. هذا تصميم آمن بالبناء، لا بالفحص.

---

## ⚖️ عامّ أم ViewSet؟

| | العرض العامّ | ViewSet |
| --- | --- | --- |
| النطاق | مسار واحد | المورد كامل |
| الـ URLs | تكتبها في `urlpatterns` | يُولّدها الـ router |
| استخدمه حين | لديك تركيبة واحدة شاذّة | لديك المجموعة كاملة |

---

## 🎯 قاعدة الاختيار

| الحالة | الاختيار |
| --- | --- |
| CRUD قياسي على نموذج | `ModelViewSet` |
| بعض العمليات لا كلّها | mixins + `GenericViewSet` |
| مورد للقراءة فقط | `ReadOnlyModelViewSet` |
| نقطة نهاية واحدة شاذّة | `APIView` |

إقحام نقطة نهاية شاذّة داخل ViewSet يُكلّف أكثر ممّا يوفّر.

---

## 🔗 روابط ذات صلة

- [[مجموعات العروض ViewSet]] · [[المُسلسِلات]] · [[الصلاحيات]]
- [[EN/20.Reference/3.ViewSet and Router Cheatsheet|ورقة ViewSet والـ Router]] 🇬🇧
