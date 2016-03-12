---
layout: post
title: 如何来创建流畅的动画效果
categories:
- Life
tags:
- javascript
- 翻译
---

在制作动画效果时，浏览器为使动画效果更佳流畅，需要避免在执行动画效果任务的时候执行其它诸如javascript、样式计算、布局以及重绘等计算任务。不过，现代浏览器可以把一些主线程的任务委托给GPU来计算执行。利用GPU执行多个任务，最终合成最终的效果在屏幕上呈现出来。

在这篇文章中，我们主要是来关注在执行**animations**和**transitions**的渲染性能，因为它们是影响性能的一个主要因素。

### Animations和Transitions的性能如何 ###

当我们给一个元素执行动画效果的时候，浏览器会重新计算整个页面，以确定元素的变动是否会影响其它元素以及整个页面的dom节点和布局属性。如果该次变化涉及元素布局 (如 width), 浏览器则抛弃原有属性, 重新计算并把结果传递给 render 以重新描绘页面元素, 此过程称为 reflow即回流。而回流更是性能的关键因为其变化涉及到部分页面（或是整个页面）的布局。一个元素的回流导致了其所有子元素以及DOM中紧随其后的祖先元素的随后的回流。

当 DOM 元素的属性发生变化 (如 color) 时, 浏览器会通知 render 重新描绘相应的元素, 此过程称为 repaint即重绘。但是不影响布局。类似的例子包括：outline, visibility, or background color。每次发生回流或者是重绘的时候，浏览器都需要重新渲染计算，非常消耗浏览器的性能。

所以要提高动画的性能关键是要尽可能减少回流和重绘的发生。但是我们该如何来知道浏览器是否在必要的时候才执行重绘和回流呢？

### 测试Animations和Transitions ###

让我们来测试当浏览器在执行Animation和Transition的时候，浏览器是怎样工作的呢？使用chrome的开发者工具中的[Timeline panel](https://developers.google.com/chrome-developer-tools/docs/timeline)和[rendering settings](https://developers.google.com/chrome-developer-tools/docs/rendering-settings)来作为测试工具，我们来测试四组transition或者是animation来看看那一组的执行结果更加平滑、
流畅高效。

我们的目标是使浏览器尽可能的以每秒60帧的速度来执行动画效果。

要学习更多的有关chrome的开发者工具，可以去这个[网站](http://teamtreehouse.com/library/website-optimization)看看。

### Transition Top/Left Margin测试 ###

首先来测试使用transition来制作改变logo的top、left和margin时的性能(视频在youtu，要自备梯子哦)：

<iframe src="//www.youtube.com/embed/-1YpUaqS8I8?rel=0" height="315" width="560" allowfullscreen="" frameborder="0"></iframe>

**测试结果：**平均大概是花费了19ms的时间来渲染，但是有时运动会降低到30fps。当使用animation和transition来改变元素的margin和padding的时候，会导致回流的发生，所以在制作一些动画效果的时候，最好是避免使用transition改变元素的margin属性。

### Animation  Top/Left Margin测试###

下面我们来改变logo元素的定位属性，我们把它设置为绝对定位，然后使用animation来改变元素的top和left的值。

<iframe src="//www.youtube.com/embed/n-edaoddFXU?rel=0" height="315" width="560" allowfullscreen="" frameborder="0"></iframe>

**测试结果：**花费了23ms的时间来渲染，运动的帧率也远远低于60fps。

不过值得指出的是，由于元素使用了绝对定位的属性，脱离了普通的文档流。也就是说不会导致其它元素的回流。所以使用绝对定位可以减低浏览器的的回流。

### 使用Transform中的Translate3D  ###

让我们来看看当我们使用**translate3d**来改变logo元素位置的时候，会发生什么：

<iframe src="//www.youtube.com/embed/2uOMAOo9_0M?rel=0" height="315" width="560" allowfullscreen="" frameborder="0"></iframe>

**测试结果：**可以看到当动画开始的时候，在0.0ms的时候运动的帧率是在30fps以上——这就是我们需要的效果。在00:13ms的时候，就开始由GPU单独一个线程来渲染动画效果，这样就会加快渲染的速度和提高运动的帧率。

另外一些CSS属性也会触发加速如**translateZ()**和**opacity**。

**使用jQuery来制作动画效果怎么样呢？**

很多的[jQuery effect](http://api.jquery.com/category/effects/)，比如**animate()**方法，让我们来看看使用**animate**方法来制作动画效果的性能如何？

<iframe src="//www.youtube.com/embed/8MoyrddvwDA?rel=0" height="315" width="560" allowfullscreen="" frameborder="0"></iframe>

**测试结果：**渲染的时间是20ms左右。但是运动的帧率却很低，低于30fps——可以在视频中看到。

使用jQuery或者是javascript来编写动画效果的时候浏览器不会去优化它像使用CSS一样。

但是我们也不应该完全抛弃使用javascript来编写动画效果。使用javascript我们可以控制动画的更多的细节。如，我们可以控制动画效果的开始，暂停、重复以及取消。

当我们使用animation或者是transition的时候，我们没办法来终止其运行。但是正因为使用CSS只有开始和结束两个状态，浏览器才会更加容易的优化它们。所以在制作动画效果的时候最好是使用javascript来控制触发CSS的动画效果。

### 总结 ###

虽然我们不能完全控制用CSS编写的动画效果，但是使用CSS编写动画效果浏览器就能使用单独的线程来计算可以大大的提高动画的性能。不过，如果大量的使用**translateZ()**和**translate3d()**来计算动画效果的话，也会导致性能上的问题。

比如，在一个父元素下，给大量的子元素使用animation来执行**transform:translateZ(0)**的话，当把他们添加到父元素的时候，就会花很多的时间来计算从而导致动画效果出现一些问题。

使用animation或者是transition来操纵一些CSS属性的时候，如margin、padding、border、position、font-size以及宽和高的时候会导致回流。而操纵像visibility、border-style、border-radius和background这些的时候会导致重绘。

所以在制作动画效果的时候最好使用**transform**和**opacity**来做。这样可以阻止浏览器在执行帧动画的时候发生重绘或者是回流，因为浏览器是直接使用GPU来执行动画效果，它会花很少的时间来绘制执行动画效果以呈现在屏幕上。

原文地址[How to Create Smoother Animations and Transitions in the Browser](http://blog.teamtreehouse.com/create-smoother-animations-transitions-browser)。


