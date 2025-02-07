# **Django REST Framework (DRF) Tutorial**  

## **What is Django REST Framework?**  
Django REST Framework (DRF) is a powerful toolkit for building Web APIs using Django. It simplifies the process of creating APIs by providing tools such as serializers, views, and authentication classes.  

With DRF, you can:  
1. Expose your Django models as RESTful APIs.  
2. Allow external systems (e.g., mobile apps, frontend apps) to interact with your database using JSON.  
3. Handle authentication, permissions, and request validation easily.  

---

## **1. Setting Up the Environment**  

### **Step 1: Install Python**  
Make sure Python is installed on your system. You can check by running:  
```bash
python --version
```

If Python is not installed, download it from [python.org](https://www.python.org/).

---

### **Step 2: Create a Project Directory**  
Navigate to the folder where you want your project to reside, and create a new directory for your project:  
```bash
mkdir drf_project
cd drf_project
```

---

### **Step 3: Set Up a Virtual Environment**  
A **virtual environment** isolates your project’s dependencies to avoid conflicts with other projects.  

1. Create a virtual environment:  
   ```bash
   python -m venv virtualenv
   ```

2. Activate the virtual environment:  
   - **Windows**:  
     ```bash
     virtualenv\Scripts\activate
     ```  
   - **Linux/macOS**:  
     ```bash
     source virtualenv/bin/activate
     ```

Once activated, the terminal prompt should show `(virtualenv)`.

3. Upgrade `pip` (Python's package manager):  
   ```bash
   pip install --upgrade pip
   ```

---

### **Step 4: Install Django and Django REST Framework**  
Install the necessary libraries for your project:  
```bash
pip install django djangorestframework
```

---

### **Step 5: Start a New Django Project**  
Use Django’s CLI tool to create a new project:  
```bash
django-admin startproject myproject .
```

- `myproject`: The name of your project.  
- The `.` creates the project files in the current folder instead of a subfolder.  

---

## **2. Setting Up Git for Version Control**  

### **Why Use Git?**  
Git helps track changes to your codebase, collaborate with others, and roll back to previous versions if needed.

### **Step 1: Initialize Git**  
Run the following command to initialize a Git repository in your project folder:  
```bash
git init
```

---

### **Step 2: Create a `.gitignore` File**  
The `.gitignore` file tells Git which files and folders to exclude from version control. Create this file in your project directory and add the following content:  
```plaintext
*.pyc
__pycache__/
virtualenv/
db.sqlite3
.env
```

- `*.pyc`: Excludes compiled Python files.  
- `__pycache__/`: Excludes cache files.  
- `virtualenv/`: Excludes the virtual environment folder.  
- `db.sqlite3`: Excludes the local SQLite database.  
- `.env`: Excludes environment variable files (if used).  

---

### **Step 3: Make Your Initial Commit**  
1. Stage all files:  
   ```bash
   git add .
   ```
2. Commit the changes:  
   ```bash
   git commit -m "Initial commit: Project setup"
   ```

---

## **3. Create a Django App**  

### **Step 1: Create a New App**  
Run the following command to create a new app:  
```bash
python manage.py startapp api
```

The `api` app will handle all API-related code.  

---

### **Step 2: Add the App to `INSTALLED_APPS`**  
Open `myproject/settings.py` and add `api` and `rest_framework` to the `INSTALLED_APPS` list:  
```python
INSTALLED_APPS = [
    ...
    'api',
    'rest_framework',
]
```

---

## **4. Creating Models**  

### **What Are Models?**  
Models in Django define the structure of your database tables. Each model maps to a table, and its fields correspond to table columns.  

### **Create a Model**  
In `api/models.py`, define a model for storing book information:  
```python
from django.db import models

class Book(models.Model):
    title = models.CharField(max_length=255)
    author = models.CharField(max_length=255)
    published_date = models.DateField()
    isbn = models.CharField(max_length=13, unique=True)

    def __str__(self):
        return self.title
```

---

### **Apply Migrations**  
1. Create migration files for the `Book` model:  
   ```bash
   python manage.py makemigrations
   ```
2. Apply the migrations to the database:  
   ```bash
   python manage.py migrate
   ```

---

## **5. Serializers**  

### **What Are Serializers?**  
Serializers convert complex Python objects (like models) into JSON format and vice versa.  

### **Create a Serializer**  
In `api/serializers.py`, create a serializer for the `Book` model:  
```python
from rest_framework import serializers
from .models import Book

class BookSerializer(serializers.ModelSerializer):
    class Meta:
        model = Book
        fields = '__all__'
```

- `ModelSerializer`: Automatically creates fields based on the model.  
- `fields = '__all__'`: Includes all fields in the JSON response.

---

## **6. Create Views**  

### **What Are Views?**  
Views handle the logic of your application. In DRF, views process API requests and return responses.

### **Define a View**  
In `api/views.py`, create an API view for listing and creating books:  
```python
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status
from .models import Book
from .serializers import BookSerializer

class BookList(APIView):
    def get(self, request):
        books = Book.objects.all()
        serializer = BookSerializer(books, many=True)
        return Response(serializer.data)

    def post(self, request):
        serializer = BookSerializer(data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
```

- **`get`**: Fetches all books.  
- **`post`**: Creates a new book.  

---

## **7. URL Configuration**  

### **Set Up URLs for the App**  
In `api/urls.py`, define routes for the API views:  
```python
from django.urls import path
from .views import BookList

urlpatterns = [
    path('books/', BookList.as_view(), name='book-list'),
]
```

---

### **Include App URLs in Project URLs**  
In `myproject/urls.py`, include the `api` app’s URLs:  
```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/', include('api.urls')),
]
```

---

## **8. Test the API**

### **Run the Development Server**  
Start the server:  
```bash
python manage.py runserver
```

---

### **Test the Endpoints**  
- **GET /api/books/**: Retrieve all books.  
- **POST /api/books/**: Add a new book (JSON payload).  
  Example:
  ```json
  {
      "title": "The Great Gatsby",
      "author": "F. Scott Fitzgerald",
      "published_date": "1925-04-10",
      "isbn": "9780743273565"
  }
  ```

---

## **9. Authentication and Pagination**  

### **Add Pagination**  
In `settings.py`, configure pagination:  
```python
REST_FRAMEWORK = {
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
    'PAGE_SIZE': 5,
}
```

---

### **Add Authentication**  
Install Simple JWT for token-based authentication:  
```bash
pip install djangorestframework-simplejwt
```

Update `settings.py` to include JWT authentication:  
```python
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    ),
}
```

---

## **10. Commit Your Work**  

### **Save Progress to Git**  
1. Add changes:  
   ```bash
   git add .
   ```
2. Commit the changes:  
   ```bash
   git commit -m "Added book API with basic CRUD functionality"
   ```
3. Push to a remote repository:  
   ```bash
   git remote add origin <repository_url>
   git push -u origin main
   ```

---
