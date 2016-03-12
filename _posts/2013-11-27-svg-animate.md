---
layout: post
title: 用SVG配合javascript动态的来绘图
categories:
- Life
tags:
- svg
- 翻译
---

> 在WEB端，可供选择的图形绘制技术并不多。以前一般都是用Flash来解决这方面的问题的。不过现在随着web技术的发展，web绘图技术也不再是Flash一家独大了，比如现在火热的CANVAS和慢热的SVG这两种绘图技术，已经在web上应用越来越广泛了。特备是在一些创意领域以及广告媒体领域，更是有取代Flash的趋势。现在打算翻译一些国外优秀的关于SVG应用的文章。至于基础方面的知识推荐去这个网站看看，[xbingoz](http://xbingoz.com/tag/svg)。这第一篇问斩来自于[jakearchibald](http://jakearchibald.com/)的[Animated line drawing in SVG](http://jakearchibald.com/2013/animated-line-drawing-svg/)。具体有一些删减。

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/W4s8E/embedded/result" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

我喜欢用图片的形式来展示信息和浏览器的行为，但是要制作一张大的图片确实让人望而生畏。我在这两片关于性能的文章里[ the Application Cache](http://www.dailymotion.com/video/xwsc3y_application-cache-by-jake-archibald_tech)和[ rendering performance ](http://vimeo.com/69385032)有说到关于图片对于性能的影响，我用到了SVG这样一种绘图格式：

### SVG中的路径 ###

路径path可能是SVG中最通用的一种形状，通过path元素，我们可以创建矩形（有没有圆角都行）、圆形、椭圆形、折线、多边形，以及其他一些形状，比如二次贝塞尔曲线、三次贝塞尔曲线，等等。总之path很强大也很复杂。

具体的关于PATH的一些细节知识可以去我上面的网站去了解。我们这里先看一个简单关于PATH的实例:

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/tBfLk/embedded/result,js,css,html" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

我一般使用Inkscape这个开源软件来制作SVG图形。虽然使用起来略显一点笨重，但是它能按照你设计的SVG图形，生成SVG DOM文件给你。能够导入Adobe生成的SVG格式文件。

path元素的形状是通过d属性定义的，d属性的值是一个“命令+参数”的序列，我们将学习这些命令。

每一个命令都用一个关键字母来表示，比如，字母“M”表示的是“Move to”命令，当解析器读到这个命令时，它就知道你是打算移动到某个点。跟在命令字母后面的，是你需要移动到的那个点的x和y轴坐标。比如移动到(10,10)这个点的命令，应该写成“M 10 10”。这一段字符结束后，解析器就会去读下一段命令。每一个命令都有两种表示方式，一种是用大写字母，表示采用绝对定位。另一种是用小写字母，表示采用相对定位（例如：从上一个点开始，向上移动10px，向左移动7px）。

d属性采用的是用户坐标系统，不需标明单位。

那用path怎么来制作动画效果呢？看起来好像有点复杂。其实不然，我们可以变通一下来做动画效果。我们可以通过控制路径的颜色和宽度以及strokedasharray属性创建虚线来制作动画效果（在svg中的stroke-dasharray的付值如果个数是偶数就表示为画多少空多少的形式；如果是奇数就自动在后面加相同的数形成偶数，比如1 2 3就会变成1 2 3 1 2 3 于是就可以表示为画多少空多少的形式--），看下面的实例就明白了：

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/xy6R9/1/embedded/result,js,css,html" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

你可以拉动dasharray那个拉杆来改变dasharray的数值，我们就可以看到虚实之间的变化。

这样我们就可以利用这点配合javascript制作动画效果。利用javascript我们可以从DOM中获得path的一个长度：

    var path = document.querySelector('.squiggle-container path');
	path.getTotalLength(); // 988.0062255859375

### 动起来 ###

利用CSS3的animations或者是transitions也很容易制作动画效果。但是在不支持IE,如果想在IE上也有动画效果，那需要用到[requesAnimationFrame](https://developer.mozilla.org/en-US/docs/Web/API/window.requestAnimationFrame)这个API来制作动画效果。

当然我们也要避免使用SMIL，因为它既不支持IE,在chrome和safari也不尽如意。

我这里的这个效果是使用了CSS的transition来制作的，不支持IE。

在第一个例子中，我是使用SVG的属性来定义一条虚线，当然用CSS同样也可以做到。想知道更多的SVG和CSS相对应的属性可以去这个地址了解详细的[ SVG presentational attributes have identical CSS properties](http://www.w3.org/TR/SVG/styling.html)。

    var path = document.querySelector('.squiggle-animated path');
	var length = path.getTotalLength();
	// Clear any previous transition
	path.style.transition = path.style.WebkitTransition =
	  'none';
	// Set up the starting positions
	path.style.strokeDasharray = length + ' ' + length;
	path.style.strokeDashoffset = length;
	// Trigger a layout so styles are calculated & the browser
	// picks up the starting position before animating
	path.getBoundingClientRect();
	// Define our transition
	path.style.transition = path.style.WebkitTransition =
	  'stroke-dashoffset 2s ease-in-out';
	// Go!
	path.style.strokeDashoffset = '0';

实例如下：

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/W96DP/4/embedded/result,js,css,html" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

这里使用了getBoundingClientRect 这个方法。该方法获得页面中某个元素的左，上，右和下分别相对浏览器视窗的位置，他返回的是一个对象，即Object，该对象有是个属性：top，left，right，bottom；这里的top、left和css中的理解很相似，但是right，bottom和css中的理解有点不一样。关于这个可以去[Smashing Magazine](http://coding.smashingmagazine.com/2013/03/04/animating-web-gonna-need-bigger-api/)这篇文章看看。

### 动画2 ###

[Lea Verou](http://lea.verou.me/)也是用了差不多的技术制作一个加载的效果[create a loading spinner](http://dabblet.com/gist/6089395)以及Chrome的[Josh Matz](https://twitter.com/joshmatz)和[El Yosh](https://twitter.com/El_Yosh)也用相同的技术创建了一个动画效果[ this funky cube animation](http://dabblet.com/gist/6089409)。

上面的几个动画效果，我们都是利用**stroke-dasharray**的这个属性来创建动画效果，利用它我们还可以才创建更加复杂的效果出来，比如下面的这个效果：

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/5RZg2/1/embedded/result,js,css,html" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

