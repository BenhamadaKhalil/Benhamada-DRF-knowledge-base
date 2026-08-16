---
title: "🖼️ Adding Images to Recipe Model (Django)"
section: "13.Build Image Api"
stage: 3
status: growing
tags: [drf, django, images, file-upload, media]
updated: 2026-08-16
---
## Goal

Allow the **Recipe model** to store uploaded images using Django’s `ImageField`, with a **unique file path** for each image.

---

## Step 1: Write the Unit Test First (TDD)

### Import mock tool

In `core/tests/test_models.py`:

```python
from unittest.mock import patch
```

### Test: generate image file path

```python
@patch('core.models.uuid.uuid4')
def test_recipe_image_file_path(self, mock_uuid):
    uuid = 'test-uuid'
    mock_uuid.return_value = uuid

    file_path = models.recipe_image_file_path(None, 'example.jpg')

    self.assertEqual(
        file_path,
        f'uploads/recipe/{uuid}.jpg'
    )
```

### Why mock `uuid4`?

- `uuid4()` generates random values
    
- Mocking ensures **predictable test results**
    
- Makes assertions reliable
    

---

## Step 2: Implement Image Path Function

### Imports in `core/models.py`

```python
import uuid
import os
```

### Image path generator function

```python
def recipe_image_file_path(instance, filename):
    """Generate file path for new recipe image"""
    ext = os.path.splitext(filename)[1]
    filename = f'{uuid.uuid4()}{ext}'

    return os.path.join('uploads', 'recipe', filename)
```

### Explanation

- Extract file extension (`.jpg`, `.png`, etc.)
    
- Generate a unique filename using UUID
    
- Preserve original file extension
    
- Use `os.path.join()` for OS compatibility
    
> Note: for more explanation go  [[path generator function]] 
---

## Step 3: Add Image Field to Recipe Model

```python
image = models.ImageField(
    null=True,
    upload_to=recipe_image_file_path
)
```

### Notes

- `null=True` → image is optional
    
- `upload_to` receives a **function reference**
    
- Django calls the function automatically on upload
    

---

## Step 4: Create Migrations

Because the model changed:

```bash
docker-compose run --rm app python manage.py makemigrations
```

Then apply migrations if needed.

---

## Step 5: Run Tests Again

```bash
docker-compose run --rm app python manage.py test
```

✅ All tests pass  
✅ Image upload support successfully added

---

## Key Takeaways

- Always write tests before adding features
    
- Use UUIDs to avoid filename collisions
    
- Mock random behavior in unit tests
    
- `ImageField + upload_to function` is the Django best practice
    
- Tests help catch hidden bugs early
    

---
