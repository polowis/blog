---
date: 2020-02-21 23:28:28
layout: post
title: "Reflection Journal #2: The flask microframework"
subtitle:
description: Week 3, Semester 1. How I build my own web framework.
image: https://upload.wikimedia.org/wikipedia/commons/thumb/3/3c/Flask_logo.svg/1200px-Flask_logo.svg.png
optimized_image:
category: blog
tags:
 - web
 - programming
 - flask
 - python
 - reflection
author: polowis
paginate: false
---

## When to use Flask?
Web framework isn't new to me, I have worked on several different frameworks such as Express, Laravel, Symfony, etc. Flask isn't new to me either, I have read Flask's documentation a while ago and personally, I think flask may not always be a good tool to use for the job. 

**Why is that?**

Flask's API is very limited, unlike Django's. You can't do much with flask unless you have to use other libraries or flask extension such as flask-login, flask-classy, etc. Flask is not a bad choice, I like flask because of its flexibility and simplicity. It has no design or structure pattern, basically you can do whatever you want. But when it comes to building a larger application, Django might does a better job at handling. 


## The Pandoru framework

When it comes to the matter of clean code, flask isn't the right choice. The code might look great at the beginning, but as I expanding the web, it looks a lot more messy. 

Given this routing example
```python
@app.route('/', methods=['GET'])
def index():
    return render_template('index.html')
```

Imagine if we have hundreds of routes, how will the code look like? How readable is it?

I tried to simplify the code, and it looked as follow:

```python
@app.route('/', methods=['GET'])
def index():
    return MyController.myfunction()
```

Hmm, so it looks a lot cleaner, but there are few things that I want to get rid of. the function and the route part. How am I going to implement this? The code base is somewhat big, here are some examples of it. 

```python

#flask framework
@app.route('/', methods=['GET'])
def index():
    return MyController.myfunction()

#pandoru framework (my framework)

#first way
route.get('/', MyController.myfunction)

#second way
route.get('/', 'app.http.controller.mycontroller.myFunction')


```
But still, it did not satisfy me. I tried to improve it. After many hours, here is my additional feature on routing and controller

```python
from app import app
from app.framework.controller import Controller, route
from app.framework.requests import requests

class MyController(Controller):

    route_prefix = '/myprefix' # optional
    middleware = 'mymiddleware' # optional

    def construct(cls):
    """
    This function is automatically called when the app runs
    """
        MyController.register(app)
    
    """
    Register a new route
    """
    @route('/home', methods=["GET"])
    def home(self):
        return self.view('home')
    
    """
    Another way of getting request
    """
    @route('/create/blog', methods=["POST"])
    def create(self):
        blog_name = requests('name')

    

```

And that's it. Just by defining a few things, everything will be setup correctly. I'm still trying to improve this framework so it can handle most of the stuff for me such as authentication, controller, model, validation, routing,database, etc.