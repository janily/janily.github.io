---
layout: post
title: javascript学习侧栏滑入滑出制作
categories:
- Life
tags:
- javascript
---

这次在项目中，有一个侧边栏的效果，具体效果可以看下这个[链接](http://jsbin.com/igesuq/1)。正好正在学习js，就记录下整个的实现思路。

这里推荐下妙味这个网站，真心不错，非常适合刚开始学习js的童鞋们用。当然javascript高级程序设计这本书得备着。

首先是写好html代码。

<pre>
<code>
&lt;div id="share"&gt;
		&lt;span&gt;分享到&lt;/span&gt;
	&lt;/div&gt;
</code>
</pre>

首先在写css前，我们从实际效果来构思我们的布局和css，我们要的效果是当鼠标划入的时候，分享到展现具体内容，而鼠标划出的时候隐藏内容。

当然具体到css来说，我们有很多种方法乱来达到这样的效果。如用绝对定位、宽度等。用什么方法也关系到你javascript的写法。

我们这里用绝对定位的方法来实现这样的布局，具体的交互就有javascript来实现。css如下：

<pre>
<code>
#share {
	width: 100px;
	height: 200px;
	background: #eee;
	position: absolute;
	left: -100px;
}
#share span {
	width: 20px;
	height: 60px;
	line-height: 20px;
	text-align: center;
	background: yellow;
	position: absolute;
	left: 100px;
}
</code>
</pre>

css思路是这样的，share这个层是整个内容层，我们用绝对定位和改变left值达到把它隐藏的目的，而share这个层中的span是我们用来实现滑入滑出交互的钩子，我们就需要把这个span用定位来使它处于可见的状态。

下面就javascript的部分，先不要急着写代码，而是要想清楚整个交互实现的思路，再根据思路去写代码，这样就快很多啦。

当鼠标划入的时候，要share层显示，我们就要把它的left值设为0，而当鼠标划出的时候就是默认的left值。如果就这样，我们想象一下left值从负100变化到0中间没有一个过渡的效果，如慢慢把left值从负100变化到0。这样就会很突兀，体验不好。所以在我们写js的时候要把平滑过渡这一效果考虑进去。

具体开始前，我们要先搭好这个js的代码结构，如下

<pre>
<code>
window.onload=function ()
{};
</code>
</pre>

首先我们定义一个move函数

<pre>
<code>
var timer = null;
function move(iTarget)
{
	var oDiv = document.getElementById('share');
	clearInterval(timer);
};
</code>
</pre>

函数中的oDiv变量是用来存储id为share这个dom节点的（其实大部分的交互效果就是跟dom打交道）。timer这个变量在后面的运动定时器中会用到。函数有一个iTarget参数。是用来指出我们需要运动的距离。

下面就是move函数具体实现部分。

<pre>
<code>
function move(iTarget) {
var oDiv = document.getElementById('share');
clearInterval(timer);
timer=setInterval(function(){
	var speed = 10;
	if (oDiv.offsetLeft&lt;iTarget) {
		speed=10;  //这里判断当share层的left值小于我们传入的参数时，速度设为10，否则设为-10
	} else
	{
		speed=-10;
	};

	if (oDiv.offsetLeft==iTarget) 
		{
			clearInterval(timer);   //当left值等于我们传入的参数时，就清除定时器。否则就继续执行下面的，直到left等于iTarget
		} else
		{
			oDiv.style.left=oDiv.offsetLeft+speed+'px';   //这一句就是不断地以speed递增或者递减来达到匀速运动的效果
		};
	
},30)
}
</code>
</pre>

在这里面我们定义了一个speed参数，我们上面已经说过要share层的left值从-100到0缓慢的变化，就有必要引入一个速度参数。具体请看代码的里的注释。

整个的运动效果，用到了setInterval()这一方法，这一方法在为w3school中有详细的介绍，链接在[这里](http://www.w3school.com.cn/htmldom/met_win_setinterval.asp)。

全部的javascript代码如下

<pre>
<code>
window.onload=function ()
{
	var oDiv=document.getElementById('share');

	oDiv.onmouseover=function ()
	{
		move(0);  //鼠标划入的时候把share层的left设为0，share层的内容就会显现出来
	};

	oDiv.onmouseout=function ()
	{
		move(-100); //鼠标划出的时候把share层的left设为-100，share层的内容就会隐藏
	};

	var timer = null;
	function move(iTarget) {
		var oDiv = document.getElementById('share');
		clearInterval(timer);

		timer=setInterval(function(){
			var speed = 10;
			if (oDiv.offsetLeft&lt;iTarget) {
				speed=10;
			} else
			{
				speed=-10;
			}

			if (oDiv.offsetLeft==iTarget) 
				{
					clearInterval(timer);
				} else
				{
					oDiv.style.left=oDiv.offsetLeft+speed+'px';
				}
			
		},30);
	}

};
</code>
</pre>

最终就实现了我们想要的效果[demo](http://jsbin.com/igesuq/1)。


























