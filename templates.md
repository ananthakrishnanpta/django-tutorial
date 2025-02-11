# Django Templates Tutorial
---

## 1. The "T" in Django's MVT Architecture

In Django's **MVT (Model-View-Template)** design pattern:
- **Model**: Handles data and defines the database schema.
- **View**: Acts as the intermediary between the model and the template. It fetches data from models, processes it, and passes it to templates.
- **Template**: Handles the **presentation layer**, displaying the data received from views in a structured format.

Templates allow dynamic rendering of HTML pages using placeholders, tags, and filters.

---

## 2. What is a Django Template?

A Django template is an HTML file enriched with Django’s **template language**. It allows you to include:
- **Dynamic content**: Inject data passed from views.
- **Template tags**: Logic like loops, conditions, and template inclusion.
- **Filters**: Formatting for displayed data.

### Example: A Basic Django Template

```html
<!DOCTYPE html>
<html>
<head>
    <title>{{ title }}</title>
</head>
<body>
    <h1>Hello, {{ user.name }}!</h1>
    <p>{{ description }}</p>
</body>
</html>
```

**Explanation**:
- `{{ title }}`: Placeholder for a variable provided by the view.
- `{{ user.name }}`: Accessing a nested attribute of a variable.
- `{{ description }}`: Injecting a variable’s value.

---

## 3. Template Workflow in Django

Templates are linked to the Django application through views. Here's the workflow:

1. **View** retrieves or processes data.
2. **View** passes the data to the **template** using a **context dictionary**.
3. **Template** renders the dynamic content and returns the final HTML.

### Example: Rendering a Template in a View

```python
from django.shortcuts import render

def home_view(request):
    context = {
        'title': 'Home Page',
        'user': {'name': 'Alice'},
        'description': 'Welcome to our Django tutorial!'
    }
    return render(request, 'home.html', context)
```

- The `context` dictionary contains the data passed to the `home.html` template.

---

## 4. Setting Up Templates in Django

### 4.1 Configuring Templates Directory

In your `settings.py`, configure the `TEMPLATES` setting:

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'templates'],  # Directory for custom templates
        'APP_DIRS': True,                 # Automatically search app-level templates
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                # Additional context processors
            ],
        },
    },
]
```

- `DIRS`: Specifies the path where Django will look for templates.
- `APP_DIRS`: Enables Django to look for `templates` directories inside each app.

### 4.2 Template Directory Structure

A typical structure:

```
project/
    templates/
        base.html
        home.html
    app_name/
        templates/
            app_name/
                app_specific_template.html
```

---

## 5. Template Tags

Template tags allow you to add logic in templates. They are enclosed within `{% %}` and support operations like loops, conditions, and template inheritance.

### 5.1 Common Template Tags

#### `for` Tag

Used to iterate over a sequence.

```html
<ul>
{% for item in items %}
    <li>{{ item }}</li>
{% endfor %}
</ul>
```

---

#### `if` Tag

Used for conditional statements.

```html
{% if user.is_authenticated %}
    <p>Welcome back, {{ user.name }}!</p>
{% else %}
    <p>Hello, Guest! Please log in.</p>
{% endif %}
```

---

#### `block` and `endblock`

Used in template inheritance to define placeholders for dynamic content.

```html
{% block content %}
    <p>This is the content block.</p>
{% endblock %}
```

---

#### `extends`

Used to inherit a base template. This helps maintain consistency across pages.

```html
{% extends "base.html" %}
```

---

#### `include`

Includes another template within the current template. Useful for reusable components like headers or footers.

```html
{% include "header.html" %}
```

---

## 6. Template Filters

Filters modify the display of variables. They are applied using the `|` (pipe) operator.

### Example: Common Filters

| Filter         | Syntax                              | Description                                |
|----------------|-------------------------------------|--------------------------------------------|
| `date`         | `{{ date|date:"Y-m-d" }}`          | Formats a date.                            |
| `default`      | `{{ value|default:"Default Text" }}`| Provides a default value if the variable is empty. |
| `length`       | `{{ items|length }}`               | Returns the length of a list or string.    |
| `lower`        | `{{ text|lower }}`                 | Converts a string to lowercase.            |
| `upper`        | `{{ text|upper }}`                 | Converts a string to uppercase.            |
| `yesno`        | `{{ value|yesno:"Yes,No,Maybe" }}` | Maps boolean values to strings.            |

---

## 7. Template Inheritance

### 7.1 Base Template

A **base template** contains common HTML structure, which other templates can inherit and extend.

```html
<!DOCTYPE html>
<html>
<head>
    <title>{% block title %}My Django Site{% endblock %}</title>
</head>
<body>
    <header>
        <h1>Welcome to My Site</h1>
    </header>
    <main>
        {% block content %}
        {% endblock %}
    </main>
    <footer>
        <p>© 2025 My Site</p>
    </footer>
</body>
</html>
```

---

### 7.2 Extending the Base Template

Child templates extend the base template and override specific blocks.

```html
{% extends "base.html" %}

{% block title %}Home Page{% endblock %}

{% block content %}
    <h2>Welcome to the Home Page</h2>
    <p>This is dynamically generated content.</p>
{% endblock %}
```

---

## 8. Including Templates

The `{% include %}` tag allows for the inclusion of reusable template snippets.

### Example: Header and Footer Templates

**header.html**:

```html
<header>
    <h1>Welcome to My Site</h1>
</header>
```

**footer.html**:

```html
<footer>
    <p>© 2025 My Site</p>
</footer>
```

**home.html**:

```html
{% include "header.html" %}
<h2>Welcome to the Home Page</h2>
{% include "footer.html" %}
```

---

## 9. Static Files in Templates

Static files include CSS, JavaScript, and images. Django provides the `{% static %}` tag for managing these files.

### Example: Adding Static Files

```html
<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" href="{% static 'css/style.css' %}">
</head>
<body>
    <img src="{% static 'images/logo.png' %}" alt="Logo">
</body>
</html>
```

---

## 10. Debugging Tips for Templates

1. **Check Template Directories**: Ensure your `TEMPLATES` setting is correctly configured.
2. **Handle Missing Variables**: Use `{{ variable|default:"Fallback value" }}` to handle missing data.
3. **Use `{% debug %}`**: To inspect the context variables passed to a template.

---

## 11. Complete Example

**base.html**:

```html
<!DOCTYPE html>
<html>
<head>
    <title>{% block title %}My Site{% endblock %}</title>
</head>
<body>
    {% include "header.html" %}
    <main>
        {% block content %}
        {% endblock %}
    </main>
    {% include "footer.html" %}
</body>
</html>
```

**home.html**:

```html
{% extends "base.html" %}

{% block title %}Home{% endblock %}

{% block content %}
    <h2>Welcome to the Home Page</h2>
    <p>This page is powered by Django Templates.</p>
    <ul>
        {% for item in items %}
            <li>{{ item|upper }}</li>
        {% endfor %}
    </ul>
{% endblock %}
```

---

## 12. Summary Table of Features

| Feature          | Syntax                           | Description                              |
|-------------------|----------------------------------|------------------------------------------|
| `for` Tag         | `{% for item in items %}`       | Loops through a sequence.               |
| `if` Tag          | `{% if condition %}`            | Adds conditional logic.                 |
| `block`/`extends` | `{% block name %}`, `{% extends "base.html" %}` | Enables template inheritance. |
| `include` Tag     | `{% include "file.html" %}`     | Includes another template.              |
| Filters           | `{{ var|filter }}`             | Modifies variable display.              |

---

## 13. Additional Resources

- [Django Documentation: Templates](https://docs.djangoproject.com/en/stable/ref/templates/)
- [Django Template Language](https://docs.djangoproject.com/en/stable/topics/templates/)

---
