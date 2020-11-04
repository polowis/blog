---
date: 2020-10-20 05:47:48
layout: post
title: "Reflection Journal #17: Custom server"
subtitle:
description: semester 2, week 12
image: https://i.ytimg.com/vi/N3bdPGPyGfQ/maxresdefault.jpg
optimized_image:
category:
tags:
author: polowis
paginate: false
---

Thse website is built using Flask and Beymax. Beymax has two version, the first version is a standalone version, which means it is dependency free. It does not depend on any other libraries at all. The second version which is a flask version is the version that built on top of Flask. 

In the standalone version, I built a separate web server that serve for multiple purposes. A simple web server can be written as below:

```py
import socketserver
import http.server
import threading, logging
from .request_handler import RequestHandler

class Server():

    @staticmethod
    def run(port=None, host="", server_class=http.server.HTTPServer, handler_class=RequestHandler):
        """Run the server, this should not be used for production mode
        The code is not tested for security usage
        """
        handler_object = handler_class
        if port is None:
            port = 8000
            server = server_class((host, 8000), handler_object)
        else:
            server = server_class((host, int(port)), handler_object)
            
        server.allow_reuse_address = True
        logging.info('Starting http server...\n')
        print('\033[93m' + f'Listening on port {port}' + '\033[0m', flush=True)
        print('\033[93m' + 'Use Ctrl - C to quit the process' + '\033[0m', flush=True)
        try:
            server.serve_forever()
        except KeyboardInterrupt:
            pass
        server.server_close()
        logging.info('Stopping server...\n')
```

There are many other issues that we need to deal with, for example routing. It's hard to build a web server from scratch but it allows us to understand deeper of how modern web frameworks work. The development of Beymax allow beginners, particular students who are interested in learning the core functionality of web frameworks to see and explore how a web framework is built. And it can bring them to the next level of understanding technical problems. 

A request handler part is a little bit more complicated. This is just a simple one, using it might cause several problems. 

```py
import http.server
from os import path
import mimetypes
import sys, logging
from .request import Request

routes = {'/': 'hello.html'}

class RequestHandler(http.server.SimpleHTTPRequestHandler):

    directory = path.abspath(path.dirname(__file__))
    
    def _set_header(self, path, content_view):
        if content_view is None:
            self.send_response(404)
        else:
            content_type = mimetypes.guess_type(path)
            self.send_response(200)
            self.send_header('Content-Type', content_type)
            self.end_headers()

    def do_GET(self):
        if self.path.startswith('/static/'):
            http.server.SimpleHTTPRequestHandler.do_GET(self)
        else:
            view = routes.get(self.path)
            content_view = self._get_content(view)
            self._set_header(self.path, content_view)
            if content_view is not None:
                self.wfile.write(content_view)
    
    def do_POST(self):
        '''Reads post request body'''
        content_length = int(self.headers['Content-Length'])
        post_body = self.rfile.read(content_length)
        logging.info("POST request,\nPath: %s\nHeaders:\n%s\n\nBody:\n%s\n",
                str(self.path), str(self.headers), post_body.decode('utf-8'))

        self.send_response(200)
        self.send_header('Content-type', 'text/html')
        self.end_headers()
        self.wfile.write(post_body)

    def _get_content(self, view_path):
        if view_path is None:
            return 
        with open(view_path, mode='r', encoding='utf-8') as f:
            content = f.read()
        return bytes(content, 'utf8')
```

By doing this, we can allow user to extend the WSGI server or use the one the like to use instead of this. There is also a better way to do this is by using ```wsgiref```, it is a built-in python module that is built on top of HTTP server, it serves the same purpose but it would be easier when it comes to production deployment. 

Beymax uses this concept as the staring point and because Beymax is desgined to work like Flask but provides many additional features to help beginners get started. 

### What about template engine?

Beymax has that as well. The syntax is similar to Jinja2 but it provides a way to add custom function by extending the class. In Beymax you can change everything just by extending the class. The code will be much better and cleaner than Flask. But beymax wasn't developed to be like this, personally I just wanted to show my understanding of web development by putting every piece together. Not all of them but the knowledge that I can't show it visually. 

### Current stage of Beymax

I'm planning to release another version. It basically changes almost everything Beymax has so far to another level. The way of using it will be the same as before, only the core functionality of the code changes. It took quite a few days to come up with a suitable design pattern .