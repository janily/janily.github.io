---
layout: post
title: 	用javascript打造优美的动态滚动效果(1)
categories:
- Life
tags:
- javascript
- 翻译
---

> 一直以来滚动这个常见的web效果在移动设备上体验不是很好有各种各样的问题。主要是因为在移动设备上的交互操作和桌面PC是完全不一样的，桌面上的交互操作是通过鼠标来完成的，而移动设备是通过手指来完成交互的。像滚动这个效果在PC上和容易处理，一句**overflow:auto**就能搞定。而在移动设备上确有很多的问题，在浏览器没有解决这个问题前不得不借助javascript来解决，比如**iscroll**这个知名的第三方的库就是用来处理在移动设备上滚动问题的。那如果我们自己使用javascript来处理移动设备上的滚动该如何做呢？根据这篇文章我们来一步一步使用javascript来编写移动设备上的滚动效果。文章来自[JavaScript Kinetic Scrolling](http://ariya.ofilabs.com/tag/kinetic)。

自从iphone问世以后，它所创造的用户界面和人机交互效果几乎成为了整个移动设备在人机交互处理上的一个标杆，比如符合动力学原理的列表滚动效果就是其中之一。而是用HTML和javascript实现这种效果对于广大的开发人员来说确实有不小挑战。基于此，我将通过几个简单的例子来实现这种符合动力学原理的滚动效果。

正所谓万丈高楼平地起，开始之前打造一个坚实的基础是非常重要的。因此第一部分先来实现一个没有任何动力效果基本的支持拖拽滚动的效果。这个效果是实现其它高级效果的基础，你可以去[这个地址](https://github.com/ariya/kinetic/)下载所有的代码。

![](http://pic.yupoo.com/reicky_v/DAZm1Y3A/medium.jpg)

首先让我们来假设一下，我们需要滚动的视图区域即[ DOM element](http://dom.spec.whatwg.org/#interface-element)内容非常多。在移动设备上，如果设备的屏幕尺寸不足以容纳内容的时候，可以通过滚动视图区域的内容来浏览内容但是要视图区域要保持固定不动。为了准确的控制内容的滚动[CSS3 transform](http://dev.w3.org/csswg/css3-transforms/)，我们需要准确的捕捉用户跟视图的交互动作。如当用户触摸屏幕并拖拽视图的时候，我们需要控制视图滚动的距离和用户手指滚动之间的关系。

在本系列文章中示例中的内容主要是用[假文生成器](http://en.wikipedia.org/wiki/Lorem_ipsum)生成的[这个地址](http://www.lipsum.com/)可以生成假文。滚动方向仅仅是垂直方向的滚动。为了更形象的理解这个效果，你可以使用你的智能手机上打开[这个示例](http://ariya.github.io/kinetic/1)来体验下这个滚动效果。在Android 4.3上的chrome和firefox、Android2.3(Kindle Fire)、和IOS 6(Mobile Safari)都已经测试通过。

![](http://pic.yupoo.com/reicky_v/DAZtEZeg/FvgDk.png)

既然我们不希望浏览器使用自身的功能来处理用户的手势，那我们就需要阻止它。使用事件监听就可以做到如鼠标和触摸事件，而**tap(触摸)**、**drag(拖拽)**和**release(释放)**这几个事件是实现滚动效果重要的基本事件。

    view = document.getElementById('view');
	if (typeof window.ontouchstart !== 'undefined') {
	    view.addEventListener('touchstart', tap);
	    view.addEventListener('touchmove', drag);
	    view.addEventListener('touchend', release);
	}
	view.addEventListener('mousedown', tap);
	view.addEventListener('mousemove', drag);
	view.addEventListener('mouseup', release);

初始化一些变量和状态也是非常重要的一步。特别需要定义视图滚动具体的距离而添加动力动画效果。一般我们的视图是占据整个屏幕的，我们可以使用**innerHeight**获得视图的高度。在真实的开发实践中，你可能会使用视图区域父元素的高度来确定滚动的触发距离。我们这里还定义了**pressed**这个表示状态的变量，以确定用户是否使用拖拽的手势。

    max = parseInt(getComputedStyle(view).height, 10) - innerHeight;
	offset = min = 0;
	pressed = false;

如果你注意到我们上面的那个demo，你会发现在视图的左边有一个id为**indicator**的滚动条的元素，并不是原生浏览器的滚动条。这个元素需要放在屏幕的最上面或者是最底部，因此我们需要定义一个**relative**变量来确定**indicator**的位置。

    indicator = document.getElementById('indicator');
	relative = (innerHeight - 30) / max;

然后我们会使用到CSS3的[transform](http://dev.w3.org/csswg/css3-transforms/)来移动视图的位置，需要针对不同的浏览器使用正确的样式。我们可以使用更加全面的浏览器侦测方法来侦测浏览器的特性，不过这里我们仅仅这需要下面的几行代码也可搞定这个问题。

    xform = 'transform';
	['webkit', 'Moz', 'O', 'ms'].every(function (prefix) {
	    var e = prefix + 'Transform';
	    if (typeof view.style[e] !== 'undefined') {
	        xform = e;
	        return false;
	    }
	    return true;
	});

在正式处理事件前，先来看看下面两个重要的函数。

当然这里鼠标和触摸事件浏览器都是支持的，下面的**ypos**函数是用来获取事件触发时手指在屏幕上位置的。

    function ypos(e) {
    // touch event
    if (e.targetTouches && (e.targetTouches.length >= 1)) {
        return e.targetTouches[0].clientY;
    }
 
    // mouse event
    return e.clientY;
	}

另外一个重要的函数是**scroll**，它主要是用来处理视图的移动以及**indicator**元素的位置的。注意一下这里我们需要计算滚动触发动力动画效果的值不至于超出我们指定的值之外。

    function scroll(y) {
    offset = (y > max) ? max : (y < min) ? min : y;
    view.style[xform] = 'translateY(' + (-offset) + 'px)';
    indicator.style[xform] = 'translateY(' + (offset * relative) + 'px)';
	}

基本的工作准备好了。而**tap(触摸)**、**drag(拖拽)**和**release(释放)**这几个事件也放入到核心的代码里。一切看起是那么的简洁！

首先来处理**tap**事件，当用户触摸屏幕的时候触发**tap**事件。就需要把**pressed**变量的值设置为true。

当用户释放手指的时候，我们取消对**release**事件的标记。

    function release(e) {
    pressed = false;
    e.preventDefault();
    e.stopPropagation();
    return false;
	}

当用户移动手指的时候，我们需要知道用户手指移动了多少距离。这里我们还设置了一个2px的值用来控制视图的滚动用来阻止滚动抖动的情形。

    function drag(e) {
    var y, delta;
    if (pressed) {
        y = ypos(e);
        delta = reference - y;
        if (delta > 2 || delta < -2) {
            reference = y;
            scroll(offset + delta);
        }
    }
    e.preventDefault();
    e.stopPropagation();
    return false;
	}

详细的代码可以去[这个地址](https://github.com/ariya/kinetic/tree/master/1)查看，总共才80行代码。

是不是有种顿悟想进一步完善滚动代码的感觉？当然我们这里要根据**pressed**的状态来准确的控制**indicator**元素的可见状态。我们可以使用CSS3的[transition](http://dev.w3.org/csswg/css-transitions/)来做这个事情。

当然，我们这里主要是抛砖引玉引发开发思路来编写代码的。性能上并没有考虑很多——如[javascript性能优化](http://ariya.ofilabs.com/2012/12/javascript-performance-analysis-sampling-tracing-and-timing.html)，[GPU compositing](http://ariya.ofilabs.com/2013/06/optimizing-css3-for-gpu-compositing.html)等。在实际项目中我们应该更多从性能上来编写代码以及测试代码。

![](http://pic.yupoo.com/reicky_v/DB0a5qgw/10mZf9.png)

性能怎么样呢？从图中可以看出几乎没有什么开销。我们可以使用chrome的[frame rate HUD](http://ariya.ofilabs.com/2013/03/frame-rate-hud-on-chrome-for-android.html)或者是[painting time](http://ariya.ofilabs.com/2013/08/continuous-painting-mode-in-chrome.html)测试滚动时的性能以及速度等状态。更多的细节可以使用chrome的开发者工具查看。上面这张图是在Nexus4中使用chrome28截的图。可以看到我们的速度是在60fps这个极限值摆动。

先到这里了，下篇文章我们来添加滚动动力学的动画效果。未完待续！

