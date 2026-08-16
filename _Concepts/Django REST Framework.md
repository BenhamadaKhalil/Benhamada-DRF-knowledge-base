---
title: Django REST Framework
aliases: [DRF]
type: concept
tags: [concept, drf, django, overview]
updated: 2026-08-16
---

# 🧰 Django REST Framework

Django gives you the ORM, URL routing and an admin. **DRF adds the API layer on top.**

| DRF gives you | Without it you'd hand-write |
| --- | --- |
| [[Serializers]] | JSON ↔ model conversion, plus validation |
| [[APIView]] / [[ViewSet]] / [[Generic Views]] | CRUD plumbing, five times per resource |
| [[Token Authentication\|Authentication]] classes | header parsing, user lookup |
| [[Permissions]] classes | authorization checks in every view |
| Content negotiation | `Accept` header handling |
| The browsable API | a client just to test with |
| Pagination, throttling, filtering | all of it |

You *can* build an API in plain Django. You just end up rewriting the table above, worse.

```python
# app/app/settings.py
INSTALLED_APPS = [
    "rest_framework",
    "rest_framework.authtoken",
    ...
]

REST_FRAMEWORK = {
    "DEFAULT_AUTHENTICATION_CLASSES": [
        "rest_framework.authentication.TokenAuthentication",
    ],
    "DEFAULT_PERMISSION_CLASSES": [
        "rest_framework.permissions.IsAuthenticated",
    ],
}
```

## Where DRF starts

Middleware and URL routing are still plain Django. DRF takes over at `APIView.dispatch()` — step 3 — and owns authentication, permissions, the view handler, serialization and rendering from there.

![[drf-request-lifecycle.svg]]

Knowing that boundary is most of debugging: a `400` is DRF's serializer, a `401` is DRF's auth, but a `Host header` `400` is Django's `ALLOWED_HOSTS` and has nothing to do with DRF at all.

---

## 🔗 Deeper

- [[Technologies]] · [[REST API Design]]
- [[3.ViewSet and Router Cheatsheet]] · [[2.Serializer Field Matrix]]
- [[0.Production Readiness Checklist]]
