---
title: "🧱 Filtering Tags & Ingredients"
section: "16.Implement Filtering"
stage: 3
status: growing
tags: [drf, django, filtering, query-params]
updated: 2026-08-16
---
## 🎯 Goal of This Lesson

- Add an `assigned_only` filter to:
    
    - **Tags**
        
    - **Ingredients**
        
- Apply the change **once** using inheritance
    
- Document the filter clearly for frontend developers
    
- Keep behavior safe using tests (TDD)
    

---

## 1️⃣ Why Use a Base View (Inheritance)

### Base Class

`BaseRecipeAttrViewSet`

### Purpose

- Shared logic for:
    
    - TagViewSet
        
    - IngredientViewSet
        

### Why this is important

- Avoids code duplication
    
- One change applies to multiple endpoints
    
- Easier maintenance and fewer bugs
    

✅ **Best practice: DRY (Don’t Repeat Yourself)**

---

## 2️⃣ Reading the `assigned_only` Query Parameter

### Parameter

```text
assigned_only=0 | 1
```

### Implementation

- Read from `request.query_params`
    
- Default value: `0`
    
- Convert to boolean
    

```python
assigned_only = bool(
    int(self.request.query_params.get('assigned_only', 0))
)
```

### Why convert this way?

- Query params are always **strings**
    
- Frontend sends `0` or `1`
    
- Backend converts safely to `False / True`
    

---

## 3️⃣ Applying the Filter Conditionally

### Base QuerySet

```python
queryset = self.queryset
```

### Conditional Filtering

```python
if assigned_only:
    queryset = queryset.filter(recipe__isnull=False)
```

### What this means

- `assigned_only=1` → only return:
    
    - Tags used by at least one recipe
        
    - Ingredients used by at least one recipe
        
- `assigned_only=0` → return everything
    

### Why this matters for frontend

- Cleaner dropdowns
    
- No unused tags or ingredients
    
- Better UX
    

---

## 4️⃣ Returning the Final QuerySet

```python
return queryset.filter(
    user=self.request.user
).order_by('-id').distinct()
```

### Explanation

- `user=self.request.user`  
    → each user sees only their data
    
- `order_by('-id')`  
    → newest items first
    
- `distinct()`  
    → removes duplicates caused by joins
    

---

## 5️⃣ Why `distinct()` Is Required

### Problem

- One tag/ingredient can belong to **many recipes**
    
- SQL joins can return duplicate rows
    

### Solution

```python
.distinct()
```

✅ Guarantees **unique results**

---

## 6️⃣ Updating API Documentation (OpenAPI)

### Decorator Used

```python
@extend_schema_view(
    list=extend_schema(...)
)
```

### Documented Parameter

```python
OpenApiParameter(
    'assigned_only',
    OpenApiTypes.INT,
    enum=[0, 1],
    description='Filter by items assigned to recipes'
)
```

### Why this helps frontend developers

- Swagger shows:
    
    - Parameter name
        
    - Allowed values (`0` or `1`)
        
    - What the filter does
        
- No guessing
    
- No Slack/Discord questions
    

---

## 7️⃣ Test-Driven Development (TDD) Benefit

### What happened

- Feature added
    
- Tests confirmed behavior
    
- No regressions introduced
    

### Why this matters

- Confident refactoring
    
- Safe feature additions
    
- Production-ready code
    

---

## ✅ Final Result

After this lesson:

- Tags & ingredients can be filtered by usage
    
- One implementation serves multiple endpoints
    
- API docs clearly explain how to use the filter
    
- Frontend can implement without backend help
    

---

## 📌 Example Request (Frontend)

```text
GET /api/tags/?assigned_only=1
GET /api/ingredients/?assigned_only=1
```

---
