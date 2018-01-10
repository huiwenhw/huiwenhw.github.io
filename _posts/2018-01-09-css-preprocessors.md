---
layout: post 
title: CSS - Preprocessors 
category: normal
---

What is a CSS preprocessor? It extends the CSS language and provides extra functionality on top of CSS. [SASS](http://sass-lang.com/), [less](http://lesscss.org/) and [stylus](http://stylus-lang.com/) are the more well known preprocessors. 

Why use preprocessors? The funtionality they provide make CSS easier to work with and increases readability. Some of the useful functionalities are listed below. I will be using the SASS language for examples! 

## Variables
With preprocessors, we are able to declare our own variables for consistency:
```sass
$my-font: Helvetica, sans-serif;
$light-grey: #d3d3d3;
$dark-grey: #666666;

body {
	font: $my-font;
	background-color: $dark-grey;
}
```
We can store colors, fonts, or even numbers for opacity etc. The example above compiles to:
```css
body {
	font: Helvetica, sans-serif;
	background-color: #666666;
}
```

## Nesting 
Nesting increases readibility:
```sass
#nav {
	ul {
		list-style-type: none;
	}
	li {
		color: $dark-grey;
	}
}
```
We know at a glance that ul and li are children of the #nav element. We can also use psuedo-selectors such as the & in nesting: 
```sass
a {
	&:hover {}
	&:active {}
}
```
This compiles to:
```css
a:hover {}
a:active {}
```
There are a whole bunch of other selectors mentioned in this [css-tricks article](https://css-tricks.com/the-sass-ampersand/), so check it out if you're interested! 

## Mixins 
Mixins allow reusability of css declarations and it is very useful if we want to declare similar styles for multiple components or for cross-browser styling. An example taken from the [SASS website](http://sass-lang.com/guide#topic-6): 
```sass
@mixin border-radius($radius) {
  -webkit-border-radius: $radius;
     -moz-border-radius: $radius;
      -ms-border-radius: $radius;
          border-radius: $radius;
}

.box { @include border-radius(10px); }
```
We have created a mixin (think of it like a function) that declares the different border-radius for cross browser styling. It takes in the radius that we want to declare and outputs:
```css
.box {
  -webkit-border-radius: 10px;
  -moz-border-radius: 10px;
  -ms-border-radius: 10px;
  border-radius: 10px;
}
```
Mixins are very flexible and great to use if you've code that is reusable! 
