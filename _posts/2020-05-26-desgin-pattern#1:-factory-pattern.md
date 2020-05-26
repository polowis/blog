---
date: 2020-05-26 01:34:32
layout: post
title: "Desgin pattern#1: Factory Pattern"
subtitle:
description: Understanding factory pattern
image: https://encrypted-tbn0.gstatic.com/images?q=tbn%3AANd9GcT2DS9D4WAesmFlRMXDAeFP_7mntxKYwJtR-AC6ouhHCh1ZlbxC&usqp=CAU
optimized_image:
category: blog
tags: 
    - design pattern
    - pattern
    - factory
    - clean code
author: polowis
paginate: false
---

If you have read my other articles, you probably see that I mentioned about software design or clean code a lot. In software engineering, there are lots of way to remove redundant code or to improve the quality of your code. One of them is **design pattern**. 

Design patterns are the solutions for many problems that you might face during your development process. These patterns are obtained from thousands of software developers over a long period of time. 

When you follow these design patterns, you will make you life two times easier. In addtion, you will be able to develop your program in an easy and faster way. 

Before you continue reading this post, you will need to have some basic understanding of OOP concepts or at least some **Java** knowledge or **C#**.

## Factory pattern

One of the most used design patterns is the factory pattern. Think about this problem: you are to develop a RPG game that contains a **Player** class and **Enemy** class. 

The very first version of your game is to handle every actions of the enemy in the **Enemy** class. And then your game attracts many players, they request to have more classes in game, instead of having only one type of enemies, they want to have multiple types of enemies.

Hmm, it sounds great, doesn't it? You can have like **Goblin** class, **Dracula** class, and so on. But what about your code? Most of your code is in your **Enemy** class. Now if you want to add another extra class like **Goblin**, which means you will have to change the entire code base. Eventually your game might contains tons of conditional statements in order to switch between classes because your game is heavily dependent on the class object. 

Well, there is a solution for it. And probably you have used it many times without even notice that it is factory pattern. 

Here is how the factory pattern looks like in diagram:
<img src="https://raw.githubusercontent.com/heidyhe/img/master/design/factorymethod.png"/>


At this point you might think it's pointless. You can just change the constructor to another one. Think about this, when the factory pattern comes into play, you don't have to think about rewriting the logic of the game. 

Let's take a look at this code below before you use the factory pattern

```java
class Enemy{
    public int health = 100;
    public void attack(){
        // some code goes here
    }
}
```
use the enemy class
```java
new Enemy().attack();
```
If you consider to implement another class which is **Goblin**, normally you would do like this:

```java
new Goblin().attack();
```
And what if you have tons of enemies? Do you want to do this all the time? Of course, no. If you use design pattern, you don't have to worry about this all the time, all you need to do is to **extend** the base class and all the methods should follow the same. 

```java
abstract class Enemy{
    abstract public void attack();
}

// Goblin
class Goblin extends Enemy{
    public void attack(){

    }
}

// dracula
class Dracula extends Enemy{
    public void attack(){

    }
}
```
Using it:

```java

class Application{
    public Enemy enemy;
    public static void main(String[] args){
        String enemyName = getEnemyName();
        if(enemyName.equals('goblin')){
            enemy = new Goblin();
        }
        else if(enemyName.equals('dracula')){
            enemy = new Dracula();
        }

        // attack player
        enemy.attack()
    }

    public String getEnemyName(){

    }
}
```

By using factory pattern, it helps you to achieve one of the SOLID principles **Open/Closed principle**. The open/closed principle states that: <br>

<i style="font-size: 20px; background-color:lightgreen;">You should be able to extend a class's behaviour, without modifying it.</i>
