---
title: "Technologies"
section: "1.APP Desing"
stage: 1
status: growing
tags: [drf, django, app-design, architecture]
updated: 2026-08-16
---
# Django Course — Technologies Overview (Python, Django, DRF, Postgres, Docker, etc.)

---

## 🎯 Overview

In this course, we will build an **API using Django**, and multiple technologies will work together to create a complete backend system.

Each technology has a specific role in the architecture.

---

## 🧱 Global Architecture (Big Picture)

```
Client (Browser / Mobile App)
            ↓
      Django REST Framework
            ↓
           Django
            ↓
          Python
            ↓
        Django ORM
            ↓
         PostgreSQL
```

Supporting tools:

- Docker → runs services
    
- Swagger UI → API documentation & testing
    
- GitHub Actions → automation (testing, CI/CD)
    

---

# 1️⃣ Python — Foundation of Everything

## 🎯 Definition

Python is the **programming language used to build the API**.

All Django code is written in Python.

## 🧠 Role in the system

Python handles:

- Business logic
    
- Data processing
    
- API behavior
    
- Communication with database
    

## 💻 Example

```python
def hello():
    return "Hello World"
```

In Django:

```python
from django.http import HttpResponse

def home(request):
    return HttpResponse("Hello from Django")
```

## 🧩 Mental Model

Python = The engine of the car  
Django = The car built using that engine

---

# 2️⃣ Django — The Web Framework

## 🎯 Definition

Django is a **Python web framework used to build websites and APIs quickly and efficiently**.

It provides built-in tools for common backend tasks.

---

## 🧠 Django handles:

|Feature|Role|
|---|---|
|URL routing|Connect URL → code|
|ORM|Database interaction|
|Admin panel|GUI to manage database|
|Views|Logic execution|
|Models|Database structure|

---

## 💻 Example

```python
# views.py
def home(request):
    return HttpResponse("Hello")
```

```python
# urls.py
path("", views.home)
```

---

## 🧩 Mental Model

Django = The backend framework that organizes everything

Without Django, you would need to build everything manually.

---

# 3️⃣ Django ORM — Database Interaction Layer

## 🎯 Definition

ORM = Object Relational Mapper

Allows you to interact with the database using Python instead of SQL.

---

## Without ORM (SQL)

```sql
SELECT * FROM users;
```

## With ORM (Python)

```python
User.objects.all()
```

---

## Benefits

|Without ORM|With ORM|
|---|---|
|SQL required|Python only|
|Harder|Easier|
|More errors|Safer|

---

## 🧩 Mental Model

ORM = Translator between Python and Database

---

# 4️⃣ Django Admin — Built-in Admin Panel

## 🎯 Definition

Django provides a **ready-to-use graphical interface** to manage database data.

Accessible in browser:

```
http://127.0.0.1:8000/admin/
```

---

## Features

You can:

- Add data
    
- Edit data
    
- Delete data
    
- Manage users
    

No frontend coding required.

---

## 🧩 Mental Model

Admin panel = Control panel for your database

---

# 5️⃣ Django REST Framework (DRF)

## 🎯 Definition

Django REST Framework is an extension of Django used to build REST APIs.

It adds tools specifically for API development.

---

## Without DRF

You must manually create API logic.

## With DRF

It provides:

- Serializers
    
- API Views
    
- Authentication tools
    
- API browsing interface
    

---

## Example API response

```json
{
  "id": 1,
  "name": "Khalil"
}
```

---

## 🧩 Mental Model

Django = builds websites  
DRF = builds APIs

---

# 6️⃣ PostgreSQL — Database

## 🎯 Definition

PostgreSQL is the database used to store application data.

---

## Stores things like:

- Users
    
- Products
    
- Orders
    
- API data
    

---

## Flow

```
Django Model → ORM → PostgreSQL
```

---

## 🧩 Mental Model

PostgreSQL = Storage system

Like a hard drive for your app data.

---

# 7️⃣ Docker — Containerization Software

## 🎯 Definition

Docker allows you to run your application and database in isolated environments called containers.

---

## Why use Docker?

Without Docker:

- Hard to configure environment
    
- Different behavior on each machine
    

With Docker:

- Same environment everywhere
    
- Easy deployment
    
- Easy setup
    

---

## Runs services like:

- Django API
    
- PostgreSQL database
    

---

## 🧩 Mental Model

Docker = Virtual box that contains your app

---

# 8️⃣ Swagger UI — API Documentation Tool

## 🎯 Definition

Swagger generates automatic documentation for your API.

---

## Features

You can:

- See all endpoints
    
- Test APIs directly
    
- View request and response format
    

---

## Example

```
GET /api/users/
POST /api/users/
```

---

## 🧩 Mental Model

Swagger = Interactive API manual

---

# 9️⃣ GitHub Actions — Automation Tool (CI/CD)

## 🎯 Definition

GitHub Actions automatically runs tasks when you push code.

---

## Example tasks

- Run tests
    
- Check code quality
    
- Deploy application
    

---

## Flow

```
You push code → GitHub Actions runs tests automatically
```

---

## 🧩 Mental Model

GitHub Actions = Robot that checks your code automatically

---

# 🔥 Complete Stack Summary Table

|Technology|Role|
|---|---|
|Python|Programming language|
|Django|Web framework|
|Django REST Framework|API framework|
|ORM|Database communication|
|PostgreSQL|Database|
|Docker|Environment & deployment|
|Swagger UI|API documentation|
|GitHub Actions|Automation|

---

# 🧠 How Everything Works Together

Step-by-step flow:

```
User sends request
      ↓
Django REST Framework receives request
      ↓
Django processes logic
      ↓
ORM communicates with PostgreSQL
      ↓
Data returned to user
```

---

# ❓ QCM — Test Your Understanding

## Question 1

What is the main programming language used in Django?

A. Java  
B. Python  
C. C++  
D. PHP

✅ Answer: B

---

## Question 2

What is Django?

A. Database  
B. Programming language  
C. Web framework  
D. Browser

✅ Answer: C

---

## Question 3

What is the role of PostgreSQL?

A. Run Python code  
B. Store data  
C. Handle URLs  
D. Build APIs

✅ Answer: B

---

## Question 4

What is Django REST Framework used for?

A. Build mobile apps  
B. Build APIs  
C. Build operating systems  
D. Build browsers

✅ Answer: B

---

## Question 5

What is Docker used for?

A. Write Python code  
B. Store images  
C. Run applications in containers  
D. Build frontend

✅ Answer: C

---

# ✅ Final Mental Model (IMPORTANT)

Think of it like a restaurant:

|Component|Role|
|---|---|
|Python|Chef|
|Django|Kitchen|
|DRF|Waiter (handles requests)|
|PostgreSQL|Storage room|
|Docker|Building|
|Swagger|Menu|
|GitHub Actions|Quality inspector|

---

Paste the next video subtitle when ready.