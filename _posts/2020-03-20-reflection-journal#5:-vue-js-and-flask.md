---
date: 2020-03-19 00:38:15
layout: post
title: "Reflection Journal#5: VueJS and flask"
subtitle:
description: Week 6, Semester 1. Using vue in flask project
image: https://hackernoon.com/hn-images/1*ACR0gj0wbx91V_xgURifWg.png
optimized_image:
category: blog
tags:
  - programming
  - reflection
  - flask
  - javascript
  - vue
author: polowis
paginate: false
---
JavaScript is currently the trendiest programming language with many libraries and frameworks come out frequently. When it comes to the popularity, React is still the king and Angular. Beside, there is Vue. 

VueJS is a JavaScript framework used in building user interface(UI) and single page applications (SPA). I have experiences using VueJS before, so I thought it would be easier to use it again. But this time with the Flask framework.

## Realtime To do list 

I will be using Socket.io (The JavaScript realtime engine) and VueJS. 

### Laravel-mix
Laravel-mix is a great library. Although Laravel-mix is optimized for Laravel project, I can also use it for other type of applications. I will show how useful it is. 

## The package.json

My package.json looks like this:
```json
 "devDependencies": {
    "@vue/test-utils": "^1.0.0-beta.32",
    "axios": "^0.19",
    "bootstrap": "^4.0.0",
    "cross-env": "^6.0",
    "jquery": "^3.2",
    "laravel-mix": "^5.0.1",
    "lodash": "^4.17.13",
    "popper.js": "^1.12",
    "resolve-url-loader": "^2.3.1",
    "sass": "^1.20.1",
    "sass-loader": "^8.0.0",
    "vue": "^2.5.17",
    "vue-jest": "^3.0.5",
    "vue-template-compiler": "^2.6.10"
  },
  "dependencies": {
    "express": "^4.17.1",
    "nodemon": "^2.0.2",
    "socket.io": "^2.3.0",
    "socket.io-client": "^2.3.0",
    "vue-js-modal": "^1.3.33"
  }
```
Then I just need to add the following NPM script to speed up my workflow

```json
"scripts": {
    "dev": "npm run development",
    "development": "cross-env NODE_ENV=development node_modules/webpack/bin/webpack.js --progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js",
    "watch": "npm run development -- --watch",
    "watch-poll": "npm run watch -- --watch-poll",
    "hot": "cross-env NODE_ENV=development node_modules/webpack-dev-server/bin/webpack-dev-server.js --inline --hot --config=node_modules/laravel-mix/setup/webpack.config.js",
    "prod": "npm run production",
    "production": "cross-env NODE_ENV=production node_modules/webpack/bin/webpack.js --no-progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js",
    "serve": "nodemon index.js"
  },
```
Then I will need to create a ```webpack.mix.js``` file.
```webpack.mix.js``` is your configuration layer on top of webpack. 

In my webpack.mix.js, I just need to tell laravel-mix the path to compile the assets file:

```js
const mix = require('laravel-mix');

mix.js('app/resources/js/app.js', 'app/static/js')
```
Just a few notes on the paths. You can modify the path to match the preferred structure. But here I can just create another file in my project simply running ```touch app/resources/js/app.{js}``` to create the file. Then I can run ```npm run dev``` from the command line. This will compile everything down from ```app/resources/js``` directory

## Vue Components

Now, I need to register Vue components. I'm not going to walk you through on how Vue works. There are many tutorials and official documentation explain things better than me. The socket.io-client responsible for handling realtime communication. And V-Modal is just Vue extension for builidng modal UI. 

```js
require('./bootstrap');

window.Vue = require('vue');
import io from 'socket.io-client'
window.socket = io('http://localhost:2000')

/**
 * The following block of code may be used to automatically register your
 * Vue components. It will recursively scan this directory for the Vue
 * components and automatically register them with their "basename".
 *
 * Eg. ./components/ExampleComponent.vue -> <example-component></example-component>
 */

Vue.component('example-component', require('./components/ExampleComponent.vue').default)
Vue.component('todolist-component', require('./components/ToDoListComponent.vue').default)
import VModal from 'vue-js-modal'

Vue.use(VModal, { dialog: true })


/**
 * Next, we will create a fresh Vue application instance and attach it to
 * the page. Then, you may begin adding components to this application
 * or customize the JavaScript scaffolding to fit your unique needs.
 */

const app = new Vue({
    el: '#app',
});

```
Then in the HTML file in templates folder. Just call the component again. Here in my case, I'm building a todolist so I will call the ```todolist-component```

```html
<!DOCTYPE html>
<html>
    <head>
        <meta name="csrf-token" content="{{ csrf_token() }}">
        <link rel="stylesheet" type="text/css" href="{{ url_for('static', filename='css/todo.css') }}">
    </head>
    <body>
    <!-- register app id. Vue will need this id in order to work. You can use different id but make sure the one in your app.js matches. > --->
        <div id="app">
        <!-- register component> --->
        <!-- The user property is passed from backend-->
       <todolist-component :user="{{ user }}"></todolist-component>
      
    </div>
    <script src=" {{ url_for('static', filename='js/app.js')}}"></script>
    </body>
</html>
```

## Networking

So everytime I want to make the changes to the database, I'm going to emit an event, in other words, I'm going to push the entire todolist to the server side. The server recieves the changes and push it to other devices that currently connecting to the server.

On the client side, I'm also going to listen to the events for any changes and apply it to the current todolist. 

```js
io.on('connection', function(socket){
  socket.on('update list', function(data){
      io.emit('update list', data)
  })
```

**But if we have multiple accounts, they are going to listen to the events from other accounts as well.**

I'm going to split them up into different rooms. So when I'm pushing the changes, only the account in the correct room's id can receive the changes. Here is the code on the server side:

```js
io.on('connection', function(socket){
  socket.on('update list', function(data){
      io.in(data.room).emit('update list', data.task)
  })
  socket.on('privateroom', (room) => {
    socket.join(room)
  })
});
```
And on the client side, when a user loads the page, the user will be forced to join the room with their correct id. In this way, we can make sure they don't receive the changes from other accounts. 

```js
 created() {
        socket.emit('privateroom', this.user.name+this.user.email)
        socket.on('update list', (tasks) => {
          this.task = tasks
        })
    },
```

## Axios
The most common way for client side to communicate with server side is through the HTTP protocol. I'm quite familiar with the Fetch API and the ```XMLHttpRequest``` interface ( allow you to fetch resources and make HTTP requests). In jQuery, there is a popular function called ```$.ajax()```, but as we move away from using jQuery, [Axios](https://github.com/axios/axios) provides better way of dealing with HTTP requests. 

### Axios's Features include

From Axios's github repository, its features include:

- Make XMLHttpRequests from the browser
- Make http requests from node.js
- Supports the Promise API
- Intercept request and response
- Transform request and response data
- Cancel requests
- Automatic transforms for JSON data
- Protection against XSRF

Let's take a look at how I'm going to deal with the todolist task. Everytime I make a new task, I want to push the changes to the database without refreshing the webpage but also update the current list of tasks with the data from the database. 

I'm going to to write a function called ```addItem```. The function is responsible for pushing new tasks to the database. When the database receive the requests, if it successfully saves them to the database, it will return the message ```Success``` and also all the information about the item we just pushed. Then all I have to do is to grab the data and push them to our current list of tasks. Also, I will need to emit the changes to the server so the server can update it for every other devices that currently logged in with our account. 

```js
 addItem() {
        
        axios.post('/api/create', {title: this.title, description: this.description, progress: this.progress}).then(response => {
           if(response.data.message == 'Success'){
            this.task.push({
              id: response.data.id,
              title: response.data.title,
              description: response.data.description,
              done: response.data.done,
              progress: response.data.progress,
            })
            this.title = ""
            this.description = ""
            socket.emit('update list', {task: this.task, room: this.room})
        }}).then((res) =>{
        }).catch(error =>{
                
          });
       
      },
```

Here is also another function that receive the data sent from the server
```js
fetchItemList() {
        axios.get('/api/todolist/'+ this.user.user_id).then(response => {
          this.task = response.data;
         
        });
      },
```
