---
layout: post
title: 深入理解CSS3 Transform(1)
categories:
- Life
tags:
- css
---

最近这段时间，运用CSS3技术的网站越来越多，特别CSS3中的用来制作动画的transform和animation两个属性更是两大神器，给网站添加一些优雅动画就靠它们了。平时也写过一些关于动画方面的，但是一直没有深入系统的来理解它。今天先就Transformations这个属性来系统的聊一聊。

transform的含义是：改变，使…变形；转换。transform的属性包括：rotate() / skew() / scale() / translate(,) ，分别还有x、y之分，比如：rotatex() 和 rotatey() ，以此类推。配合 Transition 来使用就可以制作出优雅的动画效果。

w3c标准中的CSS3 2D模块（CSS3 2D Transformation Module）能够让我们单独用CSS就可以轻松的操纵DOM节点来完成一些变形效果。当然一些复杂的效果要使用图片和javascript来完成或者是使用像FLASH和Silverlight这类的软件来制作复杂的动画效果。

### 语法基础 ###

制作变形效果，需要用到*transform*这个属性，具体如下：

    transform: function(values);

这个属性在Firefox,IE10和Opera12浏览器中可以直接使用，不需要添加特定的前缀。

但是对已以webkit/blink为内核的chrome，safari和Opera 15+浏览器则需要添加-webkit前缀：

    -webkit-transform: function(value);

IE9则只支持2D的transform：

    -ms-transform: function(value);

而IE10支持3D的transform，也只是部分支持。

当然，需要注意的是，当你用transform和其它属性来制作动画的时候，要做到跨浏览器保持一致还是有点困难的。

上面，对于transform的语法有了一个基本的阐述，下面先来聊聊transform 2D这个东东。

#### translate, translateX and translateY ####

*transform*这个属性是用来使元素从其当前位置移动，根据给定的 left（x 坐标） 和 top（y 坐标） 位置参数，坐标可以是像px、%、或者是em等单位，看一个例子：

    transform: translate(-200%, 100px);

具体运行效果如下 

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/hVDmM/embedded/result,js,html,css" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

这个例子中，我们是box这个元素像左移动200%和向下移动100px,也就是translate(x,y),它表示对象进行平移，按照设定的x,y参数值,当值为负数时，反方向移动物体，其基点默认为元素 中心点，也可以根据transform-origin进行改变基点。如transform:translate(100px,20px)。

如果你只是需要移动X轴或者是Y轴，你可以使用*translateX* 或者 *translateY*来移动。

你可能会想，在css2中，用margin或者是定位也可以移动元素，这个translate有啥意义呢？正所谓存在即合理，它当然有它的用处，要不然W3C标准的制定者们没事干么，搞这个东西玩玩，当然不是。比如，你可以使用*margin:0 auto*来使一个元素居中，但这也只是在知道元素具体宽度的时候有用，如果你想移动一个不知道宽度的元素，*margin*可能就没那么容易办到了，这时候*translate*就可以很容易办到。

#### scale, scaleX and scaleY ####

scale是用来缩放元素的。他也具有三种情况：scale(x,y)使元素水平方向和垂直方向同时缩放（也就是X轴和Y轴同时缩放）；scaleX(x)元素仅水平方向缩放（X轴缩放）；scaleY(y)元素仅垂直方向缩放（Y轴缩放），但它们具有相同的缩放中心点和基数，其中心点就是元素的中心位置，表示使元素在X轴和Y轴同时缩放。

scale(<number>[, <number>]);<number>表示缩放倍数，可以是正数，负数和小数。负数是先翻转元素然后再缩放。包含两个参数，如果缺少第二个参数，那么第二个参数的值等于第一个参数。

例如放大一个元素2倍，你可以这样做

    transform: scale(2);

当然，我们也可以通过scale(x,y)使元素水平方向和垂直方向同时缩放，比如：

    transform: scale(0.5, 2);

你也可以用*scaleX *或者*scaleY* 来缩放具体方向的值。

当然，它也接受负数，是负数的时候就是缩小啦，比如

    transform: scale(-1, 1.5);

我们具体看一个例子：

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/MSCKZ/embedded/result,js,html,css" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

#### rotate ####

rotate()函数能够旋转元素，它主要是在二维空间内进行操作，通过一个角度参数值，来设定旋转的幅度。如果对元素本身或者元素设置了perspective值，那么rotate3d()函数可以实现一个3维空间内的旋转。

具体语法是：

    rotate(<angle>);<angle>为一个角度值，单位deg，可以为正数或者负数，正数是顺时针旋转，负数是逆时针旋转。
	rotateX(angele),相当于rotate3d(1,0,0,angle)指定在3维空间内的X轴旋转
	rotateY(angele),相当于rotate3d(0,1,0,angle)指定在3维空间内的Y轴旋转
	rotateZ(angele),相当于rotate3d(0,0,1,angle)指定在3维空间内的Z轴旋转

transform的参照点默认为元素的中心点，如果要改变这个参照点，可以是用transform-origin属性进行自定义。

    transform-origin: [ [ <percentage> | <length> | left | center | right ] [ <percentage> | <length> | top | center | bottom ]? ] | [ [ left | center | right ] || [ top | center | bottom ] ]

具体取值

该属性提供2个参数值，第一个用于横坐标，第二个用于纵坐标；如果只提供一个，该值将用于横坐标，纵坐标将默认为50%。

percentage：用百分比指定坐标值。可以为负值。

length：用长度值指定坐标值。可以为负值。

left center right是水平方向取值，而top center bottom是垂直方向的取值。

比如

    /* rotate around top-left of element */
	transform-origin: left top;

还是来看一个例子吧

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/HrLc5/1/embedded/result,js,html,css" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

#### skewX 和 skewY ####

skew()函数能够让元素倾斜显示。

语法是

    transform:skew(<angle> [,<angle>]);

具体取值

- skew(<angle> [, <angle>]);包含两个参数值，分别表示X轴和Y轴倾斜的角度，如果第二个参数为空，则默认为0，参数为负表示向相反方向倾斜。
- skewX(<angle>);表示只在X轴(水平方向)倾斜。
- skewY(<angle>);表示只在Y轴(垂直方向)倾斜。

比如

    transform: skewX(-10deg);

上例子

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/HRVbG/1/embedded/result,js,html,css" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

### 综合运用transform各属性 ###

当然，我们可以把transform各个属性一起来使用，用空格隔开就行了

    transform: rotate(-20deg) skewX(-10deg) scale(0.8);

例子

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/Xs4jn/embedded/result,js,html,css" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

你也可以用矩阵函数来指定一个2D变换，matrix()是矩阵函数，以一个含六值的(a,c,e,b,d,f)变换矩阵的形式指定一个2D变换，相当于直接应用一个[a c e b d f]变换矩阵。

注意：c,e的值用正弦或余弦值表示

IE虽然不支持大部分的CSS3变形，但是支持matrix滤镜。

具体取值是：

matrix(a,b,c,d,e,f);

该函数包括6个参数(a,c,e,b,d,f)，实际上它是一个3*3的矩阵:

![](http://pic.yupoo.com/reicky_v/D9UGGGLg/medium.jpg)

当然，如果你想把元素所有的transformations关掉，只需要设置为none就可以了，如下

    transform: none;

好了，2D Transform的知识差不多介绍的差不多，下一篇接着聊聊3D Transform的知识点。

	










