# **Django Beginner’s Tutorial: From Installation to Your First Project**

---

## **1. Installation: Python, Virtualenv, and Django**

### **Step 1: Installing Python**
Django requires Python, so ensure you have it installed on your machine.

#### **On Windows:**
1. Download the latest version of Python from the [official Python website](https://www.python.org/downloads/).
2. During installation:
   - Check **Add Python to PATH**.
   - Install using default settings.

#### **On macOS/Linux:**
1. Open the terminal.
2. Use the following command to install Python:
   - macOS (with Homebrew):  
     ```bash
     brew install python
     ```
   - Linux (Debian/Ubuntu):  
     ```bash
     sudo apt update
     sudo apt install python3 python3-pip
     ```

#### **Verify Installation:**
Run this command:
```bash
python --version
```
or
```bash
python3 --version
```

---

### **Step 2: Installing Virtualenv**
Virtual environments are used to manage dependencies for different projects.

#### Install Virtualenv:
```bash
pip install virtualenv
```

#### Create a Virtual Environment:
```bash
virtualenv venv
```
If Python 3 is required:
```bash
virtualenv -p python3 venv
```

#### Activate the Virtual Environment:
- **Windows**:
  ```bash
  venv\Scripts\activate
  ```
- **macOS/Linux**:
  ```bash
  source venv/bin/activate
  ```

You’ll see `(venv)` in your terminal prompt, indicating the environment is active.

---

### **Step 3: Installing Django**
With the virtual environment activated, install Django:
```bash
pip install django
```

#### Verify Django Installation:
```bash
django-admin --version
```

---

## **2. Django MVT Architecture**

Django follows the **Model-View-Template (MVT)** architecture. Let’s break it down:

### **Model**
- Represents the data layer.
- Handles database interactions, such as creating, retrieving, updating, and deleting records.

### **View**
- Contains business logic.
- Fetches data from the model and passes it to the template.

### **Template**
- Manages the presentation layer.
- Generates dynamic HTML based on the data passed by the view.

---

### **How MVT Works**
1. A user requests a URL.
2. Django’s URL dispatcher maps the URL to a corresponding view.
3. The view fetches data from the model and sends it to the template.
4. The template renders the data into an HTML response.

---

## **3. Create Your First Django Project**

Let’s build a basic shopping site with product listings.

---

### **Step 1: Start a New Project**
1. Run this command to create a new project:
   ```bash
   django-admin startproject shopping_site
   ```

2. Navigate to the project directory:
   ```bash
   cd shopping_site
   ```

---

### **Step 2: Start a New App**
Django projects are organized into apps, which handle specific functionalities. For this example, we’ll create a `products` app for managing products.

1. Run this command:
   ```bash
   python manage.py startapp products
   ```

2. Add the app to your project settings:
   Open `shopping_site/settings.py` and add `'products'` to `INSTALLED_APPS`:
   ```python
   INSTALLED_APPS = [
       'django.contrib.admin',
       'django.contrib.auth',
       'django.contrib.contenttypes',
       'django.contrib.sessions',
       'django.contrib.messages',
       'django.contrib.staticfiles',
       'products',  # Our app
   ]
   ```

---

### **Step 3: Setting Up the Model**
Define the `Product` model in `products/models.py`:
```python
from django.db import models

class Product(models.Model):
    name = models.CharField(max_length=100)
    description = models.TextField()
    price = models.DecimalField(max_digits=10, decimal_places=2)
    stock = models.IntegerField()

    def __str__(self):
        return self.name
```

#### Run Migrations:
1. Generate migration files:
   ```bash
   python manage.py makemigrations
   ```

2. Apply migrations:
   ```bash
   python manage.py migrate
   ```

---

### **Step 4: Adding Admin Support**
Register the `Product` model in `products/admin.py`:
```python
from django.contrib import admin
from .models import Product

admin.site.register(Product)
```

Run the development server:
```bash
python manage.py runserver
```
Visit the admin site at `http://127.0.0.1:8000/admin`, log in, and add some products.

---

### **Step 5: Create a View**
Define a view to list all products in `products/views.py`:
```python
from django.shortcuts import render
from .models import Product

def product_list(request):
    products = Product.objects.all()
    return render(request, 'products/product_list.html', {'products': products})
```

---

### **Step 6: Set Up URLs**
1. Create a `urls.py` file in the `products` app:
   ```python
   from django.urls import path
   from . import views

   urlpatterns = [
       path('', views.product_list, name='product_list'),
   ]
   ```

2. Include the app URLs in the project’s `urls.py`:
   ```python
   from django.contrib import admin
   from django.urls import path, include

   urlpatterns = [
       path('admin/', admin.site.urls),
       path('products/', include('products.urls')),
   ]
   ```

---

### **Step 7: Create a Template**
Create the `products/templates/products/product_list.html` file:
```html
<!DOCTYPE html>
<html>
<head>
    <title>Product List</title>
</head>
<body>
    <h1>Available Products</h1>
    <ul>
        {% for product in products %}
            <li>{{ product.name }} - ${{ product.price }}</li>
        {% endfor %}
    </ul>
</body>
</html>
```

---

### **Step 8: Adding Static Files**
1. Create a `static` folder in the `products` app:
   ```
   products/static/products/
   ```
2. Add a CSS file:
   ```css
   /* products/static/products/style.css */
   body {
       font-family: Arial, sans-serif;
   }
   ```
3. Include the static file in the template:
   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <link rel="stylesheet" type="text/css" href="{% static 'products/style.css' %}">
       <title>Product List</title>
   </head>
   <body>
       <h1>Available Products</h1>
       <ul>
           {% for product in products %}
               <li>{{ product.name }} - ${{ product.price }}</li>
           {% endfor %}
       </ul>
   </body>
   </html>
   ```

Ensure `django.contrib.staticfiles` is in `INSTALLED_APPS` and set the `STATIC_URL` in `settings.py`:
```python
STATIC_URL = '/static/'
```

---

### **Step 9: Run the Server**
Start the development server:
```bash
python manage.py runserver
```

Visit `http://127.0.0.1:8000/products/` to see your product list.

---
