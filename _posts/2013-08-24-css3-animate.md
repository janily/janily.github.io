---
layout: post
title: css3动画学习
categories:
- Life
tags:
- css3
---

css3动画动画属性出现以前，网页上的动画要么是Flash做的或者就是javascript来做，当然得牺牲浏览器的性能了。

自从CSS3出了个动画属性出来后，我们做交互动画又多了一个选择。特别是移动设备这种对性能特别敏感的设备，就更能体现它的优势了。当然做起一些复杂的动画来，还是要配合下javascript。

好了废话不多说，首先来学习下CSS3动画的一些基本知识，再结合下实际例子来加深理解。

说之前，还是要看看浏览器对CSS3动画的支持度

Internet Explorer 10、Firefox 以及 Opera 支持 animation 属性。Safari 和 Chrome 支持替代的 -webkit-animation 属性。Internet Explorer 9 以及更早的版本不支持 animation 属性。

CSS3动画的用法如下

使用简写属性，将动画与 div 元素绑定：

    div
	{
	animation:mymove 5s infinite;
	-webkit-animation:mymove 5s infinite; /* Safari 和 Chrome */
	}

animation 属性是一个简写属性，用于设置六个动画属性。

### CSS3动画属性 ###

css3的动画主要有六个属性，分别如下

语法是

    animation: name duration timing-function delay iteration-count direction;

- animation-name 规定需要绑定到选择器的 keyframe 名称。（animation-name 属性为 @keyframes 动画规定名称。）
- animation-duration 规定完成动画所花费的时间，以秒或毫秒计。
- animation-timing-function 规定动画的速度曲线。
- animation-delay 规定在动画开始之前的延迟。
- animation-iteration-count 规定动画应该播放的次数。（这里一般都用infinite这个值，规定动画应该无限次播放。）
- animation-direction 规定是否应该轮流反向播放动画。

这里需要注意的是：请始终规定 animation-duration 属性，否则时长为 0，就不会播放动画了。

下面我们就结合实际的例子来加深对CSS3动画属性的理解。

首先来看看这个效果是什么样子的

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/zXcMU/7/embedded/result,js,html,css" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

### HTML结构和CSS样式 ###

我们先来创建下这个动画所需要的html结构和一些样式

#### HTML结构 ####

	<div class="loading">
		<div class="spinner">
			<div class="mask">
				<div class="maskedCircle"></div>
			</div>
		</div>
	</div>

#### CSS样式 ####

    @-webkit-keyframes spin {
	from { -webkit-transform: rotate(0deg); }
	to { -webkit-transform: rotate(360deg); }
	}

	@-moz-keyframes spin {
	from { -moz-transform: rotate(0deg); }
	to { -moz-transform: rotate(360deg); }
	}

	@-o-keyframes spin {
	from { -o-transform: rotate(0deg); }
	to { -o-transform: rotate(360deg); }
	}

	@keyframes spin {
		from { transform: rotate(0deg); }
		to { transform: rotate(360deg); }
	}

	.loading {
		position: absolute;
		top: 50%;
		left: 50%;
		width: 28px;
		height: 28px;
		margin: -14px 0 0 -14px;
	}

	.loading .maskedCircle {
		width: 20px;
		height: 20px;
		border-radius: 12px;
		border: 3px solid white;
	}

	.loading .mask {
		width: 12px;
		height: 12px;
		overflow: hidden;
	}
	.loading .spinner {
		position: absolute;
		left: 1px;
		top: 1px;
		width: 26px;
		height: 26px;
		-webkit-animation: spin 1s infinite linear;
		-moz-animation: spin 1s infinite linear;
		-o-animation: spin 1s infinite linear;
		animation: spin 1s infinite linear;
	}

这里为了达到我们需要的圆形效果，主要是用类名为mask的div和类名为maskedCircle的div来制作的，用一个遮罩层盖在maskedCircle层的上面配合overflow属性制作我们想要的效果。

当然现在还需要加上浏览器的前缀才能在各个浏览器下正常工作。

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/zXcMU/7/embedded/result,js,html,css" allowfullscreen="allowfullscreen" frameborder="0"></iframe>





