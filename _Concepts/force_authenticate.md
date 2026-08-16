---
title: force_authenticate
type: concept
tags: [concept, drf, testing, authentication]
updated: 2026-08-16
---

# 🎫 `force_authenticate`

Marks a test client as authenticated **without going through the token flow**.

```python
self.client = APIClient()
self.user = create_user()
self.client.force_authenticate(self.user)
```

Every subsequent request arrives with `request.user` set. No token row, no `Authorization` header, no login round trip.

**Why that's the right default:** you're testing the *endpoint*, not the auth system. Making twenty test cases each perform a real login makes them slower and couples them to something they aren't about.

**Test the auth machinery separately, once:**

```python
def test_create_token_for_user(self):
    """Test generating a token for valid credentials."""
    create_user(email="test@example.com", password="test-pass-123")

    res = self.client.post(TOKEN_URL, {
        "email": "test@example.com",
        "password": "test-pass-123",
    })

    self.assertIn("token", res.data)
    self.assertEqual(res.status_code, status.HTTP_200_OK)
```

Clear it to test the unauthenticated path:

```python
self.client.force_authenticate(user=None)
```

> [!WARNING] It bypasses authentication, not permissions
> Permission classes still run. `force_authenticate` gets you past step 4 of the request lifecycle; step 5 is unchanged — which is exactly what you want when testing whether user A can reach user B's data.

---

## 🔗 Deeper

- [[APIClient]] · [[5.Testing Patterns]]
- [[8.DRF – Tests for “Manage User” API]]
- [[Permissions]]
