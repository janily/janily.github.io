---
layout: post
title: Snapjs基本知识入门
categories:
- Life
tags:
- javascript
- snapjs
- svg

---

工欲善其事，必先利其器。要用svg制作复杂或者是高级的动画效果，javascript就必不可少来。今天我们就来学习下svg中的jQuery库[Snap.svg](http://snapsvg.io/)这一js库，它的功能跟jQuery在dom的作用差不多，只不过它是专门用来操作svg的。有了它，我们就可以轻松的使用javascript和svg打交道了。

我们会以实际的例子来讲解Snap的使用方法。

### Snap的那些事儿

说起Snap就不得不提[Raphäel.js](http://raphaeljs.com/)这个库。因为Snap的创造者正是Raphäel.js的创造者[Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)，而Raphäel.js也是用来操作svg的。它的主要一个功能是能使老版本的IE浏览器也能支持svg，比如ie6等。

而snap的出现，则是实现了svg中的一些高级特性的功能，比如蒙板、渐变、分组以及动画等高级特性，而且也不再对老版本的不支持svg的浏览器进行兼容，大大减少了代码量更加能发挥svg的特性。

### Snap能做些什么

从官方的文档[API documentation](http://snapsvg.io/docs/)可以看到，所有svg的特性我们都可以使用Snap来操作，比如[mask](http://snapsvg.io/docs/#Paper.mask),[group](http://snapsvg.io/docs/#Paper.mask),[gradient](http://snapsvg.io/docs/#Paper.gradient),[filter](http://snapsvg.io/docs/#Snap.filter.blur),[animate](http://snapsvg.io/docs/#Snap.animate),[pattern](http://snapsvg.io/docs/#Element.pattern)。

使用snap能帮助我们创建svg格式的图形，当然也能基于现有的svg图形来进行操作。意味着我们不一定要使用snap来创建图形，我们可以先使用一些适量编辑软件如Adobe IIIustrator,Inkscape,或者是Sketch来制作svg图形，然后再使用snap来进行一些操作。

### 开始使用Snap

方便起见，我们这里使用[codepen](http://codepen.io/)来做一些demo。

首先准备一个基本开始骨架，基本的hmtl结构以及引入snapsvg.js这个库。

准备好后就可以开始编码啦。

其实它的使用方法跟jQuery差不多，开始之前需要初始化Snap，即使用Snap来制定我们需要操作svg的节点并把它指定给一个变量。我们这里就定义为s。

	
```
var s = Snap("#svg");
```
	
是不是似增相识。

现在我们就可以使用Snap提供的各种方法来操作**s**这个变量，比如圆或者是矩形。

* 圆，创建圆需要三个参数：X(x轴的坐标)，Y(y轴的坐标)，半径。具体可以参考文档[circle API](http://snapsvg.io/docs/#Paper.circle)
* 矩形，需要四个参数：X，Y，宽，高。文档地址[rect API](http://snapsvg.io/docs/#Paper.rect)
* 椭圆，需要四个参数：X，Y，horizontal radius(水平方向的半径)，vertical radius(垂直方向的半径)。文档地址[ellipse API](http://snapsvg.io/docs/#Paper.ellipse)

我们输入下面的js代码：

	
```
	// Circle with 80px radius
	var circle = s.circle(90,120,80);
	// Square with 160px side width
	var square = s.rect(210,40,160,160);
	// Ellipse with 80px vertical radius and 50px horizontal radius
	var ellipse = s.ellipse(460,120,50,80);

```	

就会为我们绘制下面这三个图形：

<p data-height="286" data-theme-id="0" data-slug-hash="Igmka" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/Igmka/'>Igmka</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>


从代码运行的结果来看，默认填充的颜色是黑色。下面我们使用snap来设置一些样式，如填充、透明度、边框、边框的宽度等属性。具体可以去看看文档
[SVG attributes](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute)。

	
```
	circle.attr({
  	fill: 'coral',
  	stroke: 'coral',
  	strokeOpacity: .3,
  	strokeWidth: 10
	});
 
	square.attr({
  	fill: 'lightblue',
  	stroke: 'lightblue',
  	strokeOpacity: .3,
  	strokeWidth: 10
	});
 
	ellipse.attr({
  	fill: 'mediumturquoise',
  	stroke: 'mediumturquoise',
  	strokeOpacity: .2,
  	strokeWidth: 10
	});

```	

这样我们的图形看起来比前面就更漂亮来些！

<p data-height="298" data-theme-id="0" data-slug-hash="hKpaA" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/hKpaA/'>hKpaA</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

### 分组操作图形

Snap[http://snapsvg.io/]为我们提供来分组操作[group](http://snapsvg.io/docs/#Paper.group)这一强大的功能，顾名思义，使用它我们可以把多个图形编成一组，使之变为一个图形。

先来创建两个图形，然后把它们编成一组。再来操作它们的属性。

	
```
	var circle_1 = s.circle(200, 200, 140);
	var circle_2 = s.circle(150, 200, 140);
 
	var circles = s.group(circle_1, circle_2);
 
	circles.attr({
  	fill: 'coral',
  	fillOpacity: .6
	});
```
	
结果如下：

<p data-height="372" data-theme-id="0" data-slug-hash="Dgros" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/Dgros/'>Dgros</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

在文章开始部分，我们说过会做一个眼睛的例子。需要用到svg中的蒙板属性[mask](http://snapsvg.io/docs/#Paper.mask)。首先我们来创建一个椭圆并放置在上组图形的中间。

	
```
	var circle_1 = s.circle(300, 200, 140);
	var circle_2 = s.circle(250, 200, 140);
 
	var circles = s.group(circle_1, circle_2);
 
	var ellipse = s.ellipse(275, 220, 170, 90);
 
	circles.attr({
  	fill: 'coral',
  	fillOpacity: .6,
	});
 
	ellipse.attr({
  	opacity: .4
	});
	
```
	
<p data-height="355" data-theme-id="0" data-slug-hash="BovgL" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/BovgL/'>BovgL</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

现在我们就以椭圆为蒙板来对图形进行剪裁，并且对椭圆填充为白色：

	
```
	circles.attr({
  	fill: 'coral',
  	fillOpacity: .6,
  	mask: ellipse
	})
 
	ellipse.attr({
  	fill: '#fff',
  	opacity: .8
	});
	
```
	
<p data-height="356" data-theme-id="0" data-slug-hash="fAFJa" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/fAFJa/'>fAFJa</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

### 让图形动起来

眼睛的形状就完成来，不过要是让眼睛动起来会更加有趣一点。使用Snap中的[animate](http://snapsvg.io/docs/#Set.animate)方法来实现动画效果非常方便。要使眼睛动起来，我们只需要让椭圆的垂直的半径从1增加到90就可以了。

先来创建一个名为**blink**的动画函数：

	
```
	function blink(){
		ellipse.animate({ry:1)},220,function(){
			ellipse.animate({ry:90},300);
			)}
	};
	
```
	
现在我们可以使用**setInterval**函数来循环执行**blink**动画，这样我们的眼睛就会不停的运动。

	
```
	setInterval(blink,3000);

```	
最后完整的代码如下所示：

	
```
	var circle_1 = s.circle(300, 200, 140);
	var circle_2 = s.circle(250, 200, 140);
 
	// Group circles together
 
	var circles = s.group(circle_1, circle_2);
 
	var ellipse = s.ellipse(275, 220, 170, 90);
 
	// Add fill color and opacity to circle and apply
	// the mask
	circles.attr({
  	fill: 'coral',
  	fillOpacity: .6,
  	mask: ellipse
	});
 
	ellipse.attr({
  	fill: '#fff',
  	opacity: .8
	});
 
	// Create a blink effect by modifying the rx value
	// for ellipse from 90px to 1px and backwards
 
	function blink(){
  	ellipse.animate({ry:1}, 220, function(){
    ellipse.animate({ry: 90}, 300);
  	});
	};
 
	// Recall blink method once every 3 seconds
 
	setInterval(blink, 3000);
	
```
	
效果如下：

<p data-height="357" data-theme-id="0" data-slug-hash="DGebI" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/DGebI/'>DGebI</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

### 浏览器支持

要注意的使，Snap只支持**IE9+**,chrome,safari,firefox以及opera等现代浏览器。

### 开源

Snap.svg使开源的，意味着我们可以免费使用它来开发。

### 总结

Snap.svg使我们更加容易的来操作svg。svg未来必将会大放异彩。

参考文章：

[How to Manipulate and Animate SVG With Snap.svg](http://webdesign.tutsplus.com/articles/how-to-manipulate-and-animate-svg-with-snapsvg--cms-21323)




	








