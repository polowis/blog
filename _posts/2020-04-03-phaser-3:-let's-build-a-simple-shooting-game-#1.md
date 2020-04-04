---
date: 2020-04-03 23:31:59
layout: post
title: "Phaser 3: Let's build a simple shooting game #1"
subtitle:
description:
image: https://upload.wikimedia.org/wikipedia/commons/6/6c/Phaser_%28game_framework%29_logo.png
optimized_image:
category: code
tags:
    - programming
    - javascript
    - game
    - phaser
author: polowis
paginate: false
---
# Before you read this

All you need to know is basic JavaScript knowledge, HTML and a little bit of Maths.

It will be really hard for those who are not familiar with programming or JavaScript in particular. If you want to understand more about JavaScript, [MDN](https://developer.mozilla.org/en-US/docs/Web/Tutorials) has a very good tutorial and well written documentation.

Additionally, you should know basic trignometry. Although Phaser already handles most of the calculations but knowing sine/cosine will help you understand better, knowing what's going on under the hood.

<img src="https://upload.wikimedia.org/wikipedia/commons/6/6c/Phaser_%28game_framework%29_logo.png"/>

# Introduction
There are plenty of libraries and frameworks that let you build your games on browsers. Phaser is one of them.

Phaser is a JavaScript game framework that let you build modern 2D games. There are two major version of Phaser, Phaser 2/CE and Phaser 3. Phaser 2 and 3 are completely different, even if you already have a fundamental knowledge of Phaser 2, you still have to learn everything from the start with Phaser 3. At the time I wrote this post, I was new to Phaser, the fact that I only spent 2 days reading its documentation. 

I'm going to use Phaser 3 for now. Phaser 3 offers more feature than Phaser 2, don't expect much from Phaser 2, mostly just bug fixes and small update changes. The only reason why you should use Phaser 2 instead is because it is well covered with docs, tutorials and examples. Phaser 3 is indeed harder to learn and that's why I think it's worth to write some posts about it.

# Set up

You will definitely need a browser, a web server and a text editor (**not notepad**). You have to use a web server in order to run your game. In the docs, [Phaser](http://www.phaser.io/tutorials/getting-started/index) already explained why you need a web server. 

About text editor, you can use any editor you like (**Again not notepad**). If you don't know which to choose, I suggest you to use Sublime Text or VSCode. VSCode has an extension that allows you to run web server which is pretty handy.

What are we missing??
Yes, the Phaser framework. There are two ways you can download Phaser, either via npm or cdn. [Phaser](https://phaser.io/download/stable) has a well written download tutorial if you want to take a look at.

But for now we will stick with the NPM. And also you should take a look at Phaser [tutorial](https://phaser.io/tutorials/getting-started-phaser3) to get an idea of  its struture. 

# Part 1

## Sprites and game loop


### Preload
We will definitely need sprites, sprites are game objects, they are something that we can see it visually. In Phaser, before you can use any assets/images, you must load them in preload function. Why? Because if you have a bunch of assets (sounds/images/plugins etc.), it will take time for all of them to be loaded in game. And the ```preload()``` function will make a huge different. You don't really have to worry about how the function works under the hood, once you declare it, it will be automatically run by Phaser.

You need to have a background image, it can be whatever you like and a bullet image (obviously). 

```js
preload() {
    this.load.image('space', 'assets/space.png')
    this.load.image('bullet', 'assets/bullet.png')
}
```

Just in case, the following syntax are for all intents and purposes the same.
```js
preload(){

}
preload: function(){

}
```
Let's talk about what is going on here. The ```load.image()``` function loads an image with a given path (in our case: ```assets/space.png``` ) and gives it a name (ie. ```space```). Why it needs a name? Because we will use it later. You can think it as a variable that holds the image for us.

### Create

Now we need to display the images that we just loaded from preload function. In Phaser, this needs to be put in ```create()``` function. This is also part of Phaser's functions that handle this for us.

```js
create(){
    this.space = this.add.tileSprite(600, 300, 'space');
    this.bullet = this.add.sprite(400, 300, 'bullet')
}
```
The function ```add.sprite()``` takes 3 parameters, the x and y coordinates of the sprite and the given image name that we previously declared in ```preload()``` function. 

## Screen coordinates
You probably learn about the cartesian plane where you need to define a value pair ```(x, y)``` in order to plot a point. The centre point is at ```(0,0)``` and the value x increases as you go further to the right and value y increases as you go up.
<img src="https://processing.org/tutorials/drawing/imgs/drawing-03.svg"/>

Sadly, computers don't use cartesian plane. Instead the point ```(0, 0)``` being placed on the center, it represents at the top-left at the screen. And instead of decreasing, the y value increases as you down. The x stays the same. 

