---
layout: post
title: How To Write Mobile-first css
categories:
- Life
tags:
- css

---

对于身处移动互联网时代的前端工程师来说，面向多终端设备来构建响应式网站已经是一项必不可少的技能。所谓响应式网站，其实就是移动优先的一种网站开发的策略。

对于设计师来说，移动优先的设计理念是非常重要的。可是对于前端开发来说(这里主要是指网页重构这部分)，在代码上移动优先确没有一个很好的编写策略。

今天，我们就来谈谈移动优先的css编写方法，从中，你就可以看出它可以让我们在面向多终端设备构建网站的时候，能够做的更好。

(如果你有用到Susy这个css框架，那这篇文章就更加是你要阅读的)

### 移动优先和面向桌面设备(PC)之间的不同

在开始移动优先css代码编写方法之前，我们先来看看面向桌面设备和面向移动设备css代码编写之间的差异。

移动优先的css编写意味着css首先是要适配移动设备的。然后再是使用媒体查询方法来适配其它桌面等设备。

主要是运用了**min-width**这个媒体查询的技术。

来看下面这个例子：

	
	body { 
  		background: red; 
	}

	// 这是制定屏幕宽度为600以及600px以上设备的body的背景颜色
	@media (min-width: 600px) {
  	body { 
    	background: green; 
  		}
	}
	
在上面代码中，屏幕宽度为600px以下的body的颜色是红色的。而在600px以上，则body的颜色是红色的。

如果是面向桌面设备，样式编写则首先是适配桌面设备，然后再是移动设备的适配。

这时候使用到是**max-width**这个媒体查询技术。

同样来看下下面的代码：

	body { 
  		background: green; 
	}

	// 这是适配屏幕宽度为600px及以下的样式
	@media (max-width: 600px) {
  	body { 
    	background: red; 
  	}
	}
	
上面代码中首先是指定了所有宽度设备的body的颜色，然后再指定移动设备的body的颜色。

### 编写css为什么要移动优先？

为桌面设备编写代码总是要比为移动设备编写代码复杂一些。

先看下面这两种不同的布局，在移动设备上内容区域的宽度是100%，而在桌面设备上的宽度则是66%。

![image](http://www.zell-weekeat.com/wp-content/uploads/2014/12/mw-5.png)

在这个时候，移动设备上我们可以使用默认的宽度属性。即**div**默认的宽度就是100%。

如果我们使用移动优先的css编写方法，那么样式代码如下所示(这里是用来Sass来编写样式的)

	.content {
  	// 直接使用默认的属性即可 

  	// 适配桌面设备的样式
  	@media (min-width: 800px) {
    	float: left; 
    	width: 60%; 
  		}
	}
	
如果我们采取的是面向桌面设备的样式编写方法，我们不得不重新设置元素的宽度，代码如下所示：

	.content {
  	// 桌面设备的样式
  	float: left; 
  	width: 60%; 

  	//适配移动设备的样式
  	@media (max-width: 800px) {
    	float: none; 
    	width: 100%; 
  	}
	}
	
从这个简单的例子，我们可以看到，如果是采取桌面设备的css编写方法，我们会多写两行代码。设想一下，如果是在编写一个大型的网站，我们会发挥更多的时间和精力。

大部分的场景，我们使用**min-width**足以。不过，同时使用**min-width**和**max-width**能使用我们更加灵活的来适配不同的设备。

让我们来看看具体一些场景。

### 使用Max-width来编写移动优先的css

当你要编写指定屏幕宽度尺寸一下的css的时候，可以使用**max-width**这个媒体查询。如果同时使用**min-width**和**max-width**能使用我们更加灵活的来适配不同的设备。

我们来看下面这个同时面向移动和桌面设备的相册的布局情形。

![image](http://www.zell-weekeat.com/wp-content/uploads/2014/12/mw-1.png)

如果每一个相片之间没有间隙，那编写css就简单了：
	
	.gallery__item {
  		float: left; 
  		width: 33.33%; 
  		@media (min-width: 800px) {
    	width: 25%; 
  		}
	}
	
如果布局改成如下图所示的情形：

![image](http://www.zell-weekeat.com/wp-content/uploads/2014/12/mw-2.png)

每一个相片之间的间隙是宽度的5%：
	
	 .gallery__item {
  		float: left; 
  		width: 30.333%;
  		margin-right 5%;
  		margin-bottom: 5%;
	  }
	  
当然，最后一个相片时不需要外边距的，这个使用伪类就可以很好的解决这个问题：

	.gallery__item {
  		float: left; 
  		width: 30.333%;
  		margin-right 5%;
  		margin-bottom: 5%;
  		&:nth-child(3n) {
    	margin-right: 0; 
  	}
	}
	
详细代码如下所示：

	.gallery__item {
  		float: left; 
  		width: 30.333%;
  		margin-right 5%;
  		margin-bottom: 5%;
  		&:nth-child(3n) {
    	margin-right: 0; 
  	}

  	@media (min-width: 800px) {
    	width: 21.25%; // (100% - 15%) / 4
    	&:nth-child (4n) {
      	margin-right: 0; 
    }
  	}
	}
	
最终表现如下图所示：
	
![image](http://www.zell-weekeat.com/wp-content/uploads/2014/12/mw-3.png)

这并不是我们想要的表现形式。因为我们指定了第三个相片的**margin-right**的值为0，而在大屏幕设备上就会出现如上图所示的情况。

我们可以调整下代码来修复这个问题，如下所示：

	.gallery__item {
  	// ...
  	@media (min-width: 800px) {
    // ...
    &:nth-child (3n) {
      margin-right: 5%; 
    }
    &:nth-child(4n) {
      margin-right: 0%;
    }
  	}
	}
	
这个方法不是很优雅，如果要适配一些更大的设备，那我们不得不不停的重复来设置**margin-right**的值。

代码编写要尽量坚持DRY原则。

一个更好的方法是使用**max-width**和**min-width**组合来更精细的适配不同宽度下的样式，如下所示：

	.gallery__item {
  		float: left; 
  		margin-right: 5%;
  		margin-bottom: 5%;
  	@media (max-width: 800px) {
    width: 30.333%;
    &:nth-child(3n) {
      margin-right: 0; 
    }  
  	}

  	@media (min-width: 800px) {
    width: 21.25%; // (100% - 15%) / 4
    &:nth-child (4n) {
      margin-right: 0; 
    }
  	}
	}
	
![image](http://www.zell-weekeat.com/wp-content/uploads/2014/12/mw-2.png)

使用了**max-width**就可以控制在800px以下宽度的样式不影响其它宽度的表现形式。

OK，现在设想一下，如果在一些更大的设备上一行显示5张相片，该怎么办呢？这个时候就可以组合**max-width**和**min-width**来适配啦：

	.gallery__item {
  		float: left; 
  		margin-right: 5%;
  		margin-bottom: 5%;
  	@media (max-width: 800px) {
    	width: 30.333%;

    &:nth-child(3n) {
      margin-right: 0; 
    }  
  	}

  	// min-width 和 max-width 
  	@media (min-width: 800px) and (max-width: 1200px) {
    width: 21.25%; // (100% - 15%) / 4
    &:nth-child (4n) {
      margin-right: 0; 
    }
  	}
  	
  	@media (min-width: 1200px){
    width: 16%; // (100% - 20%) / 5
    &:nth-child (5n) {
      margin-right: 0; 
    }
  	}
	}
	 　
![image](http://www.zell-weekeat.com/wp-content/uploads/2014/12/mw-4.png)

### 总结

**min-width**对于mobile－first的响应式网站的构建非常有用，而且能使我们的代码更加健壮。不过从上面的例子也可以看出，它也不能解决所有的问题。有时候，我们需要配合**max-width**来解决一些问题。总之，编写代码，要坚持DRY原则。

[原文地址](http://www.zell-weekeat.com/how-to-write-mobile-first-css/)
	
	




	
