---
layout: post
title: 	用javascript打造优美的动态滚动效果(2)
categories:
- Life
tags:
- javascript
- 翻译
---

上面一篇文章我们简单地编写了使用javascript在移动设备模拟原生浏览器滚动条的效果但是没有一些动力的动画效果，这篇文章我们来添加一个动力的动画效果简单地理解就是跟踪用户手指拖拽的运动轨迹然后以一定的速度运动到运动到用户拖拽停止的位置。

第一部分基本上实现了拖拽的技术的准备。在我们编写动画效果前，让我们来看看用户在发生触摸、拖拽和释放他们手指的时候这个过程之间都发生了一些什么。

![](http://pic.yupoo.com/reicky_v/DB8qdzRP/medium.jpg)

上图的阴影区域是表示用户正在控制视图的一个过程，如手指触摸视图并拖动最后释放手指。当用户最后释放控制的时候，视图就会开始进入自动滚动的模式滚动到用户释放手指的位置。在这个滚动过程中，视图会以某个速度符合动力学的原理滚动目标区域(所谓动力学原理就是模拟自然中运动的原理，要考虑摩擦力一个物体运动的时候如果没有外力介入的时候就是先快后慢。)

说了一大堆有点抽象，看下面这张图就可以明白是怎么回事了，可以去[这个地址](http://ariya.github.io/kinetic/2/)看看实际效果。

![](http://pic.yupoo.com/reicky_v/DB8z59Ht/9Fn8F.gif)

你可以看到滚动的动画效果是一个由快到慢逐渐停止的效果。

那怎么去定义这个滚动的速度呢？一个简单的方法是，就是使用定时器来做到。当触发定时器的时候，根据这个定时器来计算速度。按照以往的开发经验来说一般定时器的时长10秒就可以了。

这个定时器应该是在用户触摸屏幕的时候就触发它。我们这里是定义了一个**track()**函数来定义定时器以及计算速度的。

    function tap(e) {
    pressed = true;
    reference = ypos(e);
 
    velocity = amplitude = 0;
    frame = offset;
    timestamp = Date.now();
    clearInterval(ticker);
    ticker = setInterval(track, 100);
 
    e.preventDefault();
    e.stopPropagation();
    return false;
	}

这个track()方法非常简单。它做的事情就是根据用户触发位置和释放手指的位置来计算视图滚动的速度的。变量**v**表示运动的速率。最后的一个velocity表示[moving average filter](https://en.wikipedia.org/wiki/Moving_average)即移动平均线是为了防止一些意外的情况的出现可以使用velocity这个值来表示运动的速率。

**track()**方法是在用户触摸拖动的时候触发的。当用户释放手指的时候，这里的情形就有点点复杂了，我们需要计算这个时候视图滚动的速率，因为当用户释放的时候也就是视图快要到达目标点了，这个时候我们需要计算再一次计算速度的速率实现符合动力学的运动功能。如下所示：

    function release(e) {
    pressed = false;
 
    clearInterval(ticker);
    if (velocity > 10 || velocity < -10) {
        amplitude = 0.8 * velocity;
        target = Math.round(offset + amplitude);
        timestamp = Date.now();
        requestAnimationFrame(autoScroll);
    }
 
    e.preventDefault();
    e.stopPropagation();
    return false;
	}

上面的这个**release**方法是当用户释放手指来触发的，跟上一篇文章的[代码](http://ariya.ofilabs.com/2013/08/javascript-kinetic-scrolling-part-1.html)相比，我们这里多了一个自动滚动的方法**autoAcroll**。这里定义了滚动的速度是10px/s是为了防止滚动速度过低的情形。而当用户释放手指的时候需要重新据算一下运动速率，我们这里是使用**amplitude**变量来表示的。我们这里使用了0.8这个常量来作为速率的一个参数，如果你想让列表的滚动效果更加平滑和自然可以增大速率的0.8这个参数，这里我们会用到[requestAnimationFrame](http://www.html5rocks.com/en/tutorials/speed/animations/)这个新的API来执行动画效果。

一般来说像列表这样的滚动的动画效果大多数都是采取指数衰减的形式来运动的[exponential decay](http://en.wikipedia.org/wiki/Exponential_decay)。(译者注：这个运动的原理涉及比较多的专业术语对于我来说理解起来确实有点困难，要翻译好确实很难只能是尽最大可能按照原文的意思来翻译)。其实这个指数衰减原理就是一个弹簧质点系统即[spring-mass system](http://en.wikipedia.org/wiki/Spring%E2%80%93mass_system#Spring.2Fmass_system)。幸运的是它有个标准的[公式](http://en.wikipedia.org/wiki/Exponential_decay#Solution_of_the_differential_equation)如下图所示：(ŷ表示目标位置，A就是我们上面说过的amplitude，而t表示目前的时间，T表示一个常量)。

![](http://pic.yupoo.com/reicky_v/DB99n9b8/DKxLF.png)

上面说了这么些个原理公式什么，相信你和我一样头都晕了其实我们只要会用那个公式就行了。这个自动滚动方法每次滚动的距离是0.5px，并会多次执行它。实际上如果仔细看看代码的话，它实现的效果其实就是非常有名的减速运动效果，如CSS3中animation中的**ease-out**有点像。

    function autoScroll() {
    var elapsed, delta;
    if (amplitude) {
        elapsed = Date.now() - timestamp;
        delta = -amplitude * Math.exp(-elapsed / timeConstant);
        if (delta > 0.5 || delta < -0.5) {
            scroll(target + delta);
            requestAnimationFrame(autoScroll);
        } else {
            scroll(target);
        }
    }
	}

**timeConstant**这个常量的值非常重要，如果你想逼真的模仿IOS的滚动效果的话[decelerationRate normal](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UIScrollView_Class/Reference/UIScrollView.html#/apple_ref/doc/c_ref/UIScrollViewDecelerationRateNormal)，苹果官方推荐的值是325即325ms。对数学知识有一定了解的话，你可能会了解对于动画来说16.7ms是一个不错运动速率，对应到上面的公式就是**Math.exp(-16.7 / 325)**。

我们可以使用google来搜索[https://www.google.com/search?q=plot+150*(1-exp(-t%2F325))&oq=plot](https://www.google.com/search?q=plot+150*(1-exp(-t%2F325))&oq=plot)。它会返回一张图标给你非常形象的展示这个公式所代表的具体的意义。

![](http://pic.yupoo.com/reicky_v/DB9mt3KB/Af6c1.png)

这种自然的滚动按照动力学的原理它总会停止下来，这也是我们设置了速率这个变量的原因。按照我们上面的原理，物体在按照一定速度运动在指定的时间里滚动到目标位置，我们可以自由的设置运动时间这个常量按照自己的需求来决定动画的效果。

剩下的代码跟第一篇文章中的[代码](http://ariya.ofilabs.com/2013/08/javascript-kinetic-scrolling-part-1.html)差不多。

最终的代码130行左右，可以去[这个地址](https://github.com/ariya/kinetic/)详细查看。在安卓4.3上的chrome、safari、Android2.3(Kindle Fire)和IOS6以上测试通过。就像在第一篇中说的那样我这个代码仅仅是一个示例，代码在可读性和性能上还有很多值得优化的地方，这里只是一个抛砖引玉的作用。这也不是说我这个代码运行很慢。至少在Nexus4和Nexus7上即使是在60fps的情况下也没有什么问题。你可以使用chrome的开发者工具来查看性能当然[ painting time HUD](http://ariya.ofilabs.com/2013/08/continuous-painting-mode-in-chrome.html)也非常不错。

未完待续。