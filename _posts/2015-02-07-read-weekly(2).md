---
layout: post
title: javascript经典实例-读书周总结(2)
categories:
- Life
tags:
- javascript
- 读书

---

上周继续利用晚上的空闲时间阅读<<js经典实例>>的第三章到第六章，是关于数组、时间日期、数字处理这一块点内容。下面就来说说这三块的一些应用场景和解决方案。

### 时间

时间在web开发中是一个经常碰到的应用场景，在javascript中，可以通过Date对象来管理。可以使用各种技术来创建一个日期。

比如你想要把当前的日期打印到一个web页面上。

你可以这样做。

创建一个新的对象，没有任何参数，并且将其输入到web页面上。

	var drElm = document.getElementById("date");
	var dt = new Date();
	dtElm.innerHTML = "<p>" + dt + "</p>";
	
当然，你可以在新建一个Date对象的时候，你可以给构造函数传不同的参数，来创建一个特定的时间对象，例如，你可以使用**getMonth**、**getFullYear**、**getTime**、**getDate**等方法来访问Date的单个部分。然后再构建日期字符串：

	var dt = new Date();
	var month = dt.getMonth();
	month++;
	var day = dt.getDate();
	var yr = dt.getFullYear();
	dtElm.innerHTML = "<p>" + month + "/" + day + "/" + yr;
	
上面的代码运行效果如下：

<a class="jsbin-embed" href="http://jsbin.com/hafuje/1/embed?js,output">test</a><script src="http://static.jsbin.com/js/embed.js"></script>

再来看看时间的另一个应用场景，比如记录两个事件之间的时间。

解决方法是，在第一个时间发生点时候，创建一个Date对象，当第二个事件发生的时候，创建一个新的Date对象，并且从第二个对象红减去第一个对象。之间差以毫秒表示的，要转换为秒，就除以1000。

如下代码所示：

<a class="jsbin-embed" href="http://jsbin.com/henocu/1/embed?html,js,console">test</a><script src="http://static.jsbin.com/js/embed.js"></script>

在时间的应用中定时器这个是频率很高的应用，特别是在动画制作中。javascript中提供了两种定时器，延迟执行和重复性定时器。

	//延迟执行
	window.onload = function() {
		setTimeout("alert('timeout!)",3000);
	}

	//重复执行
	var x = 0;
	setInterval(moveElement,1000);
	
	function moveElement() {
		x+= 10;
		var left = x + "px";
		
		document.getElementById("redbox").style.left = left;
	}
	
如果有一个带有定时器的函数，但是想要直接把该函数添加到定时器方法调用中，我们可以像下面这样做，试着点击红色的方框：

<a class="jsbin-embed" href="http://jsbin.com/kovonu/1/embed?js,output">test</a><script src="http://static.jsbin.com/js/embed.js"></script>

### 使用number和math来对数字进行操作

在javascript中可以使用number和math两个对象来进行相关的操作。

javascript中Numbers是浮点数，然而可能不会显示出一个小数部分。如果没有显示小数部分，它们就好像是整数一样。

	var someValue = 10 //在十进制中，当作整数10对待
	
也可以使用构造函数来构造一个Number:

	var newNum = new Number(23);
	
Number对象提供了各种方法来操作数字：

	var tst = .0004532
	
	console.log(tst.toExponenttial())  //输出4.532-4
	
还有一个特殊的Number静态属性NaN，即*Not a Number()非数字*，当想要使用一个数字操作中无法解析为一个数字的一个值，将得到一个NaN错误:

	alert(parseInt("3.5")); //输出3
	alert(parseInt("three is point")); //输出NaN
	
math对象，该对象和number对象不同，math对象没有构造函数，该对象所有的功能都是静态的，如果想要实例化对象;

	var newMath = new Math();
	
会得到一个错误。访问属性方法直接在该对象上进行，不需要创建新的构造函数。

	var topValue = Math.max(firstValue,secondValue); //返回较大的数字
	
####递增计数

比如

	var globalCounter = 0;
	function nextTest() {
	
	 globalCounter++;
	 ...
	}
	
这里使用了js自增和自减的运算符。

	nameValue = nameValue + 1 //等价于  nameValue++
	nameValue = nameValue - 1 //等价于  nameValue--
	
这两个运算符可以放在数字的前面和后面，那么它们有什么区别呢？

如果运算符前置，运算数的值首先调整，然后才使用其值；如果运算符后置，那么先使用运算数，然后再调整其值。具体代码及运行效果如下所示：

<a class="jsbin-embed" href="http://jsbin.com/hoqehu/1/embed?js,console">test</a><script src="http://static.jsbin.com/js/embed.js"></script>

如果需要找到一个等价于一个十进制的十六进制的值该怎么做呢？

可以使用下面的代码来做到：

	var num = 255;
	console.log(num.toString(16)); //输出ff，这是255等价的十六制形式
	
### 创建一个随机数生成器

	var randomNumber = Math.floor(Math.random() * 256);
	
random方法会产生一个0~1之间的随机数字。不过，为了增加数字的范围，可以将这个结果乘以你想要的值的范围上限。如果需要一个具有较高下限的随机值，例如，在5~10之间，将random的值乘以这样一个值：它等于上线减去下限，加上一。然后相乘的结果加上下限值，得到最后的结果：

	var randomNumber = Math.floor(Math.random() * 6) + 5;
	
floor方法将浮点值向下舍入到最近的整数。

### 随机产生颜色

随机产生一个web颜色值。可以使用Math的对象产生每个RGB值，如下面的例子：

<a class="jsbin-embed" href="http://jsbin.com/rivoci/1/embed?js,output">test</a><script src="http://static.jsbin.com/js/embed.js"></script>

### 在角度和弧度之间转换

在javascript中，弧度和角度可以借助于Math对象中的三角函数来做到。

入将角度转换为弧度，将其值乘以(Math.PI / 180):

	var radians = degress * (Math.PI / 180);
	
要把弧度转换为角度，将值乘以(180 / Math.PI):

	var degress = radians * (180 / Math.PI);
	
### 在一个给定宽高的元素中计算出一个可放置圆的半径和圆心

给定一个页面元素的宽度和高度，你需要求得该页面元素中可容纳的最大的圆的半径及其圆心。

解决方案：

求出宽度和高度中较小多一个，用其除以2得到点半径：

var circleRadius = Math.min(elementWidth,elementHeight) / 2;

通过将页面的宽度和高度除以2，找到其中心点：

	var x = elementWidth / 2;
	var y = elelmentHeight / 2;
	
实际运行效果如下：

<a class="jsbin-embed" href="http://jsbin.com/jolifo/1/embed?js,output">test</a><script src="http://static.jsbin.com/js/embed.js"></script>

数组的内容比较多，也比较重要，特别是在web开发中，更是如此。得单独写一篇来说说。

