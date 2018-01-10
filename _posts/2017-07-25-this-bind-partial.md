---
layout: post 
title: this, bind() and partially applied functions 
category: normal
---

I recently came across partially applied functions, and in order to understand that, it helps to know more about the `this` keyword in Javascript and the bind() function. So this post is to help us understand more about them (:

# Understanding bind() and 'this' 
The bind() method allows us to create a function that has a particular 'this' value. We'll use the example from [MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind#Examples) to understand bind(). Follow along in your console! 

We first create a global x variable.
```javascript
this.x = 9;
```

The 'this' keyword here refers to the global object in our browser, aka the window. 

Try it! Type: this === window in ur dev console // true

We then create a module object:
```javascript
var module = {
    x: 81, 
    getX: function() { return this.x; }
}
```

Now, when we call module.getX(), we get 81 cause 'this' refers to the module object and we just declared that x = 81.
```javascript
module.getX();  // 81
```
We then test our knowledge of 'this'. What will retrieveX() return? 
```javascript
var retrieveX = module.getX; 
retrieveX();    // 9!
```

When we call retrieveX(), we're calling it in the window, which is the global scope. What retrieveX() does is it returns the this.x value. In this case, our 'this' keyword refers to the global object and thus is actually returning the global x variable == 9;

This is where bind() comes in handy. If we want retrieveX() to return the x: 81 value in the module object, we can do 
```javascript
var boundGetX = retrieveX.bind(module);
boundGetX();    // 81
```

Now, we are binding the module object's 'this' keyword to the retrieveX function. So whenever we call boundGetX(), it is returning the x:81 value in the module object.

To test that we really know what's going on, let's have a super short Q&A: 
```javascript
this.x = 9;

var module = { 
    x: 81, 
    getX: function() { return this.x; }
}

var firstX = module.getX();
firstX;
```
What do we get? 81! We're using the module.getX() func, so no tricks here. 

```javascript
this.x = 9;

var module = { 
    x: 81, 
    getX: function() { return this.x; }
}

var secX = module.getX; 
secX(); 
```
What do we get? 9! secX is now a function that returns this.x; and we're calling it in the global scope, so its 9.

```javascript
this.x = 9;

var thirdX = {
    x: 11,
    getX: function() { return this.x; }
}   

var retrieveThird = thirdX.getX;
var retrieveFour = retrieveThird.bind(thirdX);
retrieveFour();
```
What do we get? 11! We binded the 'this' keyword to the thirdX object, so this.x refers to 11 now.

# What are partially applied functions in Javascript?
Partially applied functions are useful when we don't have all of the required arguments when using a particular function.
We can take a function that takes in two parameters and create a partial function that only takes in one parameter. We can then call the same partial function that we declared using another parameter to complete the initial function. 
Example: 

```javascript
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
