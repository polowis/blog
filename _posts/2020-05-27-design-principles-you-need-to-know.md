---
date: 2020-05-27 01:39:45
layout: post
title: "Design principles you need to know"
subtitle:
description:
image: https://miro.medium.com/max/800/0*aYobhP_7_dSiNlWZ
optimized_image:
category: blog
tags:
    - clean
    - design 
    - principles
author: polowis
paginate: false
---

Software development design principles or design principles (for short) are the rules of desgining software, one of the most important terms in software development industry. Design principles can be understood as advices for you to design a program that easy to develop and easy to maintain. Keep in mind that design principles are different from design patterns. These two terms give you the similar performance but slightly different concepts.

I have written about the **SOLID** principles, however I'm gonna share some more principles that are also interesting beside **SOLID**.

## DRY

**DRY** is a short term for **Don't Repeat Yourself**. **DRY** wants to tell you to stop repeating your code. **DRY** is not only applied in coding but also in writting documentation or desgining database schemas, etc.

By implementing **DRY** will help you maintain the code better, or solve the problems of changing the logic of the system a lot easier later on. 

<img src="https://miro.medium.com/max/686/1*rAqvkElSismRQsJvEeuh0g.png"/>

There is also another term people often refer to the opposite concept. **WET** can be understood as *write every time*, *write everything twice*, *we enjoy typing* or *waste everyone's time*. In short, if you violate **DRY** principle, you will get **WET**. 

## KISS
**KISS** stands for **Keep It Simple, Stupid**. The **KISS** principle refers to the fact that simple should be set as the goal of every system design and most system will work best when it is kept in a simple state, instead of being complicated. Any attempt to make unnecessary problems more complex should be removed.

<img src="https://livecode.com/wp-content/uploads/2015/06/062112_kiss.png"/>

You might find that **KISS** principle is very similar to many famous quotes. For instance, *Simplcity is the ultimate sophistication*, *Make simple tasks simple*. However, this principle needs to be understood properly. The term *simple* here is just a *problem solving*, not *simple* to the point where it is impossible to operate the system as desired.

## YAGNI

The term **YAGNI** stands for **You aren't gonna need it**. **YAGNI** is the principle that is introduced in *extreme programming*. It states that you should never need to include functions until they are absolutely necessary, or only implement the function that you feel you need not the one you feel that *you might need it later*.

<img src="https://itsadeliverything.com/images/pain-driven-development.png"/>

**Martin Fowler** addressed 4 issues of YAGNI as follow: <br>
1. **Cost of building**: It's when you make the function and you don't really need it at the end. It wastes you lots of effort in designing, testing, and debugging.

2. **Cost of repair**: When the function you are working on is neccessary but you implement it in an unreasonable way. it will take you lots of effort to re-plan, re-code and retest the functionality you did because that's not really what you need. 

3. **Cost of delay**: In any case, you will encounter this problem. You are wasting time on a function that you do not need at the moment which leads to the fact that the functions needed to be implemented at the present time cannot be completed and released soon. 

4. **Cost of carry**: In any case, you will also encounter this problem. You are adding a new amount of code to your project, making the system more complex and will take more effort to maintain, modify and debug. 