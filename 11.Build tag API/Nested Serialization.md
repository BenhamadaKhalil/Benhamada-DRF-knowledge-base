### 🔹 What Is Nested Serialization?

- **Nested serialization** = using a serializer _inside_ another serializer.
    
- Instead of a simple field (string, int, boolean), a nested field represents a **complex object**.
    
- Example:
    
    - `title` → simple string
        
    - `tags` → list of objects, each with their own fields → **nested serializer**
        

---

### 🔹 Why Use Nested Serialization?

- Useful when an API returns or accepts **structured data**, not just primitive values.
    
- Helps express relationships between models, such as:
    
    - Recipe → Tags
        
    - User → Profile
        
    - Order → OrderItems
        

---

### 🔹 Example (Conceptual)

JSON response example:

```json
{
  "title": "Pizza",
  "user": "john",
  "tags": [
    { "name": "Italian" },
    { "name": "Dinner" }
  ]
}
```

Here, **tags** is a nested list of objects → handled by a nested serializer.

---

### 🔹 Serializer Structure

You normally define:

- **TagSerializer** → handles tag objects
    
- **RecipeSerializer** → includes a field like:
    
    ```python
    tags = TagSerializer(many=True)
    ```
    

This links the recipe serializer to the tag serializer → **nested serialization**.

---

### 🔹 Default Limitations

- In Django REST Framework, nested serializers are **read-only by default**.
    
    - You **can display** nested data.
        
    - You **cannot create or update** nested objects automatically.
        
- To make nested fields writable:
    
    - You must **override serializer methods** (e.g., `create()`, `update()`).
        
    - Implement custom logic to handle nested input.
        

---

### 🔹 What You’ll Do Next in the Course

- Begin implementing nested serializers inside the project.
    
- Learn how to handle nested data in API responses and (later) how to write custom logic to make them writable.
    

---
