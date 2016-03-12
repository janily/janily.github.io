---
layout: post
title: 	只需要3行CSS代码就搞定元素垂直对齐
categories:
- Life
tags:
- css
- 翻译
---

> 页面中元素垂直对齐这个需求在web开发中是非常常见的，比如文字垂直对齐或者是图片对齐等等。方法也是多种多样，比如已知一个元素的宽高那么就可以使用定位的方式来对齐，不过最常见的是不知道一个元素具体的宽高而要垂直对齐解决方法也是层出不穷。今天看到一篇文章讲到的是使用CSS3中的transform属性实现垂直对齐的另外一种思路非常不错。不过由于是使用了CSS3，在IE浏览器中有兼容性问题了，要IE9以上的浏览器支持transform这个属性。

在**transform:translateY**的帮助下仅仅只需要三行CSS代码就能实现元素的垂直对齐即使不知道元素高度(当然不包括各种浏览器前缀的CSS代码)。

在CSS3中**transform**属性是用于元素的旋转和变形的，但是**translateY**这个属性可以用来实现元素的垂直对齐。一般我们会使用绝对定位或者是行高来垂直对齐元素，不过这些方法都有一个缺陷就是要知道元素的宽高才会起作用。

如果是使用translate的话，我们可以这样写：

    .element {
		position:relative;
		top:50%;
		transform:translateY(50%);
	}

就是这么简单。看起来跟绝对定位垂直对齐的方法差不多，但是这里我们不需要知道元素的具体的高度或者是设置元素父元素的定位属性。

当然为了能在其它浏览器能正常运行我们需要添加其它浏览器的前缀，我们可以把它定义为一个SASS的**mixin**，如下所示：

    @mixin vertical-align {
	  position: relative;
	  top: 50%;
	  -webkit-transform: translateY(-50%);
	  -ms-transform: translateY(-50%);
	  transform: translateY(-50%);
	}
	
	.element p {
	  @include vertical-align;
	}

或者是使用SASS的[placeholder selector](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#placeholder_selectors_)：

    %vertical-align {
	  position: relative;
	  top: 50%;
	  -webkit-transform: translateY(-50%);
	  -ms-transform: translateY(-50%);
	  transform: translateY(-50%);
	}
	
	.element p {
	  @extend %vertical-align;
	}

下面来看一个实例：

<p data-height="268" data-theme-id="0" data-slug-hash="owmqj" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/owmqj'>Vertical center with only 3 lines of CSS</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

原文地址[Vertical align anything with just 3 lines of CSS](http://zerosixthree.se/vertical-align-anything-with-just-3-lines-of-css/)。



