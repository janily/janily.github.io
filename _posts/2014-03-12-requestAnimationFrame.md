---
layout: post
title: 	使用requestAnimationFrame编写高效的动画效果
categories:
- Life
tags:
- javascript
- html5
- 翻译
---

> 在上一篇文章中提到了使用**requestAnimationFrame**这个方法来编写动画效果，这个是W3C推出的一个用于编写动画效果的API。今天这篇文章就来学学requestAnimationFrame这个东东的使用方法以及其它的一些信息。这篇文章来自于[Efficient Animations with requestAnimationFrame](http://blog.teamtreehouse.com/efficient-animations-with-requestanimationframe)。

如果你想在你的项目中编写流畅的交互动画，那使用**requestAnimationFrame**(有时也简称为rAF)这个新的API是一个非常好的选择。

使用**requestAnimationFrame**可以处理非常复杂的动画效果。

在**requestAnimationFrame**出现以前，我们一般使用**setTimeout**和**setInterval**来编写交互动画效果。不过使用它们的有一个问题是为了保持动画的流畅性而流畅动画的最佳时间间隔为1000毫秒/ 60，约17ms。在这个频率你会看到流畅的动画，那是因为你最大的接近了浏览器能达到的频率。结果是花费了不必要的计算开销。另外一个问题是使用**setInterval**和**setTimeout**编写的动画效果在页面不是当前显示的页面的时候它还会在后台执行。

### 为什么使用requestAnimationFrame？ ###

那requestAnimationFrame相比**setInterval**和**setTimeout**有什么大的有时呢？

#### 性能优化 ####

使用**requestAnimationFrame**浏览器会自动帮你优化使动画更加平滑。这里就不打算深入讨论浏览器内部实现**requestAnimationFrame**优化的相关的细节仅仅需要知道
它会帮你避免一些不必要的重绘和回流而引起性能方面的开销而提高你动画的性能和平滑。

当使用**requestAnimationFrame**编写动画效果的时候，它只会在你页面当前页面是在激活的状态或者是可见的情况下才会运行。这就意味着能节省很多的CPU，GPU以及内存这才是**requestAnimationFrame**带来的一个重要的特性。

它还能节省电池的使用，而这对于移动设备来说是非常重要的。

### 使用requestAnimationFrame ###

**requestAnimationFrame**是通过一个回调函数来实现动画效果。为了创建一个完整的动画效果你需要通过一个回调函数来执行动画效果。RequestAnimationFrame()方法接受一个参数，是一个屏幕重绘前被调用的函数。这个函数用来对生成下合适的dom样式的改变，这些改变用在下一次重绘中。

当然也可以通过传递给一个高效的时间戳给回调函数[DOMHighResTimeStamp](https://developer.mozilla.org/en-US/docs/Web/API/DOMHighResTimeStamp)来控制动画效果，虽然不是很常用但是有时候也会用到。

下面的代码是来展示**requestAnimationFrame**的使用方法。

    // Animate.
	function animate(highResTimestamp) {
	  requestAnimationFrame(animate);
	  // Animate something...
	}
	
	// Start the animation.
	requestAnimationFrame(animate);

它默认的是以16.67ms来渲染动画的每一帧的，所以你在执行回调函数的时候你应该注意到这个问题。如果你想加大渲染动画每一帧的时间，那动画可能不是很流畅。

**requestAnimationFrame**这个方法会返回一个**requestID**可以用来取消动画。

    var requestID = requestAnimationFrame(animate);

### 取消动画效果 ###

你可以使用**cancelAnimationFrame**这个方法来取消动画效果。

    cancelAnimationFrame(requestID);

### Polyfill(回退方案) ###

由于**requestAnimationFrame**这个新的API退出的时间不久所以浏览器的支持也不是很全面，这里有一个回退方案，[地址](https://gist.github.com/paulirish/1579671)。

### 使用requestAnimationFrame创建一个简单的的实例 ###

![](http://pic.yupoo.com/reicky_v/DBgaagcz/9XeZQ.png)

理论说的差不多了，现在就来使用**requestAnimationFrame**来创建一个简单的动画实例。

<p data-height="268" data-theme-id="0" data-slug-hash="bGdEC" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/matt-west/pen/bGdEC'>requestAnimationFrame Demo</a> by Matt West (<a href='http://codepen.io/matt-west'>@matt-west</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

准备HTML和CSS

创建一个**index.html**文件。编写以下代码：

    <!DOCTYPE html>
	<html lang="en">
	<head>
	  <meta charset="UTF-8">
	  <title>requestAnimationFrame Demo</title>
	
	  <link rel="stylesheet" href="style.css">
	</head>
	<body>
	  <div id="page-wrapper">
	    <h1>requestAnimationFrame Demo</h1>
	
	    <div class="controls">
	      <button type="button" id="startBtn">Start Animation</button>
	      <button type="button" id="stopBtn">Stop Animation</button>
	      <button type="button" id="resetBtn">Reset</button>
	    </div>
	
	    <canvas id="stage" width="640" height="100"></canvas>
	  </div>
	
	  <script src="raf-polyfill.js"></script>
	  <script src="script.js"></script>
	</body>
	</html>

我们这里定义了三个按钮用来控制开始、终止和取消动画。你也可以自定义其它的一些内容。

你可能会注意到我们引入两个js文件**script**。**raf-polyfill.js**是上面提到的那个对不支持**requestAnimationFrame**浏览器提供的一个回退方案。你可以从[这个地址](http://cl.ly/3h020t143Y19)下载相关的代码。

编写javascript

这里我们使用了HTML5中的canvas这个新的API来绘制我们的UI。如果你以前对canvas不是很熟悉，不要担心这里我会讲解一些必要的基础知识。

创建一个**script.js**文件编写以下代码。

    (function() {

	  // Get the buttons.
	  var startBtn = document.getElementById('startBtn');
	  var stopBtn = document.getElementById('stopBtn');
	  var resetBtn = document.getElementById('resetBtn');
	
	  // The rest of the code goes here...
	}());

首先定义了三个变量使用来存储三个控制按钮。

下面需要编写canvas设置相关的代码，编写以下代码。

    // Canvas
	var canvas = document.getElementById('stage');
	
	// 2d Drawing Context.
	var ctx = canvas.getContext('2d');
	
	// Set the fill style for the drawing context.
	ctx.fillStyle = '#212121';
	
	// A variable to store the requestID.
	var requestID;
	
	// Variables to for the drawing position and object.
	var posX = 0;
	var boxWidth = 50;
	var pixelsPerFrame = 5; // How many pixels the box should move per frame.
	
	// Draw the initial box on the canvas.
	ctx.fillRect(posX, 0, boxWidth, canvas.height);

上面的代码中，我们首先创建了一个**canvas**变量来存储HTML中**&lt;canvas&gt;**元素。然后获得一个2d的一个绘图环境。它提供给我们一些绘图的方法比如定义样式等方法来绘制我们的图形。

下面的**fillStyle**方法是用来定义填充颜色的。

**requestID**是requestAnimationFrame返回给我们一个值可以用来控制动画用的。

**posX**、**boxWidth**和**pixelsPerFrame**变量用来设置画布box的位置，宽度高度等信息；以及每一帧box每一帧移动的距离。

最后你还要使用**fillRect**这个方法来绘制矩形，通过传递X和Y的值来决定绘制图形的位置。

### 编写动画方法 ###

下面我们来编写**animate**方法来实现动画效果。

在**script.js**编写下面的代码。

    // Animate.
	function animate() {
	  requestID = requestAnimationFrame(animate);
	
	  // If the box has not reached the end draw on the canvas.
	  // Otherwise stop the animation.
	  if (posX <= (canvas.width - boxWidth)) {
	    ctx.clearRect((posX - pixelsPerFrame), 0, boxWidth, canvas.height);
	    ctx.fillRect(posX, 0, boxWidth, canvas.height);
	    posX += pixelsPerFrame;
	  } else {
	    cancelAnimationFrame(requestID);
	  }
	}

在代码的顶部我们调用了**requestAnimationFrame**它会动画效果的下一帧才执行。还记得我们上面引入的那个**raf-polyfill.js**文件么，那就是用当浏览器不支持requestAnimationFrame的时候会使用**setTimeout**的方法来实现动画效果。

下面的代码是一个if的判断语句判断盒子是否已经移到到画布的右边。如果盒子还没移动到画布的右边，就会使用**clearRect**方法来清除上一帧动画所绘制的图形然后使用**fillRect**方法开始绘制下一帧动画。如果盒子已经移动到画布的右边，就会调用**cancelAnimationFrame**方法清除**animate**方法。最后更新**posX**变量用来存放新的位置信息为下一帧运动做准备。

> 我们上面使用的**clearRect**方法只是用来清除上一帧动画的位置信息。如果是清除整个画布信息的话那要花费多一点的计算。当然上面的代码还有很多可以优化的地方。

下面是来绑定相关事件监听的代码。在**script.js**编写以下代码：

    // Event listener for the start button.
	startBtn.addEventListener('click', function(e) {
	  e.preventDefault();
	
	  // Start the animation.
	  requestID = requestAnimationFrame(animate);
	});
	
	// Event listener for the stop button.
	stopBtn.addEventListener('click', function(e) {
	  e.preventDefault();
	
	  // Stop the animation;
	  cancelAnimationFrame(requestID);
	});
	
	// Event listener for the reset button.
	resetBtn.addEventListener('click', function(e) {
	  e.preventDefault();
	
	  // Reset the X position to 0.
	  posX = 0;
	
	  // Clear the canvas.
	  ctx.clearRect(0, 0, canvas.width, canvas.height);
	
	  // Draw the initial box on the canvas.
	  ctx.fillRect(posX, 0, boxWidth, canvas.height);
	});

这里绑定了三个按钮事件的监听。第一个和第二个事件是开始执行和结束动画事件的监听，最后一个事件是重置按钮事件的监听，重置事件会把盒子的位置重置为0即把盒子的位置重新定位到左边。

OK，一个简单的例子就完成了。在浏览器中打开**index.html**点击开始按钮可以看到盒子动起来了。源代码[下载地址](http://cl.ly/3h020t143Y19)

<p data-height="268" data-theme-id="0" data-slug-hash="bGdEC" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/matt-west/pen/bGdEC'>requestAnimationFrame Demo</a> by Matt West (<a href='http://codepen.io/matt-west'>@matt-west</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

### 浏览器支持 ###

浏览器对**requestAnimationFrame**的支持还是比较好的。Firefox和Chrome现在都支持这个APIOpera和IE最新版本的浏览器也开始支持了。当然为了考虑一些旧的浏览器你可以引入[这个代码](https://gist.github.com/paulirish/1579671)来使用**setTimeout**方法准备一个回退方案。不过在中国现在这个浏览器的环境下还是要准备回退方案的。[http://caniuse.com/requestanimationframe](http://caniuse.com/requestanimationframe)。

**requestAnimationFrame**方法可以使我们更容易的编写平滑的javascript动画效果。这个方法会利用浏览器原生的能力来处理动画这样性能上就得到了比较好的优化特别是在移动设备上性能这个就特别重要的。

对于**requestAnimationFrame**这个API你有什么可以分享的么。

下面推荐一些有用的资源：

- [Can i use requestAnimationFrame](http://caniuse.com/requestanimationframe)
- [requestAnimationFrame](https://gist.github.com/paulirish/1579671)
- [MDN requestAnimationFrame Docs](https://developer.mozilla.org/en-US/docs/Web/API/window.requestAnimationFrame)
- [Better performance with requestAnimationFrame](http://dev.opera.com/articles/view/better-performance-with-requestanimationframe/)
- [Timing control fro script-based animations](http://www.w3.org/TR/2013/CR-animation-timing-20131031/)





