---
title: "User API design"
section: "11.Build User API"
stage: 3
status: growing
tags: [drf, django, user-api, authentication, serializers]
updated: 2026-08-16
---
# 👤 DRF – Tests for “Manage User” API (`/me/` Endpoint)

```yaml
Tags: [django, drf, testing, tdd, user-api, authentication]
Date: 2026-03-02
Topic: Write tests for Manage User (Retrieve/Update current user)
```

---

## 🎯 Core Concept

### ✅ Why?

We want a **“manage profile”** endpoint (commonly `/me/`) that lets a logged-in user:

- **GET** their own profile ✅
    
- **PATCH/PUT** update their profile ✅
    
- **POST** should be blocked ❌ (because we’re not creating a new user here)
    

We’re using **TDD**: write tests first → they fail (because URL/view not implemented yet) → then implement.

> [!INFO]  
> The point of these tests is to enforce **authentication rules** and **allowed HTTP methods** for the `/me/` endpoint.

---

## 🏗️ Logic Flow

```text
Client Request
  ↓
URL (reverse('user:me'))
  ↓
ManageUserView (protected)
  ↓
Serializer validates payload
  ↓
User instance updated (PATCH) OR returned (GET)
  ↓
Response (200 OK)
```

---

## 💻 Implementation (Tests)

📂 `app/user/tests/test_user_api.py`

### 1) Define the endpoint URL once (top of file)

```python
from django.urls import reverse

ME_URL = reverse('user:me')
```

> [!PROTIP]  
> Keeping URLs as constants avoids repeating `reverse(...)` in every test and makes refactors easy.

---

### 2) Public tests (unauthenticated)

```python
from django.test import TestCase
from rest_framework.test import APIClient
from rest_framework import status


class PublicUserApiTests(TestCase):
    """Test the public features of the user API."""

    def setUp(self):
        self.client = APIClient()

    def test_retrieve_user_unauthorized(self):
        """Test authentication is required for users."""
        res = self.client.get(ME_URL)

        self.assertEqual(res.status_code, status.HTTP_401_UNAUTHORIZED)
```

✅ What this enforces:

- If user is not logged in → `/me/` must return **401 Unauthorized**
    

> [!CAUTION]  
> If this test returns 200, your endpoint is accidentally public (security bug).

---

### 3) Private tests (authenticated)

We split authenticated tests into a separate class so `setUp()` can **force authentication once**.

```python
from django.contrib.auth import get_user_model

def create_user(**params):
    """Create and return a user."""
    return get_user_model().objects.create_user(**params)


class PrivateUserApiTests(TestCase):
    """Test API requests that require authentication."""

    def setUp(self):
        self.user = create_user(
            email='test@example.com',
            password='testpass123',
            name='Test Name',
        )
        self.client = APIClient()
        self.client.force_authenticate(user=self.user)
```

### 🔐 Why `force_authenticate`?

It **bypasses real login** and treats all requests from this client as authenticated.

> [!INFO]  
> Unit tests should be isolated: we’re testing `/me/` behavior, not re-testing authentication every time.

---

#### ✅ Test: Retrieve profile success (GET)

```python
    def test_retrieve_profile_success(self):
        """Test retrieving profile for logged in user."""
        res = self.client.get(ME_URL)

        self.assertEqual(res.status_code, status.HTTP_200_OK)
        self.assertEqual(
            res.data,
            {
                'name': self.user.name,
                'email': self.user.email,
            }
        )
```

✅ Enforces:

- Authenticated user can GET `/me/`
    
- Returned fields match the logged-in user (not someone else)
    

---

#### 🚫 Test: POST is not allowed on `/me/`

```python
    def test_post_me_not_allowed(self):
        """Test POST is not allowed on the /me/ endpoint."""
        res = self.client.post(ME_URL, {})

        self.assertEqual(res.status_code, status.HTTP_405_METHOD_NOT_ALLOWED)
```

✅ Enforces:

- `/me/` is not for creating objects
    
- Creation belongs in something like `/create/` or registration endpoint
    

---

#### 🔄 Test: Update profile (PATCH)

```python
    def test_update_user_profile(self):
        """Test updating the user profile for authenticated user."""
        payload = {
            'name': 'Updated name',
            'password': 'newpassword123',
        }

        res = self.client.patch(ME_URL, payload)

        self.user.refresh_from_db()  # Important!

        self.assertEqual(self.user.name, payload['name'])
        self.assertTrue(self.user.check_password(payload['password']))
        self.assertEqual(res.status_code, status.HTTP_200_OK)
```

✅ Enforces:

- PATCH updates name and password
    
- Password must be verified via `check_password()` (hashed in DB)
    
- Response is 200
    

> [!CAUTION]  
> `refresh_from_db()` is required because the `self.user` object in memory doesn’t auto-update after API calls.

---

## ⚡ Pro-Tips & Gotchas

> [!PROTIP]  
> **Split public/private tests**: it keeps tests clean and avoids repeating auth setup.

> [!PROTIP]  
> Use `force_authenticate()` for unit tests. Real login/token tests can exist separately.

> [!CAUTION]  
> Don’t compare stored password directly—always use:

- `user.check_password(...)`
    

> [!INFO]  
> Expected TDD outcome: tests fail with **NoReverseMatch** until you implement:

- the `user:me` URL pattern
    
- the view handling `/me/`
    

---

## 🧪 Expected Result When Running Tests (TDD)

When you run tests now, they should fail with something like:

- `NoReverseMatch` (because `user:me` URL isn’t implemented yet)
    

This is the correct next step before implementing the Manage User API.

---

## 🔗 Related Concepts

- [[DRF APIClient]]
    
- [[force_authenticate]]
    
- [[TDD]]
    
- [[RetrieveUpdateAPIView]]
    
- [[reverse URL resolution]]