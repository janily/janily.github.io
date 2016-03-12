---
layout: post
title: Focal Point之CSS响应式图片解决方案新思路
categories:
- Life
tags:
- css
- 翻译
---

随着各种移动终端设备尺寸层出不穷，网页中对图片的适配要求也越来越高。如何让图片在各种不同设备上完美适应显示，现在也有很多不同的解决方法。

像CSS的媒体查询配合background-size的解决方案，以及CSS image Level 4 中就实现了这种原生语法的image-set，通过image-set选择不同的图片的解决方法。可以去看看[这篇](http://www.iyunlu.com/view/Front-end/70.html)文章，有详细的介绍。

今天我们就来说说除了以上解决方案以外另外两种也是关于响应式图片解决方案的新思路。

首先我们要讲的也是关于响应式图片的一个解决方案，不过是一个CSS的方法库。名字叫做focal-point，这里是它的[github](https://github.com/adamdbradley/focal-point#readme)地址。

###关于Focal Point

Focal Point是一个关于响应式图片解决方案的CSS框架，作者是Adam Bradley。在过去的一些关于响应式解决方法中大部分都是通过判断浏览器的窗口大小来改变图片的尺寸从而达到响应式的目的。而Focal Point则是通过剪裁图片显示区域来适应浏览器窗口的。并没有改变图片的尺寸。

通过下面的图片就可以很清楚的明白了

<img src="http://pic.yupoo.com/reicky_v/D4Be4mW5/medium.jpg" />

图片中的大图是就是正常显示的，而小图就是经过剪裁等比例显示的。

如果仅仅是没有规则的剪裁，这里就会有一个问题？就是有可能你把一张图片重要的部分给剪裁掉了。还是回到我们上面那一张图片，可以看出在那张图片中，右边是图片的主要部分。那我们怎么做到在剪裁的时候不要把主要的部分不剪裁掉呢？

Focal Point就能让我们指定剪裁区域，从而不把图片的主要部分不剪裁掉。

如图 

<img src="http://pic.yupoo.com/reicky_v/D4BAlykR/medium.jpg" />

###Focal Point是如何做到的

Focal Point用了一个很巧妙的方法来实现指定剪裁的区域。首先我们来讲讲Focal Point是通过啥原理来实现指定剪裁区域的。然后我们再通过编码来实现它。

当我们用Focal Point的方法在页面中插入一张图片的时候，图片上面就会自动产生一个无形的12*12的网格遮罩层，这个网格遮罩层会自适应图片的大小。如下图

<img src="http://pic.yupoo.com/reicky_v/D4BDpn1s/medium.jpg" />

知道了图片是如何被分割后，我们得为Focal Point的网格系统指定一个特别的地方作为Focal Point的剪裁区域。什么意思呢？就是当我们选择指定一个剪裁区域的时候，那么Focal Point将围绕我们指定的剪裁区域为中心来剪裁图片。所以当我们选择途中人物的头部作为剪裁区域的时候，就可以保证我们指定的剪裁区域不被剪裁掉。

在剪裁的过程中，将从网格的中心计算找到我们指定的剪裁区域，也就是途中人物的脸。在这张图片中可以看到人物的脸距离网格中心向上是三个单位和向右三个单位。如图

<img src="http://pic.yupoo.com/reicky_v/D4BKmbZW/medium.jpg" />

分析我们后，接着我们分析下怎么用代码来实现它

###代码实战

首先我们得下载Focal Point这个库，地址在[这里](https://github.com/adamdbradley/focal-point)。

下载完后，来设置下我们的html结构。下面就是一个基本的html结构

<pre>
<code>
&lt;div class="focal-point"&gt;
  &lt;div>&lt;img src="guy.jpg" alt="guy"&gt;&lt;/div&gt;
&lt;/div&gt;
</code>
</pre>

在上面的结构中，最外面的div的类名是focal-point，而图片img被一个没有类名的div包裹着。

####CSS代码编写

结构写完后，接下来就是用focal-point中的css类名，来指定我们要剪裁的区域。上面已经说过我们要指定的剪裁区域是距离网格中心向上是三个单位和向右三个单位。在focal-point中用“right-3″ 和 “up-3″这两个类名来指定我们的剪裁区域。写法如下

<pre>
<code>
&lt;div class="focal-point right-3 up-3"&gt;
  &lt;div>&lt;img src="guy.jpg" alt="guy"&gt;&lt;/div&gt;
&lt;/div&gt;
</code>
</pre>

如图所示

<img src="http://pic.yupoo.com/reicky_v/D4BOcXRt/medium.jpg" />

编写完后，我们的这张图片无论是在正常屏幕下还是在缩放屏幕里，保持图片始终能够围绕在我们指定的剪裁区域内自适应浏览器视窗大小的变化。

如图

<img src="http://pic.yupoo.com/reicky_v/D4BRCqD2/medium.jpg" />

从上面图片中可以看到，右边不仅仅是变小拉，还把在我们指定的剪裁区域以外的部分被剪裁掉。没有做不到，只有想不到。仅仅是用css就能实现这么神奇的效果。怎么办到的呢？再通过一个例子来来好好的理解理解focal-point。

###翻滚吧！！！

首先我们来写下基本的结构。在上面的图片的基础上，增加了一张图片。

<pre>
<code>
&lt;div class="column"&gt;
  &lt;h1>Focal Point&lt;/h1&gt;
  &lt;p>Lorem ipsum...&lt;/p&gt;
 
  &lt;div class="focal-point right-3 up-3"&gt;
   &lt;div&gt;&lt;img src="guy.jpg" alt="guy"&gt;&lt;/div&gt;
  &lt;/div&gt;
&lt;/div&gt;
 
&lt;div class="column"&gt;
  &lt;h1&gt;Focal Point&lt;/h1&gt;
  &lt;p&gt;Lorem ipsum...&lt;/p&gt;
 
  &lt;div class="focal-point right-2 up-2"&gt;
    &lt;div&gt;&lt;img src="couple.jpg" alt="couple"&gt;&lt;/div&gt;
  &lt;/div&gt;
&lt;/div&gt;
</code>
</pre>

对于第二章图片我们指定的剪裁区域是距离网格中心向上是二个单位和向右二个单位。如图所示

<img src="http://pic.yupoo.com/reicky_v/D4BWxHn6/medium.jpg" />

接下来是为我们的demo写一些简单的样式，当然主要部分是图片的自适应，这部分我们可以放心的交给
focal-point来做。当然为了自适应各种分辨率的屏幕，我们用css3的media来做了点响应式处理。代码如下，很简单的，就不解释啦。

<pre>
<code>
* {
  margin: 0;
  padding: 0;
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  -ms-box-sizing: border-box;
  box-sizing: border-box;
}
 
.column {
  float: left;
  overflow: auto;
  padding: 20px;
  width: 50%;
}
 
h1  {
  text-transform: uppercase;
  font: bold 45px/1.5 Helvetica, Verdana, sans-serif;
}
 
p {
  margin-bottom: 20px;
  color: #888;
  font: 14px/1.5 Helvetica, Verdana, sans-serif;
}
 
@media all and (max-width: 767px) {
  p {
    font-size: 12px;
  }
 
  h1 {
    font-size: 35px;
  }
}
 
@media all and (max-width: 550px) {
  h1 {
    font-size: 23px;
  }
}
</code>
</pre>

一个简单的demo就做完了，来看看吧，地址在[这里](http://designshack.net/tutorialexamples/focalpoint/index.html)。无论是在PC上看，还是在笔记本或者是各种大小的移动终端设备上看，我们可以看到图片能够很好的适应我们指定的column大小的变化。

<img src="http://pic.yupoo.com/reicky_v/D4C08val/medium.jpg" />

我们可以试着缩小浏览器的大小来看看，图片不仅仅是变的小了，重要的是会围绕着我们指定的剪裁区域来剪裁我们的图片适应浏览器窗口大小的变化。

<img src="http://pic.yupoo.com/reicky_v/D4C1pGtI/medium.jpg" />

看到此，focal-point够酷吧！

###浏览器支持

focal-point对大部分现代的浏览器基本上都支持，IE8也能够很好的工作。

###知其然，知其所以然

那么focal-point是如何做到这一点的呢？

focal-point框架的基础部分的CSS是我们平时经常用到的图片自适应的方法，当然有javascript的辅助就更好啦，但是通过CSS也可以打下一个坚实的基础。正如你所见，我们把图片的宽度和最大的宽度设置为100%，高度设置为自适应。如下

<pre>
<code>
.focal-point {
    width: 100%;
    height: auto;
    overflow: hidden;
}
 
.focal-point img {
    width: 100%;
    max-width: 100%;
    height: auto;
    -ms-interpolation-mode: bicubic;
}
 
.focal-point div {
    position: relative; 
    max-width: none; 
    height: auto;
}
</code>
</pre>

接下来就有点稍稍复杂啦。首先我们用 media query 在宽度为767px设置了一个响应点。然后就是见证奇迹的时候啦！剪裁开始。我们事先规定好的一些剪裁区域单位设置了负边距用来达到剪裁的目的。

我们可以来看看当我们制定裁剪区域为向上是三个单位和向右三个单位的时候，发生了些什么？代码如下

<pre>
<code>
@media all and (max-width: 767px) {
 
    /* 4x3 Landscape Shape (Default) */
    .focal-point div {
        margin: -3em -4em;
    }
 
    /* Landscape up (Total 6em) */
    .up-3 div {
        margin-top:    -1.5em;
        margin-bottom: -4.5em;
    }
     
    .right-3 div {
        margin-left:  -6em;
        margin-right: -2em;
    }
}
</code>
</pre>

正如你所见，这里并没用一些很高深的很牛逼的代码来达到剪裁的目的，只是很巧妙地运用了负边距和overflow:hidden这一属性配合达到了剪裁的目的。看到这里面是不是又泪流满面的感觉，不得不惊叹于作者的想象力啊！大道无形，巧夺天工，有没有。

如此精妙的富有想象力的解决方案，得赶紧在项目中使用了。

这篇文章主要是翻译自[这里](http://designshack.net/articles/css/focal-point-intelligent-cropping-of-responsive-images/)。









