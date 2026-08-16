---
title: ObtainAuthToken
type: concept
tags: [concept, drf, authentication, token-auth]
updated: 2026-08-16
---

# 🎟️ `ObtainAuthToken`

DRF's built-in view for exchanging credentials for a token. You subclass it because the default authenticates on **username**, and this project uses **email**.

```python
# app/user/views.py
from rest_framework.authtoken.views import ObtainAuthToken
from rest_framework.settings import api_settings

from user.serializers import AuthTokenSerializer


class CreateTokenView(ObtainAuthToken):
    """Create a new auth token for user."""
    serializer_class = AuthTokenSerializer
    renderer_classes = api_settings.DEFAULT_RENDERER_CLASSES
```

```python
# app/user/serializers.py
class AuthTokenSerializer(serializers.Serializer):
    """Serializer for the user auth token."""
    email = serializers.EmailField()
    password = serializers.CharField(
        style={"input_type": "password"},
        trim_whitespace=False,
    )

    def validate(self, attrs):
        user = authenticate(
            request=self.context.get("request"),
            username=attrs.get("email"),
            password=attrs.get("password"),
        )

        if not user:
            raise serializers.ValidationError(
                "Unable to log in with provided credentials.",
                code="authorization",
            )

        attrs["user"] = user
        return attrs
```

Three details that matter:

| Detail | Why |
| --- | --- |
| `renderer_classes = api_settings.DEFAULT_RENDERER_CLASSES` | without it the view renders JSON only, and you lose it from the [[Browsable API]] |
| `trim_whitespace=False` | a trailing space is a legitimate password character; trimming it silently breaks login |
| `username=attrs.get("email")` | `authenticate()`'s parameter is still called `username`; it maps to whatever `USERNAME_FIELD` is |

> [!CAUTION] Keep the error message generic
> `"Unable to log in with provided credentials."` — never `"No user with that email."` The second one is a free account-enumeration oracle.

This endpoint is the **single most attacked route in your API.** Throttle it tightly — see [[2.Throttling and Rate Limiting]].

---

## 🔗 Deeper

- [[7.Custom Token Authentication]] · [[6.Token API Tests]]
- [[Token Authentication]] · [[Password Hashing]]
