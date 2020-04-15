---
date: 2020-04-15 01:58:29
layout: post
title: "HTTP vs HTTPS"
subtitle:
description: Is HTTPS neccessary?
image: https://gnosysdigital.com/wp-content/uploads/2018/07/http-to-https.png
optimized_image:
category: blog
tags: 
    - https
    - http
author: polowis
paginate: false
---
There are lots of websites use HTTP. However, in 2014, Google advised them to switch to HTTPS. Until then, only e-commerce sites would really bother using HTTPS.
Many websites use HTTP. However, in 2014, Google advised websites to switch to HTTPS. Until then, only e-commerce sites would really bother using HTTPS. To encourage conversion, Google has announced that it will provide HTTPS sites with some minor rankings bump. 

Now you may be wondering - why is it so important that you switch to HTTPS? Do you really need to use it? What is the difference between HTTP and HTTPS?

### The concept
The first thing we should understand is what HTTP and HTTPS really are. It's difficult to understand the impact of switching from one to another or choosing between HTTP and HTTPS without an understanding of both.

#### What is HTTP?

<img src="https://images.viblo.asia/retina/2f11a9f7-96d4-4cc7-8bf5-aa5042eda309.png"/>

HTTP stands for HyperText Transfer Protocol, a basic protocol used for the World Wide Web (www) to transmit data in the form of text, images, video, and audio. and other files from Web server to web browsers and vice versa.

#### What is HTTPS?

<img src="https://images.viblo.asia/retina/4aa8124d-9b44-4db6-b1b6-81c758627b0b.png">

HTTPS stands for Hypertext Transfer Protocol Secure. The problem with the regular HTTP protocol is that the information from the server to the browser is unencrypted, which means it can easily be stolen. The HTTPS protocol overcomes this by using an SSL certificate (secure socket layer), which helps create a secure encrypted connection between the server and the browser, thereby protecting sensitive information from being stolen when communicating. The message is transferred between server and browser.

#### How do HTTP and HTTPS work?

HTTP operates on the Client – Server model. Clients will send a request to the server and wait for the server's response. To be able to exchange information with each other, all hosts and clients must implement on a unified protocol, which is HTTP.

To make it easier to understand, think about when you enter a web address and press enter. An HTTP request will be sent to the server to find the website you have entered. After the server receives the request, it will return the search to that requested website and return the result by displaying the website on your web browser. This process takes place quickly or slowly depending on your Internet connection. 

HTTPS works similar to HTTP but is added with SSL and the TSL protocol. These protocols ensure that no one other than the client and server can hack the information and data out. Regardless of whether you use a public or private computer, SSL certificates ensure that the client's communication with the server is always secure and protected from snooping.

To learn more about SSL, see [here](https://www.ssl.com/faqs/faq-what-is-ssl/).

#### The key difference between HTTP and HTTPS

The most important difference between the two protocols is the SSL certificate. In fact, HTTPS is basically an HTTP protocol with additional security. However, this additional security is extremely important, especially for websites that have sensitive data from users, such as credit card information and passwords.

1. HTTP uses port number 80 to communicate and to use HTTPS 443
2. HTTP is considered insecure and HTTPS is secure
3. HTTP operates at the application layer and HTTPS works at the transport layer
4. HTTP does not require any certificates, and HTTPS requires an SSL certificate

#### Switch to HTTPS

1. Buy SSL certificates and dedicated IP addresses from companies that host them.
2. Install and configure SSL certificate.
3. Make a full backup of your website in case you need to return to the original state
4. Configure any internal hard links in your site, from HTTP to HTTPS.
5. Update your code libraries, such as JavaScript, Ajax or any third party plugins.
6. Redirect any external links you control to HTTPS, such as directory listings.
7. Update the [htaccess](https://httpd.apache.org/docs/current/howto/htaccess.html), such as Apache Web Server, [LiteSpeed](https://www.litespeedtech.com/), [Nginx](http://nginx.org/en/docs/beginners_guide.html) Configurate your internet service management function (such as Windows Web Server), to redirect from HTTP to HTTPS.
8. If you are using a [CDN](https://en.wikipedia.org/wiki/Content_delivery_network), update the SSL settings of the CDN.
9. Implement 301 redirects on each page.
10. Update any links you use in marketing automation tools, such as email links.
11. Update any landing page and paid search link.
12. Set up HTTPS website in [Google Search Console](https://search.google.com/search-console/about?hl=en&utm_source=wmx&utm_medium=wmx-welcome) and [Google Analytics](https://marketingplatform.google.com/about/analytics/).

<img src="https://cdn.ssl.com/wp-content/uploads/2019/10/faq-what-is-ssl-01b.png?x90506">

HTTPS is obviously more secure than HTTP in encrypting data and protecting personal information. However, the advantage of HTTP is that the response speed of the website is much faster than HTTPS and is used for news sites that need information quickly, when the website need data such as bank accounts, personal emails. use HTTPS. In addition, we can easily identify with the lock icon on the address bar to distinguish whether the website uses HTTPs or not.
