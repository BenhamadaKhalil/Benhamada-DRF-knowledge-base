---
title: request.user
type: concept
tags: [concept, drf, authentication, request]
updated: 2026-08-16
---

# 👤 `request.user`

Set by the [[Token Authentication|authentication class]] at step 4 of the request lifecycle. Everything downstream — permissions, querysets, ownership — reads from it.

```python
request.user                    # a User instance, or AnonymousUser
request.user.is_authenticated   # bool — always check this first
request.auth                    # the Token instance itself, if any
```

**Its three jobs in this API:**

```python
# 1 · who may see it
def get_queryset(self):
    return self.queryset.filter(user=self.request.user)

# 2 · who owns what they create
def perform_create(self, serializer):
    serializer.save(user=self.request.user)

# 3 · who "me" is
def get_object(self):
    return self.request.user
```

> [!WARNING] `AnonymousUser` is truthy
> ```python
> if request.user:                   # ❌ True even when nobody is logged in
> if request.user.is_authenticated:  # ✅
> ```
> And `request.user.id` on an `AnonymousUser` is `None` — comparisons like `obj.user == request.user` are safely `False`, but attribute access on the result is not.

**The security rule:** ownership comes from `request.user`, never from the request body. A client sending `{"user": 3}` must not be able to create a row owned by user 3.

---

## 🔗 Deeper

- [[perform_create]] · [[Permissions]] · [[Serializer Context]]
- [[9.Implement “Manage User”]]
- [[10.Security Checklist]]
