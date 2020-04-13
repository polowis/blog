---
date: 2020-04-13 06:54:21
layout: post
title: "The three dots (...) in JavaScript"
subtitle:
description: The three dots (...) syntax in JavaScript
image: https://www.wisdomgeek.com/wp-content/uploads/2019/10/rest-and-spread-javascript-1280x720.png
optimized_image:
category:
tags:
    - programming
    - javascript
    - js
    - ES6
author: polowis
paginate: false
---

In some cases, you might find people using this notation in their JavaScript code **(...)** and you might also wonder how it works or what it is. I got the same question a few months ago and I want to pass what I know to other people. 

The three dots syntax is a new feature introduced in ES6. And it might change the way how you write code in JavaScript. There are two different ways for you to write, as spread operator or as rest operator. We will look at code examples, because it brings more efficiency in your learning. 

## Spread syntax 1

<h4 style="color: red">Without Spread Syntax</h4>

```js
const arr = [2, 3];

const add = (x, y) => {

    // return the addition of two given parameters x, y
    return x + y;
}
const result = add(arr[0], arr[1]);

console.log(result) // output 5
```

<h4 style="color: red">With Spread Syntax</h4>

```js
const arr = [1, 3];

const add = (x, y) => {

    // return the addition of two given parameters x, y
    return x + y;
}
const result = add(...arr); // spread operator

console.log(result) // output 4

```
## Spread Syntax 2

<h4 style="color: red">Without Spread Syntax</h4>

```js
const array = ['a', 'b', 'c', 'd', 'e', 'f'];
const array2 = array;

array2.push('g');
console.log(array); //output ['a', 'b', 'c','d', 'e', 'f','g']
console.log(array2); //output ['a', 'b', 'c','d', 'e', 'f','g']
```
<h4 style="color: red">With Spread Syntax</h4>

```js
const array = ['a', 'b', 'c', 'd', 'e', 'f'];
const array2 = [...array]; //spread operator

array2.push('g');
console.log(array); //output ['a', 'b', 'c','d', 'e', 'f','g']
console.log(array2); //output ['a', 'b', 'c','d', 'e', 'f','g']
```

Okay, so what have you noticed so far? On the first example, how the spread syntax works? You might say that, well, the code looks a lot cleaner and shorter. However, in the second example, it might not look as simple as how it does in the first example. It is related to ```Immutable``` in JavaScript. In the above example, I've created an array called ```array``` and then copied it to ```array2```. That means ```array``` is assigned to ```array2```, in other words, everything we do with ```array2``` also effects ```array```because they have the same references.

But with spread operator, we can use it to eliminate this ambiguity. 

## Three dots (...) Definition

As previously mentioned, here are two different ways for you to write, as spread operator or as rest operator.

#### Spread operator

This is really helpful when you want to copy properties of an object to another object but with modifications. For example:

```js
const person1 = {
    username: 'Alice',
    occupation: 'Software developer',
    age: 28
}
console.log(person1); //output { username: 'Alice', occupation: 'Software developer', age: 28 }
```

Now I want to modify the object a little bit with the help from spread operator.

```js
const person2 = {
    ...person1,
    username: 'Bob'
}
console.log(person2) //output { username: 'Bob', occupation: 'Software developer', age: 28 }
```
And all it takes is just a single step. 

#### Rest operator

This is very powerful, sometimes you want to write a function that takes n number of arguments, and this is very useful to solve these situations. Let me explain in through an example. Let's create a function that computes the sum of n numbers. 

```js
function sum(...num){
    
    return num.reduce((sum, val) => {
        return sum + val;
    })
}

console.log(sum(7, 2)); // output 9
console.log(sum(7, 2, 5, 1)); // output 15
```

#### Key different

By now, you should be familiar with this three dot syntax ```(...)```.

1. Whenever you see a function arguments with ```(...)``` at the end, it is rest operator
2. And whenever you see ```(...)``` in a function call or alike, it is spread operator.
