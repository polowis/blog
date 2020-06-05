---
date: 2020-05-19 23:17:22
layout: post
title: "Reflection Journal#9: The power of my own framework"
subtitle:
description: week 13 semester 1
image: https://res.cloudinary.com/springboard-images/image/upload/q_auto,f_auto,fl_lossy/wordpress/2019/07/sb-blog-programming.png
optimized_image:
category:
tags:
author: polowis
paginate: false
---


In the previous posts, I have mentioned a lot about the Pandoru framework. Pandora is a flask project template that provides a convenient way of building web applications. I’ve been working on the documentation for this template and expect to release it soon. 

## Why did I come up with this idea? 
Talking about my experience in web development, I have one year experience developing trading web service using PHP programming language and have been maintaining it. So I know exactly how I should structure a project that will make my life easier later on. 

The way flask provides to you is just a bunch of functions or methods without any set of rules on how to structure it. And that makes it hard when it comes to large scale projects or long term projects due to the fact that most people don’t have a good sense of how to structure their project. 

My template introduces a friendly approach using MVC architecture. Instead of having to store all the routes inside ```route.py```, you can now separate it into different files ending with the word ```controller```. And then my template will handle all the routing for you. 

Another problem with flask is too much import statement. If you have a chance looking at other beginners’ projects, you can see that there are a bunch of import statements on the top of every single file. At some time, it might get really frustrating due to the fact that every time you make a new file you have to do it again. To solve this issue in my template, I created a class called ```controller``` and every time I want to make a new route or a new logical part of the system, I can just inherit this class. It can handle routing, redirect, view and many more. 

Next part is the configuration. As previously mentioned, Flask is not that well structured. Therefore the configuration key of the whole project can look like a mess. For example: 

```py
app.config['APP_URL'] = 'http://mywebsite'
```

So if you have bunch of configuration key-value pairs, it will be a mess when trying to implement it. To make this easier. I have created a file called ```config.py``` with comments along with it. Instead of doing like the above code, you can do it like this:

```py
APP_URL = 'http://mywebsite'
APP_KEY = 'mysecretkey'
```
And my template will handle the rest for you including the configuration of database. 

## Default error layout

Another good thing about this is it has bunch of error views for different HTTP status codes. Which is sometime Flask doesn't have.

## How much time do I spend on creating another flask project?

With this approach, it takes about 5 minutes to setup the project.