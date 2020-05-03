---
date: 2020-05-02 06:51:00
layout: post
title: "JavaScript: Strict Mode"
subtitle:
description: Understand strict mode
image: https://i0.wp.com/programmingwithmosh.com/wp-content/uploads/2019/05/strict-mode-__.png?ssl=1
optimized_image:
category: code
tags:
    - programming
    - js
    - javascript
author: polowis
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


### Some limitations in Strict mode

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

```js
"use strict";

a = 1;
// ReferenceError: a is not defined
```

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
<img src="https://flaviocopes.com/javascript-strict-mode/read-only-property.png">

#### Throws errors when you try to delete undeletable properties.

There will be errors when you perform operations to remove variables, functions, or arguments.

In addition, there will also be errors when you intentionally delete a property of an object that cannot be ```configurable```.


```js
"use strict";
var foo = 1;
function bar() {};
delete foo; // Uncaught SyntaxError: Delete of an unqualified identifier in Strict Mode.
delete bar;

var obj = {};
Object.defineProperty(obj, "baz", {
    value: 1,
    configurable: false
});
delete obj.baz; // Uncaught TypeError: Cannot delete property 'baz' of #<Object>

```

Please note that even in```normal mode```, you will not be able to perform the above example. However, the code will still run normally without any errors.

#### Functions parameters cannot be the same

If the parameters of a function have the same name, it will throw an error.

```js
"use strict";
function foo(bar, bar) { 
    // Uncaught SyntaxError: Duplicate parameter name not allowed in this context

}
foo(1, 2);
```

This will help you avoid silly errors if you accidentally write a wrong parameter's name. Having two or more parameters with the same is probably only caused by mistakes, there's almost no need for such as case.

While in ```normal mode```, when you have parameters with the same name upon declaring a functon, of course the value of this parameter will override the previous one.

#### You cannot use octal numbers

Normally, if a number starts with 0, then JavaSript will interpret that as base 8 (octal). For example ```010``` === ```8``` will return ```true```.

However, not everyone knows that, and the writing of ```010``` will probably make many people still understand that it is ```10```. Therefore in ```Strict Mode```, it is considered a syntax error.

But with the introduction of **ES6**, you can still use the above cases by using prefix **0o**.  Even in ```strict mode```, ```var foo = 0o10``` is still considered as a valid way of writing, and ```0o10 === 8``` will return ```true```.

#### You cannot use with

```with``` is a dangerous statement, which can be confusing in many cases. Therefore, in ```Strict Mode```, it is completely removed, and if you intentionally use ```with```, you will get a Syntax Error.


```js
"use strict";
var foo = 1;
var bar = {foo: 2}
with (bar) {
  console.log(foo); 
  // here you might get confused in determine whether foo is a variable or a property of bar
}

```

#### You cannot use variables that are declared inside eval
Normally, if your ```eval``` function has a variable declaration, the scope of that variable will be Global, or inside the function, where eval is called.

```js
eval("var foo = 1");
foo // output: 1
```
But in ```strict mode```, you cannot use the variables outside ```eval```function.

```js
"use strict";
eval("var foo = 1");
foo // Uncaught ReferenceError: foo is not defined
```

#### You cannot use eval and arguments as an identifier

In ```strict mode```, you will not be able to the keyword ```eval``` and ```arguments``` as variables name or function names or parameters name, etc.. If you try to use it purposely or unintentionally, it will throw syntax error.

```js
"use strict";
var eval = 1;
// Syntax Error
function arguments() { };
var foo = function eval() { };
function bar(eval) { };

```

#### You cannot declare a function inside a block statement.

```strict mode``` only allows you to declare a function in the outer scope or right inside a function. You will not be able to declare a function inside an ```if``` statement, or ```for``` loop, or a block (anything that start and ends with ```{}```).

```js
"use strict";
function foo() {
    function bar() { }; // OK
}

if (true) {
    var baz = function () { return true }; // OK
}

{
    function qux() { return true }; // SyntaxError
}
```

#### You cannot use some words that may become keywords in the future


From ES5, with Strict Mode, a name *will probably be used* in the future as a keyword could no longer be used as the ```identifier```.

These words are included: 
1. ```implements```
2. ```interface```
3. ```let```
4. ```package```
5. ```protected```
6. ```private```
7. ```public```
8. ```static```
9. ```yield```

```js
"use strict";
// Uncaught SyntaxError: Unexpected Strict Mode reserved word
var let = 1;
function public() { };
```
----------------------------------------------------------------

Strict Mode was created to help you avoid some unnecessary or silly errors when working with Javascript, it makes your code looks cleaner and more readable.


Although when working with Strict Mode, you may initially have a lot of difficulties when some features can no longer be used, but in fact, what becomes bugs in ```Strict Mode``` are all things you should never be committed even when writing code in ```non-strict Mode```.

If you ask whether you use Strict Mode, I think it's YES!
