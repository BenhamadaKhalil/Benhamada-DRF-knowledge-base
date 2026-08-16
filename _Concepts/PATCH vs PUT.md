---
title: PATCH vs PUT
aliases: ["PATCH vs PUT HTTP"]
type: concept
tags: [concept, http, rest, api-design]
updated: 2026-08-16
---

# 🔧 PATCH vs PUT

| | `PUT` | `PATCH` |
| --- | --- | --- |
| Semantics | **replace** the whole resource | **modify** the given fields |
| Omitted fields | reset to default / rejected as required | left untouched |
| Idempotent | ✅ | usually, not guaranteed |
| DRF action | `update` | `partial_update` |
| DRF flag | `partial=False` | `partial=True` |

```python
# recipe: {"title": "Curry", "time_minutes": 45, "price": "12.00"}

PUT   /recipes/1/  {"title": "Thai Curry"}
# ❌ 400 — time_minutes and price are required, and you didn't send them

PATCH /recipes/1/  {"title": "Thai Curry"}
# ✅ 200 — only the title changes
```

That `partial=True` flag is the entire mechanism: it makes every serializer field `required=False` for this one request.

## Which to use

**`PATCH` for almost everything.** A client updating one field shouldn't need to re-send the whole object — and if it does, you've created a lost-update race between two clients editing different fields.

**`PUT` when replacement is genuinely what you mean**, and the client legitimately holds the complete resource.

## In tests

```python
def test_partial_update(self):
    """Test PATCH leaves other fields alone."""
    recipe = create_recipe(user=self.user, title="Old", link="http://x.com")

    res = self.client.patch(detail_url(recipe.id), {"title": "New"})

    self.assertEqual(res.status_code, status.HTTP_200_OK)
    recipe.refresh_from_db()
    self.assertEqual(recipe.title, "New")
    self.assertEqual(recipe.link, "http://x.com")     # ← the actual assertion
```

> [!WARNING] `PATCH` with an empty list
> `{"tags": []}` means *"remove all tags"*. Your `update()` must distinguish it from *"tags not mentioned"*:
> ```python
> if tags is not None:   # ✅
> if tags:               # ❌ treats [] as "don't touch"
> ```

---

## 🔗 Deeper

- [[Serializer create and update]] · [[6.Implement update recipe tags feature Follow Along]]
- [[1.HTTP Status Codes]] · [[REST API Design]]
