---
title: Python getattr
type: concept
tags: [concept, python, introspection]
updated: 2026-08-16
---

# 🐍 `getattr` / `setattr`

Read or write an attribute **by name, at runtime**.

```python
getattr(recipe, "title")            # same as recipe.title
getattr(recipe, "nope", "default")  # no AttributeError — returns "default"
setattr(recipe, "title", "New")     # same as recipe.title = "New"
hasattr(recipe, "title")            # True
```

## Why it shows up in tests

Asserting on a whole payload without naming each field:

```python
def test_create_recipe(self):
    """Test creating a recipe through the API."""
    payload = {"title": "Curry", "time_minutes": 30, "price": Decimal("5.99")}

    res = self.client.post(RECIPES_URL, payload)

    self.assertEqual(res.status_code, status.HTTP_201_CREATED)
    recipe = Recipe.objects.get(id=res.data["id"])

    for k, v in payload.items():
        self.assertEqual(getattr(recipe, k), v)      # ← here

    self.assertEqual(recipe.user, self.user)
```

Add a field to `payload` and the assertion covers it automatically.

## Why it shows up in serializers

`ModelSerializer.update()` is essentially this loop:

```python
def update(self, instance, validated_data):
    for attr, value in validated_data.items():
        setattr(instance, attr, value)
    instance.save()
    return instance
```

Which is exactly why the user serializer must override it — a blind `setattr(user, "password", raw)` would write a plaintext password. See [[Password Hashing]].

> [!WARNING] Never `getattr(obj, request.GET["field"])`
> Attribute names from user input let a caller read anything on the object, including `password` and `_state`. If you must, validate against an explicit allow-list first.

---

## 🔗 Deeper

- [[10.Testing Recipe Creation via API]] · [[5.Testing Patterns]]
- [[Serializer create and update]] · [[Tuple Unpacking]]
