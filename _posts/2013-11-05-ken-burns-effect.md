---
layout: post
title: 用javascript和css制作KenBurns效果
categories:
- Life
tags:
- javascript
- css
- 翻译
---

> 这篇文章主要是来自[demosthenes](http://demosthenes.info/blog)网站的[Create-A-Random-Ken-Burns-Effect-For-Images-With-CSS-amp-JavaScript](http://demosthenes.info/blog/761/Create-A-Random-Ken-Burns-Effect-For-Images-With-CSS-amp-JavaScript)，用于图片，过渡效果还不错。

[AskMen.com](http://ca.askmen.com/)在它们的网站的网站设计，上使用了很多的有趣的技术。比如在它们网站上的头部的图片就用了一个*Ken Burns*的过渡效果，非常漂亮。具体是何种效果可以看下面的的实例。

<p data-height="614" data-theme-id="0" data-slug-hash="lgGHb" data-user="janily" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/lgGHb'>Random Ken Burns Image Animation Effect</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a></p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

最近也有很多的同学问起这个效果是如何实现的。当然我并没有去看AskMen这个网站的代码，不知道到它们是如何实现的，不过我倒是很乐意和大家分享我的实现方法，只需一点点**javascript**和**css动画**。

首先我们先把结构写好，如下：

    <figure id="burnsbox">
	<img src="janelle-monae.jpg" alt="Photograph of Janelle Monae in concert, shot in silhouette against a blue light">
	</figure>

在这里为了更好地控制图片，我们使用定位技术，如下：

    figure#burnsbox { overflow: hidden; position: relative;
	padding-top: 60%; }
	figure#burnsbox img { position: absolute;
	top: 0; left:-5%; width: 110%; height: 110%; }

这里说明下，figure我们用了*overflow：hidden*，这样方便我们控制图片展示区域的大小。

我们来看看，要做这样的一个效果，我们需要做些什么：

- 对图片进行放大缩小，当然可以随机生成一个放大缩小的比例。但是要确保放大或缩小尺寸不要破坏图片原本的比例。
- 至于放大缩小我们用CSS动画来处理
- 现阶段来说，针对各个浏览器还是要加上前缀

下面根据上面的规划，我们来确定下相关的参数：

- 缩放的比例是1.1到1.4
- 动画执行的时间是12秒

根据上面的规划，我们可以很 轻松的先把这个随机缩放的方法写出来，如下：

    function randomizer(min,max) {
	randomresult = Math.random() * (max - min) + min;
	return randomresult;
	}
	var maxscale = 1.4;
	var minscale = 1.1;
	var minMov = 5;
	var maxMov = 10;
	var scalar = randomizer(minscale,maxscale).toFixed(2);
	var moveX = randomizer(minMov,maxMov).toFixed(2);
	moveX = Math.random() < 0.5 ? -Math.abs(moveX) : Math.abs(moveX);
	var moveY = randomizer(minMov,maxMov).toFixed(2);
	moveY = Math.random() < 0.5 ? -Math.abs(moveY) : Math.abs(moveY);

这里我们用到javascript中的*toFixed()*方法可把 Number 四舍五入为指定小数位数的数字。从而避免生成的像1.269274802这样的浮点数。 moveX 和 moveY是的值使用正负数来控制的，这样才能利用css3中的*translate*属性来控制图片能更好的产生缩放效果。

下面我们来看看怎么用javascript来生成样式并插入到网页中：

    var lastSheet = document.styleSheets[document.styleSheets.length - 1];

不要重复自己，应该是身为一个程序员的的自我要求，比如在这个效果中，需要用到CSS3，而现在由于浏览器的原因，我们需要不停的写各种浏览器的前缀，如何避免呢，用js就可以办到，如下：

    var prefix = "";
	if (CSSRule.WEBKIT_KEYFRAMES_RULE) { prefix = "-webkit-"; }
	else if (CSSRule.MOZ_KEYFRAMES_RULE) { prefix = "-moz-"; }

接下来是CSS动画部分了。这里关键的一点主要是动画运行时间的控制，问你这里是平移和缩放的执行时间是整个动画所需要时间的80%，而动画开始结束的时间各占10%。样式写好后，我们用insertRule来写入样式。如下：

    lastSheet.insertRule("@"+prefix+"keyframes zoomzoom {" +
	"10% { " + prefix + "transform: scale(1); } " +
	"90% { " + prefix + "transform: scale(" + scalar +" ) translate(" + moveX +"%," + moveY + "%); } " +
	"100% { " + prefix + "transform: scale(" + scalar + ") translate(" + moveX +"%," + moveY +"%); } }", 0);

如果是在火狐下运行的话，脚本会自动生成针对火狐前缀的样式，如下：

    @keyframes zoomzoom {
	10% { transform: scale(1); }
	90% { transform: scale(1.34) translate(7.21%, 5.89%); }
	100% { transform: scale(1.34) translate(7.21%, 5.89%); }
	}

最后是要把动画样式添加到图片上，这个可以用脚本来做，当然也可以直接用CSS来做：

    figure#burnsbox img { position: absolute; top: 0; left:-5%;
	width: 110%; height: 110%;
	-moz-animation: zoomzoom 12s linear alternate infinite;
	-webkit-animation: zoomzoom 12s linear alternate infinite;
	animation: zoomzoom 12s linear alternate infinite; }

而我们这里由于需要传入随机参数给CSS，用脚本来生成样式还是比较好一点，这里需要注意的一点的是，需要在DOM节点加载完毕的时候，就把样式添加操页面上，如下：

    window.onload = function(){
	sheet = document.createElement('style');
	document.head.appendChild(sheet);
	var anim = "@"+prefix+"keyframes burnseffect { 10% { " + prefix + "transform: scale(1); } 90% { " + prefix + "transform: scale(" + scalar +" ) translate(" + moveX +"%," + moveY + "%); } 100% { " + prefix + "transform: scale(" + scalar + ") translate(" + moveX +"%," + moveY +"%); } }";
	sheet.appendChild(document.createTextNode(anim));
	document.head.appendChild(sheet);
	monae = document.querySelector("figure#burnsbox img");
	monae.style.webkitAnimationName = 'burnseffect';
	monae.style.mozAnimationName = 'burnseffect';
	monae.style.animationName = 'burnseffect';
	}

当然，你也可以使用其它的javascript方法来处理，有很多选择。

就这样，下面再看一下这个效果，的确不错。

<p data-height="614" data-theme-id="0" data-slug-hash="lgGHb" data-user="janily" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/lgGHb'>Random Ken Burns Image Animation Effect</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a></p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

