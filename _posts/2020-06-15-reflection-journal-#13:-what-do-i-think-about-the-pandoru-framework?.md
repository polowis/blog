---
date: 2020-06-15 01:16:05
layout: post
title: "Reflection Journal #13: What do I think about the Pandoru framework?"
subtitle:
description: week 17, semester 1.
image: https://world.edu/wp-content/uploads/2018/06/COMPUTER-SCIENCE.jpg
optimized_image:
category: blog
tags:
    - reflection
author: polowis
paginate: false
---

The ```Pandoru``` project is a 2 months project that provides a convenient way to build a web application using Flask. I use it in almost every web application project using Flask. Because I can setup in 2-3 minutes. 

## Is this built on top of Flask?

The special thing about this framework is that it uses Flask but you would never know that it uses Flask by just looking at the code. The code style and the structure is completely different. I achieve this by using MVC architecture and build it based on Ruby on rails and Laravel framework. After 2 months and several bug fixes and feature improvements, I'm happy with what I've done so far. ```Pandoru``` is stable and ready to use as a template for any project or for anyone that wants to see how exactly I built it. It's also great for beginners to view the source code of the project to gain a deeper understanding of how web framework is built. 

## Will there be more features?

I planned to provide a flexible way to structure the application using my framework but as it turned out, the project is just something I built for myself. In this way I can easily create or build an application without using much effort. There are many features that I would like to add to the project including routed localization, file handling in request and even larger, my own ORM framework inside this framework. Also I want to make a NodeJS package that will help with setting up ```Webpack```. About security, I planned to add throttle in order to limit the request can be made in a certain period of time to prevent ```DDOS```. However, I can only do it when I have time. 

## My experience with Flask

Because of ```Pandoru``` project, I have finally be able to use all the core functionality of Flask. I actually improved Flask by making it a lot clever. Instead of proving the function with string arguments instead I can now use method to represent my goal. For instance ```request.input('myinput')``` instead of ```request.form.get('myinput')```. Also, I have provides a better way to handle validation errors through ```validation``` class. Instead of making bunch of ugly validation statements, I can now do:
```py
form = FormRequest({
        'user_email': 'email',
        'username': 'alphanumeric',
        'remember_me': 'boolean'
})
if form.is_validated():
    return 
```

And It looks a lot cleaner that what it looked like 2 months before. 

Here is the link to the [project](https://github.com/polowis/Pandoru_template)