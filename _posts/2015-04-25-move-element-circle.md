---
layout: post
title: 群魔乱舞－使用javascript来使元素做各种曲线运动
categories:
- Life
tags:
- javascript
- css3
- 动画

---

先来看最终的实例吧。

<iframe width="100%" height="300" src="//jsfiddle.net/heygrady/j5tHC/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

现在CSS3来做些动画已经成为了一个web页面的标配，简直就是，无动画、不web。特别是在一些营销类型的活动页面上，CSS3动画满天飞。

不过有些动画，用CSS3来做的还是有些困难滴。比如今天要说的曲线运动，用CSS的话当然可以实现一些简单的，要是稍微复杂一点的，那还是得javascript出马。

先来看一个使用CSS3动画做的一个简单圆周运动动画例子：

<iframe width="100%" height="300" src="//jsfiddle.net/0uny3s51/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

像上面这个简单，做一个圆形动画使用CSS3来做当然是不在话下。但如果要是使用碰到下面这样的动画效果，使用CSS3来做的话，只能呵呵了。

<iframe width="100%" height="300" src="//jsfiddle.net/heygrady/BLMLu/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

下面我们就一步一步使用javascript来实现这样一个曲线的动画。当然是基于jquery来实现的，这里为了更能精细到控制动画，使之能更符合真实运动原理，这里借助了两个插件[jQuery Tween和jQuery Curve](https://github.com/heygrady/curve)。

大伙都知道在flash中动画有鼎鼎大名的tween算法，在javascript中，当然也有一套tween算法的实现方案。上面用到的插件就是实现方案之一，而且它还支持**canvas**动画，这个如果使用jquery的话，就有点麻烦了。

首先我们来看下下面这个例子：

<iframe width="100%" height="300" src="//jsfiddle.net/heygrady/Rx5Wv/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

在上面的这个例子中，我们使用了canvas元素来绘制了一条曲线。从源代码可以看到。

从上面的代码可以看出，tween接受一个step函数为参数，所谓的step函数就是用来控制动画运行时间等动画方面的时间控制。而且step函数也接受两个参数(now和fx)。

* now: 每一步动画属性的数字值
* fx: 动画fx原型对象的一个引用，其中包含了多项属性，比如elem 表示前正在执行动画的元素，start和end分别为动画属性的第一个和最后一个的值，prop为进行中的动画属性。

需要注意的是step函数被每个动画元素的每个动画属性调用。

当运行tween函数的时候，当前的时间是和当前动画时间相等的。上面的实例，就是使用tween这个插件就能简单的画一条曲线出来。

事实上，上面的tween方法跟jquery自带的animate方法大同小异。下面的代码是使用jquery自带的animate方法实现同样的效果。

<iframe width="100%" height="300" src="//jsfiddle.net/heygrady/jMe5K/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

那使用tween插件和使用jquery本身自带的animate方法有什么不同呢？当你同时要改变一个元素5个属性值的时候(比如，宽、高等)的时候，jquery本身自带的方法会在每一步的时候去回调一次动画函数，会消耗很多的性能；而使用tween插件的话就不需要担心这个问题啦。

### 使用Curves插件，绘制曲线变得如此简单

Curves插件提供了一些非常有用的方法配合跟tween插件使用。Curves插件提供了一些绘制曲线的方法，比如圆，椭圆，正弦，以及贝塞尔曲线的方法，非常容易使用，不用再像上面那样去计算了。每个方法会返回比如x,y的值或者是其它选项来给tween插件调用。

<iframe width="100%" height="300" src="//jsfiddle.net/heygrady/eFDe5/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

上面的例子，就是使用了curve插件中方法来画一条正弦曲线。curve方法会返回三个参数，分别是：x，y和rotate方法。rotate可以用来指定给对应的元素来跟随曲线运动。从上面的代码可以看到，使用curve插件来画曲线是如此的简单。

再jquery本身的animate方法中，提供了一个简单的通过百分比的方法来执行动画的一个变化过程。比如在一个1秒钟的动画中，我们来改变元素的高度jquery将会得到元素目前的高度值和最重要变化的高度值，并且计算出两个值之间的差别。下一步，jquery将会计算动画时间。得到上面这些参数之后，jquery会计算在这一秒钟之内，高度变化的百分比。然后执行动画方法，这样就产生了我们最终看到的一个一秒钟内高度变化的动画效果。

如高度在一秒钟之内从0增加到10px。在500ms的时候，动画完成50%。在step方法中，**fx**对象中的**fx.state**的值为0.5，**fx.pos**返回的值略微有一点点不同为0.4999。所以在动画50%的时候，元素的高度是0px + 10px * 0.4999即4.999px。

**fx.pos**本质上是动画当前进度上一个百分比，这个值对于curves来说是核心。任何曲线，比如圆，有一个开始和结束的点。通过一个简单的三角函数，我们可以计算出圆形中任意一个坐标点。

通过这一点，我们就可以来画任意的曲线，比如下面这个圆。

<iframe width="100%" height="300" src="//jsfiddle.net/heygrady/X5fw4/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

上面的代码，没有使用动画的方法来画一个圆，只是简单的通过三角函数来画一个圆。可以从代码中看出，我们通过三角函数来计算出圆的坐标点，然后通过循环不断来根据坐标点绘制。当然实际中不用这么麻烦，在canvas中提供对应的方法来绘制各种形状，可以去[这里](https://developer.mozilla.org/en-US/docs/Trash/Canvas_tutorial-broken/Drawing_shapes)看看。

### 元素跟随曲线运动

在curve插件的方法中，除了返回x,y值以外，还返回了一个rotate的方法，使用它，我们就可以指定元素跟随曲线的路径运动。比如在下面的这个实例中，火箭就跟随我们绘制好的曲线在运动。

<iframe width="100%" height="300" src="//jsfiddle.net/heygrady/BLMLu/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

### 群魔乱舞

下面的这个例子，我们通过使用curve插件来绘制各种曲线，并且使火箭跟随对应的曲线的路径来运动。tween插件负责动画效果，绘制全部是绘制在canvas中。

<iframe width="100%" height="300" src="//jsfiddle.net/heygrady/j5tHC/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

原文链接

[Animating With Curves in jQuery](http://heygrady.com/blog/2011/07/20/animating-with-curves-in-jquery/)





