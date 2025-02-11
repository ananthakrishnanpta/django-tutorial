### Django Tutorial: Accessing and Manipulating `db.sqlite3` Through Command Line

---

#### Prerequisites
- Ensure Django is installed in your environment.
- Your project should have a configured `db.sqlite3` database.
- Have at least one Django app with models and migrations applied.

---

### 1. **Open the Django Shell**
To interact with the database, use the Django shell:
```bash
python manage.py shell
```

This opens an interactive Python shell with Django's environment loaded.

---

### 2. **Import Models**
To manipulate data, you need to import your models. Replace `your_app` with your app's name.

```python
from your_app.models import *
```

---

### 3. **Basic Database Operations**

#### a. Query All Data
Retrieve all records from a model:
```python
ModelName.objects.all()
```

Example:
```python
from your_app.models import User
User.objects.all()
```

#### b. Create New Records
Insert new data into the database:
```python
ModelName.objects.create(field1=value1, field2=value2, ...)
```

Example:
```python
User.objects.create(username="john_doe", email="john@example.com", password="securepassword")
```

#### c. Retrieve Specific Records
Filter data using `filter` or `get`:
```python
ModelName.objects.filter(field=value)  # Returns a queryset
ModelName.objects.get(field=value)    # Returns a single object
```

Example:
```python
User.objects.filter(username="john_doe")
User.objects.get(id=1)
```

#### d. Update Records
Modify existing data:
```python
record = ModelName.objects.get(field=value)
record.field = new_value
record.save()
```

Example:
```python
user = User.objects.get(username="john_doe")
user.email = "new_email@example.com"
user.save()
```

#### e. Delete Records
Remove records from the database:
```python
record = ModelName.objects.get(field=value)
record.delete()
```

Example:
```python
user = User.objects.get(username="john_doe")
user.delete()
```

---

### 4. **User-Specific Operations**

#### a. List All Users
```python
from django.contrib.auth.models import User
User.objects.all()
```

#### b. Create a New User
```python
User.objects.create_user(username="new_user", email="user@example.com", password="securepassword")
```

#### c. Change Password
```python
user = User.objects.get(username="new_user")
user.set_password("new_secure_password")
user.save()
```

#### d. Check if User Exists
```python
User.objects.filter(username="new_user").exists()
```

#### e. Retrieve User by Email
```python
User.objects.get(email="user@example.com")
```

---

### 5. **Advanced Queries**

#### a. Filter with Multiple Conditions
```python
User.objects.filter(username="new_user", email="user@example.com")
```

#### b. Order Records
```python
User.objects.all().order_by('username')  # Ascending
User.objects.all().order_by('-username')  # Descending
```

#### c. Count Records
```python
User.objects.count()
```

#### d. Exclude Specific Records
```python
User.objects.exclude(username="admin")
```

---

### 6. **Customizing Data with Querysets**
Django allows chaining queries for more complex operations:
```python
User.objects.filter(is_staff=True).exclude(username="admin").order_by('-date_joined')
```

---

### 7. **Exiting the Shell**
When done, type:
```bash
exit()
```

---

### Common Errors and Solutions

1. **Error: `ModelName` not defined**
   - Ensure the model is imported: `from your_app.models import ModelName`.

2. **Error: `DoesNotExist`**
   - The `get` method was called on a non-existent record. Use `filter` to avoid this error.

3. **Error: `IntegrityError`**
   - Ensure all required fields are provided when creating a record.

---
