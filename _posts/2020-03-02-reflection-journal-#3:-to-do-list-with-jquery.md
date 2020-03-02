---
date: 2020-03-02 08:18:09
layout: post
title: "Reflection Journal #3: To-do list with jQuery"
subtitle:
description: I built a to-do list app with jQuery in 1 hour.
image: https://www.edureka.co/blog/wp-content/uploads/2018/12/What-is-JavaScript-jQuery-Tutorial-1.png
optimized_image:
category: blog
tags:
    - programming
    - reflection
    - javascript
author: polowis
paginate: false
---

The famous application for beginners is the to-do app. It can go from the very basic to the more advanced. It's easy to create and also hard if you want to make it more complicated. The hardest part of this application, I think is the local storage, how should I use the web storage effectively. 

## What is to-do list?
To-do list is a list of tasks that need to be completed. I will create a tool or a program (whatever you would like to call it) that help other people manage their life effectively. An app that will motivate you, increase productivity and automatically generate task upon your requests. I will be using the famous JavaScript library, jQuery.

To-do app consists of multiple features such as create tasks, delete tasks, update tasks and read tasks. These four basic functions make up the foundation of the term **CRUD**. 

## The create function

As the user's view, whenever the user clicks on the add button, we want their new items to be displayed as well as not delete the old items. That's fairly easy to do, when the button is clicked, I'm gonna grab the text of the input field and render it as a HTML element. 

```js
 $(document).ready(
    function(){
        $('#button').click(
            function(){
                var itemToBeAdded = $('input[name=ListItem]').val();
                 $('ol').append('<li>' + itemToBeAdded + '</li>');
            });
       
```

With the code above, the application will run smoothly just like what I expected. But to-do app is more than that. We will need a delete function, so the user can select which task to delete or which task is finished. I'm not going to add more properties such as progess tracking, due date for each tasks, etc. 

## The delete function

Just like other to-do app, normally, they will have a delete button displayed somewhere on the web page. But I want to make some changes to the delete button, I don't want to follow other people style, I want to make something unique. Whenever the user double-clicks on the item, I will cross that item off. Too many buttons are not going to look great on the web page, some people might find it hard to do something because they often get confused with many buttons or interactive elements. 

```js
$(document).on('dblclick','li', function(){
        $(this).toggleClass('strike').fadeOut('slow');    
      });
```
All I need to do is listen to the double click event (dbclick), get the element and prevent it from being displayed. 

## The edit function

What's wrong with a button called *edit* being displayed next to the item? No, there is nothing wrong with it, But I want to get rid of many buttons as possible. I don't want to display an edit button, delete button, add button, clear button and even update button. It's just going to make people confused. 

Instead of double click, when the user click on the item, I want to make the field editable. Sounds like lots of works but nope. Here is what I did 

```js
 $(document).on("click", 'li',  function(){
       $(this).closest("li").prop("contenteditable", true).focus();
```

## The local storage

Let's think about how I'm going to store these items. There are many ways to do it, I can use the database, I can use local storage, I can even use JSON file. But I'm just going to stick with local storage due to its simplicity. 

I will start with the variable that holds the value of the items in local storage. 

```js
var data = JSON.parse(localStorage.getItem("todo"));
data = data || {};
```

Then I need to update the create function, so every item added will be saved to local storage. In order to make my life easier when manage item, every item will have its own unique id, the id will be the time when the item was created.

```js
$('#button').click(
            function(){
                if (!localStorage) return alert('Storage is not available in your browser!');
                var today = new Date();
                var date = today.getFullYear()+'-'+(today.getMonth()+1) + '-'+ today.getDate();
                var itemTobeAdded = $('input[name=ListItem]').val();
                let id = new Date().getTime()

                 if(isValidated(itemToBeAdded)){
                  var tempData = {
                      id: id,
                      task: itemToBeAdded,
                      date: date
                  }
                  
                  data[id] = tempData
                  localStorage.setItem('todo', JSON.stringify(data))
                  
                  generateElement(tempData)
                  $('input[name=ListItem]').val('');
                  $('#error').text('')
                 } else{
                   
                 }
                 
            });
```
There are more works can be done with this function, perhaps you can add the title of the item, the description of it and maybe the due date if possible. And that's easy, I'm not going to implement something that would do the exact same thing as the old one, but with more fields. 

```js
/* The generate element function from the code above*/
const generateElement = (value) => {
  $('ol').append(`<li id="${value.id}" class="toDo">` + `${value.task}` + ` </li>` + `<b class="toDo">Posted at: `+ value.date + "   "+ `</b> ` );
}
```

## The read function

I have to read whatever in the local storage and display it on the web page. The **data** variable contains all the things that I need, my job is just going to display it. I'm going to use the for-loop so I can run the code multiple times until all the items in the local storage are displayed.

jQuery already has a function called **.each()** that does this job for us. 

```js
$.each(data, function(key, value){
    generateElement(value)
})
```

## Conclusion

There are many feature that I could possibly implement in the app. But I decided to not to, those features mostly follow the same pattern, like the one that I already implemented. For instane, the title of the task, I can easily add another input field which means I have to add another properties in the local storage and add another variable to display it on the screen, pretty much the same approach like what I just did. 

Something I didn't know about local storage, it can holds multiple value just like JSON :)