---
layout: post
title: 	SVG动画原理初探
categories:
- Life
tags:
- svg
- 翻译
---

> 前面也说过一些关于SVG在web中的应用，比如字体图标和SVG动画。特别是现在随着浏览器的功能越来越强大，SVG在web中的应用也越来越广泛，尤其是在网页的交互方面SVG更是大大拓展了交互动画应用领域，当然作为一个开发者来说更重要的是知其然，更要知其所以然。弄清SVG制作动画的原理更能使我们在制作SVG动画得心应手，正好在[css-tricks](css-tricks.com)网站上看到一篇关于SVG动画原理的文章，赶紧着翻译过来学习学习加深对SVG动画的理解。原文地址[How SVG Line Animation Works](http://css-tricks.com/svg-line-animation-works/)。具体翻译结合自己的理解，有所删减。

相信大伙在网页上都有看到过使用SVG制作路径动画效果。互联网上也有很多关于制作SVG动画这方面的探索，比如Jake Archibald的这篇文章[a super good interactive blog post](http://jakearchibald.com/2013/animated-line-drawing-svg/)。Brian Suda的[这篇](http://24ways.org/2013/animating-vectors-with-svg/)。还有[SVG多边形的应用](http://product.voxmedia.com/post/68085482982/polygon-feature-design-svg-animations-for-fun-and)。Codrops关于[SVG](http://tympanus.net/Development/SVGDrawingAnimation/)也有很多的探索看过之后令人印象深刻。

下面让我们一步步来了解SVG的动画效果是怎么样一回事？

### **SVG图片** ###

首先你得有一张SVG图片。

![](http://pic.yupoo.com/reicky_v/DA2SriHp/dwyya.png)

### **SVG的路径** ###

然后使用编辑器或者是图形设计工具来打开SVG图片，你可以看到SVG图片的具体代码，一般都是由路径构成的并且有**stroke**这个属性。如下图所示：

![](http://pic.yupoo.com/reicky_v/DA2UC6Jp/W5gNC.png)

### **CSS与Stroke** ###

SVG是一种用来描述二维矢量图形的XML标记语言。跟HTML一样也是一种文本节点的文档也可以使用CSS来定义相关标签节点的属性。比如SVG中的**stroke**属性跟HTML中的定义元素边框**border**属性差不多。

    <svg ...>
	  <path class="path" stroke="#000000" ... >
	</svg>

CSS:

    .path {
		stroke-dasharray:20;
	}

(译者注：stroke-dasharray属性的参数，是一组用逗号分割的数字组成的序列。需要注意的是，这里的数字必须用逗号分开。每一组数组，第一个用来表示虚线，第二个用来表示空白。比如**stroke-dasharray="5,5"**则表示先画5px实现，紧接着画5px的空白，从而形成虚线。再比如**stroke-dasharray="5,10,5"**定义了3个数字，这种情况下，数字会循环两次，形成一个偶数的虚线模式。所以改路径首先是5px实线，然后10px空白，然后5px实线，接下来循环这组数字，形成5px空白、10px实线、5px空白。然后这种模式会继续循环。)

回到上面的代码，把SVG中的路径定义为一段由20px实线和20px空白构成的虚线闭合的路径。如下图所示：

![](http://pic.yupoo.com/reicky_v/DA3NCUan/RTPAP.png)

至于虚线数量，我们可以定义更大数量的虚线：

    .path {
		stroke-dasharray:100;
	}

![](http://pic.yupoo.com/reicky_v/DA3Ot1BH/dbsxB.png)

### **让路径动起来** ###

SVG 我们还可以使用**stroke-dashoffset**来定义虚线开始的位置。所以我们可以用CSS3中的animation来移动虚线的位置从而制作动画效果。如下图所示：

![](http://pic.yupoo.com/reicky_v/DA3XZuvR/x3T08.gif)

具体代码如下：

    .path {
	  stroke-dasharray: 100;
	  animation: dash 5s linear;
	}
	
	@keyframes dash {
	  to {
	    stroke-dashoffset: 1000;
	  }
	}

我们来看看如何利用**stroke-dasharray**和**stroke-dashoffset**这两个属性配合animation使它动起来。首先想象一下我们把**stroke-dasharray**的数值设置的足够大，那实际在网页上呈现的图形就像一个没有完成的图像一样。比如这样：

![](http://pic.yupoo.com/reicky_v/DA3Ot1BH/dbsxB.png)

把**stroke-dasharray**的值设置足够大以后，然后利用**stroke-offset**来移动虚线的位置，数值从大依次递减直至为0，这样虚线就会就会从不完整的状态直至还原成本来的形状。如下所示：

<p data-height="268" data-theme-id="0" data-slug-hash="DwAly" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/DwAly'>DwAly</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>


在CSS中，我们把animation的动画模式设置为**forwards**，表示结束后保持动画结束时的状态。如下代码所示：

    .path {
	  stroke-dasharray: 1000;
	  stroke-dashoffset: 1000;
	  animation: dash 5s linear forwards;
	}
	
	@keyframes dash {
	  to {
	    stroke-dashoffset: 0;
	  }
	}

上面大概就是大多数SVG动画的一个原理。

### **JavaScript** ###

一般我们看到的大部分的SVG动画都是使用SVG和javascript一起来制作的。如果不使用javascript的话，我们无法准确知道SVG形状实际的长度，在上面的例子中我们设置**1000**也仅仅是这个数字能达到我们想要的效果。

如果使用javascript的话，我们就可以使用下面代码来获得SVG路径的准确长度。

    var path = document.querySelector('.path');
	var length = path.getTotalLength();

得到它之后我们就可以使用它来进行相关的操作了。可以去文章开头列举的那些文章进一步了解SVG动画精彩使用实例，通过这篇文章理解SVG动画原理之后，再去深入理解制作SVG动画就轻松多了。







