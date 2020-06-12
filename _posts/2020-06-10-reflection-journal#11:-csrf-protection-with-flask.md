---
date: 2020-06-10 03:04:30
layout: post
title: "Reflection Journal#11: CSRF protection with Flask"
subtitle:
description: week 15, semester 1.
image: https://www.netsparker.com/statics/img/webo/same-site-cookie-diagram-1.png
optimized_image:
category:
tags:
    - flask
    - reflection
    - csrf
    - security
author: polowis
paginate: false
---


## What is CSRF and why should you care?

When we compare one web application to the other web application, we talk about the performance, the features it provides and the security of the web application. Imagine if your website has too many security vulnerabilities, do you think the users will trust your service? No, absolutely no. There is no way they can make sure that their sensitive information is securely stored. There are many types of attacks introduced by **OWASP** (Open Web Application Security Project), and one of our luckly candidates in previous years is **CSRF**.

### What is CSRF?

**CSRF** (Cross Site Request Forgery) is a technique that attacks the web applications by using user's authentication authority on a website. So hackers can perform any action that require authentication. 

**CSRF** is also known as **session riding**, **XSRF**

### History

**CSRF** attacks have beeen around since 1990s, but these attacks came from users IP addresses, so the log files of website did not show any sign of **CSRF**. **CSRF** attacks have not been fully reported until 2007, with a few papers describing the details of **CSRF** attacks. 

In 2008, it was reported that about 18 million **eBay** accounts in **Korea** lost thier personal information. Also in 2008, a huge number of **Mexican** bank accounts lost their personal information. In both cases, the hackers used **CSRF** to attack. 

### How it works

First, we need to understand how web applications work. Web applications work by receiving HTTP requests from the user and executing them. **CSRF** attack is a technique that trick users' browser to send HTTP requests to the web applications. This can be done by inserting malicious code or linking to websites that the user has already authenticated. In this case, the user's session has not expired and therefore the HTTP request will get executed with user's authentication rights.

<img src="https://s3.amazonaws.com/com.twilio.prod.twilio-docs/original_images/el0otNMnu_Py7VYSUmd3KfC2f58jVjIAX24ff6e8DnY2qrh3Jw9pKYyGN4qQIIg2Dnl39evUcD.png">

Consider the following example:

Alice goes to her favorite web application as usual. Another user called Bob posts a message to a forum. Assume that Bob has a bad idea and he wants to delete all important project that Alice is working on. In this case, Bob is very smart so he will create a forum post including the following malicious code. 

```html
{% raw %}
<img height="0" width="0" src="https://mywebapp.com/projects/destroy">
{% endraw %}
```

In order to maximise the effectiveness, Bob tend to add the image with 0x0 pixel in size and cannot be seen by the user. In this way, Alice might not pay attention to the post.

Let's say Alice is accessing her account at the given address ```mywebapp.com``` and has not made a logout to finish. By viewing the post from Bob, Alice's browser will attempt to load the image from the given ```url``` as above and therefore execute the delete request.

The web application receives the request and says, ``` yes, this guy is authenticated and should be able to make the changes```. Potentially, the web application will delete all the projects from Alice's account. 

Of course there are many techniques beside injecting the image tag but for the sake of simplicity, we will not look into too much details. 

## Prevent CSRF in Flask app

There are many ways to prevent CSRF attack. 

These include **captcha** validation. **Flask** provides a handy way to prevent CSRF by using ```CSRFProtect()``` class. To make it easier, I decided to include the configuration logic inside my template. In the template, I have added an easy way to configurate CSRF protection. In the ```config.py```, you can configure CSRF protection by either enable or disable it.

```py
#Enable CSRF protection
CSRF_ENABLED = True
```
