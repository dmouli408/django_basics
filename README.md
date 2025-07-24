# 📓 Django Full Notes: Basic Structure, Features, Settings

---

## 🔍 Overview of Django

Django is a high-level Python Web framework that encourages rapid development and clean, pragmatic design.

* **Language:** Python
* **Architecture:** MVT (Model - View - Template)
* **Main Features:**

  * Built-in admin interface
  * ORM (Object Relational Mapping)
  * URL routing
  * Authentication system
  * Template engine
  * Security features
  * Scalability and reusability

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
| `static/`                  | App         | Static files: CSS, JS, images.                                                     |

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

## 📄 settings.py Key Configurations

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'basic',  # Custom app
]

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

ROOT_URLCONF = 'myproject.urls'

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],  # For custom template paths
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

WSGI_APPLICATION = 'myproject.wsgi.application'

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}

STATIC_URL = '/static/'
```

## ⚙️ Important `settings.py` Configurations

* `DEBUG = True` – Development only
* `ALLOWED_HOSTS = ['*']` – Add hostnames/IPs in production
* `INSTALLED_APPS` – Add your app names here
* `MIDDLEWARE` – Request/response middleware chain
* `TEMPLATES` – HTML template configuration
* `DATABASES` – Default is SQLite, can use PostgreSQL, etc.
* `STATIC_URL`, `MEDIA_URL` – For static & media files


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

## ✨ Django Features

* MTV (Model-Template-View) pattern
* ORM (Object Relational Mapping)
* Built-in Admin Interface
* Middleware Support
* Security features (XSS, CSRF, SQL injection protection)
* Scalable and reusable components

---




