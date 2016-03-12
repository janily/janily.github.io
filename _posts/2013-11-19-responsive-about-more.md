---
layout: post
title: 伪元素、媒体查询等更多关于响应式web开发的一些东东
categories:
- Life
tags:
- css
- 翻译
---

> 这篇文章来自[toptal](http://www.toptal.com/)网站的[Introduction to Responsive Web Design: Pseudo-Elements, Media Queries, and More](http://www.toptal.com/front-end/introduction-to-responsive-web-design-pseudo-elements-media-queries/)。

今时今日，人们可以通过各种设备，比如桌面PC电脑，笔记本电脑，平板设备以及智能手机等等来访问互联网。

为了用户体验的一致性，你的网站需要适配各种不同的设备。这个适配各种设备的开发过程就称之为[响应式设计](http://alistapart.com/article/responsive-web-design)即**RWD**。

为什么现在响应式会显得如此重要呢？一些网页设计师，比如为了保持网站一个稳定统一的用户体验，通常会发一些时间来调整在IE浏览器的特定问题。

在如今日新月异的互联网的时代，还以IE浏览器为开发标准的开发方式不得不说是一个愚蠢的开发方法。

> 一些网页开发者为了IE的兼容性而发很多时间调整，而把移动用户的体验问题放在IE之后，这种开发方式是及其愚蠢的。

我们先来看看这个统计数据[Mashable called 2013 the year of responsive web design](http://mashable.com/2012/12/11/responsive-web-design/)。为什么呢？从这个统计数据可以看出，想在web的流量超过30%是来自移动设备的。他们预计这个数字在年底会超过50%。而且另一组统计数也看出2013年17.4%的流量来自于智能手机[17.4% of web traffic came from smartphones in 2013](http://mashable.com/2013/08/20/mobile-web-traffic/)。同时整个IE浏览器的流量仅仅只有12%，相较去年还下降了4%。可以去这个地址看看[W3School](http://www.w3schools.com/browsers/browsers_stats.asp)。如果你现在开发一个web，还是只针对一个特定的浏览器，而忽视移动设备的用户，那无疑是拣了芝麻丢了西瓜。从另外一方面来说，响应式设计对于[SEO](http://moz.com/blog/seo-of-responsive-web-design)、[转化率](http://conversionxl.com/how-responsive-design-boosts-mobile-conversions/#)、和用户的[跳出率](http://mashable.com/2012/02/02/lower-bounce-rate-tips/)这些web指标来说有着重要影响。

### 响应式设计理论 ###

响应式是设计不仅仅是调整你的页面布局，而是要把重点放在调整你的整个网站的展现逻辑来适应不同的设备。比如鼠标和手势支持的差异，在桌面上和移动设备上这就是一个不同的用户体验，你说呢？移动设备和桌面PC应该要体现这些差异。

同时，你也不会想重写整个网站以来适应不同的设备。相反，我们应该采用的解决方案是，采用同一套HTML代码来灵活的适配不同的设备。

CSS3为我们的响应式设计提供了强有力的支持，主要是用到了这几个技术[CSS media queries](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Media_queries)，[pseudo-elements](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-elements)以及网格系统，还有其它的一些技术。

### 媒体查询(Meida Queries) ###

媒体查询其实在[HTML4](http://www.w3.org/TR/REC-html40/)和[CSS2.1](http://www.w3.org/TR/CSS2/)就已经存在了，比如，我们一般链接CSS的时候，会这样写：

    <link rel="stylesheet" type="text/css" href="screen.css" media="screen">
	<link rel="stylesheet" type="text/css" href="print.css"  media="print">

或者是：

    @media screen {
    * {
        background: silver
	   }
	}

但是在[CSS3](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS3)中，你可以用媒体查询来侦测页面的宽度。这样你就可以针对不同宽度的设备来定义样式。*现在大部分的浏览器已经支持这个媒体查询的属性了[支持度](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS3)*

关于媒体查询有几个基础的属性：**max-width**,**device-width**,**orientation**和**color**。最重要的是这两个属性，关于屏幕辨率的设置和横屏和竖屏的判断(landscape和portrait)。

下面的这个例子，是判断如果屏幕的最大的宽度是480px，那么就会加载*main_1.css*文件，如果大于480px，则不会加载。

    <link rel="stylesheet" media="screen and (max-device-width: 480px)" href="main_1.css" />

当然我们也可以在css文件中，用媒体查询来定义不同屏幕分辨率的样式。比如，下面的这个例子就定义屏幕的宽度在480px以上就是用这个样式：

    @media screen and (min-width: 480px) {
    div {
        float: left;
        background: red;
    }
    .......
	}

### 页面放大缩小问题(Smart Zoom) ###

在移动设备上我们经常会碰到页面智能放大缩小的问题。这个问题的触发一般是两个途径，一种是用户引起的（比如用两个手指在智能手机的屏幕上放大缩小）以及智能手机在加载网页的时候，就按照网页实际的尺寸来加载，就会呈现一个放大版的网页。

有了媒体查询我们就可以解决这个问题，它能够禁止页面的放大缩小和呈现在屏幕上的比例。

    <meta name="viewport" content="width=device-width, initial-scale=1" />

设置**initial-scale**的值为1，即表示页面在加载的时候缩放比例。如果你的web应用准备用响应式开发，那么设置**initial-scale**的值是非常重要的。

当然我们也可以禁止用户来缩放网页，我们可以设置**user-scalable**的值为false就可以了。

### 网页的宽度 ###

如果你仅仅是想适配三种不同设备的情况：桌面PC、平板设备和智能设备。那用哪个屏幕的尺寸来作为适配的断点呢？

不幸的是，现在市场上的设备非常多，比如下面这些尺寸就非常常见，很难选一种尺寸来作为断点设置：

- 320px
- 480px
- 600px
- 768px
- 900px
- 1024px
- 1200px

那我们该怎么做呢，这多不同大小的设备？我们先看看别人是怎么做的，比如[320 and Up](http://stuffandnonsense.co.uk/projects/320andup/)这个响应式的框架，主要是用媒体查询设置了5个断点：480，600，768，992和1382。像这样的设置方法随便就可以捣鼓出十多个类似的方法出来。

当然，在实际开发中你完全可以针对每一种设备来单独写样式。但是这有点杀鸡用牛刀了，实际上，完全没有必要针对每一种不同的设备来适配样式，只要适配常见的分辨率就可以了。以我的经验，我一般是适配320px,768px和1200这三种大小的屏幕；这三种大小的屏幕差不多覆盖了常见的三种设备，桌面PC，平板设备和智能手机。

### 伪类 ###

用媒体查询来编写样式的时候，你可能要根据设备的大小来显示或者隐藏一些信息。这里可以用CSS很好的做到。

一般来说，我们用**display：none**这个属性来隐藏元素是一种很常见的做法并且也是一种值得推荐的做法，随着设备屏幕的变小，总减少布局空间。这样一来隐藏元素是一种非常好的解决方案。

不过这里，你也可以使用伪类元素[pseudo-elements](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-elements)这一个属性来做到，比如**：before**和**after**。现在大部分浏览器已经支持伪类了，[支持度](http://caniuse.com/#feat=css-gencontent)。

CSS伪元素用于向某些选择器设置特殊效果。或者是选择一个特定元素的子元素。比如**first-line**伪元素用于向文本的首行设置特殊样式。与之相似的是**a:visited**伪元素可以定义访问后链接的样式。

这里有一个简单的实例，我们创建一个有三种不同布局的登陆按钮。分别为桌面PC、平板设备和智能手机。在智能手机上仅仅显示一个按钮，在平板设备上显示一个按钮和文字"User name"。最后在桌面PC上显示按钮和完全的文字信息"Insert your user name"。

样式如下：

    .username:after {
    content:"Insert your user name";
	}
	@media screen and (max-width: 1024px) {
	    .username:before {
	        content:"User name";
	    }
	}
	@media screen and (man-width: 480px) {
	    .username:before {
	        content:"";
	    }
	}

仅仅使用**：before**这个伪元素就可以做到，如图：

![](http://pic.yupoo.com/reicky_v/Dk8iho6D/medium.jpg)

关于伪类的更多使用方法，可以去看看这篇文章[write-up](http://css-tricks.com/pseudo-element-roundup/)。

### 如何开始响应式设计开发 ###

我们已经建立了一些响应式开发的知识，那么我们该如何开始呢？

第一步，我们应该把页面先在不同的设备适配设计好效果图。如下图：

![](http://pic.yupoo.com/reicky_v/Dk8j11LU/medium.jpg)

在这个效果图里，左边的绿色的方块是一个主菜单的。当在小屏幕上的时候，它会全屏显示，用媒体查询很容易做到：

    @media screen and (max-width: 1200px) {
    .menu { 
        width: 100%;
    }
	}
	
	@media screen and (min-width: 1200px) {
	    .menu {
	        width: 30%;
	    }
	}

当然这种方法也有很多的不足之处，在移动设备和桌面设备上，网站的内容往往也有很大的不同，这就决定网站的响应式不仅仅取决于CSS的适配，而已和HTML和javascript有很大的关系。

当决定为不同设备适配不同的布局的时候，有几个元素很关键。不同于桌面设备我们有足够的空间用来布局，智能手机对布局要求更为严格。这时候更加要突出重要的内容和内容的层级性。

对于响应式设计来说，当然你的内容展现方式也很重要。比如在桌面PC电脑上，你可以把一些信息隐藏起来，只有当用户把鼠标移动到目标元素的时候，信息才展示出来。这种信息展现方式在web上很常见。但是在智能手机上，这种信息的展现方式就不是很适当。

此外，如果你把一个在web上按钮同比例的在智能手机等设备上显示，这是非常不好的。看下面这张图，你会看到左边的是一个web标准的视图同比例的在智能手机上的显示，可用性大打折扣。

![](http://pic.yupoo.com/reicky_v/DkdrESGj/medium.jpg)

一般更好地实践是，突出重要内容的显示，一些次要的内容可以不需要显示。

当我们使用媒体查询的时候，我们要时刻注意页面上的元素在页面尺寸变化的时候的表现行为，而不仅仅是着眼于依靠一些网格系统就了事了。元素尺寸大小的变化都会影响到页面的布局。比如图片元素，我们可以这样来设定它的样式：

    img {
    max-width: 100%
	}

这样图片元素的尺寸就自适应页面大小的变化，另外一些也可使用类似的方法来解决，比如用[iconFonts](http://css-tricks.com/examples/IconFont)来制作一些图标对于响应式开发来说是一种非常好的解决方案。

### 关于流体网格系统 ###

当我们在设计一个web的时候，我们常常会在优化视觉体验上花费很多的时间。主要是在易用性上，易读性，直观的导航这三个方面来展现你的内容。在这三个方面来说，最重要的是关于元素的宽度的设置。比如，关于流体网格系统一个重要的特性是，设置元素的宽度是以白分比来设置的。

当然这里不详细讨论关于网格系统，需要额外写一篇文章才能详细阐述相关的细节。因此我这里推荐一些现在比较好的前端系统：[Bootstrap](http://getbootstrap.com/2.3.2/)，[Unsemantic](http://unsemantic.com/)和[Brackets](http://www.brackets.io/)。

现在的web优化，重要的是不仅要知道你的目标用户是谁，而且还要知道用户是如何来使用的你的web的。这样我们的web的优化才会事半功倍。





