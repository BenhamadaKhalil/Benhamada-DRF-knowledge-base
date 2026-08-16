---
title: "Recipe API – Overview"
section: "12.Build Recipe API"
stage: 3
status: growing
tags: [drf, django, recipe-api, viewsets, serializers]
updated: 2026-08-16
---
## 📌 Purpose of This Section

In this part of the course, we build a **Recipe API** with full CRUD functionality.  
All actions are restricted to the **authenticated user** (only the logged-in user can manage their own recipes).

---

## 🔧 Features of the Recipe API

The API will support the following operations:

- ✅ Create a recipe
    
- ✅ List all recipes
    
- ✅ View detailed information of a recipe (by ID)
    
- ✅ Update a recipe (using PUT or PATCH)
    
- ✅ Delete a recipe
    

All recipes are linked to the **current authenticated user**.

---

## 📡 API Endpoints

### 1. `/recipes/`

Used to manage the list of recipes.

**Allowed methods:**

- `GET` → List all recipes of the authenticated user
    
- `POST` → Create a new recipe
    

---

### 2. `/recipes/{id}/`

Used to manage a specific recipe.

**Allowed methods:**

- `GET` → Get detailed information of a single recipe
    
- `PUT` → Fully update a recipe
    
- `PATCH` → Partially update a recipe
    
- `DELETE` → Remove a recipe
    

---

## 🧠 Important Notes

- All endpoints are **protected** → user must be authenticated.
    
- Users can only access **their own recipes**.
    
- `PUT` replaces the whole object.
    
- `PATCH` updates specific fields only.
