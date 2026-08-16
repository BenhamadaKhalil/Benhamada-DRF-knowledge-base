---
title: SessionAuthentication
type: concept
tags: [concept, drf, authentication, sessions, csrf]
updated: 2026-08-16
---

# 🍪 SessionAuthentication

Cookie-based auth backed by a server-side session store. What the Django admin and the DRF browsable API use.

```python
REST_FRAMEWORK = {
    "DEFAULT_AUTHENTICATION_CLASSES": [
        "rest_framework.authentication.SessionAuthentication",
        "rest_framework.authentication.TokenAuthentication",
    ],
}
```

**Best when** your API is consumed by a browser on the **same domain**. An `httpOnly` `Secure` `SameSite=Lax` cookie is the most XSS-resistant option available — JavaScript literally cannot read it — and the browser manages it for you.

**Worst when** the client isn't a browser, or lives on another origin. Then you're fighting CORS, `SameSite`, and CSRF for no benefit.

> [!WARNING] `403 CSRF Failed`
> `SessionAuthentication` **enforces CSRF** on unsafe methods. A non-browser client hitting a session-authenticated endpoint gets a 403 that reads like a permission bug and isn't. Send `X-CSRFToken`, or use [[Token Authentication|token auth]] — which is exempt, because there's no cookie to ride on.

## Why this project keeps it enabled anyway

The browsable API and `/admin/` both need it. Token auth is the *primary* scheme; session auth sits alongside it so you can click around the API in a browser during development.

That's also why you occasionally get a confusing CSRF error while testing with `curl` against a dev server.

---

## 🔗 Deeper

- [[6.Token vs JWT vs Session]]
- [[4.Authentication in Django REST Framework]]
- [[Browsable API]] · [[Django Admin]]
