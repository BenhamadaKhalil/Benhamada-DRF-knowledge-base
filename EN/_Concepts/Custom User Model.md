---
title: Custom User Model
aliases: ["Django Custom User Model", "User Model Customization", "User Model in Django", "Django User Manager"]
type: concept
tags: [concept, django, user-model, auth, models]
updated: 2026-08-16
---

# 👥 Custom User Model

> **Do it on day one.** Swapping the user model after you have migrations and data is the single most painful refactor in Django.

Django's default `User` keys on **username**. Almost every modern app keys on **email**. That's the reason.

```python
# app/core/models.py
class UserManager(BaseUserManager):
    """Manager for users."""

    def create_user(self, email, password=None, **extra_fields):
        if not email:
            raise ValueError("User must have an email address.")
        user = self.model(email=self.normalize_email(email), **extra_fields)
        user.set_password(password)          # ← hashes it
        user.save(using=self._db)
        return user

    def create_superuser(self, email, password):
        user = self.create_user(email, password)
        user.is_staff = True
        user.is_superuser = True
        user.save(using=self._db)
        return user


class User(AbstractBaseUser, PermissionsMixin):
    """User in the system."""
    email = models.EmailField(max_length=255, unique=True)
    name = models.CharField(max_length=255)
    is_active = models.BooleanField(default=True)
    is_staff = models.BooleanField(default=False)

    objects = UserManager()

    USERNAME_FIELD = "email"
```

```python
# app/app/settings.py
AUTH_USER_MODEL = "core.User"
```

| Piece | Why |
| --- | --- |
| `AbstractBaseUser` | password handling + `last_login`, nothing else |
| `PermissionsMixin` | `is_superuser`, groups, permissions — needed for the admin |
| `UserManager` | `create_user` / `create_superuser` are called by Django and by `createsuperuser` |
| `normalize_email` | lowercases the domain so `A@Example.COM` and `a@example.com` aren't two accounts |
| `set_password` | hashes. **Never** assign `user.password = raw` |
| `USERNAME_FIELD` | what Django authenticates against |

> [!CAUTION] `InconsistentMigrationHistory`
> If you run `migrate` before setting `AUTH_USER_MODEL`, Django creates the default `auth.User` tables and refuses to switch. In development: drop the volume and start over. In production: it's a multi-step data migration nobody enjoys.

**Always reference it lazily:**

```python
user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)   # ✅
from core.models import User                                                   # ❌
```

---

## 🔗 Deeper

- [[1.Custom User Model in Django]] · [[2.Custom User Model – Design]]
- [[4.Implementing a Custom User Model]] · [[5.normalize email in `create_user`]]
- [[Django Models]] · [[Password Hashing]]
