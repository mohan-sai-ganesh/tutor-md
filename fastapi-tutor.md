# The Complete FastAPI Tutorial
### Every Concept Explained Clearly — From Zero to Production

---

## Table of Contents

1. What is FastAPI and Why Use It?
2. How APIs Work (Prerequisites)
3. Setting Up Your Environment
4. Your First FastAPI Application
5. Understanding the Project Structure
6. How FastAPI Handles a Request (The Request-Response Cycle)
7. Path Operations (Routes)
8. Path Parameters
9. Query Parameters
10. Request Body and Pydantic Models
11. Response Models and Status Codes
12. Form Data and File Uploads
13. Dependency Injection
14. Database Integration with SQLAlchemy
15. Database Migrations with Alembic
16. Authentication and Authorization
17. Middleware
18. CORS (Cross-Origin Resource Sharing)
19. Background Tasks
20. WebSockets
21. Error Handling and Custom Exceptions
22. Templates and Static Files (Server-Side Rendering)
23. Testing
24. Application Structure and Routers
25. Settings and Configuration
26. Caching
27. Rate Limiting
28. Logging
29. Async and Await — Understanding Asynchronous Programming
30. Events — Startup and Shutdown
31. Security Best Practices
32. Deployment — Going Live
33. Quick Reference and Cheat Sheet

---

## 1. What is FastAPI and Why Use It?

### What is FastAPI?

FastAPI is a modern, high-performance Python web framework for building APIs. It was created by Sebastián Ramírez in 2018 and has quickly become one of the most popular Python frameworks.

The name tells the story: it's **fast** to run (one of the fastest Python frameworks, on par with Node.js and Go), and **fast** to develop with (it minimizes the code you need to write).

### What is an API?

Before understanding FastAPI, you need to understand what an API is.

An API (Application Programming Interface) is a way for software programs to talk to each other. Think of a restaurant: you (the client) don't go into the kitchen to cook your food. Instead, you tell the waiter (the API) what you want, the waiter tells the kitchen (the server), and the waiter brings back your food (the response).

A **web API** specifically uses HTTP (the same protocol your browser uses) to communicate. Instead of returning HTML pages like a traditional website, an API typically returns data in **JSON format** — a structured text format that programs can easily read.

```json
{
    "id": 1,
    "title": "My First Post",
    "author": "Alice",
    "published": true
}
```

### Why FastAPI Over Other Frameworks?

**Speed:** FastAPI is built on top of Starlette (for the web parts) and Pydantic (for the data parts). It's one of the fastest Python frameworks available, comparable to Node.js and Go in benchmarks.

**Automatic Documentation:** FastAPI automatically generates interactive API documentation. You get two documentation interfaces for free — Swagger UI at `/docs` and ReDoc at `/redoc`. You don't write any documentation code; FastAPI generates it from your code.

**Type Hints:** FastAPI is built around Python's type hint system. You declare what types your data should be, and FastAPI automatically validates input, converts data types, and generates documentation from those hints.

**Data Validation:** Using Pydantic, FastAPI validates all incoming data automatically. If someone sends invalid data (like a string where a number is expected), FastAPI returns a clear error message without you writing any validation code.

**Async Support:** FastAPI natively supports asynchronous programming with `async` and `await`. This means it can handle many concurrent connections efficiently — critical for modern APIs.

**Standards-Based:** FastAPI is built on open standards — OpenAPI (formerly Swagger) for API creation and JSON Schema for data validation. This means your API works with a vast ecosystem of tools.

### When to Use FastAPI vs Django

**Use FastAPI when:**
- You're building an API (REST or GraphQL)
- You need high performance and async support
- Your frontend is a separate app (React, Vue, mobile app)
- You want automatic data validation and documentation
- You prefer a lightweight, flexible framework

**Use Django when:**
- You're building a full website with server-rendered HTML
- You need a built-in admin panel
- You want an all-in-one solution (ORM, auth, admin, forms, templates)
- You're building a content management system

You can also use both together — Django for the admin and traditional pages, FastAPI for the API layer.

---

## 2. How APIs Work (Prerequisites)

### HTTP Methods (Verbs)

APIs use HTTP methods to specify what action to perform:

- **GET** — Retrieve data. "Give me the list of users" or "Give me user #5."
- **POST** — Create new data. "Here's a new user to add."
- **PUT** — Replace existing data completely. "Replace user #5 with this new data."
- **PATCH** — Update part of existing data. "Change user #5's email address."
- **DELETE** — Remove data. "Delete user #5."

### HTTP Status Codes

Responses include a status code that tells you what happened:

- **200 OK** — Success. Here's the data you asked for.
- **201 Created** — Success. A new resource was created.
- **204 No Content** — Success. Nothing to return (often used for DELETE).
- **400 Bad Request** — You sent invalid data.
- **401 Unauthorized** — You need to log in.
- **403 Forbidden** — You're logged in but not allowed to do this.
- **404 Not Found** — The resource doesn't exist.
- **422 Unprocessable Entity** — The data format is correct, but the values are invalid.
- **500 Internal Server Error** — Something went wrong on the server.

### JSON (JavaScript Object Notation)

JSON is the standard format for API data exchange. It looks like Python dictionaries and lists:

```json
{
    "name": "Alice",
    "age": 30,
    "is_active": true,
    "hobbies": ["reading", "coding"],
    "address": {
        "city": "New York",
        "country": "USA"
    }
}
```

### REST (Representational State Transfer)

REST is a set of conventions for designing APIs. A RESTful API organizes data into **resources** (like users, posts, comments) and uses HTTP methods to perform actions on them:

```
GET    /users/        → List all users
POST   /users/        → Create a new user
GET    /users/5/      → Get user #5
PUT    /users/5/      → Replace user #5
PATCH  /users/5/      → Update part of user #5
DELETE /users/5/      → Delete user #5
```

---

## 3. Setting Up Your Environment

### 3.1 Install Python

FastAPI requires Python 3.7 or later (3.10+ recommended for the best type hint syntax).

```bash
python --version
# Should show 3.10 or later
```

### 3.2 Create a Virtual Environment

Just like with any Python project, always use a virtual environment:

```bash
# Create a virtual environment
python -m venv venv

# Activate it
# Mac/Linux:
source venv/bin/activate
# Windows:
venv\Scripts\activate
```

### 3.3 Install FastAPI and Uvicorn

FastAPI is the framework. Uvicorn is the ASGI server that actually runs your application — it's the engine that listens for HTTP requests and passes them to FastAPI.

```bash
pip install fastapi
pip install "uvicorn[standard]"

# Or install both together:
pip install "fastapi[standard]"
```

**What is ASGI?** ASGI (Asynchronous Server Gateway Interface) is the protocol that connects your Python application to the web server. It's the modern, async successor to WSGI. Uvicorn is an ASGI server, similar to how Gunicorn is a WSGI server.

### 3.4 Recommended Additional Packages

```bash
pip install sqlalchemy          # Database ORM
pip install alembic             # Database migrations
pip install python-multipart    # Form data and file uploads
pip install python-jose[cryptography]  # JWT tokens for auth
pip install passlib[bcrypt]     # Password hashing
pip install python-dotenv       # Environment variables
pip install httpx               # Async HTTP client (for testing)
pip install pytest              # Testing framework
```

---

## 4. Your First FastAPI Application

### 4.1 Hello World

Create a file called `main.py`:

```python
from fastapi import FastAPI

# Create the FastAPI application instance
app = FastAPI()

# Define a route (path operation)
@app.get("/")
def read_root():
    return {"message": "Hello, World!"}
```

Let's break this down line by line:

- `from fastapi import FastAPI` — Import the FastAPI class.
- `app = FastAPI()` — Create an instance of the FastAPI application. This `app` object is the main point of interaction for creating your API. All your routes, middleware, and configuration attach to this object.
- `@app.get("/")` — This is a **decorator** that tells FastAPI: "When someone sends a GET request to the root URL `/`, call the function below."
- `def read_root()` — A regular Python function. FastAPI calls it when the route is matched.
- `return {"message": "Hello, World!"}` — Return a Python dictionary. FastAPI automatically converts it to JSON.

### 4.2 Run the Application

```bash
uvicorn main:app --reload
```

Let's break down this command:
- `main` — The file name (`main.py`, without the `.py`)
- `app` — The name of the FastAPI instance in that file
- `--reload` — Automatically restart the server when code changes (development only)

Visit `http://127.0.0.1:8000` in your browser. You'll see:

```json
{"message": "Hello, World!"}
```

### 4.3 Automatic Documentation

Now visit these URLs:
- `http://127.0.0.1:8000/docs` — Swagger UI (interactive documentation)
- `http://127.0.0.1:8000/redoc` — ReDoc (alternative documentation)

These are generated automatically from your code. You can even test your API directly from the Swagger UI — send requests and see responses without any external tools.

### 4.4 Adding More Routes

```python
from fastapi import FastAPI

app = FastAPI(
    title="My Blog API",
    description="A simple blog API built with FastAPI",
    version="1.0.0",
)

@app.get("/")
def read_root():
    return {"message": "Welcome to the Blog API"}

@app.get("/posts")
def get_posts():
    return [
        {"id": 1, "title": "First Post", "published": True},
        {"id": 2, "title": "Second Post", "published": False},
    ]

@app.get("/about")
def about():
    return {"app": "Blog API", "version": "1.0.0"}
```

---

## 5. Understanding the Project Structure

Unlike Django, FastAPI doesn't enforce a specific project structure. You have complete freedom to organize your code. However, there are recommended conventions.

### 5.1 Simple Project (Small API)

```
my_api/
├── main.py              ← Application entry point
├── requirements.txt     ← Dependencies
└── .env                 ← Environment variables
```

### 5.2 Medium Project

```
my_api/
├── app/
│   ├── __init__.py
│   ├── main.py          ← FastAPI app creation and configuration
│   ├── config.py        ← Settings and configuration
│   ├── database.py      ← Database connection and session
│   ├── models.py        ← SQLAlchemy models (database tables)
│   ├── schemas.py       ← Pydantic models (request/response validation)
│   ├── crud.py          ← Database operations (Create, Read, Update, Delete)
│   ├── dependencies.py  ← Shared dependencies
│   └── routers/
│       ├── __init__.py
│       ├── posts.py     ← Post-related endpoints
│       ├── users.py     ← User-related endpoints
│       └── auth.py      ← Authentication endpoints
├── alembic/             ← Database migration files
├── tests/
│   ├── __init__.py
│   ├── conftest.py      ← Test configuration and fixtures
│   ├── test_posts.py
│   └── test_users.py
├── alembic.ini          ← Alembic configuration
├── requirements.txt
└── .env
```

### 5.3 Large Project

```
my_api/
├── app/
│   ├── __init__.py
│   ├── main.py
│   ├── config.py
│   ├── database.py
│   ├── dependencies.py
│   ├── posts/
│   │   ├── __init__.py
│   │   ├── router.py     ← Endpoints
│   │   ├── models.py     ← Database models
│   │   ├── schemas.py    ← Pydantic schemas
│   │   ├── service.py    ← Business logic
│   │   └── crud.py       ← Database operations
│   ├── users/
│   │   ├── __init__.py
│   │   ├── router.py
│   │   ├── models.py
│   │   ├── schemas.py
│   │   ├── service.py
│   │   └── crud.py
│   └── auth/
│       ├── __init__.py
│       ├── router.py
│       ├── schemas.py
│       ├── service.py
│       └── utils.py
├── alembic/
├── tests/
├── alembic.ini
├── requirements.txt
├── Dockerfile
├── docker-compose.yml
└── .env
```

### 5.4 Key Files Explained

**main.py** — Creates the FastAPI application, includes routers, sets up middleware, and configures startup/shutdown events. This is the entry point.

**models.py** — SQLAlchemy models that define your database tables. These map Python classes to database tables.

**schemas.py** — Pydantic models that define the shape of request and response data. These handle validation and serialization. (In FastAPI, "schemas" usually means Pydantic models, not database schemas.)

**crud.py** — Functions that perform database operations. Keeping these separate from routes makes your code reusable and testable.

**database.py** — Database connection setup — the engine, session factory, and base model class.

**routers/** — Groups of related endpoints. Each router handles one resource (posts, users, etc.).

**config.py** — Application settings loaded from environment variables.

---

## 6. How FastAPI Handles a Request (The Request-Response Cycle)

When someone sends a request to your FastAPI application, here's what happens:

**Step 1 — Uvicorn Receives the Request:** The ASGI server (Uvicorn) receives the raw HTTP request from the client.

**Step 2 — Middleware Processing:** The request passes through any middleware you've configured (CORS, authentication, logging, etc.). Each middleware can modify the request, reject it, or pass it along.

**Step 3 — Route Matching:** FastAPI checks the request's URL path and HTTP method against your defined routes. It finds the matching path operation function.

**Step 4 — Dependency Resolution:** If the route has dependencies (database sessions, current user, etc.), FastAPI resolves them first. Dependencies can have their own dependencies, forming a tree.

**Step 5 — Request Validation:** FastAPI uses Pydantic to validate all incoming data — path parameters, query parameters, request body, headers, and cookies. If validation fails, it immediately returns a 422 error with details about what's wrong.

**Step 6 — Path Operation Function:** Your function runs. It receives the validated, type-converted data and performs business logic (querying the database, processing data, etc.).

**Step 7 — Response Serialization:** The return value is converted to JSON (if it's a dict, list, or Pydantic model). If you specified a `response_model`, the output is filtered and validated against it.

**Step 8 — Response Middleware:** The response passes back through middleware in reverse order.

**Step 9 — Client Receives Response:** The JSON response with the appropriate status code is sent back to the client.

---

## 7. Path Operations (Routes)

### 7.1 What is a Path Operation?

In FastAPI terminology, a "path operation" is the combination of an HTTP method and a URL path. The `@app.get("/users")` decorator creates a path operation that responds to GET requests at the `/users` path.

"Path" refers to the URL path (everything after the domain name). "Operation" refers to the HTTP method (GET, POST, PUT, DELETE, etc.).

### 7.2 HTTP Method Decorators

FastAPI provides a decorator for each HTTP method:

```python
@app.get("/items")          # Read / List
def get_items():
    return [{"id": 1, "name": "Item 1"}]

@app.post("/items")         # Create
def create_item():
    return {"id": 3, "name": "New Item"}

@app.put("/items/{id}")     # Full Update (replace)
def update_item(id: int):
    return {"id": id, "name": "Updated Item"}

@app.patch("/items/{id}")   # Partial Update
def patch_item(id: int):
    return {"id": id, "name": "Patched Item"}

@app.delete("/items/{id}")  # Delete
def delete_item(id: int):
    return {"message": "Item deleted"}
```

### 7.3 Path Operation Configuration

You can configure each path operation with metadata that appears in the documentation:

```python
@app.get(
    "/posts",
    summary="Get all blog posts",
    description="Retrieve a list of all published blog posts, sorted by date.",
    response_description="A list of posts",
    tags=["Posts"],            # Groups endpoints in the docs
    status_code=200,           # Default response status code
    deprecated=False,          # Mark as deprecated in docs
)
def get_posts():
    return []
```

**Tags** are especially useful — they group related endpoints together in the documentation, making it much easier to navigate.

### 7.4 Order Matters

FastAPI evaluates routes in the order they're defined. This matters when routes could overlap:

```python
@app.get("/users/me")        # This MUST come first
def get_current_user():
    return {"user": "current user"}

@app.get("/users/{user_id}")  # This would match "me" as a user_id if it came first
def get_user(user_id: int):
    return {"user_id": user_id}
```

If `/users/{user_id}` were defined first, a request to `/users/me` would try to convert "me" to an integer and fail. Always put fixed paths before parameterized paths.

---

## 8. Path Parameters

### 8.1 What are Path Parameters?

Path parameters are variable parts of the URL path. They let you capture values from the URL and use them in your function.

```python
@app.get("/users/{user_id}")
def get_user(user_id: int):
    return {"user_id": user_id}
```

When someone requests `/users/42`, FastAPI:
1. Matches the URL pattern
2. Extracts `42` from the path
3. Converts it to an integer (because of the `int` type hint)
4. Passes it to the function as `user_id=42`

If someone requests `/users/abc`, FastAPI automatically returns a 422 error because "abc" can't be converted to an integer. You get this validation for free — no code needed.

### 8.2 Type Hints for Validation

The type hint on the parameter determines the validation:

```python
@app.get("/items/{item_id}")
def get_item(item_id: int):       # Must be an integer
    return {"item_id": item_id}

@app.get("/files/{file_path:path}")
def get_file(file_path: str):     # Can contain slashes: /files/home/user/doc.txt
    return {"file_path": file_path}
```

### 8.3 Predefined Values with Enums

When a parameter should only accept specific values, use an Enum:

```python
from enum import Enum

class PostStatus(str, Enum):
    draft = "draft"
    published = "published"
    archived = "archived"

@app.get("/posts/status/{status}")
def get_posts_by_status(status: PostStatus):
    return {"status": status, "message": f"Fetching {status.value} posts"}

# Valid:   /posts/status/published
# Invalid: /posts/status/deleted → 422 error
```

By inheriting from both `str` and `Enum`, the values work as strings in JSON serialization and in the URL.

### 8.4 Multiple Path Parameters

```python
@app.get("/users/{user_id}/posts/{post_id}")
def get_user_post(user_id: int, post_id: int):
    return {"user_id": user_id, "post_id": post_id}

# /users/5/posts/42 → {"user_id": 5, "post_id": 42}
```

---

## 9. Query Parameters

### 9.1 What are Query Parameters?

Query parameters are key-value pairs that appear after the `?` in a URL:

```
/products?category=electronics&min_price=100&sort=price
```

In this URL, `category`, `min_price`, and `sort` are query parameters. They're commonly used for filtering, sorting, searching, and pagination.

### 9.2 Defining Query Parameters

Any function parameter that is NOT a path parameter is automatically treated as a query parameter:

```python
@app.get("/products")
def get_products(category: str, min_price: float = 0, max_price: float = 10000):
    return {
        "category": category,
        "min_price": min_price,
        "max_price": max_price,
    }

# /products?category=electronics → category is required, others use defaults
# /products?category=electronics&min_price=50&max_price=200
# /products → 422 error (category is required because it has no default)
```

### 9.3 Optional Parameters

Use `None` as a default to make a parameter optional:

```python
from typing import Optional

@app.get("/search")
def search(q: str, category: Optional[str] = None, page: int = 1, limit: int = 10):
    results = {"query": q, "page": page, "limit": limit}
    if category:
        results["category"] = category
    return results

# /search?q=laptop              → q="laptop", category=None, page=1, limit=10
# /search?q=laptop&category=electronics&page=2
# /search                       → 422 error (q is required)
```

### 9.4 Boolean Parameters

FastAPI handles boolean query parameters smartly:

```python
@app.get("/items")
def get_items(published: bool = True):
    return {"published": published}

# /items?published=true    → True
# /items?published=1       → True
# /items?published=yes     → True
# /items?published=on      → True
# /items?published=false   → False
# /items?published=0       → False
```

### 9.5 Advanced Query Parameters with Query()

For more control, use the `Query()` function:

```python
from fastapi import Query

@app.get("/items")
def get_items(
    q: str = Query(
        default=None,
        min_length=3,          # minimum 3 characters
        max_length=50,         # maximum 50 characters
        pattern="^[a-zA-Z]",   # must start with a letter (regex)
        title="Search Query",  # for documentation
        description="The search query string to filter items",
        example="laptop",      # example value in docs
    ),
    page: int = Query(default=1, ge=1),           # >= 1
    limit: int = Query(default=10, ge=1, le=100),  # between 1 and 100
):
    return {"q": q, "page": page, "limit": limit}
```

### 9.6 List Query Parameters

To accept multiple values for the same parameter (like `/items?tag=python&tag=fastapi`):

```python
from typing import List
from fastapi import Query

@app.get("/items")
def get_items(tags: List[str] = Query(default=[])):
    return {"tags": tags}

# /items?tags=python&tags=fastapi → {"tags": ["python", "fastapi"]}
```

---

## 10. Request Body and Pydantic Models

### 10.1 What is a Request Body?

A request body is data sent by the client to the API, typically in POST, PUT, and PATCH requests. When a user fills out a form or an app sends data to create/update a resource, that data goes in the request body.

The request body is usually sent as JSON:

```json
{
    "title": "My New Post",
    "content": "This is the post content.",
    "published": true
}
```

### 10.2 What is Pydantic?

Pydantic is a data validation library that FastAPI uses extensively. You define a model (a Python class) that describes what your data should look like, and Pydantic automatically:
- Validates that incoming data has the right types
- Converts data to the correct types when possible (e.g., string "42" to integer 42)
- Provides clear error messages when validation fails
- Generates JSON Schema for documentation

Pydantic is to FastAPI what forms are to Django — but much more powerful and automatic.

### 10.3 Defining Pydantic Models (Schemas)

```python
from pydantic import BaseModel, Field, EmailStr
from typing import Optional
from datetime import datetime

class PostCreate(BaseModel):
    """Schema for creating a new post. This defines what data the client must send."""

    title: str = Field(
        ...,                    # ... means required (no default)
        min_length=3,
        max_length=200,
        example="My First Post",
    )
    content: str = Field(
        ...,
        min_length=10,
        description="The full content of the blog post",
    )
    published: bool = Field(
        default=False,
        description="Whether the post is published immediately",
    )
    tags: list[str] = Field(
        default=[],
        max_length=10,          # maximum 10 tags
        example=["python", "fastapi"],
    )
    category_id: Optional[int] = Field(
        default=None,
        description="The ID of the category (optional)",
    )
```

### 10.4 Using Pydantic Models in Routes

```python
@app.post("/posts", status_code=201)
def create_post(post: PostCreate):
    """
    FastAPI sees that 'post' has a Pydantic model type hint.
    It automatically:
    1. Reads the JSON body from the request
    2. Validates it against the PostCreate model
    3. Returns a 422 error if validation fails
    4. Passes the validated data to your function
    """
    return {
        "message": "Post created",
        "data": {
            "title": post.title,
            "content": post.content,
            "published": post.published,
            "tags": post.tags,
        }
    }
```

If someone sends invalid data:

```json
// Request:
{
    "title": "Hi",
    "content": "Short"
}

// Response (422 Unprocessable Entity):
{
    "detail": [
        {
            "loc": ["body", "title"],
            "msg": "String should have at least 3 characters",
            "type": "string_too_short"
        },
        {
            "loc": ["body", "content"],
            "msg": "String should have at least 10 characters",
            "type": "string_too_short"
        }
    ]
}
```

### 10.5 Multiple Schemas for Different Operations

A common pattern is to create different schemas for different operations:

```python
from pydantic import BaseModel, ConfigDict
from typing import Optional
from datetime import datetime


class PostBase(BaseModel):
    """Shared fields between create and update."""
    title: str = Field(..., min_length=3, max_length=200)
    content: str = Field(..., min_length=10)
    published: bool = False
    tags: list[str] = []


class PostCreate(PostBase):
    """Schema for creating a post. Inherits all fields from PostBase."""
    pass


class PostUpdate(BaseModel):
    """Schema for updating a post. All fields are optional."""
    title: Optional[str] = Field(default=None, min_length=3, max_length=200)
    content: Optional[str] = Field(default=None, min_length=10)
    published: Optional[bool] = None
    tags: Optional[list[str]] = None


class PostResponse(PostBase):
    """Schema for the response. Includes fields set by the server."""
    id: int
    author_id: int
    created_at: datetime
    updated_at: datetime

    model_config = ConfigDict(from_attributes=True)
    # from_attributes=True tells Pydantic to read data from object attributes
    # (like SQLAlchemy model instances) not just dictionaries.


class PostListResponse(BaseModel):
    """Schema for paginated list responses."""
    posts: list[PostResponse]
    total: int
    page: int
    limit: int
```

### 10.6 Nested Models

Pydantic models can contain other Pydantic models:

```python
class Address(BaseModel):
    street: str
    city: str
    state: str
    zip_code: str

class UserCreate(BaseModel):
    name: str
    email: EmailStr
    address: Address        # Nested model
    tags: list[str] = []

# The client sends:
# {
#     "name": "Alice",
#     "email": "alice@example.com",
#     "address": {
#         "street": "123 Main St",
#         "city": "New York",
#         "state": "NY",
#         "zip_code": "10001"
#     }
# }
```

### 10.7 Custom Validators

```python
from pydantic import BaseModel, field_validator, model_validator

class UserCreate(BaseModel):
    username: str
    email: str
    password: str
    password_confirm: str

    @field_validator('username')
    @classmethod
    def username_must_be_alphanumeric(cls, v):
        if not v.isalnum():
            raise ValueError('Username must be alphanumeric')
        return v.lower()  # normalize to lowercase

    @field_validator('email')
    @classmethod
    def email_must_contain_at(cls, v):
        if '@' not in v:
            raise ValueError('Invalid email address')
        return v.lower()

    @model_validator(mode='after')
    def passwords_match(self):
        if self.password != self.password_confirm:
            raise ValueError('Passwords do not match')
        return self
```

---

## 11. Response Models and Status Codes

### 11.1 Why Use Response Models?

Response models control what data is sent back to the client. Even if your internal data has sensitive fields (like passwords), the response model filters them out.

```python
class UserResponse(BaseModel):
    id: int
    username: str
    email: str
    is_active: bool

    model_config = ConfigDict(from_attributes=True)

# The database model might have a 'hashed_password' field,
# but UserResponse doesn't include it, so it's never sent to the client.

@app.post("/users", response_model=UserResponse, status_code=201)
def create_user(user: UserCreate):
    # Even if the returned object has a password field,
    # FastAPI will only include fields defined in UserResponse
    new_user = save_to_database(user)
    return new_user
```

### 11.2 Status Codes

```python
from fastapi import status

@app.post("/items", status_code=status.HTTP_201_CREATED)
def create_item(item: ItemCreate):
    return {"id": 1, **item.model_dump()}

@app.delete("/items/{item_id}", status_code=status.HTTP_204_NO_CONTENT)
def delete_item(item_id: int):
    # 204 means success with no response body
    return None

# Common status codes:
# status.HTTP_200_OK
# status.HTTP_201_CREATED
# status.HTTP_204_NO_CONTENT
# status.HTTP_400_BAD_REQUEST
# status.HTTP_401_UNAUTHORIZED
# status.HTTP_403_FORBIDDEN
# status.HTTP_404_NOT_FOUND
# status.HTTP_422_UNPROCESSABLE_ENTITY
```

### 11.3 Multiple Response Types

```python
from fastapi.responses import JSONResponse, RedirectResponse, HTMLResponse

@app.get("/redirect")
def redirect():
    return RedirectResponse(url="/docs")

@app.get("/html", response_class=HTMLResponse)
def get_html():
    return "<h1>Hello HTML</h1>"

@app.get("/custom")
def custom_response():
    return JSONResponse(
        content={"message": "Custom response"},
        status_code=200,
        headers={"X-Custom-Header": "my-value"},
    )
```

---

## 12. Form Data and File Uploads

### 12.1 Receiving Form Data

HTML forms typically send data as form-encoded data, not JSON. To receive this in FastAPI, use `Form()`:

```bash
# Required package:
pip install python-multipart
```

```python
from fastapi import Form

@app.post("/login")
def login(username: str = Form(...), password: str = Form(...)):
    """
    This endpoint expects form-encoded data (like from an HTML form),
    NOT JSON. The Content-Type will be application/x-www-form-urlencoded.
    """
    return {"username": username}
```

### 12.2 File Uploads

```python
from fastapi import File, UploadFile

@app.post("/upload")
async def upload_file(file: UploadFile):
    """
    UploadFile gives you:
    - file.filename  → original filename
    - file.size      → file size in bytes
    - file.content_type → MIME type (e.g., "image/jpeg")
    - file.read()    → read the file contents (async)
    - file.seek(0)   → move back to the start of the file
    """
    contents = await file.read()

    # Save the file
    with open(f"uploads/{file.filename}", "wb") as f:
        f.write(contents)

    return {
        "filename": file.filename,
        "size": file.size,
        "content_type": file.content_type,
    }

# Multiple file uploads
@app.post("/upload-many")
async def upload_files(files: list[UploadFile]):
    filenames = [f.filename for f in files]
    return {"filenames": filenames}
```

### 12.3 File with Form Data

```python
@app.post("/profile")
async def update_profile(
    name: str = Form(...),
    bio: str = Form(default=""),
    avatar: UploadFile = File(default=None),
):
    result = {"name": name, "bio": bio}
    if avatar:
        result["avatar_filename"] = avatar.filename
    return result
```

---

## 13. Dependency Injection

### 13.1 What is Dependency Injection?

Dependency injection is one of FastAPI's most powerful features. It's a way to share code, resources, and logic across multiple routes without repeating yourself.

Think of it like this: many of your routes need a database connection. Instead of creating a new connection in every route, you define a "dependency" that provides the connection, and FastAPI injects it into any route that needs it.

Dependencies can:
- Provide shared resources (database sessions, configuration)
- Enforce rules (authentication, authorization, rate limiting)
- Extract and validate common parameters (pagination, filtering)
- Have their own dependencies (creating a dependency tree)

### 13.2 Basic Dependencies

```python
from fastapi import Depends

# A dependency is just a function (or class)
def get_common_params(page: int = 1, limit: int = 10, sort: str = "created_at"):
    """Common pagination and sorting parameters."""
    return {"page": page, "limit": limit, "sort": sort}


@app.get("/posts")
def get_posts(params: dict = Depends(get_common_params)):
    # FastAPI calls get_common_params, resolves its parameters from the
    # query string, and passes the result as 'params'
    return {"params": params}


@app.get("/users")
def get_users(params: dict = Depends(get_common_params)):
    # Same dependency reused — DRY!
    return {"params": params}

# Both routes accept ?page=2&limit=20&sort=name
```

### 13.3 Database Session Dependency

This is the most common real-world dependency:

```python
from sqlalchemy.orm import Session
from .database import SessionLocal

def get_db():
    """
    Creates a new database session for each request and closes it
    when the request is done. The 'yield' keyword makes this a
    generator — the code before yield runs first, and the code
    after yield runs as cleanup (like a finally block).
    """
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()


@app.get("/posts")
def get_posts(db: Session = Depends(get_db)):
    posts = db.query(Post).all()
    return posts
```

### 13.4 Authentication Dependency

```python
from fastapi import Depends, HTTPException, status
from fastapi.security import OAuth2PasswordBearer

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="auth/login")

def get_current_user(token: str = Depends(oauth2_scheme)):
    """
    This dependency:
    1. Extracts the Bearer token from the Authorization header
    2. Validates the token
    3. Returns the user
    If the token is invalid, it raises a 401 error.
    """
    user = verify_token_and_get_user(token)
    if not user:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Invalid authentication credentials",
            headers={"WWW-Authenticate": "Bearer"},
        )
    return user


def get_current_active_user(user: User = Depends(get_current_user)):
    """Dependency that builds on get_current_user — checks if the user is active."""
    if not user.is_active:
        raise HTTPException(status_code=400, detail="Inactive user")
    return user


@app.get("/users/me")
def read_current_user(user: User = Depends(get_current_active_user)):
    return user
```

### 13.5 Class-Based Dependencies

```python
class Pagination:
    def __init__(self, page: int = 1, limit: int = 10):
        self.page = page
        self.limit = limit
        self.offset = (page - 1) * limit

@app.get("/items")
def get_items(pagination: Pagination = Depends()):
    # When you use Depends() with a class (no argument), FastAPI
    # instantiates the class using query parameters
    return {
        "page": pagination.page,
        "limit": pagination.limit,
        "offset": pagination.offset,
    }
```

### 13.6 Global Dependencies

Apply dependencies to all routes:

```python
# Apply to all routes in the app
app = FastAPI(dependencies=[Depends(verify_api_key)])

# Apply to all routes in a router
router = APIRouter(dependencies=[Depends(get_current_user)])
```

---

## 14. Database Integration with SQLAlchemy

### 14.1 What is SQLAlchemy?

SQLAlchemy is Python's most popular ORM (Object-Relational Mapper). Like Django's ORM, it lets you interact with databases using Python classes instead of writing raw SQL. FastAPI doesn't include its own ORM — it's designed to work with any database library, but SQLAlchemy is the most common choice.

### 14.2 Setting Up the Database Connection

```python
# app/database.py
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker, DeclarativeBase

# Database URL format: dialect+driver://username:password@host:port/database

# SQLite (for development — stores data in a file)
SQLALCHEMY_DATABASE_URL = "sqlite:///./app.db"

# PostgreSQL (for production)
# SQLALCHEMY_DATABASE_URL = "postgresql://user:password@localhost:5432/mydb"

# Create the engine — this manages the connection pool to the database
engine = create_engine(
    SQLALCHEMY_DATABASE_URL,
    connect_args={"check_same_thread": False},  # needed for SQLite only
    echo=True,  # logs all SQL statements (set to False in production)
)

# Create a session factory — sessions are how you interact with the database
SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

# Base class for all models
class Base(DeclarativeBase):
    pass
```

### 14.3 Defining Models

```python
# app/models.py
from sqlalchemy import (
    Column, Integer, String, Text, Boolean, DateTime, Float,
    ForeignKey, Table, Enum as SQLEnum
)
from sqlalchemy.orm import relationship
from sqlalchemy.sql import func
from .database import Base
import enum


class PostStatus(str, enum.Enum):
    draft = "draft"
    published = "published"
    archived = "archived"


class User(Base):
    __tablename__ = "users"
    # __tablename__ tells SQLAlchemy what to name the database table

    id = Column(Integer, primary_key=True, index=True)
    # primary_key=True → this is the unique identifier
    # index=True → create a database index for faster lookups

    username = Column(String(50), unique=True, nullable=False, index=True)
    # unique=True → no two users can have the same username
    # nullable=False → this field cannot be NULL (it's required)

    email = Column(String(100), unique=True, nullable=False)
    hashed_password = Column(String(255), nullable=False)
    is_active = Column(Boolean, default=True)

    created_at = Column(DateTime(timezone=True), server_default=func.now())
    # server_default=func.now() → the database sets this to the current time

    updated_at = Column(
        DateTime(timezone=True),
        server_default=func.now(),
        onupdate=func.now(),
        # onupdate=func.now() → automatically updates when the row is modified
    )

    # Relationship: one user has many posts
    posts = relationship("Post", back_populates="author", cascade="all, delete-orphan")
    # back_populates connects this to the 'author' relationship on Post
    # cascade="all, delete-orphan" → when a user is deleted, delete their posts too


class Category(Base):
    __tablename__ = "categories"

    id = Column(Integer, primary_key=True, index=True)
    name = Column(String(100), unique=True, nullable=False)
    slug = Column(String(100), unique=True, nullable=False)
    description = Column(Text, default="")

    posts = relationship("Post", back_populates="category")


# Many-to-many relationship: posts <-> tags
# This creates a join table in the database
post_tags = Table(
    "post_tags",
    Base.metadata,
    Column("post_id", Integer, ForeignKey("posts.id"), primary_key=True),
    Column("tag_id", Integer, ForeignKey("tags.id"), primary_key=True),
)


class Post(Base):
    __tablename__ = "posts"

    id = Column(Integer, primary_key=True, index=True)
    title = Column(String(200), nullable=False)
    slug = Column(String(200), unique=True, nullable=False, index=True)
    content = Column(Text, nullable=False)
    excerpt = Column(Text, default="")
    status = Column(SQLEnum(PostStatus), default=PostStatus.draft)
    view_count = Column(Integer, default=0)

    # Foreign key: each post belongs to one author
    author_id = Column(Integer, ForeignKey("users.id"), nullable=False)
    category_id = Column(Integer, ForeignKey("categories.id"), nullable=True)

    created_at = Column(DateTime(timezone=True), server_default=func.now())
    updated_at = Column(DateTime(timezone=True), server_default=func.now(), onupdate=func.now())

    # Relationships
    author = relationship("User", back_populates="posts")
    category = relationship("Category", back_populates="posts")
    tags = relationship("Tag", secondary=post_tags, back_populates="posts")
    comments = relationship("Comment", back_populates="post", cascade="all, delete-orphan")


class Tag(Base):
    __tablename__ = "tags"

    id = Column(Integer, primary_key=True, index=True)
    name = Column(String(50), unique=True, nullable=False)
    slug = Column(String(50), unique=True, nullable=False)

    posts = relationship("Post", secondary=post_tags, back_populates="tags")


class Comment(Base):
    __tablename__ = "comments"

    id = Column(Integer, primary_key=True, index=True)
    body = Column(Text, nullable=False)
    is_approved = Column(Boolean, default=False)

    post_id = Column(Integer, ForeignKey("posts.id"), nullable=False)
    author_id = Column(Integer, ForeignKey("users.id"), nullable=False)

    created_at = Column(DateTime(timezone=True), server_default=func.now())

    post = relationship("Post", back_populates="comments")
    author = relationship("User")
```

### 14.4 Creating Tables

```python
# In main.py or a startup script
from .database import engine, Base
from . import models  # import models so SQLAlchemy knows about them

# Create all tables that don't exist yet
Base.metadata.create_all(bind=engine)
```

This is fine for development, but for production, use Alembic migrations (covered in the next chapter).

### 14.5 CRUD Operations

```python
# app/crud.py
from sqlalchemy.orm import Session
from sqlalchemy import func
from . import models, schemas


# === CREATE ===

def create_user(db: Session, user: schemas.UserCreate) -> models.User:
    hashed_pw = hash_password(user.password)
    db_user = models.User(
        username=user.username,
        email=user.email,
        hashed_password=hashed_pw,
    )
    db.add(db_user)        # add to the session (stage the insert)
    db.commit()            # execute the SQL INSERT
    db.refresh(db_user)    # reload from database to get auto-generated fields (id, created_at)
    return db_user


def create_post(db: Session, post: schemas.PostCreate, author_id: int) -> models.Post:
    db_post = models.Post(
        **post.model_dump(),   # unpack all fields from the Pydantic model
        author_id=author_id,
    )
    db.add(db_post)
    db.commit()
    db.refresh(db_post)
    return db_post


# === READ ===

def get_user(db: Session, user_id: int) -> models.User | None:
    return db.query(models.User).filter(models.User.id == user_id).first()
    # .first() returns the first result or None


def get_user_by_email(db: Session, email: str) -> models.User | None:
    return db.query(models.User).filter(models.User.email == email).first()


def get_posts(
    db: Session,
    skip: int = 0,
    limit: int = 10,
    status: str = None,
) -> list[models.Post]:
    query = db.query(models.Post)

    if status:
        query = query.filter(models.Post.status == status)

    return query.order_by(models.Post.created_at.desc()).offset(skip).limit(limit).all()
    # .offset(skip) → skip this many records (for pagination)
    # .limit(limit) → return at most this many records
    # .all() → execute the query and return all results as a list


def get_post_by_slug(db: Session, slug: str) -> models.Post | None:
    return db.query(models.Post).filter(models.Post.slug == slug).first()


def count_posts(db: Session, status: str = None) -> int:
    query = db.query(func.count(models.Post.id))
    if status:
        query = query.filter(models.Post.status == status)
    return query.scalar()
    # .scalar() returns a single value instead of a row


# === UPDATE ===

def update_post(db: Session, post_id: int, post_update: schemas.PostUpdate) -> models.Post | None:
    db_post = db.query(models.Post).filter(models.Post.id == post_id).first()
    if not db_post:
        return None

    # Only update fields that were provided (not None)
    update_data = post_update.model_dump(exclude_unset=True)
    for field, value in update_data.items():
        setattr(db_post, field, value)

    db.commit()
    db.refresh(db_post)
    return db_post


# === DELETE ===

def delete_post(db: Session, post_id: int) -> bool:
    db_post = db.query(models.Post).filter(models.Post.id == post_id).first()
    if not db_post:
        return False
    db.delete(db_post)
    db.commit()
    return True
```

### 14.6 Using CRUD in Routes

```python
# app/routers/posts.py
from fastapi import APIRouter, Depends, HTTPException, status
from sqlalchemy.orm import Session
from .. import crud, schemas
from ..dependencies import get_db, get_current_user

router = APIRouter(prefix="/posts", tags=["Posts"])


@router.get("/", response_model=schemas.PostListResponse)
def list_posts(
    page: int = 1,
    limit: int = 10,
    status_filter: str = None,
    db: Session = Depends(get_db),
):
    skip = (page - 1) * limit
    posts = crud.get_posts(db, skip=skip, limit=limit, status=status_filter)
    total = crud.count_posts(db, status=status_filter)
    return {"posts": posts, "total": total, "page": page, "limit": limit}


@router.get("/{slug}", response_model=schemas.PostResponse)
def get_post(slug: str, db: Session = Depends(get_db)):
    post = crud.get_post_by_slug(db, slug=slug)
    if not post:
        raise HTTPException(status_code=404, detail="Post not found")
    return post


@router.post("/", response_model=schemas.PostResponse, status_code=201)
def create_post(
    post: schemas.PostCreate,
    db: Session = Depends(get_db),
    current_user: models.User = Depends(get_current_user),
):
    return crud.create_post(db, post=post, author_id=current_user.id)


@router.patch("/{post_id}", response_model=schemas.PostResponse)
def update_post(
    post_id: int,
    post_update: schemas.PostUpdate,
    db: Session = Depends(get_db),
    current_user: models.User = Depends(get_current_user),
):
    post = crud.get_post(db, post_id)
    if not post:
        raise HTTPException(status_code=404, detail="Post not found")
    if post.author_id != current_user.id:
        raise HTTPException(status_code=403, detail="Not authorized")
    return crud.update_post(db, post_id, post_update)


@router.delete("/{post_id}", status_code=204)
def delete_post(
    post_id: int,
    db: Session = Depends(get_db),
    current_user: models.User = Depends(get_current_user),
):
    post = crud.get_post(db, post_id)
    if not post:
        raise HTTPException(status_code=404, detail="Post not found")
    if post.author_id != current_user.id:
        raise HTTPException(status_code=403, detail="Not authorized")
    crud.delete_post(db, post_id)
    return None
```

---

## 15. Database Migrations with Alembic

### 15.1 What is Alembic?

Alembic is a database migration tool for SQLAlchemy. It works like Django's migrations — it tracks changes to your models and generates scripts to update the database schema.

Without Alembic, you'd have to manually write SQL to add columns, change types, or create tables. Alembic automates this and keeps a history of all changes.

### 15.2 Setup

```bash
pip install alembic
alembic init alembic
```

This creates an `alembic/` directory and an `alembic.ini` configuration file.

### 15.3 Configuration

```python
# alembic/env.py — Edit this file
from app.database import Base
from app import models  # import your models so Alembic can detect them

# Set the target metadata
target_metadata = Base.metadata
```

```ini
# alembic.ini — Set the database URL
sqlalchemy.url = sqlite:///./app.db
```

### 15.4 Creating and Running Migrations

```bash
# Auto-generate a migration by comparing models to the database
alembic revision --autogenerate -m "Create initial tables"

# Apply all pending migrations
alembic upgrade head

# View migration history
alembic history

# Roll back one migration
alembic downgrade -1

# Roll back to the beginning
alembic downgrade base
```

### 15.5 Typical Workflow

1. Change your SQLAlchemy models (add a field, create a new table, etc.)
2. Run `alembic revision --autogenerate -m "Description of change"`
3. Review the generated migration file in `alembic/versions/`
4. Run `alembic upgrade head` to apply the changes

---

## 16. Authentication and Authorization

### 16.1 Overview

FastAPI doesn't include a built-in auth system like Django, but it provides excellent tools for building one. The most common approach for APIs is **JWT (JSON Web Tokens)**.

**How JWT works:**
1. User sends username and password to the login endpoint
2. Server verifies credentials and creates a signed token (JWT)
3. Server sends the token back to the client
4. Client includes the token in the `Authorization` header of every subsequent request
5. Server verifies the token and identifies the user

### 16.2 Password Hashing

Never store passwords in plain text. Always hash them.

```python
# app/auth/utils.py
from passlib.context import CryptContext

pwd_context = CryptContext(schemes=["bcrypt"], deprecated="auto")

def hash_password(password: str) -> str:
    """Converts a plain text password into a bcrypt hash."""
    return pwd_context.hash(password)

def verify_password(plain_password: str, hashed_password: str) -> bool:
    """Checks if a plain text password matches the stored hash."""
    return pwd_context.verify(plain_password, hashed_password)
```

### 16.3 JWT Token Creation and Verification

```python
# app/auth/jwt.py
from datetime import datetime, timedelta, timezone
from jose import JWTError, jwt
from ..config import settings

SECRET_KEY = settings.secret_key
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 30


def create_access_token(data: dict, expires_delta: timedelta = None) -> str:
    """
    Creates a JWT token.
    The 'data' dict typically contains the user's ID or username.
    The token expires after a set time for security.
    """
    to_encode = data.copy()
    expire = datetime.now(timezone.utc) + (expires_delta or timedelta(minutes=ACCESS_TOKEN_EXPIRE_MINUTES))
    to_encode.update({"exp": expire})
    return jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)


def verify_access_token(token: str) -> dict | None:
    """
    Decodes and verifies a JWT token.
    Returns the token payload if valid, None otherwise.
    """
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        return payload
    except JWTError:
        return None
```

### 16.4 Auth Dependencies

```python
# app/dependencies.py
from fastapi import Depends, HTTPException, status
from fastapi.security import OAuth2PasswordBearer
from sqlalchemy.orm import Session
from .auth.jwt import verify_access_token
from .database import SessionLocal
from . import models

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="auth/login")
# This tells FastAPI:
# 1. Expect a Bearer token in the Authorization header
# 2. The login endpoint is at "auth/login" (for Swagger UI's login button)


def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()


def get_current_user(
    token: str = Depends(oauth2_scheme),
    db: Session = Depends(get_db),
) -> models.User:
    """
    This dependency chain:
    1. oauth2_scheme extracts the token from the Authorization header
    2. We verify the token and get the user ID
    3. We query the database for the user
    4. We return the user object
    """
    payload = verify_access_token(token)
    if not payload:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Could not validate credentials",
            headers={"WWW-Authenticate": "Bearer"},
        )

    user_id = payload.get("sub")
    user = db.query(models.User).filter(models.User.id == int(user_id)).first()

    if not user:
        raise HTTPException(status_code=401, detail="User not found")
    if not user.is_active:
        raise HTTPException(status_code=400, detail="Inactive user")

    return user
```

### 16.5 Auth Routes

```python
# app/routers/auth.py
from fastapi import APIRouter, Depends, HTTPException, status
from fastapi.security import OAuth2PasswordRequestForm
from sqlalchemy.orm import Session
from ..dependencies import get_db
from ..auth.utils import verify_password, hash_password
from ..auth.jwt import create_access_token
from .. import models, schemas

router = APIRouter(prefix="/auth", tags=["Authentication"])


@router.post("/register", response_model=schemas.UserResponse, status_code=201)
def register(user: schemas.UserCreate, db: Session = Depends(get_db)):
    # Check if user already exists
    existing = db.query(models.User).filter(
        (models.User.email == user.email) | (models.User.username == user.username)
    ).first()
    if existing:
        raise HTTPException(status_code=400, detail="User already exists")

    db_user = models.User(
        username=user.username,
        email=user.email,
        hashed_password=hash_password(user.password),
    )
    db.add(db_user)
    db.commit()
    db.refresh(db_user)
    return db_user


@router.post("/login")
def login(
    form_data: OAuth2PasswordRequestForm = Depends(),
    db: Session = Depends(get_db),
):
    """
    OAuth2PasswordRequestForm expects form data with 'username' and 'password' fields.
    This works with Swagger UI's built-in "Authorize" button.
    """
    user = db.query(models.User).filter(
        models.User.username == form_data.username
    ).first()

    if not user or not verify_password(form_data.password, user.hashed_password):
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Incorrect username or password",
        )

    access_token = create_access_token(data={"sub": str(user.id)})
    return {"access_token": access_token, "token_type": "bearer"}
```

### 16.6 Role-Based Authorization

```python
from enum import Enum

class UserRole(str, Enum):
    user = "user"
    moderator = "moderator"
    admin = "admin"


def require_role(required_role: UserRole):
    """Factory function that creates a dependency requiring a specific role."""
    def role_checker(current_user: models.User = Depends(get_current_user)):
        if current_user.role != required_role:
            raise HTTPException(
                status_code=status.HTTP_403_FORBIDDEN,
                detail="Insufficient permissions",
            )
        return current_user
    return role_checker


@app.delete("/admin/users/{user_id}")
def admin_delete_user(
    user_id: int,
    admin: models.User = Depends(require_role(UserRole.admin)),
    db: Session = Depends(get_db),
):
    # Only admins can reach this endpoint
    ...
```

---

## 17. Middleware

### 17.1 What is Middleware?

Middleware is code that runs on every request before it reaches your route, and on every response before it reaches the client. It's like a security checkpoint that every person must pass through when entering and leaving a building.

Common uses: logging, measuring request time, adding headers, handling errors, and CORS.

### 17.2 Creating Custom Middleware

```python
import time
from fastapi import Request
from starlette.middleware.base import BaseHTTPMiddleware

class TimingMiddleware(BaseHTTPMiddleware):
    async def dispatch(self, request: Request, call_next):
        # Code here runs BEFORE the route
        start_time = time.time()

        response = await call_next(request)  # call the actual route

        # Code here runs AFTER the route
        duration = time.time() - start_time
        response.headers["X-Process-Time"] = f"{duration:.4f}"
        print(f"{request.method} {request.url.path} - {duration:.4f}s")

        return response


# Register the middleware
app.add_middleware(TimingMiddleware)
```

### 17.3 Using @app.middleware

A simpler syntax for middleware:

```python
@app.middleware("http")
async def add_custom_header(request: Request, call_next):
    response = await call_next(request)
    response.headers["X-App-Version"] = "1.0.0"
    return response
```

### 17.4 Built-in Middleware

```python
from starlette.middleware.trustedhost import TrustedHostMiddleware
from starlette.middleware.httpsredirect import HTTPSRedirectMiddleware
from starlette.middleware.gzip import GZipMiddleware

# Only allow requests from specific hostnames
app.add_middleware(TrustedHostMiddleware, allowed_hosts=["example.com", "*.example.com"])

# Redirect all HTTP to HTTPS
app.add_middleware(HTTPSRedirectMiddleware)

# Compress responses larger than 500 bytes
app.add_middleware(GZipMiddleware, minimum_size=500)
```

---

## 18. CORS (Cross-Origin Resource Sharing)

### 18.1 What is CORS?

When your frontend (running at `http://localhost:3000`) tries to make a request to your API (running at `http://localhost:8000`), the browser blocks it by default. This is a security feature called the Same-Origin Policy.

CORS (Cross-Origin Resource Sharing) is a mechanism that tells the browser: "It's okay, I trust requests from this other origin."

If you're building an API that a frontend app (React, Vue, mobile app) will consume, you almost certainly need CORS.

### 18.2 Configuring CORS

```python
from fastapi.middleware.cors import CORSMiddleware

app.add_middleware(
    CORSMiddleware,
    allow_origins=[
        "http://localhost:3000",       # React dev server
        "http://localhost:5173",       # Vite dev server
        "https://yourdomain.com",     # Production frontend
    ],
    allow_credentials=True,            # Allow cookies to be sent
    allow_methods=["*"],               # Allow all HTTP methods
    allow_headers=["*"],               # Allow all headers
)

# For development, you might allow all origins (NOT for production):
# allow_origins=["*"]
```

---

## 19. Background Tasks

### 19.1 What are Background Tasks?

Sometimes a route needs to trigger a time-consuming operation (sending an email, processing an image, generating a report) but shouldn't make the client wait for it. Background tasks let you return a response immediately while the operation continues in the background.

### 19.2 Using BackgroundTasks

```python
from fastapi import BackgroundTasks

def send_welcome_email(email: str, username: str):
    """This function runs in the background after the response is sent."""
    # Simulate sending an email
    print(f"Sending welcome email to {email}")
    # In reality: use an email library here
    import time
    time.sleep(5)  # simulating a slow operation
    print(f"Email sent to {email}")


def log_new_user(username: str):
    """Another background task."""
    print(f"Logging new user: {username}")


@app.post("/register")
def register(user: schemas.UserCreate, background_tasks: BackgroundTasks):
    # Create the user immediately
    new_user = create_user(user)

    # Schedule tasks to run AFTER the response is sent
    background_tasks.add_task(send_welcome_email, user.email, user.username)
    background_tasks.add_task(log_new_user, user.username)

    # Return immediately — the client doesn't wait for the email
    return {"message": "User created", "user_id": new_user.id}
```

For more complex background processing (long-running jobs, retries, scheduling), use a task queue like **Celery** or **ARQ**.

---

## 20. WebSockets

### 20.1 What are WebSockets?

Normal HTTP is a one-way street: the client sends a request, the server sends a response, and the connection closes. WebSockets create a persistent, two-way connection where both the client and server can send messages at any time.

Use cases: real-time chat, live notifications, collaborative editing, live data feeds, multiplayer games.

### 20.2 Basic WebSocket Endpoint

```python
from fastapi import WebSocket, WebSocketDisconnect

@app.websocket("/ws")
async def websocket_endpoint(websocket: WebSocket):
    await websocket.accept()  # accept the connection

    try:
        while True:
            # Wait for a message from the client
            data = await websocket.receive_text()

            # Send a message back
            await websocket.send_text(f"You said: {data}")
    except WebSocketDisconnect:
        print("Client disconnected")
```

### 20.3 Chat Room Example

```python
class ConnectionManager:
    """Manages active WebSocket connections."""

    def __init__(self):
        self.active_connections: list[WebSocket] = []

    async def connect(self, websocket: WebSocket):
        await websocket.accept()
        self.active_connections.append(websocket)

    def disconnect(self, websocket: WebSocket):
        self.active_connections.remove(websocket)

    async def broadcast(self, message: str):
        """Send a message to all connected clients."""
        for connection in self.active_connections:
            await connection.send_text(message)


manager = ConnectionManager()


@app.websocket("/ws/chat/{username}")
async def chat(websocket: WebSocket, username: str):
    await manager.connect(websocket)
    await manager.broadcast(f"{username} joined the chat")

    try:
        while True:
            message = await websocket.receive_text()
            await manager.broadcast(f"{username}: {message}")
    except WebSocketDisconnect:
        manager.disconnect(websocket)
        await manager.broadcast(f"{username} left the chat")
```

---

## 21. Error Handling and Custom Exceptions

### 21.1 HTTPException

The standard way to return error responses:

```python
from fastapi import HTTPException, status

@app.get("/items/{item_id}")
def get_item(item_id: int):
    item = find_item(item_id)
    if not item:
        raise HTTPException(
            status_code=status.HTTP_404_NOT_FOUND,
            detail="Item not found",
            headers={"X-Error": "Item lookup failed"},  # optional custom headers
        )
    return item
```

### 21.2 Custom Exception Classes

```python
class PostNotFoundError(Exception):
    def __init__(self, post_id: int):
        self.post_id = post_id

class PermissionDeniedError(Exception):
    def __init__(self, message: str = "Permission denied"):
        self.message = message


# Register exception handlers
@app.exception_handler(PostNotFoundError)
async def post_not_found_handler(request: Request, exc: PostNotFoundError):
    return JSONResponse(
        status_code=404,
        content={"detail": f"Post with id {exc.post_id} not found"},
    )

@app.exception_handler(PermissionDeniedError)
async def permission_denied_handler(request: Request, exc: PermissionDeniedError):
    return JSONResponse(
        status_code=403,
        content={"detail": exc.message},
    )


# Usage in routes:
@app.get("/posts/{post_id}")
def get_post(post_id: int):
    post = find_post(post_id)
    if not post:
        raise PostNotFoundError(post_id)
    return post
```

### 21.3 Global Exception Handler

Catch all unhandled exceptions to prevent exposing internal errors:

```python
@app.exception_handler(Exception)
async def global_exception_handler(request: Request, exc: Exception):
    # Log the full error for debugging
    import traceback
    traceback.print_exc()

    # Return a generic error to the client
    return JSONResponse(
        status_code=500,
        content={"detail": "An internal server error occurred"},
    )
```

---

## 22. Templates and Static Files (Server-Side Rendering)

### 22.1 When to Use Templates

While FastAPI is primarily for APIs, it can also serve HTML pages using Jinja2 templates. This is useful for admin panels, simple websites, or when you don't need a full SPA (Single Page Application).

### 22.2 Setup

```bash
pip install jinja2
```

```python
from fastapi import Request
from fastapi.templating import Jinja2Templates
from fastapi.staticfiles import StaticFiles

# Mount static files directory
app.mount("/static", StaticFiles(directory="static"), name="static")

# Set up templates
templates = Jinja2Templates(directory="templates")
```

### 22.3 Serving HTML Pages

```python
@app.get("/", response_class=HTMLResponse)
def homepage(request: Request):
    posts = get_recent_posts()
    return templates.TemplateResponse(
        "index.html",
        {
            "request": request,   # REQUIRED — FastAPI needs this
            "posts": posts,
            "title": "Home",
        }
    )
```

```html
<!-- templates/index.html -->
<!DOCTYPE html>
<html>
<head>
    <title>{{ title }}</title>
    <link rel="stylesheet" href="{{ url_for('static', path='css/style.css') }}">
</head>
<body>
    <h1>Latest Posts</h1>
    {% for post in posts %}
        <article>
            <h2>{{ post.title }}</h2>
            <p>{{ post.content[:200] }}...</p>
        </article>
    {% endfor %}
</body>
</html>
```

---

## 23. Testing

### 23.1 Why Test APIs?

API testing ensures that your endpoints return the correct data, status codes, and error messages. Since APIs are consumed by other programs, correctness is critical — a subtle bug can cascade through an entire system.

### 23.2 Setup

```bash
pip install pytest httpx
```

### 23.3 Test Client

FastAPI provides a `TestClient` built on httpx:

```python
# tests/conftest.py
import pytest
from fastapi.testclient import TestClient
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker
from app.main import app
from app.database import Base
from app.dependencies import get_db

# Use a separate test database
SQLALCHEMY_DATABASE_URL = "sqlite:///./test.db"
engine = create_engine(SQLALCHEMY_DATABASE_URL, connect_args={"check_same_thread": False})
TestingSessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)


def override_get_db():
    db = TestingSessionLocal()
    try:
        yield db
    finally:
        db.close()


# Override the database dependency with the test database
app.dependency_overrides[get_db] = override_get_db


@pytest.fixture(scope="function")
def client():
    """Create a fresh test database for each test."""
    Base.metadata.create_all(bind=engine)
    with TestClient(app) as c:
        yield c
    Base.metadata.drop_all(bind=engine)


@pytest.fixture
def auth_headers(client):
    """Register a user and return authentication headers."""
    client.post("/auth/register", json={
        "username": "testuser",
        "email": "test@example.com",
        "password": "testpass123",
    })
    response = client.post("/auth/login", data={
        "username": "testuser",
        "password": "testpass123",
    })
    token = response.json()["access_token"]
    return {"Authorization": f"Bearer {token}"}
```

### 23.4 Writing Tests

```python
# tests/test_posts.py

def test_create_post(client, auth_headers):
    """Test creating a new post."""
    response = client.post(
        "/posts/",
        json={
            "title": "Test Post",
            "slug": "test-post",
            "content": "This is a test post with enough content.",
        },
        headers=auth_headers,
    )
    assert response.status_code == 201
    data = response.json()
    assert data["title"] == "Test Post"
    assert data["slug"] == "test-post"
    assert "id" in data


def test_create_post_unauthorized(client):
    """Test that creating a post without auth returns 401."""
    response = client.post(
        "/posts/",
        json={"title": "Test", "slug": "test", "content": "Content here..."},
    )
    assert response.status_code == 401


def test_create_post_invalid_data(client, auth_headers):
    """Test that invalid data returns 422 with error details."""
    response = client.post(
        "/posts/",
        json={"title": "Hi"},  # missing required fields, title too short
        headers=auth_headers,
    )
    assert response.status_code == 422


def test_get_posts(client):
    """Test listing posts."""
    response = client.get("/posts/")
    assert response.status_code == 200
    data = response.json()
    assert "posts" in data
    assert "total" in data


def test_get_post_not_found(client):
    """Test that requesting a non-existent post returns 404."""
    response = client.get("/posts/nonexistent-slug")
    assert response.status_code == 404


def test_delete_post_not_owner(client, auth_headers):
    """Test that deleting someone else's post returns 403."""
    # Create post as testuser
    create_resp = client.post(
        "/posts/",
        json={"title": "My Post", "slug": "my-post", "content": "Content here..."},
        headers=auth_headers,
    )
    post_id = create_resp.json()["id"]

    # Register and login as another user
    client.post("/auth/register", json={
        "username": "otheruser", "email": "other@example.com", "password": "pass123",
    })
    login_resp = client.post("/auth/login", data={
        "username": "otheruser", "password": "pass123",
    })
    other_token = login_resp.json()["access_token"]
    other_headers = {"Authorization": f"Bearer {other_token}"}

    # Try to delete
    response = client.delete(f"/posts/{post_id}", headers=other_headers)
    assert response.status_code == 403
```

### 23.5 Running Tests

```bash
pytest                          # run all tests
pytest tests/test_posts.py      # run specific file
pytest -v                       # verbose output
pytest -s                       # show print statements
pytest --tb=short               # shorter tracebacks
pytest -x                       # stop on first failure
```

---

## 24. Application Structure and Routers

### 24.1 What is an APIRouter?

As your API grows, putting all routes in a single file becomes unmanageable. `APIRouter` lets you organize routes into separate modules — similar to Django's `app/urls.py` pattern.

### 24.2 Creating Routers

```python
# app/routers/posts.py
from fastapi import APIRouter, Depends
from sqlalchemy.orm import Session
from ..dependencies import get_db, get_current_user

router = APIRouter(
    prefix="/posts",       # all routes start with /posts
    tags=["Posts"],        # grouped under "Posts" in documentation
    dependencies=[],       # shared dependencies for all routes
)

@router.get("/")
def list_posts(db: Session = Depends(get_db)):
    ...

@router.get("/{post_id}")
def get_post(post_id: int, db: Session = Depends(get_db)):
    ...

@router.post("/", dependencies=[Depends(get_current_user)])
def create_post(...):
    ...
```

```python
# app/routers/users.py
from fastapi import APIRouter

router = APIRouter(prefix="/users", tags=["Users"])

@router.get("/")
def list_users():
    ...

@router.get("/{user_id}")
def get_user(user_id: int):
    ...
```

### 24.3 Including Routers in the Main App

```python
# app/main.py
from fastapi import FastAPI
from .routers import posts, users, auth

app = FastAPI(title="My Blog API", version="1.0.0")

# Include all routers
app.include_router(auth.router)
app.include_router(posts.router)
app.include_router(users.router)

# You can also add a prefix when including:
# app.include_router(posts.router, prefix="/api/v1")

@app.get("/")
def root():
    return {"message": "Welcome to the API"}
```

---

## 25. Settings and Configuration

### 25.1 Why Use Configuration?

Your application needs different settings for different environments — a local SQLite database for development, a PostgreSQL database for production, different secret keys, different logging levels. Hardcoding these values is dangerous and inflexible.

### 25.2 Using Pydantic Settings

```bash
pip install pydantic-settings python-dotenv
```

```python
# app/config.py
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    """
    Application settings loaded from environment variables and .env file.
    Pydantic validates these values at startup — if a required variable
    is missing, the app won't start, and you'll see a clear error.
    """

    # App settings
    app_name: str = "My Blog API"
    debug: bool = False

    # Database
    database_url: str = "sqlite:///./app.db"

    # Authentication
    secret_key: str    # REQUIRED — no default, must be set
    access_token_expire_minutes: int = 30

    # CORS
    allowed_origins: list[str] = ["http://localhost:3000"]

    # Email (optional)
    smtp_host: str = ""
    smtp_port: int = 587
    smtp_user: str = ""
    smtp_password: str = ""

    class Config:
        env_file = ".env"   # Load variables from .env file
        env_file_encoding = "utf-8"


# Create a single settings instance
settings = Settings()
```

```
# .env file (in the project root — add to .gitignore!)
DATABASE_URL=postgresql://user:password@localhost:5432/blogdb
SECRET_KEY=your-super-secret-key-here-change-this
DEBUG=true
ALLOWED_ORIGINS=["http://localhost:3000","http://localhost:5173"]
```

```python
# Usage anywhere in your app:
from .config import settings

engine = create_engine(settings.database_url)
token = create_access_token(data, settings.secret_key)
```

---

## 26. Caching

### 26.1 Why Cache?

If your API endpoint queries the database every time it's called, and the data rarely changes, you're wasting resources. Caching stores the result the first time and serves it from memory for subsequent requests.

### 26.2 Simple In-Memory Caching

```python
from functools import lru_cache
from datetime import datetime, timedelta

# lru_cache for function results (good for config, not for request data)
@lru_cache()
def get_settings():
    return Settings()


# Custom time-based cache
class SimpleCache:
    def __init__(self):
        self._cache = {}

    def get(self, key: str):
        if key in self._cache:
            value, expiry = self._cache[key]
            if datetime.now() < expiry:
                return value
            del self._cache[key]
        return None

    def set(self, key: str, value, ttl_seconds: int = 300):
        expiry = datetime.now() + timedelta(seconds=ttl_seconds)
        self._cache[key] = (value, expiry)

    def delete(self, key: str):
        self._cache.pop(key, None)

cache = SimpleCache()


@app.get("/posts")
def get_posts(db: Session = Depends(get_db)):
    cached = cache.get("posts_list")
    if cached:
        return cached

    posts = db.query(models.Post).filter(models.Post.status == "published").all()
    result = [schemas.PostResponse.model_validate(p) for p in posts]
    cache.set("posts_list", result, ttl_seconds=300)
    return result
```

### 26.3 Redis Caching (Production)

```bash
pip install redis
```

```python
import redis
import json

redis_client = redis.Redis(host='localhost', port=6379, db=0)

def get_cached(key: str):
    data = redis_client.get(key)
    return json.loads(data) if data else None

def set_cached(key: str, value, ttl: int = 300):
    redis_client.setex(key, ttl, json.dumps(value, default=str))

def delete_cached(key: str):
    redis_client.delete(key)
```

---

## 27. Rate Limiting

### 27.1 What is Rate Limiting?

Rate limiting controls how many requests a client can make in a given time period. It prevents abuse, protects your server from being overwhelmed, and ensures fair usage.

### 27.2 Simple Rate Limiter

```python
from fastapi import Request, HTTPException
from collections import defaultdict
import time

class RateLimiter:
    def __init__(self, max_requests: int = 100, window_seconds: int = 60):
        self.max_requests = max_requests
        self.window = window_seconds
        self.requests = defaultdict(list)

    def is_allowed(self, client_id: str) -> bool:
        now = time.time()
        # Remove old requests outside the window
        self.requests[client_id] = [
            t for t in self.requests[client_id] if now - t < self.window
        ]
        if len(self.requests[client_id]) >= self.max_requests:
            return False
        self.requests[client_id].append(now)
        return True

rate_limiter = RateLimiter(max_requests=100, window_seconds=60)


@app.middleware("http")
async def rate_limit_middleware(request: Request, call_next):
    client_ip = request.client.host
    if not rate_limiter.is_allowed(client_ip):
        return JSONResponse(
            status_code=429,
            content={"detail": "Too many requests. Please try again later."},
        )
    return await call_next(request)
```

For production, use Redis-based rate limiting with packages like `slowapi`.

---

## 28. Logging

### 28.1 Why Logging?

Logging records what your application is doing — which requests it receives, what errors occur, how long operations take. It's essential for debugging, monitoring, and auditing.

### 28.2 Setting Up Logging

```python
# app/logging_config.py
import logging
import sys

def setup_logging():
    # Create a logger
    logger = logging.getLogger("app")
    logger.setLevel(logging.INFO)

    # Console handler (output to terminal)
    console_handler = logging.StreamHandler(sys.stdout)
    console_handler.setLevel(logging.INFO)

    # Format: timestamp - level - message
    formatter = logging.Formatter(
        "%(asctime)s - %(name)s - %(levelname)s - %(message)s"
    )
    console_handler.setFormatter(formatter)
    logger.addHandler(console_handler)

    # File handler (output to file)
    file_handler = logging.FileHandler("app.log")
    file_handler.setLevel(logging.WARNING)
    file_handler.setFormatter(formatter)
    logger.addHandler(file_handler)

    return logger
```

```python
# Usage in your routes and services
import logging

logger = logging.getLogger("app")

@app.get("/posts/{post_id}")
def get_post(post_id: int, db: Session = Depends(get_db)):
    logger.info(f"Fetching post {post_id}")
    post = crud.get_post(db, post_id)
    if not post:
        logger.warning(f"Post {post_id} not found")
        raise HTTPException(status_code=404, detail="Post not found")
    return post
```

**Log levels explained:**
- `DEBUG` — Detailed information for diagnosing problems
- `INFO` — Confirmation that things are working as expected
- `WARNING` — Something unexpected happened, but the app still works
- `ERROR` — A serious problem — something failed
- `CRITICAL` — A very serious error — the app might crash

---

## 29. Async and Await — Understanding Asynchronous Programming

### 29.1 What is Async?

Synchronous code executes one thing at a time. If you're waiting for a database query to return, your program sits idle, doing nothing.

Asynchronous code can start an operation and then do other things while waiting for it to finish. It's like cooking: instead of standing at the oven watching water boil, you chop vegetables while the water heats up.

This matters for APIs because a server handles many requests simultaneously. With synchronous code, while one request waits for a database query, all other requests are blocked. With async code, the server can handle other requests during that wait time.

### 29.2 When to Use async def

```python
# Use 'async def' when your function calls async operations
# (async database queries, async HTTP requests, async file I/O)
@app.get("/posts")
async def get_posts():
    posts = await async_db_query()  # 'await' pauses this function
    return posts                     # but the server handles other requests meanwhile

# Use regular 'def' when your function only does synchronous operations
# (regular database queries with SQLAlchemy, CPU-bound work)
@app.get("/posts")
def get_posts(db: Session = Depends(get_db)):
    posts = db.query(Post).all()  # synchronous SQLAlchemy query
    return posts
# FastAPI automatically runs 'def' functions in a thread pool so they
# don't block the event loop.
```

**The rule is simple:**
- If you use `await` inside the function → use `async def`
- If you don't use `await` → use regular `def`
- Never use blocking (synchronous) calls inside `async def` — it will freeze your entire server

### 29.3 Async Database Example (with async SQLAlchemy)

```python
from sqlalchemy.ext.asyncio import create_async_engine, AsyncSession
from sqlalchemy.orm import sessionmaker

# Use an async database driver
engine = create_async_engine("postgresql+asyncpg://user:pass@localhost/dbname")
AsyncSessionLocal = sessionmaker(engine, class_=AsyncSession, expire_on_commit=False)

async def get_db():
    async with AsyncSessionLocal() as session:
        yield session

@app.get("/posts")
async def get_posts(db: AsyncSession = Depends(get_db)):
    result = await db.execute(select(Post).where(Post.status == "published"))
    posts = result.scalars().all()
    return posts
```

---

## 30. Events — Startup and Shutdown

### 30.1 What are Lifespan Events?

Lifespan events let you run code when the application starts up and when it shuts down. Common uses: creating database tables, establishing connections, loading ML models on startup; closing connections and cleaning up on shutdown.

### 30.2 Using the Lifespan Context Manager

```python
from contextlib import asynccontextmanager

@asynccontextmanager
async def lifespan(app: FastAPI):
    # === STARTUP ===
    # Code here runs when the application starts
    print("Starting up...")
    Base.metadata.create_all(bind=engine)  # create database tables
    # You could also: connect to Redis, load ML models, warm caches

    yield  # The application runs between startup and shutdown

    # === SHUTDOWN ===
    # Code here runs when the application stops
    print("Shutting down...")
    # Close connections, save state, clean up resources


app = FastAPI(lifespan=lifespan)
```

---

## 31. Security Best Practices

### 31.1 Essential Security Measures

**Use HTTPS in production:** Never send sensitive data (tokens, passwords) over plain HTTP. Configure your reverse proxy (Nginx) with TLS/SSL certificates.

**Never expose sensitive data:** Use response models to filter out fields like passwords, internal IDs, and debug information.

**Validate all input:** Pydantic does this automatically, but also validate business logic (e.g., "is this user allowed to access this resource?").

**Hash passwords:** Never store plain text passwords. Use bcrypt via passlib.

**Use environment variables for secrets:** Never commit secrets to version control.

```python
# .gitignore
.env
*.db
__pycache__/
```

**Set token expiration:** JWT tokens should expire. Use short-lived access tokens (15-30 minutes) and longer-lived refresh tokens.

**Rate limit your API:** Prevent brute-force attacks and abuse.

**Use CORS restrictively:** Only allow origins that actually need access to your API.

**Keep dependencies updated:** Regularly run `pip audit` to check for known vulnerabilities.

**Validate file uploads:** Check file types, sizes, and scan for malicious content.

```python
# Security headers middleware
@app.middleware("http")
async def add_security_headers(request: Request, call_next):
    response = await call_next(request)
    response.headers["X-Content-Type-Options"] = "nosniff"
    response.headers["X-Frame-Options"] = "DENY"
    response.headers["X-XSS-Protection"] = "1; mode=block"
    response.headers["Strict-Transport-Security"] = "max-age=31536000; includeSubDomains"
    return response
```

---

## 32. Deployment — Going Live

### 32.1 Preparation

```bash
# Generate requirements file
pip freeze > requirements.txt

# Make sure all tests pass
pytest

# Set environment variables for production
# DATABASE_URL, SECRET_KEY, DEBUG=false, etc.
```

### 32.2 Running with Uvicorn (Production Settings)

```bash
# Development
uvicorn app.main:app --reload

# Production
uvicorn app.main:app --host 0.0.0.0 --port 8000 --workers 4
# --host 0.0.0.0 → listen on all network interfaces
# --workers 4 → spawn 4 worker processes (use 2-4 × CPU cores)
# Do NOT use --reload in production
```

### 32.3 Running with Gunicorn + Uvicorn Workers

Gunicorn manages multiple Uvicorn worker processes for better reliability:

```bash
pip install gunicorn

gunicorn app.main:app \
    --workers 4 \
    --worker-class uvicorn.workers.UvicornWorker \
    --bind 0.0.0.0:8000 \
    --access-logfile - \
    --error-logfile -
```

### 32.4 Nginx Reverse Proxy

Nginx sits in front of your application server, handles SSL termination, serves static files, and load-balances across workers.

```nginx
server {
    listen 80;
    server_name yourdomain.com;
    return 301 https://$server_name$request_uri;  # redirect HTTP to HTTPS
}

server {
    listen 443 ssl;
    server_name yourdomain.com;

    ssl_certificate /path/to/cert.pem;
    ssl_certificate_key /path/to/key.pem;

    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        # WebSocket support
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }

    location /static/ {
        alias /path/to/static/;
    }
}
```

### 32.5 Docker

```dockerfile
# Dockerfile
FROM python:3.12-slim

WORKDIR /app

# Install dependencies first (for Docker layer caching)
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY . .

EXPOSE 8000

CMD ["gunicorn", "app.main:app", \
     "--workers", "4", \
     "--worker-class", "uvicorn.workers.UvicornWorker", \
     "--bind", "0.0.0.0:8000"]
```

```yaml
# docker-compose.yml
version: "3.8"

services:
  api:
    build: .
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql://user:password@db:5432/mydb
      - SECRET_KEY=your-secret-key
      - DEBUG=false
    depends_on:
      - db
      - redis

  db:
    image: postgres:16
    volumes:
      - postgres_data:/var/lib/postgresql/data
    environment:
      - POSTGRES_DB=mydb
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=password

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"

volumes:
  postgres_data:
```

### 32.6 Health Check Endpoint

Always include a health check endpoint for monitoring and orchestration:

```python
@app.get("/health")
def health_check(db: Session = Depends(get_db)):
    try:
        db.execute(text("SELECT 1"))
        return {"status": "healthy", "database": "connected"}
    except Exception:
        raise HTTPException(status_code=503, detail="Database unavailable")
```

---

## 33. Quick Reference and Cheat Sheet

### Common Imports

```python
# Core
from fastapi import FastAPI, Depends, HTTPException, status, Request
from fastapi import Query, Path, Body, Form, File, UploadFile
from fastapi import BackgroundTasks, WebSocket
from fastapi.responses import JSONResponse, HTMLResponse, RedirectResponse
from fastapi.middleware.cors import CORSMiddleware
from fastapi.security import OAuth2PasswordBearer, OAuth2PasswordRequestForm

# Pydantic
from pydantic import BaseModel, Field, EmailStr, ConfigDict
from pydantic import field_validator, model_validator
from pydantic_settings import BaseSettings

# SQLAlchemy
from sqlalchemy import create_engine, Column, Integer, String, Text, Boolean
from sqlalchemy import ForeignKey, DateTime, Float, Table, Enum
from sqlalchemy.orm import Session, relationship, sessionmaker, DeclarativeBase
from sqlalchemy.sql import func

# Standard library
from typing import Optional
from datetime import datetime, timedelta
from enum import Enum
```

### Parameter Sources

```python
@app.get("/items/{item_id}")
def get_item(
    item_id: int,                              # Path parameter (from URL)
    q: str = None,                             # Query parameter (?q=value)
    q2: str = Query(default=None, max_length=50),  # Query with validation
    item_id2: int = Path(..., ge=1),           # Path with validation
):
    ...

@app.post("/items")
def create_item(
    item: ItemCreate,                          # Request body (JSON)
    importance: int = Body(default=0),         # Extra body field
):
    ...

@app.post("/login")
def login(
    username: str = Form(...),                 # Form field
    password: str = Form(...),                 # Form field
):
    ...

@app.post("/upload")
async def upload(
    file: UploadFile,                          # File upload
    description: str = Form(default=""),       # Form field alongside file
):
    ...

def my_dependency(
    token: str = Depends(oauth2_scheme),       # Dependency injection
    db: Session = Depends(get_db),             # Dependency injection
):
    ...
```

### Pydantic Model Patterns

```python
# Create schema (for POST requests)
class ItemCreate(BaseModel):
    name: str = Field(..., min_length=1, max_length=100)
    price: float = Field(..., gt=0)
    is_active: bool = True

# Update schema (all fields optional, for PATCH requests)
class ItemUpdate(BaseModel):
    name: Optional[str] = Field(default=None, min_length=1, max_length=100)
    price: Optional[float] = Field(default=None, gt=0)
    is_active: Optional[bool] = None

# Response schema (what the API returns)
class ItemResponse(BaseModel):
    id: int
    name: str
    price: float
    is_active: bool
    created_at: datetime

    model_config = ConfigDict(from_attributes=True)
```

### SQLAlchemy ORM Cheat Sheet

```python
# Create
db_item = Model(field=value)
db.add(db_item)
db.commit()
db.refresh(db_item)

# Read
db.query(Model).all()                              # all records
db.query(Model).first()                             # first record or None
db.query(Model).filter(Model.id == 1).first()       # filter by condition
db.query(Model).filter(Model.name.ilike("%search%")).all()  # case-insensitive search
db.query(Model).order_by(Model.created_at.desc()).all()     # sorted
db.query(Model).offset(10).limit(5).all()           # pagination
db.query(func.count(Model.id)).scalar()             # count

# Update
item = db.query(Model).filter(Model.id == 1).first()
item.name = "New Name"
db.commit()

# Delete
item = db.query(Model).filter(Model.id == 1).first()
db.delete(item)
db.commit()
```

### CLI Commands

```bash
# Run development server
uvicorn app.main:app --reload

# Run production server
gunicorn app.main:app -w 4 -k uvicorn.workers.UvicornWorker

# Database migrations
alembic init alembic                        # initialize alembic
alembic revision --autogenerate -m "msg"    # create migration
alembic upgrade head                        # apply all migrations
alembic downgrade -1                        # roll back one migration
alembic history                             # view migration history

# Testing
pytest                     # run all tests
pytest -v                  # verbose output
pytest -x                  # stop on first failure
pytest --cov=app           # with coverage report

# Dependencies
pip freeze > requirements.txt   # save dependencies
pip install -r requirements.txt # install dependencies
pip audit                       # check for vulnerabilities
```

---

*This guide covers every major FastAPI concept with clear explanations. For the most up-to-date information, always refer to the official FastAPI documentation at fastapi.tiangolo.com.*
