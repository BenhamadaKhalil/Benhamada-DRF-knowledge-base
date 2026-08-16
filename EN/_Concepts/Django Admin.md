---
title: Django Admin
type: concept
tags: [concept, django, admin]
updated: 2026-08-16
---

# 🎛️ Django Admin

The fastest return on effort in the whole framework: register a model, get a full add/edit/delete UI, write no HTML.

```python
# app/core/admin.py
from django.contrib import admin
from core import models

admin.site.register(models.Tag)
admin.site.register(models.Ingredient)
```

`/admin/core/tag/` now exists.

## Customising it

```python
class UserAdmin(BaseUserAdmin):
    ordering = ["id"]
    list_display = ["email", "name"]
    fieldsets = (
        (None, {"fields": ("email", "password")}),
        ("Personal Info", {"fields": ("name",)}),
        ("Permissions", {"fields": ("is_active", "is_staff", "is_superuser")}),
        ("Important dates", {"fields": ("last_login",)}),
    )
    readonly_fields = ["last_login"]
    add_fieldsets = (
        (None, {
            "classes": ("wide",),
            "fields": ("email", "password1", "password2", "name",
                       "is_active", "is_staff", "is_superuser"),
        }),
    )


admin.site.register(models.User, UserAdmin)
```

`fieldsets` is for **editing**, `add_fieldsets` for **creating**. A custom user model needs both — the default `UserAdmin` references `username`, which yours doesn't have, and the change page 500s until you fix it.

```bash
docker compose run --rm app sh -c "python manage.py createsuperuser"
```

> [!CAUTION] The admin is not an API, and it isn't safe by default
> - It uses [[SessionAuthentication|session auth]], so it needs CSRF
> - It bypasses your serializers entirely — validation you put there does **not** run here
> - `/admin/` on a predictable path is a login form on the public internet. Restrict by IP, or move it.

> [!WARNING] Bulk actions skip `save()`
> Admin bulk deletes and `QuerySet.update()` don't fire [[7.Signals|signals]] — cache invalidation and `auto_now` silently don't happen.

---

## 🔗 Deeper

- [[1.Overview & Customization]] · [[4.Fix the User Change Page]] · [[5.Support Creating Users]]
- [[Custom User Model]] · [[10.Security Checklist]]
