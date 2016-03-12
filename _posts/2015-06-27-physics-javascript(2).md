---
layout: post
title: 使用javascript和基本的物理运动规律制作有趣的动画效果(2)
categories:
- Life
tags:
- javascript
- canvas
- animation
- 前端

---

> 上一篇讲过一些关于物理一些基本定律在动画方面的应用，这篇文章继续来说说这些基本定律在动画方面的应用。[原文地址](http://codepen.io/rachsmith/blog/hack-physics-and-javascript-part-2-solving-triangles-profit)。

内事不决问百度，外事不决问谷歌。多亏了[这篇文章](https://www.mathsisfun.com/algebra/trig-solving-triangles.html)替我补回了很多的数学知识。

有些时候，特别当我们开始编写代码的时候，我们怎么来运用数学知识应用到动画中会感到一些困惑。今天就用一个实例来演示怎么把一些基本的数学基本知识结合物理定律来编写有趣的动画效果。

我们将编写一个用户点击鼠标来打掉挂在控制杆上的企鹅这样的一个动画效果。听起来很有趣吧，下面我们就一步一步通过一些基本的数学知识来实现这个动画效果，开始吧。

###三角函数

首先第一件事情是，控制杆。我们使用canvas来画这个控制杆。


```
function drawBase() {
  ctx.beginPath();
  ctx.arc(leverPosition.x, leverPosition.y, 40, Math.PI, 0, false);
  ctx.closePath();
  ctx.lineWidth = 5;
  ctx.fillStyle = '#c1c0c8';
  ctx.fill();
}
```

现在就要开始在画布中绘制控制杆啦。控制杆的长度是200px，跟地面倾斜的角度时70度。所以在绘制画布的时候，你需要知道开始的坐标(x1,y1)以及结束的坐标(x2,y2)。到目前为止，我们知道开始的坐标点，但是还不知道结束的坐标点。

![](https://s3-us-west-2.amazonaws.com/s.cdpn.io/53148/xy.png)

当我们看完上面控制杆的示意图的时候，结束的坐标点和开始的坐标点其实是一个三角形。在知道控制杆的长度以及倾斜的角度，需要计算出结束点的坐标点。这就要用到数学中的三角函数了。

下面是一个三角形来重新绘制了控制杆的图，有三个角：A,B和C以及三条边。

![](https://s3-us-west-2.amazonaws.com/s.cdpn.io/53148/angles.png)

接下来就是解决三角形方面的问题了，我们知道三角形的三个内角的和是180度。

`A + B + C = 180`

由于这个三角形是直角三角形，知道B是90度以及A是70度，所以通过下面的公式得出A的角度。

`A = 180 - B - C`

现在我们得到三个角度多数值就可以轻易的计算出三条边的长度了。我们可以通过下面的的公式来计算出完美想要边的长度。

`a / sin(A) = b / sin(B) = c / sin(C)`

由于知道三个角的角度以及B边的长度，可以计算出C边的长度。


```
c / sin(C) = b / sin(B) c = b / sin(B) * sin(C)
```

最后可以得到A边的长度。


```
a / sin(A) = c / sin(C) a = c / sin(C) * sin(A)
```

下面这个javascript的函数就是用来计算出B边长度的。不要忘了javascript中三角形的函数如Math.sin也是需要角度这个参数的，所以需要把角度作为参数传递给三角形函数。借助于帮助手册能帮我们迅速解决问题。


```
function calculateTriangleXY(C) {
  var B = 90;
  // solve for A
  var A = 180 - C - B;
  // solve c and a
  var c = (leverLength * Math.sin(toRadians(C))) / Math.sin(toRadians(B));
  var a = (c * Math.sin(toRadians(A))) / Math.sin(toRadians(C));
  // return x/y sides
  return {x: a, y: c}
}

function toRadians(deg) {
  return deg / 180 * Math.PI;
}
```

现在有了计算对应边长度的方法，我们就可以来绘制控制杆了。


```
function drawLever() {
  var xy = calculateTriangleXY(currentAngle);
  ctx.beginPath();
  ctx.moveTo(leverPosition.x, leverPosition.y);
  ctx.lineTo(leverPosition.x - xy.x, leverPosition.y - xy.y);
  ctx.lineWidth = 4;
  ctx.strokeStyle = '#7b4b3d';
  ctx.stroke();
}
```

OK，控制杆已经画好了。接下来是和用户交互了，就是当用户使用鼠标按住控制杆并且拖拽的时候，控制杆需要跟随用户拖拽的角度实时的变换控制杆的倾斜的角度。要怎么做呢？我们可以根据鼠标的相对于画布的变化的坐标值来计算出控制杆应该倾斜的角度。

![](https://s3-us-west-2.amazonaws.com/s.cdpn.io/53148/hit-area.png)

![](https://s3-us-west-2.amazonaws.com/s.cdpn.io/53148/hit-area-mousepos.png)

三角函数又派上用场了！当用户使用鼠标点击控制杆的时候，这个时候鼠标在控制杆上与控制杆其它的两个点又构成了一个三角形。因为我们已经知道了控制杆倾斜的角度以及鼠标坐标Y的值，这里有一个问题是，我们怎么来判断用户的鼠标是在控制杆上呢？其实也很简单，我们可以判断用户鼠标的左边X的值是否和新的三角形上的X是否足够接近，如果是，我们就认定用户的鼠标是在控制杆上的，这样就可以计算出具体的值了。如下所示：


```
function calculateTriangleXY(C, y) {
  var B = 90;
  // solve for A
  var A = 180 - C - B;
  // if we only have angle C we need to solve edge c first
  var c = y || (leverLength * Math.sin(toRadians(C))) / Math.sin(toRadians(B));
  var a = (c * Math.sin(toRadians(A))) / Math.sin(toRadians(C));
  return {x: a, y: c}
}
```
现在我们就来检测鼠标的位置了。如果在控制杆范围之内，就改变鼠标**cursor**属性，变成手势可点击。然后设置变量**hitting**的值为true。


```
// get x given current angle & y pos
var xy = calculateTriangleXY(currentAngle, leverPosition.y - mousePos.y);
// if mouse x is close to the triangle x, we're in the hit area
if ((mousePos.x > leverPosition.x - xy.x - 20) && (mousePos.x < leverPosition.x - xy.x + 20)) {
    canvas.style.cursor = 'pointer';
    hitting = true;
} else {
    canvas.style.cursor = 'auto';
    hitting = false;
}
```

现在我们可以知道用户的鼠标是否放在控制杆上。这意味着当用户按下鼠标的时候我们可以来执行拖拽控制杆的事件。当用户点击并开始拖拽控制杆的时候，我们需要知道鼠标的坐标点。要做到这一点－需要知道控制杆倾斜的角度，同样解决这个问题就要用到三角函数了。

在这个实例中，通过鼠标的坐标我们可以计算出A和C边的长度以及B的角度是90度。

![](https://s3-us-west-2.amazonaws.com/s.cdpn.io/53148/drag.png)

为了得到C的角度，首先需要知道B边的长度。使用三角函数可以很容易计算出它的长度。


```
b2 = a2 + c2 − (2 * a * c * cos(B))
```

知道来B边的长度，可以通过下面的方法得出C的角度。


```
function calculateTriangleAngle(x,y) {
  var A, C;
  var B = 90;
  var c = y;
  var a = x;
  var b = Math.sqrt(Math.pow(a, 2) + Math.pow(c, 2) - 2 * a * c * Math.cos(toRadians(B)));
  C = Math.asin(Math.sin(toRadians(B)) / b * c);
  return C;
}
```

然后通过用户拖拽控制杆来设置控制杆的倾斜角度。


```
var angle = toDegrees(calculateTriangleAngle(leverPosition.x - mousePos.x, leverPosition.y - mousePos.y));
if (angle < 10) {
  release();
  return;
}
if (angle > 80) angle = 80;
currentAngle = angle;
```

最后就是当用户松开鼠标的时候，重置控制杆的倾斜角度为初始的角度70度，这里我们通过**release**这个方法来达到。还记得在第一部分提到了加速度么？为来使之更符合物理运动的规律，我们这里通过**vr**变量来给控制杆一个缓冲的效果，即控制杆不是一下子就回到70度的角度，而是有一个加速度的变化过程。


```
function release() {
  pulling = false;
  vr = (70 - currentAngle) * 0.06;
}
```

最后完整的效果如下：

<p data-height="268" data-theme-id="0" data-slug-hash="EjXqpR" data-default-tab="result" data-user="rachsmith" class='codepen'>See the Pen <a href='http://codepen.io/rachsmith/pen/EjXqpR/'>A lever</a> by Rachel Smith (<a href='http://codepen.io/rachsmith'>@rachsmith</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

###飞翔的企鹅

控制杆的动画效果编写好了，下面是企鹅部分代码的编写。

首先是编写一个创建企鹅的方法。


```
function Penguin(x, y, r) {
  var _this = this;
  var img;
  this.x = x;
  this.y = y;
  this.r = targetAngle;

  (function() {
    img = new Image();
    img.src = 'https://s3-us-west-2.amazonaws.com/s.cdpn.io/53148/penguin.png';
  })();

  this.update = function() {
    // rotating the penguin
    ctx.save();
    ctx.translate(_this.x, _this.y); 
    if(_this.latched) ctx.rotate(toRadians(currentAngle));
    else ctx.rotate(toRadians(_this.r));
    // translating a little so it sits further down the ledge
    ctx.translate(24, -16);
    ctx.drawImage(img, -20, -20, 40, 40);
    ctx.restore();
  }
}
```

创建好企鹅好后，下面是来创建掉落在地上的企鹅。


```
function addPenguin() {
  var penguin = new Penguin(140,120);
  penguin.latched = true;
  penguins.push(penguin);
  loadedPenguin = penguin;
}
```

你会注意到上面的方法中，我们在penguin对象上设置了**lathed**这个属性并且值为true。当为true的时候，企鹅会实时的更新自己到角度跟控制杆的角度保持一致。当控制杆返回到初始角度的时候，需要释放企鹅。通过下面的**updateRotate**方法来执行这个操作。


```
if (released && currentAngle > targetAngle) {
  released = false;
  throwPenguin();
}
```

上面的代码时来执行弹出企鹅，企鹅在空中飞翔的动画的。需要设置企鹅的位置和飞翔的加速度。


```
function throwPenguin() {
  loadedPenguin.vx = vr*4;
  loadedPenguin.vy = vr*-1.86;
  loadedPenguin.vr = 0.5;
  loadedPenguin.latched = false;
  loadedPenguin = null;
  setTimeout(addPenguin, 500);
}
```


```
// added to Penguin.update --
// penguin is launched!
if(!_this.latched && _this.vx) {
  _this.vy += gravity;
  _this.x += _this.vx;
  _this.y += _this.vy;
  _this.r += _this.vr;
  _this.vx *= 0.98;
  _this.vy *= 0.98;

  // check for hitting edges
  if (_this.x + 28 > canvasWidth) {
    _this.x = canvasWidth - 28;
    _this.vx *= bounce;
    _this.vr *= bounce;
  }

  if (_this.y + 38 > canvasHeight) {
    _this.y = canvasHeight - 38;
    _this.vy *= bounce;
    _this.vr *= bounce;
  } 

  if (Math.abs(_this.vx) < 0.001) _this.vx = 0;
  if (Math.abs(_this.vy) < 0.001) _this.vy = 0;
}
```

最后效果如下所示：

<p data-height="268" data-theme-id="0" data-slug-hash="zGdNmK" data-default-tab="result" data-user="rachsmith" class='codepen'>See the Pen <a href='http://codepen.io/rachsmith/pen/zGdNmK/'>A lever with penguins</a> by Rachel Smith (<a href='http://codepen.io/rachsmith'>@rachsmith</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

awesome!!终于搞完啦。不得不说javascript代码还是有点多的相信通过这篇文章，你应该知道来如何运用三角函数来解决一些问题了。

下篇文章，我会来聊聊关于spring动画方面的东东。敬请期待！




