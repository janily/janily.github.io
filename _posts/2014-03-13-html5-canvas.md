---
layout: post
title: 	HTML5之Canvas入门指南
categories:
- Life
tags:
- canvas
- html5
- 翻译
---

> canvas是一个新的HTML元素，这个元素可以被Script语言(通常是JavaScript)用来绘制图形。例如可以用它来画图、合成图象、或做简单的(和不那么简单的)动画。今天这篇来说说HTML5中的canvas这个东东。原文来自[http://www.sitepoint.com/html5-canvas-tutorial-introduction/](http://www.sitepoint.com/html5-canvas-tutorial-introduction/)。

HTML5中的canvas的推出又进一步大大的扩展了web的表现领域，使web设计师能更加自由的表现自己的想法、创意等等没有做不到只有想不到。你能使用canvas能做到什么只受限与你的技术和想象力。

web技术现在日新月异，正在飞速发展。HTML5的出现，极大地拓展了web开发人员的力量这在过去是不可想象的。我们直接可以在浏览器中绘制图形并且制作动画效果这就是[HTML5 Canvas](http://www.whatwg.org/specs/web-apps/current-work/multipage/the-canvas-element.html)。

### 什么是HTML5 Canvas? ###

canvas也是HTML中的一个标签元素也需要定义宽高属性。配合HTML Canvas API才能发挥它强大的力量，重要的是能通过javascript来操纵canvas来绘制图形。

### HTML5 Canvas的优势 ###

- 交互性。Canvas能对用户的各种操纵进行响应比如点击，按键或者是触摸事件。所以canvas就充满了无限的可能。
- 动画。Canvas不但能绘制图形并且还能进行动画创作就想Flash一样。
- 灵活性。可以使用canvas可以创建任意的形状不直线、形状、文字、图片等等，也可以添加音频和视频。
- 跨平台。HTML5 Canvas现在大部分的浏览器都支持并且可以在各种设备上使用如桌面PC、智能手机等设备。
- web标准。不像Flash和Silverlight，Canvas是一个HTML5标准的一部分即W3C标准的一部分。
- 开发门槛低。只需要一个浏览器和一个代码编辑器就可以进行Canvas开发。特别是现在有很多的canvas开发的第三方的js库和工具来辅助canvas开发。

###	Canvas能做什么？  ###

那么Canvas能做什么呢？

- 游戏。Canvas可以用来开发2D和3D游戏。
- 广告。Canvas可以用来代替Flash进行创意广告创作。
- 数据可视化。使用Canvas可以把数据用图形可视化的形式表现出来。
- 教育与培训。canvas也可以用来制作一些图形和视频、音频来提升教育和培训行业的学习体验。
- 艺术。canvas只受限与你的想象力，我们可以使用它来创作令人惊叹的艺术作品。

开始吧！

每一个canvas都必须有一个上下文(context)，上下文使用来和canvas进行交互的。2d context使用来创建2D图形的以及可以操纵位图；3d context使用来创建和管理2d图形的。3d实际上其实就是[WebGL](https://en.wikipedia.org/wiki/WebGL)是基于[OpenGL ES](https://en.wikipedia.org/wiki/OpenGL_ES)。

现在就开始吧，准备好一个代码编辑器和chrome浏览器把，当然你也可以选用其它的现代浏览器。

新建一个HTML页面编写以下代码：

    <canvas id="exampleCanvas" width="500" height="300">
 
	  <!-- OPTION 1: leave a message here if browser doesn't support canvas -->
	    <p>Your browser doesn’t currently support HTML5 Canvas. 
	    Please check caniuse.com/#feat=canvas for information on 
	    browser support for canvas.</p>
	 
	  <!-- OPTION 2: put fallback content (text, image, Flash, etc.) 
	                 if the browser doesn't support canvas -->
	 
	</canvas>

下面在HTML页面底部编写下面的javascript代码：

    var canvas = document.getElementById('exampleCanvas'),
    context = canvas.getContext('2d');
 
    // code examples will continue here...

浏览器会默认创建一个宽300px和高150px的canvas元素。当然你也可以自定义canvas的宽高。

我们给canvas元素定义了一个id是为了方便用javascript来操纵canvas元素的。你也可以使用CSS来定义元素的一些样式如边框等等，当然它不是必需的，只是为了方便我们来操纵canvas元素。

在canvas元素我们还添加了一些文字提示，当用户的浏览器不支持canvas的时候就会提示给用户。

第一行javascript代码是用来获取canvas元素，这个时候就用到了我们定义canvas的ID。第二行的context变量是使用了**getContext()**方法来存储canvas的2D绘图环境的。

这两行代码就是我们用来使用canvas API的基础。不过在此之前让我们先来了解一下canvas的一些详细的特性。

### Canvas元素大小与实际绘图的大小 ###

除了canvas元素的宽高属性，你可以使用CSS定义canvas元素的宽高属性。然而在canvas中并不是你用CSS定义了canvas的元素的宽高也定义了绘图区域的宽高。因为canvas元素有两套尺寸，一套是canvas元素本身的尺寸；一套是绘图环境的宽高。

当你设置元素的**width**和**height**属性，你可能会同时设置元素本身和绘图环境的宽高；不过，你使用CSS设置一个canvas宽高，你只是设置元素本身的宽高并不是绘图环境的宽高。当你设置canvas元素的宽高和绘图环境的宽高不匹配时，浏览器会自动把绘图环境的宽高调整匹配元素本身的宽高。

因于此，使用元素本身的**width**和**height**来定义宽高来代替使用CSS定义它的宽高。

### 理解canvas的坐标系统 ###

在2D环境里，一般用X和Y来表示位置。X轴表示水平方向的，Y轴表示垂直方向。用**x = 0**和**y = 0**表示为中心的位置，也可以用**(0,0)**来表示。详细的可以去[ Cartesian coordinate system](https://en.wikipedia.org/wiki/Cartesian_coordinate_system)了解详细的信息。

在canvas的坐标系统里，原点的位置是在画布的左上角，往X轴的右边移动是增加X轴的值而往Y轴的底部移动是增加Y的值。不过跟标准的笛卡尔坐标系统不同是，在Canvas的坐标系统里面没有负值。使用负值并不会导致你的程序出错，但是不会在画布上起到作用。

![](http://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/03/1393985491canvas-coordinate-space.png)

### 简单的例子 ###

使用canvas我们可以创建曲线、文字、线条等图形。在下面这个例子中，我们具体来看看怎么用canvas的API来绘制图形。

### 绘制直线 ###

要在canvas中绘制直线，我们可以使用canvas的API方法**beginPath()**。然后使用**moveTo(x,y)**方法来设置绘画开始的坐标，之后使用**lineTo(x,y)**方法来设置绘制线条的终点。

然后就开始绘制图形了但是现在还是没有在画布上显示的。要使它呈现在画布上我们得使用**stroke()**方法来显示。

    context.beginPath();
	context.moveTo(50, 50);
	context.lineTo(250, 150);
	context.stroke();

<p data-height="268" data-theme-id="0" data-slug-hash="aihgp" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/aihgp'>Canvas Line Example</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

### 绘制矩形 ###

绘制一个矩形，可以使用**fillRect（x,y,width,height）**方法来设置绘制的坐标和宽高，然后使用**fillStyle**方法来设置填充颜色。

    context.fillStyle = 'yellow';
	context.fillRect(50, 50, 200, 100);

<p data-height="268" data-theme-id="0" data-slug-hash="HcBCu" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/HcBCu'>Canvas Rectangle Example</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

### 绘制文字 ###

也可以在canvas上绘制文字，可以使用**fillText(string,x,y)**以及使用**font**属性来设置字体的尺寸和颜色等样式(跟CSS差不多)。

    context.font = 'italic 40pt Calibri, sans-serif';
	context.fillText('Hello World!', 50, 50);

<p data-height="268" data-theme-id="0" data-slug-hash="qurkx" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/qurkx'>Canvas Text Example</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

当然你也可能意识到用Canvas绘制一些简单的图形是非常容易的，而创建复杂的形状和动画效果需要一些数学和物理知识。关于这个可以去看看这本书[Foundation HTML5 Animation with JavaScript](http://www.amazon.com/Foundation-HTML5-Animation-JavaScript-Lamberta/dp/1430236655)。这本对于想进一步学习canvas的高级应用是一个非常好的选择。

### 有用的学习资源 ###

如果你想学习更多的HTML5的资源，下面推荐一些有用的资料：

- [Canvas tutorial](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Canvas_tutorial)
- [HTML5 Canvas Element Guide](http://sixrevisions.com/html/canvas-element/)
- [HTML5 Canvas tutorials](http://www.html5canvastutorials.com/tutorials/html5-canvas-tutorials-introduction/)

当然借助于第三方的库可以加速我们的开发效率，下面推荐一些比较好的第三方的库。

- [KineticJS](http://kineticjs.com/)
- [Paper.js](http://paperjs.org/)
- [EaselJS](http://createjs.com/#!/EaselJS)
- [Fabric.js](http://fabricjs.com/)
- [oCanvas](http://ocanvas.org/)

另外一个提高HTML5 Canvas开发的效率就是借助一些好的工具如[Ai的一个canvas插件](http://blog.mikeswanson.com/ai2canvas)，可以去[这篇文章](http://www.sitepoint.com/html5-canvas-development-with-ai-canvas/)看它的使用方法。

