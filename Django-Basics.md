# Django Basics Map Guideline by [@mhimidradhwen](https://github.com/mhimidradhwen)
## Introduction to Django

Django is a high-level Python web framework that enables rapid development of secure and maintainable websites. It follows the Model-View-Controller (MVC) architectural pattern and promotes the principles of DRY (Don't Repeat Yourself) and reusable apps. Django also includes built-in support for user authentication, form handling, and database management.

## Setting up a Django Project

To start a new Django project, you'll need to have Python and Django installed on your machine. Once you have them, you can use the `django-admin startproject` command to create a new project. This command will create a new directory with the same name as your project and within this directory, it will contain the following files and directories:
- `manage.py`: A command-line utility that lets you interact with your project.
- `settings.py`: A file where you can configure your project's settings such as database, installed apps, and middleware.
- `urls.py`: A file where you can define your project's URL patterns.
- `wsgi.py`: A file that contains the WSGI application.

## Creating a Django App

Creating a Django app is a straightforward process and can be done using the command `python manage.py startapp appname`. This command should be run from the root directory of your Django project.
```shell 
python manage.py startapp appname
```

When you run this command, Django will create a new directory with the same name as your app and within this directory, it will contain the following files and directories:
- `models.py`: A file where you can define your app's models and database tables.
- `views.py`: A file where you can define your app's views and handle HTTP requests and responses.
- `urls.py`: A file where you can define your app's URL patterns.
- `migrations`: A directory where you can store your app's database migrations.

You will also need to add the app name to the `INSTALLED_APPS` list in your `settings.py` file to make Django aware of your new app.
```python
INSTALLED_APPS = [
    ...
    'blog',
]
```
Once the app is created, you can start defining models, views, and URL patterns for the app, and also include it in the main project's `urls.py` file to make it accessible via a URL.

This command will create a new app within your project's directory, and you will also have to include the app in the `INSTALLED_APPS` list in the `settings.py` file to make Django aware of it and make it accessible via URLs.

## Defining Models and Database Relationships

In Django, models are defined as Python classes that inherit from the `django.db.models.Model` class. These classes define the fields and behavior of the models, such as the database table name, fields, and relationships.

To define a model, you'll need to import the `models` module and create a new class that inherits from `models.Model`. You can then define the fields of the model using class variables and the appropriate field type. For example:

```python
from django.db import models

class Person(models.Model):
    name = models.CharField(max_length=100)
    age = models.IntegerField()
```

In this example, we've defined a `Person` model with two fields: `name` and `age`.

You can also define database relationships between models using the appropriate fields such as ForeignKey, OneToOneField, and ManyToManyField. For example, to create a one-to-many relationship between the `Person` model and a `Car` model:
```python
class Car(models.Model):
    make = models.CharField(max_length=100)
    owner = models.ForeignKey(Person, on_delete=models.CASCADE)
```
In this example, we've defined a `Car` model with a **ForeignKey** field called `owner` that relates to the `Person` model. When a Person instance is deleted, all related `Car` instances **will also be deleted** (as specified by the `on_delete` parameter).

It's important to note that after defining the models, you will have to run migrations to create the corresponding tables in the database using the command:
```python
python manage.py makemigrations
```
and then: 
```python 
python manage.py migrate
```
This code defines the basic structure of a model in Django, and also shows an example of how to create a one-to-many relationship between two models using the `ForeignKey` field. It also reminds that after defining the models you have to run migrations to create the corresponding tables in the database.

## Using Django's ORM to Perform CRUD Operations

Django's Object-Relational Mapper (ORM) allows you to interact with the database using Python code rather than writing raw SQL queries. This makes it easy to perform common database operations such as creating, reading, updating, and deleting (CRUD) records.

To create a new record, you can create a new instance of a model and call the `save()` method on it. For example, to create a new `Person` record:
```python
person = Person(name='John Doe', age=30)
person.save()
```
To retrieve records, you can use the model's manager (by default it's objects) and call the appropriate query method on it.
For example, to retrieve all Person records:

```python
people = Person.objects.all()
```
To retrieve a single record by its primary key, you can use the get() method:
```python
person = Person.objects.get(pk=1)
```
To update a record, you can retrieve it, modify its fields, and call the `save()` method on it.
For example, to update the name of a Person record:
```python
person = Person.objects.get(pk=1)
person.name = 'Jane Doe'
person.save()
```
To delete a record, you can retrieve it and call the `delete()` method on it.
For example, to delete a Person record:
```python
person = Person.objects.get(pk=1)
person.delete()
```
You can also use the `QuerySet.delete()` method to delete multiple records at once that match a certain condition.

These are some basic examples of how to use Django's ORM to perform CRUD operations. The ORM also provides other useful methods and functions for querying and manipulating records, such as filtering, ordering, and aggregating records.

This code shows some basic examples of how to use Django's ORM to perform CRUD operations, like **creating** , **reading**, **updating** and **deleting** records. It also mentions that the ORM provides other useful methods and functions for querying and manipulating records.
##  Creating Views and URL Patterns

In Django, views are Python **functions** or **class-based views** that handle HTTP requests and return HTTP responses. Views are defined in the `views.py` file of an app, and they are responsible for performing the necessary logic and generating the appropriate response for a given URL.

To create a view, you'll need to define a new Python function or class that takes an `HttpRequest` object as an argument and returns an `HttpResponse` object. The function or class can perform any necessary logic, such as querying the database or processing form data, before returning the response.

Here's an example of a simple view that returns a `"Hello, World!"` response:

```python
from django.http import HttpResponse

def hello(request):
    return HttpResponse("Hello, World!")
```

Once the views are defined, you'll need to map them to URLs using `URL patterns`. URL patterns are defined in the `urls.py` file of the project and the app. They specify which view should be called for a given URL.

To create a URL pattern, you'll need to import the view function or class and use the `path()` or `re_path()` function from `django.urls` to define the pattern. You can also use `url()` function from `django.conf.urls` in case of **older version of django**. The first argument is the URL pattern, and the second argument is the view that should be called for that pattern.

```python
from django.urls import path
from . import views

urlpatterns = [
    path('hello/', views.hello),
]
```

In this example, when a user visits the URL `/hello/`, the `hello` view will be called, and the **"Hello, World!"** response will be returned.

It's also worth mentioning that you can use regular expressions to define more complex URL patterns, and you can also include additional parameters in the URL pattern that can be passed to the view as arguments.

In summary, views and URL patterns are essential components of a Django web application. They handle the logic and routing of HTTP requests and responses, allowing you to separate the concerns of handling requests and generating responses.

## Using Templates and Static Files

Django's templating system allows you to separate the presentation logic of your application from the views and models. `Templates` are text files that define the structure and layout of a web page, and they can be rendered with dynamic data to generate the final HTML response.

To use templates in your Django application, you'll need to create a **templates directory** in the root of your project or in your app directory. Inside this directory, you can create subdirectories to organize your templates by app or by functionality.

For example, you can create a `base_templates` directory that contains a base template that provides a common layout for all pages and a home directory that contains a template for the homepage of your site.

Once you've created your templates, you can use the `render()` function from `django.shortcuts` to render a template and return an HttpResponse object. The render() function takes the request object, the path to the template, and a **context** dictionary as arguments. The context dictionary contains the variables that will be passed to the template and can be used to inject dynamic data into the template.

Here's an example of how to use the `render()` function to render a template and return a response:

```python
from django.shortcuts import render

def hello(request):
    context = {'name': 'John Doe'}
    return render(request, 'hello.html', context)
```
In this example, when the `hello` view is called, the `hello.html` template will be rendered with the name variable set to **"John Doe"** and the result will be returned as the response.

Django also provides a way to handle `static files` like images, CSS, and JavaScript files. These files should be placed in a `static` directory at the root of your project or app. Once the static files are in place, you can use the `{% static %}` template tag in your templates to reference the files. For example, you can include an image in a template like this:
```html
<img src="{% static 'images/logo.png' %}"/>
```
In summary, using templates and static files in Django allows you to separate the presentation logic of your application from the views and models. It makes your code easier to maintain and allows for a more efficient development process.

## Handling Forms and User Input

Handling `forms` and `user` input is an essential aspect of web development. In Django, forms are defined as classes that inherit from the forms.Form or `forms.ModelForm` class. The forms.Form class provides a way to define a form as a collection of fields, while the forms.ModelForm class allows you to create a form from an existing model.

Here's an example of a simple form that has two fields, a **CharField** for the `name` and an **EmailField** for the `email`:

```python
from django import forms

class ContactForm(forms.Form):
    name = forms.CharField(max_length=100)
    email = forms.EmailField()
```
And here's an example of a form that is based on a model `"Person"`

```python
from django import forms
from .models import Person

class PersonForm(forms.ModelForm):
    class Meta:
        model = Person
        fields = ['name', 'age']
```
Once the form is defined, you can use it in a view to handle the form submission and validate the data. When a user submits a form, the data is sent in the request object as part of the `POST` or `GET` data. To handle a form submission, you can check the request method in the view and use the `is_valid()` method of the form to validate the data. If the data is valid, you can perform any necessary logic and redirect the user to a success page. If the data is not valid, you can render the form template again and display error messages to the user.

Here's an example of how to handle a form submission in a view:

```python
from django.shortcuts import render, redirect

def contact(request):
    if request.method == 'POST':
        form = ContactForm(request.POST)
        if form.is_valid():
            name = form.cleaned_data['name']
            email = form.cleaned_data['email']
            # Perform any necessary logic
            return redirect('success')
    else:
        form = ContactForm()
    return render(request, 'contact.html', {'form': form})
```
It's also worth mentioning that Django provides several built-in form fields, widgets, and validators to help you handle different types of input and validation. You can also create your custom form fields, widgets, and validators if the built-in ones don't meet your needs.

In summary, handling forms and user input in Django involves **creating forms**, **validating the data**, and **handling form submission**. By using Django's built-in form handling features, you can easily handle forms and user input, ensure data integrity, and provide helpful error messages to the users.

## Implementing Authentication and Authorization

Implementing **authentication** and **authorization** in Django is relatively simple thanks to the built-in `django.contrib.auth` app. This app provides a set of views, forms, and templates for handling user authentication and authorization.

Authentication is the process of verifying a user's identity, and authorization is the process of determining what a user is allowed to do.

Django's built-in authentication system uses a user model that defines the fields and behavior of a user, such as the username, password, email, and groups. The user model is defined in the `django.contrib.auth.models` module, and you can use it as is or create a custom user model that inherits from it.

To enable authentication in your Django project, you'll need to add `django.contrib.auth` to the `INSTALLED_APPS` list in the `settings.py` file. Then you can use the built-in views and templates for handling **login**, **logout**, and **password management**.

Here's an example of how to use the built-in views to handle login and logout:

```python
from django.contrib.auth import views as auth_views

urlpatterns = [
    path('login/', auth_views.LoginView.as_view(), name='login'),
    path('logout/', auth_views.LogoutView.as_view(), name='log
```

### Thanks

Thank you for reading this guide on Django basics. We hope that you have found it informative and useful in getting started with building web applications using Django. If you have any questions or feedback, please feel free to reach out. We wish you all the best in your journey to become a Django developer.

### Author

I'm **Radhwen**, computer Science student with a passion for coding and a drive for success. Always pushing for growth and learning new technologies.

Connect with me :

- [Github](https://github.com/mhimidradhwen)
- [Linkedin](https://linkedin.com/in/radhwenmhimid)


