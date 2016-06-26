---
layout: post
title: 运用迪斯尼动画原则来实现一个加载动画效果
categories:
- Life
tags:
- css3
- animation
- 前端

---

作为动画电影制作领域的开拓者和领导者，迪斯尼为我们奉献了许许多多的精彩动画片。与此同时，迪斯尼在动画设计方面也有它们的一些原则。其中比较出名的是，制作动画的十二原则。这些原则描述了动画能怎样用于让观众相信自己沉浸在现实世界中。

同样这些原则也适用于web动画中，使用这些原则来设计我们的动画，可以使动画更加符合真实世界中的物体的运动规律，使用户有一种沉浸在现实世界中的感觉。

至于具体的十二原则可以去[这里](https://cssanimation.rocks/principles/)看看。这里有[中文版](http://www.jianshu.com/p/1858a8733ba3)。

今天我们主要是运用其中的一条原则即挤压和拉伸来实现一个加载的动画效果。

动画原则之一。

### 挤压和拉伸

挤压和拉伸说的是这是物体存在质量且运动时质量保持不变的概念。当一个球在弹跳时，碰击到地面会变扁，恢复的时间会越来越短。

创建对象的时候最有用的方法是参照实物，比如人、时钟和弹性球。

当它和网页元件一起工作时可能会忽略这个原则。DOM 对象不一定和实物相关，它会按需要在屏幕上缩放。例如，一个按钮会变大并变成一个信息框，或者错误信息会出现和消失。

尽管如此，挤压和伸缩效果可以为一个对象增加实物的感觉。甚至一些形状上的小变化就可以创造出细微但抢眼的效果。

首先来看下实际的效果的吧。

<p data-height="387" data-theme-id="0" data-slug-hash="vORBxj" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/vORBxj/'>#1: CSS3 Loading Animations (complete)</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

可以看到上面的效果中，球形在往下掉碰到地面的时候会有一个挤压的效果。这样的动画效果也是符合真实世界中物体运动规律。想象一下当我们把一个球从高处往下抛的时候，在碰到地面的一瞬间它会有一个挤压变形的状态；而当它弹起的时候又会恢复到正常的形状。

那么这个效果用CSS怎么来实现呢？下面一步一步来拆解。

首先是基本的结构和样式，如下所示：

<p data-height="389" data-theme-id="0" data-slug-hash="bdvbRy" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/bdvbRy/'>#1: CSS3 Loading Animations (base)</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

在上面的效果中，有一个加载过程中文字后面点点点的动画。这个就不详细说了，要提一点的是，上面的样式中使用来属性选择器。


```
span[class^='dot_'] {
  opacity: 0;
}

```

在上面的例子中，为了在三个点初始化的时候透明度为0。我们使用了属性选择器来选择所有类名以**dot**打头的DOM节点的透明度为0。属性选择器使用起来还是很方便的，浏览器的支持也非常好，具体支持可以去[这里](http://caniuse.com/#feat=css-sel3)看看。

接下来就是这个球的动画效果了。上面也说过当一个球在弹跳时，碰击到地面会变扁，恢复的时间会越来越短。

具体到我们这个例子，就是当球碰到地面的时候，球有一个挤压变形的状态。那该怎么实现呢？其实我们可以在球碰到地面的时候改变下球形的宽高就可以了。

使用CSS3中的keyframe很容易实现这样的效果。


```
@keyframes bounce {
  0% {
    top: 0;
    animation-timing-function: ease-in;
  }
  34% {
    height: 4rem;
    width: 4rem;
  }
  35% {
    top: 8rem;
    height: 3rem;
    width: 4.3rem;
    animation-timing-function: ease-out;
  }
  45% {
    height: 4rem;
    width: 4rem;
  }
  90% {
    top: 0;
  }
  100% {
    top: 0;
  }
}

```

从上面的代码中可以看到，在35%这一帧的时候改变球形的top值的时候，同时也改变了球形的宽高。这样就可以在球形执行这一帧的时候，有一个在掉下碰到地面的时候有一个挤压变形的效果。而在接下来的一帧中恢复球形原来的宽高，这样就实现了球形在弹起的时候又会恢复到正常的形状。从而使这一个动画效果更加符合真实世界中的物体运动效果。

当我们在设计这些动画效果的时候，如果能很好的运用迪斯尼提到到12动画原则。
使动画效果更加符合真实世界中的物体运动规律，从而带给用户身临其境的感受。

<p data-height="387" data-theme-id="0" data-slug-hash="vORBxj" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/vORBxj/'>#1: CSS3 Loading Animations (complete)</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>








