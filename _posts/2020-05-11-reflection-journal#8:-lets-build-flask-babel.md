---
date: 2020-05-11 05:25:43
layout: post
title: "Reflection Journal#8: let's build flask-babel"
subtitle:
description: Week 12, Semester 1.
image: https://www.ulatus.com/translation-blog/wp-content/uploads/2017/04/Web_Localization.jpg
optimized_image:
category: code
tags: 
    - python
    - flask
    - pandoru
author: polowis
paginate: false
---

Flask-Babel is a flask extension that provides a convenient way to retrieve string in various language. So you can easily support multiple languages in your application. the problem with babel is the configuration is confusing, hard to implement and lack of documentation. 

So I decided to come up with a new simple way of implementing localization. A simpler package but still meets the goal. 

The goal here is instead of having to create a class instance and doing all of the configuration in there. All you need to do is to modify the localization configuration file which I have already implemented. And then in the ```config.py```, I just need to enable to localization setting and that will do all the setup for me.

## Where are the files that store the translation?

Good question. The files are stored inside ```app/resources/lang``` directory. And it must be in standard JSON format. For example, If I want to store the translation, my resources directory would look something like this:
```
\resources
    \lang
        \en
            validation.json

```

## Use translation strings as keys

Translation files that use translation strings as keys are stored as JSON files in the resources/lang directory. For example, if your application has a Spanish translation, you should create a ```resources/lang/es.json``` file. And you should give it a proper name so later you don't get confused when using them in your application. 
```js
{
    "greeting_user": "Hello user";
}
```

## Retreive the translation

By default the package already provides a translation method which is already implemented in ```view``` (only after you enable localization in the ```config.py```). Now the fun part is that this package supports ```dot``` syntax. Let's say you have a translation which is store in ```app/resources/lang/en/validation.json``` and the structure of ```validation.json``` looks something like this:

```js
{
    "email": "Please provide a valid email address"
}
```

To retrieve the translation you may use the following syntax 

```html
{% raw %}
<h1>{{locale.lang("validation.email")}}</h1>
{% endraw %}
```

It will first look for your locale and see if it is supported by the application. Then it will look into the folder ```resources``` in order to get the translation needed. 

## Replace parameters
Another feature that make you will be happy with this package is the ability to replace placeholders. 

You can define placeholders in your translation strings. All placeholders are prefixed with a ```:```. For example, you may define a welcome message with a placeholder name:
```js
{
    "welcome": "hello :name"
}
```
And then to retrieve the value of the translation


```html
{% raw %}
<h1>{{locale.lang("greeting.welcome", name="John")}}</h1>
{% endraw %}
```

output: 
```js
{
    "welcome": "hello :name" // output => hello John
}
```

## Pluralization and routed localization

Here are some complex problems that I will need to address in future versions. Again this is built inside my template, if you don't want to use the template, just copy the core code and paste it in your application. 