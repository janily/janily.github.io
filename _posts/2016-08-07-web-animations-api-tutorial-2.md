---
layout: post
title: web animations api学习系列(2)－使用web animations编写简单的动画效果
categories:
- Life
tags:
- web animations
- 动画
- javascript
- 前端

---

上一篇文章对web animations做了一个简单的介绍，这篇文章我们来介绍下使用web animations编写一个简单的动画效果。

### 编写简单的帧动画

如果你使用CSS的keyframe写过动画效果，那对使用web animations写的代码应该不会陌生：


```
var player = document.getElementById('toAnimate').animate([
    { transform: 'scale(1)', opacity: 1, offset: 0 },
    { transform: 'scale(.5)', opacity: .5, offset: .3 },
    { transform: 'scale(.667)', opacity: .667, offset: .7875 },
    { transform: 'scale(.6)', opacity: .6, offset: 1 }
  ], {
    duration: 700, //milliseconds
    easing: 'ease-in-out', //'linear', a bezier curve, etc.
    delay: 10, //milliseconds
    iterations: Infinity, //or a number
    direction: 'alternate', //'normal', 'reverse', etc.
    fill: 'forwards' //'backwards', 'both', 'none', 'auto'
  });
```

是不是跟CSS的帧动画的代码非常像：


```
@keyframes emphasis {
  0% {
    transform: scale(1); 
    opacity: 1; 
  }
  30% {
    transform: scale(.5); 
    opacity: .5; 
  }
  78.75% {
    transform: scale(.667); 
    opacity: .667; 
  }
  100% {
    transform: scale(.6);
    opacity: .6; 
  }
}
#toAnimate {
  animation: emphasis 700ms ease-in-out 10ms infinite alternate forwards;
}
```

下面我们来解释下代码：

`var player = document.getElementById('toAnimate').animate()`

首先，我们需要找到我们要动画的元素，这里我使用**getElementById**方法来找到要动画的元素并且使用一个变量存储起来，便于后面使用。然后调用**animate**方法。当然对于这个方法目前浏览器支持不是很好，你可能需要一个[polyfill](https://github.com/web-animations/web-animations-js)来处理浏览器的兼容问题。

![](http://ooo.0o0.ooo/2016/08/07/57a6df957a73b.png)

**animate**方法需要传入两个参数，分别是**KeyframeEffects**和**AnamationEffectTimingProperties**。第一个参数**KeyframeEffects**对应着CSS动画中的**@keyframes**即动画的关键帧，第二个参数**AnamationEffectTimingProperties**对应着CSS动画中控制动画的各种属性**animation-**(比如动画时间，缓动曲线等等)。使用js来控制动画的一个优势是，我们可以把参数定义在变量中，在其它需要用到的地方可以反复引用，而使用CSS动画则无法做到这一点。


```
var player = document.getElementById('toAnimate').animate([
    { transform: 'scale(1)', opacity: 1 },
    { transform: 'scale(.5)', opacity: .5 },
    { transform: 'scale(.667)', opacity: .667 },
    { transform: 'scale(.6)', opacity: .6 }
  ]);
```

在上面的代码中，在每一帧中我们改变了CSS的相关属性，偏移值是0到1，对应着CSS动画中的百分比差不多。它是可选的，如果你没有指定任何值，它们就会平均分布（比如你有三个，第一个的偏移量为0，第二个的偏移量为.5，第三个则为1）。当然，你也可以像在CSS中编写动画那样，使用诸如**animation-timing-function**这些属性来指定相关的值来控制动画。每个属性的值都应该和你在JavaScript中使用element.style指定的相匹配，即opacity的值应该是一个数字，而transform应该是字符串。


```
var player = document.getElementById('toAnimate').animate([], {
    duration: 700, //milliseconds
    easing: 'ease-in-out', //'linear', a bezier curve, etc.
    delay: 10, //milliseconds
    iterations: Infinity, //or a number
    direction: 'alternate', //'normal', 'reverse', etc.
    fill: 'forwards' //'backwards', 'both', 'none', 'auto'
  });
```

是不是很熟悉，跟CSS动画中的方法都差不多。

下面来使用**web animations**来编写一个基本的动画效果，这里使用了**polyfill**来处理浏览器的兼容性。灰色的方块是使用了**web animations api**来编写的，而红色方块是使用CSS来编写的。

<p data-height="300" data-theme-id="17491" data-slug-hash="QwrZwd" data-default-tab="result" data-user="danwilson" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/danwilson/pen/QwrZwd/">CSS Keyframes v. WAAPI</a> by Dan Wilson (<a href="http://codepen.io/danwilson">@danwilson</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

这篇文章简单的介绍了**web animations api**使用方法，下一篇再来介绍下诸如**暂停**、**播放**等方法的使用。

