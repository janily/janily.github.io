---
layout: post
title: web animations api学习系列(3)－使用web animations来控制动画
categories:
- Life
tags:
- web animations
- 动画
- javascript
- 前端

---

接着上篇文章，这篇文章来讲一讲使用web animations的API对动画进行诸如暂停、播放等状态的控制。

### 控制动画的状态

当你调用元素的**animate()**方法的时候，一个动画对象就会开始播放动画。那它会经过几个阶段呢？你可以使用**playState**来检测动画目前处于哪个状态。我们可以使用下面的代码来检测：


```
ar player = element.animate(/* ... */);
console.log(player.playState); //"running"

player.pause(); //"暂停"
player.play();  //"运行"
player.cancel(); //"取消"
player.finish(); //"完成"
```

下面来看一个实例，下面的实例中有很多圆圈在不断进行放大缩小的动画。你可以通过按钮来控制每一个圆圈的动画状态。

<p data-height="464" data-theme-id="17491" data-slug-hash="WvXRYg" data-default-tab="result" data-user="danwilson" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/danwilson/pen/WvXRYg/">Blob That Walks</a> by Dan Wilson (<a href="http://codepen.io/danwilson">@danwilson</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

### 控制动画运行速度

在上一个实例中，有一个**2x**按钮，它可以用来加快动画的运行速度，即两倍的速度来运行动画。它可以通过**playbackRate**属性来控制。


```
var player = element.animate(/* ... */);
console.log(player.playbackRate); //1

player.playbackRate = 2; //两倍速度
```

### 动画完成回调

在CSS中，transitions以及animation都有一个**end**事件，可以用来监听动画完成的事件。同样的，**Animation**也有**onfinish()**方法来监听动画完成事件。需要注意的是，如果一个动画师无线播放的即**infinite**，那么它不会用完成事件。当然，规范规定**animation**同时必须有一个**oncancle**事件来取消动画。

下面再来看一个使用**onfinish**事件的实例，是一个倒计时的实例：

<p data-height="326" data-theme-id="17491" data-slug-hash="RPMVZJ" data-default-tab="result" data-user="danwilson" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/danwilson/pen/RPMVZJ/">Timer Countdown</a> by Dan Wilson (<a href="http://codepen.io/danwilson">@danwilson</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

### 时间轴

每一个动画对象都提供了读写的两个时间属性－**currentTime**何**startTime**。现在我们来谈谈**currentTime**这个属性，返回当前动画运行的时间。

**currentTime**会返回当前动画所在的时间，单位为毫秒。最大值为delay + (duration * iterations)。

如下代码所示：


```
var player = element.animate([
  {opacity: 1}, {opacity: 0}
], {
  duration: 1000,
  delay: 500,
  iterations: 3
});

player.onfinish = function() {
  console.log(player.currentTime); // 3500
};
```

播放速度会影响到播放时间轴的速度。如果把播放速度设置为10倍，而**currentTime**仍然保持不变，你会发现在同样的时间里，播放速度快的动画的时间轴也会更快。其实在上一个倒计时的例子中就出现过。

由于currentTime是可写的，因此我们可以通过指定某个点来查看动画效果，它也同样可以让我们来保持两个动画的同步，如下面这个例子：

<p data-height="300" data-theme-id="17491" data-slug-hash="YXYWKK" data-default-tab="result" data-user="danwilson" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/danwilson/pen/YXYWKK/">Syncing Timelines - WAAPI</a> by Dan Wilson (<a href="http://codepen.io/danwilson">@danwilson</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

### 反向运行动画

所谓反向运行动画，即从动画的结束状态开始运行回滚到动画的开始状态，不过当反响运行动画结束的时候，**currentTime**的值将会是0.

<p data-height="300" data-theme-id="17491" data-slug-hash="waRKOm" data-default-tab="result" data-user="danwilson" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/danwilson/pen/waRKOm/">waRKOm</a> by Dan Wilson (<a href="http://codepen.io/danwilson">@danwilson</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

OK，这篇文章，我们了解了动画各种状态的控制方法，不过这显然不够，下篇再来更深入的介绍下其它的应用。








