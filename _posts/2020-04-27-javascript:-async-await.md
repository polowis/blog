---
date: 2020-04-27 04:30:32
layout: post
title: "JavaScript: Async/Await"
subtitle:
description: What is async/await in JavaScript?
image: https://scotch-res.cloudinary.com/image/upload/w_1050,q_auto:good,f_auto/media/38945/wLk8dU4HT9msajJ70vc8_Asynchronous_Javascript_using_Async-_Await.png.jpg
optimized_image:
category: code
tags:
    - js
    - programming
    - es6
    - callback
author: polowis
paginate: false
---

If you're not familiar with ES6, you're might not familiar with the term ```async/await```. It makes your code looks cleaner and faster. But before we dive into it, let's understand what ```Promises``` and ```callback``` are. If you already know promises and how they work, you can skip this part and move on.

1. <a href="#callback">Callback</a>
2. <a href="#promise">Promise</a>
3. <a href="#asyncawait">Async/Await</a>

### Callback

To most developers, the term **callback** is no strange to them in all programming languages, not just JavaScript. Because this post is going to be about JavaScript, so that's why I mentioned it. 

In JavaScript, functions are of course asynchronous. Everyone knows it but if you don't understand it, let me give you an example to undertstand why I say asynchronous.


```js
const getRequest = () => {
  setTimeout(() => {
    console.log('Second');
  }, 2000);
};

console.log('First');
getRequest();
console.log('Third');

```

The code above will print the word **first** to the console then call the ```getRequest()``` function to print the word **second** and finally print the word **third** to the console. Now the question is, does the code above run synchronously?

If you look at the ```getRequest()``` function, you can clearly see that it waits for 2 seconds to print the word **second**. At the same time, the last line of code still continue to run even though the ```getRequest()``` function is still not completed. 

So what's the output?
```js
//First
// Third
// Second
```
So if you are going to use some values in the ```getRequest()``` function, it will be impossible for you to use it, because other functions don't wait for it to complete. 

Let me explain a bit more. Instead of printing to the console, let's look at a more realistic example where you actually use the values in that function.


```js

let values = 'default value';
const getRequest = () => {
  setTimeout(() => {
    values = 'new value'
  }, 2000);
};
console.log('Before request');
getRequest();
console.log(values);
console.log('After request');

// output 
// Before request
// default value
// After request
```

In this function, you can see that the ```getRequest()``` function reassign the ```values``` variable. And we invoke it before we print that values to the console. Now instead of printing **new value**, it will print **default value**, why? Because it doesn't wait for the ```getRequest()``` function to finish, it will grab whatever values the variable is holding and return it. 


So what if I want to print the new values in the above example, I am afraid that it is not possible. Because ```console.log(values)``` statement is run immediately after the ```getRequest()``` function is executed. Meaning it is **Asynchronous**

The real question is, how do we make it to work? How am I going to print the new values to the console? It is called ```callback```.

Let's rewrite the code

```js
let values = 'default value';

const getRequest = (callback) => {
  setTimeout(() => {
    values = 'new value'
    callback()
  }, 2000);
};

console.log('Before request')

getRequest(function() {
    console.log(values)
    console.log('After request')
})

//output
// Before request
// new value
// After request
```
Callback is important because we need to wait for response from the server or from other functions in order to move forward in our code. Any function that is passed as an argument is called ```callback function```. You can also have multiple callback functions and that lead to an issue called ```callback hell```. For example: 

```js
const verifyAccount = function(username, password, callback){
   db.verifyAccount(username, password, (error, userInfo) => {
       if (error) {
           callback(error)
       }else{
           db.getRoles(username, (error, roles) => {
               if (error){
                   callback(error)
               }else {
                   db.getAccess(username, (error) => {
                       if (error){
                           callback(error);
                       }else{
                           callback(null, userInfo, roles);
                       }
                   })
               }
           })
       }
   })
};

```
But I'm going to talk about it later.


### Promise

What is promise? And when to use it?

In short, promises are a way to handle JavaScript asynchronous tasks. They are the alternative to callbacks. Due to many people be able to use Promise effectively, it can be considered to be superior or divine. It is true that it is powerful, but it is not so difficult to understand or to use.

Promises can have 3 states, **pending**, **resolved** and **rejected**



Let's say you want to make a request to the server. You're asking something from the server. You can think the server as the girl you want to confess. When you ask her *Do you love me?* That is the Promise.

And of course when you ask so, the result is unknown until she replies back. That is called **pending**. You don't know the result, you don't know whether she agrees or not.

Now take a look closer at the code to see how this works

```js
let promise = new Promise((resolve, reject) =>{
   
})

```
When the promise executes its job, it will call one of the functions it accepts as an argument.
And they are **resolved** (value) and **rejected** (error). It means that when you ask her, after waiting for it, there will be two results from that girl. 

If she agree, the status will be **resolved** (fulfilled), and she does not, it will be **rejected**
<img src="https://javascript.info/article/promise-basics/promise-resolve-reject.svg">

```js
let promise = new Promise((resolve, reject) =>{
    setTimeout(() => resolve("I love you too"), 1000);
})
```
When the Promise is successfully finished, it can be a good news. Keep in mind that at this stage you only know the status is **fulfilled**. That means you can't guarantee the response from the girl will be a yes or no. If you want to know, you need to run the ```.then()``` function.

```js
let promise = new Promise((resolve, reject) =>{
    setTimeout(() => resolve("I love you too"), 1000);
})

promise.then(result => console.log(result))

//output after 1000ms
// I love you too
```

What if she rejects?

```js
let promise = new Promise((resolve, reject) =>{
   setTimeout(() => reject(new Error("I don't love you")), 1000);
})
promise.then(
    result => console.log(result)
).catch(error => {
    console.log(error);
})
```

Through a very specific example, you can see that it is not difficult to use **Promise**, but it is just the basic. There are many syntax also known as **Promise chaining** Here is an example of how it should looks:

```js
let promise = new Promise((resolve, reject) => {

setTimeout(() => resolve('I love you'), 1000); 

})
promise.then(result => { 

console.log(result); 
return 'hold on';

}).then(result => { 

console.log(result);
return 'Actually';

}).then(result => {

console.log(result); 
return "I don't love you'"; 

}).then(result => {
    console.log(result);
})


```

### Async/Await

Hopefull by now you undertstand how ```callback``` and ```promise``` work.

For a long time, most JavaScript developers were miserable when they had to deal with multiple callbacks, at which point the repetition of callback function was a pain. The term **callback hell** was born because of this.

Thankfully then we have to say thank you to the developers and thanks to the promise for promptly dispelling those horrors. And they replace callback function impressively that make most developers quickly learn how to use them. Specifically Async / await with an even better implementation. 

Async/ await is a JavaScript feature that makes working with asynchronous functions much more easier and more interesting. It is built on ```Promise``` and is compatible with all existing Promise based APIs.


**Using async**
```js
async function someFunction() {...}
```
1. Automatically convert a function into a Promise ()
2. Async functions allow using await.

**Using await**
Pause executing asynchronous functions
```js
let res = await someAsyncFunction();
```

1. Await can only be used inside function that is declared as **async** (see above, put the word **async** before the **function** keyword). If you don't declare it, don't ask me why there is error.
2. Await it only works with Promise, not with functions with callbacks


Let's look at this in details. Here is how typical callback function looks like in the past

```js
const verifyAccount = function(username, password, callback){
   db.verifyAccount(username, password, (error, userInfo) => {
       if (error) {
           callback(error)
       }else{
           db.getRoles(username, (error, roles) => {
               if (error){
                   callback(error)
               }else {
                   db.getAccess(username, (error) => {
                       if (error){
                           callback(error);
                       }else{
                           callback(null, userInfo, roles);
                       }
                   })
               }
           })
       }
   })
};

```
Let's rewrite it using **Promise**

```js
const verifyAccount = function(username, password){
    db.verifyAccount(username, password)
    .then(userRole => db.getRoles(userRole))
    .then(userAccess => db.getAccess(userAccess))
    .then(final => {

        // do whatever the callback function would do

    }).catch(error =>{
        //handle error
    })
}
```

Now with **Async/Await**

```js
const verifyAccount = async function(username, password){
    try{
        const user = db.verifyAccount(username, password);
        const userRole = db.getRoles(user);
        const userAccess = db.getAccess(userRole);
        return user;
    }catch(e){
        // handle error
    }
}
```

Through the 3 examples above, in the JS development process, developers always try to optimize to help other developers can perform the process more effectively. With the declaration **async** in the function, it will ensure that it always return a **Promise** so you don't have to worry about it anymore.

