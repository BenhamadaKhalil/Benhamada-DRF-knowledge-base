---
title: get_or_create
aliases: ["Django get_or_create"]
type: concept
tags: [concept, django, orm, nested-write]
updated: 2026-08-16
---

# 🎣 `get_or_create`

Fetch a row, or create it if it isn't there. Returns a **tuple**.

```python
tag_obj, created = Tag.objects.get_or_create(user=auth_user, name="Vegan")
```

| Return | Meaning |
| --- | --- |
| `(obj, True)` | it didn't exist; we made it |
| `(obj, False)` | it already existed |

Ignore the flag with `_` when you don't care:

```python
tag_obj, _ = Tag.objects.get_or_create(user=auth_user, **tag)
```

> [!WARNING] It always returns a tuple
> ```python
> tag = Tag.objects.get_or_create(...)     # ❌ tag is (Tag, bool)
> tag, _ = Tag.objects.get_or_create(...)  # ✅
> ```
> Forgetting the unpack gives `AttributeError: 'tuple' object has no attribute 'name'` — see [[Tuple Unpacking]].

## Why it's the heart of nested writes

It's the decision DRF refused to make for you. Given `{"tags": [{"name": "vegan"}]}`, `get_or_create` says: **reuse the user's existing "vegan" tag, or make one.** No duplicates, no client-side lookup, no extra round trip.

```python
def _get_or_create_tags(self, tags, recipe):
    auth_user = self.context["request"].user
    for tag in tags:
        tag_obj, _ = Tag.objects.get_or_create(user=auth_user, **tag)
        recipe.tags.add(tag_obj)
```

**`user=auth_user` is load-bearing.** Without it, your "vegan" and my "vegan" collide into one row.

## `defaults` — lookup vs create values

```python
Tag.objects.get_or_create(user=user, name="Vegan", defaults={"color": "green"})
```

Everything outside `defaults` is used for the lookup; `defaults` is only applied on creation.

## Concurrency

Under load, two requests can both find nothing and both insert. Add a database constraint so the second one fails cleanly rather than duplicating:

```python
class Meta:
    constraints = [
        models.UniqueConstraint(fields=["user", "name"], name="unique_tag_per_user"),
    ]
```

---

## 🔗 Deeper

- [[Nested Serialization]] · [[4.Implementing Tag Creation]] · [[Creating Tags]]
- [[4.Django ORM Cookbook]] · [[Serializer create and update]]
