---
layout: post
title: 深入理解CSS3 Transform(2)
categories:
- Life
tags:
- css
---

上一篇文章中，主要介绍了2D Transform方面的知识。接下来，是介绍 3D 方面的知识，当然3D比2D稍微复杂点，为了更好的兼容浏览器，需要注意到以下几点：

- 由于还没有统一标准，各个浏览器需要写对应的前缀。
- IE要到IE10以上才部分支持3D。

具体的支持可以去[这里](http://caniuse.com/#feat=transforms3d)看看。

一般而言，在一个2D平面里，有两个轴X轴和Y轴。但是在一个3D空间里，还有另外一个轴Y轴。

对于3D来说，透视的设置（perspective变换函数）才是至关重要的。该函数会设置查看者的位置，并将可视内容映射到一个视锥上，继而投影到一个 2D 视平面上。如果不指定透视，则 Z 空间中的所有点将平铺到同一个 2D 视平面中，并且变换结果中将不存透视深概念。作用于元素的子元素。

perspective有两种写法，一种是设置所有的子元素有一个共同的透视值，perspective作为一个属性值来写；一种是直接作用于元素本身，perspective作为transform的一个函数来写如：

    .wrap{-webkit-perspective:1000px;}
	.wrap .child{-webkit-transform:perspective(1000px);}

取值

    none：没有透视变换功能。
	length：指定一个透视值。

我们来看一个例子吧

    <div id="stage">
 
    <div id="mybox1">BOX 1</div>
    <div id="mybox2">BOX 2</div>
    <div id="mybox3">BOX 3</div>
 
	</div>

我们写了一个id为stage的盒子，有三个面，分别是红，蓝，绿三个颜色。外部stage这个盒子模型并不是必需的，但是它可以很方便我们控制整个盒子的透视和场景的旋转。

首先，我们使用translateZ来调整下三个面的顺序：

    #mybox2
	{
	    transform: translateZ(-300px);
	}
	 
	#mybox3
	{
	    transform: translateZ(-600px);
	}

来看下运行的效果：

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/MWWJt/1/embedded/result,js,html,css" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

呃，这可不是我们需要的效果，平面的。这个时候是transform-style出场了，它是设置内嵌的元素在 3D 空间如何呈现。有两个值：flat：所有子元素在 2D 平面呈现。preserve-3d：保留3D空间。所以，为了使我们的元素具有3D效果，我们需要设置元素以3D空间来呈现，如下：

    #stage
	{
	    transform-style: preserve-3d;
	}

我们来看看效果

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/c2KuP/2/embedded/result,js,html,css" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

不幸的是，IE10不支持transform-style: preserve-3d property这个属性。IE10中还是会2D 平面来呈现。只能寄希望于IE11了。

现在看到的三个盒子由于没有透视的关系，还是叠在一起。这个时候perspective改出场了，设置如下：

    #stage
	{
	    transform: rotateX(-20deg) rotateY(-40deg) perspective(600px);
	}

看看效果

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/vAK5y/1/embedded/result,js,html,css" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

#### translateX, translateY, translateZ 和 translate3d ####

接下来看看translateX, translateY, translateZ 和 translate3d的综合运用。

在一个3D的空间里，当然也得可以设置translateX, translateY, translateZ这些属性，一般可以这样写，如下所示：

    #mybox1
	{
	    transform: translateX(-20px) translateY(4em) translateZ(10px);
	}

但是还有translate3d这个简写方法，还可以这样写：

    #mybox1
	{
	    transform: translate3d(-20px, 4em, 10px);
	}

#### scaleX, scaleY, scaleZ 和 scale3d ####

在2D空间里，我们一般这样设置scaleX, scaleY, scaleZ：

    #mybox2
	{
	    transform: scaleX(1.3) scaleY(-1) scaleZ(1);
	}

这里需要注意的是translateZ和scaleZ两个属性，比如：

    transform: translateZ(100px) scaleZ(2);

其实相当于：

	transform: translateZ(200px);

所以，当使用这两个属性的时候要谨慎一点。

同样也有scale3d这样简写方式：

    #mybox2
	{
	    transform: scale3d(1.3, -1, 1)
	}

#### rotateX, rotateY, rotateZ 和 rotate3d ####

在一个3D空间里，也可以通过3个轴来设置元素的旋转，比如：

    #mybox3
	{
	    transform: rotateX(45deg) rotateY(10deg) rotateZ(-20deg);
	}

这里要注意的是

- rotateX(angele),相当于rotate3d(1,0,0,angle)指定在3维空间内的X轴旋转
- rotateY(angele),相当于rotate3d(0,1,0,angle)指定在3维空间内的Y轴旋转
- rotateZ(angele),相当于rotate3d(0,0,1,angle)指定在3维空间内的Z轴旋转

比如：

	transform: rotate3d(0, 1, 0, 90deg);

相当于：

    transform: rotateY(90deg);

当然，默认的是以元素的中心为基点来旋转，但是我们可以用transform-origin来改变我们的中心点，比如：

    transform-origin: right 80% -50px;

接下来，我们试试设置多个3D Transform看看：

    transform: translateZ(-600px) rotateX(45deg) rotateY(10deg) rotateZ(-20deg);

运行效果如下

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/yeUFk/1/embedded/result,js,html,css" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

如果，你还想试试更具有挑战性的，那可以试试matrix3d。这里有一个关于matrix3d的[实例网站](http://9elements.com/html5demos/matrix3d/)可以去看看。

当然，像2D Transform一样，你也可以关掉元素的transform属性：

	transform: none;

通过3D Transform看看，我们知道可以在一个3D的空间里旋转元素，比如：

    transform: rotateY(180deg);

那这个元素的效果就是这样的

    ![](http://pic.yupoo.com/reicky_v/D9WlXEa4/medium.jpg)

有时候，我们可能需要这样的效果，也可能不需要这样的效果。让我们用3D transform来制作一个当鼠标滑过纸牌翻转的效果吧。HTML代码如下，分为正反两面：

    <div class="card">
	    <div class="front"><span>A♥</span> ♥ <span>A♥</span></div>
	    <div class="back"></div>
	</div>

我们可能会想到来围绕Y轴旋转180度，来翻转纸牌。(事实上经过测试，旋转的角度最好是-179.9度，这样可以确保浏览器以最佳的路径来旋转)。

    div.card div.back
	{
	    transform: perspective(400px) rotateY(0deg);
	}
	 
	div.front
	{
	    transform: perspective(400px) rotateY(-179.9deg);
	}

现在来处理鼠标滑过翻转的效果，我们以顺时针来围绕Y轴来旋转，记得要设置perspective属性：

    div.card:hover div.back, div.card:focus div.back
	{
	    transform: perspective(400px) rotateY(179.9deg);
	}
	 
	div.card:hover div.front, div.card:focus div.front
	{
	    transform: perspective(400px) rotateY(0deg);
	}

看看效果，这可不是我们想要的效果

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/zYGj5/3/embedded/result,js,html,css" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

我们发现牌的背部永远显示在前面，因为在HTML里面是这样定义的，就算你定义元素的层级的优先级也没用。

当然，W3C标准里面同时提供了解决方案，提供了backface-visibility这个属性（-webkit-backface-visibility），backface-visibility 属性可用于隐藏内容的背面。默认情况下，背面可见，这意味着即使在翻转后，变换的内容仍然可见。但当 backface-visibility 设置为 hidden 时，旋转后内容将隐藏，因为旋转后正面将不再可见。

取值如下

    visible：默认值，旋转的时候背景可见。
	hidden：旋转的时候背景不可见。

所以我们可以把我们扑克牌的背面设置为hidden：

    div.card div
	{
	    backface-visibility: hidden;
	}

这样我们就可以解决上面的问题，看效果：

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/HuQTL/2/embedded/result,js,html,css" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

到这里，关于transitions 和 animations, 2D and 3D transform的知识介绍的差不多，我们可以运用这些知识制作更加优雅，创新的动画效果。能做到什么程度，那就要看你想象力丰不丰富了。

这两篇文章参考并整理来自下面这一个系列里面的文章

[CSS3 Transformations](http://www.sitepoint.com/series/css3-transformations/)。


