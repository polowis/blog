---
date: 2020-04-15 00:42:00
layout: post
title: "Thinking programmatically, where are you at?"
subtitle:
description: Programming is a broad concept for everyone who follows the programming path. And that includes programmatically thinking, that's why employers often value candidates' ability to think.
image: https://papan01.com/static/8ebcd278e5c91de55c2aeae709ff056d/944c5/you-dont-know-js.png
optimized_image:
category: code
tags:
    - js
    - programming
    - javascript
author: polowis
paginate: false
---



Programming is a broad concept for everyone who follows the programming path. And that includes programmatically thinking, that's why employers often value candidates' ability to think.

Companies always have tests for candidates in the first round, the tests might not look too hard, but the questions they ask are intended to test our logical thinking. Especially, there are complex problems and they want us to find the most consise ways to solve these problems. 

There are tons of ways to approach these problems, if you want to impress them, you will need to find the most efficient method. Let's take a look at these examples. Suppose that, you want to make a function to return a specific day in a week. 

```js
const getDay1 = (day) => {
    if(day === 1) {
      return 'Sunday';
     
    } else if(day === 2) {
      return 'Monday';
  
    } else if(day === 3) {
      return 'Tuesday';
   
    } else if(day === 4) {
      return 'Wednesday';
   
    } else if(day === 5) {
      return 'Thursday';
    
    } else {
      return 'No day';
     
    }
  }
```
The code snippet above is a very specific example of how a typical developer can do. And that way almost anyone can do the same thing. What about you? Do you have another way of solving the problem? If so, then how? You can assume the code snippet above is example 1, now let's take a look at a second example and compare the performance. 

#### Example 2
```js
 const getDay2= (day) => {
    switch (day) {
      case 1:
        return 'Sunday'
      case 2:
        return 'Monday';
      case 3:
        return 'Tuesday';
      case 4:
        return 'Wednesday';
        
      case 5:
        return 'Thursday';
       
      default:
        return 'No day';
    }
  }
```

Using switch case syntax to replace if else is a common way of use with developers. Code is is equivalent, but do you really understand when to use switch and when to use if else? As for performance, I'm pretty sure switch case is much faster than if else. 



Here is the perfomance comparison:

```js
console.time('if case');
getDay1(3);
console.timeEnd('if case');

console.time('switch case');
getDay2(3);
console.timeEnd('switch case');

// output
//if case: 0.106ms
//switch case: 0.029ms
```

Wow, see that? What other ways can you use? You can use **Object literal**

#### Example 3

```js
const actions = {
    '1': ['Sunday'],
    '2': ['Monday'],
    '3': ['Tuesday'],
    '4': ['Wednesday'],
    '5': ['Thursday'],
    'default': ['No day']
  }
const getDay3 = (status) => {
    let action = actions[status] || actions['default'], LogName = action[0];
    return LogName;
}

```

You can also use **ternary operator**

#### Example 4: 
```js
const getDay4 = status =>
  status === '1'
    ? 'Sunday'
    : status === '2'
    ? 'Monday'
    : status === '3'
    ? 'Tuesday'
    : status === '4'
    ? 'Wednesday'
    : status === '5'
    ? 'Thursday'
    : 'No day';

```

#### Comparison
```js
//output

//if case: 0.112ms
//switch case: 0.033ms
//object literal: 0.035ms
//ternary operator: 0.035ms

```
You probably see a huge perfomance different. I just want to show you that there are tons of ways to deal with this problem, either you haven't tried to figure it out yet or you just just haven't given it any thought. 


I am a developer, and maybe you are also a developer, but the way of thinking is different. You have to earn lots of experiences through your personal projects, you will also have to discuss and share your knowledge with other developers. 
