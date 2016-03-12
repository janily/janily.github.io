---
layout: post
title: 使用Velocity.js来改善和提高我们编写UI动画的流程
categories:
- Life
tags:
- javascript
- 翻译
---

> 虽然现在css3中提供的动画也非常强大来，但是有时候为来更加精细的控制动画效果，一般我们会结合css3和javascript一起来制作动画效果。当然一些简单的效果，我们可以使用jQuery提供的**animate()**方法来编写动画效果。其实归根到底就是性能的问题，到底是使用css更快些呢？还是使用javascript编写动画性能更好些呢？就这个David Walsh专门写了[一篇文章](http://davidwalsh.name/css-js-animation)来比较两者上的性能，文章中的js动画部分就是使用了[Velocity.js](http://velocityjs.org)来编写的，可以去看看这片文章。今天这篇文章就来学习使用Velocity.js来代替jQuery的动画方法来编写ui动画。原文地址[Improving UI Animation Workflow with Velocity.js](http://css-tricks.com/improving-ui-animation-workflow-velocity-js/)。

[Velocity.js](http://velocityjs.org)是一个jQuery的插件，用来代替jQuery默认的**$.animate()方法**来编写ui动画。它能够显著的提高动画的性能中一些情况下甚至比css的transitions的效果还要好。

经过压缩后Velocity只有7kb并且包括默认**$,animate()**方法的所有特性。总而言之，Velocity就是为来使编写ui动画更加简单性能更加好而出现的。

Velocity兼容性也做的很好－即使使ie8和Android2.3也支持。而且它的语法跟**$.animate()**也类似，完全可以无缝转移到Velocity上面来。

Velocity到设计目标就是更好提升dom动画到性能以及使用简洁到方法编写ui动画。这个文章稍后会讲到。可以去Velocity到官网看看详细到性能比较。在这篇文章中我们将学会使用Velocity来来改善和提高我们编写UI动画的流程。当然我们会把它跟使用jQuery来编写动画来做一个比较。

### 概述

在开始Velocity之前，先来快速的过一下一些Velocity的基本知识，我们会使用**$.aniamte()**和**$.velocity()**来做对比学习它们语法上的异同。

下面我们来看一个jQuery动画的例子，这个动画效果包含这样一些参数，动画运动的类型、运动持续的时间以及动画完成后的回调函数：

	$div.animate(
  	{ 
    	opacity: 1 
  	}, 
  		1000, 
  		"linear", 
  		function() { 
    	alert("Done animating."); 
  		}
	);
	
下面是已对象语法的形式来改写上面的代码：

	$div.animate(
  	{ 
    	opacity: 1 
  	}, 
  	{ 
    	duration: 1000, 
    	easing: "linear", 
    	complete: function() { 
      	alert("Done animating!") 
    }, 
    	queue: "myQueue" 
  	}
	);

使用对象形式的语法来编写动画效果，可以使我们的代码简洁易读，还能通过传递更多的参数来更好的控制动画效果。而使用字符串语法形式编写动画效果使做不到这些的。

jQuery提供来队列这一属性来控制动画效果。Velocity当然也支持队列控制，当一个元素上有多个动画效果当时候，Velocity会自动控制它们一个接一个运行。

下面的效果是div这个元素的透明度会从1变为0，持续到时间都是1000ms：

	$div
  	.animate({ opacity: 1 }, 1000)
  	.animate({ opacity: 0 }, 1000);
  	
OK，简单的回顾来一些基本的知识，下面我们通过比较jQuery和Velocity来学些Velocity。

在Velocity中有一个**reverse**的参数。当把reverse传递给velocity的时候，velocity会自动中执行完动画后把元素运动的属性值设置为运动之前的初始值。

比如在jQuery中，我们可能会这样做：

	$div
  	/* 这个效果主要是加载完元素后隐藏元素 */
  	.animate({ opacity: 1, top: "50%" })
  	.animate({ opacity: 0, top: "-25%" });
  	
而在velocity中，要做到上面的效果可以更简单：

	$div
		.velocity({opacity:1,top:"50%"})
		.velocity("reverse");
		
其实中使用velocity的时候我们可以传递参数，改变一些动画执行的条件：

	$div
		.velocity({opacity:1,top:50%},1000)
		.velocity("reverse",500);
		
### Scrolling

在ui动画中，滚动是一个非常重要的技术。像现在流行的视差动画，就是利用滚动技术来实现的。

在jQuery中，**scrollTop**这个方法需要以**html**和**body**这两个元素为计算基准点，是因为为来兼容老版本的IE浏览器。

	$("html, body").animate(
  	{ 
    	scrollTop: $div.offset().top 
  	}, 
  		1000, 
  	function() {
    	/* We use a callback to fade in the div once the browser has completed scrolling. */
    	$div.animate({ opacity: 1 });
  	}
	);
	
而这velocity中，可以更简单：

	$div
  		.velocity("scroll", 1000)
  		.velocity({ opacity: 1 });
  		
只要浏览器以滚动就会触发我们定义好的方法。

### 循环

有时候，我们可能需要循环的执行动画。比如，为来提醒用户我们可能会循环执行一些动画效果。

中jQuery中，我们可能会用到for循环来达到目的：

	for (var i = 0; i < 5; i++) {
  	$div
    /* Slide the element up by 100px. */
    .animate({ top: -100 })
    /* Then animate back to the original value. */
    .animate({ top: 0 });
	}
	
而在velocity中，能够利用非常简洁的语法达到循环的目的：

	$div.velocity(
  	{ top: -100 }, 
  	{ loop: 5 }
	);
	
### 隐藏元素

要隐藏一个元素，我们可以把display属性的值设为**none**就可以了。我们来看下面这段代码：

	$div
  	.show()
    .css("opacity", 0)
    .animate({ 
    opacity: 1, 
    top: "50%" 
  });
  
上面的代码主要是使用来jQuery中内置的一些方法来控制元素的显示和隐藏。在velocity中，我们可以使用display(display的值可以为none，block或者是inline。)作为参数传递给velocity，同样也可以达到效果。

	$div.velocity(
  	{ 
    	opacity: 1, 
    	top: "50%" 
  	},
  	{ 
    	display: "block" 
  	}
	);
	
### Delaying

我们来看看velocity和jQuery使用延迟执行动画的语法。

	$div
  		.delay(1000)
  		.animate({ 
    	opacity: 1 
  	});	
  	
  	$div.velocity(
  	{ 
    	opacity: 1 
  	}, 
  	{ 
    	delay: 1000 
  	}
	);
	
在velocity中，会自动使用缓存来优化动画的性能。

### 类动画

为了避免在javascript代码中夹杂css代码，一般推荐用css和javascript的形式来编写动画效果，即动画效果使用css来控制，而动画的执行则用javascript来触发。比如我们会把要执行的动画效果的代码编写到指定的css类中。当要触发这个效果当时候，只需要把这个类添加到指定到元素上就可以了。

需要注意的是，在jQuery中要执行这样的类动画效果，需要用到[jQuery UI](https://jqueryui.com/addClass/)这个库。而这个库可有62kb这么大并且还是经过压缩的。而使用velocity的画则不需要加载这个jQuery UI库。

CSS

	.animate_slideIn {
  		opacity: 1;
  		top: 50%;
	}
	
jQuery UI

	$div.addClass("animate_slideIn", 1000);
	
velocity.js

	$div.velocity("slideIn", 1000);
	
在velocity中，使用css类来编写动画效果的时候，这个类需要有**animate_**这个前缀，不过在使用velocity调用的时候可以不加这个前缀。

为了提高动画的性能，velocity并不是直接把类作用于目标元素，而是把它作为属性的映射容器。

### 利用硬件加速提高动画性能

在移动设备上，利用设备本身的硬件资源能够显著的提高动画的性能。要使用硬件加速我们只需要把元素的transform的值设置为**translateZ(0)**。

	$div
  	.css("transform", "translateZ(0)")
  	.animate({ opacity: 1 })
  	.css("transform", "none");
  	
在velocity中，会自动为移动设备开启硬件加速(在桌面设备上没有多少提升。)。

	$div.velocity({ opacity: 1 });
	
### 总结

这篇文章只是为你提供另外一种更加友好、优雅编写UI动画的方法。

jQuery是非常强大，不过jQuery当初的设计目标中从来没有把高性能的动画引擎作为它的设计目标。压缩后只有7kb的velocity不失为一个好的动画引擎的选择。

有关velocity更加详细的信息可以去它[官方的网站](velocity.org)查看相关文档。

在结束本文前，这里提供一些使用velocity编写动画的实例给您参考：

[3d demo](http://julian.com/research/velocity/demo.html)

[virtual world demo](http://danielraftery.com/read/Animating-Awesomeness-with-Velocityjs)

[HUD demo](http://zachsaucier.com/fp.html)

[multimedia demo](http://julian.com/research/velocity/playground.html)



