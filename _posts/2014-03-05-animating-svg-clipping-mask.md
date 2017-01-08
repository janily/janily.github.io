---
layout: post
title: 	使用SVG的剪裁蒙版和CSS制作精彩的动画
categories:
- Life
tags:
- svg
- 翻译
---

> 继续SVG的精彩，原文地址[Animating SVG With Clipping Masks](http://www.pencilscoop.com/2014/02/animating-svg-with-clipping-masks-and-css/)，具体翻译有删减。

SVG动画在web越来越流行，也出现了很多具有非凡想象力的动画效果。在过去的的一段时间我们会发现也出现了很多的关于使用[jQuery](http://tympanus.net/codrops/2013/12/30/svg-drawing-animation/)、[Snap.svg](http://tympanus.net/codrops/2013/11/05/animated-svg-icons-with-snap-svg/)、以及配合[CSS](http://www.pencilscoop.com/2013/11/animate-svg-icons-with-css3-jquery/)来制作SVG动画效果。

不过如果仅仅是使用CSS来制作SVG动画效果的话还是有很大的不足。你不能很灵活的来操作SVG的路径，stroke或者是使用fill属性来制作动画效果，不过要是使用其它的第三方的[javascript库](http://snapsvg.io/)的话，就可以任意操作SVG的任意属性来制作动画效果。比如[操作SVG的path来制作动画效果](http://css-tricks.com/svg-line-animation-works/)。

虽然使用CSS来制作SVG动画没有用javascript来的强大。但是也能制作出很漂亮的动画效果，这篇文章就来学习使用SVG的剪裁蒙版的属性来制作一个令人惊艳的动画效果。

开始之前可以去[这个地址](http://www.pencilscoop.com/demos/animating-svg-clipping-masks)看看实际效果。或者是下载实例的[源代码](http://www.pencilscoop.com/demos/animating-svg-clipping-masks/source/animating-svg-clipping-masks.zip)。

不过在这篇文章里我不会详细的讲解这个实例的制作过程，但我会解释制作这个效果的原理。

### **什么是剪裁蒙版？** ###

啥叫剪裁蒙版呢？如果你英文不错的话可以去这个[地址看看](http://helpx.adobe.com/illustrator/using/clipping-masks.html)关于剪裁蒙版的信息。所谓剪裁蒙版按照我的理解就是裁剪蒙版是由path, text或者基本图形组成的图形。所有在裁剪路径内的图形都可见，所有在裁剪路径外的图形都不可见。其实跟photoshop中的蒙版的原理差不多。下面我们用一个例子来说明下剪裁蒙版的工作原理。

使用adobe的Illustrator新建一个圆形，由于我电脑上只装了photoshop，所以我这里就用它来实现跟原文中使用Illustrator制作的效果。使用photoshop新建一个圆形填充为白色并且复制一层填充为蓝色，然后右键点击蓝色的图层新建新建一个剪贴蒙版如下所示：

![](http://pic.yupoo.com/reicky_v/DAehytSr/KKASL.jpg)

现在如果你移动剪贴蒙版的话，这个蓝色的圆圈就会被剪切。如下图所示：

![](http://pic.yupoo.com/reicky_v/DAekdTUN/S3KpD.jpg)

那如果把这个原理应用到SVG上面我们配合CSS的animation我们就可以制作令人印象深刻的动画效果了。

当然如果是准备使用SVG来制作动画效果的话，最好是用矢量图形设计软件来设计制作图形，如adobe的Illustrator或者是Inkscape来设计制作图形，可以直接导出SVG格式的代码，如下所示：

    <svg>

	<defs>
	<path id="ID_OF_THE_ACTUAL_PATH">
	</defs>
	
	<clipPath id="CLIPPING_MASK" >
	<use xlink:href="#ID_OF_THE_ACTUAL_CLIPPING_PATH" />
	</clipPath>
	
	<path id="pink-circle" style="clip-path:url(#CLIPPING_MASK)" />
	
	</svg>

上面的代码我们把剪切蒙版的路径定义在**defs**标签里面，<defs>标签是 definitions的缩写，它允许对诸如渐变、剪切蒙版等特殊元素进行定义。我们使用**clip-path**定义了一个圆形的剪切路径即我们在上面定义在defs中的路径。然后将改剪切路径应用到一个id为pink-circle的圆形中。如果还是没有明白的话可以去[这个地址](http://msdn.microsoft.com/zh-cn/zh/library/ie/bg124134(v=vs.85).aspx)看看。其实如果你对photoshop比较熟悉的话，剪切蒙版的原理还是很容易理解的。

编写以下SVG代码：

    <svg version="1.1" id="svg-circle" width="150px" height="150px">

	<defs>
	<circle id="clipmaskpath" cx="72.375" cy="76.625" r="45.875"/>
	</defs>
	
	<clipPath id="clipmask">
	<use xlink:href="#clipmaskpath" style="overflow:visible;"/>
	</clipPath>
	
	<circle style="clip-path:url(#clipmask)" cx="72.375" cy="76.625" r="45.875"/>
	
	</svg>

如果要制作动画效果的话，只需要让剪切路径运动起来就可以了，至于运动方式完全可以使用CSS3中的transform灵活控制，如下所示：

    #svg-circle #clipmask {
	animation: move-mask infinite running 2s ease;
	}
	@keyframes move-mask {
	0%, 100% {transform: translateY(-90px)}
	50% {transform: translateY(0px)}
	}

最终效果如下：

<p data-height="268" data-theme-id="0" data-slug-hash="Fywop" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/Fywop'>SVG Clipping Mask</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

这只是一个简单的示例，你可以使用不同的[transforms属性](http://css-tricks.com/almanac/properties/t/transform/)来制作不同的动画效果。

### **其它问题** ###

按常理来说，SVG剪切路径制作的动画效果应该能在所有现代浏览器能正常工作。不过实际检测发现仅仅在webkit内核的浏览器才能正常工作。经过测试在火狐浏览器中虽然支持剪切蒙版的渲染，但是不支持CSS的动画效果。希望火狐能尽早支持解决这个问题。如果你想解决跨浏览器兼容的问题，可以选择使用透明度来解决其它浏览器不支持剪切蒙版的情形。

