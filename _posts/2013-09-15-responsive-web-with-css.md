---
layout: post
title: 用CSS创建一个简单的响应式网站
categories:
- Life
tags:
- css
- 翻译
---


现在几乎到处都在讨论响应式设计。但是你真正懂得响应式设计么？我可不确定。很多web设计人员和开发人员并没有真正懂得响应式设计。这篇文章试图讨论下这个问题。

简而言之，我们这里并不会教你怎么为特定的移动设备来制作网站，而是通过调整布局来适应不同的设备的尺寸。

在这篇文章中，我们将讨论关于响应式设计的一些细节和原理，当然得首先正确的理解概念。理解好后，我们将建立一个简单的响应式网站。

响应式设计越来越流行的原因是移动设备越来越流行，比如iphone，安卓，平板电脑等等移动设备。

首先，我们要知道响应式设计，并不是为特定的设备来制作网站，而是网站布局要自适应不同设备。理解这一点很重要。这可能这要花点时间，但是这对于你的网站用户来说，能极大的提升你网站的用户体验。

我们来看一个例子，Nike Football这个网站。在PC上是这样的：

![](http://pic.yupoo.com/reicky_v/DadzZ4Ad/medium.jpg)

而当我用Iphone访问的时候，又是这样的：

![](http://pic.yupoo.com/reicky_v/DadzYIco/medium.jpg)

这就不是响应式设计。因为它完全是两个不同的网站。

现在在进行web响应式设计的时候，媒体查询变得越来越重要，是因为这样才能知道具体设备的尺寸从而调整布局适应设备。

这里推荐一个关于提供响应式网站设计灵感的网站，[Mediaqueri.es](http://www.netmagazine.com/tutorials/build-basic-responsive-site-css)。 

![](http://pic.yupoo.com/reicky_v/DaduViKD/K7j7b.jpg)

响应式的一个关键是流体布局。要创建一个流体布局，你得需要一个大的包装容器wrapper、内容区域content和自适应不同设备宽度的容器。简单起见，我将创建一个流体布局以及导航自适应，包含一张图片和两列内容的一个简单网站。这里用到了respond.min.js和html5.js这两个js文件，主要是为了让IE6-8支持媒体查询和html5的新标签。

### HTML和CSS ###

    <!DOCTYPE html>
		<html lang="en">
		<head>
		<meta charset="utf-8"/>
		<title> Demo | Responsive Web</title>
		<meta name="viewport" content="width=device-width, minimumscale=
		1.0, maximum-scale=1.0" />
		<link href="styles/main.css" type="text/css" rel="stylesheet">
		<!--[if lt IE 9]>
		<script src="//html5shiv.googlecode.com/svn/trunk/html5.js"></script>
		<![endif]-->
		<script type='text/javascript' src='scripts/respond.min.js'></script>
		</head>
		<body>
		<div id="wrapper">
		<header>
		<nav id="skipTo">
		<ul>
		<li>
		<a href="#main" title="Skip to Main Content">Skip to Main Content</a>
		</li>
		</ul>
		</nav>
		<h1>Demo</h1>
		<nav>
		<ul>
		<li><a href="#" title="Home">Home</a></li>
		<li><a href="#" title="About">About</a></li>
		<li><a href="#" title="Work">Work</a></li>
		<li><a href="#" title="Contact">Contact</a></li>
		</ul>
		</nav>
		<div id="banner">
		<img src="images/kaws.jpg" alt="banner" />
		</div>
		</header>
		<section id="main">
		<h1>Main section</h1>
		<p>Lorem&hellip;p>
		</section>
		<aside>
		<h1>Sub-section</h1>
		<p>Lorem &hellip;p>
		</aside>
		</div>
		</body>
	</html>

重点部分CSS来啦，这里需要注意的一点的是关于图片适配不同设备的样式，其实很简单，只要把图片的宽度设置为100%就可以了。

    /* Structure */
	#wrapper {
	width: 96%;
	max-width: 920px;
	margin: auto;
	padding: 2%;
	}
	#main {
	width: 60%;
	margin-right: 5%;
	float: left;
	}
	aside {
	width: 35%;
	float: right;
	}
	/* Logo H1 */
	header h1 {
	height: 70px;
	width: 160px;
	float: left;
	display: block;
	background: url(../images/demo.gif) 0 0 no-repeat;
	text-indent: -9999px;
	}
	/* Nav */
	header nav {
	float: right;
	margin-top: 40px;
	}
	header nav li {
	display: inline;
	margin-left: 15px;
	}
	#skipTo {
	display: none;
	}
	#skipTo li {
	background: #b1fffc;
	}
	/* Banner */
	#banner {
	float: left;
	margin-bottom: 15px;
	width: 100%;
	}
	#banner img {
	width: 100%;
	}

现在你的图片就会根据父级元素的宽度来自适应设备的宽度。当然得确定你图片的宽度不要大于父级元素的宽度。用这个方法的前提是，你图片要足够大，这样才可以适应不同设备的尺寸。

最终效果运行如下：

![](http://pic.yupoo.com/reicky_v/DadIWxRe/medium.jpg)

使用大图片，可能会对网站加载速度有影响，所以根据不同的设备的尺寸来加载不同尺寸的图片是有必要的。jQuery Mobile团队的Mat Marquis提供了一种解决方法-地址在[这里](http://www.alistapart.com/articles/responsive-images-how-they-almost-worked-and-what-we-need)。

#### 响应式设计 ####

现在最主要的是来设计响应式部分的代码了。响应式设计最关键的部分，是要设置好一个响应的断点，这样接下来的工作就好开展了，根据不同尺寸下，来通过css调整布局，我们这里设置的响应断点是480px，当设备尺寸在480以下的时候，把内容区域由两列布局调整为一列布局。具体代码如下：

    /* Media Queries */
	@media screen and (max-width: 480px) {
	#skipTo {
	display: block;
	}
	header nav, #main, aside {
	float: left;
	clear: left;
	margin: 0 0 10px;
	width: 100%;
	}
	header nav li {
	margin: 0;
	background: #efefef;
	display: block;
	margin-bottom: 3px;
	}
	header nav a {
	display: block;
	padding: 10px;
	text-align: center;
	}
	}

你可能注意到，在移动设备上的浏览器允许用户缩放网页，这样的话有可能破换整个网站的布局。

解决方法也很简单，只要在html文件里的head里面添加下面这段代码就可以禁止用户缩放网页：

    <meta name="viewport" content="width=device-width, minimum-scale=1.0, maximum-scale=1.0" />

具体表示的是：

- width：控制 viewport 的大小，可以指定的一个值，如果 600，或者特殊的值，如 device-width 为设备的宽度（单位为缩放为 100% 时的 CSS 的像素）。
- height：和 width 相对应，指定高度。
- initial-scale：初始缩放比例，也即是当页面第一次 load 的时候缩放比例。
- maximum-scale：允许用户缩放到的最大比例。
- minimum-scale：允许用户缩放到的最小比例。
- user-scalable：用户是否可以手动缩放

网站运行效果如下

![](http://pic.yupoo.com/reicky_v/DadSlk9i/14UyuD.jpg)

上面已经说过了，响应式设计不是专门为特定设备来制作网站。而是指网站在不同的设备上通过调整布局自适应设备尺寸。当然你也可以把重要的内容部分优先在移动设备上显示，但至少要保持一个网站的完整性和可用性，我们应该集中精力用响应式的技术来创建更好的，用户体验有好的网站。

CSS3中Flexible Box模块，让我们可以更方便的创建流体布局，Flexbox(伸缩布局盒) 是 CSS3 中一个新的布局模式，为了现代网络中更为复杂的网页需求而设计。本文将介绍 Flexbox 语法的技术细节。浏览器的支持越来越快，所以当 Flexbox 被广泛支持并应用时你将会快人一步。

Flexbox可以简单快速的创建一个具有弹性功能的布局，当在一个小屏幕上显示的时候，Flexbox可以让元素在容器（伸缩容器）中进行自由扩展和收缩，从而容易调整整个布局。它的目的是使用常见的布局模式，比如说三列布局，可以非常简单的实现。

响应式设计，路漫漫其修远兮~~~！

最终的运行效果在[这里](http://www.netmagazine.com/files/tutorials/demos/2013/01/build-a-basic-responsive-site-with-css/demo/demo.html)。

这篇文章翻译并整理自[build-basic-responsive-site-css](http://www.netmagazine.com/tutorials/build-basic-responsive-site-css)。删减了一些跟技术无关的文字。







