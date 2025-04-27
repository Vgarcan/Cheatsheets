
# 📌 Django Class-Based Views (CBVs)  

## 🧐 Conceptos básicos  

#### ❓ ¿Qué son las Class-Based Views?  
Las CBVs son vistas en Django que utilizan clases en lugar de funciones para manejar solicitudes HTTP. Son más estructuradas y reutilizables, permitiendo extender funcionalidades mediante herencia y mixins.  

📌 **Ejemplo de una CBV con `TemplateView`**:  
```python
from django.views.generic import TemplateView

class HomePageView(TemplateView):
    template_name = "home.html"
```

#### ⚖️ Diferencias entre CBVs y FBVs  

| Característica | Function-Based Views (FBVs) | Class-Based Views (CBVs) |
|--------------|------------------|------------------|
| **Código más corto** | ❌ No siempre | ✅ Sí |
| **Modularidad y reutilización** | ❌ No | ✅ Sí |
| **Curva de aprendizaje** | ✅ Fácil | ⚠️ Más difícil |
| **Extensibilidad** | ❌ Limitada | ✅ Más flexible |
| **Explícito y fácil de leer** | ✅ Sí | ⚠️ Puede ser confuso |

📌 **Ejemplo de la misma vista en FBV y CBV**:  

🔹 **FBV:**  
```python
from django.shortcuts import render

def home_view(request):
    return render(request, "home.html")
```

🔹 **CBV:**  
```python
from django.views.generic import TemplateView

class HomePageView(TemplateView):
    template_name = "home.html"
```

✔️ **Conclusión:**  
- Las **FBVs** son útiles para vistas simples y rápidas.  
- Las **CBVs** son mejores para proyectos grandes y reutilizables.  

---
¡Vamos a ello! Aquí tienes una explicación detallada sobre **`View`** y **`TemplateView`**, incluyendo sus posibilidades de personalización y ejemplos de código bien comentados en inglés.  

---

## 📌 **Basic Generic Views in Django**

Django provides some basic class-based views that serve as the foundation for more complex views. These are:  

1️⃣ **`View`** – The most generic and minimal class-based view.  
2️⃣ **`TemplateView`** – A simple view for rendering templates with minimal logic.  

Let's explore them in detail! 🚀  

---

### 🔹 **1. The `View` Class**  

#### 🔍 **What is `View`?**  
The `View` class is the **base class for all class-based views in Django**. It does not provide any default behavior, meaning you have to explicitly define how it handles HTTP requests.  

#### 💡 **Key Features:**  
✔️ Supports multiple HTTP methods (`GET`, `POST`, `PUT`, etc.).  
✔️ Uses the `dispatch()` method to call the appropriate handler based on the request method.  
✔️ Allows customization through mixins.  

#### 🔧 **How can we customize `View`?**  
✅ Override `get()`, `post()`, or other HTTP methods.  
✅ Implement `dispatch()` to add logic before handling requests.  
✅ Restrict allowed HTTP methods using `http_method_names`.  

---

#### 📌 **Basic Example of `View`**
```python
from django.views import View
from django.http import HttpResponse

class SimpleView(View):
    """A basic Django View handling GET requests."""
    
    def get(self, request):
        return HttpResponse("<h1>Welcome to SimpleView</h1><p>This is a basic class-based view.</p>")
```

🔹 **URL Configuration** (`urls.py`)
```python
from django.urls import path
from .views import SimpleView

urlpatterns = [
    path('simple/', SimpleView.as_view(), name='simple_view'),
]
```

---

#### 📌 **Advanced Example – Custom Dispatch Logic**
You can override `dispatch()` to add logic before handling requests.  
For example, checking if the user is authenticated:  
```python
from django.views import View
from django.http import HttpResponseForbidden, HttpResponse

class SecureView(View):
    """A Django View that restricts access to authenticated users."""

    def dispatch(self, request, *args, **kwargs):
        if not request.user.is_authenticated:
            return HttpResponseForbidden("<h1>Access Denied</h1><p>You must be logged in.</p>")
        return super().dispatch(request, *args, **kwargs)

    def get(self, request):
        return HttpResponse("<h1>Welcome, authenticated user!</h1>")
```

🔹 **Customization Possibilities**  
✔️ You can override `dispatch()` to add authentication, logging, or pre-processing logic.  
✔️ You can define multiple HTTP methods (`get()`, `post()`, etc.).  
✔️ You can use `http_method_names = ['get', 'post']` to allow only specific methods.  

---

### 🔹 **2. The `TemplateView` Class**  

#### 🔍 **What is `TemplateView`?**  
`TemplateView` is a subclass of `View` that is designed **specifically for rendering templates**.  

#### 💡 **Key Features:**  
✔️ Automatically renders a template without extra logic.  
✔️ Uses `template_name` to define which template to render.  
✔️ Can pass context data to the template via `get_context_data()`.  

#### 🔧 **How can we customize `TemplateView`?**  
✅ Override `get_context_data()` to pass additional data.  
✅ Use `extra_context` to send static context data.  
✅ Combine with mixins for authentication or permissions.  

---

#### 📌 **Basic Example of `TemplateView`**
```python
from django.views.generic import TemplateView

class HomePageView(TemplateView):
    """A simple TemplateView that renders a static template."""
    template_name = "home.html"
```

🔹 **URL Configuration** (`urls.py`)
```python
from django.urls import path
from .views import HomePageView

urlpatterns = [
    path('', HomePageView.as_view(), name='home'),
]
```

🔹 **Template Example** (`templates/home.html`)
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Home</title>
</head>
<body>
    <h1>Welcome to Home Page</h1>
    <p>This is a simple template rendered using TemplateView.</p>
</body>
</html>
```

---

#### 📌 **Passing Context Data in `TemplateView`**
If you need to send data to the template, override `get_context_data()`:
```python
from django.views.generic import TemplateView

class AboutPageView(TemplateView):
    """A TemplateView that passes dynamic context data."""
    template_name = "about.html"

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context["company_name"] = "Django Corp"
        context["year"] = 2025
        return context
```

🔹 **Template Example** (`templates/about.html`)
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>About</title>
</head>
<body>
    <h1>About Us</h1>
    <p>Company: {{ company_name }}</p>
    <p>Year: {{ year }}</p>
</body>
</html>
```

---

#### 📌 **Adding Static Context Data using `extra_context`**
If your data is **static and does not require dynamic processing**, use `extra_context`:
```python
class ContactPageView(TemplateView):
    """A TemplateView using extra_context for static data."""
    template_name = "contact.html"
    extra_context = {"email": "support@djangocorp.com", "phone": "+123456789"}
```

🔹 **Customization Possibilities**  
✔️ Override `get_context_data()` to inject dynamic content.  
✔️ Use `extra_context` for static context data.  
✔️ Extend with authentication mixins (e.g., `LoginRequiredMixin`).  

---

### 🎯 **When to Use Each View?**
| View | Use Case | Customization |
|------|---------|--------------|
| `View` | When you need full control over HTTP methods and logic | Override `get()`, `post()`, `dispatch()` |
| `TemplateView` | When you only need to render a static template | Use `get_context_data()` for dynamic content |


---

## 📌 **Model-Based Views in Django (CRUD)**  
Django provides several **Class-Based Views (CBVs)** that interact with the database using Django’s ORM. These views handle common **CRUD (Create, Read, Update, Delete)** operations efficiently.  

📖 **Views covered in this section:**  
1️⃣ `ListView` – Read multiple objects (list).  
2️⃣ `DetailView` – Read a single object (detail).  
3️⃣ `CreateView` – Create a new object.  
4️⃣ `UpdateView` – Update an existing object.  
5️⃣ `DeleteView` – Delete an object.  

---

### 🔹 **1. `ListView` – Displaying a List of Objects**  

#### 🔍 **What is `ListView`?**  
`ListView` is used to **display multiple objects** from a database table in a paginated list.  

#### 💡 **Key Features:**  
✔️ Automatically queries the database and retrieves objects.  
✔️ Uses `template_name` to specify the template.  
✔️ Provides pagination with `paginate_by`.  
✔️ Allows filtering using `get_queryset()`.  
✔️ Passes `object_list` in the template context.  

---

#### 📌 **Basic Example of `ListView`**  

Let's assume we have a **`Post` model** in our Django app:  

```python
from django.db import models

class Post(models.Model):
    """A blog post model"""
    title = models.CharField(max_length=200)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.title
```

Now, we create a `ListView` to show a list of posts:  

```python
from django.views.generic import ListView
from .models import Post

class PostListView(ListView):
    """Displays a list of posts."""
    model = Post
    template_name = "posts/post_list.html"
    context_object_name = "posts"  ## Custom context variable
    ordering = ['-created_at']  ## Order by latest posts first
    paginate_by = 5  ## Show 5 posts per page
```

🔹 **URL Configuration** (`urls.py`)  
```python
from django.urls import path
from .views import PostListView

urlpatterns = [
    path('posts/', PostListView.as_view(), name='post_list'),
]
```

🔹 **Template Example** (`templates/posts/post_list.html`)  
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Post List</title>
</head>
<body>
    <h1>Blog Posts</h1>
    
    <ul>
        {% for post in posts %}
            <li>
                <a href="{% url 'post_detail' post.pk %}">{{ post.title }}</a> - {{ post.created_at }}
            </li>
        {% empty %}
            <li>No posts available.</li>
        {% endfor %}
    </ul>

    <!-- Pagination -->
    {% if is_paginated %}
        <div>
            {% if page_obj.has_previous %}
                <a href="?page=1">First</a>
                <a href="?page={{ page_obj.previous_page_number }}">Previous</a>
            {% endif %}

            Page {{ page_obj.number }} of {{ page_obj.paginator.num_pages }}

            {% if page_obj.has_next %}
                <a href="?page={{ page_obj.next_page_number }}">Next</a>
                <a href="?page={{ page_obj.paginator.num_pages }}">Last</a>
            {% endif %}
        </div>
    {% endif %}
</body>
</html>
```

---

#### 📌 **Customizing `ListView`**  

✅ **Filtering the queryset** (show only posts containing `"Django"`)  
```python
class PostListView(ListView):
    model = Post
    template_name = "posts/post_list.html"
    
    def get_queryset(self):
        return Post.objects.filter(title__icontains="Django")
```

✅ **Passing additional context data**  
```python
class PostListView(ListView):
    model = Post
    template_name = "posts/post_list.html"
    
    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context["total_posts"] = Post.objects.count()
        return context
```

---

### 🔹 **2. `DetailView` – Displaying a Single Object**  

#### 🔍 **What is `DetailView`?**  
`DetailView` is used to **display a single object** based on its primary key (`pk`).  

#### 💡 **Key Features:**  
✔️ Automatically fetches the object using `pk` from the URL.  
✔️ Uses `template_name` to specify the template.  
✔️ Passes `object` as the context variable.  

---

#### 📌 **Basic Example of `DetailView`**  

```python
from django.views.generic import DetailView
from .models import Post

class PostDetailView(DetailView):
    """Displays a single post's details."""
    model = Post
    template_name = "posts/post_detail.html"
    context_object_name = "post"
```

🔹 **URL Configuration** (`urls.py`)  
```python
urlpatterns = [
    path('posts/<int:pk>/', PostDetailView.as_view(), name='post_detail'),
]
```

🔹 **Template Example** (`templates/posts/post_detail.html`)  
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ post.title }}</title>
</head>
<body>
    <h1>{{ post.title }}</h1>
    <p>{{ post.content }}</p>
    <p>Published on: {{ post.created_at }}</p>

    <a href="{% url 'post_list' %}">Back to posts</a>
</body>
</html>
```

---

#### 📌 **Customizing `DetailView`**  

✅ **Fetching object by slug instead of `pk`**  
```python
class PostDetailView(DetailView):
    model = Post
    template_name = "posts/post_detail.html"
    slug_field = "slug"
    slug_url_kwarg = "slug"
```
🔹 **URL Configuration**  
```python
path('posts/<slug:slug>/', PostDetailView.as_view(), name='post_detail'),
```

✅ **Passing additional context data**  
```python
class PostDetailView(DetailView):
    model = Post
    template_name = "posts/post_detail.html"

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context["related_posts"] = Post.objects.exclude(id=self.object.id)[:5]
        return context
```

---

¡Vamos a seguir con la parte de **creación, actualización y eliminación de objetos** usando `CreateView`, `UpdateView` y `DeleteView`! 🚀  

---

### 🔹 **3. `CreateView` – Creating New Objects**  

#### 🔍 **What is `CreateView`?**  
`CreateView` permite crear un nuevo objeto en la base de datos y manejar formularios automáticamente.  

#### 💡 **Key Features:**  
✔️ Usa un modelo para generar automáticamente un formulario.  
✔️ Guarda automáticamente los datos del formulario en la base de datos.  
✔️ Puede personalizarse con `form_class` o `fields`.  
✔️ Redirige a una URL de éxito (`success_url`).  

---

#### 📌 **Basic Example of `CreateView`**  

```python
from django.views.generic import CreateView
from django.urls import reverse_lazy
from .models import Post

class PostCreateView(CreateView):
    """Creates a new post."""
    model = Post
    template_name = "posts/post_form.html"
    fields = ['title', 'content']  ## Fields to include in the form
    success_url = reverse_lazy('post_list')  ## Redirect after success
```

🔹 **URL Configuration** (`urls.py`)  
```python
urlpatterns = [
    path('posts/new/', PostCreateView.as_view(), name='post_create'),
]
```

🔹 **Template Example** (`templates/posts/post_form.html`)  
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Create Post</title>
</head>
<body>
    <h1>Create a New Post</h1>
    
    <form method="post">
        {% csrf_token %}
        {{ form.as_p }}
        <button type="submit">Save</button>
    </form>

    <a href="{% url 'post_list' %}">Back to posts</a>
</body>
</html>
```

---

#### 📌 **Customizing `CreateView`**  

✅ **Using a custom form instead of `fields`**  
```python
from django.views.generic import CreateView
from .models import Post
from .forms import PostForm

class PostCreateView(CreateView):
    model = Post
    form_class = PostForm  ## Using a custom form
    template_name = "posts/post_form.html"
    success_url = reverse_lazy('post_list')
```

✅ **Setting the author automatically**  
```python
class PostCreateView(CreateView):
    model = Post
    fields = ['title', 'content']
    template_name = "posts/post_form.html"
    success_url = reverse_lazy('post_list')

    def form_valid(self, form):
        form.instance.author = self.request.user  ## Assign current user
        return super().form_valid(form)
```

✅ **Adding success messages**  
```python
from django.contrib.messages.views import SuccessMessageMixin

class PostCreateView(SuccessMessageMixin, CreateView):
    model = Post
    fields = ['title', 'content']
    template_name = "posts/post_form.html"
    success_url = reverse_lazy('post_list')
    success_message = "The post was successfully created!"
```

---

### 🔹 **4. `UpdateView` – Updating Existing Objects**  

#### 🔍 **What is `UpdateView`?**  
`UpdateView` permite editar un objeto existente en la base de datos.  

#### 💡 **Key Features:**  
✔️ Busca automáticamente el objeto a editar con `pk`.  
✔️ Usa `fields` o `form_class` para generar el formulario.  
✔️ Redirige a una URL de éxito (`success_url`).  

---

#### 📌 **Basic Example of `UpdateView`**  

```python
from django.views.generic import UpdateView
from django.urls import reverse_lazy
from .models import Post

class PostUpdateView(UpdateView):
    """Updates an existing post."""
    model = Post
    template_name = "posts/post_form.html"
    fields = ['title', 'content']
    success_url = reverse_lazy('post_list')
```

🔹 **URL Configuration** (`urls.py`)  
```python
urlpatterns = [
    path('posts/<int:pk>/edit/', PostUpdateView.as_view(), name='post_update'),
]
```

✅ **Using a Custom Form**  
```python
class PostUpdateView(UpdateView):
    model = Post
    form_class = PostForm  ## Using a custom form
    template_name = "posts/post_form.html"
    success_url = reverse_lazy('post_list')
```

✅ **Restricting Edits to the Post Author**  
```python
from django.http import HttpResponseForbidden

class PostUpdateView(UpdateView):
    model = Post
    fields = ['title', 'content']
    template_name = "posts/post_form.html"
    success_url = reverse_lazy('post_list')

    def dispatch(self, request, *args, **kwargs):
        post = self.get_object()
        if post.author != request.user:
            return HttpResponseForbidden("You are not allowed to edit this post.")
        return super().dispatch(request, *args, **kwargs)
```

---

### 🔹 **5. `DeleteView` – Deleting Objects**  

#### 🔍 **What is `DeleteView`?**  
`DeleteView` permite eliminar un objeto con confirmación.  

#### 💡 **Key Features:**  
✔️ Busca automáticamente el objeto a eliminar con `pk`.  
✔️ Muestra una confirmación antes de eliminar.  
✔️ Redirige a una URL de éxito (`success_url`).  

---

#### 📌 **Basic Example of `DeleteView`**  

```python
from django.views.generic import DeleteView
from django.urls import reverse_lazy
from .models import Post

class PostDeleteView(DeleteView):
    """Deletes a post after confirmation."""
    model = Post
    template_name = "posts/post_confirm_delete.html"
    success_url = reverse_lazy('post_list')
```

🔹 **URL Configuration** (`urls.py`)  
```python
urlpatterns = [
    path('posts/<int:pk>/delete/', PostDeleteView.as_view(), name='post_delete'),
]
```

🔹 **Template Example** (`templates/posts/post_confirm_delete.html`)  
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Delete Post</title>
</head>
<body>
    <h1>Are you sure you want to delete "{{ object.title }}"?</h1>
    
    <form method="post">
        {% csrf_token %}
        <button type="submit">Yes, delete</button>
    </form>

    <a href="{% url 'post_list' %}">Cancel</a>
</body>
</html>
```

---

### ✅ **Summary**
| View | Purpose | Context Variable | Customization |
|------|---------|----------------|---------------|
| `ListView` | Display multiple objects | `object_list` | `get_queryset()`, pagination |
| `DetailView` | Display a single object | `object` | Fetch by `pk`, `slug`, extra context |
| `CreateView` | Create a new object | `form` | `form_valid()`, success messages |
| `UpdateView` | Update an object | `form`, `object` | Restrict edits to author |
| `DeleteView` | Delete an object | `object` | Add confirmation, restrict access |

---

## 📌 **Django Class-Based Views – `FormView` & `RedirectView`**  

Django proporciona dos CBVs adicionales que facilitan la gestión de formularios y redirecciones:  

1️⃣ **`FormView`** – Maneja formularios personalizados sin estar directamente ligados a un modelo.  
2️⃣ **`RedirectView`** – Redirige automáticamente a una URL sin necesidad de lógica adicional.  

---

## 🔹 **1. `FormView` – Handling Custom Forms**  

#### 🔍 **What is `FormView`?**  
`FormView` is a class-based view used when we need to process a **custom Django form** that is **not necessarily linked to a model**.  

#### 💡 **Key Features:**  
✔️ Uses `form_class` to define the form.  
✔️ Automatically validates and processes the form.  
✔️ Provides `form_valid()` and `form_invalid()` for custom logic.  
✔️ Redirects to `success_url` after successful submission.  

---

#### 📌 **Basic Example of `FormView`**  

Let's say we have a **contact form** that allows users to send a message.  

🔹 **Create a Django Form** (`forms.py`)  
```python
from django import forms

class ContactForm(forms.Form):
    """A simple contact form."""
    name = forms.CharField(max_length=100, required=True)
    email = forms.EmailField(required=True)
    message = forms.CharField(widget=forms.Textarea, required=True)
```

🔹 **Create the `FormView`** (`views.py`)  
```python
from django.views.generic.edit import FormView
from django.urls import reverse_lazy
from .forms import ContactForm

class ContactFormView(FormView):
    """Handles contact form submissions."""
    template_name = "contact.html"
    form_class = ContactForm
    success_url = reverse_lazy('thank_you')  ## Redirect after successful submission

    def form_valid(self, form):
        """Process the form data (e.g., send an email)."""
        name = form.cleaned_data['name']
        email = form.cleaned_data['email']
        message = form.cleaned_data['message']

        ## Simulate sending an email (replace with actual email sending logic)
        print(f"New message from {name} ({email}): {message}")

        return super().form_valid(form)
```

🔹 **URL Configuration** (`urls.py`)  
```python
urlpatterns = [
    path('contact/', ContactFormView.as_view(), name='contact'),
    path('thank-you/', TemplateView.as_view(template_name="thank_you.html"), name='thank_you'),
]
```

🔹 **Template Example** (`templates/contact.html`)  
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Contact Us</title>
</head>
<body>
    <h1>Contact Us</h1>
    
    <form method="post">
        {% csrf_token %}
        {{ form.as_p }}
        <button type="submit">Send</button>
    </form>
</body>
</html>
```

🔹 **Success Page (`templates/thank_you.html`)**  
```html
<h1>Thank You!</h1>
<p>Your message has been sent successfully.</p>
<a href="{% url 'contact' %}">Back to contact form</a>
```

---

#### 📌 **Customizing `FormView`**  

✅ **Handling form errors with `form_invalid()`**  
```python
def form_invalid(self, form):
    print("Form submission failed")
    return super().form_invalid(form)
```

✅ **Passing extra context to the template**  
```python
def get_context_data(self, **kwargs):
    context = super().get_context_data(**kwargs)
    context["page_title"] = "Contact Us"
    return context
```

✅ **Pre-filling form fields**  
```python
def get_initial(self):
    return {"name": "John Doe", "email": "john@example.com"}
```

---

## 🔹 **2. `RedirectView` – Handling Automatic Redirections**  

#### 🔍 **What is `RedirectView`?**  
`RedirectView` is used when we need to **redirect users to another URL** without any processing.  

#### 💡 **Key Features:**  
✔️ Redirects to a predefined URL using `url`.  
✔️ Can be temporary (`302 Found`) or permanent (`301 Moved Permanently`).  
✔️ Supports dynamic URL patterns via `pattern_name`.  
✔️ Allows query parameters forwarding (`query_string=True`).  

---

#### 📌 **Basic Example of `RedirectView`**  

🔹 **Simple Redirection to an External Site**  
```python
from django.views.generic import RedirectView

class GoogleRedirectView(RedirectView):
    """Redirects users to Google."""
    url = 'https://www.google.com'
```

🔹 **URL Configuration**  
```python
urlpatterns = [
    path('go-to-google/', GoogleRedirectView.as_view(), name='google_redirect'),
]
```

---

#### 📌 **Redirecting to a Named URL Pattern**  

We can redirect users to another **Django view** using `pattern_name`.  

🔹 **Redirect to `post_list` View**  
```python
class PostListRedirectView(RedirectView):
    """Redirects to the post list page."""
    pattern_name = 'post_list'
```

🔹 **URL Configuration**  
```python
urlpatterns = [
    path('latest-posts/', PostListRedirectView.as_view(), name='latest_posts_redirect'),
]
```

✅ **How does this work?**  
When users visit `/latest-posts/`, they will be **redirected** to the **post list page** (`/posts/`).

---

#### 📌 **Passing Query Parameters**  

By default, `RedirectView` **does not pass query parameters**, but we can enable it using `query_string=True`.  

🔹 **Example: Forward Query Parameters**  
```python
class QueryRedirectView(RedirectView):
    """Redirects to an external site and preserves query parameters."""
    url = 'https://example.com'
    query_string = True
```
🔹 **URL Configuration**  
```python
urlpatterns = [
    path('external/', QueryRedirectView.as_view(), name='query_redirect'),
]
```
✅ **How does this work?**  
If a user visits `/external/?ref=django`, they will be redirected to `https://example.com/?ref=django`.

---

#### 📌 **Permanent vs. Temporary Redirects**  

By default, `RedirectView` returns a **302 (Temporary Redirect)**. To make it permanent (`301`), set `permanent=True`.  

```python
class PermanentRedirectView(RedirectView):
    """Permanent 301 redirect."""
    url = 'https://example.com'
    permanent = True
```

---

## ✅ **Summary**
| View | Purpose | Key Features | Customization |
|------|---------|-------------|---------------|
| `FormView` | Handle custom forms | Uses `form_class`, redirects on success | `form_valid()`, `form_invalid()`, `get_initial()` |
| `RedirectView` | Redirects to a URL | Uses `url` or `pattern_name`, supports query params | `permanent=True`, `query_string=True` |

---

## 🚀 **Final Thoughts**
- Use `FormView` when you need to process **custom forms** with **validation logic**.  
- Use `RedirectView` for **simple URL redirections**, especially useful for external links or legacy URLs.  

## 📌 **How to Define Routes for Class-Based Views (CBVs) in Django**  

In Django, **Class-Based Views (CBVs)** require a specific setup in `urls.py` to be accessible in our application. In this chapter, we will cover:  

📖 **Topics covered in this section:**  
1️⃣ **Using `.as_view()` and why it is necessary**  
2️⃣ **Alternatives to `.as_view()`**  
3️⃣ **Defining routes with `path()` and `re_path()`**  

---

### 🔹 **1. Using `.as_view()` and Why It Is Necessary**  

#### 🔍 **Why do we need `.as_view()`?**  
When using **Function-Based Views (FBVs)** in Django, we define functions that receive a request (`request`) and return a response (`HttpResponse`).  

🔹 **Example of an FBV in `views.py`**  
```python
from django.http import HttpResponse

def hello_view(request):
    return HttpResponse("Hello from a function-based view!")
```
🔹 **Registering it in `urls.py`**  
```python
from django.urls import path
from .views import hello_view

urlpatterns = [
    path('hello/', hello_view, name='hello_view'),
]
```
✅ This works because Django **expects a function that takes a request and returns a response**.  

However, when using **Class-Based Views (CBVs)**, we define **a class instead of a function**.  

🔹 **Example of a CBV in `views.py`**  
```python
from django.views import View
from django.http import HttpResponse

class HelloView(View):
    def get(self, request):
        return HttpResponse("Hello from a class-based view!")
```
If we try to register this view **without `.as_view()`**, Django will throw an error:  

```python
urlpatterns = [
    path('hello/', HelloView, name='hello_view'),  ## ❌ This will NOT work!
]
```
#### 🚨 **Why does this fail?**  
- Django **expects a callable function** that takes a request and returns a response.  
- A **class is not callable in this way**—it must be instantiated.  
- `.as_view()` is required to **convert the class into a callable function**.  

#### ✅ **Correct Way: Using `.as_view()`**
```python
urlpatterns = [
    path('hello/', HelloView.as_view(), name='hello_view'),
]
```
Now, Django correctly **instantiates the class and calls the appropriate method (`get()`, `post()`, etc.)**.

---

### 🔹 **2. Alternatives to `.as_view()`**  

While `.as_view()` is the standard way to use CBVs, there are some **alternative methods**, though they are **rarely recommended**.

#### ✅ **Manually Instantiating the Class**
We can manually instantiate the view class and call its `dispatch()` method:  
```python
def manual_hello_view(request):
    view = HelloView()
    return view.dispatch(request)
```
🔹 **Registering it in `urls.py`**  
```python
urlpatterns = [
    path('hello/', manual_hello_view, name='hello_view'),
]
```
⚠️ **Why is this not recommended?**  
- It **bypasses Django's built-in request handling**.  
- It **does not support middleware and mixins correctly**.  
- It is **less readable and harder to maintain**.  

---

### 🔹 **3. Defining Routes with `path()` and `re_path()`**  

Django provides two main functions to define URL patterns:  

| Function | Usage | Example |
|----------|-------|---------|
| `path()` | Uses **simple string patterns** (recommended) | `path('posts/', PostListView.as_view(), name='post_list')` |
| `re_path()` | Uses **regular expressions** (for advanced cases) | `re_path(r'^posts/(?P<pk>\d+)/$', PostDetailView.as_view(), name='post_detail')` |

#### ✅ **Using `path()` for simple URL patterns**  
`path()` is the recommended way to define URLs in Django.  
```python
from django.urls import path
from .views import PostListView, PostDetailView

urlpatterns = [
    path('posts/', PostListView.as_view(), name='post_list'),
    path('posts/<int:pk>/', PostDetailView.as_view(), name='post_detail'),
]
```
**Dynamic URL Parameters with `path()`**  
| Type | Example | Matches |
|------|---------|---------|
| `<int:pk>` | `posts/5/` | Numbers only |
| `<str:slug>` | `posts/my-first-post/` | Any string |
| `<uuid:id>` | `posts/550e8400-e29b-41d4-a716-446655440000/` | UUIDs |

---

#### ✅ **Using `re_path()` for advanced patterns**  
`re_path()` allows **regular expressions** for complex URL patterns.  

Example: Supporting both numeric IDs (`posts/5/`) and slugs (`posts/my-first-post/`):  
```python
from django.urls import re_path
from .views import PostDetailView

urlpatterns = [
    re_path(r'^posts/(?P<identifier>[\w-]+)/$', PostDetailView.as_view(), name='post_detail'),
]
```
🔹 **Matches both:**  
- `/posts/5/`  
- `/posts/my-first-post/`  

⚠️ **When should you use `re_path()`?**  
- When you need **more complex patterns** than `path()` allows.  
- When migrating from **legacy URL structures** that use regex.  

🔹 **Most cases should use `path()` because it is easier to read and maintain.**  

---

### ✅ **Summary**
| Feature | `path()` | `re_path()` |
|---------|---------|-------------|
| Readability | ✅ Easy to read | ❌ Harder to understand |
| Performance | ✅ Faster | ⚠️ Slightly slower |
| Regex Support | ❌ No | ✅ Yes |
| Best for | Simple URLs | Complex patterns |

---

## 🚀 **Final Thoughts**
- ✅ **Always use `.as_view()`** when defining CBVs in `urls.py`.  
- ✅ **Use `path()`** for most URLs—it is simple and readable.  
- ✅ **Use `re_path()` only when you need regex support.**  

---

## 📌 **Using Mixins in Class-Based Views (CBVs)**  

**Mixins** are reusable classes that add functionality to Django’s **Class-Based Views (CBVs)**. Instead of duplicating code across multiple views, mixins allow us to extend behavior in a modular way.  

In this section, we will cover different types of mixins and how to use them.  

---

### 📖 **Mixin Overview Table**  

| **Mixin** | **Description** | **Notes** |
|-----------|---------------|-----------|
| `LoginRequiredMixin` | Restricts access to authenticated users only. | Redirects to `LOGIN_URL` if the user is not authenticated. |
| `PermissionRequiredMixin` | Ensures the user has specific permissions to access the view. | Requires `permission_required` to be defined. |
| `UserPassesTestMixin` | Allows custom validation logic to determine if a user can access a view. | Requires overriding `test_func()`. |
| `FormMixin` | Allows the use of a form in a non-form view (e.g., `DetailView`). | Useful when combining a form with another view type. |
| `SuccessMessageMixin` | Adds success messages when a form is submitted successfully. | Requires Django's messages framework. |
| `PaginatorMixin` | Enables pagination in `ListView`. | Uses `paginate_by` to set the number of items per page. |
| `RedirectView` | Redirects users to another URL. | Can be permanent (`301`) or temporary (`302`). |

---

### 🔹 **1. Authentication & Permission Mixins**  

Django provides built-in mixins for handling **user authentication and permissions**.  

#### ✅ **`LoginRequiredMixin` – Restricting Access to Authenticated Users**  

This mixin **ensures that only authenticated users can access the view**. If the user is not logged in, they are redirected to the `LOGIN_URL` (default: `/accounts/login/`).  

🔹 **Example Usage in `views.py`**  
```python
from django.contrib.auth.mixins import LoginRequiredMixin
from django.views.generic import ListView
from .models import Post

class SecurePostListView(LoginRequiredMixin, ListView):
    """Only authenticated users can access this view."""
    model = Post
    template_name = "posts/secure_post_list.html"
    login_url = "/login/"  ## Custom login page (optional)
```
🔹 **Customizing the Redirect URL**  
```python
class CustomLoginView(LoginRequiredMixin, ListView):
    login_url = "/custom-login/"
    redirect_field_name = "next"  ## Where to redirect after login
```

---

#### ✅ **`PermissionRequiredMixin` – Restricting Access Based on Permissions**  

This mixin **checks if the user has specific permissions** before granting access.  

🔹 **Example Usage**  
```python
from django.contrib.auth.mixins import PermissionRequiredMixin
from django.views.generic import ListView
from .models import Post

class AdminPostListView(PermissionRequiredMixin, ListView):
    """Only users with 'posts.view_post' permission can access this view."""
    model = Post
    template_name = "posts/admin_post_list.html"
    permission_required = "posts.view_post"
```
🔹 **Multiple Permissions**  
```python
permission_required = ["posts.view_post", "posts.change_post"]
```

---

#### ✅ **`UserPassesTestMixin` – Custom Access Control**  

This mixin **allows us to define custom logic** to determine whether a user can access a view.  

🔹 **Example: Only the Post Author Can Access the View**  
```python
from django.contrib.auth.mixins import UserPassesTestMixin
from django.views.generic import DetailView
from .models import Post

class PostOwnerView(UserPassesTestMixin, DetailView):
    """Only the post author can view this page."""
    model = Post
    template_name = "posts/post_detail.html"

    def test_func(self):
        """Check if the current user is the author of the post."""
        post = self.get_object()
        return self.request.user == post.author
```

---

### 🔹 **2. Form Handling Mixins**  

#### ✅ **`FormMixin` – Using Forms Outside `FormView`**  

`FormMixin` allows us to **combine forms with non-form views**.  

🔹 **Example: Adding a Comment Form to a `DetailView`**  
```python
from django.views.generic import DetailView
from django.views.generic.edit import FormMixin
from .models import Post
from .forms import CommentForm

class PostDetailView(FormMixin, DetailView):
    """Displays a post and includes a comment form."""
    model = Post
    template_name = "posts/post_detail.html"
    form_class = CommentForm

    def get_success_url(self):
        """Redirect to the same post after submitting the form."""
        return self.request.path
```

---

#### ✅ **`SuccessMessageMixin` – Displaying Success Messages**  

This mixin **adds a success message when a form is submitted successfully**.  

🔹 **Example Usage**  
```python
from django.contrib.messages.views import SuccessMessageMixin
from django.views.generic.edit import CreateView
from .models import Post

class PostCreateView(SuccessMessageMixin, CreateView):
    """Displays a success message after creating a post."""
    model = Post
    fields = ["title", "content"]
    template_name = "posts/post_form.html"
    success_url = "/posts/"
    success_message = "The post has been created successfully!"
```

---

### 🔹 **3. Pagination Mixins**  

#### ✅ **`PaginatorMixin` – Enabling Pagination in `ListView`**  

This mixin allows us to **paginate lists of objects** with minimal configuration.  

🔹 **Example Usage**  
```python
from django.views.generic import ListView
from .models import Post

class PaginatedPostListView(ListView):
    """Displays a paginated list of posts."""
    model = Post
    template_name = "posts/post_list.html"
    paginate_by = 5  ## Show 5 posts per page
```
🔹 **Adding Pagination Controls in the Template**  
```html
{% if is_paginated %}
    {% if page_obj.has_previous %}
        <a href="?page=1">First</a>
        <a href="?page={{ page_obj.previous_page_number }}">Previous</a>
    {% endif %}

    Page {{ page_obj.number }} of {{ page_obj.paginator.num_pages }}

    {% if page_obj.has_next %}
        <a href="?page={{ page_obj.next_page_number }}">Next</a>
        <a href="?page={{ page_obj.paginator.num_pages }}">Last</a>
    {% endif %}
{% endif %}
```

---

### 🔹 **4. Redirection Mixins**  

#### ✅ **`RedirectView` – Handling Automatic Redirections**  

This mixin allows us to **redirect users to another URL without extra processing**.  

🔹 **Example: Redirecting to Google**  
```python
from django.views.generic import RedirectView

class GoogleRedirectView(RedirectView):
    """Redirects users to Google."""
    url = "https://www.google.com"
```
🔹 **Registering in `urls.py`**  
```python
urlpatterns = [
    path("go-to-google/", GoogleRedirectView.as_view(), name="google_redirect"),
]
```

🔹 **Redirecting to a Named URL**  
```python
class HomeRedirectView(RedirectView):
    """Redirects to the home page using a named URL."""
    pattern_name = "home"
```

🔹 **Preserving Query Parameters**  
```python
class QueryRedirectView(RedirectView):
    url = "https://example.com"
    query_string = True  ## Preserve ?params=values in the redirect
```

---

### ✅ **Summary Table**  

| **Mixin** | **Purpose** | **Example Use Case** |
|-----------|------------|----------------------|
| `LoginRequiredMixin` | Restrict access to logged-in users | Secure a `ListView` |
| `PermissionRequiredMixin` | Restrict access based on user permissions | Admin-only views |
| `UserPassesTestMixin` | Apply custom access rules | Limit access to post owners |
| `FormMixin` | Use a form in a non-form view | Add a form to `DetailView` |
| `SuccessMessageMixin` | Show a success message after form submission | `CreateView` success message |
| `PaginatorMixin` | Paginate object lists | Paginate a blog post list |
| `RedirectView` | Redirect users to another page | Redirect to Google |

---

## 🚀 **Advanced Custom Mixins for Django Class-Based Views (CBVs)**  

Django provides many built-in mixins, but sometimes we need **custom mixins** to add specific functionalities to our views. In this section, we will create reusable mixins for:  

📖 **Topics covered:**  
1️⃣ **Custom Authentication Mixins**  
   - `SuperUserRequiredMixin` – Restrict access to superusers.  
   - `StaffRequiredMixin` – Restrict access to staff users.  
2️⃣ **Custom Logging & Analytics Mixins**  
   - `LoggingMixin` – Log every request made to a view.  
   - `AnalyticsMixin` – Track user visits to a view.  
3️⃣ **Custom Form Processing Mixins**  
   - `AutoPopulateUserMixin` – Automatically assign the logged-in user to a model.  
   - `SuccessRedirectMixin` – Redirect dynamically after form submission.  
4️⃣ **Custom QuerySet & Data Filtering Mixins**  
   - `FilterByUserMixin` – Show only objects owned by the logged-in user.  
   - `SearchMixin` – Add search functionality to `ListView`.  

---

### 🔹 **1. Custom Authentication Mixins**  

#### ✅ **`SuperUserRequiredMixin` – Restrict Access to Superusers**  
This mixin **only allows superusers to access a view**.  

🔹 **Implementation**  
```python
from django.http import HttpResponseForbidden
from django.contrib.auth.mixins import AccessMixin

class SuperUserRequiredMixin(AccessMixin):
    """Allows access only to superusers."""
    
    def dispatch(self, request, *args, **kwargs):
        if not request.user.is_superuser:
            return HttpResponseForbidden("Access denied. Superuser required.")
        return super().dispatch(request, *args, **kwargs)
```
🔹 **Usage Example**  
```python
class AdminDashboardView(SuperUserRequiredMixin, TemplateView):
    template_name = "admin_dashboard.html"
```

---

#### ✅ **`StaffRequiredMixin` – Restrict Access to Staff Users**  
This mixin **only allows users with `is_staff=True` to access a view**.  

🔹 **Implementation**  
```python
class StaffRequiredMixin(AccessMixin):
    """Allows access only to staff members."""
    
    def dispatch(self, request, *args, **kwargs):
        if not request.user.is_staff:
            return HttpResponseForbidden("Access denied. Staff only.")
        return super().dispatch(request, *args, **kwargs)
```
🔹 **Usage Example**  
```python
class StaffOnlyView(StaffRequiredMixin, TemplateView):
    template_name = "staff_page.html"
```

---

### 🔹 **2. Custom Logging & Analytics Mixins**  

#### ✅ **`LoggingMixin` – Log Every Request**  
This mixin **logs every request made to a view**, useful for debugging.  

🔹 **Implementation**  
```python
import logging

logger = logging.getLogger(__name__)

class LoggingMixin:
    """Logs every request to the view."""

    def dispatch(self, request, *args, **kwargs):
        logger.info(f"Request received: {request.method} {request.path} by {request.user}")
        return super().dispatch(request, *args, **kwargs)
```
🔹 **Usage Example**  
```python
class LoggedListView(LoggingMixin, ListView):
    model = Post
    template_name = "posts/logged_list.html"
```

---

#### ✅ **`AnalyticsMixin` – Track Page Visits**  
This mixin **records each visit to a database model for analytics**.  

🔹 **Implementation**  
```python
from django.db import models
from django.utils.timezone import now

class PageVisit(models.Model):
    """Stores user visit analytics."""
    user = models.ForeignKey("auth.User", on_delete=models.CASCADE, null=True, blank=True)
    url = models.CharField(max_length=255)
    timestamp = models.DateTimeField(default=now)

class AnalyticsMixin:
    """Records page visits for analytics."""
    
    def dispatch(self, request, *args, **kwargs):
        PageVisit.objects.create(user=request.user if request.user.is_authenticated else None, url=request.path)
        return super().dispatch(request, *args, **kwargs)
```
🔹 **Usage Example**  
```python
class TrackedPageView(AnalyticsMixin, TemplateView):
    template_name = "tracked_page.html"
```

---

### 🔹 **3. Custom Form Processing Mixins**  

#### ✅ **`AutoPopulateUserMixin` – Assign Logged-in User to a Model**  
This mixin **automatically assigns the logged-in user to the model instance** before saving.  

🔹 **Implementation**  
```python
class AutoPopulateUserMixin:
    """Automatically assigns the logged-in user to a model instance."""

    def form_valid(self, form):
        form.instance.user = self.request.user
        return super().form_valid(form)
```
🔹 **Usage Example**  
```python
class PostCreateView(AutoPopulateUserMixin, CreateView):
    model = Post
    fields = ["title", "content"]
    template_name = "posts/post_form.html"
    success_url = "/posts/"
```

---

#### ✅ **`SuccessRedirectMixin` – Redirect Dynamically After Form Submission**  
This mixin **redirects users dynamically based on their role or form input**.  

🔹 **Implementation**  
```python
from django.shortcuts import redirect

class SuccessRedirectMixin:
    """Redirects users dynamically after form submission."""

    def get_success_url(self):
        if self.request.user.is_staff:
            return "/staff-dashboard/"
        return "/user-dashboard/"
```
🔹 **Usage Example**  
```python
class ProfileUpdateView(SuccessRedirectMixin, UpdateView):
    model = Profile
    fields = ["bio", "avatar"]
    template_name = "profile_form.html"
```

---

### 🔹 **4. Custom QuerySet & Data Filtering Mixins**  

#### ✅ **`FilterByUserMixin` – Show Only Objects Owned by the User**  
This mixin **filters a queryset to only show objects belonging to the logged-in user**.  

🔹 **Implementation**  
```python
class FilterByUserMixin:
    """Filters queryset to show only objects belonging to the logged-in user."""

    def get_queryset(self):
        return super().get_queryset().filter(user=self.request.user)
```
🔹 **Usage Example**  
```python
class UserPostsView(FilterByUserMixin, ListView):
    model = Post
    template_name = "posts/user_posts.html"
```

---

#### ✅ **`SearchMixin` – Add Search Functionality to `ListView`**  
This mixin **adds search functionality to `ListView`**, filtering results based on a query parameter.  

🔹 **Implementation**  
```python
class SearchMixin:
    """Adds search functionality to ListView."""

    search_param = "q"
    search_fields = []

    def get_queryset(self):
        queryset = super().get_queryset()
        search_query = self.request.GET.get(self.search_param, "")
        if search_query:
            from django.db.models import Q
            query_filter = Q()
            for field in self.search_fields:
                query_filter |= Q(**{f"{field}__icontains": search_query})
            queryset = queryset.filter(query_filter)
        return queryset
```
🔹 **Usage Example**  
```python
class SearchablePostListView(SearchMixin, ListView):
    model = Post
    template_name = "posts/search_results.html"
    search_fields = ["title", "content"]
```
🔹 **Template Example**  
```html
<form method="get">
    <input type="text" name="q" placeholder="Search...">
    <button type="submit">Search</button>
</form>
<ul>
    {% for post in object_list %}
        <li>{{ post.title }}</li>
    {% empty %}
        <li>No results found.</li>
    {% endfor %}
</ul>
```

---

## 🚀 **Advanced Django Mixins – API Views, Caching, and Performance Optimization**  

In this section, we will explore **advanced custom mixins** to enhance Django’s **Class-Based Views (CBVs)**. These mixins improve API handling, caching, performance, and response customization.  

📖 **Topics covered:**  
1️⃣ **Custom API Response Mixins**  
   - `JsonResponseMixin` – Return JSON responses from CBVs.  
   - `AjaxRequiredMixin` – Restrict access to AJAX requests only.  
2️⃣ **Custom Caching & Performance Mixins**  
   - `CacheMixin` – Cache view responses for faster loading.  
   - `QueryCountDebugMixin` – Log database queries for performance analysis.  
3️⃣ **Custom Throttling & Rate-Limiting Mixins**  
   - `ThrottleMixin` – Limit the number of requests per user.  
   - `UserActivityMixin` – Track the last request made by a user.  
4️⃣ **Custom Security Mixins**  
   - `HoneypotMixin` – Prevent spam bot submissions.  
   - `BlockIPMixin` – Block specific IP addresses from accessing a view.  

---

### 🔹 **1. Custom API Response Mixins**  

#### ✅ **`JsonResponseMixin` – Return JSON Responses**  
This mixin **allows CBVs to return JSON responses instead of HTML**.  

🔹 **Implementation**  
```python
from django.http import JsonResponse

class JsonResponseMixin:
    """Returns JSON responses instead of rendering templates."""

    def render_to_response(self, context, **response_kwargs):
        return JsonResponse(context)
```
🔹 **Usage Example**  
```python
class PostJsonListView(JsonResponseMixin, ListView):
    model = Post

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context["posts"] = list(self.get_queryset().values("id", "title", "content"))
        return context
```
🔹 **Requesting `/posts/json/` returns:**  
```json
{
    "posts": [
        {"id": 1, "title": "Django CBVs", "content": "Learn Django CBVs"},
        {"id": 2, "title": "Mixins", "content": "Learn about mixins"}
    ]
}
```

---

#### ✅ **`AjaxRequiredMixin` – Restrict Access to AJAX Requests**  
This mixin **ensures that a view can only be accessed via an AJAX request**.  

🔹 **Implementation**  
```python
from django.http import JsonResponse

class AjaxRequiredMixin:
    """Allows access only if the request is an AJAX request."""

    def dispatch(self, request, *args, **kwargs):
        if not request.headers.get("X-Requested-With") == "XMLHttpRequest":
            return JsonResponse({"error": "AJAX request required"}, status=400)
        return super().dispatch(request, *args, **kwargs)
```
🔹 **Usage Example**  
```python
class AjaxPostListView(AjaxRequiredMixin, JsonResponseMixin, ListView):
    model = Post
```

---

### 🔹 **2. Custom Caching & Performance Mixins**  

#### ✅ **`CacheMixin` – Cache View Responses**  
This mixin **caches the response of a view for a set period** to improve performance.  

🔹 **Implementation**  
```python
from django.utils.decorators import method_decorator
from django.views.decorators.cache import cache_page

class CacheMixin:
    """Caches the response of a view for a specified duration."""

    cache_timeout = 60  ## Cache duration in seconds

    def dispatch(self, *args, **kwargs):
        @method_decorator(cache_page(self.cache_timeout))
        def wrapped_dispatch(request, *args, **kwargs):
            return super(self.__class__, self).dispatch(request, *args, **kwargs)

        return wrapped_dispatch(*args, **kwargs)
```
🔹 **Usage Example**  
```python
class CachedPostListView(CacheMixin, ListView):
    model = Post
    cache_timeout = 120  ## Cache for 2 minutes
```

---

#### ✅ **`QueryCountDebugMixin` – Log Database Queries**  
This mixin **logs the number of queries executed during a request** for performance analysis.  

🔹 **Implementation**  
```python
from django.db import connection

class QueryCountDebugMixin:
    """Logs the number of database queries executed."""

    def dispatch(self, request, *args, **kwargs):
        response = super().dispatch(request, *args, **kwargs)
        query_count = len(connection.queries)
        print(f"Total queries executed: {query_count}")
        return response
```
🔹 **Usage Example**  
```python
class DebugPostListView(QueryCountDebugMixin, ListView):
    model = Post
```

---

### 🔹 **3. Custom Throttling & Rate-Limiting Mixins**  

#### ✅ **`ThrottleMixin` – Limit Requests per User**  
This mixin **prevents users from making too many requests within a short time**.  

🔹 **Implementation**  
```python
import time
from django.core.cache import cache
from django.http import JsonResponse

class ThrottleMixin:
    """Limits requests per user to prevent abuse."""
    
    rate_limit = 5  ## Requests allowed per time window
    time_window = 60  ## Time window in seconds

    def dispatch(self, request, *args, **kwargs):
        user_ip = request.META.get("REMOTE_ADDR", "unknown")
        cache_key = f"throttle_{user_ip}"
        request_count = cache.get(cache_key, 0)

        if request_count >= self.rate_limit:
            return JsonResponse({"error": "Too many requests"}, status=429)

        cache.set(cache_key, request_count + 1, timeout=self.time_window)
        return super().dispatch(request, *args, **kwargs)
```
🔹 **Usage Example**  
```python
class ThrottledPostListView(ThrottleMixin, ListView):
    model = Post
```

---

#### ✅ **`UserActivityMixin` – Track Last Request**  
This mixin **records when a user last accessed a view**.  

🔹 **Implementation**  
```python
from django.utils.timezone import now

class UserActivityMixin:
    """Tracks the last request time of authenticated users."""

    def dispatch(self, request, *args, **kwargs):
        if request.user.is_authenticated:
            request.user.profile.last_seen = now()
            request.user.profile.save()
        return super().dispatch(request, *args, **kwargs)
```
🔹 **Usage Example**  
```python
class TrackedPostListView(UserActivityMixin, ListView):
    model = Post
```

---

### 🔹 **4. Custom Security Mixins**  

#### ✅ **`HoneypotMixin` – Prevent Spam Submissions**  
This mixin **adds a hidden field to forms to trap bots**.  

🔹 **Implementation**  
```python
class HoneypotMixin:
    """Prevents spam by adding a hidden honeypot field."""

    honeypot_field_name = "honeypot"

    def get_form_kwargs(self):
        kwargs = super().get_form_kwargs()
        kwargs["initial"][self.honeypot_field_name] = ""
        return kwargs

    def form_valid(self, form):
        if form.cleaned_data.get(self.honeypot_field_name):
            return JsonResponse({"error": "Spam detected"}, status=400)
        return super().form_valid(form)
```
🔹 **Usage Example**  
```python
class SecureContactView(HoneypotMixin, FormView):
    form_class = ContactForm
    template_name = "contact.html"
```
🔹 **Template Example**  
```html
<input type="text" name="honeypot" style="display:none;">
```

---

#### ✅ **`BlockIPMixin` – Block Specific IP Addresses**  
This mixin **blocks requests from specific IP addresses**.  

🔹 **Implementation**  
```python
from django.http import HttpResponseForbidden

class BlockIPMixin:
    """Blocks specific IP addresses from accessing a view."""

    blocked_ips = ["192.168.1.1", "10.0.0.1"]

    def dispatch(self, request, *args, **kwargs):
        user_ip = request.META.get("REMOTE_ADDR", "")
        if user_ip in self.blocked_ips:
            return HttpResponseForbidden("Your IP is blocked.")
        return super().dispatch(request, *args, **kwargs)
```
🔹 **Usage Example**  
```python
class SecurePageView(BlockIPMixin, TemplateView):
    template_name = "secure_page.html"
```

---

## 🚀 **Practical Examples: Advanced Class-Based Views (CBVs) in Django**  

Now that we have covered Django's **Class-Based Views (CBVs)** in depth, let’s put everything into practice! In this section, we will implement a **fully functional CRUD** with CBVs, add **authentication with `LoginRequiredMixin`**, and explore **advanced customization using mixins**.  

📖 **Topics covered in this section:**  
1️⃣ **Full CRUD implementation with CBVs**  
2️⃣ **Adding authentication with `LoginRequiredMixin`**  
3️⃣ **Advanced customization using custom mixins**  

---

### 🔹 **1. Full CRUD Implementation with CBVs**  

#### 🔍 **Project Overview**  
We will build a **blog system** where users can:  
✅ View all blog posts (`ListView`)  
✅ View a single blog post (`DetailView`)  
✅ Create new blog posts (`CreateView`)  
✅ Edit existing blog posts (`UpdateView`)  
✅ Delete blog posts (`DeleteView`)  

#### ✅ **Step 1: Create the `Post` Model**  
🔹 **`models.py`**  
```python
from django.db import models
from django.contrib.auth.models import User
from django.urls import reverse

class Post(models.Model):
    """Blog post model."""
    title = models.CharField(max_length=200)
    content = models.TextField()
    author = models.ForeignKey(User, on_delete=models.CASCADE)
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.title

    def get_absolute_url(self):
        return reverse('post_detail', kwargs={'pk': self.pk})
```

---

#### ✅ **Step 2: Create the Views using CBVs**  

🔹 **`views.py`**  
```python
from django.views.generic import ListView, DetailView, CreateView, UpdateView, DeleteView
from django.urls import reverse_lazy
from .models import Post

## List all posts
class PostListView(ListView):
    model = Post
    template_name = "posts/post_list.html"
    context_object_name = "posts"
    ordering = ['-created_at']
    paginate_by = 5  ## Enable pagination

## View a single post
class PostDetailView(DetailView):
    model = Post
    template_name = "posts/post_detail.html"

## Create a new post
class PostCreateView(CreateView):
    model = Post
    fields = ['title', 'content']
    template_name = "posts/post_form.html"
    success_url = reverse_lazy('post_list')

    def form_valid(self, form):
        form.instance.author = self.request.user  ## Assign logged-in user as author
        return super().form_valid(form)

## Update an existing post
class PostUpdateView(UpdateView):
    model = Post
    fields = ['title', 'content']
    template_name = "posts/post_form.html"

    def get_queryset(self):
        return self.model.objects.filter(author=self.request.user)  ## Restrict updates to author

## Delete a post
class PostDeleteView(DeleteView):
    model = Post
    template_name = "posts/post_confirm_delete.html"
    success_url = reverse_lazy('post_list')

    def get_queryset(self):
        return self.model.objects.filter(author=self.request.user)  ## Restrict deletions to author
```

---

#### ✅ **Step 3: Define Routes in `urls.py`**  
🔹 **`urls.py`**  
```python
from django.urls import path
from .views import PostListView, PostDetailView, PostCreateView, PostUpdateView, PostDeleteView

urlpatterns = [
    path('', PostListView.as_view(), name='post_list'),
    path('post/<int:pk>/', PostDetailView.as_view(), name='post_detail'),
    path('post/new/', PostCreateView.as_view(), name='post_create'),
    path('post/<int:pk>/edit/', PostUpdateView.as_view(), name='post_update'),
    path('post/<int:pk>/delete/', PostDeleteView.as_view(), name='post_delete'),
]
```

---

#### ✅ **Step 4: Create the Templates**  

🔹 **`templates/posts/post_list.html`**  
```html
<h1>Blog Posts</h1>
<a href="{% url 'post_create' %}">Create New Post</a>

<ul>
    {% for post in posts %}
        <li>
            <a href="{% url 'post_detail' post.pk %}">{{ post.title }}</a>
            <p>By {{ post.author }} | {{ post.created_at }}</p>
            {% if post.author == request.user %}
                <a href="{% url 'post_update' post.pk %}">Edit</a>
                <a href="{% url 'post_delete' post.pk %}">Delete</a>
            {% endif %}
        </li>
    {% empty %}
        <li>No posts yet.</li>
    {% endfor %}
</ul>
```

🔹 **Pagination (add this to `post_list.html`)**  
```html
{% if is_paginated %}
    {% if page_obj.has_previous %}
        <a href="?page=1">First</a>
        <a href="?page={{ page_obj.previous_page_number }}">Previous</a>
    {% endif %}

    Page {{ page_obj.number }} of {{ page_obj.paginator.num_pages }}

    {% if page_obj.has_next %}
        <a href="?page={{ page_obj.next_page_number }}">Next</a>
        <a href="?page={{ page_obj.paginator.num_pages }}">Last</a>
    {% endif %}
{% endif %}
```

---

### 🔹 **2. Adding Authentication with `LoginRequiredMixin`**  

We can **restrict access** so that only **logged-in users** can create, update, or delete posts.  

🔹 **Update `views.py`**  
```python
from django.contrib.auth.mixins import LoginRequiredMixin

class PostCreateView(LoginRequiredMixin, CreateView):
    model = Post
    fields = ['title', 'content']
    template_name = "posts/post_form.html"
    success_url = reverse_lazy('post_list')
    login_url = '/login/'  ## Redirect if not authenticated
```
Apply `LoginRequiredMixin` to other views (`UpdateView`, `DeleteView`):  
```python
class PostUpdateView(LoginRequiredMixin, UpdateView):
    model = Post
    fields = ['title', 'content']
    template_name = "posts/post_form.html"

class PostDeleteView(LoginRequiredMixin, DeleteView):
    model = Post
    template_name = "posts/post_confirm_delete.html"
    success_url = reverse_lazy('post_list')
```

---

### 🔹 **3. Advanced CBV Customization with Custom Mixins**  

Let’s **create a mixin** that ensures only **the author** can update or delete a post.  

🔹 **Custom Mixin in `mixins.py`**  
```python
from django.http import HttpResponseForbidden

class AuthorRequiredMixin:
    """Ensures only the author can update or delete a post."""

    def dispatch(self, request, *args, **kwargs):
        obj = self.get_object()
        if obj.author != request.user:
            return HttpResponseForbidden("You are not the author of this post.")
        return super().dispatch(request, *args, **kwargs)
```

🔹 **Apply the mixin to `UpdateView` and `DeleteView`**  
```python
class PostUpdateView(LoginRequiredMixin, AuthorRequiredMixin, UpdateView):
    model = Post
    fields = ['title', 'content']
    template_name = "posts/post_form.html"

class PostDeleteView(LoginRequiredMixin, AuthorRequiredMixin, DeleteView):
    model = Post
    template_name = "posts/post_confirm_delete.html"
    success_url = reverse_lazy('post_list')
```

---

## ✅ **Summary**  

| **Feature** | **Implementation** |
|------------|--------------------|
| Full CRUD with CBVs | `ListView`, `DetailView`, `CreateView`, `UpdateView`, `DeleteView` |
| Authentication | `LoginRequiredMixin` |
| Custom Access Control | `AuthorRequiredMixin` |
| Pagination | `paginate_by = 5` |

---
