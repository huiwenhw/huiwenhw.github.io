---
layout: post
title: CSS - Centering a div
category: normal
---

In my opinion, centering is a must-know technique for front end developers. There are many ways we can do this according to [this article](https://css-tricks.com/centering-css-complete-guide/) depending on how you want to center your element, but in this post I will touch on centering elements vertically and horizontally with examples below:

<div class="parent">
	<div class="child-one">
		First method: Using top, left and transform
	</div>
</div>
The above is useful if we do not know the height and width of our div. By doing top and left to 50%, we're positioning the top left corner of our inner div at the center of its parent. To make the inner div at the center, we can then do a negative 50% transform which will be calculated based on our inner div's width and height.
To understand the above, try using properties top and left before adding the transform property!  CSS:
```css
outer-div {
	position: relative;
}
inner-div {
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
```

<div class="parent" class="parent-two">
	<div>
		Second method: Using flexbox
	</div>
</div>
For flexbox, css tricks has another awesome [post](https://css-tricks.com/snippets/css/a-guide-to-flexbox/) on it. And its really easy to center a div. We use `align-items` to center items vertically and `justify-content` to center items horizontally. 
```css
outer-div {
	display: flex;
	align-items: center;
	justify-content: center;
}
```
My preferred option is flexbox as it is really easy to work with! 

