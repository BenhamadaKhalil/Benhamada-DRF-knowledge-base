---
title: APIView
type: concept
tags: [concept, drf, views]
updated: 2026-08-16
---

# 🧱 APIView

DRF's base class-based view. You write `get()` / `post()` yourself; DRF still gives you its `Request`, its `Response`, content negotiation, authentication, permissions and throttling.

```python
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status


class HealthView(APIView):
    permission_classes = [AllowAny]

    def get(self, request):
        return Response({"status": "ok"}, status=status.HTTP_200_OK)
```

**When to use it:** the endpoint doesn't map to CRUD on a model. `/api/user/token/`, `/export/`, a webhook receiver, a health check.

**When not to:** anything that's list-and-detail over a model. That's a [[ViewSet]] or a [[Generic Views|generic view]], and writing it by hand is just more code to get wrong.

`APIView.dispatch()` is the entry point to all of DRF — it's step 3 in the request lifecycle, where a plain Django request becomes a DRF one.

![[drf-request-lifecycle.svg]]

---

## 🔗 Deeper

- [[2.APIView vs ViewSet]]
- [[3.ViewSet and Router Cheatsheet]]
- [[ViewSet]] · [[Generic Views]]
