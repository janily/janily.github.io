---
layout: post
title: CSS3，打造平铺背景是如此简单
categories:
- Life
tags:
- css3
- repeating gradients
- 渐变
- 前端

---

在CSS3中新推出的属性中，渐变无疑是应用频率最高的一个属性之一。特别是现在在web设计中，渐变应用无处不在。更是如此！

在平时的开发中，一般使用线性渐变(linear-gradient)比较多。而对于repeating-linear-gradient这个属性可能关注不是很多。其实应用的好，这个属性也能制作很多有趣的效果。

比如，在web开发中经常会碰到要平铺背景的情况。遇到这种情形，我们一般是直接使用一小块png或者是jpg格式的图片来平铺。比如著名的背景平铺网站[地址](http://subtlepatterns.com/)上面的图片就是用来做网站背景平铺的，不过现在如果使用repeating-linear-gradient重复渐变这一属性的话，也可以打造出类型丰富的平铺背景的效果。

下面，我们就来看看，它能为我们带来哪些应用？

### 基础知识

开始之前，先来了解下repeating-linear-gradient这个属性的一些基础知识。


```
<repeating-linear-gradient> = linear-gradient([ [ <angle> | to <side-or-corner> ] ,]? <color-stop>[, <color-stop>]+)
<side-or-corner> = [left | right] || [top | bottom]
<color-stop> = <color> [ <length> | <percentage> ]?
```

取值：

下述值用来表示渐变的方向，可以使用角度或者关键字来设置：

**&lt;angle&gt;：**用角度值指定渐变的方向（或角度）。

**to left：**设置渐变为从右到左。相当于: 270deg。

**to right：**设置渐变从左到右。相当于: 90deg。

**to top：**设置渐变从下到上。相当于: 0deg。

**to bottom：**设置渐变从上到下。相当于: 180deg。这是默认值，等同于留空不写。

**&lt;color-stop&gt;** 用于指定渐变的起止颜色：

**&lt;color&gt;：**指定颜色。

**&lt;length&gt;：**用长度值指定起止色位置。不允许负值

**&lt;percentage&gt;：**用百分比指定起止色位置。

### 实战

**实战1:**

<p data-height="268" data-theme-id="0" data-slug-hash="WvjJmX" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/WvjJmX/'>#2: CSS3 Background Patterns (base)</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

像上面这种类型的背景，在web设计中也经常见到。在CSS3出现之前，只能傻傻的切图片来实现这种背景效果。现在，轻松使用代码就能实现。

**实战2:**

基于上面的实例，简单的限制下**background-size**，一个简单的背景平铺的效果就实现了。

<p data-height="268" data-theme-id="0" data-slug-hash="OVmZem" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/OVmZem/'>#2: CSS3 Background Patterns (base)</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

### 更多实例

**更多实例**

依照上面的原理，我们可以复制出很多的背景平铺效果，如下所示：

<p data-height="268" data-theme-id="0" data-slug-hash="gpWKYg" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/gpWKYg/'>#2: CSS3 Background Patterns (base)</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>








