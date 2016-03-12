---
layout: post
title: SVG基础知识入门系列(4)-SVG动画
categories:
- Life
tags:
- svg
- 翻译
---

> 本文来说一下SVG动画方面的知识，原作者主要是从SVG本身提供的的一些动画属性方法来讲的，其实SVG配合javascript能制作出更加强大的动画，现在web开发中也用它来制作一些交互动画。这系列之后，再来进一步讲讲用SVG配合javascript来制作动画方面的知识。

今天我们继续来学习SVG动画方面的知识。一听到动画效果，你可能就觉的头都打啦。不要担心，SVG本身提供的动画属性非常容易使用，今天我们就来学习一下基础的知识。

![](http://pic.yupoo.com/reicky_v/DmG6bL6l/medium.jpg)

### 基础知识 ###

SVG中提供了**animate**的方法来制作动画属性：

    <svg>  
	<rect width="200" height="200" fill="slategrey">  
	<animate attributeName="height" from="0" to="200" dur="3s"/>  
	</rect>  
	</svg>  

在上面的代码中，我们在元素里面添加了一个**&lt;animate&gt;**的标签。**&lt;animate&gt;**包含了下面的一些属性。

- attributeName 这个是用来指定元素要运动的属性
- from 这个是用来设置元素属性运动的初始值
- to 这个是用来指定运动的终止的值的
- dur 这个是用来指定动画执行的时长。它的语法格式可以去这个地址看看[Clock Value Syntax](http://www.w3.org/TR/SVG/animate.html#ClockValueSyntax)。比如*02：33*就表示2分钟33秒，如下：

![](http://pic.yupoo.com/reicky_v/DmGffl7A/medium.jpg)

比如我们要给一个圆形(*&lt;circle&gt;*)制作一个动画效果，可以这样来写：

    <svg>  
	<circle r="100" cx="100" cy="100" fill="slategrey">  
	<animate attributeName="r" from="0" to="100" dur="3s"/>  
	</circle>  
	</svg>  

具体执行效果可以去这个地址看一看[demo](http://demo.hongkiat.com/scalable-vector-graphics-animation/index.html#basic)

### 移动一个元素 ###

移动一个SVG元素，我们只要指定元素移动目标的坐标就可以了分别为**x**和**y**。

	<svg>  
	<rect x="0" y="0" width="200" height="200" fill="slategrey">  
	<animate attributeName="x" from="0" to="200" dur="3s" fill="freeze"/>  
	</rect>  
	</svg>  

在上面的代码中，我们指定把矩形元素从0移到200的位置，动画时长为3秒，这个矩形将会从左至右水平方向，移动到200的位置。这里注意一点的是在用**animate**方法的时候，有一个**fill**属性的值为**freeze**，fill="freeze" 特性；该特性可用于所有animate元素，它指示SVG查看器在动画完成时让元素“冻结”在其最终位置上。 

![](http://pic.yupoo.com/reicky_v/DmGm0DGp/medium.jpg)

我们也可以使用它来制作一个圆形的动画效果，定义**cx**和**cy**的属性值就可以了：

    <svg>  
	<circle r="100" cx="100" cy="100" fill="slategrey">  
	<animate attributeName="cy" from="100" to="200" dur="3s" fill="freeze"/>  
	</circle>  
	</svg>  

具体执行结果，可以去这个地址看看[demo](http://demo.hongkiat.com/scalable-vector-graphics-animation/index.html#moving)。

### 给多个属性执行动画 ###

上面我们只是针对一个属性来制作动画效果，我们当然也可以对多个属性来设置动画属性，我们来看一个例子：

    <svg>  
	<circle r="100" cx="110" cy="110" fill="slategrey" stroke="#000" stroke-width="7">  
	<animate attributeName="r" from="0" to="100" dur="3s"/>  
	<animate attributeName="stroke-width" from="0" to="10" dur="7s"/>  
	</circle>  
	</svg> 

这里跟上面的差不多，只不过是多了一个**animate**标签，来操纵多个属性**radius**和**stroke with**。

![](http://pic.yupoo.com/reicky_v/DmGGb1zB/medium.jpg)

### 跟随路径运动 ###

在上一篇文章**在SVG中定义文字文章中**，我们说到了可以让文字跟随路径的方向来排版文字。当然让元素跟随路径的方向来运动也可以做到，下面我们来看一个例子。

    <svg>  
  
	<defs>  
	<path id="thepath" fill="none" stroke="#000000" d="M0.905,0.643c0,0,51.428,73.809,79.047,166.19s68.095,38.572,107.144-18.095  
	c39.047-56.667,72.381-92.382,113.333-42.381S335.5,178.5,374,200.5s82-18.5,97.5-33.5"/>  
	</defs>  
	  
	<circle r="15" cx="15" cy="15" fill="slategrey">  
	  
	</svg>  

我们把路径定义在**&lt;defs&gt;**标签里，上面代码就是这样干的。为了能使元素跟随路径来运动，我们需要用**&lt;animateMotion&gt;**来引入路径**&lt;mpath&gt;**，如下所示：

    <animateMotion dur="3s">  
    <mpath xlink:href="#thepath"/>  
	</animateMotion>  

![](http://pic.yupoo.com/reicky_v/DmGWRYm3/medium.jpg)

代码执行效果可以去这个地址看看[demo](http://demo.hongkiat.com/scalable-vector-graphics-animation/index.html#path)。

### Transformations ###

我们也可以使用transformation中的**scale**，**translate**和**rotate**来制作动画效果，这个要用到**&lt;animateTransform&gt;**：

    <svg>  
	<rect width="200" height="200" fill="slategrey">  
	<animateTransform attributeName="transform" type="scale" from="0" to="1" dur="3s"/>  
	</rect>  
	</svg>  

transformation的使用方法跟CSS3中的transformation的使用方法相似，可以去这篇文章详细了解一下transformation的用法[CSS3 2d transformation](http://www.hongkiat.com/blog/css3-2d-transformation/)。

![](http://pic.yupoo.com/reicky_v/DmGYXo1t/medium.jpg)

代码执行效果可以去这个地址看看[demo](http://demo.hongkiat.com/scalable-vector-graphics-animation/index.html#transform)。

如果你很擅长用SVG来制作动画效果，可以尝试去模仿下这两个动画效果来创建两个动画效果[效果一](http://www.carto.net/svg/samples/jumping_cubes.svg)和[效果二](http://devfiles.myopera.com/articles/26/ex-c03.svg)。

用SVG创建动画效果比用Flash创建动画效果要好的一个优势是，不需要依赖第三方插件以及运行的性能也要比Flash要好。而且现在Adobe公司也停止了flash对安卓设备的支持，现在是时候在你的项目中，用SVG来创建动画效果了。

推荐阅读：

- [20 SVG Uses That Will Make Your Jaw Drop](http://www.netmagazine.com/features/20-svg-uses-will-make-your-jaw-drop)
- [SVG Animate Documentation](http://www.w3.org/TR/SVG/animate.html)
- [Advanced SVG Animation Techniques](http://dev.opera.com/articles/view/advanced-scalable-vector-graphics-animation-techniques/)

[Demo](http://demo.hongkiat.com/scalable-vector-graphics-animation/index.html)，[源代码下载](http://demo.hongkiat.com/scalable-vector-graphics-animation/source.zip)。           