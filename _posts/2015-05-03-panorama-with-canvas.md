---
layout: post
title: 使用canvas来制作全景式自动浏览图片的动画效果
categories:
- Life
tags:
- javascript
- canvas
- 动画

---

今天来使用canvas来写一个简单的动画效果。

任何事情开始做之前，我们可以先把它分解成一个一个小的步骤，这样就可以点成线，线成面。最后一气呵成的做成你想要做的事情。

### 基础的动画知识

那使用canvas来编写动画效果，有哪些步骤呢？

* 清除画布(每隔一定时间绘制图形并且清除图形)。清除画布可以使用[ clearRect() ](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/clearRect)方法来清除方法。
* 保存 canvas 状态。如果你要改变一些会改变 canvas 状态的设置（样式，变形之类的），又要在每画一帧之时都是原始状态的话，你需要先保存一下。
* 绘制动画图形（animated shapes）。这一步才是重绘动画帧。
* 恢复 canvas 状态。如果已经保存了 canvas 的状态，可以先恢复它，然后重绘下一帧。

在 canvas 上绘制内容是用 canvas 提供的或者自定义的方法，而通常，我们仅仅在脚本执行结束后才能看见结果，比如说，在 for 循环里面做完成动画是不太可能的。

所以这里，我们需要借助于一定的设置去计算图形的变换，重绘新的帧，画每一帧的过程是一样的。

这里有两个方法分别是**setInterval**和**setInterval**方法来达到这个目的。

setTimeout和setInterval的语法相同。它们都有两个参数，一个是将要执行的代码字符串，还有一个是以毫秒为单位的时间间隔，当过了那个时间段之后就将执行那段代码。

不过这两个函数还是有区别的，setInterval在执行完一次代码之后，经过了那个固定的时间间隔，它还会自动重复执行代码，而setTimeout只执行一次那段代码。

如果你不需要任何交互操作，用setInterval方法定时执行重绘是最适合的了。

当然，现在还有一个更好的方法，是使用**requestAnimationFrame(callback)**这个方法。

这个方法是用来在页面重绘之前，通知浏览器调用一个指定的函数，以满足开发者操作动画的需求。这个方法接受一个函数为参，该函数会在重绘前调用。

如果你想做逐帧动画的时候，你应该用这个方法。这就要求你的动画函数执行会先于浏览器重绘动作。通常来说，被调用的频率是每秒60次，但是一般会遵循W3C标准规定的频率。如果是后台标签页面，重绘频率则会大大降低。

**callback**在每次需要重新绘制动画时,会调用这个参数所指定的函数。这个回调函数会收到一个参数，这个 DOMHighResTimeStamp 类型的参数指示当前时间距离开始触发 requestAnimationFrame 的回调的时间。

### 全景展现动画效果

在这个动画效果这个例子中，我们在一个固定的区域里使用一张比这个画布区域大很多的全景式图片，然后使用js来自动从左滚动到右边，这样就可以看到图片的全貌。

下面来看代码：

	var img = new Image();

	// 定义一些初始化变量，图片路径、画布尺寸、速度、方向等

	img.src = 'https://mdn.mozillademos.org/files/4553/Capitan_Meadows,_Yosemite_National_Park.jpg';
	var CanvasXSize = 800;
	var CanvasYSize = 200;
	var speed = 30; //lower is faster
	var scale = 1.05;
	var y = -4.5; //vertical offset

	// 主程序

	var dx = 0.75;
	var imgW;
	var imgH;
	var x = 0;
	var clearX;
	var clearY;
	var ctx;

	img.onload = function() {
    imgW = img.width*scale;
    imgH = img.height*scale;
    if (imgW > CanvasXSize) { x = CanvasXSize-imgW; } // 画布大于当前画布的尺寸
    if (imgW > CanvasXSize) { clearX = imgW; } 
    else { clearX = CanvasXSize; }
    if (imgH > CanvasYSize) { clearY = imgH; } // 画布大于当前画布的尺寸
    else { clearY = CanvasYSize; }
    // 获取画布元素
    ctx = document.getElementById('canvas').getContext('2d');
    //设置画布刷新频率
    return setInterval(draw, speed);
	}

	function draw() {
    //在绘制前，清除画布
    ctx.clearRect(0,0,clearX,clearY);
    //如果图片小于画布尺寸
    if (imgW <= CanvasXSize) {
        //重设画布起点尺寸
        if (x > (CanvasXSize)) { x = 0; }
        //绘制图形
        if (x > (CanvasXSize-imgW)) { ctx.drawImage(img,x-CanvasXSize+1,y,imgW,imgH); }
    }
    //如果图片大于画布尺寸
    else {
        //重设画布起点尺寸
        if (x > (CanvasXSize)) { x = CanvasXSize-imgW; }
        //绘制图形
        if (x > (CanvasXSize-imgW)) { ctx.drawImage(img,x-imgW+1,y,imgW,imgH); }
    }
    //运行draw方法，绘制图形
    ctx.drawImage(img,x,y,imgW,imgH);
    //移动图片
    x += dx;
	}
	
脚本代码编写好后，我们在html中插入canvas元素，并定义好跟脚本中定义大小一样的尺寸。

	<canvas id="canvas" width="800" height="200"></canvas>
	
最后，运行效果个如下：

<p data-height="268" data-theme-id="0" data-slug-hash="dooayo" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/dooayo/'>dooayo</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

**参考链接**

[Basic animations](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Basic_animations)

[window.requestAnimationFrame](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/requestAnimationFrame)

	