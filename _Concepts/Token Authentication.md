---
title: Token Authentication
aliases: [TokenAuthentication, "DRF TokenAuthentication", "API Authentication", "Authentication in DRF", "Django Authentication", "Django Authentication System"]
type: concept
tags: [concept, drf, authentication, token-auth]
updated: 2026-08-16
---

# 🪙 Token Authentication

**Credentials once → token many times.** The client posts email + password to `/api/user/token/`, gets back an opaque key, and sends that key on every subsequent request.

```http
Authorization: Token 9c1f2e4a8b...
```

> [!WARNING] It is `Token`, not `Bearer`
> `Bearer` is JWT syntax. Every HTTP client defaults to it, and DRF's error message doesn't mention the keyword. This is the most common DRF mistake there is.

```python
# app/app/settings.py
INSTALLED_APPS = [..., "rest_framework.authtoken"]

REST_FRAMEWORK = {
    "DEFAULT_AUTHENTICATION_CLASSES": [
        "rest_framework.authentication.TokenAuthentication",
    ],
}
```

DRF looks the key up in the `authtoken_token` table and sets `request.user`. One indexed primary-key read per request.

**Trade-off:** a DB lookup on every request, in exchange for **instant revocation** — delete the row and the client is out. [[JWT]] inverts that trade.

**Default tokens never expire.** Fix that with an `ExpiringTokenAuthentication` subclass.

![[token-auth-flow.svg]]

---

## 🔗 Deeper

- [[4.Authentication in Django REST Framework]] — the full note
- [[7.Custom Token Authentication]] — building the endpoint
- [[6.Token vs JWT vs Session]] — choosing between them
- [[Authorization Header]] · [[ObtainAuthToken]] · [[Permissions]]
