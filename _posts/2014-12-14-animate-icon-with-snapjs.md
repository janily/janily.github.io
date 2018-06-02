---
layout: post
title: 使用Snapjs来制作icon动画
categories:
- Life
tags:
- javascript
- snapjs
- svg

---

> 前面有一篇文章介绍了Snapjs这个用来操作svg的javascript库的一些基本知识，这片文章我们来通过一个实际的例子来进一步学习Snapjs的具体应用，同样也是翻译的，具体行文有改变，原文地址[Animated SVG Icon](http://codyhouse.co/gem/animate-svg-icons-with-css-and-snap/)。

开始之前可以先去这个地址体验下[demo](https://codyhouse.co/demo/animated-svg-icon/index.html)。

这篇我们会学到两方面的知识：

* 压缩优化svg代码
* 通过Snapjs这个库来操作svg并制作动画

开始之前可以去看看最终的[demo](https://codyhouse.co/demo/animated-svg-icon/index.html)


svg现在被大多数浏览器支持，ie9以上也支持，下图是浏览器的支持情况：

![image](http://pic.yupoo.com/reicky_v/Eho6E92V/medium.jpg)

在以前svg，在web开发中可能不是一个必备选项。不过随着高清设备的普及，为了适配各种设备，我们不得不输出各种1x、2x、3x各种位图。这个时候已经躺在标准里的svg即矢量图形多时，重新焕发出新的活力。矢量图可以完美适配各种分辨率，这在如今这个各种分辨率齐飞的时代，是一个非常好的选择。

今天，我们要做的是使用svg格式的icon来代替位图，并且使用**inline-svg**方式插入在html文件里。优化好svg代码后，使用css和snapjs这个库来制作一个简单的动画。虽然，现在svg的浏览器支持还不是很理想，不过随着浏览器对标准的支持越来越好，2015年svg应该会得到越来越普遍的应用(国外对svg的应用已经越来越多了)。

如果你对svg还不是很熟悉，可以去下面的这些链接了解了解：

* [Resolution Indipendence with SVG ](http://www.smashingmagazine.com/2012/01/16/resolution-independence-with-svg/)
* [Using SVG](http://css-tricks.com/using-svg/)
* [Styling and Animating Scalable Vector Graphics with CSS](https://www.youtube.com/watch?v=lf7L8X6ZBu8&list=PL37ZVnwpeshHAnqFlTxhd0MIXWjLBbM3R&index=4)

### 设计svg文件

现在很多的矢量设计软件都支持导出svg格式文件，比如Adobe Illustrator或者是Sketch。使用你擅长的软件来设计，或者是下载我们已经设计好的资源。使用矢量来设计，有一个位图无法比拟的优势就是可以自由放大缩小而不失真。

有点要注意的是，你图层的组织会影响到最终代码的输出。

比如，我们要这的这个动画：第一个icon的动画有两个步骤，我就创建了两个图层，一个是正在加载的动画一个是最终加载完毕的动画。在ai中，图层的名字在输出的时候会作为id名分配给对应的路径。

![](https://ws2.sinaimg.cn/large/006tKfTcgy1frwtfn72n2j318g0oi0vh.jpg)

在输出的时候，默认的设置就可以了。不过，为了能使用样式来定义svg，我会选择入下图所示的选项：

![](https://ws2.sinaimg.cn/large/006tKfTcgy1frwtfqx8jjj318g0t4aeo.jpg)

在输出的时候，有以下要注意的几点：

* 你组织的图层会影响到输出的最终的代码
* 给图层命好名
* 设计的时候使用简单的图形删除不必要的一些锚点(可以在ai中，选择object>path>simplify)
* 保持代码简洁
* 各种矢量的设计软件对svg支持和处理也不大相同。在ai中还可以支持svg滤镜的，不过浏览器支持并不是很好。

### 压缩优化svg代码

在之前，我并不会使用一些自动化的[工具](https://github.com/svg/svgo-gui)来优化代码。不过，自从我使用设计软件来设计的并导出为svg代码的时候，发现有必要优化代码。

首先我们来看下用ai导出的代码：

	<?xml version="1.0" encoding="utf-8"?>
	<!-- Generator: Adobe Illustrator 18.1.0, SVG Export Plug-In . SVG Version: 6.00 Build 0)  -->
	<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
	<svg version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" width="200px"
	 height="200px" viewBox="0 0 200 200" style="enable-background:new 0 0 200 200;" xml:space="preserve">
	<style type="text/css">
	.st0{fill:none;stroke:#D7DCE0;stroke-width:4;stroke-miterlimit:10;}
	.st1{fill:none;stroke:#A84D54;stroke-width:4;stroke-miterlimit:10;}
	.st2{opacity:0;fill:#FFFFFF;}
	.st3{fill:#223443;}
	.st4{fill:#FFFFFF;stroke:#223443;stroke-width:4;stroke-miterlimit:10;}
	.st5{fill:none;stroke:#223443;stroke-width:4;stroke-miterlimit:10;}
	</style>
	<g id="cd-loading">
	<g id="cd-circle">
		<circle id="cd-loading-circle" class="st0" cx="100" cy="100" r="77.5"/>
		<circle id="cd-loading-circle-filled" class="st1" cx="100" cy="100" r="77.5"/>
	</g>
	<g id="cd-play-btn">
		<rect x="84" y="78.1" class="st2" width="32" height="43.9"/>
		<polygon class="st3" points="84,78.1 116,100 84,121.9 		"/>
	</g>
	<g id="cd-pause-btn">
		<rect x="81" y="80.1" class="st2" width="38" height="39.9"/>
		<rect x="81" y="80.1" class="st3" width="11" height="39.9"/>
		<rect x="108" y="80.1" class="st3" width="11" height="39.9"/>
	</g>
	</g>
	<g id="cd-buildings">
	<g id="cd-home-3">
		<rect id="cd-home-3-base" x="66" y="55" class="st4" width="58" height="123"/>
		<polygon id="cd-home-3-roof" class="st4" points="59.4,56 95,15 130.6,56 		"/>
		<circle id="cd-home-3-window" class="st4" cx="95" cy="77" r="11"/>
	</g>
	<g id="cd-home-2">
		<rect id="cd-home-2-base" x="98" y="132" class="st4" width="69" height="46"/>
		<rect id="cd-home-2-roof" x="98" y="122" class="st4" width="69" height="10"/>
		<rect id="cd-home-2-door" x="141" y="155" class="st4" width="15" height="22"/>
		<rect id="cd-home-2-window" x="106" y="143" class="st4" width="25" height="9"/>
	</g>
	<g id="cd-home-1">
		<rect id="cd-home-1-base" x="29" y="109" class="st4" width="69" height="69"/>
		<rect id="cd-home-1-roof" x="23" y="100" class="st4" width="80" height="9"/>
		<rect id="cd-home-1-door" x="53" y="151" class="st4" width="20" height="27"/>
		<rect id="cd-home-1-window" x="53" y="121" class="st4" width="20" height="18"/>
		<rect id="cd-home-1-chimney" x="34" y="89" class="st4" width="11" height="11"/>
	</g>
	<line id="cd-floor" class="st5" x1="2" y1="178" x2="198" y2="178"/>
	<g id="cd-cloud-1">
		<line class="st0" x1="5" y1="13" x2="29" y2="13"/>
		<line class="st0" x1="35" y1="13" x2="44" y2="13"/>
		<line class="st0" x1="72" y1="27" x2="14" y2="27"/>
	</g>
	<g id="cd-cloud-2">
		<line class="st0" x1="191" y1="68" x2="159" y2="68"/>
		<line class="st0" x1="153" y1="68" x2="144" y2="68"/>
		<line class="st0" x1="179" y1="83" x2="133" y2="83"/>
	</g>
	</g>
	</svg>
	
正如你看到的，ai在代码中为我们创建了一些类以及ID。当然，ai输出的代码也很简洁。

我们如果要在html中插入svg代码，只需要把**&lt;svg&gt;**标签中的代码粘贴到html文件中就可以了。

不过，我们也可以给svg代码中，可以添加title和description，提高代码的可访问性：

	<body>
	<svg version="1.1" x="0px" y="0px" width="200px" height="200px" viewBox="0 0 200 200" style="enable-background:new 0 0 200 200;" xml:space="preserve">
	<title>Buildings</title>
	<desc>3 minimal buildings with floating clouds</desc>
	<style type="text/css">
	<!-- ... -->
	</style>
	<!-- SVG elements here -->
	</svg>
	</body>
	
下面一步，是来添加一些类名和css：

	/* -------------------------------- 
 
	Manage colors
 
	-------------------------------- */
	.cd-stroke {
  	fill: none;
  	stroke-width: 4;
  	stroke-miterlimit: 10;
	}
 
	.cd-stroke-color-1 {
  	stroke: #223443;
  	fill: #f8f8f8;
	}
	.cd-stroke-color-1#floor {
  	fill: none;
	}
 
	.cd-stroke-color-2 {
  	stroke: #D7DCE0;
	}
 
	.cd-stroke-color-3 {
  	stroke: #A84D54;
	}
 
	.cd-fill-color-1 {
  	fill: #223443;
	}
 
	.cd-pointer {
  	fill: #FFFFFF;
  	opacity: 0;
	}
	
我们可以使用css类来自定义svg的样式。

当然，我也调整了下代码的结构，比如元素坐标之类的。
	
	<svg id="cd-animated-svg" version="1.1" x="0px" y="0px" width="200px" height="200px" viewBox="0 0 200 200" style="enable-background:new 0 0 200 200;" xml:space="preserve">
	<title>Buildings</title>
	<desc>3 minimal buildings with floating clouds</desc>
	<g id="cd-loading">
		<g id="cd-circle">
			<circle id="cd-loading-circle" class="cd-stroke cd-stroke-color-2" cx="100" cy="100" r="77.5"/>
			<circle id="cd-loading-circle-filled" class="cd-stroke cd-stroke-color-3" cx="100" cy="100" r="77.5"/>
		</g>
		<g id="cd-play-btn">
			<rect class="cd-pointer" x="84" y="78" width="33" height="44"/>
			<polygon class="cd-fill-color-1" points="84,78 116,100 84,122 	"/>
		</g>
		<g id="cd-pause-btn">
			<rect class="cd-pointer" x="81" y="80" width="38" height="40"/>
			<rect class="cd-fill-color-1" x="81" y="80" width="11" height="40"/>
			<rect class="cd-fill-color-1" x="108" y="80" width="11" height="40"/>
		</g>
	</g>
	<g id="cd-buildings">
		<g id="cd-cloud-1">
			<line class="cd-stroke cd-stroke-color-2" x1="5" y1="13" x2="29" y2="13"/>
			<line class="cd-stroke cd-stroke-color-2" x1="35" y1="13" x2="44" y2="13"/>
			<line class="cd-stroke cd-stroke-color-2" x1="72" y1="27" x2="14" y2="27"/>
		</g>
		<g id="cd-cloud-2">
			<line class="cd-stroke cd-stroke-color-2" x1="191" y1="68" x2="159" y2="68"/>
			<line class="cd-stroke cd-stroke-color-2" x1="153" y1="68" x2="144" y2="68"/>
			<line class="cd-stroke cd-stroke-color-2" x1="179" y1="83" x2="133" y2="83"/>
		</g>
		<g id="cd-home-3">
			<polygon id="cd-home-3-roof" class="cd-stroke cd-stroke-color-1" points="59.4,55 95,15 130.6,55"/>
			<rect id="cd-home-3-base" class="cd-stroke cd-stroke-color-1" x="66" y="54" width="58" height="124"/>
			<circle id="cd-home-3-window" class="cd-stroke cd-stroke-color-1" cx="95" cy="77" r="11"/>
		</g>
		<g id="cd-home-2">
			<rect id="cd-home-2-base" class="cd-stroke cd-stroke-color-1" x="98" y="132" width="70" height="46"/>
			<rect id="cd-home-2-roof" class="cd-stroke cd-stroke-color-1" x="98" y="122" width="70" height="10"/>
			<rect id="cd-home-2-door" class="cd-stroke cd-stroke-color-1" x="141" y="156" width="16" height="22"/>
			<rect id="cd-home-2-window" class="cd-stroke cd-stroke-color-1" x="106" y="143" width="26" height="10"/>
		</g>
		<g id="cd-home-1">
			<rect id="cd-home-1-chimney" class="cd-stroke cd-stroke-color-1" x="34" y="89" width="11" height="11"/>
			<rect id="cd-home-1-base" class="cd-stroke cd-stroke-color-1" x="29" y="108" width="70" height="70"/>
			<rect id="cd-home-1-roof" class="cd-stroke cd-stroke-color-1" x="23" y="100" width="81" height="9"/>
			<rect id="cd-home-1-door" class="cd-stroke cd-stroke-color-1" x="53" y="150" width="20" height="28"/>
			<rect id="cd-home-1-window" class="cd-stroke cd-stroke-color-1" x="53" y="121" width="20" height="18"/>
		</g>
		<line id="cd-floor" class="cd-stroke cd-stroke-color-1" x1="2" y1="178" x2="198" y2="178"/>
	</g>
	</svg>
	
OK，svg代码已经准备好了，接下来就是让它动起来了。

我把svg代码插入到类名为**.cd-svg-container**的结构中，并且把icon居中。并且把svg代码的宽度设置为**max-width:100%**，这样它会自适应屏幕的大小就像设置img一样。你可能也会注意到设置了svg的一个属性**overflow:hidden**，是因为在动画中，云会运动到画布之外，在IE下会有一个问题，会把它们显示出来，所以就设置了**overflow:hidden**。

当然，你也会注意到把矩形**.cd-pointer**放在**cd-pause-btn**元素中。这样做只是为了把播放按钮表现为可点击的形式，所以就把它的透明度设置为0**opacity:0**。

我定义了**.cd-fade-out**这个类就是为了使用javascript来控制加载完动画后元素的**cd-loading**的控制。

最后，定义了一个**.no-js**个类是为了判断当浏览器没有加载javascript的时候，就不显示加载按钮，但会显示建筑的图片。

	.cd-svg-container {
  	width: 90%;
  	max-width: 200px;
  	margin: 0 auto 100px;
	}
	.cd-svg-container svg {
  	display: block;
  	overflow: hidden;
  	max-width: 100%;
	}
	.no-js .cd-svg-container {
  	height: 200px;
  	background: url("../img/cd-icon.svg") no-repeat center center;
	}
	.no-js .cd-svg-container svg {
  	display: none;
	}
 
	/* -------------------------------- 
 
	Main elements - Loading
 
	-------------------------------- */
	#cd-loading {
  	opacity: 1;
  	visibility: visible;
  	-webkit-transition: opacity .3s 0s, visibility 0s 0s;
  	-moz-transition: opacity .3s 0s, visibility 0s 0s;
  	transition: opacity .3s 0s, visibility 0s 0s;
	}
	#cd-loading.fade-out {
  	opacity: 0;
  	visibility: hidden;
  	-webkit-transition: opacity .3s 0s, visibility 0s .3s;
  	-moz-transition: opacity .3s 0s, visibility 0s .3s;
  	transition: opacity .3s 0s, visibility 0s .3s;
	}
 
	#cd-play-btn, #cd-pause-btn {
  	cursor: pointer;
	}
 
	#cd-pause-btn {
  	pointer-events: none;
	}
 
	.play-is-clicked #cd-pause-btn {
  	pointer-events: auto;
	}
 
	/* -------------------------------- 
 
	Main elements - Buildings 
 
	-------------------------------- */
	#cd-home-1-chimney, #cd-home-3-roof {
  	visibility: hidden;
	}
	
### 让svg动起来

为了能使svg的动画能跨浏览器运行主要是为了能使IE支持，我们使用javascript来制作svg动画。当然你也可以使用SMIL来制作svg动画，可以去看看[这篇文章](http://css-tricks.com/guide-svg-animations-smil/)。

在开始的时候，建筑的图片是隐藏的。所以，我修改了一些svg的属性。

比如，我们来看看**#cd-home-1-door**这个元素，就是建筑图片左边的第一个房子。我们想让它在初始的时候是隐藏，出现的时候是由通过控制高度的动画来显示它即高度从0到28px。所以，在html结构中，我们把高度设置为0**height="0"**代替原来的28px并且添加了**data-height="28"**这样在js中可以很容易来它的高度。

	<rect id="cd-home-1-door" class="cd-stroke cd-stroke-color-1" x="53" y="150" width="20" height="0" data-height="28"/>

我们使用Snapjs给我们提供的**animate()**方法制作动画。

	var animatingSvg = Snap('#cd-animated-svg'),
	buildingDoor1 = animatingSvg.select('#cd-home-1-door');
 
	buildingDoor1.animate({'height': buildingDoor1.attr('data-height')}, 300, mina.easeinout);
	
我们可以看到门是有个从顶部到底部扩展的效果，所以我们旋转元素了180deg：

	<rect id="cd-home-1-door" class="cd-stroke cd-stroke-color-1" x="53" y="150" width="20" height="0" transform="rotate(180 63 164)" data-height="28"/>

这里注意的是：在开始的时候准备使用css3中的transform来旋转，不过在IE11中确不支持使用css3中的transform来控制svg。

**rotate()**方法接受三个参数：第一个参数是旋转的角度，第二个和第三个参数是控制旋转的坐标的。

现在所有建筑图片里的元素**#cd-buildings**都是隐藏的，我们先来处理加载动画的部分**#cd-loading**。首先，隐藏**#cd-pause-btn**元素。我们使用了transform属性来制作一个放大的动画：

	<g id="cd-pause-btn" transform="translate(100 100) scale(0)">
	<!-- .... -->
	</g>
	
要注意的一点点是：**scale()**方法不接受往里面传参数，而在svg中，transform的原点是在左上角，如果我们要控制原点的位置，我们要通过一些其它方法来达到，如下代码所示：

	transform="translate(-centerX(factor- 1) -centerY(factor- 1)) scale(factor)"

当我们指定factor的值为0的时候那(centerX,centerY) = (100,100)），这样translate(100 100)才能指向正确的位置。

然后，我们需要让**#cd-loading-circle-filled**元素动起来，就是当你按下开始的按钮的时候。为了创建这个动画，我们需要用到**stroke-dasharray**和**stroke-dashoffset**这两个属性。想象一下，这个圆是一条虚线，而stroke-dasharray可以让我们定义虚线的长度和虚线之间的距离，而stroke-dashoffset可以让我们定义虚线开始的起点。代码如下：

	var loadingCircle = animatingSvg.select('#cd-loading-circle-filled'),
	circumf = Math.PI*(loadingCircle.attr('r')*2);
 
	loadingCircle.attr({
	'stroke-dasharray': circumf+' '+circumf,
	'stroke-dashoffset': circumf,
	});
	
上面的代码中，我们定义了虚线和虚线之间的距离都是等于圆的周长，设置了**stroke-dashoffset:circumf**所以动画开始的时候是从头开始到。为了能是创建这个加载的动画，需要来控制**stroke-dashoffset**属性：

	var playBtn = animatingSvg.select('#cd-play-btn').
	globalAnimation;
 
	playBtn.click(function(){	
	var strokeOffset = loadingCircle.attr('stroke-dashoffset').replace('px', '');
	//animate strokeOffeset desn't work with circle element - we need to use 	Snap.animate() rather than loadingCircle.animate()
	globalAnimation = Snap.animate(strokeOffset, '0', function( value ){ 
		loadingCircle.attr({ 'stroke-dashoffset': value })
		}, 1500, mina.easein
	);
	});
	
这里也有注意的一点的是：stroke-dashoffset属性不能直接使用animate()方法来控制(不过对于path元素确能直接使用)，这也是为什么使用globalAnimation变量来存储使用snap方法中animate方法返回stop状态。当点击暂停按钮的时候，使用globalAnimation来暂停动画。

当加载动画完成后，就开始出现建筑的动画以及移动云朵，使用的技术跟加载动画的技术差不多。这里就不阐述了。

OK，整个就完成了，现在所希望的是浏览器对svg支持的越来越好，我们也可以看到使用svg不仅仅它是矢量的，也可以使用它来创建复杂的动画效果。










