---
title: Serializers
aliases: [Serializer, "Django Serializers", "DRF Serializers", ModelSerializer]
type: concept
tags: [concept, drf, serializers]
updated: 2026-08-16
---

# 🧬 Serializers

A serializer does **two** jobs, and most confusion comes from forgetting that an option only affects one of them:

| Direction | Name | What happens |
| --- | --- | --- |
| 📤 out | **serialization** | model instance → Python primitives → JSON |
| 📥 in | **deserialization** | JSON → validated data → `create()` / `update()` |

```python
class RecipeSerializer(serializers.ModelSerializer):

    class Meta:
        model = Recipe
        fields = ["id", "title", "time_minutes", "price", "link"]
        read_only_fields = ["id"]
```

| | `Serializer` | `ModelSerializer` |
| --- | --- | --- |
| Fields | you declare every one | inferred from the model |
| `create()` / `update()` | you write them | provided |
| Use for | login payloads, reports, webhooks | anything that maps to a model |

> [!CAUTION] Never `fields = "__all__"`
> It exposes every current column **and every column you add in future**. Add `is_premium` to the model and it ships in the API the same day, with nobody having touched the serializer.

Serializers are your data-egress boundary. `write_only` keeps passwords out of responses:

```python
extra_kwargs = {"password": {"write_only": True, "min_length": 5}}
```

---

## 🔗 Deeper

- [[2.Serializer Field Matrix]] — every field, every option
- [[Serializer Validation]] · [[Serializer create and update]] · [[Serializer Context]]
- [[Nested Serialization]]
