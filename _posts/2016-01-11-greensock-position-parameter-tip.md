---
layout: post
title: 深入理解GreenSock中的position参数的使用方法
categories:
- Life
tags:
- css3
- animation
- javascript
- greensock
- Timeline

---

> 在前面文章有介绍过GreenSock这个动画平台的使用方法，可以先去看看[这里](http://janily.gitcafe.io/life/2015/09/04/greensock-toturial/)。今天来学习下GreenSock动画平台中一个重要的参数即**position**。理解了它，我们就能更加高效的使用GreenSock制作出精彩的动画。同样文章来自GreenSock官方网站，[地址](https://greensock.com/position-parameter)。官方有更加容易理解的交互可视化实例。

### 在TimelineLite.to()方法中使用position

这篇文章我们主要是以**TimelineLite.to()**这个方法来说明**position**的使用方法，在其他诸如**from()**,**fromTo()**,**add()**等方法中同样适用。需要注意的是**position**参数需要跟在**vars**参数后面:

```
.to(target,duration,vars,position)

```

其实**position**的作用就是jquery中的链式操作一样，只不过它是用来实现链式动画的，默认值是"+=0"表示在动画结束的时候进行下一个操作，即**timeline.to(...).to(...)**。适用它的优点是我们不需要一个一个像上面这样去写链式动画了，但是如果我们想更加精细的控制动画的运行，比如各个动画之间的延迟等，怎么办呢？没问题，都可以使用**position**参数来做。

### 定义position的多种行为

**position**参数有多种行为可以定义，从而可以产生不同的效果

定义绝对的时间值(1)

相对于相对于上一个动画来增加或者是减少下一个动画开始的时间

定义一个动画时间占位标签

相对于一个占位标签来增加或者是减少时间

### 基本的代码示例


```
tl.to(element, 1, {x:200})
  //在第一个动画结束的时间轴上增加1秒的空隙时间，即在第一个动画结束后，再等一秒才开始下一个动画，而不是马上开始下一个动画
  .to(element, 1, {y:200}, "+=1")
  //在上一个动画运行到最后0.5秒的时候，开始下一个动画
  .to(element, 1, {rotation:360}, "-=0.5")
  //在动画运行6秒后，才开始运行下面这个动画
  .to(element, 1, {scale:4}, 6);

```

下面我们再来看使用**position**中的标签来达到同样的效果


```
//在动画时间轴上的2秒出添加一个标签
tl.add("scene1", 2)
  //在动画中的时间轴上插入一个标签
  .to(element, 4, {x:200}, "scene1")
  //在上面的标签的基础上再插入3秒的时间空隙
  .to(element, 1, {opacity:0}, "scene1+=3");


```

上面的解释可能还不是很清楚，正所谓，talk is cheap,show me the code。说的就是这个道理，一行代码胜过千言万语。下面通过一些实际的交互示例来解释下**position**的实际用法。

### 先来看看没有使用position参数时的效果


```
//所有的动画效果是一个接着一个运行的，即链式动画
var tl = new TimelineLite();
tl.to(".green", 1, {x:750})
  .to(".blue", 1, {x:750})
  .to(".orange", 1, {x:750})

```

<p data-height="268" data-theme-id="17491" data-slug-hash="jWwBaE" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/jWwBaE/'>jWwBaE</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

从上面的实际运行效果，可以看到动画是一个接着一个运行的。

下面来看下使用**position**参数，来改变动画的开始的时间。

### 使用position来使相对于上一个动画动画提前或者是延迟开始运行

使用**position**，我们可以控制在时间轴上的动画开始运行的时间，即相对于上一个动画提前或者是延迟开始运行。使用**+=**来表示延迟动画的开始。

还是来修改上一个动画的代码。


```
var tl = new TimelineLite();
tl.to(".green", 1, {x:750})
	//在上一个动画执行完后，增加一秒的空隙即延迟一秒开始运动
  .to(".blue", 1, {x:750},"+=1")
  //同样在上一个动画执行完后，增加一秒的空隙即延迟一秒开始运动
  .to(".orange", 1, {x:750},"+=1")

```

<p data-height="268" data-theme-id="17491" data-slug-hash="vLZxjV" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/vLZxjV/'>vLZxjV</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

### 提前开始运行动画

如果使用**-=1**的话，你就是表示相对于上一个动画提前开始执行动画。

修改代码如下：


```
//提前开始执行动画
var tl = new TimelineLite();
tl.to("#green", 2, {x:750})
   // 在上一个动画开始一秒后，开始运行动画
  .to("#blue", 2, {x:750}, "-=1")
   // 同样在上一个动画开始一秒后，开始运行动画
  .to("#orange", 2, {x:750}, "-=1");

```

<p data-height="268" data-theme-id="17491" data-slug-hash="qbjrMo" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/qbjrMo/'>qbjrMo</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

### 绝对时间的使用

如果想强制制定动画在某一个时间提前或者是延迟同时开始执行，那么绝对时间就可以派上用场啦。

修改代码如下：


```
//在指定时间同时开始
var tl = new TimelineLite();
tl.to("#green", 4, {x:750})
  //在第一个动画开始一秒后开始运行
  .to("#blue", 2, {x:750}, 1)
  //在第一个动画开始一秒后开始运行
  .to("#orange", 2, {x:750}, 1);

```

### 占位标签

占位标签标示在动画的时间轴上添加一个时间做为占位标签，当其它动画引用啦这个占位 标签的时候，就在这个标签表示的时间提前或者是延迟开始运行动画。talk is cheap,show me the code!


```
//占位标签的使用方法
var tl = new TimelineLite();
tl.to("#green", 1, {x:750})
  //在上一个动画运行后添加一个blueGreenSpin的时间占位标签，时间为1秒即上一个动画结束后隔一秒后开始动画
  .add("blueGreenSpin", "+=1")
  //引用blueGreenSpin占位标签
  .to("#blue", 2, {x:750, rotation:360}, "blueGreenSpin")
  //在blueGreenSpin占位标签的时间基础上增加0.5秒即在第一个动画结束后隔1.5秒开始动画
  .to("#orange", 2, {x:750, rotation:360}, "blueGreenSpin+=0.5");

```

<p data-height="268" data-theme-id="17491" data-slug-hash="XXgMvz" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/XXgMvz/'>XXgMvz</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

OK，通过上面的几个实例，相信你已经深入了解啦**position**参数的使用方法和强大的威力。虽然只是介绍了在**to()**方法中的使用方法。其实在 from(), fromTo(), staggerFrom(), staggerFromTo(), staggerTo(),add(), call(), 和 addPause()方法中**position**的用法差不多一样。

熟能生巧！！！







