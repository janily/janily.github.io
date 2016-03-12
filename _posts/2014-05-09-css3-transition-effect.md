---
layout: post
title: 8个简单的可以给用户带来惊喜的css3 transitions效果
categories:
- Life
tags:
- css3
- 翻译
---

> 现在css3在网页开发中用的越来越普遍，特别是css3中的transition和animation更是如此。今天我们就来看看8个简单的css3 transitions的效果，虽然简单但是只要在运用得当同样也可以给用户在浏览你的网页的时候带来惊喜。这篇文章来自[8 simple CSS3 transitions that will wow your users](http://www.webdesignerdepot.com/2014/05/8-simple-css3-transitions-that-will-wow-your-users/)。

仅仅只需要几行代码我们就可以给用户带来惊艳的交互效果，提升用户体验。最重要的是还可以利用硬件加速来提升动画效果的性能。虽然浏览器支持还不是非常全面，但是依然可以采取渐进增强的开发方式来运用它。

下面是8个简单的交互动画效果，运用得当能给你的用户带来惊喜。

首先准备基本的HTML页面，代码如下：

<p data-height="268" data-theme-id="0" data-slug-hash="BxAgn" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/BxAgn/'>BxAgn</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

具体可以查看HTML和CSS。

transition属性有三个值：第一个是定义使用transition的属性，运动的时间以及运动的方式。我们这里是使用默认的运动方式。

现在我们只要改变div的一些属性，它就会呈现一种平滑的过渡的动画效果。

### 1、淡入的动画效果 ###

淡入动画效果只需两步，第一步设置初始值；第二部是改变初始值就行了，如下所示：

<p data-height="268" data-theme-id="0" data-slug-hash="vyAhx" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/vyAhx/'>vyAhx</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

### 2、改变颜色 ###

要改变元素的颜色值，以前还是有一点点复杂的，现在我们只需要把我们想改变的颜色指定一个特定的类就行了，如下所示：


<p data-height="268" data-theme-id="0" data-slug-hash="ympkD" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/ympkD/'>ympkD</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

### 放大缩小效果 ###

以前要放大一个元素，可以改变元素的宽高或者是padding。不过现在我们可以使用CSS3中的**transform**来达到这个目的。

如下代码：

	.grow:hover
	{
	        -webkit-transform: scale(1.3);
	        -ms-transform: scale(1.3);
	        transform: scale(1.3);
	}

实际效果如下所示：

<p data-height="268" data-theme-id="0" data-slug-hash="mKJfq" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/mKJfq/'>mKJfq</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

同样缩小的效果可以用下面的代码来达到：

	.shrink:hover
	{
	        -webkit-transform: scale(0.8);
	        -ms-transform: scale(0.8);
	        transform: scale(0.8);
	}

<p data-height="268" data-theme-id="0" data-slug-hash="horJs" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/horJs/'>horJs</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

### 4、旋转元素 ###

使用transform还可以很轻松的旋转元素。如下代码所示：

	.rotate:hover
	{
	        -webkit-transform: rotateZ(-30deg);
	        -ms-transform: rotateZ(-30deg);
	        transform: rotateZ(-30deg);
	}

<p data-height="268" data-theme-id="0" data-slug-hash="DuIfx" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/DuIfx/'>DuIfx</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

### 5、由方变圆 ###

把一个矩形变成一个圆形，最近很流行这样动画效果。使用CSS很容易办到，只需要来定义**border-radius**属性就可以了。

    .circle:hover
	{
	        border-radius:50%;
	}

<p data-height="268" data-theme-id="0" data-slug-hash="mCgIi" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/mCgIi/'>mCgIi</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

### 6、3d阴影效果 ###

3d效果近一年好像不是很流行，因为最近扁平化开始风靡，3d效果被认为是拟物化设计与扁平化设计格格不入，whatever...

下面的这个3d效果是使用盒子阴影来制作的，然后使用**transform**和**translate**这两个属性来平移盒子的阴影，这样就可以制作出3d阴影的效果。

我们只需要把下面的**threed**的类添加到你指定的HTML元素上就可以了：

	.threed:hover
	{
	        box-shadow:
	                1px 1px #53a7ea,
	                2px 2px #53a7ea,
	                3px 3px #53a7ea;
	        -webkit-transform: translateX(-3px);
	        transform: translateX(-3px);
	}

效果如下所示：

<p data-height="268" data-theme-id="0" data-slug-hash="KtdFw" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/KtdFw/'>KtdFw</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

### 7、摇摆运动效果 ###

当然如果把**transition**和**animation**以及**@keyframes**，我们可以制作复杂一点的动画效果。

在下面的例子中，我们在样式里定义了CSS动画。当然为了解决浏览器兼容性问题，我们使用了**@-webkit-keyframes**和**@keyframes**来处理不同内核的浏览器的兼容性问题，这方面IE浏览器比chrome做的好一点，它遵循标准的写法。

	@-webkit-keyframes swing
	{
    15%
    {
        -webkit-transform: translateX(5px);
        transform: translateX(5px);
    }
    30%
    {
        -webkit-transform: translateX(-5px);
       transform: translateX(-5px);
    } 
    50%
    {
        -webkit-transform: translateX(3px);
        transform: translateX(3px);
    }
    65%
    {
        -webkit-transform: translateX(-3px);
        transform: translateX(-3px);
    }
    80%
    {
        -webkit-transform: translateX(2px);
        transform: translateX(2px);
    }
    100%
    {
        -webkit-transform: translateX(0);
        transform: translateX(0);
    }
	}
	@keyframes swing
	{
    15%
    {
        -webkit-transform: translateX(5px);
        transform: translateX(5px);
    }
    30%
    {
        -webkit-transform: translateX(-5px);
        transform: translateX(-5px);
    }
    50%
    {
        -webkit-transform: translateX(3px);
        transform: translateX(3px);
    }
    65%
    {
        -webkit-transform: translateX(-3px);
        transform: translateX(-3px);
    }
    80%
    {
        -webkit-transform: translateX(2px);
        transform: translateX(2px);
    }
    100%
    {
        -webkit-transform: translateX(0);
        transform: translateX(0);
    }
	}

现在只要把这个动画运用到指定的元素上，元素就产生左右摇摆的效果。效果如下所示：

<p data-height="268" data-theme-id="0" data-slug-hash="sfctD" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/sfctD/'>sfctD</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>


### 8、Inset border(这个真不知道咋翻译) ###

现在很流行**ghost button**风格的按钮样式。CSS3的出现我们有一种简单的解决方案，就是使用内阴影这个属性即**box shadow**。

只要简单的使用下面这句代码就行了：

	.border:hover
	{
	        box-shadow: inset 0 0 0 25px #53a7ea;
	}

<p data-height="268" data-theme-id="0" data-slug-hash="qsxuD" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/qsxuD/'>qsxuD</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

OK，上面就是简单的8个css3的transition效果，正所谓，运用之妙，存乎一心！