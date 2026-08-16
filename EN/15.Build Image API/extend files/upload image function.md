---
title: "upload image function"
section: "15.Build Image API"
stage: 3
status: growing
tags: [drf, django, images, file-upload, media]
updated: 2026-08-16
---
## The Decorator

```python
@action(methods= ['POST'], detail=True, url_path='upload-image')
```

- `@action`: Creates a **custom endpoint** (not the default list/create/retrieve/update/delete)
- `methods=['POST']`: Only accepts **POST** requests (not GET, PUT, etc.)
- `detail=True`: This action works on a **specific recipe** (needs an ID in URL)
    - Example URL: `/api/recipe/5/upload-image/`
    - If `detail=False`: would be `/api/recipe/upload-image/` (no ID needed)
- `url_path='upload-image'`: The **URL suffix** for this action

## Function Definition

```python
def upload_image(self, request, pk = None):
    """upload an image to recipe"""
```

- `self`: The viewset instance
- `request`: The HTTP request object (contains the uploaded image)
- `pk = None`: **Primary Key** (recipe ID) - comes from the URL
    - Example: URL `/api/recipe/5/upload-image/` → `pk=5`

## Get the Recipe Object

```python
    recipe = self.get_object()
```

- **Retrieves the recipe** from the database using the `pk` from the URL
- `get_object()` is a DRF method that:
    - Looks up the recipe by ID
    - Checks permissions (does user have access?)
    - Returns 404 if recipe doesn't exist
    - Example: Gets the Recipe with `id=5`

## Create the Serializer

```python
    serializer = self.get_serializer(recipe, data = request.data)
```

- `self.get_serializer()`: Gets the **serializer class** defined in your viewset
- `recipe`: The **existing recipe instance** we're updating (not creating new)
- `data = request.data`: The **uploaded data** from the request
    - Contains: `{'image': <uploaded_file>}`
- This prepares to **update** the recipe's image field

## Validate and Save

```python
    if serializer.is_valid():
```

- Checks if the uploaded data is **valid**:
    - Is it an actual image file?
    - Is the format allowed (jpg, png, etc.)?
    - Does it meet size requirements?

```python
        serializer.save()
```

- **Saves the image** to the recipe
- Behind the scenes:
    - Generates unique filename using `uuid.uuid4()`
    - Saves file to disk (e.g., `uploads/recipe/a3f5b2c1...jpg`)
    - Updates the database with the file path

```python
        return Response(serializer.data, status= status.HTTP_200_OK)
```

- Returns **success response** (HTTP 200)
- `serializer.data`: Contains the updated recipe data including the new image URL
    - Example: `{'id': 5, 'title': 'Pizza', 'image': '/media/uploads/recipe/a3f5b2c1...jpg'}`

## Handle Validation Errors

```python
    return Response(serializer.errors, status= status.HTTP_400_BAD_REQUEST)
```

- If validation **failed**, return error message (HTTP 400)
- `serializer.errors`: Contains what went wrong
    - Example: `{'image': ['Upload a valid image.']}`

## Complete Flow Example

```python
# User makes request:
POST /api/recipe/5/upload-image/
Content-Type: multipart/form-data
Body: image=<pizza.jpg file>

# Code execution:
1. pk=5 extracted from URL
2. recipe = Recipe.objects.get(id=5)  # get_object()
3. serializer validates the uploaded image
4. Image saved to: uploads/recipe/a3f5b2c1-4d8e-4a2b-9c1f-8e7d6f5a4b3c.jpg
5. Database updated with new image path
6. Response: {"id": 5, "image": "/media/uploads/recipe/a3f5b2c1...jpg"}
```

## Key Points

- This is a **PATCH/UPDATE** operation (updating existing recipe's image)
- `detail=True` means you **must** provide a recipe ID in the URL
- The actual file saving is handled by Django/DRF using the `recipe_image_file_path` function you showed earlier
- If upload fails (wrong file type, too big, etc.), user gets a clear error message