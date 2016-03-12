---
layout: post
title: 矢量图形与动画
categories:
- Life
tags:
- svg
- 翻译
---

> [24ways](http://24ways.org/)这个聚集前端技术知识的网站跟其它网站相比有一点另类，它只会在一年的最后一个月更新文章，文章类型大部分是关于前端技术类的而且质量都非常不错。这不2013刚过去， [24ways](http://24ways.org/)就开始了对于2013年出现的一些技术的讨论盘点。今天看到一篇关于SVG动画方面的文章非常不错，以前也写过一些svg动画方面的文章。但是这篇文章也提供了关于实现SVG动画的其它的一些思路，就翻译过来了。原文地址[Animating Vectors with SVG](http://24ways.org/2013/animating-vectors-with-svg/)，具体翻译内容有删减。

在2013年以及可以预见的2014年，SVG这种已经存在了差不多15年的技术焕发出了新的生命力。先看看下面这个实例：

<p data-height="268" data-theme-id="0" data-slug-hash="jHxvG" data-user="janily" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/jHxvG'>24 ways SVG Animation</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a></p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

或者是去这个页面看看[demo](http://media.24ways.org/2013/suda/animate.html)。

SVG不同于位图，它能够适应任何的分辨率的屏幕而保证图片的质量不会失真。而在配备视网膜高清屏的智能手机、平板电脑等移动设备上，位图就会产生严重的失真的情况。一般不外乎两种解决方案，一种是根据各种不同分辨率的屏幕设计制作不同大小的图片来适配；还有一种是采用SVG格式的图片方案，一张足以。

我强烈推荐SVG。SVG是一种XML格式的文档，这意味着可以很容易用脚本来控制它。而创建SVG也很方便，我们可以使用像IIIustrator，inkscape或者是Sketch这类软件来设计制作SVG图片。

这就会产生一个疑问，SVG与位图相比有如此多的有时，为什么在SVG标准已经诞生十多年的情况下，会默默无闻呢？

其实原因也很简单，就是在以前的浏览器中，对它的支持不是很好特别在过去浏览器还是IE一家独大的情况下更是如此，所以没有人愿意去了解和使用SVG。SVG一直受制于浏览器，不过现在不用太担心这个问题啦。[现在大部分浏览器都支持SVG](http://caniuse.com/#search=svg)，不过IE还是落后一步要到IE9才支持SVG.

尽管大部分的浏览器都支持SVG，但是各个浏览器的实现方式还是有一些差异的。

### 在HTML中应用SVG ###

一些浏览器可以允许只用用**svg**标签在HTML文件中直接插入SVG图片。就像其它HTML元素一样在HTML中，SVG是一个一等公民。也可以使用**&lt;img&gt;**标签插入SVG图片。不过在使用的时候，还是要预留一个回退方案来支持一些不支持SVG的浏览器能正常运行。一般这样的解决方案是使用**&lt;object&gt;**对象的方法来使用SVG格式图片。当浏览器不支持SVG的时候，我们可以在对象中使用**&lt;img&gt;**标签引入一张位图来正常显示图片。这个方法是一个优雅降级的解决方案，在支持SVG的浏览器中使用SVG图片；不支持SVG图片的浏览器中使用位图。不过有一个问题是，有一些浏览器可能会同时下载SVG格式和位图图片，在性能上可能有一点小的问题。

Alexey Ten提出了一个[非常聪明的使用SVG方法](http://lynn.ru/examples/svg/en.html)，使用SVG内联标签**&lt;image&gt;**元素来引入图片。这个标签有一个**href**属性，我们可以同时外链一张SVG图片和位图图片，这样当用户的浏览器支持SVG图片，则引入SVG图片；不支持的时候，则引入位图图片。如下所示：

    <svg width="96" height="96">
    	<image xlink:href="svg.svg" src="svg.png" width="96" height="96"/>
	</svg>

这个方案在一些场景下确实是非常棒。至于哪个方案适合你，那就要看你的具体场景了。

### 是时候使用SVG啦 ###

现在在WEB开发中一般都推荐用SVG来设计一些标示和图标。这是因为随着各种高清分辨率屏幕的出现，比如72dpi，200、300甚至是400dpi屏幕的也已经和普及了，而SVG图片能适应任意分辨率的而且不会影响质量。

另外一个原因是矢量图形对于响应式网站来说也是一个图形设计标准，在响应式设计中我们一般需要动态的调整一些图片的宽高。SVG在这种需求的的场景下的优势就凸显了。

而且SVG也是有类似DOM节点构成的，所以非常容易用gzipped的压缩技术把它的体积压缩到很小。利用SVG来制作图片精灵也能显著的提高web性能。其实这还不足以凸显SVG优势，用javascript来操纵SVG，才是SVG闪光之处。因为SVG也是DOM节点的一部分，所以很容易用javascript来操纵它们。

比如[Playstation](http://www.polygon.com/a/ps4-review)和[Xbox One](http://www.polygon.com/a/xbox-one-review)这两个网站就用SVG和javascript设计了两个SVG动画。可以去看看[ Vox Media 这篇文章](http://product.voxmedia.com/post/68085482982/polygon-feature-design-svg-animations-for-fun-and)它就是探讨这两个动画的。

Vox Media在文章提到了创建SVG动画的另一种思路。在它们制作两个动画中，主要是利用了SVG中line属性的**dash**来用动画的方式逐渐增加它的百分比，这样就会形成一个从起点开始绘制线条到闭合线条的运动过程。

当然像这样的动画，完成它还是要花费一点时间的。用GIF来实现也是一个选择，但是要完成这样复杂的动画的话，由于帧数过多可能会使GIF的体积变得很大，可能也会影响到动画的流畅性；也可以使用**CANVAS**和javascript来实现，但是使用CANVAS绘制的其实也是位图格式的图形，当要适应不同分辨率图片的时候，需要不断地重绘图形，这样也会对整个动画的流畅性造成影响。

如果用HTML、SVG和javascript来制作动画的话，javascript代码的体积会小于4kb，让我们来看看代码吧：

    <script>
	var current_frame = 0;
	var total_frames = 60;
	var path = new Array();
	var length = new Array();
	for(var i=0; i<4;i++){
		path[i] = document.getElementById('i'+i);
		l = path[i].getTotalLength();
		length[i] = l;
		path[i].style.strokeDasharray = l + ' ' + l; 
		path[i].style.strokeDashoffset = l;
	}
	var handle = 0;
	
	var draw = function() {
	   var progress = current_frame/total_frames;
	   if (progress > 1) {
	     window.cancelAnimationFrame(handle);
	   } else {
	     current_frame++;
	     for(var j=0; j<path.length;j++){
		     path[j].style.strokeDashoffset = Math.floor(length[j] * (1 - progress));
	     }
	     handle = window.requestAnimationFrame(draw);
	   }
	};
	draw();
	</script>

在上面的代码中，我们首先需要初始化一些变量分别是帧数和运行帧数的总数以及动画运行的时间，然后我们得到path的ID，来操纵路径的dash和dash offset属性。

    path[i].style.strokeDasharray = l + ' ' + l; 
	path[i].style.strokeDashoffset = l;

然后我们再执行**draw()**这个方法。接下来就是见证奇迹的时刻。首先说一下**Stroke-dasharray**和**Stroke-dashoffset**这两个东东，Stroke-dasharray:设定若干实线和间隔的长度，Stroke-dashoffset:设定实线的起始偏移。主要是来操纵strokeDashoffset的值，我们在**draw**方法里定义一个控制动画的进程，它的值主要是由**current_frame**的值来决定的，如果**process**的值大于1则停止动画；否则就不断地增加**current_frame**的值直至process的值大于一才停止动画。requestAnimationFrame是js中新增的一个函数，专门为创建动画网页提供了一种更平滑更高效的方法。告诉浏览器,你想要执行一个动画,该请求要求浏览器提前安排一下下一帧动画显示时需要进行的浏览器窗口的重绘。

就是这么简单！你可以把它运用到SVG中的**&lt;path&gt;**上来制作动画效果。

### 推荐一些第三方的库 ###

如果你对SVG不是很熟悉，这儿推荐一些第三方的用来绘制SVG的库，使用它们我们可以使你更好地控制和绘制SVG以及更好地管理SVG。

- [Raphael](http://raphaeljs.com/)
- [Snap.svg](http://snapsvg.io/)
- [svg.js](http://www.svgjs.com/)

    



