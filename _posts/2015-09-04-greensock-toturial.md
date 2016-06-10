---
layout: post
title: 使用GSAP(greensock)来实现高性能动画
categories:
- Life
tags:
- css3
- animation
- javascript
- greensock
- Timeline

---

>文章来自于[Simple GreenSock Tutorial – Your first steps with GSAP](https://ihatetomatoes.net/simple-greensock-tutorial-your-first-steps-with-gsap/)有删减。

在web开发中，特别是在移动端的开发中一直有一种观念，即认为CSS动画是唯一高性能的动画方式。从而使很多的开发者几乎很少用javascript来开发动画。一些简单页面的动画效果，使用CSS来实现确实不错，高效节省资源。但如果碰到现如今在朋友圈非常流行有大量交互所谓的h5动画效果页面。还使用CSS来实现的话确实吃力不讨好。

这样会导致两个结果：1、强制使用大量样式来完成复杂的UI交互动画效果(样式表里全是keyframe)；2、如果在PC端的话，基本上只能在webkit内核的浏览器上浏览效果；3、放弃只有javascript才能完成的完美模拟的物理运动效果。

事实上，使用javascript的动画也能喝基于CSS动画一样快，有时候甚至更快。

而使用CSS很难支持复杂的效果，重要的不支持逻辑控制。而使用javascript可以完成复杂的动画效果，并且支持大量的逻辑控制。

今天就来学习下javascript牛逼的GASP(greensock)动画库。它具有以下优点:

1、速度快。GSAP专门优化了动画性能，使之实现和CSS一样的高性能动画效果。
2、轻量与模块化。模块化与插件式的结构保持了核心引擎的轻量，TweenLite包非常小（基本上低于7kb）。GSAP提供了TweenLite, TimelineLite, TimelineMax 和 TweenMax不同功能的动画模块，你可以按需使用。
3、没有依赖。
4、灵活控制。不用受限于线性序列，可以重叠动画序列，你可以通过精确时间控制，灵活地使用最少的代码实现动画。
5、任何对象都可以实现动画。

开始之前先来了解下GSAP动画平台四个插件的不同功能。如下图所示：

![](http://i3.tietuku.com/6f5c0e8054dc8d27.png)

开始之前，先来做些准备工作。我们会基于[codepen](codepen.io)这个平台来编写和运行我们的动画效果。

<p data-height="268" data-theme-id="17491" data-slug-hash="xwbpGw" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/xwbpGw/'>xwbpGw</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

OK，准备工作已做好，下面来让它动起来！

我们这里操作的物体是这个ID为box的盒子。首先把它用一个变量存起来，方便后面来操作。在codepen里的js区域编写下面的代码：

```
var $box = $('#box');

```
### TweenLite.to()方法

下面就是让它动起来，可以使用**TweenLite.to()**方法来使元素动起来。比如，让元素移动到浏览器左边的边缘，我们就可以使用**TweenLite.to()**方法。

下面是**TweenLite.to()**方法的示意图：

![](https://ihatetomatoes.net/wp-content/uploads/2015/08/img_04-greensock-tweenlite-basic1.png)

在codepen中加入下面的代码：

```
TweenLite.to($box, 0.7, {left: 0});
```

上面的代码会在0.7秒之内把**$box**元素从CSS中定义的位置移动到body的边缘。如下所示：

<p data-height="268" data-theme-id="17491" data-slug-hash="meypPB" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/meypPB/'>meypPB</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

不过，你应该发现了一个奇怪的小问题。那就是**$box**元素只有一半露出来了，应该是全部显示的，这是为什么呢？

![](https://ihatetomatoes.net/wp-content/uploads/2015/08/img_box-left1.png)

这是因为我们在CSS中定义了**transform: translate(–50%, –50%)**，这个时候就可以来定义元素的**X**的值。

```
TweenLite.to($box, 0.7, {left: 0, x: 0});
```

当然，你可以来操作大部分的CSS属性来实现动画效果，每个属性使用逗号分隔开。

### TweenLite.from方法

下面来看下**TweenLite.from**这个方法。

在上面的例子上，我们修改代码如下：

```
TweenLite.from($box, 2, {x: '-=200px', autoAlpha: 0});
```

![](https://ihatetomatoes.net/wp-content/uploads/2015/08/img_05-greensock-tweenlite-basic-from.png)

这个方法是用来使元素从定义在**.from()**方法里的位置运动到在CSS中定义的位置。

运行这个例子，我们会看到元素从元素左边**200px**的位置运动到元素在CSS中定义的位置。

**autoAlpha**方法是GSAP中一个特别的属性，它把**opacity**和**visibility**两个属性合二为一了。

![](https://ihatetomatoes.net/wp-content/uploads/2015/08/img_06-greensock-tweenlite-autoalpha.png)

在代码中**autoAlpha: 0**表示它会把元素初始化为**opacity:0;visibility:hidden**。当执行动画效果的时候它会把**visibility**的值设置为**inherit**以及**opacity**值设置为**1**。从而产生一个渐现的效果。

### TweenLite.set()

有时候，我们只是想设置元素的一些CSS属性并不需要动画效果，比如，重设元素的位置。

这个时候就可以使用GreenSock提供的**.set()**方法。

去掉我们前面编写的代码除了定义好的**$box**变量，编写下面的代码：


```
TweenLite.set($box, {x: '-=200px', scale: 0.3});
TweenLite.set($box, {x: '+=100px', scale: 0.6, delay: 1});
TweenLite.set($box, {x: '-50%', scale: 1, delay: 2});

```

![](https://ihatetomatoes.net/wp-content/uploads/2015/08/img_07-greensock-tweenlite-set-relative.png)

运行上面的代码，可以看到元素只是单纯的在改变属性并没有动画效果。

<p data-height="268" data-theme-id="17491" data-slug-hash="ZbYvJE" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/ZbYvJE/'>ZbYvJE</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

在上面的代码中，我们使用**delay**这个属性来制定元素属性改变的延迟时间。

要注意一点的是，在最后一个序列中我们重新设置元素的位置为**x: '-50%'**。

Relative 和 Absolute Values

在上面的代码中，前面两个序列我们定义了**x**属性相对的补间值。

第一个序列把**$box**元素从CSS中的位置移动到元素左边的200px的位置，然后在第二序列中把元素的位置移动到100px的位置，最后移动到**-50%**的绝对的补间值。跟默认在CSS中定义的位置一样。

### TweenLite.fromTo()方法

最后来说一说**TweenLite.fromTo**这个方法。

顾名思义，这个方法我们可以定义元素的起始位置：


```
TweenLite.fromTo($box, 2, {x: '-=200px'}, {x: 150});
```

把上面的代码放入到codepen中，就可以看到运行的动画效果。

![](https://ihatetomatoes.net/wp-content/uploads/2015/08/img_08-greensock-tweenlite-fromto1.png)

我们定义了元素从左边**200px**的位置开始运动到指定的位置。

**x:150**会覆盖在CSS中定义的**transform: translate(–50%, –50%)**的样式，用新的**transform: matrix(1, 0, 0, 1, 150, -50);**样式来代替。

### GreenSock提供的运动曲线

为了使动画效果更有趣，符合真实的物体运动效果。运动曲线就能做到这些，GreenSock也提供了各种的运动曲线。

**TweenLite**提供下面这些运动曲线：

1、Power0 相当于 Linear
2、power1 相当于 Quad
3、Power2 相当于 Cubic
4、Power3 相当于 Quart
5、Power4 相当于 Quint

你如果想要更多的运动曲线，可以添加GreenSock EasePack这个插件。


```
https://cdnjs.cloudflare.com/ajax/libs/gsap/1.17.0/easing/EasePack.min.js

```

如果你使用**TweenMax**的话，它已经包含了**EasePack**。

**EasePack**包含下面的这些运动曲线：

1、Back
2、SlowMo
3、StppedEase
4、RoughEase
5、Bounce
6、Circ
7、Elastic
8、Expo
9、Sine

下面来添加**ease:Power4.easeInOut**来看看实际的效果。


```
TweenLite.fromTo($box, 2, {x: '-=200px'}, {x: 150, ease:Power4.easeInOut});

```

试着添加下面的代码，看看有什么有趣的效果发生？


```
TweenLite.to($box, 0.4, {top: '100%', y: '-100%', ease:Bounce.easeOut, delay: 2});
TweenLite.to($box, 0.7, {x: '-=200px', y: '-100%', ease:Back.easeInOut, delay: 3});
TweenLite.to($box, 0.8, {x: '-=200px', y: '-100%', ease:Back.easeInOut, delay: 4.2});
TweenLite.to($box, 2.5, {top: '50%', y: '-50%', ease:Power0.easeNone, delay: 5});
TweenLite.to($box, 2.5, {x: '+=400px', ease:Elastic.easeInOut, delay: 7.7});
TweenLite.to($box, 2.5, {x: '-=400px', rotation: -720, ease: SlowMo.ease.config(0.1, 0.7, false), delay: 10.4});

```

使用运动曲线能使我们制作的动画效果更加有趣。

具体各种运动曲线的效果可以去这个[地址](http://greensock.com/ease-visualizer)看看。

### 回调函数

GreenSock提供了丰富的回调函数来操作动画效果。

这里以**.fromTo()**方法来说明它的用法。

比如，我们想要在动画开始的时候来触发回调函数。首先来创建一个**start**的函数：


```
function start(){
  console.log('start');
}
```

触发回调函数，只需要添加这句代码就可以了**onStart:start**就可以了，非常简单吧。


```
TweenLite.fromTo($box, 2, {x: '-=200px'}, {x: 150, ease:Power4.easeInOut, onStart: start});

```

打开开发者工具，就可以看到输出的相关信息。

你也可以添加**onUpdate**和**onComplete**来触发对应的回调函数：

```
function start(){
  console.log('start');
}
function update(){
  console.log('animating');
}
function complete(){
  console.log('end');
}
```

![](https://ihatetomatoes.net/wp-content/uploads/2015/08/img_10-greensock-tweenlite-callbacks.png)

**onUpdate**会在动画的每一帧触发；**onComplete**会在动画结束的时候触发。

看看最后的效果，点击**RETURN**按钮就可以看到效果。

<p data-height="268" data-theme-id="17491" data-slug-hash="ZbYvVQ" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/ZbYvVQ/'>GSAP - TweenLite demo - Final</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

下面来一些好的tips:

1、任何的CSS属性需要从有**－**的写法变为驼峰式的写法。比如**background-color**修改为**backgroundColor**等。

2、CSS中的**transform:rotate()**变为**rotation**。

3、另外在GSAP中的2Dtransform－**scaleX**, **scaleY**, **scale**, **skewX**, **skewY**,**x**, **y**, **xPercent**,和 **yPercent** 的使用方法可以去[这个视频](https://www.youtube.com/watch?v=J5twQLXJ-vQ)看看。

4、如果使用**SublimeText**来作为开发工具，可以下载[GSAP这个代码片段](http://greensock.com/forums/topic/11071-sublimetext-snippets/)。

5、如果你使用[JSHint]()和[JSLint]()作为代码质量检测工具，可以去这看看它在**GSAP**中的[使用方法](http://greensock.com/forums/topic/11369-gsap-globals-for-jshint/)。

先到这里啦，遇到问题随时查看GreenSock的[文档](http://greensock.com/docs/#/HTML5/GSAP/TweenLite/)。

另外推荐一些有用的学习资源：

[Jump Start: GSAP JS](https://greensock.com/jump-start-js)

[Getting Started Guide](http://greensock.com/get-started-js)

[GSAP Forum](http://greensock.com/forums/forum/11-gsap/)

[GreenSock course at Noble Desktop in New York](https://nobledesktop.com/classes/greensock)

[GreenSock course workbook](https://nobledesktop.com/books/gsap)

[GreenSock Workshop](https://ihatetomatoes.net/product/greensock-workshop/)

















