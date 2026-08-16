---
title: Routers
aliases: ["Routers in DRF"]
type: concept
tags: [concept, drf, urls, routers]
updated: 2026-08-16
---

# 🧭 Routers

A router turns a registered [[ViewSet]] into URL patterns, so you never hand-write `path()` for CRUD.

```python
# app/recipe/urls.py
router = DefaultRouter()
router.register("recipes", views.RecipeViewSet)
router.register("tags", views.TagViewSet)

app_name = "recipe"

urlpatterns = [path("", include(router.urls))]
```

Generated names:

| | List | Detail |
| --- | --- | --- |
| `register("recipes", …)` | `recipe:recipe-list` | `recipe:recipe-detail` |
| `register("tags", …)` | `recipe:tag-list` | `recipe:tag-detail` |

| Router | Gives you |
| --- | --- |
| `SimpleRouter` | just the routes |
| `DefaultRouter` | routes **plus** a browsable API root listing every endpoint |

> [!WARNING] Two things that break routers
> - **No `app_name`** → `reverse("recipe:tag-list")` raises `NoReverseMatch`, and the error never mentions `app_name`.
> - **No `queryset` class attribute** → `basename argument not specified`. The router derives the basename from it, even when `get_queryset()` does the real work. Pass `basename=` if you genuinely have no queryset.

---

## 🔗 Deeper

- [[3.ViewSet and Router Cheatsheet]]
- [[ViewSet]] · [[reverse URL resolution]]
