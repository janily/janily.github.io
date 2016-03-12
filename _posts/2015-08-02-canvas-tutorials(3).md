---
layout: post
title: web图形技术canvas之旅-canvas中的位移和渐变
categories:
- Life
tags:
- html5
- canvas
- javascript

---

这篇文章来继续学习下canvas中的位移、阴影以及渐变等操作。

开始之前还是先来准备基本的代码结构：


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
###位移

在canvas中要控制形状的位置很简单，使用**translate**方法就可以了。使用它可以轻松移动物体的位置，下面来具体实操下：

首先在画布的原点(0,0)位置画一个矩形：


```
ctx.fillRect(0, 0, 100, 100);
```

上面的代码就会在画布的左上角画一个矩形。

![](https://cdn.tutsplus.com/net/uploads/legacy/964_canvas3/1.jpg)

然后，移动画布环境的位置再画另一个矩形：


```
ctx.save();
ctx.translate(100, 100);
ctx.fillStyle = "rgb(0, 0, 255)";
ctx.fillRect(0, 0, 100, 100);
ctx.restore();
```

想想会发生什么呢？新的矩形会在坐标点(100,100)的位置画一个蓝色的矩形。

<p data-height="268" data-theme-id="17491" data-slug-hash="dowVRg" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/dowVRg/'>dowVRg</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

那具体是怎么回事呢？那是因为我们使用了**translate**方法把矩形移动到了(100,100)的位置。

![](https://cdn.tutsplus.com/net/uploads/legacy/964_canvas3/3.jpg)

###缩放

正如在CSS中那样，在canvas中也有放大缩小的功能。即**scale**方法。在canvas中缩放要记住：缩放是基于“原点”进行的。scale也经常与translate搭配使用。

在canvas对当前绘图对象进行缩放时，其缩放中心点是画布(0,0)的坐标原点。如果对绘图进行缩放，所有之后的绘图也会被缩放。定位也会被缩放。例如：scale(2,2)，那么绘图将定位于距离画布左上角两倍远的位置。

scale接受两个参数，依次是水平方向的缩放和垂直方向的缩放。参数可以是小数，如果小于1就是缩小，大于1则是放大——等于1则什么都不做。

还是以上个例子为基础，删除**translate**方法的代码，添加下面的代码：

上面的代码在画布中的(100,100)位置绘制了一个100px的矩形，那怎么来放大它呢？

![](https://cdn.tutsplus.com/net/uploads/legacy/964_canvas3/4.jpg)

下面来实操下：


```
ctx.save();
ctx.scale(2, 2);
ctx.fillRect(100, 100, 100, 100);
ctx.restore();
```

<p data-height="336" data-theme-id="17491" data-slug-hash="bdOoYd" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/bdOoYd/'>bdOoYd</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

我们可以看到我们新画的矩形在经过**scale**方法后，移动了新的位置。那是因为**scale**方法将矩形的位置将定位于距离画布左上角两倍远的位置。

如果我们同时使用**translate**方法来移动矩形的位置，然后使用**scale**方法来对矩形进行缩放，这个时候矩形的位置就不会移动了。


```
ctx.save();
ctx.translate(100, 100);
ctx.scale(2, 2);
ctx.fillRect(0, 0, 100, 100);
ctx.restore();
```

如下所示：

<p data-height="268" data-theme-id="17491" data-slug-hash="EjGwoQ" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/EjGwoQ/'>EjGwoQ</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

###旋转

在canvas中也提供了**rotate**方法来选择物体。

旋转顾名思义就是使物体按一定的角度来旋转，下面的代码就是使物体来旋转45度：


```
ctx.save();
ctx.rotate(Math.PI/4); // Rotate 45 degrees (in radians)
ctx.fillRect(100, 100, 100, 100);
ctx.restore();
```

结果如下：

![](https://cdn.tutsplus.com/net/uploads/legacy/964_canvas3/7.jpg)

好像哪里不对？旋转的矩形并没有在(100,100)的位置进行旋转，那是因为**rotate**方法默认的旋转点是画布的左上角。如下图所示：

![](https://cdn.tutsplus.com/net/uploads/legacy/964_canvas3/8.jpg)

那怎么能让矩形能按我们原来在(100,100)的位置来旋转呢？这就要使用**translate**和**rotate**方法来做到。


```
ctx.save();
ctx.translate(150, 150); // 先使用translate方法移动矩形
ctx.rotate(Math.PI/4); // rotate方法的旋转点完全是按照[在其上\紧跟着它的]translate来确定旋转点的.
ctx.fillRect(-50, -50, 100, 100); // 从新将坐标点移动到(100,100)的位置
ctx.restore();
```

执行上面的代码，这样矩形就会在我们想要的位置(100,100)来旋转了。

<p data-height="260" data-theme-id="17491" data-slug-hash="PqXJRY" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/PqXJRY/'>PqXJRY</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

###阴影

canvas提供了很多的特性，比如阴影就是一个很有用的特性。

添加阴影非常简单，只需要使用**shadowColor**这个方法就可以了。而且还提供了**shadowBlur**，**shadowOffsetX**和**shadowOffsetY**等属性来渐变的各个参数。

先来试试下面的代码：


```
ctx.save();
ctx.shadowBlur = 15;
ctx.shadowColor = "rgb(0, 0, 0)";
ctx.fillRect(100, 100, 100, 100);
ctx.restore();
```

上面的代码会绘制一个模糊位15像素的阴影。

<p data-height="268" data-theme-id="17491" data-slug-hash="pJqWKe" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/pJqWKe/'>pJqWKe</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

如果把**shadowBlur**设置为0，并且改变下颜色和使用**shadowOffsetX**和**shadowOffsetY**改变阴影的位置。

结果如下所示：

<p data-height="268" data-theme-id="17491" data-slug-hash="JdwrZB" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/JdwrZB/'>JdwrZB</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

要记住一点在绘制图形和阴影的时候，使用**save**和**restore**方法是一个好的习惯。

当然，在绘制阴影的时候会耗费性能。所以，使用图片来代替用动态代码来绘制阴影会是一个更好的选择。至于怎么使用canvas来操纵图片，下篇文章会详细来聊聊。

###阴影

最后来说下canvas提供的绘制渐变这个方法的特性。支持镜像和线性两种渐变，首先来看看线性渐变即(linear gradient)使用**createLinearGradient**方法来绘制线性渐变。


```
ctx.createLinearGradient(startX,startY,endX,endY);
```

头两个参数是控制渐变开始的位置，后两个参数控制渐变结束的位置。当然渐变是对颜色来进行渐变，所以还得需要颜色。在canvas中可以使用**fillStyle**和**strokeStye**来定义渐变的颜色。

下面的代码创建了一个从顶部到下面线性渐变：


```
var gradient = ctx.createLinearGradient(0, 0, 0, canvas.height);
gradient.addColorStop(0, "rgb(255, 255, 255)");
gradient.addColorStop(1, "rgb(0, 0, 0)");
 
ctx.save();
ctx.fillStyle = gradient;
ctx.fillRect(0, 0, canvas.width, canvas.height);
ctx.restore();
```

<p data-height="268" data-theme-id="17491" data-slug-hash="mJaBGN" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/mJaBGN/'>mJaBGN</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

下面来使用**createRadialGradient**来创建下镜像渐变，语法如下：


```
ctx.createRadialGradient(startX, startY, startRadius, endX, endY, endRadius);

```

跟线性渐变的参数差不多，只是多了两个来控制镜像渐变的圆的半径即**startRadius**来控制开始渐变圆的半径和**endRadius**来控制结束渐变的半径。

听起来是不是很迷糊，直接上代码就容易明白了。


```
var gradient = ctx.createRadialGradient(350, 350, 0, 50, 50, 100);
gradient.addColorStop(0, "rgb(0, 0, 0)");
gradient.addColorStop(1, "rgb(125, 125, 125)");
 
ctx.save();
ctx.fillStyle = gradient;
ctx.fillRect(0, 0, canvas.width, canvas.height);
ctx.restore();
```

<p data-height="363" data-theme-id="17491" data-slug-hash="Jdwrma" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/Jdwrma/'>Jdwrma</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

如图所示：

![](https://cdn.tutsplus.com/net/uploads/legacy/964_canvas3/14.jpg)

当然，如果要编写一个正常的镜像渐变比如像photoshop中从中心的镜像渐变一样。需要把镜像渐变中的开始和结束的坐标点设置为一样，并且要把其中一个渐变的圆设置比另一个圆大。

比如：


```
var canvasCentreX = canvas.width/2;
var canvasCentreY = canvas.height/2;
 
var gradient = ctx.createRadialGradient(canvasCentreX, canvasCentreY, 250, canvasCentreX, canvasCentreY, 0);
gradient.addColorStop(0, "rgb(0, 0, 0)");
gradient.addColorStop(1, "rgb(125, 125, 125)");
 
ctx.save();
ctx.fillStyle = gradient;
ctx.fillRect(0, 0, canvas.width, canvas.height);
ctx.restore();
```

如下所示：

<p data-height="314" data-theme-id="17491" data-slug-hash="jPXGQy" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/jPXGQy/'>jPXGQy</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

这样就创建来一个从画布中心向四周渐变的镜像渐变。

本篇文章就到这里啦，介绍了canvas中的位移和渐变以及阴影。

下篇文章将会介绍在canvas中怎么来操纵图片和视频，这才是canvas真正有趣的地方！












