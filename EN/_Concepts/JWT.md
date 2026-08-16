---
title: JWT
type: concept
tags: [concept, drf, authentication, jwt, security]
updated: 2026-08-16
---

# 🎫 JWT (JSON Web Token)

Three base64 segments — `header.payload.signature`. The server verifies the signature and **trusts the contents**, with no database lookup.

```json
{ "token_type": "access", "exp": 1755360000, "user_id": 42, "role": "chef" }
```

That's the entire value proposition: **stateless verification**.

```bash
pip install djangorestframework-simplejwt
```

```python
SIMPLE_JWT = {
    "ACCESS_TOKEN_LIFETIME": timedelta(minutes=15),
    "REFRESH_TOKEN_LIFETIME": timedelta(days=7),
    "ROTATE_REFRESH_TOKENS": True,
    "BLACKLIST_AFTER_ROTATION": True,
}
```

```http
Authorization: Bearer eyJhbGciOi...
```

Note `Bearer` — [[Token Authentication|DRF tokens]] use `Token`. Mixing them up is the most common 401 there is.

## What it actually costs

> [!CAUTION] You cannot log someone out
> A stolen access token works until `exp`. Deleting it client-side is all you get. The short lifetime **is** the revocation story — you can't revoke it, so you make it die quickly.

| Problem | Detail |
| --- | --- |
| A denylist reintroduces the DB | `BLACKLIST_AFTER_ROTATION` needs a table and a lookup — the exact thing you chose JWT to avoid |
| `localStorage` is XSS-readable | any injected script exfiltrates it; `httpOnly` cookies bring CSRF back |
| Claims go stale | demote a user and their token still says `"role": "chef"` until it expires |
| `alg: none` | real historical CVEs in JWT libraries. Pin your algorithm. |

## When it's genuinely right

**When many services must verify the same identity without calling a shared auth service.** That's it. For a single Django service, one indexed primary-key lookup is well under a millisecond and is not your bottleneck.

"It's the standard" is not a reason.

---

## 🔗 Deeper

- [[6.Token vs JWT vs Session]] — the full comparison
- [[Token Authentication]] · [[SessionAuthentication]]
- [[3.Architecture Decision Records]]
