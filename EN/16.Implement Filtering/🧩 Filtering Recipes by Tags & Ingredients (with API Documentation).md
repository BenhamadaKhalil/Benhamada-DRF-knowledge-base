---
title: "🧩 Filtering Recipes by Tags & Ingredients (with API Documentation)"
section: "16.Implement Filtering"
stage: 3
status: growing
tags: [drf, django, filtering, query-params]
updated: 2026-08-16
---
## 🎯 Goal of This Lesson

- Allow clients (frontend) to **filter recipes** by:
    
    - Tags
        
    - Ingredients
        
- Make these filters **discoverable and understandable** via OpenAPI docs
    
- Ensure changes are **safe** using tests (TDD)
    

---

## 1️⃣ Accepting Filter Parameters from the Request

### Query Parameters Used

- `tags`
    
- `ingredients`
    

Each parameter:

- Is a **comma-separated string**
    
- Contains **IDs**, not names  
    Example:
    

```text
GET /api/recipes/?tags=1,2&ingredients=3
```

### Why comma-separated strings?

- URLs only support strings
    
- Easy to pass multiple values
    
- Common REST API convention
    

---

## 2️⃣ Converting Query Params to Integers

### Helper Method

```python
_params_to_ints(self, qs)
```

### What it does

- Takes `"1,2,3"`
    
- Converts it into `[1, 2, 3]`
    

### Why this is needed

- Django ORM expects **integers** for ID filtering
    
- Prevents query errors
    
- Keeps filtering logic reusable and clean
    

---

## 3️⃣ Custom `get_queryset()` Logic

### Base Query

```python
queryset = self.queryset
```

This allows filters to be **added step by step**.

---

### Filtering by Tags

```python
if tags:
    tag_ids = self._params_to_ints(tags)
    queryset = queryset.filter(tags__id__in=tag_ids)
```

### Filtering by Ingredients

```python
if ingredients:
    ingredient_ids = self._params_to_ints(ingredients)
    queryset = queryset.filter(ingredients__id__in=ingredient_ids)
```

### Why this approach?

- Filters are **optional**
    
- Client can filter by:
    
    - Only tags
        
    - Only ingredients
        
    - Both
        
    - Neither (get all recipes)
        

---

## 4️⃣ Ordering & Removing Duplicates

```python
return queryset.filter(user=self.request.user)\
    .order_by('-id')\
    .distinct()
```

### Explanation

- `order_by('-id')`  
    → newest recipes first
    
- `distinct()`  
    → prevents duplicate recipes when:
    
    - Multiple tags or ingredients match the same recipe
        

---

## 5️⃣ Why Tests Caught a Bug (Important Lesson)

### What happened

- New filtering code accidentally broke existing ordering logic
    

### Why tests helped

- Existing tests failed immediately
    
- Bug was caught **before production**
    
- Confirms value of **Test Driven Development (TDD)**
    

---

## 6️⃣ Improving API Documentation (OpenAPI / Swagger)

### Decorator Used

```python
@extend_schema_view(
    list=extend_schema(...)
)
```

### Why `list`?

- Filters only apply to the **list endpoint**
    
- Not detail (`/recipes/{id}/`)
    

---

### Documented Parameters

#### Tags

```python
OpenApiParameter(
    'tags',
    OpenApiTypes.STR,
    description='Comma separated list of tag IDs to filter'
)
```

#### Ingredients

```python
OpenApiParameter(
    'ingredients',
    OpenApiTypes.STR,
    description='Comma separated list of ingredient IDs to filter'
)
```

---

## 7️⃣ Why This Helps Frontend Developers 🚀

Frontend devs can now:

- See available filters **directly in Swagger**
    
- Know:
    
    - Parameter names
        
    - Expected format
        
    - Data types
        
- Avoid guessing or backend questions
    

### Result

✅ Faster frontend development  
✅ Fewer bugs  
✅ Clear API contract

---

## ✅ Final Outcome of This Section

- Recipes can be filtered by tags & ingredients
    
- Filters are optional and composable
    
- API documentation clearly explains usage
    
- Tests ensure new features don’t break old ones
    

---
