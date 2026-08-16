---
title: OpenAPI
aliases: [drf-spectacular, "Swagger OpenAPI Docs"]
type: concept
tags: [concept, drf, documentation, openapi, swagger]
updated: 2026-08-16
---

# 📄 OpenAPI & drf-spectacular

**OpenAPI** is the specification format that describes a REST API — every endpoint, parameter, request body and response shape, as machine-readable YAML or JSON.

**Swagger UI** renders that spec into an interactive page.

**drf-spectacular** generates the spec **from your code**, so it can never go stale.

```bash
pip install drf-spectacular
```

```python
# app/app/settings.py
INSTALLED_APPS = [..., "drf_spectacular"]

REST_FRAMEWORK = {
    "DEFAULT_SCHEMA_CLASS": "drf_spectacular.openapi.AutoSchema",
}

SPECTACULAR_SETTINGS = {
    "TITLE": "Recipe API",
    "VERSION": "1.0.0",
}
```

```python
# app/app/urls.py
from drf_spectacular.views import SpectacularAPIView, SpectacularSwaggerView

urlpatterns = [
    path("api/schema/", SpectacularAPIView.as_view(), name="api-schema"),
    path("api/docs/", SpectacularSwaggerView.as_view(url_name="api-schema"), name="api-docs"),
]
```

| URL | Serves |
| --- | --- |
| `/api/schema/` | the raw OpenAPI 3 YAML |
| `/api/docs/` | Swagger UI |

## Why generated beats hand-written

Hand-written API docs drift from reality within weeks — someone renames a field and nobody edits the wiki. drf-spectacular reads your serializers and viewsets, so **the docs are a projection of the code.**

## Documenting what it can't infer

Query parameters and custom actions need a hint:

```python
@extend_schema_view(
    list=extend_schema(
        parameters=[
            OpenApiParameter("tags", OpenApiTypes.STR,
                             description="Comma-separated tag IDs to filter by."),
        ]
    )
)
class RecipeViewSet(viewsets.ModelViewSet):
    ...
```

`@extend_schema` also carries `deprecated=True`, which Swagger renders with a strike-through — the cleanest way to signal a [[12.Versioning an API|sunset]].

---

## 🔗 Deeper

- [[2.Automated API Documentation in Django REST Framework]] · [[4.Enabling Swagger Documentation URLs]]
- [[🧩 Filtering Recipes by Tags & Ingredients (with API Documentation)]]
- [[Browsable API]] · [[12.Versioning an API]]
