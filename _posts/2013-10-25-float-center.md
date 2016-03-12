---
layout: post
title: 用伪元素实现元素居中的布局效果
categories:
- Life
tags:
- css
---

> 这篇文章主要是来自[css-tricksl](http://css-tricks.com/)网站的[http://css-tricks.com/float-center/](http://css-tricks.com/float-center/)。具体内容有删减，添加了一些自己的理解。

首先让我们看看下面这一种布局效果：

![](http://pic.yupoo.com/reicky_v/Dgjfo2hq/medium.jpg)

这样的布局合理么，有必要么？但是当你的文章是关于猫，那么这样的布局显然是非常合理的，而且也是有必要的。

像这样的布局还是要花点时间才能实现的。我们现在所用到的CSS中的布局方法，并没提供实现这样布局的方法。可能在CSS标准制定者的心目中，并没有从一个网站设计者的角度来审视标准，不知道我说的有道理么？在现有的CSS中的布局方法中，我是没有找到很好的方法来实现这样的布局。这有点像用*float*实现文字围绕图片这种布局的情形，但是像上面这种布局，可以用浮动来实现么？

当然可以，下面的是最终的效果的，可以点击进去看看：

[最终效果](http://css-tricks.com/examples/FloatBoth/) [源代码下载地址](http://css-tricks.com/examples/FloatBoth.zip)

这里主要的原理是，用浮动元素的伪元素做一个占位元素。我们把文字区域放到两个布局DIV中，一个向左浮动，另一个向右浮动。而伪元素的的宽高要和你的图片的宽高相等。

如下所示：

    #l:before, #r:before { 
	  content: ""; 
	  width: 125px; 
	  height: 250px; 
	}
	#l:before { 
	  float: right; 
	}
	#r:before { 
	  float: left; 
	}

![](http://pic.yupoo.com/reicky_v/DgjpqlMP/ZEjxE.jpg)

现在，文字就能围绕图片，并且图片在文字中间这样的布局。当然，我们这里是用绝对定位把图片定位在中间的，你也可以使用负边距来达到这样的效果。