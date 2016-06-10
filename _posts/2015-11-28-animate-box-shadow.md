---
layout: post
title: 打造高性能的阴影动画
categories:
- Life
tags:
- css3
- animation
- box-shadow
- 前端

---

> 在前端开发中，尤其是移动端，CSS3应用的越来越广泛。当然大家都会默认遵守一些规范，特别是关乎性能方面。比如，尽量少用圆角、阴影、渐变等性能消耗大户的属性。特别是在动画中，更是如此。那今天就来看看这篇文章，使用伪元素来打造高性能的阴影动画，原文[地址](http://tobiasahlin.com/blog/how-to-animate-box-shadow/)，翻译过程中会加入自己的一些理解。

众所周知，在改变元素的**box-shadow**阴影的时候，会触发网页不停的重绘，因此会影响网页的性能。那你是怎么解决的呢？答案非常简单，能不用就不用，或者是干脆不用，这样就不会有性能问题了。

其实要解决这个问题，有现成的方案可以解决。比如，利用**opacity**属性不会触发网页重绘的这个属性，我们可以使用它来改变元素伪元素阴影的透明度来实现阴影动画效果，保证动画绘制的FPS保持在60FPS，从而解决这个性能问题。

### DEMO

![](http://tobiasahlin.com/static/animate-box-shadow/demo.gif)

大家可以来看看这个[DEMO](http://tobiasahlin.com/demo/animate-box-shadow/)来比较下两者的性能。看起来好像没什么区别，没错，它们的区别是阴影运动的实现方式。左边的是直接改变元素的阴影；而右边是改变元素伪元素**:after**阴影的**opacity**来实现的。

测试性能，怎能少了chrome浏览器的开发者工具呢！通过开发者工具很容易可以看到两者在性能上的差异：

![](http://tobiasahlin.com/static/animate-box-shadow/animation-performance.png)

可以很清楚的看到，在左边的阴影动画上，当阴影变化的时候，FPS在30左右；而右边的伪元素的动画的性能明显高于左边。

为什么会有这样的效果呢？我们知道有些[属性](http://csstriggers.com/)在改变的时候不会触发重绘，比如**opacity**和**transform**。所以我们在编写动画效果的时候，要尽量减少网页的重绘。

下面代码是这两种动画实现方式的核心代码：


```
/* The slow way */
.make-it-slow {
  box-shadow: 0 1px 2px rgba(0,0,0,0.15);
  transition: box-shadow 0.3s ease-in-out:
}

/* Transition to a bigger shadow on hover */
.make-it-slow:hover {
  box-shadow: 0 5px 15px rgba(0,0,0,0.3);
}

/* The fast way */
.make-it-fast {
  box-shadow: 0 1px 2px rgba(0,0,0,0.15);
}

/* Pre-render the bigger shadow, but hide it */
.make-it-fast:after {
  box-shadow: 0 5px 15px rgba(0,0,0,0.3);
  opacity: 0;
  transition: opacity 0.3s ease-in-out:
}

/* Transition to showing the bigger shadow on hover */
.make-it-fast:hover:after {
  opacity: 1;
}

```

从上面的代码可以看出，高性能的关键区别是，使用**opacity**来改变伪元素的阴影的透明度从而实现阴影动画，而不是直接改变元素的阴影。

### 打造高性能阴影动画

回到文章的开头，让我们来看看高性能的[阴影动画](http://tobiasahlin.com/demo/animate-box-shadow/)是如何炼成的。第一步是移动伪元素上的阴影。看下面的代码：


```
/* All HTML you need is <div class="box"></div> */

/* Create a simple white box, and add the shadow for the initial state */
.box {
  position: relative;
  display: inline-block;
  width: 100px;
  height: 100px;
  border-radius: 5px;
  background-color: #fff;
  box-shadow: 0 1px 2px rgba(0,0,0,0.15);
  transition: all 0.3s ease-in-out;
}

/* Create the hidden pseudo-element */
/* include the shadow for the end state */
.box:after {
  content: '';
  position: absolute;
  width: 100%;
  height: 100%;
  opacity: 0;
  border-radius: 5px;
  box-shadow: 0 5px 15px rgba(0,0,0,0.3);
  transition: opacity 0.3s ease-in-out;
}

```

记的给**.box**和**.box:after**这两个元素都定义下**transition**
属性。我们会改变**.box**元素的**transform**和**.box:after**元素的**opacity**属性。

OK，基本的布局搞好了，接下来是要编写鼠标经过**.box**元素的时候放大的的一个效果，以及**.box**元素的阴影的透明度：


```
/* Scale up the box */
.box:hover {
  transform: scale(1.2, 1.2);
}

/* Fade in the pseudo-element with the bigger shadow */
.box:hover:after {
  opacity: 1;
}

```

就是这么简单，效果如下所示：

<p data-height="268" data-theme-id="17491" data-slug-hash="PPMwZW" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/PPMwZW/'>PPMwZW</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

总的代码如下：


```
.box {
  position: relative;
  display: inline-block;
  width: 100px;
  height: 100px;
  background-color: #fff;
  border-radius: 5px;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
  border-radius: 5px;
  -webkit-transition: all 0.6s cubic-bezier(0.165, 0.84, 0.44, 1);
  transition: all 0.6s cubic-bezier(0.165, 0.84, 0.44, 1);
}

.box:after {
  content: "";
  border-radius: 5px;
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
  opacity: 0;
  -webkit-transition: all 0.6s cubic-bezier(0.165, 0.84, 0.44, 1);
  transition: all 0.6s cubic-bezier(0.165, 0.84, 0.44, 1)
}

.box:hover {
  -webkit-transform: scale(1.25, 1.25);
  transform: scale(1.25, 1.25)
}

.box:hover:after {
    opacity: 1
}

```

仅仅是改变效果实现的方式，还能提高性能，一举两得，为什么不使用呢？

当然，如果仅仅是在桌面设备上使用的话，直接改变元素的**box-shadow**来实现动画效果，没有什么问题。不过在移动设备上，可不是这样了。在移动设备上进行大量重绘会引起严重的性能问题。

记住一点，要使用**transform**和**opacity**属性来编写动画效果，不仅能大大的提升性能，而且也是经过大量实践的最佳实践。






