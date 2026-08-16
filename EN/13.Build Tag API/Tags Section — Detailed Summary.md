---
title: "Tags Section — Detailed Summary"
section: "13.Build Tag API"
stage: 3
status: growing
tags: [drf, django, tags, nested-serialization]
updated: 2026-08-16
---
### 1. **Tag Model Implementation**

- Created a new **Tag** model in the database.
    
- Each tag:
    
    - Has a **name** field.
        
    - Is associated with a **specific user** (ensuring users only access their own tags).
        
- Added the necessary database migrations to create the `tags` table.
    

---

### 2. **Tags API Creation**

- Implemented a dedicated API endpoint to manage tags.
    
- Features include:
    
    - **List Tags:** Retrieve all tags belonging to the authenticated user.
        
    - **Create Tags:** Allow users to add new tags.
        
    - **Retrieve / Update / Delete Tags:** Full CRUD support depending on the design.
        
- Ensured the API:
    
    - Is **restricted to authenticated users**.
        
    - Only returns tags owned by the user.
        
    - Validates input using serializers.
        

---

### 3. **Serializer & View Enhancements**

- Built a **TagSerializer** to handle validation + representation.
    
- Added views using DRF generic viewsets (e.g., `ListCreateAPIView` or `ModelViewSet`).
    
- Connected views to the URL router for clean RESTful endpoints.
    

---

### 4. **Integrating Tags Into Recipes**

- Updated the recipe endpoint to **support tags**.
    
- Added a `tags` field to the Recipe serializer:
    
    - Supports assigning tags when creating or updating recipes.
        
    - Supports displaying tags when retrieving a recipe.
        
- Implemented logic to:
    
    - Prevent users from assigning tags that **don’t belong to them**.
        
    - Optionally allow **creating new tags on the fly** when posting a recipe (depending on the lesson implementation).
        

---

### 5. **Testing**

- Wrote unit tests for:
    
    - The Tag model.
        
    - Tag API endpoints (listing, creating, filtering).
        
    - Tag assignment in recipe operations.
        
- Ensured tests verify:
    
    - Unauthorized access is blocked.
        
    - Users only see their own tags.
        
    - Tags behave correctly when attached to recipes.
        

---

### 6. **What We Achieved**

- Added a fully functional **tagging system** to the API.
    
- Users can now:
    
    - Create and manage their own tags.
        
    - Attach tags to recipes.
        
    - Filter and organize recipes using tags (if implemented in the section).
        
- The database, API, and recipe logic are now aligned to support richer categorization.
    

---
