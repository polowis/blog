---
date: 2020-09-08 05:09:07
layout: post
title: "Reflection journal #15: Changes to the core (framework)"
subtitle:
description: semester 2, week 8
image: https://www.reachfirst.com/wp-content/uploads/2018/08/Web-Development.jpg
optimized_image:
category: blog
tags:
    - flask
    - reflection
author: polowis
paginate: false
---

## The Flask framework

Currently, I'm focusing on making my own framework which is Beymax a lot more powerful than before. I recently introduced a new Mailing system that is built inside Beymax as an additional module so that people can easily sending emails without have to worry which library or frameworks need to be installed. Here is an example of people can use it:

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

### Behind Mailing system
Thanks to the community, we have Flask-Mail that helps you deal with sending email with Flask. However, you don't really need it, why? Because python already has built-in support for sending email! The problem with python built-in module is that, it's quite hard for beginners to understand how to use it. Let's start with a simple example

```python
import stmplib
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
from email.mime.application import MIMEApplication
from email.utils import formatdate
from email.mime.image import MIMEImage
```
We're gonna load all the neccessary modules here. By now you probably realize that it takes too much stuff just to send a single email while Flask-Mail handle all of it to you. However, building one by yourself isn't difficult. 

We need to create a class here

```python
class Mail:
    def __init__(self):
        self.host = self._init_host()
        self.use_ssl = True 
```

The init host method. We need to provide the credentials keys to login to mail server
You need to specify the host, port, username and password. For security reason, I'm not going to show the sensitive information here but if you wish to obtain one, you can. There are plenty of email services that you can sign up for such as smtp, mailgun, etc.

```py
def _init_host(self):
    if self.use_ssl:
        host = smtplib.SMTP_SSL(MAIL_HOST, MAIL_PORT)
    else:
        host = smtplib.SMTP(MAIL_HOST, MAIL_PORT)

    if MAIL_USE_TLS:
        host.starttls()
        
        assert MAIL_USERNAME, "You must provide a username in order to login securely"
        assert MAIL_PASSWORD, "You must provide a password in order to login securely"

        host.login(MAIL_USERNAME, MAIL_PASSWORD)

        return host
```
So in case if you forgot to put your username and password in, it will raise errors. Simple as that, once you login, you can start sending email straight from python. You can customize the class however you like, for example in my own framework

```py
def add_recipient(self, recipient):
        """Adds another recipient to the message.
        :param recipient: email address of recipient.
        """

        if type(self.recipients) == list:
            self.recipients.append(recipient)

        if type(self.recipients) == str:
            temp = self.recipients
            self.recipients = [temp].append(recipient)

    def charset(self, charset):
        """set the character set of this email"""
        self.email_charset = charset
        return self
    
    def html(self, html):
        """Inject html code \n
        """
        if isinstance(html, str):
            self._html = html
        else:
            raise TypeError("html must be a string")
        return self

    def view(self, filename):
        """attach a HTML layout to this email"""
        self.env_template = self.env.get_template(filename)
        self.filename = filename
        return self

    def set_view_directory(self, directory_path):
        """set the path to the folder containing the view of the email \n
        :param directory_path: path to the directory
        """
        self.current_directory = directory_path
        return self
```

The last step is to send the email. This is the most important part as you need to specify the content type. Sometimes, you want to send a text instead of html, or you might want to send html instead. For plain text, you might want to consider using this

```py
self.msg = MIMEMultipart()
self.msg.attach(self._mimetext(self.text))
if(type(self.recipients) == dict):
    for recipient in self.recipients:
        self.host.sendmail(self.from_sender, recipient, msg.as_string())
    else:
        self.host.sendmail(self.from_sender, self.recipients, msg.as_string())
self.host.quit()
```

You might want to consider joining all recipients instead of looping through the list. There are more things need to be done here. How would you attach attachments? How would you let the users use html as a layout to send email? With Beymax, it handles it for you, including the most popular content formating (**Markdown**) that Flask-Mail does not have (1 point for Beymax :D), Beymax uses OOP structure ( + 1 point) Beymax allows users specify extra css links in html content through ```.add_css_link()``` method.  Instead of providng the whole text, users can also add text using ```line()``` method. By using this method, it automatically adds new line after each usage. For example:

```py
self.line("My header 1").line("My header 2")
```