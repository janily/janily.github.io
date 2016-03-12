---
layout: post
title: web图形技术canvas之旅-canvas高级绘图技巧
categories:
- Life
tags:
- html5
- canvas
- javascript

---

上篇文章简单的介绍了canvas的一些基本的知识，这篇文章来介绍下canvas一些高级的绘图知识。

首先准备一个基本的html模板文件，我们下面的编码都是基于这个模板文件来进行的。代码如下：


```
<!DOCTYPE html>
 
<html>
    <head>
        <title>Canvas from scratch</title>
        <meta charset="utf-8">
 
        <script src="http://ajax.googleapis.com/ajax/libs/jquery/1/jquery.min.js"></script>
 
        <script>
            $(document).ready(function() {
                var canvas = document.getElementById("myCanvas");
                var ctx = canvas.getContext("2d");
            });
        </script>
    </head>
 
    <body>
        <canvas id="myCanvas" width="500" height="500">
            <!-- Insert fallback content here -->
        </canvas>
    </body>
</html>
```

上面的代码是一个基础的骨架。下面就开始吧！

###绘制圆形

在上一篇文章中我们知道了怎么去绘制一些基本的图形和路径；在这篇文章，我们来学习下绘制一些相对复杂的图形，首先是圆形。

在canvas中的接口中并没有提供一行代码绘制圆形的方法，比如像绘制矩形一样**fillRect**一行代码就能解决。要绘制圆形得使用到**arc**即弧形，圆形就是一个360度的弧形。或者画一个半圆，使用**arc**方法也很容易办到。所以要绘制一个圆的话还是要花点代码的。

这里来演示下使用**arc**方法来画一个圆，如下代码所示：


```
cxt.beginPath();
ctx.arc(100, 100, 50, 0, Math.PI*2, false);
ctx.closePath();
ctx.fill();
```

上面的代码就会画一个圆。

<p data-height="268" data-theme-id="0" data-slug-hash="OVBOrw" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/OVBOrw/'>OVBOrw</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

看起来好像很简单，但是你真的了解每一行代码标示的意思么。下面就逐个逐个来解释下：

第一个参数是X轴的位置即圆心相对于画布原点的位置。

第二个参数是Y轴的位置。

第三个参数是圆形的半径

第四个参数是起始角，以弧度计。（弧的圆形的三点钟位置是 0 度）。

第五个参数是结束角，以弧度计。

第六个参数是可选。规定应该逆时针还是顺时针绘图。False = 顺时针，true = 逆时针。

总结起来**arc**方法语法如下所示：


```
arc(x, y, radius, startAngle, endAngle, anticlockwise);
```

现在我们来解释下绘制圆形具体发生了什么，首先圆是一个360度的弧形。在画布中，弧形是定义为一条从起始角开始绘制到结束角结束的一条曲线。

听起来好像听拗口的，下面上一张图就清楚了：

![](https://cdn.tutsplus.com/net/uploads/legacy/939_canvas2/2.jpg)

上面这一张图一看便知绘制圆形的原理，果然是一图胜千言。

###canvas中的角度

在canvas中，角度并不是我们通常意义上所了解的角度，而是用弧度来表示的。比如，一个圆是360°，也就是2π弧度，以圆心为坐标原点，开始计算起始弧度与终止弧度，即在圆心右侧为0，左侧为π，上方为3/2π。顺时针还是逆时针就是画线的方向了，比如0到π/2，顺时针就是四分之一个圆，逆时针就是四分之三个圆。并且角度是从右边开始的。还是直接看图吧：

![](https://cdn.tutsplus.com/net/uploads/legacy/939_canvas2/3.jpg)

如果你真不是很适应这个弧度的概念，我们仍然可以使用角度来代替弧度，只是要花点代码把这个角度转换为弧度：


```
var degrees = 270;
var radians = degrees * (Math.PI / 180);
```

上面这个简单的公式就是角度与弧度的转换公式。

###贝塞尔曲线路径

**arc**方法说了这么多，不过对于要绘制其它类型曲线的时候还是有很多的限制。比如，稍微复杂点的曲线，**贝塞尔曲线**，**二次方贝塞尔曲线**这个**arc**方法就无能为力了。这个时候就可以使用到canvas提供的**bezierCurveTo**和**quadraticCurveTo**这两个专门绘制曲线的方法来解决绘制复杂曲线的问题。

![](https://cdn.tutsplus.com/net/uploads/legacy/939_canvas2/4.jpg)

如果你经常使用像Adobe Illustrator这类的矢量设计工具的话，那么上面的图应该就很容易理解了。

上面图中的两条曲线其实就是贝塞尔曲线和二次方贝塞尔曲线。两个曲线都有一个起点一个终点，但一次方贝塞尔曲线只有一个控制点，而二次方贝塞尔曲线有两个。你要问一个与两个有什么区别，好吧，一个控制点就是在两点之间只有一个点帮助你做平滑曲线，两个点就是始末点中间有两个点帮助你做平滑。当然，两点要是在你始末点连线的两侧的话，那就成一个S形咯。

下面我们就来画一条贝塞尔曲线的路径看看：


```
ctx.lineWidth = 8;
ctx.beginPath();
ctx.moveTo(50, 150);
ctx.quadraticCurveTo(250, 50, 450, 150);
ctx.stroke();
```

上面的代码会在画布中绘制一条曲线，如下所示：

<p data-height="268" data-theme-id="0" data-slug-hash="YXJEbE" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/YXJEbE/'>YXJEbE</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

二次方贝塞尔曲线方法主要有四个参数：

第一个和第二个分别是第一个控制点的X轴和Y轴的坐标。

第三个和第四个分别是第二个控制点的X轴和Y轴的坐标。

即：


```
quadraticCurveTo(cpx, cpy, x, y);
```

曲线的起点就是**moveTo**方法指定的坐标点。

下面升级一下，使用三次方贝塞尔曲线来画一条更妖娆的曲线：

```
ctx.lineWidth = 8;
ctx.beginPath();
ctx.moveTo(50, 150);
ctx.bezierCurveTo(150, 50, 350, 250, 450, 150);
ctx.stroke();
```

上面的代码会画一条大约是S形的曲线。

<p data-height="268" data-theme-id="0" data-slug-hash="gpBXNj" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/gpBXNj/'>gpBXNj</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

上面的贝塞尔曲线方法中有六个参数即有三个控制点：

第一个和第二个参数分别是第一个控制点的X轴和Y轴的坐标位置。

第三个和第四个参数分别是第二个控制点的X轴和Y轴的坐标位置。

第五个和第六个参数分别是第三个控制点的X轴和Y轴的坐标位置。

语法如下：

```
bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y);
```

贝塞尔曲线绘制图形就介绍到这里，发挥你的想象力，使用它可以绘制一些更加复杂和有趣的图形。

这里推荐一个Ai的插件，这样就可以直接把在Ai中绘制好的图像导出为canvas代码。

###保存绘制状态

在上一篇文章中，当要改变图形的填充颜色或者是边框宽度的时候，你不得不一遍又一遍的同时改变color和width这两个属性。还好canvas给我们提供了**save**方法，它能保存当前环境的状态。save之后，可以调用Canvas的平移、放缩、旋转、错切、裁剪等操作。这样我们就可以基于当前画布的状态来进一步进行操作。

####保存当前绘制状态

使用**save**方法非常简单，如下代码所示：

```
ctx.fillStyle = "rgb(0, 0, 255)";
ctx.save();
ctx.fillRect(50, 50, 100, 100);
```

你只需要在需要保存状态的地方调用一下**save**方法就可以了。

这里比较重要的要记住一点的是这个绘制状态之间的层级关系。打个比方，比如有一叠纸在桌子上，第一张放在桌子上的纸是在最下面的，每新放一张纸就会放在当前纸堆的最上面。如果你想得到第一张纸，你就得拿掉压在它上面的所有纸张才能得到第一张纸。canvas中的绘制状态之间关系也就是这种关系。

###restore方法

restore方法是用来恢复Canvas之前保存的状态。防止save后对Canvas执行的操作对后续的绘制有影响。这个有什么用呢？还是以代码来说明下。

```
ctx.fillStyle = "rgb(255, 0, 0)";
ctx.fillRect(200, 50, 100, 100);
```

上面的代码会绘制另外一个红色的矩形：

<p data-height="268" data-theme-id="0" data-slug-hash="ZGqvYb" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/ZGqvYb/'>ZGqvYb</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

但是如果我们要绘制一个蓝色的矩形呢？我们可能会想到直接使用**fillStye**方法来填充蓝色。不过这样可能会显得不是很优雅，因为前面已经绘制过一个蓝色的矩形了，有什么方法能复用前面的蓝色矩形的填充颜色么。**restore**方法就能做到，它能把状态恢复到**save**方法之前的状态。我们改下代码：

```
ctx.restore()
ctx.fillRect(350, 50, 100, 100);
```

使用了restore方法后，我们再来绘制矩形就会使用**save**之前的填充颜色即蓝色。如下所示：

<p data-height="268" data-theme-id="0" data-slug-hash="KpGZpP" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/KpGZpP/'>KpGZpP</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

###保存多个状态

通过上面的两个例子，我们知道了使用**save**和**restore**方法。那如果要保持多个状态怎么办呢？

上代码：

```
ctx.fillStyle = "rgb(0, 0, 255)";
ctx.save();
ctx.fillRect(50, 50, 100, 100);
 
ctx.fillStyle = "rgb(255, 0, 0)";
ctx.save();
ctx.fillRect(200, 50, 100, 100);
 
ctx.restore()
ctx.fillRect(350, 50, 100, 100);
```

上面代码执行效果：

<p data-height="268" data-theme-id="0" data-slug-hash="YXJYXe" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/YXJYXe/'>YXJYXe</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

那如果还要绘制一个蓝色的矩形呢？使用下面的代码：


```
ctx.restore();
ctx.fillRect(50, 200, 100, 100);
```

<p data-height="268" data-theme-id="0" data-slug-hash="waYpaN" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/waYpaN/'>waYpaN</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

通过上面的例子应该对多个状态的操作有了更进一步的理解。

OK，关于canvas绘制图形的方法就说到这里。

下一篇来说说在canvas中怎么使用渐变，阴影以及变形等操作。













