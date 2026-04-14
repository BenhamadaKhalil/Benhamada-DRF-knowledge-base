## 🧩 What is a View?

A **view** is the code that handles a request sent to a URL.

- In classic Django → uses **functions**
    
- In Django REST Framework (DRF) → mainly uses **class-based views**
    
- A view receives the request and returns a response
    

---

## ⚙️ APIView

### What it is

`APIView` is a **class-based view** from DRF that focuses on **HTTP methods**.

### Supported methods

You define methods like:

- `get()`
    
- `post()`
    
- `put()`
    
- `patch()`
    
- `delete()`
    

### When to use APIView ✅

Use it when the endpoint:

- Is **not tied to a database model**
    
- Needs **custom logic**
    
- Doesn’t follow standard CRUD
    

### Common use cases

Good for:

- Authentication (login/register)
    
- Background jobs
    
- Calling external APIs
    

---

## 🏗️ ViewSet

### What it is

A **ViewSet** focuses on **actions**, not raw HTTP methods.

Instead of `get/post`, you use:

- `list()` → list all
    
- `retrieve()` → get one by ID
    
- `create()` → create
    
- `update()` → full update
    
- `partial_update()` → partial update
    
- `destroy()` → delete
    

These actions are automatically mapped to:  
`GET`, `POST`, `PUT`, `PATCH`, `DELETE`

---

## 🔄 ViewSet + Router (Auto URLs)

ViewSets work with **Routers** which:

- Automatically generate URL patterns
    
- Save time
    
- Require less manual configuration
    

You don’t have to manually define each route.

---

## ⚖️ When to use what?

|Use Case|Best Choice|
|---|---|
|Custom logic, not tied to a model|`APIView`|
|Standard CRUD on a model|`ViewSet`|

---

## ✅ Example Rule of Thumb

Use:

- `APIView` → for **custom endpoints** like login, register, special actions
    
- `ViewSet` → for **models like Recipe / Product / User**
    

---

If you want, I can also give you:

- Visual flow diagram
    
- Django code examples
    
- Or comparison cheatsheet