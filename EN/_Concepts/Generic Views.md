---
title: Generic Views
aliases: ["DRF Generic Views", CreateAPIView, RetrieveUpdateAPIView]
type: concept
tags: [concept, drf, views, generic-views]
updated: 2026-08-16
---

# 📦 Generic Views

Pre-mixed classes covering the standard CRUD combinations. One class = one URL.

| Class | Methods | URL |
| --- | --- | --- |
| `CreateAPIView` | POST | `/things/` |
| `ListAPIView` | GET | `/things/` |
| `ListCreateAPIView` | GET, POST | `/things/` |
| `RetrieveAPIView` | GET | `/things/{id}/` |
| `RetrieveUpdateAPIView` | GET, PUT, PATCH | `/things/{id}/` |
| `RetrieveUpdateDestroyAPIView` | GET, PUT, PATCH, DELETE | `/things/{id}/` |
| `DestroyAPIView` | DELETE | `/things/{id}/` |

```python
# app/user/views.py
class CreateUserView(generics.CreateAPIView):
    """Register — the only public endpoint."""
    serializer_class = UserSerializer
    permission_classes = [AllowAny]


class ManageUserView(generics.RetrieveUpdateAPIView):
    """GET / PUT / PATCH your own profile."""
    serializer_class = UserSerializer
    authentication_classes = [TokenAuthentication]
    permission_classes = [IsAuthenticated]

    def get_object(self):
        return self.request.user      # no {id} in the URL — /me/ resolves to you
```

That `get_object()` override is the whole trick behind `/me/`: the URL carries no ID, and the object is whoever the token belongs to. A client cannot ask for anyone else's profile because there's nowhere to put the request.

**Generic vs [[ViewSet]]:** a generic view is one URL, wired by hand in `urlpatterns`. A ViewSet is a whole resource, wired by a [[Routers|router]]. Use generic views for the odd one-off pairing; use a ViewSet when you have the full set.

![[drf-view-hierarchy.svg]]

---

## 🔗 Deeper

- [[9.Implement “Manage User”]] · [[5.Implementing Create User API]]
- [[3.ViewSet and Router Cheatsheet]]
- [[ViewSet]] · [[APIView]] · [[Permissions]]
