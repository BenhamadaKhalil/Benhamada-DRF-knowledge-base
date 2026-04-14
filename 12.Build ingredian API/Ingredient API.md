# Ingredient API – Section Overview

## Purpose of This Section

- Add an **Ingredient API** to the existing **Recipe project**
    
- Enable users to:
    
    - Create ingredients
        
    - Assign ingredients to recipes
        
    - Manage ingredients independently or through recipes
        

---

## Features Being Added

### 1. Ingredient Support in Recipes

- Recipes can have **ingredients attached**
    
- Ingredients behave similarly to **tags**
    
- Ingredients are **user-specific**
    
    - Each user has their own ingredient list
        
    - Ingredients are linked to the user who created them
        

---

### 2. Ingredient Model Design

- Very basic model structure:
    
    - **name** — name of the ingredient
        
    - **user** — owner/creator of the ingredient
        
- One ingredient can be reused across multiple recipes
    
- Updating an ingredient updates it everywhere it’s used
    

---

## Ingredients API

### Ingredients List Endpoint

- Endpoint: `/recipes/ingredients/`
    
- Supported action:
    
    - **GET**
        
        - List all ingredients for the authenticated user
            

---

### Ingredient Detail Endpoint

- Endpoint: `/recipes/ingredients/{id}/`
    
- Supported actions:
    
    - **GET** — retrieve ingredient details
        
    - **PATCH / PUT** — update ingredient (e.g., fix name or spelling)
        
    - **DELETE** — remove ingredient
        
- Changes affect all recipes using that ingredient
    

---

## Updates to Recipe API

### Recipe Creation

- Modify **POST** on recipe endpoint
    
- Allow:
    
    - Creating ingredients at the same time as creating a recipe
        

---

### Recipe Detail Update

- Modify **PATCH** on recipe detail endpoint
    
- Allow:
    
    - Updating the ingredients associated with a recipe
        

---

## Summary

- Ingredients are:
    
    - User-owned
        
    - Reusable
        
    - Managed via both Ingredients API and Recipe API
        
- API flow mirrors the existing **tags** implementation
    

---