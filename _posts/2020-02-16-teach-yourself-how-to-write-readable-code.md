---
date: 2020-02-16 04:08:40
layout: post
title: "Teach yourself how to write readable code"
subtitle: 
description: The art of improving and writing readable code
image: https://steemitimages.com/DQmP18L6k8EMHNfsvRNaRFWvka2GnRo8b8CpDuM3hbYGnqp/ff3ywn-1-800x533.jpg
optimized_image:
category: code
tags: 
    - code
    - cleancode
    - programming
    - books
author: polowis
paginate: false
---
1. Do you want to improve your code readability?
2. What is clean code?
3. How to write clean code?
4. Is it important for you to refactor your code base?

## Introduction

This post does not show you how to write clean code but rather shows you some example between good code and bad code and maybe you can improve by yourself.

What is clean code **clean code**? Clean code  is the code that everyone can understand, easy to read and maintainable. You might think that's easy, I can do that! But have you ever tried to look at a piece of code in your previous projects? Is it expressive enough for you to be able to understand within a couple of minutes? No? Then your code is not really considered to be clean enough. 

The code you write will never be perfect but that's fine. Writing a good piece of code is hard, and you must practice and watch others fail and learn from their mistakes. A programmer who writes clean code is an artist who can paint a picture. When you look at the picture, you know whether if it looks good or not but by recognising good art does not mean you know how to paint. Same goes for clean code, being able to recognise good code does not mean you know how to write it. 

## What is clean code?

I like to read code. Especially from other people. It teaches me new things, a new way of writing code. And I try to improve my coding skills day by day. 

Have you ever wondered if other people gonna read your code, would they be able to understand it? Have you ever thought about your code complexity?

Think about this famous quote from **Martin Fowler**:  <br>
<i style="background-color:lightgreen;">Any fool can write code that a computer can understand. Good programmers write code that humans can understand<i>

Poorly written code may be fast but in short term, a few months later you will be able to understand you should've spent time writing good code. 

## If clean code such a good practice, why developers don't do it?

There are plenty of reasons why most developers code are not clean enough. Such as: some developers think they are professional, some have no idea how to code and some just care about the functional of the software. In fact, it's true, customers they don't care about how you write code, what lanuage or what library you're using but I'm going to talk about this later. 

Junior developer, for instance, they've only worked on their own projects or small projects and because of that reason, most of them never understand why is it neccessary to write readable code. Another major problem is lack of time. As mentioned earlier, writing bad code takes less time than writing clean code. Because clean takes a lot of time and planning, and when developers face deadlines, writing bad code is the only option. 

## How importance is it?

The two hard things in computer science are naming conventions and cache invalidation. Why is that? Sometimes you might find yourself struggle on how to name a variable, that's good. When I write a function or assign a variable, I often think about other people who's going to read my code. Will they understand it or does it have similar meaning that may make them misunderstand. Or sometimes, you might want to have a function that is going to be repeated in multiple places but instead of making a function that can do it everywhere, you just copy and paste the function again and again, eventually make your code a lot more messy.

Think about this function
```js
function getCustomerAttributes()
{
    if(user.loggedIn() && user.hasRole('customer') && user.isVerified()){
        return user.firstName + ' ' + user.middleName + ' ' +user.lastName;
    } else{
        return user.firstName + ' ' + user.lastName;
    }
}
```
You might think that this function looks good enough. However, we can rewrite this in a better way. 

```js
function getCustomerAttributes()
{
    return this.isVerifiedUser(user) ? this.getUserLongName() : this.getUserShortName();
}

/**
*
*/
function isVerifiedUser(user)
{
    return user.loggedIn() && user.hasRole('customer') && user.isVerified();
}

/**
*
*/
function getUserLongName()
{
    return user.firstName + ' ' + user.middleName + ' ' +user.lastName;
}

/**
*
*/
function getUserShortName()
{
     return user.firstName + ' ' + user.lastName;
}

```

This function reads much better than the other one with informative name.

<i style="background-color:lightgreen;">Good code is self-documented code. In other words, you shouldn't need pages of documentation to explain it. It should explain itself.<i>

The name also needs to be pronounceable, you don't want to discuss with other people about your code and you don't know how to pronounce it. And you start speaking like an idiot. 

## Summary
1. It's easy to write code for the machines to understand. But good developers know how to write code for humans to understand. 
2. Think about when you read someone's code and unable to understand it. How long did it take you to completely understand everything regardless of your knowledge and skills? When you write code, think about how other people react when they read your code. 
3. Code is clean when everyone able to understand it easily

### Clean Code by Robert C. Martin

This is one of my favorite technical books and I would highly recommend every developer to read it. It shows you how to write a good piece of code and the consequences of poorly written code. <br> <br>
<img src="https://images-na.ssl-images-amazon.com/images/I/41jEbK-jG%2BL._AC_SY400_.jpg"/>