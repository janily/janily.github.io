---
layout: post
title: 使用CSS制作玻璃模糊效果
categories:
- Life
tags:
- css
---

> 随着CSS3的推出，以前只能依靠图片实现的一些效果现在使用CSS中的滤镜也能轻松的实现各种photoshop的滤镜效果。今天这篇文章我们就来看看使用CSS滤镜来实现玻璃模糊效果。原文地址是[Frosting Glass with CSS Filters](http://css-tricks.com/frosting-glass-css-filters/)。

在一些图片处理软件中，提供了像模糊、饱和度以及对比度的滤镜来实现一些特别的图片效果。而且在web中也大量的应用了这些图片效果，不过都需要先在图片处理软件中先处理好图片效果，再应用到web中。不过随着web技术的发展，现在我们直接可以使用CSS滤镜来实现这样的效果。这篇文章我们就来实现一个常见的玻璃模糊效果。

### 老技术实现玻璃模糊效果 ###

像玻璃模糊这种效果在很早以前就应用在web中，以前有写过这方面的文章，可以去[看看](http://css-tricks.com/blurry-background-effect/)。实现原理也很简单，就是使用一个模糊的遮罩图片遮在正常的图片上来实现模糊效果。我们来看看下面这个例子，点击箭头的时候，一个透明的层就会遮罩背景图片实现模糊效果。

<p data-height="300" data-theme-id="1" data-slug-hash="40cd4258f2d72a60f37a5e2f47124b7e" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/adobe/pen/40cd4258f2d72a60f37a5e2f47124b7e/'>Frosted Glass Effect Using Multiple Images</a> by Adobe We b Platform (<a href='http://codepen.io/adobe'>@adobe</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

我们首先来看看它的HTML结构，非常简单。

    <article class="glass down">
	  <h1>Pelican</h1>
	  <p>additional content...</p>
	</article>

再来看看CSS代码，首先是设置整个视图的宽高。然后我们使用了一个模糊的背景来覆盖原来的背景，点击触发的时候会覆盖整个背景图片。我们这里是给**.glass**元素定义了一个白色的背景并配合透明度来制作模糊背景的。

    html, body, .glass {
    width: 100%;
    height: 100%;
    overflow: hidden;
	}
	body {
	    background-image: url('pelican.jpg');
	    background-size: cover;
	}
	.glass::before {
	    display: block;
	    width: 100%;
	    height: 100%;
	    background-image: url('pelican-blurry.jpg');
	    background-size: cover;
	    content: ' ';
	    opacity: 0.4;
	}
	.glass {
	    background-color: white;
	}

上面的CSS是创建模糊背景的代码。我们还要定义点击的时候箭头方向的控制。具体点击上面的demo就知道具体的效果了。这里我们使用的是CSS3中的**transform**来控制的。关于transform可以去看看[这篇文章](http://blogs.adobe.com/webplatform/2014/03/18/css-animations-and-transitions-performance/)。

    .glass.down {
    transform: translateY(100%) translateY(-7rem);
	}
	.glass.down::before {
	    transform: translateY(-100%) translateY(7rem);
	}
	.glass.up, .glass.up::before {
	    transform: translateY(0);
	}

### 注意 ###

如果你想进一步了解这个效果的原理，我制作了一个详细的效果的分解例子，可以去[看看](http://cdpn.io/4dba97b7d03412ed1e9dcb62a23dfaa8)。

上面这个方法非常简单，并且支持大部分的浏览器。我们综合使用了CSS中的[伪元素](http://caniuse.com/#feat=css-gencontent)、[透明度](http://caniuse.com/#feat=css-opacity)、[transform](http://caniuse.com/#feat=transforms2d)和[background-size](http://caniuse.com/#feat=background-img-opts)这些属性大部分浏览器支持。

### 使用滤镜来实现玻璃模糊效果 ###

上面实现图片模糊的技术需要使用一张模糊图片来覆盖原图片从而实现模糊效果，但是当你有很多的图片需要实现这样的效果的时候，那就会变得异常痛苦了。比如，在响应式的设计中，需要在不同设备不同的尺寸情况下实现模糊效果。或者图片不是固定的而是动态变化的情况下的模糊效果。CSS滤镜的出现就很容易面对这样的情形。

关于CSS滤镜的分类可以去看看[这篇文章](http://css-tricks.com/almanac/properties/f/filter/)。

我们只要稍微调整下CSS代码，即使用**blur**滤镜来对原图进行滤镜处理。

#### 之前的CSS代码 ####

    .glass::before {
    background-image: url('pelican-blurry.jpg');
	}

#### 使用滤镜后的代码 ####

    .glass::before {
    background-image: url('pelican.jpg');
    filter: blur(5px);
	}

效果如下所示：

<p data-height="300" data-theme-id="1" data-slug-hash="d056d1b26b9683c018f9bb9e0f1b0e1c" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/adobe/pen/d056d1b26b9683c018f9bb9e0f1b0e1c/'>Frosted Glass Effect Using Filter Effects</a> by Adobe Web Platform (<a href='http://codepen.io/adobe'>@adobe</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

非常简单试用。不过由于CSS滤镜推出的时间不是很久，意味着我们要使用特定的浏览器前缀来实现效果，并且[浏览器的支持](http://caniuse.com/#feat=css-filters)也不是很很好不过，在SVG中滤镜确已经出现很久了，并且大部分的[浏览器都支持](http://caniuse.com/#feat=svg-html)，我们可以使用SVG来针对不支持CSS滤镜的浏览器做一个回退方案。

要使用SVG滤镜，我们需要在HTML中插入	SVG代码，并且使用**url()**的方式来使用滤镜。如下所示：

    <svg xmlns="http://www.w3.org/2000/svg" version="1.1">
	  <defs>
	    <filter id="blur">
	      <feGaussianBlur stdDeviation="5" />
	    </filter>
	  </defs>
	</svg>

CSS:

    .glass::before {
    background-image: url('pelican.jpg');
    /* Fallback to SVG filters */
    filter: url('#blur');
    filter: blur(5px);
	}

当然也会碰到SVG和CSS滤镜都不被支持的情形。在这种情形下，用户是看不到模糊效果的，不过也还是可以接受的。

### 总结 ###

滤镜可以使我们脱离图片处理软件来制作出很多的滤镜效果。在新版本的chrome、safari以及opera和[firefox开发版](https://bugzilla.mozilla.org/show_bug.cgi?id=869828)中都支持CSS滤镜，IE到现在都还不支持CSS滤镜。不过可以使用一些回退方案来处理这种情况。现在我们可以在web开发中大胆的使用CSS滤镜来为web增色。


