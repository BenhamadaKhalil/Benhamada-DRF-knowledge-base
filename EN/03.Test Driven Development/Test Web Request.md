---
title: "Test Web Request"
section: "03.Test Driven Development"
stage: 1
status: growing
tags: [drf, django, tdd, testing]
updated: 2026-08-16
---
# 🧪 Test Driven Development (TDD) — Django Notes

> 💡 **Core idea:** Write tests first, then write code that makes the tests pass.

---

## 🧩 1. Unit Tests

## 📌 Definition

A **unit test** is code that verifies that another piece of code works correctly.

It tests small parts of your application such as:

- Functions
    
- Methods
    
- Models
    
- API endpoints
    

---

## ⚙️ How Unit Tests Work

Unit tests follow 3 steps:

### 1️⃣ Arrange → Prepare inputs

### 2️⃣ Act → Execute the code

### 3️⃣ Assert → Verify the result

---

## 💻 Example (Python)

Code:

```python
def add(a, b):
    return a + b
```

Test:

```python
def test_add():
    result = add(2, 3)
    assert result == 5
```

✔ Input: `2, 3`  
✔ Expected output: `5`  
✔ Assertion checks correctness

---

## 🌐 Django Example (API test)

```python
response = client.get("/api/users/")
assert response.status_code == 200
```

✔ Verifies that the API endpoint works correctly.

---

## ✅ Benefits of Unit Tests

- 🐞 Detect bugs early
    
- 🔒 Improve reliability
    
- 🔁 Allow safe code changes
    
- 🎯 Ensure code works correctly
    
- 😌 Give developer confidence
    

---

## 🧠 Mental Model

> 🛡️ Unit tests = Automatic protection system for your code

They immediately notify you if something breaks.

---

## 🚀 2. Test Driven Development (TDD)

## 📌 Definition

Test Driven Development is a method where you **write tests before writing the actual code**.

---

## 🔄 Traditional vs TDD

### ❌ Traditional approach

```
Write code
   ↓
Write tests
```

---

### ✅ TDD approach

```
Write test
   ↓
Write code
   ↓
Make test pass
```

---

## 🔁 3. TDD Cycle (Core Process)

This is the most important concept.

---

## 1️⃣ Write Test ✍️

Define expected behavior.

```python
def test_add():
    assert add(2, 3) == 5
```

---

## 2️⃣ Run Test → FAIL ❌

Test fails because feature doesn't exist yet.

This is expected.

---

## 3️⃣ Write Code ⚙️

Implement the feature.

```python
def add(a, b):
    return a + b
```

---

## 4️⃣ Run Test → PASS ✅

Feature works correctly.

---

## 5️⃣ Refactor Code 🔧

Improve code quality.

Example:

- Improve readability
    
- Optimize logic
    
- Clean structure
    

---

## 6️⃣ Run Tests Again 🔁

Ensure everything still works.

---

## 🔄 TDD Cycle Diagram

```
Write Test ✍️
    ↓
Run Test → FAIL ❌
    ↓
Write Code ⚙️
    ↓
Run Test → PASS ✅
    ↓
Refactor 🔧
    ↓
Run Test → PASS ✅
```

Repeat for every feature.

---

## 🌐 4. Django Example (Real Case)

## Goal: Create API endpoint

```
/api/users/
```

---

### Step 1 — Write test ✍️

```python
response = client.get("/api/users/")
assert response.status_code == 200
```

---

### Step 2 — Run test → FAIL ❌

Endpoint doesn't exist yet.

---

### Step 3 — Create endpoint ⚙️

Write Django view and URL.

---

### Step 4 — Run test → PASS ✅

Endpoint works.

---

## 🎯 5. Why TDD is Important

## Main advantages:

- 🐞 Reduces bugs
    
- 🧠 Improves code design
    
- 🔒 Makes refactoring safe
    
- 📈 Improves code quality
    
- 👨‍💻 Increases developer confidence
    

---

## 📊 6. Key Concepts Summary

|Concept|Meaning|
|---|---|
|🧪 Unit Test|Tests a small part of code|
|✔ Assertion|Checks expected result|
|🚀 TDD|Write tests before code|
|📊 Test Coverage|How much code is tested|
|🔧 Refactoring|Improving code safely|

---

## 🧠 Final Mental Model

## ❌ Traditional

```
Write code → Test later
```

## ✅ TDD

```
Write test → Write code → Verify → Improve
```

---

## 🧱 Developer Mindset with TDD

Instead of asking:

> "How do I write this feature?"

You ask:

> "How should this feature behave?"

Then you implement it correctly.


