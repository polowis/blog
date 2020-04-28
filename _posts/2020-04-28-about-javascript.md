---
date: 2020-04-28 04:38:03
layout: post
title: "About JavaScript #1"
subtitle:
description: About JavaScript part 1
image: https://cdn.uconnectlabs.com/wp-content/uploads/sites/25/2020/01/Free-Courses-to-learn-JavaScript.jpg
optimized_image:
category: code
tags: 
    - js
    - es6
    - tips
author: polowis
paginate: false
---
JavaScript, the language has always been in top programming languages in recent years. It seems to appear in almost every aspect of programming such as web development, mobile appilcation development, VR, AR, 3D/2D games, etc. Even so, as a developer, I still don't fully understand why JavaScript is so popular and what's good about it. Why does everyone want to learn JavaScript, why is everyone a full-stack developer? Even from a person who doesn't understand JavaScript can still become a full-stack developer. 

Why is it so powerful and popular? In this article, I will cover basic JavaScript questions such as variables, lexical scope, functions, objects, DOM, prototype, etc.

#### History

Let's start with the history of JavaScript. In 1995, JavaScript was created by Brendan Eich for the purpose of the web. JavaScript has impressively impacted static web page by adding dynamic interactivity. JavaScript is incredibly powerful, people can make games, animated graphic from 2D to 3D and much more. 

More and more developers start using JavaScript, which increases the number of people who use JavaScript but doesn't really understand the beauty of it. 

#### Types and Variables

**Data types in JavaScript, how many are there?**

There are 5 data types and there are 3 ways you can declare a variable using ```const```, ```let```, and ```var```. But let's keep in simple for now. 

```js
const number = 10; // numeric
const string = 'string'; //string
const boolean = true; // boolean
const object = {
    name: 'Alice',
    email: 'alice@example.com'
}; //object
const arr = [1, 3, 5, 6, 10]; //array
```
**What about null, undefined, NaN?**

A variable will be **undefined** when it holds no value (it has not been assigned a value yet). For instance, if you declare a variable like this ```let a;```, at this time, the variable **a** has not been assigned a value, therefore will result in **undefined** Or when you try  to access an element that does not exist in the array/object will also return **undefined**. See this code below here:

```js
let a;
if (a === undefined) {
    console.log('a is undefined'); //a is undefined
}
let object = {name: 'Alice', age: 38};
console.log(object.email); //undefined

let array = [6 1, 9];
console.log(arr[4]); //undefined

```

What else about **undefined**? The data type of `undefined` is also **undefined**

**null, what is it?**
There are 3 ways that can cause a variable to return **undefined**, but only one will result in **null**. For instance, if I want to get the value of an element by finding its ```id```, say ```document.getElementById('whateverid')``` but there is no element with that id, at this time, the result will be **null**. Whenever you return an object and it turns out to hold nothing, that's a **null**. 

```js
let element = document.getElementById('whateverid');
console.log(element); 
//output: null
```
And believe it or not, if you try to access the type of null by using ```typeof``` syntax, it will return ```object``` as the result. **null** is an object. And the answer for why null is an ```object``` is because it wants to be an ```object```, therefore, its type is an ```object```. Although, **null** does not have properties like regular objects, such as ```toString``` function. 

**What about NaN, I see this multiple times**

**NaN** or Not a Number is simple but tricky. Here's why:

```js
let a = 0/0;
console.log(a); // NaN
console.log(typeof a); // number
console.log(a == NaN); //false
console.log(isNaN(a)); // true
```

**NaN** is assigned to variable that is not a number. In other words, the variable is a number but also not a number. Now, the funny part with the above code is if you look at the line ```typeof a``` it returns **number**. And when you try to print it out, it returns **NaN**. But if you look at the next line of code when I tried to compare **NaN == NaN** the result is return **false** -> it is not a **number**. There is no way it can be itself. Perhaps this is the only value type in programming languages that is not equal to itself.

**What is scope? What are local variables and global variables?**

Global variables are variables that can be used anywhere in the code, as long as you don't declare them in a function. In contrast, local variables are variables that are declared inside a function and can only be used inside that function. 

Back to the variables declaration. With the introduction of ES6, there are two new ways you can declare a variable ```const``` and ```let```. But before that let's look at how developers typically declare a variable in JavaScript in the past

```js
var globalVar = "This is global variable. You can use this one in any function.";

function foo() {
    var localVar = "This is local variable. You only can use this variable in funtion foo."
    console.log(globalVar); //This is global variable. You can use this one in any function.
    console.log(localVar); //This is local variable. You only can use this variable in funtion foo.
}
console.log(globalVar);  //This is global variable. You can use this one in any function.
console.log(localVar); // ReferenceError: localVar is not defined
```
**What is scope?**

Scope checks for the accessibility and visibility of the variable. As previously mentioned, the global variable is the global scope and the local variable is the function scope. Which means the global variable can be accessed from anywhere and the local variable is limited inside the function. There is also ```block scope```, following along with ```const``` and ```let``` help us achieve this. 

```const``` When you declare a variable with the **const** keyword, that value of the variable will remain unchanged. 

```js
const x = 10;
x = 5 // Error: Assignment to constant variable.
```
And the ```let``` keyword allow you to reassign the variable, similar to ```var```

```js
let x = 10;
x = 5 // otuput: 5
```

**Block Scope**: Whenever you see a block of code with curly brackets, that means a variable can only be accessed or modifed within that block of code. for example:

```js
function myFunction(){ // funcion block
    if(true){ // if block
        let x = 5; // block scope
        const y = 7; // block scope
        var z = 10; // function scope
    }
    // output
    console.log(x) // x is not defined
    console.log(y) // y is not defined
    console.log(z) // 10

}
```
The reason why the ```z``` variable is defined because the ```var``` keyword is not limited inside ```block scope```. 

<img src="https://www.constletvar.com/const-vs-let-vs-var.png">

#### Coercion

What is it? This is one of the way to convert type of values in JavaScript, implicitly also known as **type casting**. The coercion is the big mystery behind JavaScript's mathematical operations comparasion. Let's start with equality operator ```==```

```js
console.log(1 == '1');
//output: true
```
Why is this happening? A number equal a string? Before the comparasion actually happened, JavaScript performs the coercion. In other words, if two values have the same type, the just compare it but if they don't, JavaScript will try to convert them into the same types and then compare. Here the number 1 remains the same but the string ```"1"``` have been converted to the same type which is **number**. Therefore results in ```true```. Coercion does not follow any logic, it follows the rules we have to remember when apply it to many situations. I will list some example here:

**Number vs String**

The **string** type will be converted to **number** then do the comparison. This can lead to 2 outcomes. The first outcome is the **string** is convertible and can be converted into **number** (for example: ```"23"```, ```"1234"```, etc). We have no problem with the first outcome. But let's look at the second outcome. The **string** cannot be converted into number (eg: 'abc', 's1fe13324', etc. ) these values will be converted into **NaN** and as mentioned earlier, **NaN** is not equal to anything, therefore, the result is always ```false```

**Boolean vs other types**

First, the boolean value is converted into number  such as ```true => 1, false => 0``` and then make the comparison. For example "1" == true will be changed to 1 == 1 because true is converted into number 1 and string "1 "convert into number 1, resulting in true.

**null vs undefined**

```js
null == undefined
// output: true
```
Why true? It's the way how JavaScript works?

##### How to not get these silly errors?

**Strict equality operator (===)**

This operator will only compare 2 value, just it, no coercion, no convert type

```js
1 === "1"
// output: false
```

**Other mathematical operations (+, -, *, /)**

If you using it with **number** versus **number**, there is nothing else to say here. But if **number** with **string**, except for **+** operator, it will convert numbers into strings and then conduct string concatenation, with other operators (-, *, /) will convert **string**  into **number** and perform operator normally. What if you can't convert? The result is **NaN** in that case. 

```js
4 + "3"; // "43"
4 - "3"; // 1
"4" * "3"; //12
"4" / 3; //1.33333

4 + "a" ; "4a"
4 - "a"; // NaN
4 * "a"; // NaN
4 / "a"; //NaN
```


