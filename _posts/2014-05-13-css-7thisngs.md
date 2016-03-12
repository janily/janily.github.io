---
layout: post
title: 7件你可能还不知道使用CSS也能做到的事情
categories:
- Life
tags:
- css
- 翻译
---


> 在互联网web开发技术日新月异的时代，CSS和javascript能做的事情也越来越多。今天这篇文章，我们就来看看7件你可能还不知道使用CSS也能做到的事情。不需要依赖javascript，图片。文章原文地址是[7 Things You Didn’t Know You Could Do with CSS](http://davidwalsh.name/css-facts)。

### CSS @supports ###

在日常的前端开发中，我们一般会使用一些javascript来检测浏览器是否支持某一项特性，比如现在流行的**Modernizr**就是一个非常好的浏览器特性检测javascript库。其实在CSS的最新标准中有一个**@supports**属性，也可以用来检测浏览器特性。下面我们来看看**@supports**是如何使用的：

    /* basic usage */
	@supports(prop:value) {
		/* more styles */
	}
	
	/* real usage */
	@supports (display: flex) {
		div { display: flex; }
	}
	
	/* testing prefixes too */
	@supports (display: -webkit-flex) or
	          (display: -moz-flex) or
	          (display: flex) {
	
	    section {
	      display: -webkit-flex;
	      display: -moz-flex;
	    	display: flex;
	    	float: none;
	    }
	}

从上面简单的几行代码，我们可以发现使用**@supports**也可以来检测浏览的特性。不过现在还不能在生产环境中使用，因为浏览器支持度还比较低。只能希望各大浏览器厂商赶紧跟进早日支持**@supports**特性。

### CSS 滤镜 ###

写一个图片滤镜的应用就能卖几十亿美金给facebook，没错说的就是Instagram。开玩笑啦，写个图片滤镜应用当然不是那么简单的。我写过一个基于canvas的图片滤镜应用[tiny app](https://github.com/darkwing/fotofilter)，不过我们今天要说的是使用CSS中的绿影，如下代码所示：

	/* simple filter */
	.myElement {
		-webkit-filter: blur(2px);
	}
	
	/* advanced filter */
	.myElement {
		-webkit-filter: blur(2px) grayscale (.5) opacity(0.8) hue-rotate(120deg);
	}

使用CSS滤镜并不会真正改变你的原始图片的属性，而只是在原始图片添加一个蒙版类似的东东来实现滤镜效果。具体滤镜的使用方法可以去看看[这篇文章](http://davidwalsh.name/css-filters)。

可以点击[这个地址](http://davidwalsh.name/demo/css-filters.php)去看看各种不同滤镜的应用效果。

### Pointer Events ###

pointer-events:none顾名思意，就是禁用鼠标事件的意思。元素应用了该CSS属性，链接啊，点击都会木有反应啦。

	/* 点击等事件都会木有反应 */
	.disabled { pointer-events: none; }

	/* 就算是使用js绑定了事件也没用 */
	document.getElementById("disabled-element").addEventListener("click", function(e) {
		alert("Clicked!");
	});

在上面的代码中由于对元素使用了**pointer-events: none;**的属性，所以该元素上的事件就统统木有反应啦。详细的应用可以去[这里看看](http://www.zhangxinxu.com/wordpress/2011/12/css3-pointer-events-none-javascript/)。

### 手风琴切换效果 ###

在以前实现手风琴的切换动画效果需要借助javascript才能实现。现在好了，使用CSS我们也可以实现这样的动画效果，以下就是核心代码：

	/* slider in open state */
	.slider {
		overflow-y: hidden;
		max-height: 500px; /* approximate max height */
	
		transition-property: all;
		transition-duration: .5s;
		transition-timing-function: cubic-bezier(0, 1, 0.5, 1);
	}
	
	/* close it with the "closed" class */
	.slider.closed {
		max-height: 0;
	}

代码之中巧妙的使用了**max-height**这一属性来实现动画效果。

[实际效果地址](http://davidwalsh.name/demo/css-slide.php)。

### CSS Counters(CSS计数器) ###

本质上CSS计数器是由CSS维护的变量，这些变量可能根据CSS规则增加以跟踪使用次数。这允许你根据文档位置来调整内容表现。我们可以使用CSS计数器来计算元素的数量，使用元素的**:bdfore**或者是**:after**属性来显示计数结果。

	/* initialize the counter */
	ol.slides {
		counter-reset: slideNum;
	}
	
	/* increment the counter */
	ol.slides > li {
		counter-increment: slideNum;
	}
	
	/* display the counter value */
	ol.slides li:after {
		content: "[" counter(slideNum) "]";
	}

比如在常见的幻灯片切换或者是列表中可以使用CSS计数器。[实际例子](http://davidwalsh.name/demo/css-counters.php)。

这里推荐两篇关于CSS计数器的文章可以去看看，[文章一](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Counters)，[文章二](http://css-tricks.com/almanac/properties/c/counter-increment/)。

### 使用Unicode字符编码来定义你的CSS类 ###

在前端开发中，CSS类的命名是一项非常重要的工作。不过你有见过使用[unicode编码的形式来命名class的名字么](http://davidwalsh.name/unicode-css-classes)：

	.ಠ_ಠ {
	border: 1px solid #f00;
	background: pink;
	}
	
	.❤ {
		background: lightgreen;
		border: 1px solid green;
	}

不过在实际开发中可千万不要这样使用哦，玩玩还可以。[实际代码例子](http://davidwalsh.name/demo/unicode-css-classes.php)。

### CSS Circles ###

利用[CSS来制作一些小的三角形](http://davidwalsh.name/css-triangles)，在web开发中应用的十分普遍，同样是用**border-radius**这一属性我们也可以创建圆形。

	.circle {
		border-radius: 50%;
		width: 200px;
		height: 200px; 
		/* width and height can be anything, as long as they're equal */
	}

我们也可以使用CSS中渐变来定义渐变颜色。如下图所示：

![](http://davidwalsh.name/demo/css-circles.png)

关于CSS形状的知识可以去看看[这篇文章](http://alistapart.com/article/css-shapes-101)。

OK，这就是有趣的CSS中而且非常有用的7个你可能平时没有太关注的属性。



