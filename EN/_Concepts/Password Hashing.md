---
title: Password Hashing
type: concept
tags: [concept, django, security, passwords, auth]
updated: 2026-08-16
---

# 🔐 Password Hashing

Django never stores a password. It stores a **one-way hash** — PBKDF2 with SHA-256 and a per-user salt, by default.

```text
pbkdf2_sha256$390000$KxvT2s...$9zN4bM...
    │            │        │        │
 algorithm  iterations   salt     hash
```

```python
user.set_password(raw_password)     # ✅ hashes and stores
user.check_password(raw_password)   # ✅ hashes the input and compares

user.password = raw_password        # ❌ stores plaintext. Nothing will warn you.
```

That last line is the mistake, and it's silent. The user "logs in" successfully during your manual test — because `check_password` fails, but you never checked.

## Where it happens in this project

```python
# app/core/models.py — UserManager
def create_user(self, email, password=None, **extra_fields):
    user = self.model(email=self.normalize_email(email), **extra_fields)
    user.set_password(password)
    user.save(using=self._db)
    return user
```

```python
# app/user/serializers.py — bypass ModelSerializer's plain assignment
def create(self, validated_data):
    return get_user_model().objects.create_user(**validated_data)


def update(self, instance, validated_data):
    password = validated_data.pop("password", None)
    user = super().update(instance, validated_data)

    if password:
        user.set_password(password)
        user.save()

    return user
```

**Why the `update()` override exists:** `ModelSerializer.update()` does `setattr(instance, attr, value)` for every field — which would assign the raw password straight onto the column.

## The rest of the checklist

- `extra_kwargs = {"password": {"write_only": True, "min_length": 5}}` — never return it
- Enable `AUTH_PASSWORD_VALIDATORS` — length, common-password and numeric-only checks
- Never log a request body containing a password
- **Generic login errors.** `"Unable to log in with provided credentials."` — never `"No user with that email"`, which is a free account-enumeration oracle

> [!CAUTION] Hashing is not encryption
> There is no "decrypt". If a user forgets their password you issue a reset token; you cannot recover the original. Any service that emails you your existing password is storing it reversibly.

---

## 🔗 Deeper

- [[Custom User Model]] · [[5.Implementing Create User API]] · [[9.Implement “Manage User”]]
- [[10.Security Checklist]]
