---
layout: post
title: 贝塞尔曲线在SVG中的应用
categories:
- Life
tags:
- svg
- animation
- 贝塞尔曲线
- 前端

---

上一篇文章讲了讲贝塞尔曲线在动画中的应用，这一篇来讲讲曲线在SVG中的应用。原文地址[Grokking quadratic bézier curves in SVG](http://codepen.io/rachsmith/blog/grokking-quadratic-bezier-curves-in-svg)。主要是二次贝塞尔曲线，至于贝塞尔曲线详细的知识这里就不再阐述来，可以去[这个地址](http://www.html-js.com/article/1628)看看，有详细的介绍。

首先来看一段SVG中的path(路径)代码：

`<path d="M40,170 Q180,40 460,170" />`

这一段代码只是表示一条简单的曲线，跟二次贝塞尔曲线很相似。下面看一个简单的实例，能更加形象的展示这条路径：

<p data-height="268" data-theme-id="0" data-slug-hash="xGVVrm" data-default-tab="result" data-user="rachsmith" class='codepen'>See the Pen <a href='http://codepen.io/rachsmith/pen/xGVVrm/'>The Quadratic Bézier Curve in SVG</a> by Rachel Smith (<a href='http://codepen.io/rachsmith'>@rachsmith</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

二次贝塞尔曲线主要是由在在平面内任选3个不共线的点组成。一个起始点和一个结束点，以及一个控制点。

具体到上面的例子中，这个控制点就对应着SVG坐标系统中的X，Y轴的位置。比如，这里我们创建了一个500px宽和250px高的一个SVG的画布。坐标分别是(40px,170px)、(460px,170px)，而其中的控制点是在(180pxpx,40px)和(250px,40px)这两个坐标点之间有一个动态的变化，从而产生了一个动态的曲线。

那在实际开发中有什么用呢？

当然有用，当我们理解了怎么样使用javascript来控制曲线，我们就可以使用它来创建一些有趣的应用。

接下来就来展示下用它来创建一些有趣的交互动画效果。

可以先去[这个地址](http://panda.network/)体验一下。

<p data-height="268" data-theme-id="0" data-slug-hash="pJbPyN" data-default-tab="result" data-user="rachsmith" class='codepen'>See the Pen <a href='http://codepen.io/rachsmith/pen/pJbPyN/'>Bendy underline effect with SVG</a> by Rachel Smith (<a href='http://codepen.io/rachsmith'>@rachsmith</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

下面来详细的解析下它的原理。

首先，要使我们的path(路径)弯曲。通过上面的分析，可以通过控制第二组的坐标点来实现路径的弯曲。我们使用一个javascript函数来达到这个目的。


```
function updatePath(y) {
    // update SVG path control point
    path.setAttribute('d', 'M10,150 Q200,'+y+' 390,150');
}
```

下面是一个完整的代码，这里使用了tween这个js动画库来使它有一个缓动的动画效果。

<p data-height="268" data-theme-id="0" data-slug-hash="bdeqyb" data-default-tab="result" data-user="rachsmith" class='codepen'>See the Pen <a href='http://codepen.io/rachsmith/pen/bdeqyb/'>A bendy curve</a> by Rachel Smith (<a href='http://codepen.io/rachsmith'>@rachsmith</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

看到了么，曲线动起来了(好像跳绳的效果)。

在上面的第二个demo中，path(路径)有一个交互动画效果。当鼠标滑过path(路径)的时候，它会跟随鼠标滑动的方向，有一个像橡胶皮弯曲弹簧的效果。这是怎么做到的呢？

首先，得给path(路径)绑定一个监听事件，当鼠标滑过它的时候来执行对应的方法。这里要明确一点的是，SVG元素也是一个DOM节点，所以绑定事件就跟正常给DOM元素绑定事件的原理是一样的。

先给path(路径)绑定鼠标的**mouseover**和**mousemove**事件。


```
path.addEventListener('mouseover', function() {
  // if we haven't connected yet and we're not tweening back to center, begin connection
  if (!connected && !tweening) {
    connected = true;
    svgElement.style.cursor = 'pointer';
  }
});

window.addEventListener('mousemove', function(e) {
// storing the y position of the mouse - we want the y pos relative to the SVG container so we'll subtract the container top from clientY.
    mousePos.y = e.clientY - svgTop;
});
```

当鼠标滑过路径的时候，我们通过javascript来更新path(路径)的坐标点，从而实现path(路径)弯曲的效果。当然，为了使之更符合真实世界中物体运动规律，我们会使它有一个缓动点运动效果。


```
function updateCurve() {
  var y = mousePos.y;
  y = mousePos.y - (150-mousePos.y)*1.1;
  path.setAttribute('d', 'M10,150 Q200,'+y+' 390,150');
}
```

同样适用了tween这个js库来实现这个缓动效果。


```
function updateCurve() {
  var y = mousePos.y;
  y = mousePos.y - (150-mousePos.y)*1.1;
  // check if we've reached our threshold
  if (Math.abs(150-y) > 100) {
    connected = false;
    tweening = true;
    svgElement.style.cursor = 'default';
    // snap it back
    snapBack(y);
  } else {
    path.setAttribute('d', 'M10,150 Q200,'+y+' 390,150');
  }
}

function snapBack(y) {
  tween = new TWEEN.Tween({ y: y })
    .to({ y: 150 }, 800)
    .easing( TWEEN.Easing.Elastic.Out )
    .onUpdate( function () {
      updatePath(this.y);
    }).onComplete(function() {
      tweening = false;
    }).start();
}
```

OK，一个简单的交互动画效果就完成了。

<p data-height="268" data-theme-id="0" data-slug-hash="pJbPyN" data-default-tab="result" data-user="rachsmith" class='codepen'>See the Pen <a href='http://codepen.io/rachsmith/pen/pJbPyN/'>Bendy underline effect with SVG</a> by Rachel Smith (<a href='http://codepen.io/rachsmith'>@rachsmith</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

通过上面的几个简单的实例，适用javascript来控制SVG能打造出很有趣的动画效果，CSS3只能望洋兴叹啦。

![](https://s3-us-west-2.amazonaws.com/s.cdpn.io/53148/oprahsvg.gif)







