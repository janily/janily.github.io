---
layout: post
title: 	HTML5的fullscreen的API初探
categories:
- Life
tags:
- html5
- 翻译
---

> HTML5包含了很多的API，今天来学习下**fullscreen**这个API的使用方法，这个API从单词的本意上理解就是全屏编程API，如果页面要实现全屏预览，需要隐藏浏览器地址栏工具类等组件就可以使用这个API来实现全屏的功能。这篇文章就来看看**fullscreen**这个API的具体使用方法，文章来自[Introducing the HTML5 FullScreen API](http://demosthenes.info/blog/708/Introducing-the-HTML5-FullScreen-API)。

现在像苹果、微软、谷歌和Mozilla这些引领技术潮流的科技公司都在推行扁平设计的设计风格，尽管这种极简主义的设计趋势不可避免，但是在浏览器中一些必要的像导航和操作按钮等元素还是必不可少的。

像比如游戏、视频等这些应用有时候在浏览器中运行的时候，浏览器的其它一些元素如工具栏等会影响用户使用这类应用的体验。为了给用户一个好的体验，像Adobe Flash这类单独的浏览器应用就提供了一个**全屏模式**的选项它能够隐藏浏览器比如工具栏等元素使应用程序以全屏的方式运行。HTML5也推出了一个全屏编程的API就是**fullscreen**。

就像[Geolocation](http://demosthenes.info/blog/701/Introducing-HTML-GeoLocation-DeviceOrientation-and-Acceleration)这个API一样，全屏API也是要基于用户许可才能运行的是被动式执行的，也就是必须得到用户许可的情况下才能使用这个API，不能在用户不知情的情况下强制执行这个API。

虽然这样做是为了保护用户的安全，不过我猜想有些开发人员会认为这个全屏API只是弹窗的一个变种。不过大部分的应用并不需要执行全屏来使用，只是在一些需要用户聚焦的应用不希望被别的因素分心的应用里这个全屏API才有用武之地，比如：

- 演讲展示
- 浏览器驱动的一些应用程序
- 游戏应用
- 视频观看

需要记住的是一般用户平均在一个网页上花的时间一般不超过一分钟，正所谓钱要用在刀刃上，不能滥用全屏API。同样需要注意的是，在图片应用上你需要准备[高清的图片](http://demosthenes.info/blog/586/CSS-Fluid-Image-Techniques-for-Responsive-Site-Design)以应对全屏的使用场景。

### **浏览器支持情况** ###

全屏API在桌面浏览器上的支持度还不错，Firefox 10+, Chrome 15+, Internet Explorer 11 和 Safari 5.1 都支持这个API。现在主要的问题是每个浏览器对这个API的实现都需要加上特定浏览器的前缀，就像[CSS中一样](http://demosthenes.info/blog/217/CSS3-Vendor-Prefixes)，如果我们要使用**fullscreen**API的话，我们要使用下面的代码来实现：

    element.requestFullscreen(); element.mozRequestFullScreen();
	element.webkitRequestFullscreen(); element.msRequestFullscreen();

从上面的代码可以看到在标准的**requestFullscreen**中小写的形式，但是在添加了浏览器的前缀后风格就变成了javascript中驼峰式的编写风格也就是单词的第一个字母要大写，这个要注意下。

至于Internet Explorer11实现全屏的方式也很不同，这个需要另开一篇文章讨论。

### 编写代码实现fullscreen ###

了解了一些基本知识后，现在就让我们来编写代码检测浏览器是否支持**fullscreen**这个API。首先我们需要确保针对不同浏览器来添加特定的前缀来实现全屏的功能，我们把它写在一个函数里面如下所示：

	<script>
	function launchFullScreen(element) {
	if (element.requestFullscreen)
	{ element.requestFullscreen(); }
	else if (element.mozRequestFullScreen)
	{ element.mozRequestFullScreen(); }
	else if (element.webkitRequestFullscreen)
	{ element.webkitRequestFullscreen(); }
	else if (element.msRequestFullscreen)
	{ element.msRequestFullscreen(); }
	}
	</script>

这个方式只接受一个参数，那就是我们要触发浏览器进入全屏模式的钩子。如果你想让整个页面处于全屏模式，那就需要设置html根元素即**documentElement**就如全屏模式。一般都会使用一个按钮或者链接来触发全屏模式:

    <button onclick="launchFullScreen(document.documentElement)"> Fullscreen </button>

同样的原理，你可以设置任何你想让它进入全屏模式的元素：

    <button onclick="launchFullScreen(document.getElementById('pic'))">Fullscreen Image </button>

注意一下[HTML5的视频元素](http://demosthenes.info/blog/271/HTML5-Video)内置了支持全屏的功能。

使用全屏模式意味着也需要调整CSS，可以去看看[这篇文章](http://demosthenes.info/blog/713/Switching-To-High-Resolution-Images-For-Fullscreen-Display-On-The-Web)。

### **退出全屏模式** ###

**ESC**按键是退出全屏模式的一个标准触发按钮，至少在目前来说它是一个好的开发实践。大部分的应用程序都是使用ESC来退出的，对于用户体验来说也是节省了很多的使用成本。退出全屏的方法是**CancelFullScreen(这个是旧的退出方法)**和**exitFullscreen**这两个方法，我们使用新版的方法可以这样使用。

    <script>
	function cancelFullScreen() {
	if (document.exitFullscreen) {
	document.exitFullscreen();
	} else if (document.mozCancelFullScreen) {
	document.mozCancelFullScreen();
	} else if (document.webkitExitFullscreen) {
	document.webkitExitFullscreen();
	} else if (document.msExitFullscreen) {
	document.msExitFullscreen();
	}
	}
	</script>

OK，我们也需要触发退出全屏方式的钩子：

    <button onclick="cancelFullScreen(documentElement)">Cancel Fullscreen</button>

你可以想象，要在适当的时候提示用户这些全屏的交互元素还是有点复杂的。

如果用户忽视全屏模式的提醒，那浏览器还是会在标准模式下正常工作。



