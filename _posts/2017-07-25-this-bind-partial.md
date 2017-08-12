---
layout: post 
title: this, bind() and partially applied functions 
category: normal
---

This post is to help us to understand more about the 'this' keyword in JS, bind() method and what are partially applied functions. 

# Understanding bind() and 'this' 
The bind() method allows us to create a function that has a particular 'this' value. We'll use the example from [MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind#Examples) to understand bind(). Follow along in your console! 

We first create a global x variable.
```Javascript
this.x = 9;
```

The 'this' keyword here refers to the global object in our browser, aka the window. 

Try it! Type: this === window in ur dev console // true

We then create a module object:
```Javascript
var module = {
    x: 81, 
    getX: function() { return this.x; }
}
```

Now, when we call module.getX(), we get 81 cause 'this' refers to the module object and we just declared that x = 81.
```
module.getX();  // 81
```
We then test our knowledge of 'this'. What will retrieveX() return? 
```
var retrieveX = module.getX; 
retrieveX();    // 9!
```

When we call retrieveX(), we're calling it in the window, which is the global scope. What retrieveX() does is it returns the this.x value. In this case, our 'this' keyword refers to the global object and thus is actually returning the global x variable == 9;

This is where bind() comes in handy. If we want retrieveX() to return the x: 81 value in the module object, we can do 
```
var boundGetX = retrieveX.bind(module);
boundGetX();    // 81
```

Now, we are binding the module object's 'this' keyword to the retrieveX function. So whenever we call boundGetX(), it is returning the x:81 value in the module object.

Okay, now we want to test our understanding of 'this'. Q&A:  
```
this.x = 9;

var module = { 
    x: 81, 
    getX: function() { return this.x; }
}

var firstX = module.getX();
firstX;
````
Answer? 81! We're using the module.getX() func, so no tricks here. 

```
this.x = 9;

var module = { 
    x: 81, 
    getX: function() { return this.x; }
}

var secX = module.getX; 
secX(); 
```
Answer? 9! secX is now a function that returns this.x; We're calling it in the global scope, so its 9.

```
this.x = 9;

var thirdX = { 
    x: 11,
    getX: function() { return this.x; }
}

var retrieveThird = thirdX.getX;
retrieveThird();
```
Answer? 9! Same as the above, thirdX is now a function that returns this.x;  We're calling it in the global scope, so its 9.

```
this.x = 9;

var thirdX = {
    x: 11,
    getX: function() { return this.x; }
}   

var retrieveFour = retrieveThird.bind(thirdX);
retrieveFour();
```
Answer? 11! We binded the 'this' keyword to the thirdX object, so this.x refers to 11 now.

# What are partially applied functions in Javascript?
Partially applied functions are useful when we don't have all of the required arguments when using a particular function.
We can take a function that takes in two parameters and create a partial function that only takes in one parameter. We can then call the same partial function that we declared using another parameter to complete the initial function. 
Example: 

```Javascript
function add(a, b) {
    return a + b;
}

// Here, we're creating a partial function of add() that takes in only one argument
// using the 'bind' method. 
var partial = add.bind(null, 1);

// Completing our partial function
var complete = partial(2);
console.log(complete);  // Console logs 3 
```
<br>
So yep that's all I have for this post! Lemme know if I got my stuff wrong: huiwenhoshi@gmail.com (:
