---
title: Serializer create and update
aliases: ["Serializer create Method", "Serializer create() Method", "Serializer update Method", "Serializer save method", "ModelSerializer update"]
type: concept
tags: [concept, drf, serializers, nested-write]
updated: 2026-08-16
---

# 🏗️ Serializer `create()` and `update()`

`serializer.save()` dispatches to one of two methods, decided by whether an instance was passed in:

```python
serializer = RecipeSerializer(data=payload)              # → create()
serializer = RecipeSerializer(recipe, data=payload)      # → update()
serializer.is_valid(raise_exception=True)
serializer.save(user=request.user)                       # kwargs land in validated_data
```

`ModelSerializer` provides both for free — **until the payload contains a nested writable field**, at which point it refuses:

```text
The .create() method does not support writable nested fields by default.
```

That's deliberate. Given `{"tags": [{"name": "vegan"}]}` DRF can't know whether you mean *create*, *find existing*, or *error on duplicate* — and guessing wrong silently corrupts data. So you say:

```python
def create(self, validated_data):
    tags = validated_data.pop("tags", [])
    recipe = Recipe.objects.create(**validated_data)
    self._get_or_create_tags(tags, recipe)
    return recipe


def update(self, instance, validated_data):
    tags = validated_data.pop("tags", None)

    if tags is not None:
        instance.tags.clear()
        self._get_or_create_tags(tags, instance)

    for attr, value in validated_data.items():
        setattr(instance, attr, value)

    instance.save()
    return instance
```

> [!TIP] `pop()` before `create()`
> Nested keys aren't model fields. Leaving them in `validated_data` makes `Model.objects.create(**validated_data)` raise `TypeError`.

> [!WARNING] `tags is not None` vs `if tags:`
> An empty list `[]` means *"remove all tags"*. `if tags:` treats that as *"don't touch tags"* — a real, silent bug on PATCH.

Multi-table writes belong inside [[8.Transactions|`transaction.atomic()`]].

---

## 🔗 Deeper

- [[Nested Serialization]] — the full treatment
- [[4.Implementing Tag Creation]] · [[6.Implement update recipe tags feature Follow Along]]
- [[get_or_create]] · [[Serializers]] · [[perform_create]]
