---
layout: post
title: 	CSS3动画制作技巧
categories:
- Life
tags:
- css
- 翻译
---

> 这篇文章来自于**CSS-TRICKS**网站的[CSS Animation Tricks: State Jumping, Negative Delays, Animating Origin, and More](http://css-tricks.com/css-animation-tricks/)。主要是讲了一些CSS3动画制作技巧，非常不错。

### 1.不同状态之间转换的动画效果 ###

利用CSS动画可以很容易制作同一属性不同值之间的过渡的动画效果。只需要在两个不同的**keyframes**之间赋予两个差异很小的值就行了。

    @keyframes toggleOpacity {
	  50% { opacity: 1; } /* Turn off */
	  50.001% { opacity: 0.4; }
	
	  /* Keep off state for a short period */
	
	  52.999% { opacity: 0.4; } /* Turn back on */
	  53% { opacity: 1; }
	}

比如下面这个效果就是利用**text-shadow**这个属性来模仿灯光闪烁的效果。

<p data-height="268" data-theme-id="0" data-slug-hash="snjrJ" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/snjrJ'>No Vacancy 404 CSS Only</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

### 2.利用负的动画延迟来制作动画效果 ###

一般动画延迟我们都是使用正数来定义的，即动画需要延迟一定的时间才开始执行。那如果使用一个负数来定义延迟时间会发生什么样的效果呢？顾名思义，负数的时间即表示是已经过去的时间，所有如果一个动画的延迟时间使用负数定义的，那就表示动画会立即执行。换句话话说，就是提前执行动画效果。我们就可以利用这一点来重复的在一些元素上执行已经定义好的动画效果。

下面这个例子就是利用负数定义延迟时间的这一个技巧来制作的动画效果：

<p data-height="268" data-theme-id="0" data-slug-hash="Cxerc" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/Cxerc'>Circle Snake</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

### 3.均衡运行的动画效果 ###

CSS3不愧是一把好刀，它能做到什么完全看使用刀的人。没有做不到，只有想不到。我们还可以使用百分比来制作均衡运行的动画效果。

在我的很多的动画效果中，我经常利用圆或者矩形的宽高来制作动画效果。一般我都会使用固定的数值来定义元素的宽高。但是下面这个例子，我们将使用百分比的形式来定义元素的宽高以及内边距。而内边距需要与元素的宽成正比，比如：

    .container {
	  position: relative;
	  display: block;
	  width: 100%;
	  height: 0;
	  padding-bottom: 100%;
	}

下面这个例子就用到百分比来定义元素的宽高，当然也用到负数定义延迟时间这个技巧。

<p data-height="268" data-theme-id="0" data-slug-hash="kHEGt" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/kHEGt'>Responsive CSS bars</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

### 4.改变被转换元素的位置来制作动画效果 ###

在CSS3的transform中的[transform-origin](http://css-tricks.com/almanac/properties/t/transform-origin/)其实也可以来制作动画效果。在下面这个例子中，我们结合translate和transform-origin来制作了一个动画效果：

<p data-height="268" data-theme-id="0" data-slug-hash="HqDIF" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/HqDIF'>Change transformation-origin mid animation</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

也可以去看看[这个例子](http://codepen.io/Zeaklous/pen/fsGvH)。

### 5.使用负值来制作环绕的动画效果 ###

环绕的动画效果现在也很常见。一般是利用**translate**和**rotation**这两个属性来制作，可以去看看[这篇文章](http://lea.verou.me/2012/02/moving-an-element-along-a-circle/)，我们也可以使用**transform-origin**配合使用伪类来更简单的实现这个环绕的动画效果。

<p data-height="268" data-theme-id="0" data-slug-hash="zxCbm" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/zxCbm'>Simple CSS Circular Motion Technique</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

### 6.box-shadow的妙用 ###

对于一些没有具体的内容的形状，[box-shadow](http://css-tricks.com/almanac/properties/b/box-shadow/)这个属性就有用武之地。使用box-shadow这个属性可以[创建多个形状](http://css-tricks.com/snippets/css/multiple-borders/)。我们可以移动阴影的位置来来创建新的元素。而且还可以配合动画属性来制作动画效果，下面这个例子中的圆圈就是使用box-shadow来制作的。

<p data-height="268" data-theme-id="0" data-slug-hash="xyJbG" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/xyJbG'>Single element color loader</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

### 7.使用伪类元素来制作动画效果 ###

使用box-shadow和伪类，可以给元素定义更多的内容。而且还可以分别为它们定义不同的动画效果。下面这个例子就是使用box-shadow和伪类来制作的：

<p data-height="268" data-theme-id="0" data-slug-hash="qbrHJ" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/qbrHJ'>Single Element gif Recreation</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

在上面的这个例子中的形状都是使用box-shadow来创建的。另外两个小圆是用伪类来制作的而中间的虚线的圆圈是用SVG来创建的。

### 另外一些需要注意的事情 ###

#### 能使用transforms就是用transform ####

[Paul Irish在这篇文章中说到](http://www.paulirish.com/2012/why-moving-elements-with-translate-is-better-than-posabs-topleft/)使用transform来移动元素的位置比使用top/left值移动元素位置的性能要好。

而且Transform对响应式设计的支持也非常好，如**scale**可以去看看[这篇文章](http://codepen.io/amos/pen/Bypur)。

不使用transform也会导致其它一些问题，比如不容易捕获当前运动的元素，在这个[动画中](http://jsfiddle.net/cL5bW/)，虽然元素本身的颜色的值是正确的但是表现出来颜色却是错误的。而是用[transform来改写](http://jsfiddle.net/qA4V9/)后就解决了上面的问题。

#### z-index的问题 ####

在我制作动画的效果中由于**z-index**引起了一些问题，我花了很多时间去解决问题。不同浏览器对于z-index在动画中的渲染是有问题的，Mozilla不支持在动画中动态改变z-index的值，而webkit却支持。

另外需要注意的是，如果想要一个元素的伪元素显示在父元素的下面，那这个伪元素的z-index的值应该定义为一个负值。

最后一个需要注意的是z-index和opacity之间的一个问题。当一个元素的opacity的值为1的时候，它会形成自己的一个上下文的环境。更多的细节可以去[这篇文章](http://philipwalton.com/articles/what-no-one-told-you-about-z-index/)。

#### 灵感 ####

我们在一些网页上看到的一些有趣的动画效果，其实都是来源我们真实的生活。只是再加上一点点想象力，它就会变得与众不同。所以在生活中，只要仔细观察处处皆能为我们所用，都能给我们灵感和启发。




