---
title: مجموعات العروض ViewSet
lang: ar
type: concept
tags: [concept, drf, viewsets, views, arabic]
updated: 2026-08-16
---

# 🎛️ مجموعات العروض — ViewSet

> 🇬🇧 النسخة الإنجليزية: [[EN/_Concepts/ViewSet|ViewSet]]

---

## 🎯 الفكرة الأساسية

صنف واحد يجمع **كل عمليات مورد واحد** — `list`، `retrieve`، `create`، `update`، `destroy` — ويترك للـ router توليد الـ URLs.

```python
class RecipeViewSet(viewsets.ModelViewSet):
    serializer_class = serializers.RecipeDetailSerializer
    queryset = Recipe.objects.all()

    def get_queryset(self):
        return self.queryset.filter(user=self.request.user)
```

```python
router.register("recipes", views.RecipeViewSet)
```

سطران ← خمس نقاط نهاية.

| العملية | الطريقة | المسار |
| --- | --- | --- |
| `list` | GET | `/recipes/` |
| `create` | POST | `/recipes/` |
| `retrieve` | GET | `/recipes/{pk}/` |
| `update` / `partial_update` | PUT / PATCH | `/recipes/{pk}/` |
| `destroy` | DELETE | `/recipes/{pk}/` |

---

## 🪜 سُلَّم التجريد

![[drf-view-hierarchy.svg]]

**كل درجة أعلى تشتري كودًا أقلّ وتُكلّفك تحكّمًا أقلّ.** اصعد بقدر حاجتك فقط.

| تحتاج | استخدم |
| --- | --- |
| CRUD قياسي على نموذج | `ModelViewSet` |
| بعض العمليات لا كلّها | mixins + `GenericViewSet` |
| مورد للقراءة فقط | `ReadOnlyModelViewSet` |
| نقطة نهاية واحدة شاذّة (`/token/`، `/export/`) | `APIView` |

---

## 🧩 اختيار العمليات

`ModelViewSet` يمنحك الخمس. إن أردت ثلاثًا فقط، تُركِّب:

```python
class TagViewSet(mixins.DestroyModelMixin,
                 mixins.UpdateModelMixin,
                 mixins.ListModelMixin,
                 viewsets.GenericViewSet):     # ← الأساس دائمًا في النهاية
    serializer_class = serializers.TagSerializer
    queryset = Tag.objects.all()
```

لاحظ غياب `CreateModelMixin`: الوسوم تُنشَأ **داخل** الوصفة، لا عبر نقطة نهاية خاصّة بها.

> [!TIP] عدم إتاحة عملية قرارٌ تصميمي
> نقطة نهاية غير موجودة لا يمكن إساءة استخدامها، ولا تُنشئ تكرارًا، ولا تحتاج اختبارات.

---

## 🪝 الخطّافات الثلاثة المهمّة

```python
def get_queryset(self):
    """من يستطيع أن يرى."""
    return self.queryset.filter(user=self.request.user)

def perform_create(self, serializer):
    """من يملك ما يُنشَأ."""
    serializer.save(user=self.request.user)

def get_serializer_class(self):
    """أيّ شكل، حسب العملية."""
    if self.action == "list":
        return serializers.RecipeSerializer
    return serializers.RecipeDetailSerializer
```

اضبط هذه الثلاثة بشكل صحيح وتكون قد ضمنت معظم صحّة الـ API.

> [!WARNING] `queryset` كخاصيّة مقابل `get_queryset()` كدالّة
> الخاصيّة تُقيَّم مرة واحدة عند الاستيراد، ويستخدمها الـ router لاستنتاج اسم المسار. **الدالة** هي ما يُحدِّد فعلًا ما يراه الطالب. ضبط الخاصيّة ونسيان الدالة هو كيف يرى **كل مستخدم بيانات كل مستخدم**.

---

## ⚡ عملية مخصّصة

```python
@action(methods=["POST"], detail=True, url_path="upload-image")
def upload_image(self, request, pk=None):
    recipe = self.get_object()
    serializer = self.get_serializer(recipe, data=request.data)
    serializer.is_valid(raise_exception=True)
    serializer.save()
    return Response(serializer.data, status=status.HTTP_200_OK)
```

| المعامل | الأثر |
| --- | --- |
| `detail=True` | `/recipes/{pk}/upload-image/` |
| `detail=False` | `/recipes/upload-image/` |
| `url_path` | مقطع الـ URL |
| `permission_classes` | صلاحيات لهذه العملية وحدها |

---

## ⚠️ أخطاء شائعة

- **`basename argument not specified`** ← لا توجد خاصيّة `queryset` في الصنف. أضفها حتى لو كانت `get_queryset()` تقوم بالعمل الحقيقي.
- **`NoReverseMatch: 'recipe' is not a registered namespace`** ← ينقص `app_name = "recipe"` في `urls.py`، ورسالة الخطأ لا تذكر ذلك إطلاقًا.
- **`404` على مسار تراه في الـ router** ← الشرطة المائلة الأخيرة: `/recipes` ليست `/recipes/`.
- **كل المستخدمين يرون كل شيء** ← كتبت `queryset` ولم تكتب `get_queryset()`.

---

## 🧠 اختبر نفسك

> [!QUESTION]
> ١. لماذا يأتي `GenericViewSet` أخيرًا في تعريف الصنف؟
> ٢. ما فائدة خاصيّة `queryset` إن كانت `get_queryset()` تتجاوزها؟
> ٣. متى يكون `APIView` أفضل من `ModelViewSet`؟

---

## 🔗 روابط ذات صلة

- [[المُسلسِلات]] · [[الصلاحيات]] · [[مجموعات الاستعلام QuerySet]]
- [[EN/_Concepts/ViewSet|النسخة الإنجليزية]] 🇬🇧
- [[EN/20.Reference/3.ViewSet and Router Cheatsheet|ورقة ViewSet والـ Router]] 🇬🇧
