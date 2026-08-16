---
title: "DRF Mastery Roadmap"
section: "you should master"
stage: 4
status: growing
tags: [drf, django, roadmap, learning]
updated: 2026-08-16
---
> **Goal:** Master the core concepts of Django REST Framework before building larger features such as products, orders, carts, payments, and notifications.

---

# Progress

## Completed ✅

- Custom User Model
    
- Custom User Manager
    
- Authentication with TokenAuthentication
    
- Login endpoint
    
- Logout endpoint
    
- Register endpoint
    
- Manage User endpoint
    
- Custom Permissions
    
- Nested Serializers
    
- Signals
    
- Transactions (`transaction.atomic()`)
    
- Generic API Views (basic)
    
- File Uploads (`MultiPartParser`)
    
- Cloudinary integration
    
- Flake8 formatting
    
- Project structure
    

---

# 1. Generic API Views ⭐⭐⭐⭐⭐

Generic views eliminate repetitive CRUD code.

## CreateAPIView

Used when the endpoint only creates objects.

Example:

```python
class ProductCreateView(generics.CreateAPIView):
    serializer_class = ProductSerializer
```

HTTP:

```
POST /products/
```

---

## ListAPIView

Returns all objects.

```python
class ProductListView(generics.ListAPIView):
    queryset = Product.objects.all()
```

HTTP:

```
GET /products/
```

---

## RetrieveAPIView

Returns one object.

```
GET /products/5/
```

---

## UpdateAPIView

Updates one object.

```
PUT /products/5/
PATCH /products/5/
```

---

## DestroyAPIView

Deletes one object.

```
DELETE /products/5/
```

---

## RetrieveUpdateAPIView

Allows

- GET
    
- PUT
    
- PATCH
    

Example:

```
/me/
```

---

## RetrieveUpdateDestroyAPIView

Allows

- GET
    
- PUT
    
- PATCH
    
- DELETE
    

Usually used for admin dashboards.

---

## ListCreateAPIView ⭐⭐⭐⭐⭐

Combines two views.

Instead of

```
GET /products/
POST /products/create/
```

REST style becomes

```
GET  /products/
POST /products/
```

One view handles both.

---

# 2. Serializer Validation ⭐⭐⭐⭐⭐

Validation is one of the most important serializer features.

---

## Field validation

Use

```python
validate_<field>()
```

Example

```python
def validate_phone_number(self, value):
    if len(value) != 10:
        raise serializers.ValidationError(
            "Phone number must contain 10 digits."
        )

    return value
```

Only validates one field.

---

## Object validation

Use

```python
validate()
```

Example

```python
def validate(self, attrs):
    if attrs["password"] == attrs["name"]:
        raise serializers.ValidationError(
            "Password cannot equal name."
        )

    return attrs
```

Used when multiple fields depend on each other.

---

# 3. Serializer Fields ⭐⭐⭐⭐⭐

## Read only

```python
id = serializers.IntegerField(
    read_only=True
)
```

Client cannot modify it.

---

## Write only

Example

```python
password = serializers.CharField(
    write_only=True
)
```

Client sends it.

API never returns it.

---

## SerializerMethodField ⭐⭐⭐⭐⭐

Creates calculated fields.

Example

```python
full_name = serializers.SerializerMethodField()

def get_full_name(self, obj):
    return f"{obj.first_name} {obj.last_name}"
```

No database field required.

---

## Nested serializers

Example

```python
user = UserSerializer()
```

Useful for related objects.

Later also learn

- PrimaryKeyRelatedField
    
- SlugRelatedField
    

---

# 4. ModelSerializer Meta Options

Already learned

- fields
    
- extra_kwargs
    

Still learn

## read_only_fields

```python
read_only_fields = ["id"]
```

---

## exclude

```python
exclude = ["password"]
```

---

## depth

Automatically serializes related models.

Example

```python
depth = 1
```

Useful for quick prototypes.

Avoid excessive use in production because it can return more data than intended.

---

# 5. Django Model Fields

Already know

- CharField
    
- EmailField
    
- BooleanField
    
- OneToOneField
    

Learn next

- ForeignKey ⭐⭐⭐⭐⭐
    
- ManyToManyField
    
- DecimalField
    
- DateTimeField
    
- DateField
    
- ImageField
    
- PositiveIntegerField
    
- TextField
    

ForeignKey is the most common relationship in Django.

Example

```
Category

↓

Product

↓

OrderItem
```

---

# 6. Model Field Options ⭐⭐⭐⭐⭐

## null

Database level.

Allows NULL values.

---

## blank

Validation level.

Allows empty values.

---

Difference

```
null

↓

Database
```

```
blank

↓

Forms & Serializers
```

---

Learn

```
choices
default
unique
db_index
verbose_name
help_text
auto_now
auto_now_add
```

Know what each one does and when to use it.

---

# 7. QuerySets ⭐⭐⭐⭐⭐

Master these methods.

```python
filter()
```

```python
exclude()
```

```python
get()
```

```python
exists()
```

```python
count()
```

```python
first()
```

```python
last()
```

Later learn performance methods

```python
select_related()
```

```python
prefetch_related()
```

These reduce database queries.

---

# 8. Permissions

Already created

- IsStaff
    
- IsBuyer
    
- IsTransporter
    
- IsPrevendeur
    

Also know DRF's built-in permissions.

```
AllowAny
```

```
IsAuthenticated
```

```
IsAdminUser
```

```
IsAuthenticatedOrReadOnly
```

---

# 9. Authentication

Already learned

```
TokenAuthentication
```

Know other authentication systems exist.

- SessionAuthentication
    
- JWT
    
- OAuth
    

You don't need to master them yet.

---

# 10. Response Status Codes ⭐⭐⭐⭐

Instead of

```python
Response(status=204)
```

prefer

```python
from rest_framework import status

Response(
    status=status.HTTP_204_NO_CONTENT
)
```

More readable.

Less error-prone.

---

# 11. Model Properties

Example

```python
@property
def full_name(self):
    return f"{self.first} {self.last}"
```

Very useful for computed values.

---

# 12. Signals

Already learned

```
post_save
```

Also know

```
pre_save
post_delete
pre_delete
```

Use signals only when they simplify your design. Don't force everything into signals.

---

# 13. Pagination ⭐⭐⭐⭐⭐

Every production API should paginate list endpoints.

Learn

```
PageNumberPagination
```

```
LimitOffsetPagination
```

```
CursorPagination
```

Example

```
GET /products/?page=2
```

---

# 14. Filtering & Ordering

Example

```
GET /products/?search=laptop
```

```
GET /products/?ordering=price
```

```
GET /products/?category=phones
```

Useful packages:

- django-filter
    
- SearchFilter
    
- OrderingFilter
    

---

# 15. ViewSets ⭐⭐⭐⭐⭐

Instead of writing

- CreateAPIView
    
- ListAPIView
    
- RetrieveAPIView
    
- UpdateAPIView
    

you can use

```python
ModelViewSet
```

One class provides all CRUD operations.

---

# 16. Routers

Instead of manually writing many URL patterns,

DRF can generate them.

Example

```python
router.register(
    "products",
    ProductViewSet
)
```

Cleaner.

Less code.

---

# 17. API Documentation

You're already using **drf-spectacular**.

Next learn

```python
@extend_schema
```

Useful for

- request examples
    
- response examples
    
- endpoint descriptions
    
- parameter documentation
    

---

# 18. Testing ⭐⭐⭐⭐⭐

Don't skip testing.

Learn

```
APIClient
```

Write tests for

- Register
    
- Login
    
- Logout
    
- Permissions
    
- Create Transporter
    
- Create Prevendeur
    
- Update User
    
- Unauthorized access
    

A good test suite lets you refactor confidently without breaking existing functionality.

---

# 19. Clean Code Habits

Always try to follow these practices:

- Keep one responsibility per class.
    
- Use descriptive names.
    
- Add docstrings to public classes and methods.
    
- Comment _why_, not _what_.
    
- Use transactions when multiple database writes must succeed together.
    
- Keep serializers responsible for validation and object creation.
    
- Keep views thin; put business logic in serializers, models, or dedicated services when appropriate.
    
- Run `flake8` regularly and fix issues as you go instead of letting them accumulate.
    

---

# Learning Order (Recommended)

1. Finish Generic API Views
    
2. Serializer Validation
    
3. Model Field Options
    
4. Serializer Fields
    
5. QuerySets
    
6. Pagination
    
7. Filtering & Ordering
    
8. Response Status Codes
    
9. Model Properties
    
10. ViewSets
    
11. Routers
    
12. API Documentation
    
13. Testing
    

---

# Final Goal

After completing this roadmap, you should feel comfortable building APIs for:

- User Management
    
- Products
    
- Categories
    
- Inventory
    
- Shopping Cart
    
- Orders
    
- Payments
    
- Delivery
    
- Reviews
    
- Notifications
    

At that point, you'll have a solid foundation in Django REST Framework and be ready to focus on application design rather than framework basics.