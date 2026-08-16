---
title: Authorization Header
type: concept
tags: [concept, http, authentication, headers]
updated: 2026-08-16
---

# 🔖 The `Authorization` Header

The standard HTTP header for carrying credentials. Its value is a **scheme** plus a **token**:

```http
Authorization: <scheme> <credentials>
```

| Scheme | Used by |
| --- | --- |
| `Token` | **DRF `TokenAuthentication`** ← this project |
| `Bearer` | [[JWT]], OAuth 2.0 |
| `Basic` | base64 `user:pass` — sent on every request |

```http
Authorization: Token 9c1f2e4a8b3d6e0f1a2b3c4d5e6f7a8b9c0d1e2f
```

> [!WARNING] The 401 that costs everyone an hour
> ```http
> Authorization: Bearer 9c1f...   ❌ DRF ignores this
> Authorization: Token 9c1f...    ✅
> ```
> Every JWT tutorial uses `Bearer`. Postman and most HTTP clients default to it. And DRF's error — *"Authentication credentials were not provided."* — never mentions the keyword.

## Sending it

```bash
curl -H "Authorization: Token 9c1f..." https://api.example.com/api/recipe/recipes/
```

```python
self.client.credentials(HTTP_AUTHORIZATION="Token " + token.key)
```

```js
fetch(url, { headers: { Authorization: `Token ${token}` } })
```

Note the Django test-client spelling: `HTTP_AUTHORIZATION`. Django maps request headers into `request.META` by uppercasing, replacing `-` with `_`, and prefixing `HTTP_`.

## Why a header and not a query parameter

```text
GET /api/recipes/?token=9c1f...     ❌ never do this
```

Query strings land in server access logs, browser history, and `Referer` headers sent to third parties. Headers don't.

> [!CAUTION] The header is a secret in transit
> Always HTTPS. Never log the raw header — that's a live credential in your log aggregator. See [[11.Logging and Observability]].

---

## 🔗 Deeper

- [[Token Authentication]] · [[4.Authentication in Django REST Framework]]
- [[10.Security Checklist]] · [[2.Errors You Will Actually Hit]]
