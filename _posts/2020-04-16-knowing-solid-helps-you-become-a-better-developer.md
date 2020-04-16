---
date: 2020-04-16 00:06:11
layout: post
title: "Knowing SOLID helps you become a better developer"
subtitle:
description: Knowing SOLID helps you become a better developer
image: https://www.monterail.com/hubfs/blog/featured/SOLID.png
optimized_image:
category: blog
tags:
    - design pattern
    - solid
    - programming
author: polowis
paginate: false
---

During years of studying, almost all students are taught some basic OOP concepts as follows:

1. Abstraction
2. Encapsulation
3. Inheritance
4. Polymorphism

These concepts have been introduced clearly and almost every interview has questions about OOP concepts. These 4 concepts are quite basic, you can google it to learn more.

The principles I introduce in this article are the design principles in OOP. These principles are drawn by dozens of developers and from thousands of successful and failed projects. Any project that applies these principle will have more readable, testable and maintainable code base. If you have experience working in the software development industry, you probably notice that the coding time only takes 20%-40% of your time, the rest of the time is to maintain the code: fixing bugs, refactor, etc. Mastering these principles and apply them to your code will help you take one extra step on the path of becoming senior developer. 

SOLID principles can be break down into 5 different principle:
1. **S**ingle responsibility
2. **O**pen/close
3. **L**iskov substitution
4. **I**nterface segregation
5. **D**ependency inversion

### Single responsibility

<div style="background-color:lightgreen;">
<i>A class should have one, and only one, reason to change</i>
</div>

Think about this case, let's say you have a class look like this

```java
public class Shape{
    public static void calculateSquareArea();
    public static void calculateRectangleArea();
    public static void calculateCircleRadius();
}
```

What have you noticed in the above example? The class ```Shape``` is responsible for 3 different things. In the future, when the requirements change, the class will need to be modified. The  more responsibility of a class, the more changes request it will get, eventually it will make those changes harder to implement because you need to modify the class. 

Instead you should seprate them into several small classes. For example, the changes might be made like this:

```java
public class Square{
    public static void calculateArea();
}

public class Circle{
    public static void calculateRadius();
}

```

### Open-Close

<div style="background-color:lightgreen;">
<i >You should be able to extend a class's behaviour, without modifying it.</i>
</div>

In this principle, the class's behaviour should be able to be extended. Why? Because when there are requirements changes, you should be able to change the way of how the class behave in a different way in order to meet thoese requirements. 

But also, closed for modifications. No one is allowed to make changes to the original class. The best way to achieve this is through inheritance and abstraction. In this way you would be able to change the behaviour of the class without modifying the class base. For instance, given the above example:

```java
public class Shape{
    public abstract int calculateArea();
}

public class Square extends Shape{
    public int calculateArea(){
        return 10;
    }
}
```

### Liskov substitution 


<div style="background-color:lightgreen;">
<i >Derived classes must be substitutable for their base classes</i>
</div>

Okay, things are getting complicated. Let me give you an explanation through examples, let say you have the parent class called **Shape** and the following child classes **Square**, **Circle**, **Rectangle**. Okay, if you inherit class **Shape**, the **Square** and **Rectangle** should be able to run smoothly. Mathematically saying, the area of square and rectangle needs the width and height. But class **Circle** may not be able to run smoothly, because the area of circle doesn't need those factors and will cause errors. 

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/ea/Design_by_contract.svg/1280px-Design_by_contract.svg.png">

As suggested, each method should have preconditions and postconditions. The preconditions must be true in order to execute the method and the postconditions must be true after the execution. 

### Interface segregation

<div style="background-color:lightgreen;">
<i >Clients should not be forced to implement interface they do not use.</i>
</div>

This principle is quite easy to understand. It is better to have many smaller interfaces. Instead of making an interface with 100 methods, you should break it down to several interface. Because you don't even use all of those. For example, let's say you have an interface called **Animal**, and it has **walk**, **eat** methods. However some animals can fly as well, which means you will need to break it down by **role**, for example, you might want to break it down into **CanFly** interface, responsible for animals that can fly. Through that, the code will become more maintainable and readable. 

### Dependency inversion

<div style="background-color:lightgreen;">
<i>High level modules should not depend upon low level modules. Both should depend upon abstraction</i>
<i>Abstractions should not depend upon details. Details should depend upon abstractions </i>
</div>

This principle is quite important, often can be solved by using **dependency injection**. Dependency injection technique is injecting dependency of a class through constructor. 

**Without dependency injection**
```java
public class Animal{
    public Head head = new Head();
    public Tail tail = new Tail();
}
```

**With dependency injection**
```java
public class Animal{
    public Head Head;
    public Tail tail;

    public Animal(Head head, Tail tail) {
        this.head = head;
        this,tail = tail;
    }
}
```

These SOLID principles will make your projects maintainable, readable, testable and more. Hopefully in the future, I can write some post about each of these concepts in details, especially about **dependency injection**. 
