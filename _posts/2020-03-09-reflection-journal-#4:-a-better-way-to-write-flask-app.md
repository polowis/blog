---
date: 2020-03-09 01:26:38
layout: post
title: "Reflection Journal #4: A better way to write Flask app"
subtitle:
description: Week 5, Semester 1. A closer look at Flask's core
image: https://www.goodfirms.co/glossary/wp-content/uploads/2017/07/Web-Framework.png
optimized_image:
category: blog
tags:
    - Python
    - programming
    - flask
author: polowis
paginate: false
---

One of the reason I like Flask is because it has no pattern of project folder structure, it is very flexible. It is very good for beginners as they can learn how to organise their applications such as routes, view function, templates, etc. For small application, compare to Django (A python fullstack framework), Flask gives more performance. 

But in my point of view, I want to make something simple and easy for my future develoments with Flask. I don't want to spend too much time on writing view functions and write every single route. Or everytime I write something, I will need to import it to the main file. 

The development of this package or framework will be somewhat complicated but will handle most of the hard works for me. Following the previous post [The flask microframework](https://polowis.netlify.com/reflection-journal-2-the-flask-microframework/)

## The new approach

Building a new framework on top of Flask. All of the below functons are implemented inside framework folder. If you wish to change the location of the framework folder, the path will need to rewrite. For example, ```app.framework.requests``` to ```pandoru.request``` or ```framework.request``` if you would like to move it outside app folder.

### The power of controller

As discussed earlier in the previous [post](https://polowis.netlify.com/reflection-journal-2-the-flask-microframework/), I have made somewhat big update for the controller part. Before, everytime I delcared a controller file, I had to import it through ```__init__.py``` file. And then that file was automatically imported to kernel folder. It took quite a few works to do it but with new version. All I have to do is to create a ```.py``` file with the word ```controller``` or ```Controller``` at the end. 

This is the full code for this part
```py
"""
Reposible for handling controller files. DO NOT MODIFY!
"""
import os
controller_files = os.listdir('./app/http/controllers')
controllers = {}
files = []

def file_endswith(filename, suffix):
    """Check if file correctly ends"""
    return filename.endswith(suffix) or filename.endswith(suffix.lower())

for controller_file in controller_files:
    filename, file_extension = os.path.splitext(controller_file)
    if file_extension == '.py':
        controllers.update({filename: file_extension})

for controller_name in controllers.keys():
    if file_endswith(controller_name, 'Controller'):
        files.append(controller_name)


__all__ = files
```

This will need to be improved such as recursively looping through subfolders. 

### The Flask request

In the [docs](https://flask.palletsprojects.com/api/#flask.Request) describes all the available attribute available on request. Some common attributes are ```request.args``` ```request.form``` ```request.values``` follow by either ```get()``` or ```getlist()``` functions. However, the code will look a lot messy. We have to remember the correct attributes and if we use the wrong one it will not work properly.

I came up with the new way of approaching. 

#### Retreive input value

By simplify a few methods, I can access all of the user input without worrying about which HTTP verb was used for the request. Regardless of the HTTP verb, the input method may be used to retrieve only user input:

```python
email = request.input('email')
```
#### Retreive input from query string
While the input method gets values from form request. The query method will only retrieve values from the query string:

```python
name = request.query('name')
```

If I wish to retrieve all query string value, I can call the query method without any arguments
```python
query = request.query()
```

#### Determine if an input value is present
The ```has()``` method is used to determine if a value is present on the request. The ```has()``` method returns ```True``` if the value is present on the request

```python
if request.has('name'):
    pass
```
Or I can also give them an array of values to check. The ```has()``` method will return ```True``` if all values in array are present

```python
if request.has(['name', 'email']):
    pass
```

Or I can also check if the value is present and is not empty
```python
if request.filled('name'):
    pass
```

### I don't need Flask WTForm.

Let's take a look at a simple controller that handle our routes

```py
from app import app
from app.framework.controller import *
from app.framework.requests import FormRequest
from app.framework.requests.request import request



class UserController(Controller):
    def construct(cls):
        UserController.register(app)

    @route('/dashboard', methods=['GET'])
    def dashboard_view(self):
        return view('dashboard')

    @route('/login', methods=['GET'])
    def login_view(self):
        return view('auth/login')

    @route('/login', methods=['POST'])
    def login_action(self):

        pass
```

Now I need to fill in my ```login_action()``` method with the logic to validate the new user post. To do this, I can use ```FormRequest()``` class from ```framework.request``` then I will need to check if the validation passes. To do this, I can use the ```is_validated()``` method provided by ```framework.request``` . If the validation rules pass, the code will keep executing normally. 

However, if it fails, an exception will be thrown and the proper error response will automatically be sent back to the user.

```py

# import statement here


class UserController(Controller):
    def construct(cls):
        UserController.register(app)

    @route('/dashboard', methods=['GET'])
    def dashboard_view(self):
        return view('dashboard')

    @route('/login', methods=['GET'])
    def login_view(self):
        return view('auth/login')

    @route('/login', methods=['POST'])
    def login_action(self):

        form = FormRequest({
            'user_email': 'email'
        })

        if form.is_validated():
            user = {'username': request.input('user_email')}
            return view('dashboard', user=user)
        else:
            return redirect('/login')
```
There are many validation attributes such as
```py
form = FormRequest({
    'user_email': 'email',
    'username': 'alphanumeric',
    'remember_me': 'boolean'
    #more goes here...
})
```
### Display errors from FormRequest

You can just display the errors like how you usually do in flask
```html
{% raw %}
{% with messages = get_flashed_messages() %}
      {% if messages %}
        {% for message in messages %}
            <p style="margin-left: 10px; color:red;">{{ message }}</p>
        {% endfor %}
      {% endif %}
    {% endwith %}
{% endraw %}
```
Alternatively, if I wish to display specify message, I can just simply use the ``` error_message()``` function. Again, this is built inside framework folder. I don't have to reimplement it again for other projects

```html
{% raw %}
{% if error('user_email') %}
      <p style="margin-left: 10px; color:red; font-size: 13px;">{{ error_message('user_email')}}</p>
      {% endif %}
{% endraw %}
```
## Future plans

I want to improve the FormRequest class instead of just validate one attribute: 

```py
form = FormRequest({
    'user_email': 'email',
    'username': 'alphanumeric',
    'remember_me': 'boolean'
    #more goes here...
})
```

I would like to make it better by having more attribute field
```py
form = FormRequest({
    'user_email': 'required|email|max:32|min:1',
    'username': 'required|alphanumeric|min: 8',
    'remember_me': 'boolean'
    #more goes here...
})
```

And instead of having like ```form.is_validated()```, the validation part can just redirect the user back with error code if fails, there is no need to have ```is_validated()``` function. 