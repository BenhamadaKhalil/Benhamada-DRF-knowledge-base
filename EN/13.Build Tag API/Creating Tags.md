---
title: "Creating Tags"
section: "13.Build Tag API"
stage: 3
status: growing
tags: [drf, django, tags, nested-serialization]
updated: 2026-08-16
---
## 🏷️ Creating Tags When Creating a Recipe (Nested Writable Serializer)

### 🎯 Goal

Enable the API to **create new tags automatically when creating a recipe**, instead of only reading existing tags.

---

### 1️⃣ Preparing the Serializers File

- Open `serializers.py` in the `recipe` app.
    
- Move `TagSerializer` **above** `RecipeSerializer`.
    
- Reason:
    
    - `RecipeSerializer` uses `TagSerializer` as a **nested serializer**.
        
    - Python must define a class before it can be referenced.
        

---

### 2️⃣ Adding Tags as a Nested Field

Inside `RecipeSerializer`:

```python
tags = TagSerializer(many=True, required=False)
```

- `many=True` → a recipe can have multiple tags.
    
- `required=False` → tags are optional when creating a recipe.
    

Add `tags` to the serializer’s `fields` list to enable full management through the serializer.

---

### 3️⃣ Default Behavior of Nested Serializers

- Nested serializers in DRF are **read-only by default**.
    
- This means:
    
    - ✅ Tags can be viewed on a recipe.
        
    - ❌ Tags cannot be created or saved during recipe creation.
        
- To fix this, we must **override the `create()` method**.
    

---

### 4️⃣ Overriding the `create()` Method

A custom `create()` method is added to `RecipeSerializer` to support writable nested tags.

#### 🔑 Key Steps in the Logic:

#### ✅ Remove Tags from Validated Data

```python
tags = validated_data.pop('tags', [])
```

- Removes `tags` from `validated_data`.
    
- Prevents passing tags directly into the `Recipe` model (which would cause an error).
    
- Stores tags separately for later processing.
    

---

#### ✅ Create the Recipe First

```python
recipe = Recipe.objects.create(**validated_data)
```

- Creates the recipe using only its direct fields.
    
- Tags are handled _after_ the recipe exists.
    

---

#### ✅ Get the Authenticated User

```python
user = self.context['request'].user
```

- Serializers don’t automatically know the request.
    
- The `context` is passed from the view and gives access to the logged-in user.
    

---

#### ✅ Create or Reuse Tags

```python
for tag in tags:
    tag_obj, created = Tag.objects.get_or_create(
        user=user,
        **tag
    )
    recipe.tags.add(tag_obj)
```

- Loops through all provided tags.
    
- Uses `get_or_create()` to:
    
    - Prevent duplicate tags.
        
    - Reuse existing tags if they already exist.
        
- Associates each tag with the recipe.
    

✅ `**tag` future-proofs the code so new tag fields are automatically supported later, here with use [[Tuple Unpacking]] **go to page if you  want more information**

---

#### ✅ Return the Created Recipe

```python
return recipe
```

- Required for the serializer’s normal behavior.
    
- Must be **outside the loop**.
    

---

### 5️⃣ Why This Approach Works Well

- Prevents duplicate tags.
    
- Keeps tag creation user-specific.
    
- Allows flexible extension of the Tag model later.
    
- Fully supports nested writable data in DRF.
    

---

### 6️⃣ Final Result

✅ Recipes can now be created **with tags**  
✅ Tags are created automatically if they don’t already exist  
✅ Tests pass successfully

This completes the implementation of **writable nested serializers for tags**.

---
