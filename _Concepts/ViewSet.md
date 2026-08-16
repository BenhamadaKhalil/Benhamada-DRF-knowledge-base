---
title: ViewSet
aliases: [ModelViewSet, "DRF ModelViewSet"]
type: concept
tags: [concept, drf, viewsets, views]
updated: 2026-08-16
---

# 🎛️ ViewSet

A class that groups **all the actions for one resource** — `list`, `retrieve`, `create`, `update`, `partial_update`, `destroy` — and lets a [[Routers|router]] generate the URLs.

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

Two lines → five endpoints.

| Action | Method | Path |
| --- | --- | --- |
| `list` | GET | `/recipes/` |
| `create` | POST | `/recipes/` |
| `retrieve` | GET | `/recipes/{pk}/` |
| `update` / `partial_update` | PUT / PATCH | `/recipes/{pk}/` |
| `destroy` | DELETE | `/recipes/{pk}/` |

**Want only some actions?** Compose mixins onto `GenericViewSet` instead — that's how the tag and ingredient viewsets get list/update/destroy without create.

**Choosing:** standard CRUD on a model → `ModelViewSet`. One odd endpoint (`/token/`, `/export/`) → [[APIView]]. Forcing an odd endpoint into a ViewSet costs more than it saves.

![[drf-view-hierarchy.svg]]

---

## 🔗 Deeper

- [[2.APIView vs ViewSet]] — the full comparison
- [[3.ViewSet and Router Cheatsheet]]
- [[Routers]] · [[Generic Views]] · [[perform_create]]
