---
title: "Filtering & API Improvements"
section: "14.Implement Filtering"
stage: 3
status: growing
tags: [drf, django, filtering, query-params]
updated: 2026-08-16
---
## 1️⃣ Filter Recipes by Tags & Ingredients

We implemented the ability to **filter recipes** using:

- Tags
    
- Ingredients
    

### Why this matters

- Allows more powerful queries (e.g. _“show me vegan dinner recipes”_)
    
- Improves API usability for frontend filtering and search
    
- Reduces unnecessary data transfer to the client
    

Example use case:

```text
GET /api/recipes/?tags=1,2&ingredients=3
```

---

## 2️⃣ Filter Tags & Ingredients Assigned to Recipes

We added filtering to:

- Return **only tags** that are linked to recipes
    
- Return **only ingredients** that are linked to recipes
    

### Why this matters

- Prevents showing unused tags/ingredients
    
- Makes UI dropdowns and filters cleaner
    
- Improves performance and user experience
    

Example:

```text
GET /api/tags/?assigned_only=1
```

---

## 3️⃣ Customized OpenAPI Schema

We enhanced the **OpenAPI (Swagger) schema** to document:

- Available filters
    
- Expected query parameters
    
- How to use filtering correctly
    

### Why this matters

- Helps frontend developers understand the API faster
    
- Reduces confusion and incorrect API usage
    
- Improves long-term maintainability of the project
    

---

## ✅ Section Summary

In this section we focused on:

- Advanced filtering logic
    
- Cleaner API responses
    
- Better developer experience through documentation
    
---
