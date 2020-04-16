---
date: 2020-03-25 06:09:38
layout: post
title: "You think you understand AJAX in Jquery?
"
subtitle:
description: How do you handle multiple AJAX requests in the best way?
image: https://www.accuwebtech.com/wp-content/uploads/2017/10/ajax-development-4.jpg
optimized_image:
category: code
tags:
    - js
    - javascript
    - programming
    - jquery
    - promises
author: polowis
paginate: false
---

AJAX-Jquery isn't new to developers, but there are many things I think many of you may not know or have never used, like using AJAX with Promise and deffered objects? How do you handle multiple AJAX requests in the best way? hopefully this article will bring a deeper understanding about AJAX.

### What is AJAX in Jquery?

If you're looking for this question then this article is probably isn't for you. You can learn more about Ajax [here](https://api.jquery.com/jquery.ajax/)

```js
$.ajax({
  data: someData,
  dataType: 'json',
  url: '/path/to/script',
  success: function(data, textStatus, jqXHR) {
  // When AJAX call is successfuly
    console.log('AJAX call successful.');
    console.log(data);
  },
  error: function(jqXHR, textStatus, errorThrown) {
   // When AJAX call has failed
    console.log('AJAX call failed.');
    console.log(textStatus + ': ' + errorThrown);
  },
  complete: function() {
  // When AJAX call is complete, will fire upon success or when error is thrown
    console.log('AJAX call completed');
  };
})

```

Here is an example of how basic structure of ```$.ajax()``` function. If you're familiar with it, you will see that it's really easy to use and understand. But hold on there, the next section will change the way how you think.

### Promises and deffered objects


With Promises and deferred objects, there are new changes in the ajax function such as:

1. **.done ()** replaces .success ()
2. **.fail ()** replaces .error ()
3. **.always ()** replaces .complete ()

Then you will have a structure looks like this:
```js
$.ajax({
    data: someData,
    dataType: 'json',
    url: '/path/to/script'
}).done(function(data) {
  // If successful
    console.log(data);
}).fail(function(jqXHR, textStatus, errorThrown) {
  // If fail
    console.log(textStatus + ': ' + errorThrown);
})

```

It looks a lot cleaner if you're already familiar with **Promises**. You can even make it better than the original **Ajax** function by assigning it to a variable.

```js
let ajaxCall = $.ajax({
    context: $(element),
    data: someData,
    dataType: 'json',
    url: '/path/to/script'
});

ajaxCall.done(function(data) {
    console.log(data);
});

```

Once you are familiar with programming with Promise, .```on ()``` is one of the pretty good ```callbacks ()```, we can separate the processing of the results explicitly, which is also an advantage in building the application. Unlike the original ```$ .ajax``` function, all ```callbacks ()``` are nested, they are not user-friendly right? especially in the situation that may call many asynchronous functions.

### Multiple Ajax requests

Calling multiple Ajax reqests is no longer difficult. Because with promises, calling multiple ajax is easier than ever through the use of ```.when()``, you just need to listen to the returned status.

```js
var a1 = $.ajax({...}),
    a2 = $.ajax({...});

$.when(a1, a2).done(function(r1, r2) {
  // Each returned resolve has the following structure:
  // [data, textStatus, jqXHR]
  // e.g. To access returned data, access the array at index 0
  console.log(r1[0]);
  console.log(r2[0]);
});
```

It's great, right? you can handle many Ajax in parallel. 

Let's look at another situation. What if the second Ajax call depend on the first one?

```js
var a1 = $.ajax({
      url: '/path/to/file',
      dataType: 'json'
    }),
    a2 = a1.then(function(data) {
    // .then() returns a new promise
        return $.ajax({
            url: '/path/to/another/file',
            dataType: 'json',
            data: data.sessionID
        });
    });

a2.done(function(data) {
  console.log(data);
})

```

Perhaps many of you do not know how to use AJAX effectively like this. And hopefully this article has helped you gain more knowledge, so you can be more flexible in handling problems in your projects.



