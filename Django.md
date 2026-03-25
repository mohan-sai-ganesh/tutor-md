# The Complete Django Tutorial
### Every Concept Explained Clearly — From Zero to Production

---

## Table of Contents

1. What is Django and Why Use It?
2. How the Web Works (Prerequisites)
3. Setting Up Your Environment
4. Creating Your First Project
5. Understanding the Project Structure
6. How Django Handles a Request (The Request-Response Cycle)
7. URLs and Routing
8. Views — The Brain of Your Application
9. Templates — The Face of Your Application
10. Models — The Heart of Your Data
11. Migrations — Version Control for Your Database
12. The Django ORM — Talking to Your Database in Python
13. The Django Admin Panel
14. Forms — Collecting User Input
15. Authentication and Authorization
16. Static Files and Media Files
17. Class-Based Views (CBVs)
18. Relationships Between Models
19. Middleware — The Gatekeepers
20. Signals — Event Listeners
21. Sessions and Cookies
22. Messages Framework
23. Pagination
24. Email Sending
25. File Uploads
26. Custom Template Tags and Filters
27. Management Commands
28. Caching — Making Things Faster
29. Testing — Making Sure Things Work
30. REST APIs with Django REST Framework
31. Security Best Practices
32. Deployment — Going Live
33. Quick Reference and Cheat Sheet

---

## 1. What is Django and Why Use It?

### What is a Web Framework?

Imagine you want to build a house. You could start from scratch — making your own bricks, mixing your own cement, cutting your own wood. Or, you could use pre-made materials and follow an architectural blueprint. A web framework is like that blueprint and those pre-made materials. It gives you ready-made components so you don't have to build everything from zero.

Django is a **web framework written in Python**. It helps you build websites and web applications quickly and cleanly.

### Why Django?

**"Batteries Included" Philosophy:** Django comes with almost everything you need built in. Unlike some frameworks where you have to install dozens of extra packages for basic features, Django includes an admin panel, authentication system, database tools, form handling, security features, and much more — all out of the box.

**The DRY Principle (Don't Repeat Yourself):** Django is designed so that you write each piece of logic only once. If you need the same functionality in multiple places, Django provides ways to reuse code instead of copying and pasting.

**Security:** Django automatically protects you against many common web attacks like SQL injection, cross-site scripting (XSS), cross-site request forgery (CSRF), and clickjacking. You get these protections without doing anything special.

**Scalability:** Some of the world's biggest websites use Django — Instagram, Pinterest, Disqus, Mozilla, and The Washington Post. It can handle millions of users.

**The MVT Architecture:** Django follows the Model-View-Template pattern (similar to Model-View-Controller in other frameworks). This separates your code into three concerns:

- **Model** = Your data (what information you store)
- **View** = Your logic (what happens when someone visits a page)
- **Template** = Your presentation (what the user sees — the HTML)

This separation makes your code organized, maintainable, and easier to debug.

---

## 2. How the Web Works (Prerequisites)

Before diving into Django, it helps to understand the basics of how the web works, because Django is essentially a tool for handling this process.

### The Request-Response Cycle

When you type a URL into your browser and press Enter, here's what happens:

1. **Your browser sends a "request"** to a server somewhere on the internet. This request says: "I want to see the page at this address."

2. **The server receives the request** and figures out what you're asking for. It might need to look up data in a database, process some logic, or just grab a file.

3. **The server sends back a "response"** — usually an HTML page, but it could also be JSON data, an image, or a file.

4. **Your browser receives the response** and displays it.

Django sits on the server side. Its job is to receive requests, figure out what to do with them, and send back responses. Every feature in Django — URLs, views, models, templates — is a piece of this puzzle.

### HTTP Methods

When your browser sends a request, it also includes a "method" that tells the server what kind of action you want:

- **GET** = "Give me some data." Used when you visit a page, click a link, or search for something. It should never change data on the server.
- **POST** = "Here's some data to process." Used when you submit a form — like creating an account, posting a comment, or uploading a file. It usually changes data on the server.
- **PUT/PATCH** = "Update this existing data."
- **DELETE** = "Remove this data."

Django lets you handle each of these methods differently in your views.

---

## 3. Setting Up Your Environment

### 3.1 Installing Python

Django is written in Python, so you need Python installed. Django requires Python 3.8 or later.

```bash
# Check if Python is installed and which version
python --version
# or
python3 --version
```

If you don't have Python, download it from python.org.

### 3.2 What is a Virtual Environment and Why Do You Need One?

Imagine you're working on two projects. Project A needs version 2.0 of a library, but Project B needs version 3.0 of the same library. If you install both globally, they'll conflict with each other.

A **virtual environment** is like a sealed box for each project. Each box has its own copy of Python and its own set of installed packages. This way, Project A and Project B can each have the exact versions they need without interfering with each other.

```bash
# Create a virtual environment named "myenv"
python -m venv myenv

# Activate it (you must do this every time you work on the project)

# On Mac/Linux:
source myenv/bin/activate

# On Windows:
myenv\Scripts\activate

# You'll see (myenv) appear at the start of your terminal prompt.
# This means you're "inside" the virtual environment.

# To deactivate (leave the virtual environment):
deactivate
```

### 3.3 Installing Django

With your virtual environment activated:

```bash
pip install django

# Verify the installation
python -m django --version
```

`pip` is Python's package installer. It downloads and installs packages from the Python Package Index (PyPI).

---

## 4. Creating Your First Project

### 4.1 Project vs App — Understanding the Difference

This is a concept that confuses many beginners, so let's be very clear:

A **project** is your entire website. It contains all the configuration and settings.

An **app** is a specific piece of functionality within your website. Think of apps as building blocks.

For example, if you're building an e-commerce website (the project), you might have these apps:
- A `products` app (handles product listings)
- A `cart` app (handles shopping carts)
- An `orders` app (handles order processing)
- An `accounts` app (handles user registration and profiles)

Each app is designed to do one thing well, and apps can be reused across different projects.

### 4.2 Creating the Project

```bash
# This creates a new Django project called "mysite"
django-admin startproject mysite

# Move into the project directory
cd mysite
```

### 4.3 Creating Your First App

```bash
# This creates a new app called "blog" inside your project
python manage.py startapp blog
```

After creating an app, you **must register it** in your project's settings, or Django won't know it exists:

```python
# mysite/settings.py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog',   # Add your app here
]
```

### 4.4 Running the Development Server

```bash
python manage.py runserver
```

Open your browser and go to `http://127.0.0.1:8000/`. You should see a rocket ship and "The install worked successfully!" This is Django's default welcome page.

`127.0.0.1` means "this computer" (localhost), and `8000` is the port number. The development server is only for development — never use it in production.

---

## 5. Understanding the Project Structure

After creating a project and an app, your file structure looks like this:

```
mysite/                  ← The outer folder (just a container, name doesn't matter)
├── manage.py            ← A command-line tool for managing your project
├── mysite/              ← The inner folder (the actual Python package for your project)
│   ├── __init__.py      ← Tells Python this folder is a package
│   ├── settings.py      ← All configuration for your project
│   ├── urls.py          ← The "table of contents" for your website's URLs
│   ├── asgi.py          ← Entry point for ASGI servers (async)
│   └── wsgi.py          ← Entry point for WSGI servers (traditional)
└── blog/                ← Your app
    ├── __init__.py      ← Tells Python this folder is a package
    ├── admin.py         ← Configuration for the admin panel
    ├── apps.py          ← Configuration for the app itself
    ├── models.py        ← Your data models (database tables)
    ├── tests.py         ← Your tests
    ├── views.py         ← Your views (logic for handling requests)
    └── migrations/      ← Database migration files (auto-generated)
        └── __init__.py
```

Let's explain each important file:

**manage.py** — You never edit this file. You use it to run commands like starting the server, creating database tables, making migrations, creating admin users, and running tests. Think of it as a Swiss Army knife for your project.

**settings.py** — This is the control center of your project. It contains every setting: which database to use, which apps are installed, where to find templates, security settings, time zone, language, and much more. We'll explore this in detail throughout the tutorial.

**urls.py** — This maps URLs to views. When someone visits `/about/`, this file tells Django which view function should handle that request. Think of it as a receptionist who directs visitors to the right office.

**wsgi.py and asgi.py** — These are entry points for web servers. WSGI (Web Server Gateway Interface) is the traditional protocol for Python web apps. ASGI (Asynchronous Server Gateway Interface) is the newer async protocol. You rarely need to modify these.

**models.py** — This is where you define your data structure. Each model becomes a database table. Instead of writing SQL, you describe your data in Python classes.

**views.py** — This is where you write the logic. A view receives a web request, does something (like query the database), and returns a web response (like an HTML page).

**admin.py** — This is where you register your models to appear in Django's built-in admin panel.

**migrations/** — This folder stores migration files. Migrations are like version control for your database schema. When you change a model, Django generates a migration file that describes the change, and then applies it to the database.

---

## 6. How Django Handles a Request (The Request-Response Cycle)

Understanding this cycle is fundamental to understanding everything else in Django. Here's what happens when someone visits `http://yoursite.com/blog/post/5/`:

**Step 1 — URL Resolution:** Django looks at the URL `/blog/post/5/` and checks it against the patterns defined in `urls.py`. It finds a match and identifies which view function should handle this request.

**Step 2 — View Processing:** Django calls the matched view function, passing it the request object and any URL parameters (like the `5`). The view function contains your business logic — it might query the database for post #5, check if the user is logged in, or perform calculations.

**Step 3 — Template Rendering (optional):** If the view needs to return an HTML page, it loads a template and fills in the dynamic data (like the post's title, content, and author). This process is called "rendering."

**Step 4 — Response:** The view returns an HTTP response — usually the rendered HTML. Django sends this response back to the user's browser, which displays the page.

Middleware (which we'll cover later) can intervene at various points in this cycle to add functionality like authentication checking, logging, or security headers.

---

## 7. URLs and Routing

### 7.1 What is URL Routing?

URL routing is the process of mapping URLs (web addresses) to the code that should handle them. When someone visits `/about/`, Django needs to know which piece of your code should respond.

Think of it like a phone system: when you call a company and press "1" for sales, "2" for support, or "3" for billing, the system routes your call to the right department. URL routing does the same thing for web addresses.

### 7.2 The Project-Level urls.py

This is the "main switchboard." It's the first place Django looks when a request comes in.

```python
# mysite/urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),        # Built-in admin panel
    path('blog/', include('blog.urls')),     # Send all /blog/... URLs to the blog app
    path('shop/', include('shop.urls')),     # Send all /shop/... URLs to the shop app
]
```

The `include()` function is very important. It says: "For any URL that starts with `blog/`, chop off that prefix and send the rest to `blog/urls.py` for further matching." This keeps your URL configuration modular — each app manages its own URLs.

### 7.3 The App-Level urls.py

You need to create this file yourself — it doesn't exist by default.

```python
# blog/urls.py
from django.urls import path
from . import views

app_name = 'blog'  # This is the "namespace" — explained below

urlpatterns = [
    path('', views.post_list, name='post_list'),
    # Matches: /blog/

    path('post/<int:pk>/', views.post_detail, name='post_detail'),
    # Matches: /blog/post/1/, /blog/post/42/, etc.

    path('category/<slug:slug>/', views.category_posts, name='category_posts'),
    # Matches: /blog/category/python/, /blog/category/web-dev/, etc.

    path('archive/<int:year>/<int:month>/', views.archive, name='archive'),
    # Matches: /blog/archive/2025/3/, /blog/archive/2024/12/, etc.
]
```

### 7.4 Understanding path() Parameters

The `path()` function takes these arguments:

- **Route** (first argument): The URL pattern to match. `'post/<int:pk>/'` means: match the literal text "post/", then capture an integer and call it "pk", then match a trailing slash.

- **View** (second argument): The view function to call when this URL is matched.

- **Name** (keyword argument): A unique name for this URL pattern. This is extremely useful because you can refer to URLs by name instead of hardcoding the actual path. If you ever change a URL, you only change it in one place.

### 7.5 URL Path Converters

The angle brackets `< >` in URL patterns capture parts of the URL and pass them to the view:

- `<int:pk>` — Matches an integer. Example: `42`. The captured value is passed to the view as an integer named `pk`.
- `<str:name>` — Matches any non-empty string, excluding `/`. Example: `hello`.
- `<slug:slug>` — Matches a slug (letters, numbers, hyphens, underscores). Example: `my-first-post`.
- `<uuid:id>` — Matches a UUID. Example: `075194d3-6885-417e-a8a8-6c931e272f00`.
- `<path:file_path>` — Matches any string, including `/`. Example: `docs/2025/report.pdf`.

### 7.6 Naming URLs and the app_name Namespace

```python
app_name = 'blog'  # namespace

urlpatterns = [
    path('post/<int:pk>/', views.post_detail, name='post_detail'),
]
```

The `app_name` creates a namespace. Combined with the URL name, you can refer to this URL as `'blog:post_detail'` anywhere in your project. This prevents naming conflicts — if both your `blog` app and `shop` app have a URL named `detail`, namespaces keep them separate.

**Using named URLs in templates:**

```html
<a href="{% url 'blog:post_detail' pk=post.pk %}">Read More</a>
```

**Using named URLs in Python code:**

```python
from django.urls import reverse

url = reverse('blog:post_detail', kwargs={'pk': 42})
# Returns: '/blog/post/42/'
```

The `{% url %}` tag and `reverse()` function look up the URL by its name and build the actual path. If you later change the URL pattern from `post/<int:pk>/` to `article/<int:pk>/`, every link using the name `'blog:post_detail'` automatically points to the new URL. You never have to search through your codebase and update hardcoded paths.

---

## 8. Views — The Brain of Your Application

### 8.1 What is a View?

A view is a Python function (or class) that takes a web request and returns a web response. It's where your application's logic lives.

Every view does three things:
1. Receives a request (what the user is asking for)
2. Does something (queries the database, processes data, checks permissions)
3. Returns a response (usually an HTML page or JSON data)

### 8.2 Function-Based Views (FBVs)

The simplest type of view is a function. It receives a request object and returns a response object.

```python
# blog/views.py
from django.http import HttpResponse

def hello(request):
    return HttpResponse("Hello, World!")
```

`request` is an object that contains everything about the incoming request — what URL was visited, what method was used (GET/POST), what data was sent, who the user is, what cookies are present, and more.

`HttpResponse` is the simplest type of response. You give it a string, and it sends that string back to the browser.

### 8.3 Returning HTML with Templates

In practice, you almost never return raw HTML strings. Instead, you use templates:

```python
from django.shortcuts import render

def post_list(request):
    posts = Post.objects.filter(status='published').order_by('-created_at')

    context = {
        'posts': posts,
        'page_title': 'All Blog Posts',
    }

    return render(request, 'blog/post_list.html', context)
```

Let's break this down:

- `render()` is a shortcut function that combines a template with data and returns an `HttpResponse`. It takes three arguments: the request, the template path, and a dictionary of data to pass to the template.

- `context` is a dictionary. The keys become variable names available inside the template. So in the template, you can use `{{ posts }}` and `{{ page_title }}`.

- `'blog/post_list.html'` is the path to the template file, relative to the templates directory.

### 8.4 Handling Different HTTP Methods

Views often need to handle GET and POST differently:

```python
from django.shortcuts import render, redirect
from .forms import PostForm

def create_post(request):
    # When the user submits the form (POST request)
    if request.method == 'POST':
        form = PostForm(request.POST, request.FILES)
        if form.is_valid():
            post = form.save(commit=False)
            post.author = request.user
            post.save()
            return redirect('blog:post_detail', pk=post.pk)
    # When the user first visits the page (GET request)
    else:
        form = PostForm()

    return render(request, 'blog/post_form.html', {'form': form})
```

The flow works like this: When the user first visits the page (GET request), they see an empty form. When they fill it out and click submit (POST request), the view validates the data. If it's valid, it saves and redirects. If not, it re-displays the form with error messages.

### 8.5 The Request Object

The `request` object contains a wealth of information:

```python
def my_view(request):
    request.method        # 'GET', 'POST', etc.
    request.GET           # Dictionary of query string parameters (?key=value)
    request.POST          # Dictionary of POST data (form submissions)
    request.FILES         # Dictionary of uploaded files
    request.user          # The currently logged-in user (or AnonymousUser)
    request.path          # The URL path, e.g., '/blog/post/5/'
    request.session       # The session dictionary (for storing user-specific data)
    request.COOKIES       # Dictionary of cookies
    request.META          # Dictionary of HTTP headers and server info
    request.is_ajax()     # Whether the request was made via AJAX (deprecated)
    request.content_type  # The content type of the request body
```

### 8.6 Common Response Types

```python
from django.http import (
    HttpResponse,
    HttpResponseRedirect,
    HttpResponseNotFound,
    JsonResponse,
    Http404,
)
from django.shortcuts import render, redirect, get_object_or_404

# Plain text response
return HttpResponse("Hello")

# HTML response (via template)
return render(request, 'template.html', context)

# Redirect to another URL
return redirect('blog:post_list')

# JSON response (for APIs)
return JsonResponse({'status': 'ok', 'count': 42})

# 404 Not Found
raise Http404("This post does not exist")

# Get object or 404 (very common pattern)
post = get_object_or_404(Post, pk=pk, status='published')
# This is a shortcut for: try to get the object, and if it doesn't exist,
# automatically return a 404 error page.
```

### 8.7 View Decorators

Decorators are functions that wrap your view to add extra behavior:

```python
from django.contrib.auth.decorators import login_required
from django.views.decorators.http import require_POST
from django.views.decorators.cache import cache_page

@login_required
# If the user is not logged in, redirect them to the login page
def dashboard(request):
    return render(request, 'dashboard.html')

@require_POST
# Only allow POST requests to this view. GET requests will get a 405 error.
def delete_comment(request, pk):
    comment = get_object_or_404(Comment, pk=pk)
    comment.delete()
    return redirect('blog:post_list')

@cache_page(60 * 15)
# Cache the response for 15 minutes. Subsequent requests within that time
# will get the cached response instead of re-executing the view.
def expensive_report(request):
    data = generate_expensive_report()
    return render(request, 'report.html', {'data': data})
```

---

## 9. Templates — The Face of Your Application

### 9.1 What is a Template?

A template is an HTML file with special Django syntax sprinkled in. It defines what the user sees. The special syntax lets you insert dynamic data, loop through lists, make decisions, and reuse layouts.

Think of a template as a form letter with blanks to fill in. The structure (HTML) stays the same, but the specific data (names, dates, content) changes for each request.

### 9.2 Where to Put Templates

Django looks for templates in two places:

1. **App-level templates:** Inside each app's `templates/` directory. The convention is `app_name/templates/app_name/template.html` (yes, the app name appears twice — this prevents naming conflicts between apps).

2. **Project-level templates:** A shared `templates/` directory at the project root. You configure this in settings:

```python
# mysite/settings.py
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'templates'],  # Project-level templates directory
        'APP_DIRS': True,  # Also look in each app's templates/ directory
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
```

**Context processors** are functions that automatically add variables to every template. For example, `auth` adds `{{ user }}` to every template so you can always check who's logged in.

### 9.3 Template Language Syntax

Django's template language has three types of special syntax:

**Variables: `{{ variable }}`**

These output values. When Django sees `{{ post.title }}`, it replaces it with the actual title of the post.

```html
<h1>{{ post.title }}</h1>
<p>By {{ post.author.username }}</p>
<p>Published: {{ post.created_at }}</p>
```

The dot (`.`) operator is versatile. `post.title` could mean:
- Dictionary key lookup: `post['title']`
- Attribute lookup: `post.title`
- Method call: `post.title()` (no arguments allowed)
- List index: `post.0` (first item)

Django tries each of these in order until one works.

**Tags: `{% tag %}`**

Tags provide logic — loops, conditions, template inheritance, and more. Some tags require a closing tag.

```html
{% if user.is_authenticated %}
    <p>Welcome back, {{ user.username }}!</p>
{% else %}
    <p>Please <a href="{% url 'login' %}">log in</a>.</p>
{% endif %}

{% for post in posts %}
    <article>
        <h2>{{ post.title }}</h2>
        <p>{{ post.content|truncatewords:30 }}</p>
    </article>
{% empty %}
    <p>No posts have been published yet.</p>
{% endfor %}
```

The `{% empty %}` block inside a for loop is displayed when the list is empty. This is a Django-specific convenience — most template languages don't have this.

**Filters: `{{ variable|filter }}`**

Filters transform variables before displaying them. They use the pipe character `|`.

```html
{{ post.title|upper }}
<!-- "my post title" becomes "MY POST TITLE" -->

{{ post.title|lower }}
<!-- "My Post Title" becomes "my post title" -->

{{ post.title|title }}
<!-- "my post title" becomes "My Post Title" -->

{{ post.content|truncatewords:30 }}
<!-- Shows only the first 30 words, followed by "..." -->

{{ post.content|truncatewords_html:30 }}
<!-- Same, but preserves HTML tags -->

{{ post.content|linebreaks }}
<!-- Converts newlines into <p> and <br> tags -->

{{ post.created_at|date:"F d, Y" }}
<!-- "March 15, 2025" -->

{{ post.created_at|timesince }}
<!-- "3 days, 2 hours ago" -->

{{ price|floatformat:2 }}
<!-- "19.99" -->

{{ items|length }}
<!-- Number of items in the list -->

{{ bio|default:"No bio provided." }}
<!-- Shows "No bio provided." if bio is empty or None -->

{{ post.content|safe }}
<!-- Marks the content as safe HTML (won't be escaped) -->
<!-- WARNING: Only use this on content you trust! -->

{{ text|slugify }}
<!-- "Hello World!" becomes "hello-world" -->

{{ number|add:5 }}
<!-- Adds 5 to the number -->

{{ text|wordcount }}
<!-- Returns the number of words -->
```

### 9.4 Template Inheritance — The Most Powerful Feature

Template inheritance lets you create a base "skeleton" template that contains the common parts of your site (header, navigation, footer), and then child templates that fill in the unique content for each page.

**Why is this important?** Without inheritance, every template would repeat the same HTML boilerplate — the `<head>`, navigation bar, footer, CSS links, JavaScript files, etc. If you need to change the navigation, you'd have to edit every single template. Template inheritance means you change it once in the base template, and every page updates.

```html
<!-- templates/base.html — The skeleton -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block title %}My Site{% endblock %}</title>
    {% load static %}
    <link rel="stylesheet" href="{% static 'css/style.css' %}">
    {% block extra_css %}{% endblock %}
</head>
<body>
    <header>
        <nav>
            <a href="{% url 'blog:post_list' %}">Home</a>
            <a href="{% url 'blog:about' %}">About</a>
            {% if user.is_authenticated %}
                <a href="{% url 'logout' %}">Logout ({{ user.username }})</a>
            {% else %}
                <a href="{% url 'login' %}">Login</a>
            {% endif %}
        </nav>
    </header>

    <main>
        {% if messages %}
            {% for message in messages %}
                <div class="alert alert-{{ message.tags }}">{{ message }}</div>
            {% endfor %}
        {% endif %}

        {% block content %}
        <!-- Child templates fill this in -->
        {% endblock %}
    </main>

    <footer>
        <p>© 2025 My Site. All rights reserved.</p>
    </footer>

    {% block extra_js %}{% endblock %}
</body>
</html>
```

```html
<!-- blog/templates/blog/post_list.html — A child template -->
{% extends "base.html" %}

{% block title %}Blog Posts — My Site{% endblock %}

{% block content %}
    <h1>Latest Posts</h1>

    {% for post in posts %}
        <article>
            <h2><a href="{% url 'blog:post_detail' pk=post.pk %}">{{ post.title }}</a></h2>
            <p class="meta">
                By {{ post.author.username }} |
                {{ post.created_at|date:"F d, Y" }}
            </p>
            <p>{{ post.content|truncatewords:50 }}</p>
        </article>
    {% empty %}
        <p>No posts yet.</p>
    {% endfor %}
{% endblock %}
```

**How it works:**

- `{% extends "base.html" %}` must be the first tag in the child template. It says: "Start with the base template and override specific blocks."
- `{% block name %}...{% endblock %}` defines a block that can be overridden. If a child template defines a block with the same name, its content replaces the parent's content.
- Anything in the base template that's NOT inside a block cannot be changed by child templates.
- Blocks that the child doesn't override keep their default content from the parent.

### 9.5 Template Includes

For reusable components like a sidebar, card, or widget:

```html
<!-- templates/partials/_post_card.html -->
<article class="post-card">
    <h2><a href="{% url 'blog:post_detail' pk=post.pk %}">{{ post.title }}</a></h2>
    <p>{{ post.excerpt }}</p>
</article>
```

```html
<!-- Using the partial -->
{% for post in posts %}
    {% include "partials/_post_card.html" with post=post %}
{% endfor %}
```

The `with` keyword lets you pass variables to the included template. The underscore prefix (`_post_card.html`) is a convention indicating this is a partial template, not a full page.

---

## 10. Models — The Heart of Your Data

### 10.1 What is a Model?

A model is a Python class that represents a database table. Each attribute of the class represents a column in the table. Each instance of the class represents a row.

Instead of writing SQL to create tables and query data, you define your data structure in Python, and Django translates it to the correct SQL for your database.

```python
# blog/models.py
from django.db import models

class Post(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
```

This creates a database table called `blog_post` (app name + underscore + model name in lowercase) with columns: `id` (automatic), `title`, `content`, and `created_at`.

### 10.2 Field Types Explained

Each field type tells Django two things: what kind of data to store in the database, and what kind of HTML form widget to display.

**Text Fields:**

```python
# CharField — For short text (titles, names, etc.)
# REQUIRES max_length. In the database, this becomes VARCHAR.
title = models.CharField(max_length=200)

# TextField — For long text (article content, descriptions)
# No max_length needed. In the database, this becomes TEXT.
content = models.TextField()

# SlugField — For URL-friendly strings ("my-first-post")
# Only allows letters, numbers, hyphens, and underscores.
slug = models.SlugField(max_length=200, unique=True)

# EmailField — Validates that the input is an email address
email = models.EmailField()

# URLField — Validates that the input is a URL
website = models.URLField(blank=True)
```

**Numeric Fields:**

```python
# IntegerField — Whole numbers (-2147483648 to 2147483647)
view_count = models.IntegerField(default=0)

# PositiveIntegerField — Only positive whole numbers (0 to 2147483647)
age = models.PositiveIntegerField()

# FloatField — Floating point numbers (may have rounding issues)
rating = models.FloatField()

# DecimalField — Exact decimal numbers (good for money)
# max_digits = total digits, decimal_places = digits after the point
price = models.DecimalField(max_digits=10, decimal_places=2)
# Can store up to 99999999.99
```

**Boolean Fields:**

```python
# BooleanField — True or False
is_published = models.BooleanField(default=False)
```

**Date and Time Fields:**

```python
# DateField — A date (year, month, day)
birthday = models.DateField()

# DateTimeField — A date and time
# auto_now_add=True → automatically set when the object is CREATED (never changes)
# auto_now=True → automatically set every time the object is SAVED (updates on every save)
created_at = models.DateTimeField(auto_now_add=True)
updated_at = models.DateTimeField(auto_now=True)

# TimeField — Just a time
alarm_time = models.TimeField()
```

**File Fields:**

```python
# FileField — Any file upload
document = models.FileField(upload_to='documents/%Y/%m/')
# upload_to defines the subdirectory within MEDIA_ROOT
# %Y/%m/ creates year/month subdirectories: documents/2025/03/report.pdf

# ImageField — Image file upload (requires Pillow library)
photo = models.ImageField(upload_to='photos/')
```

**Relationship Fields** (covered in detail in Chapter 18):

```python
# ForeignKey — Many-to-one relationship
# Example: Many posts belong to one author
author = models.ForeignKey(User, on_delete=models.CASCADE)

# ManyToManyField — Many-to-many relationship
# Example: A post can have many tags, and a tag can be on many posts
tags = models.ManyToManyField('Tag')

# OneToOneField — One-to-one relationship
# Example: One user has one profile
profile = models.OneToOneField(User, on_delete=models.CASCADE)
```

**Other Useful Fields:**

```python
# UUIDField — Universally Unique Identifier
import uuid
id = models.UUIDField(primary_key=True, default=uuid.uuid4, editable=False)

# JSONField — Stores JSON data
metadata = models.JSONField(default=dict)
```

### 10.3 Field Options Explained

Every field type accepts certain options (keyword arguments) that control its behavior:

```python
# null=True → The database can store NULL for this field.
# Means "no value in the database."
# Use for non-string fields (numbers, dates, ForeignKeys).
age = models.IntegerField(null=True)

# blank=True → The field can be left empty in forms.
# Means "no value from the user."
# Use for optional form fields.
bio = models.TextField(blank=True)

# COMMON CONFUSION: null vs blank
# null is about the DATABASE. blank is about FORMS/VALIDATION.
# For string fields (CharField, TextField), use blank=True but NOT null=True.
# Django stores empty strings "" instead of NULL for text fields.
# For non-string fields, use both: null=True, blank=True

# default → The default value if none is provided.
status = models.CharField(max_length=10, default='draft')
view_count = models.IntegerField(default=0)

# unique=True → No two rows can have the same value in this column.
email = models.EmailField(unique=True)

# choices → Limits the field to a predefined set of options.
# The first element of each tuple is the database value,
# the second is the human-readable label.
STATUS_CHOICES = [
    ('draft', 'Draft'),
    ('published', 'Published'),
    ('archived', 'Archived'),
]
status = models.CharField(max_length=10, choices=STATUS_CHOICES, default='draft')
# In templates: {{ post.get_status_display }} outputs "Draft" or "Published"

# db_index=True → Creates a database index for faster lookups.
# Use on fields you frequently filter or search by.
email = models.EmailField(db_index=True)

# verbose_name → A human-readable name for the field (used in the admin).
first_name = models.CharField("First Name", max_length=100)

# help_text → Extra text shown in forms to guide the user.
slug = models.SlugField(help_text="URL-friendly version of the title.")

# editable=False → The field won't appear in any form.
created_at = models.DateTimeField(auto_now_add=True, editable=False)

# primary_key=True → Makes this field the primary key instead of the auto-generated id.
# You rarely need this — Django creates an auto-incrementing id by default.
```

### 10.4 The Meta Class

The `Meta` class inside a model sets options that apply to the whole model (not a single field):

```python
class Post(models.Model):
    title = models.CharField(max_length=200)
    created_at = models.DateTimeField(auto_now_add=True)

    class Meta:
        # Default ordering for queries. "-" means descending.
        ordering = ['-created_at']

        # Human-readable name for the model
        verbose_name = 'blog post'
        verbose_name_plural = 'blog posts'

        # Database table name (defaults to appname_modelname)
        db_table = 'posts'

        # Unique constraints across multiple fields
        unique_together = [['title', 'author']]
        # OR using the newer syntax:
        constraints = [
            models.UniqueConstraint(
                fields=['title', 'author'],
                name='unique_post_per_author'
            )
        ]

        # Database indexes for performance
        indexes = [
            models.Index(fields=['status', 'created_at']),
        ]
```

### 10.5 Model Methods

You can add methods to your models to encapsulate business logic:

```python
class Post(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    status = models.CharField(max_length=10, choices=STATUS_CHOICES)

    def __str__(self):
        """
        This controls how the object is displayed as a string.
        Used in the admin panel, shell, and debugging.
        Without this, you'd see "Post object (1)" which isn't helpful.
        """
        return self.title

    def get_absolute_url(self):
        """
        Returns the canonical URL for this object.
        Used by the admin, and by the redirect after form submission.
        """
        from django.urls import reverse
        return reverse('blog:post_detail', kwargs={'pk': self.pk})

    @property
    def is_published(self):
        """
        A property lets you access this like an attribute: post.is_published
        instead of like a method: post.is_published()
        """
        return self.status == 'published'

    def word_count(self):
        """Returns the number of words in the content."""
        return len(self.content.split())

    def reading_time(self):
        """Estimates reading time in minutes (assuming 200 words per minute)."""
        return max(1, self.word_count() // 200)
```

---

## 11. Migrations — Version Control for Your Database

### 11.1 What are Migrations?

Imagine you build a database table for blog posts with `title` and `content`. A week later, you realize you also need a `published_date` field. You can't just add it to your Python code — the actual database table doesn't have that column yet.

Migrations solve this problem. They're files that describe changes to your database schema. When you change a model, Django generates a migration file that says things like "add column `published_date` to table `blog_post`." Then you apply that migration, and Django executes the SQL to modify the database.

It's like Git, but for your database structure.

### 11.2 The Migration Workflow

```bash
# Step 1: Make changes to your models.py

# Step 2: Generate migration files
python manage.py makemigrations
# Django compares your models to the existing migrations and creates
# new migration files for any changes it detects.
# Output: blog/migrations/0001_initial.py (for a new model)
# Output: blog/migrations/0002_post_published_date.py (for a change)

# Step 3: Apply migrations to the database
python manage.py migrate
# Django reads the migration files and executes the SQL commands
# to update the database schema.

# Useful commands:
python manage.py showmigrations
# Shows which migrations have been applied and which haven't.

python manage.py sqlmigrate blog 0001
# Shows the SQL that a specific migration will execute,
# without actually running it. Useful for debugging.

python manage.py migrate blog 0001
# Roll back to a specific migration (undo later migrations).
```

### 11.3 Common Migration Scenarios

**Adding a new field to an existing model:** When you add a field that isn't nullable and has no default, Django will ask you what to do about existing rows. You'll either provide a one-time default value or set a default in the model.

**Renaming a field:** Django will ask if you renamed the field or deleted the old one and created a new one. Choose "rename" to preserve existing data.

**Deleting a field:** The migration will remove the column and ALL data in it. This is irreversible.

**Important:** Never edit migration files unless you know exactly what you're doing. Never delete migration files that have been applied. Always run `makemigrations` and `migrate` together.

---

## 12. The Django ORM — Talking to Your Database in Python

### 12.1 What is the ORM?

ORM stands for Object-Relational Mapper. It's a bridge between your Python code and the database. Instead of writing SQL queries, you write Python code, and the ORM translates it to SQL behind the scenes.

```python
# Instead of writing SQL:
# SELECT * FROM blog_post WHERE status = 'published' ORDER BY created_at DESC;

# You write Python:
Post.objects.filter(status='published').order_by('-created_at')
```

Every model has a `Manager` called `objects` that provides methods for querying the database. These methods return `QuerySets` — lazy collections of database objects.

### 12.2 QuerySets are Lazy

This is a crucial concept. When you write `Post.objects.all()`, Django does NOT immediately hit the database. It creates a QuerySet object that describes what you want. The database is only queried when you actually need the data — when you iterate over the QuerySet, convert it to a list, access a specific item, or print it.

```python
# No database query yet — just building the QuerySet
qs = Post.objects.filter(status='published')
qs = qs.filter(author=user)
qs = qs.order_by('-created_at')
qs = qs[:10]

# NOW the database is queried — because we're iterating
for post in qs:
    print(post.title)
```

This laziness lets you chain multiple operations together without wasting database queries.

### 12.3 CRUD Operations

**CREATE — Adding New Records:**

```python
# Method 1: create() — creates and saves in one step
post = Post.objects.create(
    title='My First Post',
    slug='my-first-post',
    author=user,
    content='Hello, world!',
    status='published',
)
# The post is now saved in the database and has a pk (primary key).

# Method 2: Instantiate and save — gives you a chance to modify before saving
post = Post(title='My Post', slug='my-post', author=user)
post.content = 'I can set fields after creating the object.'
post.save()  # NOW it's saved to the database.

# Method 3: get_or_create() — get an existing object, or create it if it doesn't exist
post, created = Post.objects.get_or_create(
    slug='my-post',  # lookup criteria
    defaults={        # values to set if creating
        'title': 'My Post',
        'author': user,
        'content': 'Default content',
    }
)
# created is True if a new object was created, False if it already existed.
```

**READ — Retrieving Records:**

```python
# Get ALL records
all_posts = Post.objects.all()

# Get a SINGLE record (raises exception if not found or if multiple found)
post = Post.objects.get(pk=1)
# If no match: raises Post.DoesNotExist
# If multiple matches: raises Post.MultipleObjectsReturned

# SAFER: get_object_or_404 — returns a 404 page if not found
from django.shortcuts import get_object_or_404
post = get_object_or_404(Post, pk=1)

# FILTER — Get records matching conditions (returns QuerySet, may be empty)
published_posts = Post.objects.filter(status='published')
user_posts = Post.objects.filter(author=user)

# EXCLUDE — Get records NOT matching conditions
non_draft_posts = Post.objects.exclude(status='draft')

# ORDER BY
posts = Post.objects.order_by('-created_at')      # newest first
posts = Post.objects.order_by('title')             # alphabetical
posts = Post.objects.order_by('-status', 'title')  # multiple fields

# LIMIT (slicing)
latest_5 = Post.objects.order_by('-created_at')[:5]  # first 5
next_5 = Post.objects.order_by('-created_at')[5:10]  # items 6-10

# FIRST and LAST
first_post = Post.objects.order_by('created_at').first()  # or None
latest_post = Post.objects.order_by('-created_at').first()

# COUNT
total = Post.objects.count()
published_count = Post.objects.filter(status='published').count()

# EXISTS — Check if any records match (more efficient than count > 0)
has_posts = Post.objects.filter(author=user).exists()  # True or False
```

**Field Lookups (used with filter, exclude, get):**

Field lookups use double underscores `__` to specify the type of comparison:

```python
# Exact match (default — you don't need to write __exact)
Post.objects.filter(title='My Post')
Post.objects.filter(title__exact='My Post')  # same thing

# Case-insensitive exact match
Post.objects.filter(title__iexact='my post')

# Contains (case-sensitive substring search)
Post.objects.filter(title__contains='Django')

# Case-insensitive contains (most commonly used for search)
Post.objects.filter(title__icontains='django')

# Starts with / Ends with
Post.objects.filter(title__startswith='How')
Post.objects.filter(title__endswith='?')
Post.objects.filter(title__istartswith='how')  # case-insensitive

# Greater than, less than, etc.
Post.objects.filter(view_count__gt=100)    # > 100
Post.objects.filter(view_count__gte=100)   # >= 100
Post.objects.filter(view_count__lt=10)     # < 10
Post.objects.filter(view_count__lte=10)    # <= 10

# In a list
Post.objects.filter(status__in=['published', 'archived'])

# Range (inclusive)
from datetime import date
Post.objects.filter(created_at__range=(date(2025, 1, 1), date(2025, 12, 31)))

# NULL check
Post.objects.filter(category__isnull=True)   # category is NULL
Post.objects.filter(category__isnull=False)  # category is NOT NULL

# Date parts
Post.objects.filter(created_at__year=2025)
Post.objects.filter(created_at__month=3)
Post.objects.filter(created_at__day=15)

# Spanning relationships (using double underscores)
# "Get posts where the author's username is 'admin'"
Post.objects.filter(author__username='admin')
# "Get posts in the 'Python' category"
Post.objects.filter(category__name='Python')
```

**UPDATE — Modifying Records:**

```python
# Update a single object
post = Post.objects.get(pk=1)
post.title = 'Updated Title'
post.status = 'published'
post.save()
# save() saves ALL fields. For efficiency, you can specify which fields:
post.save(update_fields=['title', 'status'])

# Bulk update — updates all matching records in a single SQL query
# Much more efficient than looping and saving individually
Post.objects.filter(status='draft').update(status='published')
# Returns the number of rows updated.
```

**DELETE — Removing Records:**

```python
# Delete a single object
post = Post.objects.get(pk=1)
post.delete()

# Bulk delete
Post.objects.filter(status='draft').delete()
# Returns a tuple: (number_deleted, {model: count})
```

### 12.4 Advanced Queries

**Q Objects — Complex Lookups (OR, AND, NOT):**

By default, conditions in `filter()` are joined with AND. To use OR or NOT, you need `Q` objects:

```python
from django.db.models import Q

# OR: posts that contain "django" in the title OR content
Post.objects.filter(
    Q(title__icontains='django') | Q(content__icontains='django')
)

# AND: (you can also just chain .filter(), but Q makes complex queries clearer)
Post.objects.filter(
    Q(status='published') & Q(author=user)
)

# NOT: posts that are NOT drafts
Post.objects.filter(~Q(status='draft'))

# Complex combination
Post.objects.filter(
    (Q(status='published') | Q(author=user)) & ~Q(title='')
)
```

**F Expressions — Referencing Other Fields:**

`F()` lets you reference other fields in the same row, so the database does the comparison without loading data into Python:

```python
from django.db.models import F

# Posts where updated_at is later than created_at
Post.objects.filter(updated_at__gt=F('created_at'))

# Increment a field without loading the object (atomic — safe for concurrent access)
Post.objects.filter(pk=1).update(view_count=F('view_count') + 1)
```

**Aggregation — Computing Summary Statistics:**

```python
from django.db.models import Count, Avg, Sum, Max, Min

# Aggregate across all records (returns a dictionary)
Post.objects.aggregate(
    total_posts=Count('id'),
    total_views=Sum('view_count'),
    avg_views=Avg('view_count'),
    max_views=Max('view_count'),
    min_views=Min('view_count'),
)
# Returns: {'total_posts': 42, 'total_views': 10000, 'avg_views': 238.1, ...}
```

**Annotation — Adding Computed Fields:**

```python
from django.db.models import Count

# Add a comment_count to each post
posts = Post.objects.annotate(
    comment_count=Count('comments')
).order_by('-comment_count')

for post in posts:
    print(f"{post.title}: {post.comment_count} comments")
```

**Performance: select_related and prefetch_related:**

These solve the "N+1 query problem," one of the most common performance issues in web apps.

```python
# THE PROBLEM (N+1 queries):
posts = Post.objects.all()  # 1 query
for post in posts:
    print(post.author.username)  # 1 query PER post! (N additional queries)

# SOLUTION for ForeignKey and OneToOne: select_related
# Uses a SQL JOIN to fetch related objects in one query
posts = Post.objects.select_related('author', 'category').all()
# Now only 1 query! Author and category data is included.

# SOLUTION for ManyToMany and reverse ForeignKey: prefetch_related
# Makes a separate query for the related objects and joins them in Python
posts = Post.objects.prefetch_related('tags', 'comments').all()
# Now 3 queries total (posts + tags + comments) instead of potentially hundreds.
```

**Values and Values_list:**

When you only need specific fields, not full objects:

```python
# values() returns dictionaries
Post.objects.values('title', 'author__username')
# [{'title': 'Post 1', 'author__username': 'admin'}, ...]

# values_list() returns tuples
Post.objects.values_list('title', 'created_at')
# [('Post 1', datetime(...)), ...]

# flat=True with a single field returns a flat list
Post.objects.values_list('title', flat=True)
# ['Post 1', 'Post 2', 'Post 3']
```

### 12.5 Custom Managers

You can create custom managers to encapsulate common queries:

```python
class PublishedManager(models.Manager):
    def get_queryset(self):
        return super().get_queryset().filter(status='published')

class Post(models.Model):
    # ... fields ...
    objects = models.Manager()       # default manager (all posts)
    published = PublishedManager()   # custom manager (only published)

# Usage
Post.objects.all()      # all posts
Post.published.all()    # only published posts
```

---

## 13. The Django Admin Panel

### 13.1 What is the Admin Panel?

Django's admin is an automatically-generated web interface for managing your database records. It reads the metadata from your models and provides a fully functional interface for creating, reading, updating, and deleting records — with no additional code.

It's one of Django's most celebrated features. Most frameworks require you to build your own admin interface from scratch.

### 13.2 Setting Up the Admin

```bash
# Create a superuser account (admin account)
python manage.py createsuperuser
# You'll be prompted for a username, email, and password.
```

Visit `http://127.0.0.1:8000/admin/` and log in.

### 13.3 Registering Models

By default, the admin only shows Users and Groups. To manage your own models, register them:

```python
# blog/admin.py
from django.contrib import admin
from .models import Post, Category, Tag, Comment

# Simplest registration
admin.site.register(Category)
admin.site.register(Tag)
```

### 13.4 Customizing the Admin

For a more powerful admin experience, use the decorator syntax with a ModelAdmin class:

```python
@admin.register(Post)
class PostAdmin(admin.ModelAdmin):

    # list_display — Which columns to show on the list page
    list_display = ('title', 'author', 'category', 'status', 'created_at')

    # list_filter — Adds filter sidebar for quick filtering
    list_filter = ('status', 'category', 'created_at')

    # search_fields — Adds a search bar that searches these fields
    search_fields = ('title', 'content')

    # prepopulated_fields — Auto-fills the slug from the title as you type
    prepopulated_fields = {'slug': ('title',)}

    # list_editable — Makes fields editable directly on the list page
    list_editable = ('status',)

    # date_hierarchy — Adds date-based navigation at the top
    date_hierarchy = 'created_at'

    # ordering — Default sort order
    ordering = ('-created_at',)

    # raw_id_fields — For ForeignKey fields with many options,
    # shows a search popup instead of a dropdown
    raw_id_fields = ('author',)

    # filter_horizontal — For ManyToMany fields, shows a dual-list widget
    filter_horizontal = ('tags',)

    # readonly_fields — Fields that can be viewed but not edited
    readonly_fields = ('created_at', 'updated_at')

    # list_per_page — Number of records per page
    list_per_page = 25

    # fieldsets — Organize the edit form into sections
    fieldsets = (
        (None, {
            'fields': ('title', 'slug', 'author', 'category')
        }),
        ('Content', {
            'fields': ('content', 'excerpt', 'featured_image')
        }),
        ('Publishing', {
            'fields': ('status', 'tags'),
            'classes': ('collapse',),  # this section starts collapsed
        }),
        ('Timestamps', {
            'fields': ('created_at', 'updated_at'),
        }),
    )

    # Custom admin actions
    actions = ['make_published', 'make_draft']

    @admin.action(description='Mark selected posts as published')
    def make_published(self, request, queryset):
        count = queryset.update(status='published')
        self.message_user(request, f'{count} posts marked as published.')

    @admin.action(description='Mark selected posts as draft')
    def make_draft(self, request, queryset):
        count = queryset.update(status='draft')
        self.message_user(request, f'{count} posts marked as draft.')
```

### 13.5 Inline Admin (Editing Related Objects)

You can edit related objects on the same page as the parent:

```python
class CommentInline(admin.TabularInline):  # or admin.StackedInline
    model = Comment
    extra = 1  # number of empty forms to display
    readonly_fields = ('author', 'created_at')

@admin.register(Post)
class PostAdmin(admin.ModelAdmin):
    inlines = [CommentInline]
    # Now when editing a Post, you'll also see and edit its Comments.
```

---

## 14. Forms — Collecting User Input

### 14.1 What are Django Forms?

Django forms handle three things:
1. **Rendering** — Generating the HTML form fields
2. **Validation** — Checking that the submitted data is correct
3. **Cleaning** — Converting the raw data into Python objects

You could write HTML forms by hand, but Django forms give you automatic validation, CSRF protection, and error message handling.

### 14.2 Regular Forms (forms.Form)

Use these when you need a form that doesn't map directly to a model:

```python
# blog/forms.py
from django import forms

class ContactForm(forms.Form):
    name = forms.CharField(
        max_length=100,
        widget=forms.TextInput(attrs={
            'class': 'form-control',       # CSS class
            'placeholder': 'Your name',     # Placeholder text
        })
    )
    email = forms.EmailField(
        widget=forms.EmailInput(attrs={
            'class': 'form-control',
        })
    )
    subject = forms.CharField(max_length=200)
    message = forms.CharField(
        widget=forms.Textarea(attrs={
            'class': 'form-control',
            'rows': 5,
        })
    )

    # Custom validation for a specific field
    def clean_email(self):
        email = self.cleaned_data.get('email')
        if 'spam' in email:
            raise forms.ValidationError("We don't accept this email domain.")
        return email

    # Custom validation that involves multiple fields
    def clean(self):
        cleaned_data = super().clean()
        name = cleaned_data.get('name')
        message = cleaned_data.get('message')
        if name and message and name.lower() in message.lower():
            raise forms.ValidationError(
                "Don't repeat your name in the message body."
            )
        return cleaned_data
```

### 14.3 Model Forms (forms.ModelForm)

Use these when you want a form that creates or updates a model instance. Django automatically creates form fields based on the model's fields.

```python
from django import forms
from .models import Post

class PostForm(forms.ModelForm):
    class Meta:
        model = Post
        fields = ['title', 'slug', 'category', 'content', 'excerpt',
                  'featured_image', 'status', 'tags']
        # OR: exclude = ['author', 'created_at', 'updated_at']
        # OR: fields = '__all__'  (not recommended for security)

        widgets = {
            'title': forms.TextInput(attrs={'class': 'form-control'}),
            'content': forms.Textarea(attrs={'class': 'form-control', 'rows': 10}),
            'status': forms.Select(attrs={'class': 'form-control'}),
        }

        labels = {
            'content': 'Post Body',
        }

        help_texts = {
            'slug': 'URL-friendly version of the title (e.g., "my-first-post").',
        }
```

### 14.4 Using Forms in Views

```python
def contact(request):
    if request.method == 'POST':
        form = ContactForm(request.POST)
        if form.is_valid():
            # form.cleaned_data is a dictionary of validated data
            name = form.cleaned_data['name']
            email = form.cleaned_data['email']
            message = form.cleaned_data['message']
            # Send email, save to database, etc.
            return redirect('blog:thanks')
    else:
        form = ContactForm()

    return render(request, 'blog/contact.html', {'form': form})


def create_post(request):
    if request.method == 'POST':
        form = PostForm(request.POST, request.FILES)  # FILES for image uploads
        if form.is_valid():
            post = form.save(commit=False)  # create object but don't save yet
            post.author = request.user       # set the author
            post.save()                      # now save to database
            form.save_m2m()                  # save ManyToMany relationships (tags)
            return redirect(post.get_absolute_url())
    else:
        form = PostForm()
    return render(request, 'blog/post_form.html', {'form': form})


def edit_post(request, pk):
    post = get_object_or_404(Post, pk=pk)
    if request.method == 'POST':
        form = PostForm(request.POST, request.FILES, instance=post)
        # instance=post tells the form to UPDATE this post instead of creating a new one
        if form.is_valid():
            form.save()
            return redirect(post.get_absolute_url())
    else:
        form = PostForm(instance=post)  # pre-fill the form with existing data
    return render(request, 'blog/post_form.html', {'form': form})
```

### 14.5 Rendering Forms in Templates

```html
<form method="post" enctype="multipart/form-data">
    {% csrf_token %}
    <!-- csrf_token is REQUIRED for all POST forms. It's a security feature
         that prevents Cross-Site Request Forgery attacks. -->

    <!-- Option 1: Render the whole form automatically -->
    {{ form.as_p }}     <!-- each field wrapped in <p> tags -->
    {{ form.as_div }}   <!-- each field wrapped in <div> tags -->

    <!-- Option 2: Render fields individually (for custom styling) -->
    <div class="form-group">
        <label for="{{ form.title.id_for_label }}">Title</label>
        {{ form.title }}
        {% if form.title.help_text %}
            <small>{{ form.title.help_text }}</small>
        {% endif %}
        {% for error in form.title.errors %}
            <span class="error">{{ error }}</span>
        {% endfor %}
    </div>

    <!-- Non-field errors (errors from clean() method) -->
    {% if form.non_field_errors %}
        <div class="errors">
            {% for error in form.non_field_errors %}
                <p>{{ error }}</p>
            {% endfor %}
        </div>
    {% endif %}

    <button type="submit">Submit</button>
</form>
```

The `enctype="multipart/form-data"` attribute is required when your form has file upload fields. Without it, file uploads won't work.

---

## 15. Authentication and Authorization

### 15.1 Authentication vs Authorization

**Authentication** = "Who are you?" (Logging in, verifying identity)
**Authorization** = "What are you allowed to do?" (Permissions, access control)

Django provides both out of the box.

### 15.2 The User Model

Django comes with a built-in User model that includes: username, password (hashed), email, first_name, last_name, is_active, is_staff, is_superuser, date_joined, and last_login.

### 15.3 Using Django's Built-in Auth Views

Django provides ready-made views for login, logout, password change, and password reset:

```python
# mysite/urls.py
from django.urls import path, include

urlpatterns = [
    path('accounts/', include('django.contrib.auth.urls')),
]
```

This single line gives you all these URL patterns:
- `accounts/login/` — Login page
- `accounts/logout/` — Logout
- `accounts/password_change/` — Change password
- `accounts/password_change/done/` — Password changed confirmation
- `accounts/password_reset/` — Request password reset email
- `accounts/password_reset/done/` — Email sent confirmation
- `accounts/reset/<uidb64>/<token>/` — Reset password form
- `accounts/reset/done/` — Password reset confirmation

You need to create the templates. Django looks for them in `registration/` directory:

```html
<!-- templates/registration/login.html -->
{% extends "base.html" %}

{% block content %}
<h2>Login</h2>
<form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Login</button>
</form>
{% endblock %}
```

```python
# settings.py
LOGIN_REDIRECT_URL = '/'    # Where to go after login
LOGOUT_REDIRECT_URL = '/'   # Where to go after logout
LOGIN_URL = '/accounts/login/'  # Where to send users who need to login
```

### 15.4 Registration (Custom — Django Doesn't Include This)

```python
# accounts/views.py
from django.shortcuts import render, redirect
from django.contrib.auth import login
from django.contrib.auth.forms import UserCreationForm

def register(request):
    if request.method == 'POST':
        form = UserCreationForm(request.POST)
        if form.is_valid():
            user = form.save()
            login(request, user)  # automatically log in after registration
            return redirect('blog:post_list')
    else:
        form = UserCreationForm()
    return render(request, 'registration/register.html', {'form': form})
```

### 15.5 Protecting Views (Authorization)

```python
# Function-based views: use decorators
from django.contrib.auth.decorators import login_required, permission_required

@login_required
# If not logged in → redirect to LOGIN_URL
def my_dashboard(request):
    return render(request, 'dashboard.html')

@login_required
@permission_required('blog.add_post', raise_exception=True)
# Must be logged in AND have the 'blog.add_post' permission
def create_post(request):
    ...

# Class-based views: use mixins
from django.contrib.auth.mixins import LoginRequiredMixin, UserPassesTestMixin

class PostCreateView(LoginRequiredMixin, CreateView):
    ...

class PostUpdateView(LoginRequiredMixin, UserPassesTestMixin, UpdateView):
    ...
    def test_func(self):
        post = self.get_object()
        return self.request.user == post.author
        # Only the author can edit their own post
```

### 15.6 Checking Auth in Templates

```html
{% if user.is_authenticated %}
    <p>Welcome, {{ user.username }}!</p>
    <a href="{% url 'logout' %}">Logout</a>
    {% if user.is_staff %}
        <a href="{% url 'admin:index' %}">Admin Panel</a>
    {% endif %}
{% else %}
    <a href="{% url 'login' %}">Login</a>
    <a href="{% url 'register' %}">Register</a>
{% endif %}
```

### 15.7 Custom User Model (Best Practice)

Django's default User model may not have all the fields you need. It's strongly recommended to create a custom user model at the very start of a new project. Changing the user model later is extremely difficult.

```python
# accounts/models.py
from django.contrib.auth.models import AbstractUser
from django.db import models

class CustomUser(AbstractUser):
    bio = models.TextField(blank=True)
    avatar = models.ImageField(upload_to='avatars/', blank=True)
    date_of_birth = models.DateField(null=True, blank=True)
    website = models.URLField(blank=True)

    def __str__(self):
        return self.username
```

```python
# settings.py — MUST be set before the first migration
AUTH_USER_MODEL = 'accounts.CustomUser'
```

```python
# In other models, always reference the user model this way:
from django.conf import settings

class Post(models.Model):
    author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    # NOT: ForeignKey(User, ...) — this breaks if you change the user model
```

---

## 16. Static Files and Media Files

### 16.1 What's the Difference?

**Static files** are files that are part of your application code — CSS stylesheets, JavaScript files, images used in your design (logos, icons, backgrounds). They don't change based on user actions.

**Media files** are files uploaded by users — profile photos, document attachments, post images. They're dynamic and can change.

### 16.2 Static Files Configuration

```python
# settings.py

# The URL prefix for static files
STATIC_URL = '/static/'

# Extra directories to look for static files (project-level)
STATICFILES_DIRS = [BASE_DIR / 'static']

# Where collectstatic puts files for production
STATIC_ROOT = BASE_DIR / 'staticfiles'
```

```
mysite/
├── static/            ← project-level static files
│   ├── css/
│   │   └── style.css
│   ├── js/
│   │   └── script.js
│   └── images/
│       └── logo.png
```

```html
<!-- Using static files in templates -->
{% load static %}
<!-- The "load static" tag makes the "static" tag available -->

<link rel="stylesheet" href="{% static 'css/style.css' %}">
<script src="{% static 'js/script.js' %}"></script>
<img src="{% static 'images/logo.png' %}" alt="Logo">
```

### 16.3 Media Files Configuration

```python
# settings.py
MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / 'media'
```

```python
# mysite/urls.py (only for development!)
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    ...
]

if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

In production, your web server (Nginx, Apache) serves media files, not Django.

### 16.4 Collecting Static Files for Production

```bash
python manage.py collectstatic
```

This copies all static files from all apps and `STATICFILES_DIRS` into `STATIC_ROOT`. Your web server then serves them from there.

---

## 17. Class-Based Views (CBVs)

### 17.1 Why Class-Based Views?

Function-based views are simple and explicit, but when you start building many views that do similar things (list all objects, show one object, create an object, edit an object, delete an object), you end up writing the same code over and over.

Class-based views provide reusable, generic implementations of these common patterns. Instead of writing 30 lines of code for a list view, you write 3.

### 17.2 Generic Class-Based Views

```python
# blog/views.py
from django.views.generic import (
    ListView, DetailView, CreateView, UpdateView, DeleteView
)
from django.urls import reverse_lazy
from django.contrib.auth.mixins import LoginRequiredMixin


class PostListView(ListView):
    model = Post
    template_name = 'blog/post_list.html'   # default: blog/post_list.html
    context_object_name = 'posts'             # default: object_list
    paginate_by = 10                          # show 10 posts per page

    def get_queryset(self):
        """Override to customize which objects are listed."""
        return Post.objects.filter(status='published')

    def get_context_data(self, **kwargs):
        """Override to add extra data to the template context."""
        context = super().get_context_data(**kwargs)
        context['categories'] = Category.objects.all()
        return context


class PostDetailView(DetailView):
    model = Post
    template_name = 'blog/post_detail.html'  # default: blog/post_detail.html
    context_object_name = 'post'              # default: object

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context['comments'] = self.object.comments.filter(is_approved=True)
        context['comment_form'] = CommentForm()
        return context


class PostCreateView(LoginRequiredMixin, CreateView):
    model = Post
    form_class = PostForm
    template_name = 'blog/post_form.html'

    def form_valid(self, form):
        """Called when the form is valid. Set the author before saving."""
        form.instance.author = self.request.user
        return super().form_valid(form)
    # After successful save, redirects to post.get_absolute_url()


class PostUpdateView(LoginRequiredMixin, UpdateView):
    model = Post
    form_class = PostForm
    template_name = 'blog/post_form.html'

    def get_queryset(self):
        """Only allow users to edit their own posts."""
        return Post.objects.filter(author=self.request.user)


class PostDeleteView(LoginRequiredMixin, DeleteView):
    model = Post
    template_name = 'blog/post_confirm_delete.html'
    success_url = reverse_lazy('blog:post_list')
    # reverse_lazy is used instead of reverse because the URL is evaluated
    # at class definition time, before the URL configuration is loaded.

    def get_queryset(self):
        return Post.objects.filter(author=self.request.user)
```

### 17.3 URL Configuration for CBVs

```python
urlpatterns = [
    path('', PostListView.as_view(), name='post_list'),
    path('post/<int:pk>/', PostDetailView.as_view(), name='post_detail'),
    path('post/new/', PostCreateView.as_view(), name='post_create'),
    path('post/<int:pk>/edit/', PostUpdateView.as_view(), name='post_update'),
    path('post/<int:pk>/delete/', PostDeleteView.as_view(), name='post_delete'),
]
```

The `.as_view()` method converts the class into a callable view function that Django's URL resolver can use.

---

## 18. Relationships Between Models

### 18.1 One-to-Many (ForeignKey)

This is the most common relationship. One record in Table A can be related to many records in Table B, but each record in Table B relates to only one record in Table A.

Example: One author can write many posts, but each post has only one author.

```python
class Post(models.Model):
    author = models.ForeignKey(
        User,
        on_delete=models.CASCADE,    # if the user is deleted, delete their posts
        related_name='posts',        # allows user.posts.all()
    )
```

**on_delete options explained:**
- `CASCADE` — Delete the related objects too. (Delete user → delete all their posts)
- `PROTECT` — Prevent deletion. (Can't delete user if they have posts)
- `SET_NULL` — Set the field to NULL. (Delete user → post.author becomes NULL). Requires `null=True`.
- `SET_DEFAULT` — Set to the default value.
- `DO_NOTHING` — Do nothing. (Can cause database integrity errors)

**Querying:**

```python
# Forward: from Post to User
post = Post.objects.get(pk=1)
post.author                    # the User object
post.author.username           # the user's username

# Reverse: from User to Posts (using related_name)
user = User.objects.get(pk=1)
user.posts.all()               # all posts by this user
user.posts.count()             # number of posts
user.posts.filter(status='published')
```

### 18.2 Many-to-Many (ManyToManyField)

Both sides can have multiple related objects. Example: A post can have many tags, and a tag can be on many posts.

```python
class Post(models.Model):
    tags = models.ManyToManyField('Tag', blank=True, related_name='posts')
```

Django automatically creates an intermediary table to store these relationships.

```python
# Adding/removing relationships
post.tags.add(tag1, tag2)           # add tags to a post
post.tags.remove(tag1)              # remove a tag
post.tags.set([tag1, tag2, tag3])   # replace all tags
post.tags.clear()                   # remove all tags

# Querying
post.tags.all()          # all tags for this post
tag.posts.all()          # all posts with this tag
tag.posts.count()        # number of posts with this tag
```

### 18.3 One-to-One (OneToOneField)

Each record in Table A relates to exactly one record in Table B, and vice versa. Example: One user has one profile.

```python
class Profile(models.Model):
    user = models.OneToOneField(User, on_delete=models.CASCADE, related_name='profile')
    bio = models.TextField(blank=True)
    avatar = models.ImageField(upload_to='avatars/', blank=True)
```

```python
# Accessing
user.profile          # the Profile object (not a QuerySet)
user.profile.bio      # the bio field

profile.user          # the User object
profile.user.username
```

---

## 19. Middleware — The Gatekeepers

### 19.1 What is Middleware?

Middleware is a series of hooks that process every request and response that passes through Django. They sit between the web server and your views, like a series of checkpoints.

Each middleware can:
- Modify the request before it reaches the view
- Modify the response before it reaches the user
- Short-circuit the process and return a response without calling the view

Django's built-in middleware handles security headers, sessions, authentication, CSRF protection, and more.

### 19.2 How Middleware Works

Middleware processes in order for requests (top to bottom) and in reverse order for responses (bottom to top):

```
Request →  SecurityMiddleware → SessionMiddleware → AuthenticationMiddleware → YourView
Response ← SecurityMiddleware ← SessionMiddleware ← AuthenticationMiddleware ← YourView
```

### 19.3 Writing Custom Middleware

```python
# mysite/middleware.py

import time
import logging

logger = logging.getLogger(__name__)


class RequestTimingMiddleware:
    """Logs how long each request takes to process."""

    def __init__(self, get_response):
        # get_response is the next middleware or the view itself.
        # __init__ is called once when the server starts.
        self.get_response = get_response

    def __call__(self, request):
        # Code here runs BEFORE the view (and subsequent middleware)
        start_time = time.time()

        response = self.get_response(request)  # call the view

        # Code here runs AFTER the view
        duration = time.time() - start_time
        logger.info(f"{request.method} {request.path} took {duration:.2f}s")

        return response
```

```python
# settings.py
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
    'mysite.middleware.RequestTimingMiddleware',  # your custom middleware
]
```

---

## 20. Signals — Event Listeners

### 20.1 What are Signals?

Signals allow certain senders to notify a set of receivers when specific actions occur. They're Django's implementation of the Observer pattern.

Think of it like a doorbell. When someone presses the button (the signal is sent), the bell rings inside the house (the receiver is called). The person pressing the button doesn't need to know what happens inside — they just press.

Common signals: `pre_save`, `post_save`, `pre_delete`, `post_delete`, `m2m_changed`.

### 20.2 Using Signals

```python
# blog/signals.py
from django.db.models.signals import post_save, pre_save
from django.dispatch import receiver
from django.utils.text import slugify
from .models import Post

@receiver(pre_save, sender=Post)
def auto_generate_slug(sender, instance, **kwargs):
    """Automatically generate a slug from the title before saving."""
    if not instance.slug:
        instance.slug = slugify(instance.title)

@receiver(post_save, sender=Post)
def notify_subscribers(sender, instance, created, **kwargs):
    """Send notifications when a new post is published."""
    if created and instance.status == 'published':
        # Send email notifications to subscribers
        pass
```

```python
# blog/apps.py — IMPORTANT: You must import the signals here
class BlogConfig(AppConfig):
    default_auto_field = 'django.db.models.BigAutoField'
    name = 'blog'

    def ready(self):
        import blog.signals  # This connects the signal handlers
```

---

## 21. Sessions and Cookies

### 21.1 What are They?

HTTP is "stateless" — each request is independent, with no memory of previous requests. But websites need to remember things (who's logged in, what's in the shopping cart). Sessions and cookies solve this.

**Cookies** are small pieces of data stored in the user's browser. Django sends a cookie, and the browser sends it back with every subsequent request.

**Sessions** store data on the server, linked to a session cookie in the browser. The cookie only contains a session ID — the actual data is on the server where it's safe.

### 21.2 Using Sessions

```python
def add_to_cart(request, product_id):
    cart = request.session.get('cart', [])
    cart.append(product_id)
    request.session['cart'] = cart
    return redirect('shop:cart')

def view_cart(request):
    cart = request.session.get('cart', [])
    products = Product.objects.filter(id__in=cart)
    return render(request, 'shop/cart.html', {'products': products})

def clear_cart(request):
    if 'cart' in request.session:
        del request.session['cart']
    return redirect('shop:home')
```

---

## 22. Messages Framework

### 22.1 What is it?

The messages framework lets you store short messages for the user (success notices, error alerts, warnings) that are displayed on the next page they visit. This is commonly used after form submissions.

### 22.2 Using Messages

```python
from django.contrib import messages

def create_post(request):
    if request.method == 'POST':
        form = PostForm(request.POST)
        if form.is_valid():
            form.save()
            messages.success(request, 'Your post has been created!')
            return redirect('blog:post_list')
        else:
            messages.error(request, 'Please correct the errors below.')
    ...
```

```html
<!-- In base.html -->
{% if messages %}
    {% for message in messages %}
        <div class="alert alert-{{ message.tags }}">
            {{ message }}
        </div>
    {% endfor %}
{% endif %}
```

Message levels: `messages.debug()`, `messages.info()`, `messages.success()`, `messages.warning()`, `messages.error()`.

---

## 23. Pagination

### 23.1 What is Pagination?

When you have hundreds or thousands of records, showing them all on one page would be slow and overwhelming. Pagination splits them into manageable pages.

### 23.2 Pagination in Function-Based Views

```python
from django.core.paginator import Paginator

def post_list(request):
    all_posts = Post.objects.filter(status='published')
    paginator = Paginator(all_posts, 10)  # 10 posts per page

    page_number = request.GET.get('page')  # get ?page=N from the URL
    page_obj = paginator.get_page(page_number)

    return render(request, 'blog/post_list.html', {'page_obj': page_obj})
```

### 23.3 Pagination Template

```html
{% for post in page_obj %}
    <h2>{{ post.title }}</h2>
{% endfor %}

<nav>
    {% if page_obj.has_previous %}
        <a href="?page=1">First</a>
        <a href="?page={{ page_obj.previous_page_number }}">Previous</a>
    {% endif %}

    Page {{ page_obj.number }} of {{ page_obj.paginator.num_pages }}

    {% if page_obj.has_next %}
        <a href="?page={{ page_obj.next_page_number }}">Next</a>
        <a href="?page={{ page_obj.paginator.num_pages }}">Last</a>
    {% endif %}
</nav>
```

In Class-Based Views, just add `paginate_by = 10` to your `ListView` and the `page_obj` is automatically available in the template.

---

## 24. Email Sending

### 24.1 Configuration

```python
# settings.py

# For development (prints emails to the console)
EMAIL_BACKEND = 'django.core.mail.backends.console.EmailBackend'

# For production (using SMTP)
EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
EMAIL_HOST = 'smtp.gmail.com'
EMAIL_PORT = 587
EMAIL_USE_TLS = True
EMAIL_HOST_USER = 'your-email@gmail.com'
EMAIL_HOST_PASSWORD = 'your-app-password'
DEFAULT_FROM_EMAIL = 'noreply@yoursite.com'
```

### 24.2 Sending Emails

```python
from django.core.mail import send_mail, send_mass_mail

# Simple email
send_mail(
    subject='Welcome to Our Site',
    message='Thank you for registering!',
    from_email='noreply@yoursite.com',
    recipient_list=['user@example.com'],
    fail_silently=False,  # raise exception on error
)

# HTML email
from django.core.mail import EmailMultiAlternatives

email = EmailMultiAlternatives(
    subject='Welcome!',
    body='Thank you for registering.',  # plain text fallback
    from_email='noreply@yoursite.com',
    to=['user@example.com'],
)
email.attach_alternative('<h1>Welcome!</h1><p>Thank you.</p>', 'text/html')
email.send()
```

---

## 25. File Uploads

### 25.1 Handling File Uploads in Views

```python
def upload_document(request):
    if request.method == 'POST' and request.FILES.get('document'):
        uploaded_file = request.FILES['document']

        # Validate file size (e.g., max 5MB)
        if uploaded_file.size > 5 * 1024 * 1024:
            messages.error(request, 'File too large. Max size is 5MB.')
            return redirect('upload')

        # Validate file type
        allowed_types = ['application/pdf', 'image/jpeg', 'image/png']
        if uploaded_file.content_type not in allowed_types:
            messages.error(request, 'Invalid file type.')
            return redirect('upload')

        # Save using a model
        doc = Document(file=uploaded_file, uploaded_by=request.user)
        doc.save()

        messages.success(request, 'File uploaded successfully!')
        return redirect('document_detail', pk=doc.pk)

    return render(request, 'upload.html')
```

Remember: forms with file uploads must have `enctype="multipart/form-data"`, and the view must access `request.FILES` in addition to `request.POST`.

---

## 26. Custom Template Tags and Filters

### 26.1 When to Use Them

When Django's built-in tags and filters aren't enough, you can create your own. Common uses include formatting data, performing calculations, or adding reusable template components.

### 26.2 Setting Up

Create this directory structure:

```
blog/
├── templatetags/       ← create this directory
│   ├── __init__.py     ← required (empty file)
│   └── blog_tags.py    ← your custom tags and filters
```

### 26.3 Custom Filters

```python
# blog/templatetags/blog_tags.py
from django import template

register = template.Library()

@register.filter(name='reading_time')
def reading_time(text):
    """Estimates reading time in minutes."""
    word_count = len(text.split())
    minutes = max(1, word_count // 200)
    return f"{minutes} min read"

@register.filter
def truncate_smart(text, length=100):
    """Truncates text at a word boundary."""
    if len(text) <= length:
        return text
    truncated = text[:length].rsplit(' ', 1)[0]
    return truncated + '...'
```

```html
{% load blog_tags %}

<span>{{ post.content|reading_time }}</span>
<p>{{ post.content|truncate_smart:150 }}</p>
```

### 26.4 Custom Simple Tags

```python
@register.simple_tag
def total_posts():
    """Returns the total number of published posts."""
    return Post.objects.filter(status='published').count()
```

```html
{% load blog_tags %}
<p>We have {% total_posts %} published articles.</p>
```

---

## 27. Management Commands

### 27.1 What are They?

Management commands are custom scripts you run via `manage.py`. They're useful for database maintenance, data imports, sending scheduled emails, generating reports, or any recurring task.

### 27.2 Creating a Custom Command

```
blog/
├── management/
│   ├── __init__.py
│   └── commands/
│       ├── __init__.py
│       └── publish_scheduled.py
```

```python
# blog/management/commands/publish_scheduled.py
from django.core.management.base import BaseCommand
from django.utils import timezone
from blog.models import Post

class Command(BaseCommand):
    help = 'Publishes all posts whose scheduled date has passed.'

    def add_arguments(self, parser):
        parser.add_argument(
            '--dry-run',
            action='store_true',
            help='Show what would be published without actually publishing.',
        )

    def handle(self, *args, **options):
        now = timezone.now()
        scheduled = Post.objects.filter(
            status='scheduled',
            publish_date__lte=now,
        )

        if options['dry_run']:
            self.stdout.write(f'Would publish {scheduled.count()} posts.')
            for post in scheduled:
                self.stdout.write(f'  - {post.title}')
        else:
            count = scheduled.update(status='published')
            self.stdout.write(
                self.style.SUCCESS(f'Successfully published {count} posts.')
            )
```

```bash
python manage.py publish_scheduled
python manage.py publish_scheduled --dry-run
```

---

## 28. Caching — Making Things Faster

### 28.1 What is Caching?

Caching stores the results of expensive operations (database queries, API calls, complex calculations) so they can be reused without re-executing. Instead of querying the database 1000 times for the same data, you query once and cache the result.

### 28.2 Cache Backends

```python
# settings.py

# Development: stores cache in memory (lost when server restarts)
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.locmem.LocMemCache',
    }
}

# Production: Redis (fast, persistent, recommended)
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.redis.RedisCache',
        'LOCATION': 'redis://127.0.0.1:6379',
    }
}

# Production: Database-backed cache
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.db.DatabaseCache',
        'LOCATION': 'cache_table',
    }
}
```

### 28.3 Using the Cache

```python
from django.core.cache import cache

# Store a value (timeout in seconds)
cache.set('latest_posts', posts_list, timeout=60 * 15)  # 15 minutes

# Retrieve a value (returns None if not found or expired)
posts = cache.get('latest_posts')

# Retrieve with a default
posts = cache.get('latest_posts', [])

# Delete a cached value
cache.delete('latest_posts')

# Check if a key exists
cache.has_key('latest_posts')

# get_or_set — get if it exists, otherwise compute and store it
posts = cache.get_or_set('latest_posts', Post.objects.all()[:10], 60 * 15)
```

**Cache entire views:**

```python
from django.views.decorators.cache import cache_page

@cache_page(60 * 15)  # cache for 15 minutes
def post_list(request):
    ...
```

**Cache template fragments:**

```html
{% load cache %}
{% cache 300 sidebar %}
    <!-- This section is cached for 300 seconds (5 minutes) -->
    <aside>
        <h3>Popular Posts</h3>
        {% for post in popular_posts %}
            <p>{{ post.title }}</p>
        {% endfor %}
    </aside>
{% endcache %}
```

---

## 29. Testing — Making Sure Things Work

### 29.1 Why Test?

Tests verify that your code works correctly. Without tests, you can't confidently change or add code — any modification might break existing features. With tests, you run them after every change to ensure nothing is broken.

### 29.2 Types of Tests

- **Unit tests** test individual functions or methods in isolation.
- **Integration tests** test how multiple components work together.
- **Functional tests** test the application from the user's perspective (simulating browser actions).

### 29.3 Writing Tests

```python
# blog/tests.py
from django.test import TestCase, Client
from django.urls import reverse
from django.contrib.auth.models import User
from .models import Post

class PostModelTest(TestCase):
    """Tests for the Post model."""

    @classmethod
    def setUpTestData(cls):
        """Set up data for the whole TestCase (runs once)."""
        cls.user = User.objects.create_user(username='testuser', password='pass123')
        cls.post = Post.objects.create(
            title='Test Post',
            slug='test-post',
            author=cls.user,
            content='Some test content.',
            status='published',
        )

    def test_str_representation(self):
        """Test that __str__ returns the title."""
        self.assertEqual(str(self.post), 'Test Post')

    def test_get_absolute_url(self):
        """Test that get_absolute_url returns the correct URL."""
        expected_url = reverse('blog:post_detail', kwargs={'pk': self.post.pk})
        self.assertEqual(self.post.get_absolute_url(), expected_url)

    def test_default_status_is_draft(self):
        """Test that new posts default to 'draft' status."""
        post = Post.objects.create(
            title='Draft Post', slug='draft-post',
            author=self.user, content='Content',
        )
        self.assertEqual(post.status, 'draft')


class PostViewTest(TestCase):
    """Tests for Post views."""

    @classmethod
    def setUpTestData(cls):
        cls.user = User.objects.create_user(username='testuser', password='pass123')
        cls.post = Post.objects.create(
            title='Published Post', slug='published-post',
            author=cls.user, content='Content', status='published',
        )

    def test_post_list_status_code(self):
        """Test that the post list page returns 200."""
        response = self.client.get(reverse('blog:post_list'))
        self.assertEqual(response.status_code, 200)

    def test_post_list_uses_correct_template(self):
        response = self.client.get(reverse('blog:post_list'))
        self.assertTemplateUsed(response, 'blog/post_list.html')

    def test_post_list_contains_post(self):
        response = self.client.get(reverse('blog:post_list'))
        self.assertContains(response, 'Published Post')

    def test_post_create_requires_login(self):
        """Test that creating a post redirects to login if not authenticated."""
        response = self.client.get(reverse('blog:post_create'))
        self.assertEqual(response.status_code, 302)

    def test_post_create_with_login(self):
        """Test that a logged-in user can access the create form."""
        self.client.login(username='testuser', password='pass123')
        response = self.client.get(reverse('blog:post_create'))
        self.assertEqual(response.status_code, 200)
```

### 29.4 Running Tests

```bash
python manage.py test                      # run all tests
python manage.py test blog                  # run tests in the blog app
python manage.py test blog.tests.PostModelTest  # run specific test class
python manage.py test --verbosity=2         # show detailed output
```

---

## 30. REST APIs with Django REST Framework

### 30.1 What is an API?

An API (Application Programming Interface) is a way for programs to communicate with each other. A REST API uses HTTP requests to perform CRUD operations and returns data in JSON format instead of HTML.

APIs are used when your frontend is a JavaScript app (React, Vue, mobile app) that needs to get data from the backend, or when you want other services to interact with your application.

### 30.2 Setup

```bash
pip install djangorestframework
```

```python
# settings.py
INSTALLED_APPS = [
    ...
    'rest_framework',
]
```

### 30.3 Serializers

Serializers convert complex data (model instances, QuerySets) into JSON, and also validate incoming JSON data to create/update model instances. They're the API equivalent of Django forms.

```python
# blog/serializers.py
from rest_framework import serializers
from .models import Post, Category

class CategorySerializer(serializers.ModelSerializer):
    class Meta:
        model = Category
        fields = ['id', 'name', 'slug']

class PostSerializer(serializers.ModelSerializer):
    author = serializers.StringRelatedField(read_only=True)
    category = CategorySerializer(read_only=True)

    class Meta:
        model = Post
        fields = ['id', 'title', 'slug', 'author', 'category',
                  'content', 'status', 'created_at']
        read_only_fields = ['author']
```

### 30.4 ViewSets

ViewSets combine the logic for listing, creating, retrieving, updating, and deleting into a single class.

```python
# blog/api_views.py
from rest_framework import viewsets, permissions
from .models import Post
from .serializers import PostSerializer

class PostViewSet(viewsets.ModelViewSet):
    queryset = Post.objects.filter(status='published')
    serializer_class = PostSerializer
    permission_classes = [permissions.IsAuthenticatedOrReadOnly]

    def perform_create(self, serializer):
        serializer.save(author=self.request.user)
```

### 30.5 Router and URLs

```python
# blog/urls.py
from rest_framework.routers import DefaultRouter
from .api_views import PostViewSet

router = DefaultRouter()
router.register(r'posts', PostViewSet)

urlpatterns = [
    ...
    path('api/', include(router.urls)),
]
```

This automatically creates these endpoints:
- `GET /api/posts/` — List all posts
- `POST /api/posts/` — Create a new post
- `GET /api/posts/1/` — Get post #1
- `PUT /api/posts/1/` — Update post #1
- `PATCH /api/posts/1/` — Partially update post #1
- `DELETE /api/posts/1/` — Delete post #1

---

## 31. Security Best Practices

```python
# settings.py — Production security

DEBUG = False
ALLOWED_HOSTS = ['yourdomain.com']

# Force HTTPS
SECURE_SSL_REDIRECT = True
SESSION_COOKIE_SECURE = True
CSRF_COOKIE_SECURE = True

# HTTP Strict Transport Security
SECURE_HSTS_SECONDS = 31536000
SECURE_HSTS_INCLUDE_SUBDOMAINS = True
SECURE_HSTS_PRELOAD = True

# Prevent content type sniffing
SECURE_CONTENT_TYPE_NOSNIFF = True

# Prevent your site from being embedded in iframes
X_FRAME_OPTIONS = 'DENY'

# Use environment variables for secrets
import os
SECRET_KEY = os.environ['DJANGO_SECRET_KEY']
```

**Security checklist:**
- Never set `DEBUG = True` in production (it exposes sensitive information)
- Store `SECRET_KEY` in environment variables, never in code
- Always use `{% csrf_token %}` in POST forms
- Use `get_object_or_404()` to avoid exposing database details
- Validate and sanitize all user input
- Use parameterized queries (the ORM does this automatically)
- Keep Django and all packages updated
- Run `python manage.py check --deploy` before deploying

---

## 32. Deployment — Going Live

### 32.1 Preparation

```bash
pip freeze > requirements.txt       # save all dependencies
python manage.py check --deploy     # check for security issues
python manage.py collectstatic      # gather static files
python manage.py migrate            # ensure database is up to date
```

### 32.2 Gunicorn (WSGI Server)

The development server (`runserver`) is not suitable for production. Gunicorn is a production-grade WSGI server.

```bash
pip install gunicorn
gunicorn mysite.wsgi:application --bind 0.0.0.0:8000 --workers 3
```

### 32.3 Nginx (Reverse Proxy and Static File Server)

Nginx sits in front of Gunicorn. It serves static and media files directly (which is much faster) and forwards other requests to Gunicorn.

```nginx
server {
    listen 80;
    server_name yourdomain.com;

    location /static/ {
        alias /path/to/staticfiles/;
    }

    location /media/ {
        alias /path/to/media/;
    }

    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### 32.4 Docker

Docker packages your application and all its dependencies into a container that runs the same everywhere.

```dockerfile
FROM python:3.12-slim
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
RUN python manage.py collectstatic --noinput
EXPOSE 8000
CMD ["gunicorn", "mysite.wsgi:application", "--bind", "0.0.0.0:8000", "--workers", "3"]
```

```yaml
# docker-compose.yml
version: '3.8'
services:
  web:
    build: .
    ports:
      - "8000:8000"
    environment:
      - DJANGO_SECRET_KEY=your-secret-key
      - DB_HOST=db
      - DB_NAME=mydb
      - DB_USER=myuser
      - DB_PASSWORD=mypassword
    depends_on:
      - db
  db:
    image: postgres:16
    volumes:
      - postgres_data:/var/lib/postgresql/data
    environment:
      - POSTGRES_DB=mydb
      - POSTGRES_USER=myuser
      - POSTGRES_PASSWORD=mypassword
volumes:
  postgres_data:
```

### 32.5 Environment Variables Pattern

Never hardcode secrets. Use environment variables:

```python
# settings.py
import os

SECRET_KEY = os.environ.get('DJANGO_SECRET_KEY', 'fallback-for-dev-only')
DEBUG = os.environ.get('DJANGO_DEBUG', 'False') == 'True'
ALLOWED_HOSTS = os.environ.get('ALLOWED_HOSTS', 'localhost').split(',')
```

Or use the `python-decouple` package:

```python
from decouple import config

SECRET_KEY = config('DJANGO_SECRET_KEY')
DEBUG = config('DJANGO_DEBUG', default=False, cast=bool)
```

---

## 33. Quick Reference and Cheat Sheet

### Django Commands

```bash
django-admin startproject <name>       # create a new project
python manage.py startapp <name>       # create a new app
python manage.py runserver             # start development server
python manage.py makemigrations        # create migration files
python manage.py migrate               # apply migrations
python manage.py createsuperuser       # create admin user
python manage.py collectstatic         # collect static files
python manage.py shell                 # interactive Python shell with Django
python manage.py dbshell               # database shell
python manage.py test                  # run tests
python manage.py check --deploy        # security audit
python manage.py showmigrations        # list all migrations
python manage.py dumpdata blog         # export app data as JSON
python manage.py loaddata data.json    # import data from JSON
python manage.py flush                 # delete all data (keep tables)
```

### Common Import Paths

```python
# Views
from django.shortcuts import render, redirect, get_object_or_404
from django.http import HttpResponse, JsonResponse, Http404
from django.views.generic import ListView, DetailView, CreateView, UpdateView, DeleteView

# URLs
from django.urls import path, include, reverse, reverse_lazy

# Models
from django.db import models
from django.db.models import Q, F, Count, Avg, Sum, Max, Min

# Forms
from django import forms

# Auth
from django.contrib.auth.decorators import login_required
from django.contrib.auth.mixins import LoginRequiredMixin, UserPassesTestMixin
from django.contrib.auth import login, logout, authenticate

# Other
from django.contrib import messages
from django.core.paginator import Paginator
from django.core.cache import cache
from django.core.mail import send_mail
from django.conf import settings
from django.utils import timezone
```

### ORM Cheat Sheet

```python
Model.objects.all()                           # get all
Model.objects.get(pk=1)                       # get one (or error)
Model.objects.filter(field=value)             # filter
Model.objects.exclude(field=value)            # exclude
Model.objects.order_by('-field')              # sort (- = descending)
Model.objects.first() / .last()              # first/last record
Model.objects.count()                         # count records
Model.objects.exists()                        # any records?
Model.objects.values('field1', 'field2')     # dicts
Model.objects.values_list('field', flat=True) # flat list
Model.objects.distinct()                      # unique records
Model.objects.select_related('fk')           # join FK
Model.objects.prefetch_related('m2m')        # prefetch M2M
Model.objects.create(field=value)            # create
Model.objects.filter(...).update(field=val)  # bulk update
Model.objects.filter(...).delete()           # bulk delete
Model.objects.aggregate(total=Count('id'))   # aggregate
Model.objects.annotate(n=Count('related'))   # annotate
```

---

*This guide covers every major Django concept with clear explanations. For the most up-to-date information, always refer to the official Django documentation at docs.djangoproject.com.*
