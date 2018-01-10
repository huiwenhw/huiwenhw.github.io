---
layout: post 
title: CSS - Styling placeholders
category: normal
---

How do we style input placeholders text? Say I have an input:

```css
input::-webkit-input-placeholder { /* Chrome/Opera/Safari */
  color: pink;
}
input::-moz-placeholder { /* Firefox 19+ */
  color: pink;
}
input:-ms-input-placeholder { /* IE 10+ */
  color: pink;
}
input:-moz-placeholder { /* Firefox 18- */
  color: pink;
}
```

The usual way would be to do the above but what if we had multiple placeholders we want to style and we want to style them differently? That's when mixins come in handy. First, we declare the mixin:

```sass
@mixin placeholder {
  &::-webkit-input-placeholder {@content}
  &:-moz-placeholder           {@content}
  &::-moz-placeholder          {@content}
  &:-ms-input-placeholder      {@content}  
}
```

And we use it by:

```sass
input { 
	@include placeholder {
		font-weight: bolder;
		color: white;
	}
}
```

When we do this, the @content variable is substituted by whatever we declared in the curly braces {} when we called @include placeholder. The above compiles to:
```css
input::-webkit-input-placeholder { 
	font-weight: bolder;
	color: white;
}
```
We can then reuse the placeholder mixins by changing its content! :) 

Credits:
- [CSS tricks styling placeholders](https://stackoverflow.com/questions/17181849/placeholder-mixin-scss-css#answer-20320475)
- [Stackoverflow styling placeholders](https://stackoverflow.com/questions/17181849/placeholder-mixin-scss-css#answer-20320475)
