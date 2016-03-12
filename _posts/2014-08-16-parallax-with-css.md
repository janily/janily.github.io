---
layout: post
title: 使用css来实现视差滚动效果
categories:
- Life
tags:
- css
- 翻译
---

> 视差滚动效果在web开发设计中，已经流行蛮久啦。一般都要借助与javascript来实现，而且关于视差效果这方面的插件也越来越多。这篇文章我们就试着使用css3中的transform,perspective等属性来实现一个css版的视差滚动效果。[原文地址](http://blog.keithclark.co.uk/pure-css-parallax-websites/)。

现在视差滚动效果一般都需要借助于javascript来实现,但是由于需要频繁的监听滚动事件，以及修改dom节点而造成浏览器的**reflow**和**paint**，所以在性能上并不是很好。如果在执行滚动以及操纵dom的时候通过**requesAnimationFrame**来实现，那可以显著提高它的性能。试想以下，如果不需要任何的javascript来实现视差滚动，那岂不是更好？

使用css来实现滚动效果，上面提到的问题都会烟消云散。而且还可以使用css中的**media queries**或者是**support**来做响应式的视差滚动效果。

正所谓，光说不练假把式。先来感受下使用css打造的视差滚动效果吧。

[实际运行效果](http://blog.keithclark.co.uk/wp-content/uploads/2014/08/demos/3/)

### 实现原理

在分析原理之前，我们先来看看一个基本的html结构：

	<div class="parallax">
  	<div class="parallax__group">
    <div class="parallax__layer parallax__layer--back">
      ...
    </div>
    <div class="parallax__layer parallax__layer--base">
      ...
    </div>
  	</div>
  	<div class="parallax__group">
    ...
  	</div>
	</div>
	
下面是基本的样式：

	.parallax {
  		perspective: 1px;
  		height: 100vh;
  		overflow-x: hidden;
  		overflow-y: auto;
	}
	.parallax__layer {
  		position: absolute;
  		top: 0;
  		right: 0;
  		bottom: 0;
  		left: 0;
  	}
	.parallax__layer--base {
  		transform: translateZ(0);
	}
	.parallax__layer--back {
  		transform: translateZ(-1px);
	}
	
视差滚动效果主要依靠的就是**parallax**这个类来实现的。这里分别定义了元素的**height**和**perspective**这两个属性，定义**perspective**属性，我们就有了3d的视角。而高度我们使用了vh这个单位，他表示当前的视窗的高度，这样我们就不需要使用javascript来动态计算当前视窗的高度了。而**overflow－y:auto**则定义浏览器竖向自动出现滚动条。

**parallax__layer**这个层则是用来滚动的内容层，使用绝对定位来使它脱离文档流，并且宽高和当前的视窗相等。

而**parallax__layer--base**和**parallax__layer--back**则是用了translateZ的属性来定义元素滚动的视差效果，看下这个实例，这里我们只定义来两个层来滚动：

[实际例子](http://blog.keithclark.co.uk/wp-content/uploads/2014/08/demos/1/)

### 深入优化

我们使用3D的属性来创建视差滚动的效果，translateZ的功能就是让元素在自己的眼前或近或远也就是离整个浏览器整个视窗的远近。我们都知道近大远小的道理，元素在应用里translateZ的属性后，在视觉上会有大小变化的感觉。所以需要配合**scale**来使元素能本身呈现在屏幕上。

	.parallax__layer--back {
  		transform: translateZ(-1px) scale(2);
	}
	
那scale跟translateZ之间有什么关系呢？这里有一个简单的公式来计算scale的值**1 + (translateZ * -1) / perspective)**。比如，**perspective**的值是**1px**，而**translateZ**的值是**－2px**，那我们就可以计算出scale的值是**3**：

	.parallax__layer--deep {
  		transform: translateZ(-2px) scale(3);
	}
	
[实际运行例子](http://blog.keithclark.co.uk/wp-content/uploads/2014/08/demos/2/)

### 控制滚动速度

可以通过perspective和translateZ的值来控制内容滚动的速度。当把translateZ的值设置为负值的话，它在滚动的时候会比translateZ比它大的值的层滚动慢一些，比如**translateZ(-10px)**就会比**translateZ（－1px）**滚动慢一些。

### 创建视差滚动效果

在上面的几个部分，我们讲解来实现视差滚动一个基本原理。那如果要创建页面不同区域的不同的滚动效果，该怎么做呢？

首先，我们使用**parallax__group**来组织我们需要创建不同滚动效果的html：

	<div class="parallax">
  	<div class="parallax__group">
    	<div class="parallax__layer parallax__layer--back">
      		...
    	</div>
    	<div class="parallax__layer parallax__layer--base">
      		...
    	</div>
  	</div>
  	<div class="parallax__group">
    	...
  	</div>
	</div>
	
下面是css内容：

	.parallax__group {
  		position: relative;
  		height: 100vh;
  		transform-style: preserve-3d;
	}
	
在这个实例中，为想要每一个滚动的区域都填满整个浏览器视窗所以这里使用来**vh**这个单位来作为高度单位**height:100vh**。**transform－style:preserve-3d**是为来实现3d透视的效果，模拟真实世界中的思维认识。而**parallax－layer**元素设置为相对定位是为了子元素使用绝对定位来布局用的。

还有一点需要注意的是，我们不能给**parallax__group**设置**overflow:hidden**属性，因为这样它会剪裁在当前视窗内容以外的内容，那我们的滚动效果就无从做起了。如果不剪裁溢出的内容，它们又会重叠在一起。所以我们需要设置它们的**z-index**的值，让它们按顺序排列滚动的时候才会按顺序依次出现。

这里层与层之间并没有一个统一的设计原则。如果你明白这个滚动效果的原理的话，你可以添加下面这条规则来更加清楚的知道并观察它实际运行的效果：

	.parallax__group {
    transform: translate3d(700px, 0, -800px) rotateY(30deg);
	}
	
点击下面这个例子看最终的运行效果，注意左上角的**debug**这个按钮，点击它看看，你会有惊喜发现。

[最终效果](http://blog.keithclark.co.uk/wp-content/uploads/2014/08/demos/3/)

### 浏览器支持

* 新版本的Firefox、Safari、Opera和Chrome都支持这个效果。
* Firefox虽然也支持，但是也还是有一点点问题。
* IE不支持**preserve-3d**属性，也就是说不支持视差滚动效果。不过可以使用渐进增强的开发理念来对待它。





	




