---
layout: post
title: 用SVG打造优雅迷人的用户界面元素
categories:
- Life
tags:
- svg
- 翻译
---

> 随着技术的发展和浏览器对SVG支持的越来越好，SVG变的越来越流行，本文就SVG来制作用户界面元素进行了探索。本文翻译自[demosthenes](http://demosthenes.info/blog)网站的[Create-SciFi-User-Interface-Elements-With-SVG](http://demosthenes.info/blog/796/Create-SciFi-User-Interface-Elements-With-SVG)。

不知道啥时候开始，制作一些不规则的界面元素非常流行。在互联网到处都可以发现不规则设计元素的运用，特别是在游戏方面的网站，对不规则元素的运用更是无所不用其极。

如下图所示就是不规则图片元素

![](http://pic.yupoo.com/reicky_v/Doexk8Ny/ZOOn4.jpg)

在过去，要想在网页中运用不规则元素，我们一般是先设计好不规则元素，然后切成图片插入到网页中或者是用CSS背景图片的技术来实现。这样的方法灵活性就大大的受到了限制。随着CSS和SVG的技术的发展，我们用CSS和SVG就可以设计出不规则的视觉元素。

你可能会想到CSS中的[clip](http://demosthenes.info/blog/384/CSS-clip)这个剪裁属性，它就是用来对元素进行切割剪裁的。但是[clip](http://demosthenes.info/blog/384/CSS-clip)这个属性毕竟是有限制的，对于一些规则的矩形等到是可以一用，可是面对复杂的不规则的形状，就力不从心啦。不过最近又添加了一个*clip-path*的属性，可以用来剪裁制作多边形。也是由一些列的坐标点来剪裁的，比如下面我们就用clip-path来制作一个多边形：

    clip-path: polygon(0px 0px, 245px 0px, 285px 50px, 285px 80px, 40px 80px, 0px 30px);

当然，你可以根据这些坐标点，用Adobe Illustrator或者是其它矢量设计软件很容易的绘制一个多边形。

### 开始 ###

首先新建一个简单的的HTML文件，代码如下：

    <nav id="futurebar">
	<a href="#"> Mathematics</a>
	<a href="#">Rocketry</a>
	<a href="#">Astronomy</a>
	</nav>

我们可以利用CSS的clip属性结合其它的属性来对图片进行剪裁：

    #futurebar a {
	display: inline-block; width: 285px; height: 80px;
	background-image: url(flask.jpg);
	-webkit-clip-path: polygon(0px 0px, 245px 0px, 285px 50px, 285px 80px, 40px 80px, 0px 30px);
	font-family: Agenda-Light;
	text-decoration: none;
	background-position: top center;
	background-size: cover;
	font-size: 3rem; color: #fff;
	text-shadow: 1px 1px 2px rgba(0,0,0,0.4);
	}

我们这里故意剪裁出和元素本身大小一样的形状。当然我们可以任意控制剪裁区域的大小，需要明白一点的是，图片不会自适应大小来适配剪裁区域，*clip-path*只是相当于创建了一个固定的蒙版来对图片进行剪裁。

### 跨浏览器支持 ###

*clip-path*目前需要添加特定的浏览器的前缀，并且只有webkit内核的浏览器直接有渲染引擎支持。火狐和IE9以上，我们可以用SVG来制作剪裁图形。我们在HTML文件的底部添加下面的SVG代码。

    <svg width="0px" height="0px">
	<defs>
	<clipPath id="shape">
	<polygon points="0 0, 245 0, 285 50, 285 80, 40 80, 0 30" />
	</clipPath>
	</defs>
	</svg>

注意一下**&lt;defs&gt;**和**&lt;clipPath&gt;**这两个元素。&lt;defs&gt; 标签是 definitions 的缩写，它可对诸如渐变之类的特殊元素进行定义。而这里把SVG的宽度和高度都设置为0，只是把SVG作为一个容器引入到页面中，为了控制其不可见，所以把宽高设置为0.

接下来在CSS中添加如下链接，把我们用SVGZ制作的形状引入进来：

    clip-path: url(#shape);

就这样，我们就可以制作跨浏览器的图形剪裁了。

### 制作一个动态背景 ###

为了使我们的背景能够产生若隐若现的效果，我们需要改动我们的HTML结构，如下所示：

    <nav id="futurebar">
	<a href="#"><span>Mathematics</span></a>
	<a href="#"><span>Rocketry</span></a>
	<a href="#"><span>Astronomy</span></a>
	</nav>

然后添加下面的CSS代码：

    #futurebar a span {
	background: rgba(0,0,0,0.4); display: block; width: 100%; height: 100%;
	transition: .8s; padding-top: .7rem; padding-left: 3rem;
	}
	#futurebar a:hover span { background: rgba(0,0,0,0); }

### 优势和劣势 ###

**clip-path**对于不支持改属性的浏览器的降级支持方案是简单的显示一个矩形。当然用**clip-path**制作一个剪裁的图形的时候，由于**clip-path**并没有把图形真正的剪裁掉，只是新建了一个遮罩蒙版来达到剪裁的目的，所以图形的热点区域还是原来剪裁前的区域，比如在本例中，你把鼠标移到剪裁区域边缘处，你会发现依然可以激活图片的热点区域。当然这个缺点在这个例子中显示的还不明显，但是如果是在一个比如八角形的剪裁图形上，那这个就很明显了。在这种情况下，你可能想到完全用SVG来制作图形就可以避免这个问题。我以前用SVG制作过一个SVG的热点地图的实例，在这里[Interactive SVG Map](http://demosthenes.info/blog/696/Using-SVG-as-an-Alternative-To-Imagemaps)可以去看看。

实例效果:

<p data-height="268" data-theme-id="0" data-slug-hash="KBygJ" data-user="dudleystorey" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/dudleystorey/pen/KBygJ'>SciFi User Interface Elements With SVG</a> by Dudley Storey (<a href='http://codepen.io/dudleystorey'>@dudleystorey</a>) on <a href='http://codepen.io'>CodePen</a></p>
<script async src="//codepen.io/assets/embed/ei.js"></script>
