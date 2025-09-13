# ProDev Backend Engineering program.
## General overview of the alx prodev Backend Engineering program

## Key Technologies covered
1. Python: Python is a high-level, interpreted programming language known for its simplicity and readability. It is widely used in various fields due to its versatility, extensive libraries, and strong community support.Python is widely adopted because of its simplicity and efficiency in solving real-world problems and it follows an easy-to-learn syntax that makes it ideal for both beginners and experienced developers. Python is used in various domains, including:
    - Web Development (Django, Flask)
    - Data Science & Machine Learning (Pandas, NumPy, TensorFlow)
    - Automation & Scripting (Automating tasks, web scraping)
    - Cybersecurity & Ethical Hacking
    - Game Development (Pygame)
    - Embedded Systems & IoT
    - Desktop & GUI Applications (Tkinter, PyQt)
```bash
# Class declaration in python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def introduce(self):
        return f"My name is {self.name} and I am {self.age} years old."

p = Person("Temitope", 25)
print(p.introduce())
```

2. Django: Django is a high-level Python web framework that promotes rapid development and clean, pragmatic design. It follows the Model-View-Template (MVT) architecture and comes with built-in features like authentication, database management, and security. Django is used for developing:
    - Web Applications (e.g., job portals, social media platforms)
    - E-commerce Websites
    - APIs & RESTful Services (Django REST Framework)
    - CMS (Content Management Systems)
    - Data-Driven Applications
```bash
# Installation
pip install django

# Create Django project
django-admin startproject myproject
cd myproject

# Start server
python manage.py runserver

# Create Django app
python manage.py startapp myapp
```
```bash
# Create database model in models.py file
from django.db import models

class UserProfile(models.Model):
    name = models.CharField(max_length=100)
    email = models.EmailField(unique=True)
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.name

# Migrate model to actual db to create tables
python manage.py makemigrations
python manage.py migrate

# Create api logic in views.py file
from django.http import JsonResponse
from .models import UserProfile

def user_list(request):
    users = list(UserProfile.objects.values())
    return JsonResponse(users, safe=False)

# Create url path for views in urls.py file
from django.urls import path
from .views import user_list

urlpatterns = [
    path('users/', user_list),
]

# Running the Django Dvelopment server
python manage.py runserver

# Now, visiting http://127.0.0.1:8000/users/ will return a JSON response of all users.
```
Django makes web development fast, secure, and scalable!

3. Django REST Framework (DRF): Django REST Framework (DRF) is a powerful toolkit for building RESTful APIs in Django. It provides authentication, serialization, and easy integration with Django models. Django REST Framework makes it easy to build scalable and secure APIs. With just a few lines of code, you can create a fully functional REST API that supports CRUD operations, authentication, and more!  This can be used for:
    - Building APIs for web and mobile applications
    - Authentication & Authorization (JWT, OAuth, Token-based auth)
    - CRUD Operations (Create, Read, Update, Delete)
    - Filtering, Pagination & Search
    - Handling JSON data efficiently

```bash
# Installation
pip install djangorestframework

# Make it work by adding it to INSTALLED_APPS in settings.py
INSTALLED_APPS = [
    'rest_framework',
    'myapp',  # Your app
]

# Create serializers for your Django app models
from rest_framework import serializers
from .models import UserProfile

class UserProfileSerializer(serializers.ModelSerializer):
    class Meta:
        model = UserProfile
        fields = '__all__'  # Include all fields

# Create view for your Django app
from rest_framework import generics
from .models import UserProfile
from .serializers import UserProfileSerializer

# List all users or create a new one
class UserProfileListCreate(generics.ListCreateAPIView):
    queryset = UserProfile.objects.all()
    serializer_class = UserProfileSerializer

# Retrieve, update, or delete a specific user
class UserProfileDetail(generics.RetrieveUpdateDestroyAPIView):
    queryset = UserProfile.objects.all()
    serializer_class = UserProfileSerializer

# Define API routes for your views
from django.urls import path
from .views import UserProfileListCreate, UserProfileDetail

urlpatterns = [
    path('users/', UserProfileListCreate.as_view(), name='user-list'),
    path('users/<int:pk>/', UserProfileDetail.as_view(), name='user-detail'),
]

# Run your server
python manage.py runserver

```
```plaintext
# Authentication & Permission (Optional)
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.SessionAuthentication',
        'rest_framework.authentication.TokenAuthentication',
    ],
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAuthenticated',
    ],
}
```

**Testing your endpoint using postman or curl**
```bash
# GET
curl -X GET http://127.0.0.1:8000/users/
curl -X GET http://127.0.0.1:8000/users/1/

# POST
curl -X POST http://127.0.0.1:8000/users/ -H "Content-Type: application/json" -d '{"name": "Alice", "email": "alice@example.com"}'

# PUT
curl -X PUT http://127.0.0.1:8000/users/1/ -H "Content-Type: application/json" -d '{"name": "Alice Updated"}'

# PATCH
curl -X PATCH http://127.0.0.1:8000/users/1/ -H "Content-Type: application/json" -d '{"name": "Alice Updated"}'

# DELETE
curl -X DELETE http://127.0.0.1:8000/users/1/

```

4. Docker - Containerized deployments

Summary of each step and its role in modern containerized deployments:

**Development (App Building):**
This is the preliminary stage of the whole software development process. At this stage, developers - both Frontend and Backend, develop their applications respectively. This is where containerization of the application starts. During the development stage, developers may use different versions of packages and libraries, which, when merging code together, might result in conflicts and dependency issues. Dockerizing your application helps solve this issue.

Developers save their dependencies in a file, e.g., in Python, `requirements.txt`, and in JavaScript, `package.json`. Once they have this in place, they can create a configuration to build a Docker image in a file called `Dockerfile` and specify the packages to install with their versions inside that image. Once the code is pushed, other developers working on it can create an image and run a container based on the Dockerfile. This ensures that they have the same environment and the same dependencies. With Docker, you do not need to install all of these dependencies on your local machine as you can just install them on your image or container, and that's all.

**Steps in dockerizing a Python Application:**

- Create a `requirements.txt` file:
```bash
    flask==2.0.1
    requests==2.25.1
```
Above file content can be achieve in python application by using this:
```bash
    pip freeze > requirements.txt
 ```

- Create a `Dockerfile`:
```dockerfile
    # Use an official Python runtime as a parent image
    FROM python:3.8-slim

    # Set the working directory in the container
    WORKDIR /app

    # Copy the current directory contents into the container at /app
    COPY . /app

    # Install any needed packages specified in requirements.txt
    RUN pip install --no-cache-dir -r requirements.txt

    # Make port 80 available to the world outside this container
    EXPOSE 80

    # Define environment variable
    ENV NAME World

    # Run app.py when the container launches
    CMD ["python3", "app.py"]
```

- Build the Docker image:
```bash
    docker build -t my-python-app .
```

- Run the Docker container:
```bash
docker run -p 4000:80 my-python-app
```
N.B:
In some cases, you might need to access the shell inthe command, to do so, use the command below to access as the root user:
```bash
docker exec -it --user root jenkins-container bash
```

By following these steps, developers can ensure that their applications run consistently across different environments, avoiding the "it works on my machine" problem. Docker provides a standardized unit of software that packages up code and all its dependencies, making it easier to develop, ship, and run applications.

5. CI/CD (Continuous Integration/Continuous Deployment)

- CI/CD: CI/CD stands for Continuous Integration and Continuous Deployment/Delivery. It is a method to frequently deliver apps to customers by introducing automation into the stages of app development. The main concepts attributed to CI/CD are continuous integration, continuous deployment, and continuous delivery.

- Continuous Integration (CI): CI is a development practice where developers integrate code into a shared repository frequently, preferably several times a day. Each integration can then be verified by an automated build and automated tests. CI aims to detect and fix integration issues earlier, improve software quality, and reduce the time it takes to validate and release new software updates.

- Continuous Deployment (CD): CD is a software release process that uses automated testing to validate whether changes to a codebase are correct and stable for immediate autonomous deployment to a production environment. CD ensures that software can be reliably released at any time.

**CI/CD find it's use in**:
- Automated Testing: Ensures that code changes do not break the existing functionality.
- Automated Deployment: Reduces the manual effort required to deploy applications.
- Faster Release Cycles: Enables more frequent releases of software.
- Improved Collaboration: Encourages collaboration among team members by integrating changes frequently.

**Advantages of CI/CD**:
- Early Bug Detection: Bugs are detected early in the development cycle.
- Reduced Integration Issues: Frequent integration reduces the risk of integration issues.
- Faster Time to Market: Automated processes speed up the release cycle.
- Consistent Delivery: Ensures consistent and reliable delivery of software.
- Improved Quality: Automated testing and deployment improve the overall quality of the software.

**Setting Up CI/CD**
What is Jenkins?: Jenkins is an open-source automation server designed to help developers and teams automate tasks related to building, testing, and deploying software. It's a key tool in the DevOps ecosystem because it simplifies continuous integration (CI) and continuous delivery (CD), ensuring that code changes are automatically tested and deployed quickly and reliably. Think of Jenkins as your software-building assistant — it automates repetitive tasks so developers can focus on writing code instead of worrying about deployment, testing, or builds.

**Why Use Jenkins?**
- Automation: Automates tasks like building your code, running tests, and deploying to servers.
- Continuous Integration (CI): Allows developers to merge their code changes into a shared repository multiple times a day, with automated testing to catch issues early.
- Continuous Delivery (CD): Streamlines the process of delivering new code to production, reducing downtime.
- Plugins: Offers over 1,800 plugins to integrate with tools like GitHub, Docker, Kubernetes, Slack, and more.
- Customizable Pipelines: Lets you create pipelines (sequences of steps) to define how your code is built, tested, and deployed.
- Containerized Workflows: Build Docker images for your application and push them to container registries like Docker Hub.

Install Jenkins: Run jenkins in a docker container by executing the following command
```bash
docker run -d --name jenkins -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts
```
**This command will**:
- Pull the latest Long-Term Support (LTS) Jenkins image.
- Expose Jenkins on port 8080.
- Map the Jenkins home directory to the host machine to persist data.

**Configure Jenkins**:
- Get your admin password by running this command:
```bash
    # Open interactive shell of the jenkins container
    docker -exec -it --user root <name of your jenkins container>
```
```bash
    # Run this to get the admin password
    cat /var/lib/jenkins/secrets/initialAdminPassword
```
- Access Jenkins at `http://localhost:8080`
- Input your admin password when propted
- Follow the setup wizard to install recommended plugins and create an admin user.
- Create your first pipeline or job.

**Setting up a Jenkins Pipeline**:
- Create a New Pipeline Job:
- Go to the Jenkins dashboard, click "New Item", and select "Pipeline".
- Write a Jenkinsfile: A Jenkinsfile defines your CI/CD pipeline. Example below

```groovy
    pipeline {
        agent any
        stages {
            stage('Build') {
                steps {
                    echo 'Building the application...'
                }
            }
            stage('Test') {
                steps {
                    echo 'Running tests...'
                }
            }
            stage('Deploy') {
                steps {
                    echo 'Deploying the application...'
                }
            }
        }
    }
```
**Run the Pipeline**:
- Save the job and click "Build Now" to see your pipeline execute step by step.

**Example with GitHub Actions**

Create a GitHub Actions Workflow:
- Create a `.github/workflows/main.yml` file in your repository.

```yaml
name: CI/CD Pipeline

on:
    push:
    branches:
        - main

jobs:
    build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
        uses: actions/checkout@v2

    - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
        node-version: '14'

    - name: Install dependencies
        run: npm install

    - name: Run tests
        run: npm test

    - name: Build project
        run: npm run build

    - name: Deploy
        run: npm run deploy
```

**Jenkins vs. GitHub Actions**

1. Jenkins

**Pros**:
- Highly customizable with a wide range of plugins.
- Can be hosted on-premises, providing more control over the environment.
- Supports complex workflows and pipelines.

**Cons**:
- Requires maintenance and management of the Jenkins server.
- Steeper learning curve for beginners.

2. GitHub Actions

**Pros**:
- Integrated with GitHub, making it easy to set up and use.
- No need to manage servers or infrastructure.
- Supports a wide range of actions and workflows.

- **Cons**:
  - Limited to GitHub repositories.
  - Less customizable compared to Jenkins.

**Differences Between CI and CD**

**Continuous Integration (CI)**:
- Focuses on integrating code changes frequently.
- Automated builds and tests are run to ensure code quality.
- Aims to detect and fix integration issues early.

**Continuous Deployment (CD)**:
- Focuses on deploying code changes automatically to production.
- Ensures that the code is always in a deployable state.
- Reduces the manual effort required for deployment.

By implementing CI/CD, development teams can improve their workflow, reduce errors, and deliver high-quality software more efficiently.

## Important backend development concepts
1. Database Design: Database design is the process of structuring data to ensure efficiency, integrity, and scalability in an application. It involves defining tables, relationships, indexing, and normalization.

**Key Concepts in Database Design**
- Normalization: Organizing tables to reduce redundancy (1NF, 2NF, 3NF).
- Denormalization: Optimizing for performance by reducing joins.
- Indexes: Speed up queries but require careful usage to avoid overhead.
- Relationships: One-to-One, One-to-Many, and Many-to-Many relationships.
- Constraints: Primary key, foreign key, unique, not null, and check constraints.

**Example: Relational Database Design using SQL**
```sql
CREATE TABLE Users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100),
    email VARCHAR(100) UNIQUE NOT NULL
);

CREATE TABLE Posts (
    id INT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(255),
    content TEXT,
    user_id INT,
    FOREIGN KEY (user_id) REFERENCES Users(id) ON DELETE CASCADE
);
```

2. Asynchronous Programming: Asynchronous programming allows multiple tasks to run concurrently, improving performance in I/O-heavy applications like web APIs, database queries, and real-time messaging.

**Key Concepts in Async Programming**
- Synchronous vs. Asynchronous: Blocking vs. non-blocking execution.
- Concurrency: Multiple tasks making progress simultaneously.
- Parallelism: Running tasks at the same time using multiple CPU cores.
- Event Loop: Manages asynchronous tasks (e.g., JavaScript, Python's asyncio).
- Callbacks, Promises, and Async/Await: Techniques for handling async code.
- Example in Python (Async/Await)

```python
import asyncio

async def fetch_data():
    print("Fetching data...")
    await asyncio.sleep(2)  # Simulating a network call
    return "Data received!"

async def main():
    data = await fetch_data()
    print(data)

asyncio.run(main())
```

**Benefits of Asynchronous Programming:**
- Faster execution for I/O-bound tasks.
- Non-blocking operations improve responsiveness.
- Useful in web frameworks like FastAPI and Django Channels.

3. Caching Strategies: Caching improves application performance by storing frequently accessed data in memory, reducing the need for expensive database queries.

**Types of Caching**
- In-Memory Cache: Stores data in RAM (e.g., Redis, Memcached).
- Database Cache: Uses database mechanisms to cache queries.
- CDN (Content Delivery Network): Caches static assets globally (e.g., Cloudflare).
- Application-Level Cache: Stores precomputed results (e.g., Django Cache).

**Example: Caching with Redis in Django**
```python
from django.core.cache import cache

def get_data():
    data = cache.get('my_data')
    if not data:
        data = expensive_query()
        cache.set('my_data', data, timeout=60)  # Cache for 60 seconds
    return data
```

**Best Practices:**
- Use Redis for caching frequently accessed database queries.
- Set expiration times (TTL) to prevent outdated data.
- Invalidate cache when data is updated in the database.

## Challenges Faced and Solutions Implemented
1. Authentication & Security Risks
**Challenge:**
- User session hijacking & token leakage.
- Expired but still valid JWTs allowing access.
- Weak password security.

**Solution**
- Revoke tokens when a user is deleted or logs out
```python
from rest_framework_simplejwt.tokens import OutstandingToken

def revoke_user_tokens(user):
    OutstandingToken.objects.filter(user=user).delete()
```
- Enforce strong password policies
```plaintext
AUTH_PASSWORD_VALIDATORS = [
    {"NAME": "django.contrib.auth.password_validation.MinimumLengthValidator"},
    {"NAME": "django.contrib.auth.password_validation.CommonPasswordValidator"},
]
```
- Use Django’s @login_required and DRF’s permission classes
```python
from rest_framework.permissions import IsAuthenticated

class SecureView(APIView):
    permission_classes = [IsAuthenticated]
```

## Best Practices and Personal Takeaways in Django
Django is a powerful and high-level web framework that promotes rapid development and clean, pragmatic design. Over time, I've learned and applied best practices that improve performance, maintainability, and security in Django projects. Here are some key best practices and personal takeaways.

1. Project Structure and Code Organization
**Best Practices:**
- Use a modular structure
    - Split large projects into multiple apps (e.g., users, jobs, payments).
    - Keep business logic inside services.py instead of views or models.
- Follow the "Fat Models, Thin Views" principle
    - Business logic should reside in models or services, not views.
- Use Django's built-in features instead of reinventing the wheel
    - Example: Use django.contrib.auth instead of building a custom authentication system.

2. Database and ORM Best Practices
- Use indexing to optimize query performance
```python
class Job(models.Model):
    title = models.CharField(max_length=255, db_index=True)  # Indexing improves search speed
```
- Use select_related and prefetch_related to reduce queries
```python
# Use select_related for foreign key relationships (1-to-1, Many-to-1)
jobs = Job.objects.select_related("company").all()

# Use prefetch_related for many-to-many relationships
users = User.objects.prefetch_related("groups").all()
```
- Avoid unnecessary queries with the only() and defer() methods
```python
User.objects.only("id", "username")  # Fetch only required fields
```
- Use database migrations properly
    - Always review migrations before running them in production.
    - Use makemigrations and migrate correctly.

3. Security Best Practices
-  Use Django’s built-in authentication and permissions
```python
from rest_framework.permissions import IsAuthenticated

class SecureView(APIView):
    permission_classes = [IsAuthenticated]
```
- Prevent CSRF attacks by enabling CSRF protection
```plaintext
MIDDLEWARE = [
    'django.middleware.csrf.CsrfViewMiddleware',
]
```
-  Hash passwords using Django’s built-in User model
```python
from django.contrib.auth.hashers import make_password

hashed_password = make_password("my_secure_password")
```
- Use ALLOWED_HOSTS to prevent host header attacks
```plaintext
ALLOWED_HOSTS = ["yourdomain.com"]
```
- Enable security headers using Django Security Middleware
```plaintext
SECURE_HSTS_SECONDS = 31536000  # Enforce HTTPS
SECURE_BROWSER_XSS_FILTER = True
SECURE_CONTENT_TYPE_NOSNIFF = True
```
- Store secrets in environment variables instead of hardcoding
```plaintext
import os
SECRET_KEY = os.getenv("DJANGO_SECRET_KEY")
```
4. Performance Optimization
- Enable caching for frequently accessed data
```python
from django.core.cache import cache

cache.set("top_jobs", jobs, timeout=60 * 5)  # Cache for 5 minutes
```
- Use pagination to prevent loading too much data at once
```python
from rest_framework.pagination import PageNumberPagination

class CustomPagination(PageNumberPagination):
    page_size = 10
```
- Use background tasks for heavy operations (e.g., Celery)
```python
from celery import shared_task

@shared_task
def send_email_task(email):
    send_email(email)
```

5. API Design and RESTful Principles
- Use Django REST Framework (DRF) for building APIs
- Implement proper status codes and error handling
- Use filtering and searching for better API usability
```python
from django_filters.rest_framework import DjangoFilterBackend

class JobViewSet(ModelViewSet):
    queryset = Job.objects.all()
    filter_backends = [DjangoFilterBackend]
    filterset_fields = ['category', 'location']
```

6. Testing and Debugging
-  Write unit and integration tests
```python
from django.test import TestCase

class JobTestCase(TestCase):
    def test_create_job(self):
        job = Job.objects.create(title="Software Engineer")
        self.assertEqual(job.title, "Software Engineer")
```
- Use Django Debug Toolbar for performance monitoring
```plaintext
INSTALLED_APPS += ["debug_toolbar"]
MIDDLEWARE += ["debug_toolbar.middleware.DebugToolbarMiddleware"]
```

7. Deployment and DevOps
- Use Docker for consistency across environments
- Use Gunicorn and Nginx for production deployment
```plaintext
gunicorn --workers=3 myapp.wsgi
```
- Use CI/CD pipelines (GitHub Actions, Jenkins) for automated deployment
```yaml
jobs:
  build:
    steps:
      - name: Deploy to Server
        run: ssh user@server "cd /app && git pull && docker compose up --build -d"
```
- Monitor application errors using Sentry or logging
```python
import logging

logger = logging.getLogger(__name__)

def view_function():
    try:
        risky_operation()
    except Exception as e:
        logger.error(f"Error occurred: {e}")
```
