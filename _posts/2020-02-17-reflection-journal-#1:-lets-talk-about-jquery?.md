---
date: 2020-02-17 04:46:00
layout: post
title: "Reflection Journal #1: Let's talk about jQuery"
subtitle:
description: Week 2, Semester 1. A lot of websites rely on jQuery. But is it neccessary to learn it?
image: https://cdn-media-1.freecodecamp.org/images/0*ngXgBNNdx6iiWP8q.png
optimized_image:
category: blog
tags:
    - reflection
author: polowis
paginate: false
---

## Before you read this post

This website was built with [Jekyll](https://jekyllrb.com/), you can find the entire source code of this website on my [github](https://github.com/polowis/blog). 

You can find all the posts about reflection journal by clicking on the tag :)

## Let's talk about jQuery 
jQuery makes every web developer life a lot easier. And lots of website rely on jQuery but do you need it? I don't have much experience working with jQuery. Why? Because I prefer working with Vanilla JavaScript (JavaScript without any other libraries/frameworks).

### The jQuery's trap
jQuery was the first library I used when I got myself into web development, just a few months before school had a chance to teach me about this library. I usually see lots of starters know how to write code in jQuery but not JavaScript. They don't even know how to deal with the DOM directly or how to code in Vanilla JavaScript if they don't have jQuery. Because this library does everything for them, they don't even bother learing proper JavaScript. Eventually, create a huge gap in knowledge. 

There are lots of JavaScript libraries out there with different style of writing. But learning the basic will help them understand the syntax a lot easier and faster. 
```js
$('#myid'); // jQuery
// Same as 
document.getElementById('#myid');
```
But who knows what might happen to jQuery itself in the next 10 years? With lack of basic knowledge, they will struggle a lot to learn other libraries. Given VueJS example:
```html
<div id="app">
  <p>{{ message }}</p>
  <input v-model="message">
</div>

```
```js
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello World!'
  }
})
```

So these are just my opinions on jQuery. From my point of view, it is not worth spending time on jQuery, learn modern front-end frameworks such as React, Angular and tools like Webpack and Browserify is definitely the future. 

If you work on small application, jQuery is good but for complex applications, using frameworks like React, Angular will handle lots of DOM manipulation and potentially avoid creating more bugs. 

### jQuery isn't hard to use

The documentation is well written, easy to understand, literally everyone can use it within couple of minutes reading. I also use jQuery on my other web services when I want to handle simple interaction. Here is the code snippet taken from the code base. 

```js
$('#hashiddeneffect').on('change',function(){
	var selection = $(this).val();
		switch(selection)
		{
			case "1":
				$("#item").show()
				$("#monster").hide()
				$("#hiddentextdescription").show()
				break;
			default:
				$("#item").show()
				$("#monster").hide()
				$("#hiddentextdescription").hide()
				break;
		}
});
```
### Classwork

I built with website using Jekyll (a static-website generator written in Ruby). I spent most of the time in class setting up this blog. The normal html page is great, but when I try to create a website with several posts, Jekyll does a better job for me. I can seprate my website into multiple layouts such as header, footer, etc. Jekyll is fast and simple. For example, take a look at the code snippet below:

```html
<!--{% if user.age > 18 %}-->
Login here
<!--{% else %}-->
Sorry, you are too young
<!--{% endif %}-->

```

Even non-code can understand the code above. Jekyll is faster than any wordpress site, if you compare Jekyll site with any other site on [Google Page Insight](https://developers.google.com/speed/pagespeed/insights/)




