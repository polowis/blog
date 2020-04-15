---
date: 2020-04-12 07:31:13
layout: post
title: "Higher order function in JavaScript"
subtitle:
description: Write cleaner code with Higher order function
image: https://i2.wp.com/codesource.io/wp-content/uploads/2019/12/Writing-cleaner-code-with-higher-order-functions.png?resize=950%2C500&ssl=1
optimized_image:
category: code
tags:
    - js
    - programming
    - javascript
    - es6
author: polowis
paginate: false
---

### What is higher order function?

There are probably dozens of articles talk about Higher-Order Functions. But instead of giving you tons if theory, we'll look at examples to see how Higher-Order Functions work. Let's begin with a simple problem. I have a list of friends:

```js
const friends = [
    {name: 'Alice', age: 14, sex: 'M'},
    {name: 'Sarah', age: 12, sex: 'F'},
    {name: 'Bob', age: 16, sex: 'M'},
    {name: 'Johnny', age: 2, sex: 'M'},
    {name: 'Ethan', age: 4, sex: 'M'},
    {name: 'Paula', age: 18, sex: 'F'},
    {name: 'Donald', age: 5, sex: 'M'},
    {name: 'Jennifer', age: 13, sex: 'F'},
    {name: 'Courtney', age: 15, sex: 'F'},
    {name: 'Jane', age: 9, sex: 'F'}
]
```
Here are the problems you need to solve. 
1. Find the average age
2. Find the average age of male
3. Find the average age of female
4. Find the oldest male
5. Find the oldest female
6. Find the yougest male
7. Find the yougest female
8. Find the oldest person
9. Find the yougest person 

Now, I asked this question to two people, let's say A and B. Both of them knew how to solve this but let's take a look at how they solved it. Begin with A:

Note: *The first character denote the responder's name and the second character denote the question number.* 

**A1** Find the average age
```js

let averageAge = friends.reduce(function (acc, curr) {
    return acc + curr.age;
  }, 0) / friends.length;

console.log('Average age', averageAge);// 10.8
```

**A2** Find the average age of male

```js
let findMale = friends.filter( function(people){
    return people.sex === 'M';
})


let averageMale = findMale.reduce(function (acc, curr) {
    return acc + curr.age;
  }, 0) / findMale.length;
console.log('Average age of male', averageMale);// 8.2
```

**A3** Find the average age of female

```js
let findFemale = friends.filter( function(people){
    return people.sex === 'F';
})


let averageFemale = findFemale.reduce(function (acc, curr) {
    return acc + curr.age;
  }, 0) / findFemale.length;
console.log('Average age of female', averageFemale);// 13.4
```

**A4** Find the oldest male

```js
let oldestMale = Math.max.apply(Math,findMale.map(function(o){return o.age}));
console.log('Oldest Male',oldestMale); // 16
```

**A5** Find the oldest female

```js
let oldestFemale = Math.max.apply(Math,findFemale.map(function(o){return o.age}));
console.log('Oldest Female',oldestFemale); // 18
```

**A6** Find the yougest male

```js
let youngestMale = Math.min.apply(Math,findMale.map(function(o){return o.age}));
console.log('Youngest Male',youngestMale); // 2
```

**A7** Find the youngest female

```js
let youngestFemale = Math.min.apply(Math,findFemale.map(function(o){return o.age}));
console.log('Oldest Female',youngestFemale); // 9
```

**A8** Find the oldest person

```js
let oldest = Math.max.apply(Math,friends.map(function(o){return o.age}));
console.log('Oldest person',oldest); // 18

```

**A9** Find the youngest person

```js
let youngest = Math.min.apply(Math,friends.map(function(o){return o.age}));
console.log('Oldest person',youngest); // 2
```
**Final output of person A**
```s
Average age 10.8
Average age of male 8.2
Average age of male 13.4
Oldest Male 16
Oldest Female 18
Youngest Male 2
Oldest Female 9
Oldest person 18
Oldest person 2
```

So the first responder seemed to give me his ability through the use of ES6. But let's move on to the next responder.

```js
let isBoy = friend => friend.sex === 'M'

let isGirl = friend => friend.sex === 'F'

let getBoys = friends => (
    friends.filter(isBoy)
)

let getGirls = friends => (
    friends.filter(isGirl)
)

let average = friends => (
    friends.reduce((acc, curr) => (
        acc + curr.age
    ), 0) / friends.length
)

let maxage = friends => (
    Math.max(...friends.map(friend => friend.age))
)

let minage = friends => (
    Math.min(...friends.map(friend => friend.age))
)

console.log(average(friends)) // 10.9
console.log(average(getBoys(friends))) // 8.2
console.log(average(getGirls(friends))) // 13.4
console.log(maxage(friends)) // 18
console.log(minage(friends)) // 2
console.log(maxage(getBoys(friends))) // 16
console.log(minage(getBoys(friends))) // 2
console.log(maxage(getGirls(friends))) // 18
console.log(minage(getGirls(friends))) // 9

```

The result of two answers are exactly the same. Through these two methods, what do you think you can do? That's the difference between without higher-order functions and higher-order functions. By now, do you have any idea what higher-order functions are?

### What are Higher-order functions?
Higher-order functions are functions that take other functions as arguments or return functions as their results. Taking another function as an argument is often called the callback function, because of this, it is called Higher-order functions. This is a concept that Javascript uses a lot.


Higher-order functions are widely used in JavaScript. If you've been coding in JavaScript for a while, you might have used them without even knowing it. Often Higher-order functions are one of many higher-order functions that are integrated into built-in functions such as sort, less, filter, forEach, map (). 


One of the great advantages of using Higher-order functions is that we can create smaller functions that take care of only one piece of logic. We then compose more complex functions using different smaller functions.

This technique reduces errors and makes our code easier to read and understand. By learning how to use Higher-order functions, you can start coding better than other developers.