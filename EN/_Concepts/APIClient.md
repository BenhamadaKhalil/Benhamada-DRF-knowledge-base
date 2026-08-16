---
title: APIClient
aliases: ["DRF APIClient", "APIClient DRF", "Django API Testing", "Django REST Framework Testing", "Django Testing", "API Testing DRF", "TDD in Django"]
type: concept
tags: [concept, drf, testing, tdd]
updated: 2026-08-16
---

# 🧪 APIClient

DRF's test client. Like Django's, but it speaks JSON and knows about DRF authentication.

```python
from rest_framework.test import APIClient

self.client = APIClient()
```

```python
self.client.get(URL)
self.client.get(URL, {"tags": "1,2"})                    # query params
self.client.post(URL, payload)
self.client.post(URL, payload, format="json")            # nested data
self.client.post(URL, {"image": f}, format="multipart")  # file upload
self.client.patch(url, {"title": "New"})
self.client.delete(url)
```

**Two ways to authenticate:**

```python
self.client.force_authenticate(self.user)                          # skip the token entirely
self.client.credentials(HTTP_AUTHORIZATION="Token " + token.key)   # exercise the real header
```

Use [[force_authenticate]] when you're testing an endpoint. Use `credentials()` when you're testing the auth machinery itself — once, in its own test.

> [!WARNING] `format="json"` for nested payloads
> The default encoding flattens nested structures. `{"tags": [{"name": "Vegan"}]}` arrives mangled and the test fails for a reason unrelated to your code.

## The house pattern

Every API test file in this project splits into two classes:

```python
class PublicRecipeApiTests(TestCase):
    """Unauthenticated. The test everyone skips."""

    def test_auth_required(self):
        res = self.client.get(RECIPES_URL)
        self.assertEqual(res.status_code, status.HTTP_401_UNAUTHORIZED)


class PrivateRecipeApiTests(TestCase):
    """Authenticated."""

    def setUp(self):
        self.client = APIClient()
        self.user = create_user()
        self.client.force_authenticate(self.user)
```

The strongest list assertion available compares against the serializer — fields, values and shape in one line:

```python
serializer = RecipeSerializer(Recipe.objects.all().order_by("-id"), many=True)
self.assertEqual(res.data, serializer.data)
```

---

## 🔗 Deeper

- [[5.Testing Patterns]] — the full set of idioms
- [[Test Driven Development]] · [[3.What Is Mocking]]
- [[force_authenticate]] · [[reverse URL resolution]]
