---
date: 2020-09-08 05:09:07
layout: post
title: "Reflection journal #15: Changes to the core (framework)"
subtitle:
description:
image: https://www.reachfirst.com/wp-content/uploads/2018/08/Web-Development.jpg
optimized_image:
category:
tags:
author:
paginate: false
---

## The Flask framework

Currently, I'm focusing on making my own framework which is Pandoru a lot more powerful than before. I recently introduced a new Mailing system that is built inside Pandoru as an additional module so that people can easily sending emails without have to worry which library or frameworks need to be installed. Here is an example of people can use it:

```python
from app.framework.util.mail import Mail
class VerificationEmail(Mail):
    def build(self):
        (
            self.from_sender("example@gmail.com")
            .to("receiver@example.com")
            .view("path/to/emaillayout")
            .attach({"receiver": "John", "subject": "Hello"})
        )

class AuthController(Controller):
    def after_register(self):
        Mail.send(VerificationEmail())
```

People can also be able to specify their own layout beside just plain text. I use Jinja2 as a frontend compiler to generate the HTML for email in order to reduce the need of having to learn another syntax for building the layout.

There will be also an upgrade on security which is about limiting the request to prevent DDOS attack and a cache handling system to speed up the performance of the website. For example, let's say you have a function that takes 2 minutes to execute. And then the user just request that function again and still it will take another 2 minutes to execute just to show the same result, therefore, we can store the result in cache for a short amount of time so instead of having to re-execute, we can just load them from cache.

## The website

I'm working on the recommendation system and also update on the models. All models are now stored in a model folder (*just like before*) but instead of having to load the model manually, it is now loaded automatically. For the controllers folder, I'm working on to load the controllers from sub-folder automatically and also there will be an update on Validation request. 

I also implemented a configuration command. All configuration files are stored in a configuration folder which is also stored in a framework directory. The copies of all these files are stored in the app directory and will be used and loaded automatically when the web application runs. The issue is what if you or someone accidentally deletes or erased some important configuration keys and they have no idea what it is. To solve this issue, I decided to store the configuration files in framework directory and only use the copies in app directory so in case if I accidentally delete, I can easily generate them by using this command:

**To load only mail configuration**
```sh
$ flask config:mail
```



