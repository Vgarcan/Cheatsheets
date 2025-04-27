# Job Search Portal Development Guide

## Table of Contents

- [Job Search Portal Development Guide](#job-search-portal-development-guide)
  - [Table of Contents](#table-of-contents)
  - [1. Introduction](#1-introduction)
  - [2. Prerequisites](#2-prerequisites)
  - [3. Project Setup](#3-project-setup)
  - [4. Database Design](#4-database-design)
    - [Users Collection](#users-collection)
    - [Jobs Collection](#jobs-collection)
  - [5. Flask Application Setup](#5-flask-application-setup)
  - [6. User Authentication](#6-user-authentication)
    - [User Model](#user-model)
    - [Login and Registration Forms](#login-and-registration-forms)
    - [Login and Logout Routes](#login-and-logout-routes)
  - [7. Job Management](#7-job-management)
    - [Job Posting Model](#job-posting-model)
    - [Job Posting Form](#job-posting-form)
    - [Job Routes](#job-routes)
  - [8. Templates and Frontend](#8-templates-and-frontend)
    - [`base.html`](#basehtml)
    - [`index.html`](#indexhtml)
    - [`login.html`](#loginhtml)
    - [`register.html`](#registerhtml)
    - [`post_job.html`](#post_jobhtml)
  - [9. Running the Application](#9-running-the-application)
    - [Summary](#summary)


## 1. Introduction

In this guide, we will build a job search portal using Flask. We will use MongoDB for our database, Flask-WTF for form handling, Flask-Login for user authentication, and Flask-PyMongo for integrating Flask with MongoDB.

## 2. Prerequisites

- Basic knowledge of Python and Flask
- MongoDB installed and running
- Python environment set up with Flask installed

## 3. Project Setup

First, install the necessary packages:

```bash
pip install Flask Flask-WTF Flask-Login Flask-PyMongo
```

Next, create the directory structure for your project:

```
job_portal/
    ├── app.py
    ├── templates/
    │   ├── base.html
    │   ├── index.html
    │   ├── login.html
    │   ├── register.html
    │   ├── post_job.html
    ├── static/
    │   └── style.css
```

## 4. Database Design

We will design the following collections in MongoDB:

1. **Users** - To store user information.
2. **Jobs** - To store job postings.

### Users Collection

```json
{
  "_id": ObjectId("..."),
  "username": "john_doe",
  "email": "john@example.com",
  "password_hash": "hashed_password",
  "role": "candidate",  // or "recruiter"
  "profile": {
    "full_name": "John Doe",
    "contact_number": "123-456-7890",
    "resume_url": "http://example.com/resume.pdf",
    "skills": ["Python", "Flask", "MongoDB"],
    "experience": [
      {
        "job_title": "Software Developer",
        "company": "Tech Solutions",
        "start_date": "2020-01-01",
        "end_date": "2022-01-01",
        "description": "Developed web applications using Flask and MongoDB."
      }
    ],
    "education": [
      {
        "institution": "University of Example",
        "degree": "BSc in Computer Science",
        "start_date": "2015-09-01",
        "end_date": "2019-06-01"
      }
    ]
  }
}
```

### Jobs Collection

```json
{
  "_id": ObjectId("..."),
  "title": "Backend Developer",
  "company_name": "Tech Solutions",
  "description": "Looking for an experienced backend developer...",
  "requirements": ["Python", "Flask", "API development"],
  "location": "Remote",
  "salary_range": "50000-70000",
  "date_posted": ISODate("2024-06-01T00:00:00Z"),
  "application_deadline": ISODate("2024-07-01T00:00:00Z"),
  "posted_by": ObjectId("...")  // Reference to a user (recruiter)
}
```

## 5. Flask Application Setup

Create the `app.py` file and set up the Flask application with Flask-PyMongo and Flask-Login:

```python
# app.py
from flask import Flask, render_template, redirect, url_for, flash
from flask_wtf import FlaskForm
from wtforms import StringField, PasswordField, SubmitField, TextAreaField
from wtforms.validators import DataRequired, Email, EqualTo
from flask_login import LoginManager, UserMixin, login_user, login_required, logout_user, current_user
from flask_pymongo import PyMongo
from bson.objectid import ObjectId

app = Flask(__name__)
app.config['SECRET_KEY'] = 'your_secret_key_here'  # Change this to a secure secret key
app.config['MONGO_URI'] = 'mongodb://localhost:27017/job_portal_db'  # MongoDB URI

mongo = PyMongo(app)
login_manager = LoginManager(app)
login_manager.login_view = 'login'

# Define User model for Flask-Login
class User(UserMixin):
    def __init__(self, user_id):
        self.id = user_id

@login_manager.user_loader
def load_user(user_id):
    return User(user_id)

# Routes
@app.route('/')
def index():
    jobs = mongo.db.jobs.find()
    return render_template('index.html', jobs=jobs)

if __name__ == '__main__':
    app.run(debug=True)
```

## 6. User Authentication

### User Model

We'll use MongoDB to store user information and define a user model for Flask-Login.

### Login and Registration Forms

Create forms for user login and registration using Flask-WTF.

```python
# app.py (continued)

# Flask-WTF forms
class LoginForm(FlaskForm):
    username = StringField('Username', validators=[DataRequired()])
    password = PasswordField('Password', validators=[DataRequired()])
    submit = SubmitField('Login')

class RegisterForm(FlaskForm):
    username = StringField('Username', validators=[DataRequired()])
    email = StringField('Email', validators=[DataRequired(), Email()])
    password = PasswordField('Password', validators=[DataRequired()])
    confirm_password = PasswordField('Confirm Password', validators=[DataRequired(), EqualTo('password')])
    submit = SubmitField('Register')
```

### Login and Logout Routes

Define routes for user registration, login, and logout.

```python
# app.py (continued)

from werkzeug.security import generate_password_hash, check_password_hash

@app.route('/register', methods=['GET', 'POST'])
def register():
    form = RegisterForm()
    if form.validate_on_submit():
        existing_user = mongo.db.users.find_one({'username': form.username.data})
        if existing_user is None:
            hashpass = generate_password_hash(form.password.data, method='sha256')
            mongo.db.users.insert_one({
                'username': form.username.data,
                'email': form.email.data,
                'password': hashpass
            })
            flash('Registration successful!', 'success')
            return redirect(url_for('login'))
        flash('User already exists!', 'danger')
    return render_template('register.html', form=form)

@app.route('/login', methods=['GET', 'POST'])
def login():
    form = LoginForm()
    if form.validate_on_submit():
        user = mongo.db.users.find_one({'username': form.username.data})
        if user and check_password_hash(user['password'], form.password.data):
            user_obj = User(user['_id'])
            login_user(user_obj)
            return redirect(url_for('index'))
        flash('Invalid username or password', 'danger')
    return render_template('login.html', form=form)

@app.route('/logout')
@login_required
def logout():
    logout_user()
    return redirect(url_for('index'))
```

## 7. Job Management

### Job Posting Model

Store job postings in MongoDB.

### Job Posting Form

Create a form for posting jobs.

```python
# app.py (continued)

class JobForm(FlaskForm):
    title = StringField('Job Title', validators=[DataRequired()])
    company_name = StringField('Company Name', validators=[DataRequired()])
    description = TextAreaField('Job Description', validators=[DataRequired()])
    requirements = TextAreaField('Job Requirements', validators=[DataRequired()])
    location = StringField('Location', validators=[DataRequired()])
    salary_range = StringField('Salary Range', validators=[DataRequired()])
    submit = SubmitField('Post Job')
```

### Job Routes

Define routes for posting and viewing jobs.

```python
# app.py (continued)

@app.route('/post_job', methods=['GET', 'POST'])
@login_required
def post_job():
    form = JobForm()
    if form.validate_on_submit():
        mongo.db.jobs.insert_one({
            'title': form.title.data,
            'company_name': form.company_name.data,
            'description': form.description.data,
            'requirements': form.requirements.data,
            'location': form.location.data,
            'salary_range': form.salary_range.data,
            'date_posted': datetime.utcnow(),
            'posted_by': ObjectId(current_user.id)
        })
        flash('Job posted successfully!', 'success')
        return redirect(url_for('index'))
    return render_template('post_job.html', form=form)
```

## 8. Templates and Frontend

Create templates for different pages.

### `base.html`

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Job Portal</title>
  <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
</head>
<body>
  <nav>
    <ul>
      <li><a href="{{ url_for('index') }}">Home</a></li>
      {% if current_user.is_authenticated %}
        <li><a href="{{ url_for('post_job') }}">Post a Job</a></li>
        <li><a href="{{ url_for('logout') }}">Logout</a></li>
      {% else %}
        <li><a href="{{ url_for('login') }}">Login</a></li>
        <li><a href="{{ url_for('register') }}">Register</a></li>
      {% endif %}
    </ul>
  </nav>
  <div class="container">
    {% with messages = get_flashed_messages(with_categories=true) %}
      {% if messages %}
        {% for category, message in messages %}
          <div class="flash {{ category }}">{{ message }}</div>
        {% endfor %}
      {% endif %}
    {% endwith %}
    {% block content %}{% endblock %}
  </div>
</body>
</html>
```

### `index.html`

```html
{% extends "base.html" %}
{% block content %}
<h1>Job Listings</h1>
{% for job in jobs %}
  <div class="job">
    <h2>{{ job.title }}</h2>
    <p><strong>Company:</strong> {{ job.company_name }}</p>
    <p><strong>Description:</strong> {{ job.description }}</p>
    <p><strong>Requirements:</strong> {{ job.requirements }}</p>
    <p><strong>Location:</strong> {{ job.location }}</p>
    <p><strong>Salary Range:</strong> {{ job.salary_range }}</p>
    <p><strong>Date Posted:</strong> {{ job.date_posted }}</p>
  </div>
{% endfor %}
{% endblock %}
```

### `login.html`

```html
{% extends "base.html" %}
{% block content %}
<h1>Login</h1>
<form method="POST">
  {{ form.hidden_tag() }}
  <div>
    {{ form.username.label }}<br>
    {{ form.username(size=32) }}<br>
    {% for error in form.username.errors %}
      <span style="color: red;">[{{ error }}]</span>
    {% endfor %}
  </div>
  <div>
    {{ form.password.label }}<br>
    {{ form.password(size=32) }}<br>
    {% for error in form.password.errors %}
      <span style="color: red;">[{{ error }}]</span>
    {% endfor %}
  </div>
  <div>
    {{ form.submit() }}
  </div>
</form>
{% endblock %}
```

### `register.html`

```html
{% extends "base.html" %}
{% block content %}
<h1>Register</h1>
<form method="POST">
  {{ form.hidden_tag() }}
  <div>
    {{ form.username.label }}<br>
    {{ form.username(size=32) }}<br>
    {% for error in form.username.errors %}
      <span style="color: red;">[{{ error }}]</span>
    {% endfor %}
  </div>
  <div>
    {{ form.email.label }}<br>
    {{ form.email(size=32) }}<br>
    {% for error in form.email.errors %}
      <span style="color: red;">[{{ error }}]</span>
    {% endfor %}
  </div>
  <div>
    {{ form.password.label }}<br>
    {{ form.password(size=32) }}<br>
    {% for error in form.password.errors %}
      <span style="color: red;">[{{ error }}]</span>
    {% endfor %}
  </div>
  <div>
    {{ form.confirm_password.label }}<br>
    {{ form.confirm_password(size=32) }}<br>
    {% for error in form.confirm_password.errors %}
      <span style="color: red;">[{{ error }}]</span>
    {% endfor %}
  </div>
  <div>
    {{ form.submit() }}
  </div>
</form>
{% endblock %}
```

### `post_job.html`

```html
{% extends "base.html" %}
{% block content %}
<h1>Post a Job</h1>
<form method="POST">
  {{ form.hidden_tag() }}
  <div>
    {{ form.title.label }}<br>
    {{ form.title(size=64) }}<br>
    {% for error in form.title.errors %}
      <span style="color: red;">[{{ error }}]</span>
    {% endfor %}
  </div>
  <div>
    {{ form.company_name.label }}<br>
    {{ form.company_name(size=64) }}<br>
    {% for error in form.company_name.errors %}
      <span style="color: red;">[{{ error }}]</span>
    {% endfor %}
  </div>
  <div>
    {{ form.description.label }}<br>
    {{ form.description(rows=5, cols=40) }}<br>
    {% for error in form.description.errors %}
      <span style="color: red;">[{{ error }}]</span>
    {% endfor %}
  </div>
  <div>
    {{ form.requirements.label }}<br>
    {{ form.requirements(rows=5, cols=40) }}<br>
    {% for error in form.requirements.errors %}
      <span style="color: red;">[{{ error }}]</span>
    {% endfor %}
  </div>
  <div>
    {{ form.location.label }}<br>
    {{ form.location(size=64) }}<br>
    {% for error in form.location.errors %}
      <span style="color: red;">[{{ error }}]</span>
    {% endfor %}
  </div>
  <div>
    {{ form.salary_range.label }}<br>
    {{ form.salary_range(size=64) }}<br>
    {% for error in form.salary_range.errors %}
      <span style="color: red;">[{{ error }}]</span>
    {% endfor %}
  </div>
  <div>
    {{ form.submit() }}
  </div>
</form>
{% endblock %}
```

## 9. Running the Application

To run the application, execute the following command in your terminal:

```bash
python app.py
```

Visit `http://127.0.0.1:5000` in your browser to see the job portal in action.

---

### Summary

This guide provides a step-by-step process for setting up a job search portal using Flask, Flask-WTF, Flask-Login, and Flask-PyMongo. We covered setting up the project, designing the database, creating the Flask application, handling user authentication, managing job postings, and creating the necessary templates. This should give you a good foundation to build and expand upon for a fully functional job search portal.