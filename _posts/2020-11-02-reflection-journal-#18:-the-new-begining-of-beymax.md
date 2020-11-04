---
date: 2020-11-02 07:32:44
layout: post
title: "Reflection Journal #18: The new begining of Beymax"
subtitle:
description:
image: https://github.com/polowis/Pandoru_template/blob/master/docs/beymax.png
optimized_image:
category:
tags:
author: polowis
paginate: false
---

Beymax has put a new begining of writing Flask applications for me. It has many features that I would like to use because I built it. But I think it would be better if this can be used by students so they can observe and learn from it. I tried to keep the code as simple as possible but I can't promise it will continue to be like this because as time goes on, there will many things need to be added resulting in complicated code base. I took a look at Flask code base, personally, I think it's quite hard to read it because basically the developers put everthing in one file. And one file has around 4000 lines! 

### The command line
I presented this project to many of my friends and they absolutely love it. It has many things that Flask does not have and also it has a similar way of writing code. It took me 2 days to figure out the best method to create custom commands. Here is the result: 

```py
from .command import Command
from ..http.server import Server

class Core_command(Command):
    command_prefix = 'run'
    description = 'Run the server'
    keys = '?port'

    def construct(cls):
        cls.register()
    
    
    def handle(self, args):
        Server.run(args.port)
```

To use this just simply run ```beymax run```. By extending the class ```command```, we can easily create any command line a lot easier. No need ```click```, other any other libraries. Use ```args``` if you want to retrieve the input from the command line. When you run ```pip install beymax```, the beymax keyword will be setup automatically. 


### Template engine (bx)

Next one is the template engine. It has similar structure to Jinja2, a Flask's template engine. Here is a few example:

```py
html = """
<h1>Hello {{name}}!</h1>
"""
template = Template(html, name="Jack").render()
# or
template = Template(html).render(name="Jack")
```

If statement:
```py
html = """
<div> {% if condition > 1 %} <h1>Hello</h1> {% endif %}</div>
"""
```
Loop:

```py

html = """
<div> {% foreach users as user %}
    <li>{{user}}</li>
    {% endfor %}
<div>
"""
```
Beymax also provides a way to write your own Validation Rules by extend the class as well!

