---
title: perform_create
aliases: ["perform_create DRF"]
type: concept
tags: [concept, drf, viewsets, hooks]
updated: 2026-08-16
---

# 🪝 `perform_create`

The ViewSet hook that runs **after validation, just before saving** — the place to inject data the client must not supply.

```python
def perform_create(self, serializer):
    serializer.save(user=self.request.user)
```

The kwarg lands in `validated_data` and flows into `Model.objects.create(**validated_data)`.

**Why not override `create()`?** Because `create()` also owns validation, the response body, and the status code. `perform_create` changes only the part you need.

## Why it's a security control, not a convenience

```python
# ❌ if "user" is a writable serializer field
POST /api/recipe/recipes/  {"title": "x", "user": 3}
# → a recipe owned by user 3, created by whoever asked

# ✅ ownership comes from the token, and "user" isn't in fields at all
```

This is **mass assignment** prevention — OWASP API #6.

## The siblings

| Hook | Runs on |
| --- | --- |
| `perform_create(serializer)` | POST |
| `perform_update(serializer)` | PUT / PATCH — also where cache invalidation goes |
| `perform_destroy(instance)` | DELETE — soft delete, file cleanup |

> [!TIP] Side effects belong in `on_commit`
> ```python
> def perform_create(self, serializer):
>     recipe = serializer.save(user=self.request.user)
>     transaction.on_commit(lambda: notify.delay(recipe.id))
> ```
> A `.delay()` outside `on_commit` can hit the worker before the row commits — see [[8.Transactions]].

---

## 🔗 Deeper

- [[11.Implementing Recipe Creation in DRF]]
- [[3.ViewSet and Router Cheatsheet]]
- [[request.user]] · [[ViewSet]] · [[Serializer create and update]]
