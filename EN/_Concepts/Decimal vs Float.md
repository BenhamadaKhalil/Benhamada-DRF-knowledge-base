---
title: Decimal vs Float
type: concept
tags: [concept, python, django, models, money]
updated: 2026-08-16
---

# 💰 Decimal vs Float

```python
>>> 0.1 + 0.2
0.30000000000000004
```

That is not a Python bug. Binary floating point **cannot represent 0.1 exactly**, any more than decimal can represent ⅓ exactly. Every arithmetic operation carries a tiny error, and the errors accumulate.

For a physics simulation that's fine. For money it is a defect.

```python
price = models.FloatField()                                   # ❌
price = models.DecimalField(max_digits=5, decimal_places=2)    # ✅
```

`DecimalField` maps to Postgres `numeric` — arbitrary-precision, exact decimal arithmetic. `max_digits=5, decimal_places=2` means up to `999.99`.

```python
from decimal import Decimal

Decimal("0.1") + Decimal("0.2")     # Decimal('0.3') ✅
Decimal(0.1)                        # Decimal('0.1000000000000000055511151231257827') ❌
```

> [!WARNING] Construct `Decimal` from a **string**
> Passing a float hands the error in before you start. In tests: `create_recipe(price=Decimal("5.25"))`, never `Decimal(5.25)`.

## It serializes as a string

```json
{ "price": "5.25" }
```

Deliberately — turning it into a JSON number would hand it back to the same float problem in the client. To change it:

```python
REST_FRAMEWORK = {"COERCE_DECIMAL_TO_STRING": False}
```

Do that knowingly; it's a [[12.Versioning an API|breaking change]] for existing clients.

> [!TIP] The alternative: store integer cents
> Some teams use `PositiveIntegerField` for cents (`525` = £5.25) and format at the edge. Exact, fast, and impossible to misuse — at the cost of conversion everywhere. `DecimalField` is the better default; know this exists.

---

## 🔗 Deeper

- [[7.Model Field Reference]] · [[2.Serializer Field Matrix]]
- [[3.Creating the Recipe Model Test]] · [[2.Common Bugs and Fixes]]
