---
title: "Create Ingredients Feature — Implementation Notes"
section: "14.Build Ingredient API"
stage: 3
status: growing
tags: [drf, django, ingredients, refactoring]
updated: 2026-08-16
---
## 1. Modify `RecipeSerializer`

### Add `ingredients` field

```python
ingredients = IngredientSerializer(
    many=True,
    required=False,
)
```

### Update `fields`

Add `"ingredients"` at the end of the `fields` list.  
(Keep line length under 80 chars → split into new lines.)

---

## 2. Add internal method: `_get_or_create_ingredients`

Purpose: Retrieve or create ingredient objects and attach them to a recipe.

### Method definition

```python
def _get_or_create_ingredients(self, ingredients, recipe):
    """Handle getting or creating ingredients as needed."""
    auth_user = self.context['request'].user

    for ingredient in ingredients:
        ingredient_obj, created = Ingredient.objects.get_or_create(
            user=auth_user,
            **ingredient,
        )
        recipe.ingredients.add(ingredient_obj)
```

### Notes

- Leading underscore (`_`) indicates **private/internal method**.
    
- Should **not** be called by external code; only inside the serializer.
    
- Helps refactoring—safe to rename internally.
    

---

## 3. Update `create()` method

Inside `RecipeSerializer.create`:

### Extract ingredients from `validated_data`

```python
ingredients = validated_data.pop('ingredients', [])
```

### After handling tags

Call internal ingredient handler:

```python
self._get_or_create_ingredients(ingredients, recipe)
```

---

## 4. Purpose of Logic

- Accept nested ingredient data with a recipe.
    
- For each ingredient:
    
    - Try to **get** if it exists.
        
    - Else **create** a new one.
        
- Attach the resulting ingredient object to the recipe.
    

---
