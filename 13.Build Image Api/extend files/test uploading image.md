
## Test Setup

```python
def test_upload_imgae(self):
    """Test uploading an image to a recipe."""
    url = image_upload_url(self.recipe.id)
```

- Creates the URL endpoint for uploading an image to a specific recipe
- Example: `/api/recipe/5/upload-image/`

## Creating a Temporary Test Image

```python
    with tempfile.NamedTemporaryFile(suffix = '.jpg') as image_file:
```

- Creates a **temporary file** that will be **automatically deleted** when done
- `suffix='.jpg'` ensures the file has a `.jpg` extension
- `with` statement ensures cleanup even if test fails

```python
        img = Image.new('RGB', (10, 10))
```

- Creates a **fake image** in memory using Pillow library
- `'RGB'` = color mode (Red, Green, Blue)
- `(10, 10)` = image dimensions: 10 pixels wide × 10 pixels tall
- This is a tiny image just for testing

```python
        img.save(image_file, format='JPEG')
```

- Saves the fake image to the temporary file
- `format='JPEG'` specifies the image format

```python
        image_file.seek(0)
```

- **CRITICAL LINE**: Moves the file pointer back to the **beginning** of the file
- After writing, the pointer is at the **end** of the file
- Without this, Django would try to read from the end and get nothing!

## Making the Upload Request

```python
        payload = {'image': image_file}
        res = self.client.post(url, payload, format = 'multipart')
```

- `payload`: Dictionary with the image file
- `format='multipart'`: Tells Django REST Framework to send as **multipart/form-data** (required for file uploads, not JSON)

## Verifying the Upload Worked

```python
    self.recipe.refresh_from_db()
```

- **Reloads the recipe from the database** to get the updated `image` field
- Without this, `self.recipe` still has the old data from before the upload

```python
    self.assertEqual(res.status_code, status.HTTP_200_OK)
```

- Checks that the response was successful (200 OK)

```python
    self.assertIn('image', res.data)
```

- Checks that the response includes the `'image'` field
- This confirms the API returned the image URL

```python
    self.assertTrue(os.path.exists(self.recipe.image.path))
```

- Checks that the **image file actually exists on disk**
- `self.recipe.image.path` = full file system path like `/tmp/media/uploads/recipe/a3f5b2c1...jpg`
