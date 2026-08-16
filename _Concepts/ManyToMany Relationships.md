---
title: ManyToMany Relationships
aliases: ["Django ManyToMany Field", "ManyToMany Relationships Django"]
type: concept
tags: [concept, django, models, relationships, orm]
updated: 2026-08-16
---

# 🔀 ManyToMany Relationships

**Many ↔ many.** A recipe has many tags; a tag belongs to many recipes. Django creates a hidden **join table** holding just the two IDs.

```python
tags = models.ManyToManyField("Tag")
ingredients = models.ManyToManyField("Ingredient")
```

```text
core_recipe_tags
├── id
├── recipe_id  →  core_recipe
└── tag_id     →  core_tag
```

## The manager API

```python
recipe.tags.all()          # read
recipe.tags.add(tag)       # link (idempotent — adding twice is a no-op)
recipe.tags.remove(tag)    # unlink
recipe.tags.set([t1, t2])  # replace the whole set
recipe.tags.clear()        # unlink all
tag.recipe_set.all()       # reverse
```

> [!WARNING] You cannot `.add()` before the row exists
> `recipe.tags.add(tag)` needs `recipe.pk`. Inside a serializer's `create()`, save the recipe first, then attach.

## The chained-filter trap

```python
# rows where ONE tag is both — usually zero
Recipe.objects.filter(tags__name="Vegan", tags__name="Dessert")

# rows having a vegan tag AND (separately) a dessert tag — what you meant
Recipe.objects.filter(tags__name="Vegan").filter(tags__name="Dessert")
```

Each `.filter()` on a M2M creates its own JOIN. This bites everyone exactly once.

## Performance

`select_related` **cannot** handle M2M — joining 100 recipes to 5 tags each returns 500 rows to rebuild 100 objects. Use a second query:

```python
Recipe.objects.prefetch_related("tags", "ingredients")
```

Writing tags through a recipe payload is [[Nested Serialization|nested serialization]], powered by [[get_or_create]].

---

## 🔗 Deeper

- [[Nested Serialization]] · [[4.Implementing Tag Creation]]
- [[4.The N+1 Problem]] · [[4.Django ORM Cookbook]]
- [[ForeignKey Relationships]] · [[Django Models]]
