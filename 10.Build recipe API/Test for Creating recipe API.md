### **Key Concepts Explained:**

**1. Writing the Test (Test-Driven Development)**

- They're writing a test BEFORE implementing the actual feature
- The test is called `test_create_recipe`
- They intentionally DON'T use a helper function because they want to test the real API

**2. The Payload**

```python
payload = {
    'title': 'Sample recipe',
    'time_minutes': 30,
    'price': Decimal('5.99')
}
```

This is the recipe data being sent to the API.

**3. Making a POST Request**

- `self.client.post(RECIPES_URL, payload)` - sends data to create a recipe
- Should return HTTP 201 (Created) status code

**4. Verification Steps:**

- Check if response status is 201 (success)
- Retrieve the created recipe from database using the returned ID
- Loop through the payload and verify each field was saved correctly
- Check that the recipe belongs to the authenticated user

**5. The `getattr()` Function** This is a Python built-in that lets you access object attributes dynamically:

- `recipe.title` ← normal way
- `getattr(recipe, 'title')` ← dynamic way using a variable

This is needed because in the loop, they're using variables (k) for field names.

### **Why the Test Fails**

At the end, the test fails with an "integrity error" because they haven't yet written the code to automatically assign the authenticated user to the recipe when it's created. That's what they'll fix in the next lesson!

**This is normal in Test-Driven Development (TDD)** - write test first, watch it fail, then write code to make it pass.