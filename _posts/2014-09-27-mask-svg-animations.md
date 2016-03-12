---
layout: post
title: 使用SVG的剪裁蒙版和javascript制作动画效果
categories:
- Life
tags:
- svg

---

这篇文章来学习学习使用svg的蒙板和javascript来制作一个动画效果，在开始之前，建议先去看看我以前写的关于使用svg和css制作动画效果的文章这里是[地址](http://janily.gitcafe.com/life/2014/03/05/animating-svg-clipping-mask/)。

### 第一步：使用矢量编辑软件来制作图形

开始之前，我们需要使用软件来制作图形并导出为svg格式的代码。比如Adobe Illustrator或者是sketch等矢量设计软件都支持导出svg格式的功能。

### 第二步：格式化代码

由于要使用svg剪裁蒙板来实现动画效果，所以我们需要设计蒙板和图形的路径(path)，并导出为svg代码。如下所示：

	<?xml version=”1.0" encoding=”utf-8"?>
	<! — Generator: Adobe Illustrator 16.0.0, SVG Export Plug-In . SVG Version: 6.00 Build 0) →
	<!DOCTYPE svg PUBLIC “-//W3C//DTD SVG 1.1//EN” “http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
	<svg version=”1.1" xmlns=”http://www.w3.org/2000/svg" x=”0px” y=”0px” width=”100px” height=”100px” viewBox=”0 0 100 100">
    <path d=”M48.28,7.333v82.838c0,1.381–1.117,2.496–2.495,2.496H14.762c-1.379,0–2.499–1.115–2.499–2.496V7.333H25.64v72.029h9.268V7.333H48.28z M65.1,92.667V20.64h9.265v72.027h13.374V9.83c0–1.379–1.117–2.497–2.497–2.497H54.219c-1.379,0–2.497,1.118–2.497,2.497v82.837H65.1z”/>
    <path fill=”none” stroke=”#000000" stroke-width=”20" d=”M18.5,7c0,0,0,68.5,0,75.5s22.75,7,22.75,0C41.417,56.5,41.301,7,41.301,7"/>
    <path fill=”none” stroke=”#000000" stroke-width=”20" d=”M81.5,93c0,0,0–68.5,0–75.5s-22.91–7–22.91,0c-0.166,26,0.029,75.5,0.029,75.5"/>
	</svg>
	
你可能会注意到，其中有一个path的数据比其它两个path的数据要多很多。没错，第一个path就是作为蒙板来使用的。当然现在计算机还不知道哪个path是作为蒙板，以及要用来作为动画的path又是哪个？

在上面的svg代码中，我们来创建一个**&lt;defs&gt;**的标签。然后再用ID为**myClip**的蒙板标签**&lt;clipPath&gt;**把要作为蒙板的path包裹起来，这样蒙板在浏览器中将处于永远不可见的状态

最后，在需要制作动画效果中的path里添加**clip－path**的属性，并把URL的地址的值赋值为**url(#myClip)**，这样就可以把蒙板应用于path上。

调整后的代码如下：

	<svg version=”1.1" xmlns=”http://www.w3.org/2000/svg" x=”0px” y=”0px” width=”100px” height=”100px” viewBox=”0 0 100 100">
    <defs>
        <clipPath id=”myClip”>
            <path d=”M48.28,7.333v82.838c0,1.381–1.117,2.496–2.495,2.496H14.762c-1.379,0–2.499–1.115–2.499–2.496V7.333H25.64v72.029h9.268V7.333H48.28z M65.1,92.667V20.64h9.265v72.027h13.374V9.83c0–1.379–1.117–2.497–2.497–2.497H54.219c-1.379,0–2.497,1.118–2.497,2.497v82.837H65.1z”/>
        </clipPath>
    </defs>
    <path clip-path=”url(#myClip)” fill=”none” stroke=”#000000" stroke-width=”20" d=”M18.5,7c0,0,0,68.5,0,75.5s22.75,7,22.75,0C41.417,56.5,41.301,7,41.301,7"/>
    <path clip-path=”url(#myClip)” fill=”none” stroke=”#000000" stroke-width=”20" d=”M81.5,93c0,0,0–68.5,0–75.5s-22.91–7–22.91,0c-0.166,26,0.029,75.5,0.029,75.5"/>
	</svg>
	
现在你就可以把代码插入到你的html文件中了。

### 第三步：使用javascript来制作动画效果

这里我们会使用[Greensock TweenMax](http://greensock.com/gsap)这个javascript库来制作动画效果。制作动画效果的原理是利用了path的**stroke－dashffset**属性来制作的，关于这个也可以去看为以前写的文章[SVG动画原理初探](http://janily.gitcafe.com/life/2014/03/04/how-animation-svg/)。这里就不多阐述啦。

首先，是获取我们要制作动画效果的path，当然不包括**defs**标签里的path元素。

	var paths = $(‘path:not(defs path)’);
	
然后，设置每一个path的stroke－dasharray和stroke－dashoffset的值都等于path的长度。如果不是很明白的话，还是建议去看看我以前写的文章[SVG动画原理初探](http://janily.gitcafe.com/life/2014/03/04/how-animation-svg/)。

	paths.each(function(i, e) {
    e.style.strokeDasharray = e.style.strokeDashoffset = e.getTotalLength();
	});
	
需要创建一个Greensock timeline的对象来创建动画。

	var tl = new TimelineMax();
	
为了制作path的动画效果，需要把path的stroke－dashoffset的值设置为0。我们可以使用duration,delay,和easing等参数来使动画效果更加精致。

	tl.add([
    TweenLite.to(paths.eq(0), 1, {strokeDashoffset: 0, delay: 0.0}),
    TweenLite.to(paths.eq(1), 1, {strokeDashoffset: 0, delay: 0.5}),
	]);
	
最终效果如下：

<p data-height="331" data-theme-id="0" data-slug-hash="Ltvls" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/Ltvls/'>Ltvls</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

不过，一些简单的效果也可以使用css来制作。

参考文章：

[Masking SVG Animations](https://medium.com/@gordonnl/stylised-line-animations-ded23320ffe5)


