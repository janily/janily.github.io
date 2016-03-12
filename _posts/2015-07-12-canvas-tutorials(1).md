---
layout: post
title: web图形技术canvas之旅-canvas基本介绍
categories:
- Life
tags:
- html5
- canvas
- javascript

---

> 最近在做一些web图形技术方面的东东，对canvas以前也是零零散散有一些应用。刚好在[tutsplus](code.tutsplus.com)网站上看到一个canvas系列的教程还不错，就学习了下。原文系列教程[地址](https://code.tutsplus.com/series/canvas-from-scratch--net-19650)。这里只是把学习过程中的一些知识点纪录下来，方便以后查询。

要在网页中使用canvas很简单；只需要在html文件中定义一个**canvas**元素就可以了。


```
<canvas width="500" height="500">
    <!-- Insert fallback content here -->
</canvas>
```
上面代码会在网页中显示一个透明的画布。如果浏览器不支持canvas，还可以提示用户当前的环境不支持canvas。

###浏览器支持度

现在浏览器对canvas的支持已经非常好了。大部分的现代浏览器都支持canvas技术。详细的支持情况可以去这个[地址](http://caniuse.com/#feat=canvas)看看。

###canvas画布

canvas跟其他标签一样，也可以通过css来定义样式。但这里需要注意的是：canvas的默认宽高为300px * 150px,在css中为canvas定义宽高，实际上把宽高为300px * 150px的画布进行了拉伸，如果在这样的情况下进行canvas绘图，你得到的图形可能就是变形的效果。所以，在canvas绘图时，应该在canvas标签里直接定义宽高。

canvas 提供了通过 JavaScript 绘制图形的方法，此方法使用简单但功能强大。每一个canvas 元素都有一个”上下文( context )” (想象成绘图板上的一页)，在其中可以绘制任意图形。浏览器支持多个 canvas 上下文，并通过不同的 API 提供图形绘制功能。

####canvas坐标系统

在canvas中2d环境中，也使用标准的笛卡尔坐标系统。即原点(0,0)在画布的左上角，往右边的就是X轴下方是Y轴。这个很容易理解，如下图所示：

![](https://cdn.tutsplus.com/net/uploads/legacy/916_canvas1/1.jpg)

###在canvas中绘图

创建好了画布后，让我们来准备画笔。要在画布中绘制图形需要使用 JavaScript 。首先通过 getElementById 函数找到 canvas
元素，然后初始化上下文。之后可以使用上下文 API 绘制各种图形。


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

在上面的代码中，我们使用了jquery这个库，并且把canvas中的2d绘制环境存储在ctx这个变量中。

下面我们来试着画一些基本的图像。

###绘制矩形

准备好相关的canvas绘图环境后，我们就可以使用canvas的各种API来绘制图形了。比如**fillRect**方法，使用它可以绘制一个矩形。

把下面这行代码，添加到**ctx**变量下面：

```
ctx.fillRect(50, 50, 100, 100);
```

<p data-height="268" data-theme-id="0" data-slug-hash="WvJPaW" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/WvJPaW/'>WvJPaW</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

简单的一句代码，我们就在画布中绘制来一个矩形。

**fillRect**方法有四个参数：

第一个是相对画布原点X轴的坐标点。

第二个是Y轴的坐标点。

第三个是矩形的宽度。

第四个是高度。


```
ctx.fillRect(x, y, width, height);
```

当然，有时候我们可能想绘制一个没有填充的矩形即只有边框的。canvas也提供来这样的方法，即**strokeRect**方法，比如这样：


```
ctx.strokeRect(50, 50, 100, 100);
```

参数跟**fillRect**方法的参数一样：

<p data-height="268" data-theme-id="0" data-slug-hash="QbrYJR" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/QbrYJR/'>QbrYJR</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

所有的这些方法看起来都很简单，但是如果把它们组合起来使用，我们可以做更多酷炫的事情。

###绘制路径

一些常规的形状，canvas都有提供相应的方法。但如果要绘制一些比较复杂多图形，比如曲线。这个时候就要使用到canvas提供path方法来绘制了。

绘制一个简单的路径，我们需要使用下面的方法：

**beginPath** 开启一个新的路径。

**moveTo** 指定路径要绘制的下一个坐标点。

**lineTo** 根据moveTo方法定义的坐标点来绘制路径到指定的坐标点。

**closePath** 绘制完成后，闭合路径。

**fill** 填充路径的颜色。

**stroke** 路径的边框。

比如：


```
ctx.beginPath();
ctx.moveTo(50, 50);
ctx.lineTo(50, 250);
ctx.lineTo(250, 250);
ctx.closePath();
ctx.fill();
```

上面的代码会绘制一个三角形：

<p data-height="268" data-theme-id="0" data-slug-hash="GJdzPG" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/GJdzPG/'>GJdzPG</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

根据上面代码的原理，我们可以绘制任意图像。在后面的文章中会讲到，使用Path绘制一些复杂的图形，比如弧形以及使用贝塞尔曲线绘制一些曲线。

只要知道一点，使用path可以绘制任何复杂的图形。

###填充图形颜色

到目前为止，我们绘制的图形都是默认的颜色黑色。要改变图形的颜色也很简单，使用canvas提供的**fillStyle**和**strokeStyle**方法就可以做到。

下面来试试看，绘制一个红色的矩形：


```
ctx.fillStyle = "rgb(255, 0, 0)";
ctx.fillRect(50, 50, 100, 100);
```

<p data-height="268" data-theme-id="0" data-slug-hash="yNjZZL" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/yNjZZL/'>yNjZZL</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

当然，我们也可以绘制只有边框的矩形：


```
ctx.strokeStyle = "rgb(255, 0, 0)";
ctx.strokeRect(50, 50, 100, 100);
```

如图所示：

![](https://cdn.tutsplus.com/net/uploads/legacy/916_canvas1/6.jpg)

**fillStyle**和**strokeStyle**支持多种颜色模式。比如RGB, RGBA, HSA以及直接使用颜色的名称如**red**。就像在CSS中使用颜色一样简单。

这里有一点需要注意的是，当你在canvas中改变颜色的时候，它不会影响到已经绘制图形的颜色。比如，如果已经绘制了一个黑色的矩形，然后你改变颜色值，再绘制一个其它的图形；那么第一个图形不会受到影响，仍然是黑色的。

###定义图片的宽高

除了改变填充颜色，同样也可以改变边框的宽度。这就要使用到**lineWidth**方法。

比如：


```
ctx.lineWidth = 20;
ctx.strokeStyle = "rgb(255, 0, 0)";
ctx.strokeRect(50, 50, 100, 100);
```

上面的代码会绘制一个边框为20像素度矩形：


```
https://cdn.tutsplus.com/net/uploads/legacy/916_canvas1/7.jpg
```

同样，在路径中也可以使用：


```
ctx.lineWidth = 20;
ctx.beginPath();
ctx.moveTo(50, 50);
ctx.lineTo(50, 250);
ctx.lineTo(250, 250);
ctx.closePath();
ctx.stroke();
```

它就会绘制一个20像素的三角形：

![](https://cdn.tutsplus.com/net/uploads/legacy/916_canvas1/8.jpg)

在canvas中，还提供了一些方法可以改变边框的样式。如**lineCap**方法可以设置或返回线条末端线帽的样式，比如我们可以利用它来设置边框末端的样式为圆角。如**lineJoin**方法可以设置或返回所创建边角的类型，当两条线交汇时。详细内容可以去这个[地址](http://www.whatwg.org/specs/web-apps/current-work/multipage/the-canvas-element.html#line-styles)看看。

###擦除图形

既然有绘制必然有擦除。最后我们来说说在canvas中怎么来擦除已经绘制的图形。

canvas中可以使用**clearRect**方法来擦除图形。

在上面的代码中，我们绘制的矩形都是500像素的，所以在擦除的时候，也需要擦除500像素的内容：


```
ctx.fillRect(50, 50, 100, 100);
ctx.clearRect(0, 0, 500, 500);
```

运行这个代码，不会看到任何的变化。因为在绘制完，又马上擦除了。

**clearRect**的参数跟**fillRect**的参数表示的意义一样。如果你不是很确定画布的宽高，你也可以这样来使用它。

```
ctx.clearRect(0, 0, canvas.width, canvas.height);
```

下面来看一个擦除画布的实力，首先为了直观的表示**clearRect**方法的作用，我们绘制两个矩形：


```
ctx.fillRect(50, 50, 100, 100);
ctx.fillStyle = "rgb(255, 0, 0)";
ctx.fillRect(200, 50, 100, 100);
```
![](https://cdn.tutsplus.com/net/uploads/legacy/916_canvas1/9.jpg)

然后使用**clearRect**方法来清除黑色的矩形：


```
ctx.clearRect(50, 50, 100, 100);
```

在**clearRect**方法中，指定了要擦除图形的位置和尺寸。这样在清除的时候，只会清楚黑色的矩形。

<p data-height="268" data-theme-id="0" data-slug-hash="pJVGBo" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/pJVGBo/'>pJVGBo</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

当然，擦除画布的方法不止这么简单。在后面的关于canvas动画的文章中，你会看到要大量运用到**clearRect**方法。

这篇文章只介绍了一些canvas的基本知识，在下一篇文章来学习一下使用canvas绘制图像的一些高级技巧。比如绘制圆形，曲线路径以及一些更加复杂的图形。










