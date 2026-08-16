---
title: "Refactoring"
section: "12.Build ingredian API"
stage: 3
status: growing
tags: [drf, django, ingredients, refactoring]
updated: 2026-08-16
---
## 🟦 What Is Refactoring?

Refactoring = **restructuring code** without changing its external behavior.

### Goals of refactoring:

- Improve **readability**
    
- Improve **performance**
    
- Reduce **code duplication**
    
- Make the codebase easier to maintain and extend
    

**Important:**

> After refactoring, the code should behave _exactly the same_ as before.

---

## 🟩 Why TDD Helps Refactoring

- Tests validate the current behavior **before** refactoring.
    
- If refactoring breaks something → tests will fail immediately.
    
- Makes it safe to reorganize or optimize code.
    

Workflow:

1. Write tests → ensure they pass.
    
2. Refactor implementation.
    
3. Run tests again → confirm behavior is unchanged.
    

---

## 🟧 Areas Identified for Refactoring

The next lesson will focus on simplifying duplicated logic.

### Specifically:

### **TagViewSet** and **IngredientViewSet**

- These two viewsets share **very similar code**.
    
- This duplication can be improved by **using inheritance**.
    
- Goal: Move shared functionality into a **base viewset**, then let Tag & Ingredient viewsets inherit from it.
    

Benefit:

- Less repeated code
    
- Easier to read
    
- Easier to maintain
    

---

## 🟪 Summary

Refactoring:

- Improves code without changing behavior
    
- Works best with TDD
    
- Next lesson: Refactor Tag & Ingredient endpoints using inheritance
