### Comprehensive Django Tutorial: Building "Django Project 1" 

#### **Step 1: Environment Setup**

1. **Install Python**  
   Ensure Python (3.9 or later) is installed. Download it from [python.org](https://www.python.org/downloads/).

2. **Set Up the Project Directory**
   ```bash
   mkdir django-project-1
   cd django-project-1
   ```
   
3. **Create Virtualenv**
   ```bash
   python -m venv <virtual_env_name>
   ```
   - Another method (using virtualenv) : 
      ```bash
      pip install virtualenv
      ```
      - *Create a Virtual Environment*
      ```bash
      virtualenv <name_of_virtual_environment>
      ```

5. **Activate the Virtual Environment**
   - On Windows:
     ```bash
     <virtual_env_name>\Scripts\activate
     ```
   - On macOS/Linux:
     ```bash
     source <virtual_env_name>/bin/activate
     ```

6. **Install Django and other dependencies**
   ```bash
   pip install django
   ```
   For example, in case you are using the ImageField, we need to install Pillow
   ```bash
   pip install pillow
   ```

7. **Save Your Dependencies** - repeat this following step every time you install or upgrade any dependencies.
   ```bash
   pip freeze > requirements.txt
   ```

---

#### **Step 2: Creating the Django Project**

1. **Start the Django Project**
   ```bash
   django-admin startproject yshop
   ```

   Project structure:
   ```
   django-project-folder/
   ├──virtual-env-folder/
   ├──yshop/
      ├── manage.py
      ├── yshop/
          ├── __init__.py
          ├── settings.py
          ├── urls.py
          ├── asgi.py
          ├── wsgi.py
   ```

   1. a. **Migrate Schema changes to DB**
         ```bash
         cd yshop
         python manage.py migrate
         ``` 

2. **Run the Development Server**
   ```bash
   python manage.py runserver
   ```
   Visit [http://127.0.0.1:8000](http://127.0.0.1:8000) to confirm it works.

---

#### **Step 3: Setting Up Git and .gitignore**
*Ensure you are in the main folder outside django project folder where you can see the django project as well as the virtual environment folder*

1. **Initialize a Git Repository**
   ```bash
   git init
   ```

2. **Create a `.gitignore` File**
   Add:
   ```
   # Virtual environment
   # name of your virtual environment folder, eg: here, I'm creating a virtual environment named `virt`
   virt/ 

   # Bytecode files
   __pycache__/
   *.pyc
   *.pyo

   # Django-specific
   *.log
   db.sqlite3
   media/

   # VS Code
   .vscode/

   # Environment variables
   .env
   ```

3. **Commit Your Setup**
   ```bash
   git add .
   git commit -m "Initial project setup"
   ```

---

#### **Step 4: Building the Products App**

1. **Create an App**
   ```bash
   python manage.py startapp mainapp
   ```

2. **Register the App**
   Add `'mainapp'` to the `INSTALLED_APPS` list in `yshop/settings.py`:
   ```python
   INSTALLED_APPS = [
       'django.contrib.admin',
       'django.contrib.auth',
       'django.contrib.contenttypes',
       'django.contrib.sessions',
       'django.contrib.messages',
       'django.contrib.staticfiles',
       'mainapp',
   ]
   ```

---

#### **Step 5: Defining Models**

1. **Create a Model**
   Open `mainapp/models.py`:
   ```python
   from django.db import models

   class Product(models.Model):
       name = models.CharField(max_length=200)
       price = models.DecimalField(max_digits=10, decimal_places=2)
       description = models.TextField()

       def __str__(self):
           return self.name
   ```

2. **Run Migrations**
   ```bash
   python manage.py makemigrations mainapp
   python manage.py migrate
   ```

---

#### **Step 6: Admin and Superuser Setup**

1. **Register Models with the Admin**
   Open `mainapp/admin.py`:
   ```python
   from django.contrib import admin
   from .models import Product

   @admin.register(Product)
   class ProductAdmin(admin.ModelAdmin):
       list_display = ('name', 'price', 'description')
   ```

2. **Create a Superuser**
   ```bash
   python manage.py createsuperuser
   ```
   Enter details like username, email, and password when prompted.

3. **Access the Admin Panel**
   Run the server:
   ```bash
   python manage.py runserver
   ```
   Visit [http://127.0.0.1:8000/admin/](http://127.0.0.1:8000/admin/) and log in with your superuser credentials.

4. **Add Sample Products**
   Use the admin interface to add a few sample products.

---

#### **Step 7: Building Views and URLs**

1. **Create a View**
   Open `mainapp/views.py`:
   ```python
   from django.shortcuts import render
   from .models import Product

   def product_list(request):
       products = Product.objects.all()
       return render(request, 'product_list.html', {'products': products})
   ```

2. **Set Up URLs**
   Create `mainapp/urls.py`:
   ```python
   from django.urls import path
   from . import views

   urlpatterns = [
       path('', views.product_list, name='product_list'),
   ]
   ```

   Include app URLs in `yshop/urls.py`:
   ```python
   from django.urls import path, include

   urlpatterns = [
       path('admin/', admin.site.urls),
       path('', include('mainapp.urls')),
   ]
   ```

---

#### **Step 8: Creating Templates**

1. **Set Up Template Directory**
   Create `mainapp/templates/`.

2. **Create a Template**
   Save this as `product_list.html`:
   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>Product List</title>
   </head>
   <body>
       <h1>Products</h1>
       <ul>
           {% for product in products %}
           <li>{{ product.name }}: ${{ product.price }}</li>
           {% endfor %}
       </ul>
   </body>
   </html>
   ```

---

#### **Step 9: Testing Your Application**

1. **Run the Server**
   ```bash
   python manage.py runserver
   ```

2. **Visit the Product List Page**
   Navigate to [http://127.0.0.1:8000/products/](http://127.0.0.1:8000/) to see your products.

---

#### **Step 10: Pushing to GitHub**

1. **Create a Repository**
   Create a new repository on GitHub.

2. **Push Your Code**
   ```bash
   git remote add origin https://github.com/<your-username>/django-project-1.git
   git branch -M main
   git push -u origin main
   ```

---
