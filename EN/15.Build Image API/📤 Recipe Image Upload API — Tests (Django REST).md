---
title: "📤 Recipe Image Upload API — Tests (Django REST)"
section: "15.Build Image API"
stage: 3
status: growing
tags: [drf, django, images, file-upload, media]
updated: 2026-08-16
---
## Goal

Add **API support for uploading images to recipes**, starting with **unit tests (TDD)** before implementing the endpoint.

---

## Step 1: Test Setup Imports

In `recipe/tests/test_recipe_api.py`:

```python
import tempfile
import os
from PIL import Image
```

- `NamedTemporaryFile` → create temp files for testing
    
- `PIL.Image` → generate fake images
    
- `os` → check file existence
    

---

## Step 2: Helper Function for Upload URL

```python
def image_upload_url(recipe_id):
    """Create and return image upload URL"""
    return reverse('recipe:recipe-upload-image', args=[recipe_id])
```

Purpose:

- Generates the URL for the image upload endpoint
    
- Keeps tests clean and reusable
    

---

## Step 3: Image Upload Test Class

```python
class ImageUploadTests(TestCase):
    """Tests for the image upload API"""
```

### `setUp`

```python
self.client = APIClient()
self.user = get_user_model().objects.create_user(
    'user@example.com',
    'password123'
)
self.client.force_authenticate(self.user)
self.recipe = create_recipe(user=self.user)
```

What happens:

- Create API client
    
- Create authenticated user
    
- Create a recipe owned by that user
    

---

### `tearDown`

```python
self.recipe.image.delete()
```

Why:

- Deletes uploaded images after each test
    
- Prevents test files accumulating on disk
    
- Keeps test environment clean
    

---

## Step 4: Test Successful Image Upload

```python
def test_upload_image(self):
    """Test uploading an image to a recipe"""
```

### Key Steps

1. Generate upload URL
    
2. Create a temporary image file
    
3. Save a small test image using Pillow
    
4. Upload via multipart form
    
5. Refresh recipe from DB
    
6. Validate response and file existence
    
> Note: for more understanding function go to [[test uploading image]] 
### Core Assertions

```python
self.assertEqual(res.status_code, status.HTTP_200_OK)
self.assertIn('image', res.data)
self.assertTrue(os.path.exists(self.recipe.image.path))
```

---

## Step 5: Test Invalid Image Upload

```python
def test_upload_image_bad_request(self):
    """Test uploading invalid image"""
```

### Payload

```python
payload = {'image': 'not an image'}
```

### Expected Result

```python
self.assertEqual(res.status_code, status.HTTP_400_BAD_REQUEST)
```

Purpose:

- Ensure API rejects invalid image data
    
- Confirms validation is working correctly
    

---

## Why Multipart Form Is Used

- Image uploads contain binary data
    
- Best practice for DRF file uploads
    
- Simulates real browser behavior
    

---

## Test Results (Expected)

- Tests **fail initially** with:
    
    - `NoReverseMatch`
        
- This is expected because the endpoint is **not implemented yet**
    

---

## Key Takeaways

- Use TDD: write tests before implementation
    
- Temporary files prevent disk pollution
    
- Pillow is ideal for creating test images
    
- Always test both **success** and **failure** cases
    
- Multipart uploads are required for file uploads
    

---

## Next Step

👉 Implement the **recipe image upload API endpoint** (view + serializer).

---

If you want next, I can:  
✅ Write the image upload serializer  
✅ Implement the API view action  
✅ Explain how React uploads images to this endpoint