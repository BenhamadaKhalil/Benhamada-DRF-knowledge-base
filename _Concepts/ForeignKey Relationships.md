---
title: ForeignKey Relationships
type: concept
tags: [concept, django, models, relationships, orm]
updated: 2026-08-16
---

# 🔗 ForeignKey Relationships

**Many → one.** Many recipes belong to one user. The `user_id` column lives on the *recipe* table.

```python
user = models.ForeignKey(
    settings.AUTH_USER_MODEL,
    on_delete=models.CASCADE,
    related_name="recipes",
)
```

```python
recipe.user           # forward — the one
user.recipes.all()    # reverse — the many (needs related_name; else user.recipe_set)
```

## `on_delete` — choose deliberately

| Value | When the parent is deleted |
| --- | --- |
| `CASCADE` | delete this row too |
| `PROTECT` | **refuse** to delete the parent |
| `SET_NULL` | set to `NULL` (needs `null=True`) |
| `SET_DEFAULT` | use the field's default |
| `DO_NOTHING` | leave a dangling FK — you're on your own |

`CASCADE` for owned data (a user's recipes). `PROTECT` for anything you'd hate to lose silently (orders referencing a product).

> [!TIP] Set `related_name`
> `user.recipes` reads; `user.recipe_set` doesn't — and the default breaks entirely the moment you have two FKs to the same model.

## The FK that makes this API safe

Every model here carries `user = ForeignKey(...)`, which is what lets every `get_queryset()` end in `.filter(user=self.request.user)` — object isolation with no per-object permission class.

## Performance

Traversing an FK in a loop triggers one query per row. Fetch them in the same query:

```python
Recipe.objects.select_related("user")     # SQL JOIN — see [[4.The N+1 Problem]]
```

`select_related` is for FK and OneToOne only. [[ManyToMany Relationships|M2M]] needs `prefetch_related`.

---

## 🔗 Deeper

- [[7.Model Field Reference]] · [[4.Django ORM Cookbook]]
- [[Django Models]] · [[ManyToMany Relationships]] · [[4.The N+1 Problem]]
