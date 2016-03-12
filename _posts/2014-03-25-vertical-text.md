---
layout: post
title: 使用css来垂直放置文本
categories:
- Life
tags:
- css
---

有时候在web开发中，我们可能需要把HTML元素垂直显示比如文字。一般碰到这样的需求，我们会设置元素的宽高等属性来达到这样的目的，或者是用图片代替。不过CSS3的出现解决这样的问题就容易多了。

首先来看看实际的效果：

<p data-height="268" data-theme-id="0" data-slug-hash="ouEas" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/ouEas/'>ouEas</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

### CSS ###

使用CSS的transform很容易就可以完成这样的效果：

    .vertical-text {
	transform: rotate(90deg);
	transform-origin: left top 0;
	}

取决于你旋转的方向，你可以设置不同的旋转角度。我们还可以给一个浮动的设置，这样元素就会自动来调整它的宽度：

    .vertical-text {
	float: left;
	}

简单的几句代码就可以实现文字的垂直排列，CSS3的出现的确大大的提高了生产效率，不是吗？