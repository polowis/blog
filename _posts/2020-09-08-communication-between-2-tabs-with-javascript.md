---
date: 2020-09-08 05:43:05
layout: post
title: "Communication between 2 browser tabs with JavaScript"
subtitle:
description:
image:
optimized_image:
category:
tags:
author:
paginate: false
---

A few days ago I thought of an interesting problem while I was working, which is how to get the value of a form submitted in another tab. This is an extremely simple task when operating on one tab, but what about two, three or more tabs? How would I be able to achieve this?

So what exactly is a communication between two or more tabs?

Communication between two tabs is being able to send and receive data (messages) between multiple tabs, windows, iframes (also called instances).

### When you need to use it?

Before the demonstration,I would like to present a few cases where you might want to apply the communication between tabs. Sometimes, users will have to open multiple tabs or windows of your website which is quite common. It is important to achieve the best user experience possible. So for example, let's look at the dark mode toggle function, when you click on it, it will change the look of all web pages that being opened without having to refresh the pages. Or you might think of the case where you submit the form in ```iFrame``` and get the value of the form outside without having to refresh. 

### Notes

This has some limitations, as it can only be used against domains on the same origin.

- Not available between HTTP and HTTPS.
- Cannot be used between different hosts.
- Cannot be used between different ports.

### Demonstration

Currently, I think there are two most common and effective ways to use the ```localStorage``` and ```BroadcastChannel API```

#### LocalStorage
In order to catch the event sending messages from one tab to another, simply ```bind``` event storage for all the tabs:

```js
window.addEventListener("storage", messageReceive);
```
The ```messageReceive``` function will be executed every time you ```set``` the value for ```localStorage``` on all tabs.  The object event listener already contains the data you just ```set``` for ```localStorage```, so you don't need to ```parse``` the ```localStorage``` object anymore.  This is quite handy when you can reset the value immediately after the set, so the data is clean. You can use this example function below to handle sending and receiving messages from a form:

```js
// send message
function messageBroadcast(message) {
  var message = document.getElementById('input').value; // get form input
  localStorage.setItem('broadcast', JSON.stringify(message)); // set form input value to localStorage under key 'broadcast'
  localStorage.removeItem('broadcast'); // remove value of the key 'broadcast' in localStorage to clean the data
};

// receive message
function messageReceive(event) {
  if (event.key == 'broadcast') {
    var message = JSON.parse(event.newValue); //
  }

  if (!message) return; // if message is null then pass

  // handle received message
  // you can also send object, for example: { 'title': 'tiêu đề', 'message': 'nội dung' }
  alert(message);
  // more code goes here.
};
```
html part

```html
{% raw %}
<input type="text" id="input" placeholder="Type here" />
<button onclick="messageBroadcast()">send</button>
{% endraw %}
```

There are a few limitations of using ```localStorage```

- You cannot store an ```Object``` directly in local storage / session storage, you must stringify the object. Hence you will always have to parse the value before using it. I think this is not very optimal.

- The ```event``` will not be triggered if its value is either constant or updated to an identical value. To handle this, you can use ```Date.now ()``` so that other instances always catch the event.

```js
{'message': 'Hello', 'uid': Date.now()}
```

Apart from the limitations, using localStorage has the advantage of being supported in multiple browsers.

<img src="https://miro.medium.com/max/2450/1*X-4gkxes_ZsC0IxihdH4-A.png"/>


#### BroadcastChannel API


To send data to another tab, we first need to create a ```BroadcastChannel``` instance.

```js
const channel = new BroadcastChannel('myChannel');
```

```myChannel``` here is the name of the channel we ```subscribe``` to. After subscribing to a channel, you can send and receive messages from that channel.

To send a data:

```js
channel.postMessage('Hello World!');

```

You can send many forms of data

```js
// array
channel.postMessage(['Car', 'Dog', 'Animal']);

// object
channel.postMessage({ name: "Alice", age: 23 });

// blob
channel.postMessage(new Blob(['Hello World!'], {
    type: "text/plain"
}));

```

#### Retreive data

To receive data we ```subscribe``` to the channel 'myChannel' and ```catch``` the channel's message event:

```js
const channel = new BroadcastChannel('myChannel'); 

channel.addEventListener("message", function(e) {
  alert(e.data);
});
```
As the result:
```html
{% raw %}
<script>
    const channel = new BroadcastChannel('myChannel');

    channel.addEventListener('message', function(e) {
      alert(e.data);
    });

    function messageBroadcast() {
      var message = document.getElementById('input').value;

      channel.postMessage(message);
    };
</script>

<input type="text" id="input" placeholder="Type here" />
<button onclick="messageBroadcast()">Send</button>
{% endraw %}
```

#### Advantages / disadvantages

Unlike using ```localStorage```, using ```BroadcastChannel``` lets you post full objects, arrays, and other types of object.

However, the second way can be a bit more complicated and somewhat overkill depending on the purpose of use.

The ```BroadcastChannel API``` is also not very widely supported. Compared to local storage (~ 96%), and BroadcastChannel just over 78%. **Chrome** and **Firefox** are both supported, however **IE**, **Safari** and **Edge** do not. (**Chromium Edge** still supports this API).

### Summary

- It can be used to tell a user that they are editing in a different tab / window, or used to sync data in multiple tabs together.
- Content that needs to be authorized / locked will be opened upon login in another tab, keeping all instances in sync.
- Change avatar.
- Communication between the iframe - iframe, parent - iframe, iframe - parent.
- Change the look / setting of website and need to sync with all other tabs without manual refresh.
