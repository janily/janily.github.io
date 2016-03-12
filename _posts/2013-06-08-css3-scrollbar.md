---
layout: post
title: 用css3自定义滚动条
categories:
- Life
tags:
- css3
---

滚动条这一web设计中的常见的元素随处可见,我们也会经常遇到设计中自定义滚动条的需求,一般碰到这种需求的时候,我一般首先会努力说服设计师用原生的滚动条,但是这样还是回避不了问题,最近在项目中遇到这个问题,真好来说一下,但是要用样式兼容所有浏览器的滚动条目前还是无法实现,如果真有这种需求的话,我一般是用javascript来解决的.

这次的需求中是在webkit为内核的浏览器中实现自定义滚动条样式的需求.

###webkit自定义滚动条样式

webkit实现了自定义的滚动条样式的支持,我们先看一个简单的demo

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/mzB6C/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

webkit实现自定义滚动条,不是用几个简单的css属性,而是一堆css伪元素来控制的,如图所示:

<img src="/media/images/scrollbarparts.png" width="570" height="448"/>


1. ::-webkit-scrollbar 滚动条整体部分
2. ::-webkit-scrollbar-button 滚动条两端的按钮
3. ::-webkit-scrollbar-track 外层轨道
4. ::-webkit-scrollbar-track-piece 内层轨道，滚动条中间部分（除去）
5. ::-webkit-scrollbar-thumb （拖动条滚动条里面可以拖动的那个）
6. ::-webkit-scrollbar-corner 边角
7. ::-webkit-resizer 定义右下角拖动块的样式

当然它还提供了一些伪类来定义滚动条的各种状态

<pre>
<code>
	:horizontal
	:vertical
	:decrement
	:increment
	:start
	:end 
	:double-button
	:single-button
	:no-button
	:corner-present
	:window-inactive
</code>
</pre>

这些伪类是啥意思呢?从神飞的博客里搬来的

* :horizontal – horizontal伪类应用于水平方向的滚动条
* :vertical – vertical伪类应用于竖直方向的滚动条
* :decrement – decrement伪类应用于按钮和内层轨道(track piece)。它用来指示按钮或者内层轨道是否会减小视窗的位置(比如，垂直滚动条的上面，水平滚动条的左边。)
* :increment – increment伪类和decrement类似，用来指示按钮或内层轨道是否会增大视窗的位置(比如，垂直滚动条的下面和水平滚动条的右边。)
* :start – start伪类也应用于按钮和滑块。它用来定义对象是否放到滑块的前面。
* :end – 类似于start伪类，标识对象是否放到滑块的后面。
* :double-button – 该伪类以用于按钮和内层轨道。用于判断一个按钮是不是放在滚动条同一端的一对按钮中的一个。对于内层轨道来说，它表示内层轨道是否紧靠一对按钮。
* :single-button – 类似于double-button伪类。对按钮来说，它用于判断一个按钮是否自己独立的在滚动条的一段。对内层轨道来说，它表示内层轨道是否紧靠一个single-button。
* :no-button – 用于内层轨道，表示内层轨道是否要滚动到滚动条的终端，比如，滚动条两端没有按钮的时候。
* :corner-present – 用于所有滚动条轨道，指示滚动条圆角是否显示。
* :window-inactive – 用于所有的滚动条轨道，指示应用滚动条的某个页面容器(元素)是否当前被激活。(在webkit最近的版本中，该伪类也可以用于::selection伪元素。webkit团队有计划扩展它并推动成为一个标准的伪类)

另外，:enabled、:disabled、:hover 和 :active 等伪类同样可以用于滚动条中。

当然,我前面做的demo只是很简单应用下,其实根据上面这些相关的元素属性,我们完全可以应对大部分的需求了.

随着web标准和浏览器技术的发展,属于我么前端的春天也不远了.


