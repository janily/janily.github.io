## CSS动画
由于SVG是一种XML格式的文档，和HTML中的DOM类似，所以，SVG也能通过CSS执行动画效果。

通过CSS的关键帧（keyframes）可以很轻松的对SVG进行操控，进而执行指定的动画效果。

在开始之前，你可能会有一个疑问，使用CSS控制HTML不是也可以制作动画么，为啥还要用到SVG呢？

下面就来说说使用SVG的一些优势。

首先从一个小小的实例来看一下SVG的一些优势，比如，我这里要绘制一个六边形，我只需要在SVG编辑器中绘制一个六边形，然后导出SVG代码就可以了，代码如下：

```
<path id="svg_1" d="m213.4325,184.65295l9.21021,-42.72391l37.28971,-19.01395l37.28972,19.01395l9.21038,42.72391l-25.80602,34.26214l-41.38814,0l-25.80585,-34.26214z" stroke-width="1.5" stroke="#000" fill="#fff"/>
```

只需要三行代码就可以得到一个六边形，而且可以无限放大缩小，改变颜色也是一句代码的事儿。这代码量明显少于使用HTML和CSS来实现的代码量，你也许会说，我直接且一张图不久得了，不必SVG的代码量更小。

可是你想过么，如果后面变更了需求，需要改变它的颜色，或者是增加它的尺寸，那你不得重新在设计软件中切图调整，反复折腾，相信我，你不会愿意这样做的。

而使用SVG，比如，当我们想改变这个形状颜色的时候，只需要一句代码就可以完成：

```
fill="#000"
```

这个形状就变成黑色的，就是这么简单。

说了这么多，下面我们来一个SVG和CSS配合动画实例来初步领略下SVG动画的魅力，我们将要完成的效果如下图所示：

![图片](https://images-cdn.shimo.im/2cErXifm6P0BG2cO/ss.gif)
这个动画是由一个颜色填充动画、描边线条动画和放大动画这三个动画组成。描边线条动画可以说是SVG的独门武器了，现在在互联网上几乎是遍地开花，无处不在。

开始之前，我们先来了解下在SVG中描边动画原理，在后面的章节中，我们还会跟它经常打交道。

通过第一章节，我们了解到在SVG中，很多的形状都是由**path**元素构成的，这里就不再多介绍了，这里主要介绍和线条描边动画的3个属性，分别为**stroke，stroke-dasharray**和**stroke-dashoffset。**

stroke：是用来定义边框的颜色。

stroke-dasharray：定义 dash 和 gap 的长度。它主要是通过使用 , 来分隔实线和间隔的值。其实就是用来实现虚线的效果。和CSS中的dash的效果一样。例如：stroke-dasharray="5, 5" 表示，按照 实线为 5，间隔为 5 的排布重复下去。如下图：

![图片](https://images-cdn.shimo.im/BIE7NpnvnZcIAKLP/dash.png!thumbnail)

stroke-dashoffset：用来设置 dasharray 定义 dash 线条开始的位置。值可以为 number || percentage。百分数是相对于 SVG 的 viewport。通常结合 dasharray 可以实现线条的运动。

介绍完关于 path 的所有 stroke 属性之后，下面就来解释下SVG线条描边动画的原理。简单来说，就是通过 stroke-dashoffset 和 stroke-dasharray 来做。主要是以下两个步骤：

1. 通过 dasharray 将实线部分隐藏，空余为全线段长。然后，将实线部分增加至全长。比如：dasharray: 0,1000 变为 dasharray: 1000,1000。
2. 同时，通过 dashoffset 来移动新增的实线部分，造成线段移动的效果。有: dashoffset:0，变为 dashoffset:1000。

简简单单的两步就可以实现一个线条的描边动画。

下面就来实现我们上边的这个动画效果。

首先在软件中设计好这个图形，然后导出SVG代码：

```
<svg class="checkmark" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 52 52">
  <circle class="checkmark__circle" cx="26" cy="26" r="25" fill="none"/>
  <path class="checkmark__check" fill="none" d="M14.1 27.2l7.1 7.2 16.7-16.8"/>
</svg>
```

再来看下这个效果：

![图片](https://images-cdn.shimo.im/2cErXifm6P0BG2cO/ss.gif)

初始状态是各个元素都没有显示，需要描边的动画效果是圆圈和中间的钩，在SVG代码也可以看到，圆圈的fill属性的值是none，也就是没有填充颜色。而它们的描边动画则需要使用样式来设置，代码如下所示：

```
.checkmark__circle {
  stroke-dasharray: 166;
  stroke-dashoffset: 166;
  stroke-width: 2;
  stroke-miterlimit: 10;
  stroke: #7ac142;
  fill: none;
}

.checkmark {
  width: 56px;
  height: 56px;
  border-radius: 50%;
  display: block;
  stroke-width: 2;
  stroke: #fff;
  stroke-miterlimit: 10;
  margin: 10% auto;
  box-shadow: inset 0px 0px 0px #7ac142; 
}
.checkmark__check {
  transform-origin: 50% 50%;
  stroke-dasharray: 48;
  stroke-dashoffset: 48;
}

```
至于stroke-dasharray的值怎么获取，SVG有提供一个JavaScript的API来获取，只需要执行下面这行代码就可以获取整条path的长度：

```
var path = document.querySelector('.path');
var length = path.getTotalLength();
```
首先是圆圈和钩的线条描边动画，只需要使用关键帧把stroke-dashoffset设置为0就可以来。

```
@keyframes stroke {
  100% {
    stroke-dashoffset: 0;
  }
}
```
然后是整个画布的填充和缩放动画：

```
@keyframes scale {
  0%, 100% {
    transform: none;
  }
  50% {
    transform: scale3d(1.1, 1.1, 1);
  }
}
@keyframes fill {
  100% {
    box-shadow: inset 0px 0px 0px 30px #7ac142;
  }
}
```
最后完整代码如下所示：

```
.checkmark__circle {
  stroke-dasharray: 166;
  stroke-dashoffset: 166;
  stroke-width: 2;
  stroke-miterlimit: 10;
  stroke: #7ac142;
  fill: none;
  animation: stroke 0.6s cubic-bezier(0.65, 0, 0.45, 1) forwards;
}

.checkmark {
  width: 56px;
  height: 56px;
  border-radius: 50%;
  display: block;
  stroke-width: 2;
  stroke: #fff;
  stroke-miterlimit: 10;
  margin: 10% auto;
  box-shadow: inset 0px 0px 0px #7ac142;
  animation: fill .4s ease-in-out .4s forwards, scale .3s ease-in-out .9s both;
}

.checkmark__check {
  transform-origin: 50% 50%;
  stroke-dasharray: 48;
  stroke-dashoffset: 48;
  animation: stroke 0.3s cubic-bezier(0.65, 0, 0.45, 1) 0.8s forwards;
}

@keyframes stroke {
  100% {
    stroke-dashoffset: 0;
  }
}
@keyframes scale {
  0%, 100% {
    transform: none;
  }
  50% {
    transform: scale3d(1.1, 1.1, 1);
  }
}
@keyframes fill {
  100% {
    box-shadow: inset 0px 0px 0px 30px #7ac142;
  }
}
```
一个简单线条描边动画就完成了。

这篇教程，只是简单的介绍了下SVG配合CSS，就可以做出非常好玩的动画效果。配合CSS其它的属性，比如transform，opacity等，可以做出很多有趣的动画效果，凡是使用CSS和HTML能实现的动画效果，SVG一样可以实现。反过来，使用SVG能做的动画效果，HTML就只能望洋兴叹了。

下一章节来介绍下SVG中的蒙版在动画中的运用。





