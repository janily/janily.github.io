---
layout: post
title: 使用javascript和基本的物理运动规律制作有趣的动画效果
categories:
- Life
tags:
- javascript
- canvas
- animation
- 前端

---

> 要创建有趣有吸引力的动画效果，一些基本的物理定律是要遵守的。这样才符合人们在现实生活中的认知，感同身受。一个在地上匀速运动的物体，考虑的地面的摩擦力，其物体的速度必定是从快到慢的一个过程。那在使用代码编写这样一些动画效果的时候，就要符合这样一些的基本物体运动定律。今天这篇文章就来聊聊使用javascript和canvas来编写一些有趣的动画效果。[原文地址](http://codepen.io/rachsmith/blog/hack-physics-and-javascript-1)。

当然，这篇文章不会去详细的阐述各种高大上的物体定律。你只需要了解一些基本的加速和减速运动就可以来，以及怎么去使用代码实现它就可以了。

### 使用javascript创建匀速运动

方向单一（设定一个正方向），速度大小可改变【速度在（0，∞）这个范围内】，速度递增的话是加速运动。反之减速运动。速度大小可变，速度递增的话是加速运动。反之减速运动。

我们这里使用javascript来创建加速运动。主要是包括两个参数，一个是X轴的方向，另一个是Y轴的方向。下面的这个效果，是一个物体从X轴和Y轴的方向，在指定的画布中朝两个方向不断的以每帧2px的速率运动。

效果如下所示：

<p data-height="268" data-theme-id="0" data-slug-hash="VLPVYG" data-default-tab="result" data-user="rachsmith" class='codepen'>See the Pen <a href='http://codepen.io/rachsmith/pen/VLPVYG/'>Velocity with JavaScript</a> by Rachel Smith (<a href='http://codepen.io/rachsmith'>@rachsmith</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

### 创建加速运动

加速运动，顾名思义就是速度不停的递增的运动。那使用javascript怎么来编写呢？代码中vx和vy是物体运动的速率，如果要使物体有一个加速的运动，可以在物体运动每一帧中加0.5px。实际效果如下所示：

<p data-height="268" data-theme-id="0" data-slug-hash="oXBQXy" data-default-tab="result" data-user="rachsmith" class='codepen'>See the Pen <a href='http://codepen.io/rachsmith/pen/oXBQXy/'>Acceleration with JavaScript</a> by Rachel Smith (<a href='http://codepen.io/rachsmith'>@rachsmith</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

### 一个简单的粒子特效

粒子特效在flash中非常常见。不过，现在借助javascript和canvas也能编写这样的粒子动画效果。下面就会使用javascript和canvas来创建这样一个粒子动画效果。如果，你对在canvas中怎么使用javascript来绘制图形不是很熟悉的话，可以去[这篇文章](http://codepen.io/rachsmith/blog/controlling-the-canvas-with-javascript-objects)看看。

创建粒子效果，首先需要在画布中绘制一些基本的粒子。每一个粒子都有，宽、高以及位置和运动速率这几个基本是属性。这里我们把所有粒子的位置全部放置在画布的中心，然后随机的运动粒子，使它有一个从中心往外散发的效果。


```function Particle(x, y, vx, vy, size, color, opacity) {

  this.update = function() {
    x += vx;
    y += vy;
  }

  this.draw = function() {
    ctx.globalAlpha = opacity;
    ctx.fillStyle = color;
    ctx.fillRect(x, y, size, size);
  } 
}

function createParticle(i) {
  // initial position in middle of canvas
  var x = width*0.5;
  var y = height*0.5;
  // randomize the vx and vy a little - but we still want them flying 'up' and 'out'
  var vx = -2+Math.random()*4;
  var vy = Math.random()*-3;
  // randomize size and opacity a little & pick a color from our color palette
  var size = 5+Math.random()*5;
  var color = colors[i%colors.length];
  var opacity =  0.5 + Math.random()*0.5;
  var p = new Particle(x, y, vx, vy, size, color, opacity);
  particles.push(p);
}
```

效果如下所示（可能需要点击return按钮来观看这个效果）

<p data-height="268" data-theme-id="0" data-slug-hash="NqdmGg" data-default-tab="result" data-user="rachsmith" class='codepen'>See the Pen <a href='http://codepen.io/rachsmith/pen/NqdmGg/'>Particle fountain - step 1</a> by Rachel Smith (<a href='http://codepen.io/rachsmith'>@rachsmith</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

上面这个效果只是不断的往外散发的动画效果，结合现实生活中的物体运动的基本定律。上面效果中的物体在散发的过程中应该往下掉，才符合真实生活中的物体运动常识。

这里需要添加一个往下掉的重力加速度这一个参数，重力加速度的值为0.04px。同时，也给粒子添加了一个透明度的变化，逐渐消失。然后，当每一个粒子的透明度为零的时候，在画布的中心重设粒子。效果如下：

<p data-height="268" data-theme-id="0" data-slug-hash="oXBOwg" data-default-tab="result" data-user="rachsmith" class='codepen'>See the Pen <a href='http://codepen.io/rachsmith/pen/oXBOwg/'>Particle fountain - with gravity!</a> by Rachel Smith (<a href='http://codepen.io/rachsmith'>@rachsmith</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

OK，明白了一些基本的动画效果的编写。

下一篇来点高级的应用。




