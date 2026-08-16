---
title: Serializer Context
aliases: ["Django REST Framework Context"]
type: concept
tags: [concept, drf, serializers, request]
updated: 2026-08-16
---

# 🎒 Serializer Context

The dict a serializer carries so it can reach things outside its own data — above all, **the request**.

```python
auth_user = self.context["request"].user
```

That's how `_get_or_create_tags()` knows who owns the tag it's creating, without the client ever sending a `user` field.

**Generic views and ViewSets populate it automatically:**

```python
{"request": <Request>, "view": <ViewSet>, "format": None}
```

**Instantiating a serializer by hand does not** — you must pass it:

```python
# ❌ KeyError: 'request'
serializer = RecipeSerializer(recipe)

# ✅
serializer = RecipeSerializer(recipe, context={"request": request})
```

This is why a serializer that works in a view raises `KeyError` in a management command or a test.

Add your own context via the view:

```python
def get_serializer_context(self):
    ctx = super().get_serializer_context()
    ctx["today"] = timezone.now().date()
    return ctx
```

> [!TIP] `HiddenField` avoids the context dance entirely
> ```python
> user = serializers.HiddenField(default=serializers.CurrentUserDefault())
> ```
> Sets the owner from the request without exposing a writable field.

---

## 🔗 Deeper

- [[4.Implementing Tag Creation]]
- [[Serializers]] · [[request.user]] · [[2.Serializer Field Matrix]]
