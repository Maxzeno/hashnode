# Building a CRUD API with Django

If you're looking to build a RESTful API using Django, one of the easiest ways to do so is by utilizing Django's built-in views and serializers. In this article, we'll explore how to create a basic CRUD (create, read, update, delete) API using these tools.

First, let's set up our Django project and create an app to house our API. In the terminal, navigate to the directory where you want to create your project and enter the following commands:

```bash
django-admin startproject myproject
cd myproject
python manage.py startapp api
```

Next, we'll need to set up our models for our API. Let's say we want to create an API for a simple to-do list app. Our model might look something like this:

```python
from django.db import models

class Task(models.Model):
    title = models.CharField(max_length=200)
    description = models.TextField()
    completed = models.BooleanField(default=False)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
```

Now that we have our model set up, we can create our serializers. Serializers allow us to convert our Django models into JSON data that can be sent over the web. To do this, we'll need to import Django's `ModelSerializer` class and create a serializer class for our `Task` model:

```python
from rest_framework import serializers

class TaskSerializer(serializers.ModelSerializer):
    class Meta:
        model = Task
        fields = '__all__'
```

With our serializer in place, we can now create our views. Django provides several built-in views for handling common API actions, such as creating, updating, and deleting records. Let's start by creating a view for listing all tasks and a view for creating new tasks:

```python
from django.shortcuts import render
from rest_framework import generics
from .serializers import TaskSerializer
from .models import Task

class TaskListView(generics.ListCreateAPIView):
    queryset = Task.objects.all()
    serializer_class = TaskSerializer

class TaskCreateView(generics.CreateAPIView):
    queryset = Task.objects.all()
    serializer_class = TaskSerializer
```

With these views in place, we can now access our API at the `/api/tasks/` endpoint, which will list all tasks and allow us to create new ones.

Next, let's create views for updating and deleting tasks. We can do this by creating a `TaskDetailView` that subclasses `generics.RetrieveUpdateDestroyAPIView`:

```python
class TaskDetailView(generics.RetrieveUpdateDestroyAPIView):
    queryset = Task.objects.all()
    serializer_class = TaskSerializer
```

Now we can access individual tasks at the `/api/tasks/<task_id>` endpoint and perform update and delete actions on them.

With these views in place, we now have a fully functional CRUD API for our task list app.

In this article, we walk through the steps of building a RESTful API using Django's built-in views and serializers. We start by setting up a Django project and creating an app to house our API. Then we define our models and create serializers to convert them into JSON data. Next, we create views for common API actions, such as listing tasks, creating tasks, updating tasks, and deleting tasks. With these views in place, we have a fully functional CRUD API for our task list app, with endpoints available for listing tasks, accessing individual tasks, and performing update and delete actions on them.

**My name is Emmanuel and I post helpful articles daily follow me for more and don't forget to leave a comment.**