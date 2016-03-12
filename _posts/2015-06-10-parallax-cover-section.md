---
layout: post
title: CSS3实现Parallax Cover Section
categories:
- Life
tags:
- css3
- animation
- parallax
- 前端

---

上一篇文章使用了CSS3种的**repeating-linear-gradient**属性来实现了背景平铺的效果。今天来使用CSS3中的**3D Transform**和**vh**来实现一个**Parallax Cover Section**效果，用中文不知道怎么好翻译，姑且翻译为**视差滚动封面效果**。

要实现这个效果，你首先要了解和熟悉CSS3中的**3D Transform**和**vh**属性。可以先去这里了解它们[地之一](http://blog.ilikecss.com/responsive-new-units-vw-vh/)，[地址二](http://www.zhangxinxu.com/wordpress/2012/09/css3-3d-transform-perspective-animate-transition/)。

<p data-height="268" data-theme-id="0" data-slug-hash="MweEVE" data-default-tab="result" data-user="tutsplus" class='codepen'>See the Pen <a href='http://codepen.io/tutsplus/pen/MweEVE/'>#5: Parallax Cover Section (complete)</a> by Tuts+ (<a href='http://codepen.io/tutsplus'>@tutsplus</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

上面就是**Parallax Cover Section**这个效果。试着滚动鼠标看看，上面图片和下面内容有一个视差滚动的效果。

从上面的代码可以看出，主要是使用了**perspective**这个属性来声明元素的3D元素查看3D元素的视图。

以及下面的子元素使用了**transform**中的**translateZ**来做一个3D的变换。使用**margin－top**来定义元素的位置。

具体可以看代码，简单明了。

组合使用，一个简单的**视差滚动封面效果**就做出来了。

运用之妙，存乎一心。



