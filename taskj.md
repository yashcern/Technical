# **Task Manager Web application using Django Framework**

### **About Project**
1. TaskMannager a web-based application that can add store your task in an unordered list.
2. You are going to use the basic features of the Django framework to build this web application.
### **Prerequisite**
1. Python.
1. Object-Oriented Programing (eg. Classes)
1. Command Line Interface (CLI).
1. Markup language.
***
### **1. Installing the Dependencies**
1. [Python](https://www.python.org/downloads/) 
1. Code Editor. (any of your choice)
   1. [Vscode](https://code.visualstudio.com/).
   1. [Sublime Text](https://www.sublimetext.com/3).
   1. [Atom](https://atom.io/).     
1. Django framework for building dynamic web applications.

You can use "pip" the python package manager to install Django using the command line interface easily.

To install Django you can Type in Your CLI.
````
pip install Django
````
***
### **2. Create a project on Django**
To do this you can run a command in your CLI with project "name"
````
django-admin startproject task
````

> Note: "task" is the name of the project. (You can give any name of your choice)

Now Type ``ls`` in CLI you will see "task"  directory or folder has been created for you by Django.

Open this  "task"  directory or folder in your code editor and you will see pre-created files in it by Django for you.

>Note: Look at the file menu of your editor

#### **Important pre-created files**
1. ``manage.py`` - To execute commands on this Django project
    #### Application of ``manage.py``
    1. To run the webserver
    1. To create an app inside the project.

2. ``setting.py`` - Contain important configurations - preloaded with a couple of default settings. You might want to make some modifications to how the application behaves.
3. ``urls.py`` - Table of contents for your web application that has the number of different URLs or routes that you can visit.
***
### **3. Create an application**
To do this you can run a command in your CLI with application "name"
````
python manage.py startapp taskmannager
````
> Note: "taskmannager" is the name of the project. (You can give any name of your choice)

So you created a Django project called "task" and inside of which you have created your application that you have called taskmannager.

In addition to the "task directory,"  you now have "taskmannager" directory the Django created for you - that contains the information needed for the "taskmannager" app to work and inside it again pre-created files are generated.
> Note: Look at the file menu of your editor.

#### **Important file in taskmannager app**
``views.py`` - describe what it is the user sees when they visit a particular route. For eg., you can decide what gets rendered to the user.

> **Django Project generally consist of one or more Django application. One project may have multiple applications within it. Django comes with its ability to take a project and divide it into multiple distinct apps.**
***
### **4. Install "taskmannager" app**
To install "taskmannager" app into the "task" project. Go into the ``setting.py`` created by Django with the default configuration for the Django project

Scroll down into the ``seting.py`` you will see a variable called INSTALLED_APPs [ ].

Now type the name of your application which is "taskmannager" inside the square brackets.

![GO](https://github.com/yashcern/img/blob/img/q1.svg)

Notice the line "34" in the above figure.
You successfully added your app to this particular project.
Now "taskmannager" is an installed app on this particular project.

> Note.1: Line 35 to 40 are preinstalled Django apps listed in the INSTALLED_APPS variable.

***

### **5. Create URL configuration**
To do this You need to go into the "task" directory and look at ``urls.py`` (for the entire project).

![a](https://github.com/yashcern/img/blob/img/z2.svg)

1. Add "include" you can see at line 17.
2. This  is the table of contents for Your entire web application you can see at line 21 under the variable urlpattern 

Now create a new file ``urls.py`` (for the taskmannager app) inside the "taskmannager" directory and type.

````python
from django.urls import path

from . import views

app_name = "tasks"    # To prevent from name space colision if in case you have multiple applications inside one Django projrct

urlpatterns = [
    path("", views.index, name= "index")
    path("add", views.add, name= "add")
]
````
***
### **6. Create HTML files.**
To do this go to the task directory and create a folder named "templates" then create again a folder named "tasks" inside the "templates". Now create "HTML" files inside it.

>(folder)task -> (folder)tempelates -> (folder)tasks -> (file)HTML

Fundamental HTML syntex:
````HTML
<!DOCTYPE HTML>
<html lang = "en" >
<head>
    <title>Task manager</title>
</head>

<body>

</body>

</html>
````

Now create HTML files with names as:

1. index.html
1. add.html
1. layout.html

![a](https://github.com/yashcern/img/blob/img/z3.svg)

Now in the **``layout.html``** type inside the body 
````HTML
<!--Portion in the body might change-->
<!--depending upon the change in index.html and add.html-->
<body>                  
   {% block body %}   
   {% endblock %} 
</body>
````

Now in the **``index.html``** type 
````HTML

{% extends "tasks/layout.html" %}

{% block body %}
       <ul>
          {% for task in tasks %}
                <li>{{ task }}</li>
          {% empty %}
                <li>No tasks.</li> 
          {% endfor %}
        </ul>

        <a href="{% url 'tasks:add' %}">ADD a new Task</a>  <!--link for redirect to add.html-->
        <!--Django fetches wherever this 'tasks:add' is located in the directory-->

{% endblock %}  
````

Now in the ``add.html`` Type:
````HTML
{% extends "tasks/layout.html" %}  

{% block body %}
  <h1>Add Task</h1>
  <form action ="{% url'task:add' %}" method = "post">
    {% crf_token %}  <!--To prevent form cross-site request forgery-->
                    <!--Server-side application generates a unique unpredictable value -->
    {{ form }}
    <input type = "submit"> 
   </form>
    <a href="{% url 'tasks:index' %}">view Task</a> <!--link for redirect to index.html-->
    <!--Django fetches wherever this 'tasks:index' is located in the directory-->

{% endblock %} 
````
***
### **7. Creating a View**
To do this go into the "taskmannager" directory and look at ``views.py`` inside your taskmannager app

![a](https://github.com/yashcern/img/blob/img/z1.svg)

In order to create a view you need to import some python modules then define a class and then define functions.

#### Here are Some important python modules

````python
from django import forms
from django.http import HttpResponseRedirect
from django.shortcuts import render
from django.urls import reverse
````

#### Now define a class with the name "NewTaskForm" in Python

````python
class NewTaskForm(form.form):
   task = forms.CharField(label="New Task") # It create a type bar with label "New Task"
````
Now define a function with the name "index" in Python

````python
def index(request):
    if "tasks" not in request.session:
        request.session["tasks"] = []
    return render(request, "tasks/index.html", {
        "tasks": request.session["tasks"]
    })
````

Now define a function with the name "add" in Python

````python
def add(request):
    if request.method == "POST":
        form = NewTaskForm(request.POST)
        if form.is_valid():
           form.cleaned_data["task"]
           request.session["tasks"] += [tasks]
           return HttpResponseRedirect(reverse("task:index"))
        else:
        return render(request, "tasks/add.html",{
            "form": form
        })

    return render(request, "tasks/add.html",{
        "form": NewTaskForm()
    })

````
> Note Django can automatically do client-side validation.
***
Now in your CLI type:
````
python manage.py migrate
````
This allows us to create all of the default tables inside of Django's database

***
### **8. Run web application**
To run this web application type in your CLI. 
````
python manage.py runserver
````
You will get starting development server at **``http://127.0.0.1:8000/``**. Copy it and paste it in your **"Browser"**.

Initially your tasks list is Empty!

![a](https://user-images.githubusercontent.com/68737803/107150992-6078dd80-6986-11eb-926a-eb42e13a220c.jpg)



![a](https://user-images.githubusercontent.com/68737803/107150869-ef392a80-6985-11eb-8cd6-aa50670762c2.jpg)

Now Let's add a task and click on Submit. ⬇️

![a](https://user-images.githubusercontent.com/68737803/107150706-34109180-6985-11eb-85f3-ec47abf25c1b.jpg)

Now you can see one entity added to your task list. ⬇️

![a](https://user-images.githubusercontent.com/68737803/107150602-843b2400-6984-11eb-83f1-171d01db1b01.jpg)

You now have your Task Manager web application currently running on your local computer.

"Technical" 
