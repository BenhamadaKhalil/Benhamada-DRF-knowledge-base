---
title: Permissions
aliases: ["DRF Permissions", "Django Permissions", IsAuthenticated]
type: concept
tags: [concept, drf, permissions, authorization, security]
updated: 2026-08-16
---

# 🛡️ Permissions

[[Token Authentication|Authentication]] asks *who are you*. Permissions ask *are you allowed*.

| Class | Allows |
| --- | --- |
| `AllowAny` | everyone — the default if you set nothing |
| `IsAuthenticated` | any logged-in user |
| `IsAdminUser` | `user.is_staff` |
| `IsAuthenticatedOrReadOnly` | anyone reads, authenticated users write |

```python
authentication_classes = [TokenAuthentication]
permission_classes = [IsAuthenticated]
```

Set a safe global default and opt out deliberately — **fail closed**:

```python
REST_FRAMEWORK = {
    "DEFAULT_PERMISSION_CLASSES": ["rest_framework.permissions.IsAuthenticated"],
}
```

> [!CAUTION] `IsAuthenticated` is not authorization
> It proves someone is *a* user. It says nothing about whether they're *the* user. That gap is OWASP API #1 — IDOR.

## The thing everyone gets wrong

`has_object_permission()` is **never called on a list endpoint** — DRF has no single object to check. If your only protection is an object-level permission class, `GET /recipes/` returns every recipe in the database.

**List protection comes from the queryset. Always:**

```python
def get_queryset(self):
    return self.queryset.filter(user=self.request.user)
```

That also yields **404** instead of 403 for another user's object — better, because a 403 confirms the row exists and lets an attacker enumerate your IDs.

---

## 🔗 Deeper

- [[5.Custom Permissions]] — writing your own
- [[10.Security Checklist]]
- [[request.user]] · [[1.HTTP Status Codes]]
