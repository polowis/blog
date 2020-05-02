---
date: 2020-05-02 06:51:00
layout: post
title: "JavaScript: Strict Mode"
subtitle:
description:
image:
optimized_image:
category:
tags:
author:
paginate: false
---

## What is strict mode?

Let's take a look at this statement

```js
"use strict";
```

Perhaps, you have seen this statement multiples times when working with JavaScript. And it appears in almost all modern JavaScript libraries and frameworks. So what exactly is ```use strict```, how does it effect the way how you write code and is it neccessary for you to use it. 

```strict mode``` is a feature that brought by ES5. Now let's dive deeper. 

The word **strict** is clear enough for everyone to understand it. I don't think it I need to provide explanation here. **Strict mode** is a stricter pattern of writing JavaScript code. If you consider the way how you typically learn or use JavaScript as **Normal mode**, then **Strict mode** will have many other rules than **Normal mode**. And that makes a normal function that can run smoothly become error.

**Strict mode** was created to prevent programmers from executing processes or functions that are considered to be **unsafe**. It also disables features that may become confusing or should not be used and prevent the use of some words that may be used as keywords in the future. 

As easier stated, **strict mode** has more restrictions than **normal mode**. And by following these rules, you will make your code cleaner, easier to read, potentially not encounter unwanted errors. 

Now, why don't people just change the ECMAScript completely to what is mentioned in **Strict Mode**, but instead create another feature?

Perhaps, part of it is to ensure that some backward compatibility between versions, the ES5 and ES3, furthermore, it makes ECMAScript keeps its simplicity and flexibility from previous versions, rather than being limited by new rules. 

But after you get used to it, you need to change the way how you write code. And that's why you need ```strict mode```

### Using strict mode

In order to use ```strict mode```, you will need to declare it. 

```js
"use strict";
```
It's quite weird when you first see it, it's a ... string.


You can put that text at the beginning of the file, or at the beginning of the body of a function. The declaration ```"use strict";``` Where will determine the scope of the effect of Strict Mode.

If you declare ```"use strict";``` At the top of the file, the scope of Strict Mode will be the entire file.

```js
"use strict";
function foo(){
    var bar = 0;
    return bar;
}

// Uncaught ReferenceError: bar is not defined
bar = 1;

```

If you declare ```"use strict";``` At the top of the function, Strict Mode will only be applied to that function.

```js
function foo(){
    "use strict";
    // Uncaught ReferenceError: bar is not defined
    bar = 0;
    return bar;
}

// This will run normally
bar = 1;
```


### Some limited in Strict mode

Let's see some rules in strict mode, which you won't be able to do once you use strict mode.

#### You cannot use a variable without first declaring it.

Typically in JavaScript, you can use the ```var``` keyword to declare a variable, with ES6, you can even use ```let``` or ```const```. However, you can still use the variable without declare it with any keyword.

For example, look at the following code. 
```js
var foo = 1;
console.log(foo); // output: 1
bar = 2;
console.log(bar); // output: 2

```

The reason there is no error is because if you do not use the ```var``` keyword, we will treat the variable used as a Global variable. We can see the difference between using var and not using var through the following example:

```js
// Global variable
function foo() {
    bar = 1;
}

foo();
console.log(bar) // 1

// Local variable
function foo() {
    var bar = 1; 
    console.log(bar); 
}

foo();
console.log(bar); // ReferenceError: bar is not defined

```

As you can see, normally, if you have "forgot" (or purposely "forgot"), without using var (or let) to declare variables, your javascript code will still run normally.

But with **strict mode**, you won't be able to do it. 

#### Throw errors when assignments cannot be performed

Normally, if an object has a property ```writable``` equals false, then of course, you will not be able to overwrite the data on that property. But the problem is that the code keeps running. In **strict mode**, an error will be thrown. Now to understand it better, let's consider the following code

**Without strict mode**

```js
NaN = 1; // nothing happen
var object = {}
Object.defineProperty(object, 'prop', {value: 1, writable:false});
object.prop; // => 2
object.prop = 10;
object.prop; // => 2

```

**With strict mode** 

```js
NaN = "error"; // TypeError
var object = {};
Object.defineProperty(object, 'prop', {value: 1, writable:false});
object.prop; // => 2
object.prop = 10; 
// // Uncaught TypeError: Cannot assign to read only property 'prop' of object #<Object>

```