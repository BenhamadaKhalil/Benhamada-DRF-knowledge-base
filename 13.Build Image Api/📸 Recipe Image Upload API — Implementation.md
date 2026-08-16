---
title: "📸 Recipe Image Upload API — Implementation"
section: "13.Build Image Api"
stage: 3
status: growing
tags: [drf, django, images, file-upload, media]
updated: 2026-08-16
---
## Goal

Implement the **API endpoint** for uploading images to recipes using Django REST Framework, based on the tests written earlier.

---

## Step 1: Create Image Upload Serializer

📁 `recipe/serializers.py`

```python
class RecipeImageSerializer(serializers.ModelSerializer):
    """Serializer for uploading images to recipes"""

    class Meta:
        model = Recipe
        fields = ['id', 'image']
        read_only_fields = ['id']
        extra_kwargs = {
            'image': {'required': True},
        }
```

### Why a separate serializer?

- Image upload only needs **one field** (`image`)
    
- Keeps API clean and focused
    
- Avoids mixing multipart image uploads with JSON recipe data
    
- Best practice: **one responsibility per endpoint**
    

---

## Step 2: Prepare View Imports

📁 `recipe/views.py`

Add required imports:

```python
from rest_framework import status
from rest_framework.decorators import action
from rest_framework.response import Response
```

---

## Step 3: Update `get_serializer_class`

Inside `RecipeViewSet`:

```python
def get_serializer_class(self):
        """Return the serializer class for request."""

        if self.action == 'list':
            return serializers.RecipeSerializer
        elif self.action == 'upload_image':
            return serializers.RecipeImageSerializer
```

Purpose:

- Use the **image serializer only** for the upload action
    
- Default serializers remain unchanged for other actions
    

---

## Step 4: Add Custom Action to ViewSet

```python
@action(
    methods=['POST'],
    detail=True,
    url_path='upload-image',
)
def upload_image(self, request, pk=None):
    """Upload an image to a recipe"""
    recipe = self.get_object()
    serializer = self.get_serializer(recipe, data=request.data)

    if serializer.is_valid():
        serializer.save()
        return Response(
            serializer.data,
            status=status.HTTP_200_OK
        )

    return Response(
        serializer.errors,
        status=status.HTTP_400_BAD_REQUEST
    )
```

### Key Concepts

- `@action` → adds custom endpoint to ViewSet
    
- `methods=['POST']` → only accepts POST
    
- `detail=True` → applies to a **single recipe**
    
- `url_path='upload-image'` → endpoint URL
    

📌 Final endpoint:

```
POST /api/recipe/recipes/{id}/upload-image/
```

>Note: for more understanding go to [[upload image function]]
---

## Step 5: Enable Multipart Support for API Docs

📁 `settings.py`

```python
SPECTACULAR_SETTINGS = {
    'COMPONENT_SPLIT_REQUEST': True,
}
```

Why:

- Required to upload images via **DRF browser interface**
    
- Ensures OpenAPI schema handles multipart requests correctly
    

---

## Step 6: Fix Typos & Run Tests

- Fixed typo: `request` misspelled
    
- Re-ran tests
    

```bash
docker-compose run --rm app python manage.py test
```

✅ All image upload tests passed successfully

---

## Key Takeaways

- Use a **dedicated serializer** for image uploads
    
- Custom actions extend ViewSet cleanly
    
- Multipart uploads require POST + `ImageField`
    
- Tests validate both success & error cases
    
- Small typos are caught quickly with TDD
    

---