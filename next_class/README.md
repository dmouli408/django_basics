# 📓 Django Full Notes: Basic Structure, Features, Settings

---

## 🔍 Overview of Django

Django is a high-level Python Web framework that encourages rapid development and clean, pragmatic design.

* **Language:** Python
* **Architecture:** MVT (Model - View - Template)
* **Main Features:**

  * Built-in admin interface
  * MTV (Model-Template-View) pattern
  * ORM (Object Relational Mapping)
  * Middleware Support
  * URL routing
  * Authentication system
  * Template engine
  * Security features (XSS, CSRF, SQL injection protection)
  * Scalability and reusability

---


## ✨ Django Key Features Summary

* **Admin Interface**: Auto-generated dashboard for models
* **ORM**: Pythonic way to interact with databases
* **Templates**: Clean separation of HTML and backend
* **Security**: CSRF, SQL injection protection, password hashing
* **Scalability**: Modular apps, reusable code
* **Routing**: Clear, Python-based URL mapping
* **Authentication**: Login/logout, user management
* **Sessions & Messages**: Easy flash messages and session handling

---


## 🗂️ Django Project & App Structure: Full Reference

### 📋 Quick Reference Table

| File/Folder                | Project/App | Definition & Usage                                                                 |
|----------------------------|-------------|------------------------------------------------------------------------------------|
| `manage.py`                | Project     | Command-line utility for managing the project (runserver, migrations, shell, etc.) |
| `settings.py`              | Project     | Main configuration: database, apps, middleware, static files, etc.                 |
| `urls.py`                  | Project/App | URL routing: maps URLs to views. Project-level is required, app-level is optional.  |
| `wsgi.py`                  | Project     | WSGI entry point for deployment (production servers).                              |
| `asgi.py`                  | Project     | ASGI entry point for async servers (channels, websockets).                         |
| `__init__.py`              | Both        | Marks directory as a Python package/module.                                        |
| `apps.py`                  | App         | App configuration class (metadata, signals, etc.).                                 |
| `admin.py`                 | App         | Register models for Django admin interface.                                        |
| `models.py`                | App         | Define database models (ORM classes).                                              |
| `views.py`                 | App         | Business logic: request handling, responses.                                       |
| `tests.py`                 | App         | Unit tests for app logic/models/views.                                             |
| `migrations/`              | App         | Auto-generated DB schema changes (one per model change).                           |
| `templates/`               | App         | HTML templates for rendering views.                                                |

---


### 🏗️ Django Project Structure (after `django-admin startproject myproject`)

```text
myproject/
├── manage.py           # Project management script
├── myproject/          # Main project package
│   ├── __init__.py     # Python package marker
│   ├── settings.py     # Project settings/configuration
│   ├── urls.py         # Project-level URL routing
│   ├── asgi.py         # ASGI entry point (async support)
│   └── wsgi.py         # WSGI entry point (deployment)
```

#### **File/Folder Details**

- **manage.py**
  - *Definition*: Command-line tool to interact with your project.
  - *Usage*: Run server, migrations, shell, custom commands.
  - *Example*: `python manage.py runserver`

- **myproject/**
  - *Definition*: Main project package (contains settings and config).
  - **__init__.py**
    - *Definition*: Marks this directory as a Python package.
    - *Usage*: Usually empty, but can include startup code.

  - **settings.py**
    - *Definition*: Central configuration (database, apps, middleware, etc.).
    - *Usage*: Edit to add apps, change DB, set static/media paths, etc.

  - **urls.py**
    - *Definition*: Root URL configuration for the project.
    - *Usage*: Includes app URLs, admin URLs, etc.

  - **asgi.py**
    - *Definition*: ASGI application entry point (for async servers).
    - *Usage*: Needed for channels, websockets, async features.

  - **wsgi.py**
    - *Definition*: WSGI application entry point (for deployment).
    - *Usage*: Used by most production servers (Gunicorn, uWSGI, etc.).

---

### � Django App Structure (after `python manage.py startapp appname`)

```text
appname/
├── __init__.py          # Python package marker
├── admin.py             # Register models for admin
├── apps.py              # App config class
├── models.py            # Database models (ORM)
├── tests.py             # Unit tests
├── views.py             # View functions/classes
├── urls.py              # App-level URL routing (create manually)
├── migrations/          # DB schema migrations
│   └── __init__.py      # Python package marker for migrations
├── templates/appname/   # HTML templates (Django looks here by default)
│   └── ...
├── static/appname/      # Static files (CSS, JS, images)
│   └── ...
```

#### **File/Folder Details**

- **__init__.py**
  - *Definition*: Marks the app directory as a Python package.
  - *Usage*: Usually empty, but can import signals or startup code.

- **admin.py**
  - *Definition*: Register models to appear in Django admin.
  - *Usage*: Add `admin.site.register(YourModel)` to manage models via admin UI.

- **apps.py**
  - *Definition*: App configuration class (subclass of `AppConfig`).
  - *Usage*: Set app name, ready hooks, signals, etc.

- **models.py**
  - *Definition*: Define database models (ORM classes).
  - *Usage*: Each class = table; fields = columns. Run `makemigrations` and `migrate` after changes.

- **tests.py**
  - *Definition*: Unit tests for your app.
  - *Usage*: Write test cases for models, views, forms, etc. Run with `python manage.py test`.

- **views.py**
  - *Definition*: Request handlers (functions or classes).
  - *Usage*: Return HTTP responses, render templates, handle logic.

- **urls.py** (create manually)
  - *Definition*: App-level URL routing.
  - *Usage*: Map URLs to views. Include in project `urls.py` with `include()`.

- **migrations/**
  - *Definition*: Auto-generated DB schema changes (one per model change).
  - *Usage*: Never edit manually. Run `makemigrations` and `migrate`.
  - **__init__.py**: Marks as a Python package.

- **templates/appname/**
  - *Definition*: HTML templates for this app.
  - *Usage*: Place templates here for Django to find them automatically.

- **static/appname/**
  - *Definition*: Static files (CSS, JS, images) for this app.
  - *Usage*: Place static assets here. Collect with `collectstatic` for deployment.

---

## 🛡️ Essential `settings.py` Options Explained

```python
# Enable debug mode (shows detailed errors in development)
DEBUG = True  # Set to False in production!

# Hosts/domain names your site can serve (use ['*'] for dev, set real domains in production)
ALLOWED_HOSTS = ['your_IP_address', 'your_web_site_name']

# List of installed apps (add your app names here)
INSTALLED_APPS = [
    # ...default Django apps...
    'your_app',  # Add your custom app(s)
]

# Middleware chain (handles requests/responses)
MIDDLEWARE = [
    # ...default middleware...
]

# Template engine configuration
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],  # Add template directories if needed
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                # ...default context processors...
            ],
        },
    },
]

# Database settings (default is SQLite, can use PostgreSQL, MySQL, etc.)
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}

# Static files (CSS, JS, images)
STATIC_URL = '/static/'

# Media files (user uploads)
MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / 'media'
```

**Key Explanations:**

- `DEBUG`: Shows detailed error pages and auto-reloads code. **Never use `True` in production!**
- `ALLOWED_HOSTS`: List of valid hostnames for your site. Use `['*']` for local dev, set real domains for deployment.
- `INSTALLED_APPS`: Register all Django and custom apps here.
- `MIDDLEWARE`: Controls request/response processing (security, sessions, etc.).
- `TEMPLATES`: Configures template engine and context processors.
- `DATABASES`: Database connection settings. Default is SQLite; can be changed to PostgreSQL, MySQL, etc.
- `STATIC_URL`/`MEDIA_URL`: URLs for serving static and media files.
- `MEDIA_ROOT`: Filesystem path for storing uploaded media.
| `static/`                  | App         | Static files: CSS, JS, images.                                                     |

---


## 🧠 Django Project Creation, Structure & Homepage Rendering – Notes

---

### 🐍 Step-by-Step: Create Django Application with Homepage

#### 1️⃣ Create Virtual Environment

```bash
python -m venv environ
```

Activate it:

* Windows: `environ\Scripts\activate`
* Linux/macOS: `source environ/bin/activate`

#### 2️⃣ Install Django

```bash
pip install django
```

#### 3️⃣ Create Django Project

```bash
django-admin startproject sample
cd sample
```

#### 4️⃣ Create Django App

```bash
python manage.py startapp basic
```

#### 5️⃣ Register App in `settings.py`

In `INSTALLED_APPS`, add:

```python
'basic',
```

#### 6️⃣ Create View in `basic/views.py`

```python
from django.shortcuts import render

def home(request):
    return render(request,'basic/home.html')
```

#### 7️⃣ Configure URLs

📄 In `basic/urls.py` (create this file):

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home, name='home'),
]
```

📄 In `sample/urls.py`:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('basic/', include('basic.urls')),
]
```

#### 8️⃣ Run Server

```bash
python manage.py runserver
```

Visit: `http://127.0.0.1:8000/basic/`

---

#### 8️⃣ Run Server for whole local network

```bash
python manage.py runserver 0.0.0.0:8000
```
check in command promt 

```
ipconfig
```

Visit: `http://(your IP Address)/basic/`






# 🏫 Django Authentication: Built-in User (with form.as_p) & Custom User (manual fields, no form.as_p)

---

## 1️⃣ Built-in Django User: Registration & Login (Bootstrap, with form.as_p)

### Registration View
```python
from django.contrib.auth.forms import UserCreationForm
from django.shortcuts import render, redirect

def register(request):
    if request.method == 'POST':
        form = UserCreationForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('login')
    else:
        form = UserCreationForm()
    return render(request, 'register.html', {'form': form})
```

### Login View (Django built-in)
```python
from django.contrib.auth import views as auth_views

# In urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('register/', views.register, name='register'),
    path('login/', auth_views.LoginView.as_view(template_name='login.html'), name='login'),
]
```

### Bootstrap Registration Template (`register.html`) — uses `form.as_p`
```html
<div class="container mt-5">
  <div class="row justify-content-center">
    <div class="col-md-6">
      <div class="card">
        <div class="card-header bg-primary text-white">Register</div>
        <div class="card-body">
          <form method="post">
            {% csrf_token %}
            {{ form.as_p }}
            <button type="submit" class="btn btn-success w-100">Sign Up</button>
          </form>
        </div>
      </div>
    </div>
  </div>
</div>
```

### Bootstrap Login Template (`login.html`) — uses `form.as_p`
```html
<div class="container mt-5">
  <div class="row justify-content-center">
    <div class="col-md-6">
      <div class="card">
        <div class="card-header bg-primary text-white">Login</div>
        <div class="card-body">
          <form method="post">
            {% csrf_token %}
            {{ form.as_p }}
            <button type="submit" class="btn btn-primary w-100">Login</button>
          </form>
        </div>
      </div>
    </div>
  </div>
</div>
```

---

## 2️⃣ Custom User Model (No forms.py): Registration & Login (Bootstrap, **manual fields, no form.as_p**)

### models.py
```python
from django.contrib.auth.models import AbstractUser
from django.db import models

DEPARTMENT_CHOICES = [
    ('HR', 'Human Resources'),
    ('IT', 'Information Technology'),
    ('FIN', 'Finance'),
    ('MKT', 'Marketing'),
]

DESIGNATION_CHOICES = [
    ('JR', 'Junior'),
    ('SR', 'Senior'),
    ('MG', 'Manager'),
    ('DIR', 'Director'),
]

class CustomUser(AbstractUser):
    name = models.CharField(max_length=100)
    phone_number = models.CharField(max_length=15)
    email = models.EmailField(unique=True)
    department = models.CharField(max_length=3, choices=DEPARTMENT_CHOICES)
    designation = models.CharField(max_length=4, choices=DESIGNATION_CHOICES)

    USERNAME_FIELD = 'email'
    REQUIRED_FIELDS = ['username', 'name', 'phone_number', 'department', 'designation']
```

### settings.py
```python
AUTH_USER_MODEL = 'myapp.CustomUser'
```

### Registration View (no forms.py, inline form class)
```python
from django.shortcuts import render, redirect
from django.contrib.auth.forms import UserCreationForm
from .models import CustomUser

class CustomUserCreationForm(UserCreationForm):
    class Meta(UserCreationForm.Meta):
        model = CustomUser
        fields = ['name', 'phone_number', 'email', 'department', 'designation', 'password1', 'password2']

def register(request):
    if request.method == 'POST':
        form = CustomUserCreationForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('login')
    else:
        form = CustomUserCreationForm()
    return render(request, 'register.html', {'form': form})
```

### urls.py
```python
from django.urls import path
from . import views
from django.contrib.auth import views as auth_views

urlpatterns = [
    path('register/', views.register, name='register'),
    path('login/', auth_views.LoginView.as_view(template_name='login.html'), name='login'),
]
```

### Bootstrap Registration Template (`register.html`) — **manual fields, no form.as_p**
```html
<div class="container mt-5">
  <div class="row justify-content-center">
    <div class="col-md-6">
      <div class="card">
        <div class="card-header bg-primary text-white">Register</div>
        <div class="card-body">
          <form method="post">
            {% csrf_token %}
            <div class="mb-3">
              <label for="id_name" class="form-label">Name</label>
              {{ form.name }}
            </div>
            <div class="mb-3">
              <label for="id_phone_number" class="form-label">Phone Number</label>
              {{ form.phone_number }}
            </div>
            <div class="mb-3">
              <label for="id_email" class="form-label">Email</label>
              {{ form.email }}
            </div>
            <div class="mb-3">
              <label for="id_department" class="form-label">Department</label>
              {{ form.department }}
            </div>
            <div class="mb-3">
              <label for="id_designation" class="form-label">Designation</label>
              {{ form.designation }}
            </div>
            <div class="mb-3">
              <label for="id_password1" class="form-label">Password</label>
              {{ form.password1 }}
            </div>
            <div class="mb-3">
              <label for="id_password2" class="form-label">Confirm Password</label>
              {{ form.password2 }}
            </div>
            <button type="submit" class="btn btn-success w-100">Sign Up</button>
          </form>
        </div>
      </div>
    </div>
  </div>
</div>
```

### Bootstrap Login Template (`login.html`) — **manual fields, no form.as_p**
```html
<div class="container mt-5">
  <div class="row justify-content-center">
    <div class="col-md-6">
      <div class="card">
        <div class="card-header bg-primary text-white">Login</div>
        <div class="card-body">
          <form method="post">
            {% csrf_token %}
            <div class="mb-3">
              <label for="id_username" class="form-label">Email</label>
              {{ form.username }}
            </div>
            <div class="mb-3">
              <label for="id_password" class="form-label">Password</label>
              {{ form.password }}
            </div>
            <button type="submit" class="btn btn-primary w-100">Login</button>
          </form>
        </div>
      </div>
    </div>
  </div>
</div>
```

---

