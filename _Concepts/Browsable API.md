---
title: Browsable API
type: concept
tags: [concept, drf, browsable-api, renderers]
updated: 2026-08-16
---

# 🌐 Browsable API

DRF's self-documenting HTML interface. Hit any endpoint in a browser and you get a rendered page with the response, a form to POST with, and the allowed methods — instead of raw JSON.

It's **content negotiation** doing its job: the same view returns JSON for `Accept: application/json` and HTML for a browser's `Accept: text/html`.

```python
REST_FRAMEWORK = {
    "DEFAULT_RENDERER_CLASSES": [
        "rest_framework.renderers.JSONRenderer",
        "rest_framework.renderers.BrowsableAPIRenderer",
    ],
}
```

`DefaultRouter` also gives you a **root view** listing every registered endpoint — a free index of your API at `/api/recipe/`.

## What it's good for

- Clicking through an API you're building, with no client to write
- Seeing exactly what a serializer produces
- Trying a POST body without reaching for `curl` or Postman

## What to watch

> [!CAUTION] In production, decide deliberately
> The browsable API is a complete, interactive map of your endpoints for anyone who finds it. It also uses [[SessionAuthentication|session auth]] for its forms, which is why a browser can be logged in while your API client isn't.
>
> Either disable it in production, or make that exposure a conscious choice:
> ```python
> if not DEBUG:
>     REST_FRAMEWORK["DEFAULT_RENDERER_CLASSES"] = [
>         "rest_framework.renderers.JSONRenderer",
>     ]
> ```

**Not a substitute for real docs.** [[OpenAPI|drf-spectacular]] generates a schema clients can consume and code-generate from. The browsable API is a development convenience.

---

## 🔗 Deeper

- [[10.Test the User API in the Browser]]
- [[OpenAPI]] · [[SessionAuthentication]] · [[Routers]]
