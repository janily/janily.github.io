---
layout: post
title: CSS中的clip-path和transform在设计中的妙用
categories:
- Life
tags:
- css3
- mask
- transform
- 前端

---

> 设计与技术之间互相推动着发展，就像生产力与生产关系一样。比如，现在的各种移动终端设备能呈现百花齐放式的发展，就得益于技术的发展才能得以出现。同样在web开发领域，随着技术的发展，如CSS3以及浏览器越来越强大，也推动着设计向前发展，各位设计师们才能可以在web中展示各种天马行空的设计和交互，提升体验。今天就来说说使用CSS中的clip-path和transform在设计中的运用，原文地址是[这里](https://viget.com/inspire/angled-edges-with-css-masks-and-transforms)。翻译过程中会加入自己的一些理解。

通过一定的角度倾斜一些元素，能创造出一种独特的视觉效果。虽然在web中这种效果不是很常见，但是运用的好的话，能给用户带来不一样的视觉效果，比如[ The National Trust for Historic Preservation](https://savingplaces.org/)这个网站就恰到好处的使用了倾斜的设计效果。这篇文章我们就来使用CSS技术来实现如下图所示的这些倾斜效果。

![](https://viget.com/uploads/image/blog/angled-edge-1.jpg)

![](https://viget.com/uploads/image/blog/angled-edge-2.png)

![](https://viget.com/uploads/image/blog/angled-edge-3.png)

看到这些效果，我们可能会想到使用CCS中的transform属性来实现，不过使用transform也有它的局限，即只能用来实现平行边缘的倾斜。下面我们来看看实现倾斜效果的几种方法。

### CSS CLIP_PATH

第一种方法是使用CSS中的clip-path来实现倾斜效果。具体关于clip-path这个属性的使用方法可以去这两个[地址](https://css-tricks.com/almanac/properties/c/clip/)和[地址](http://www.w3cplus.com/css3/css-svg-clipping.html)看看。

比如我们可以直接使用它对图片边缘进行剪裁来实现倾斜效果。

```
.hero-image__figure img {
  -webkit-clip-path: polygon(0 0, 100% 0, 100% 96%, 0 100%);
  clip-path: polygon(0 0, 100% 0, 100% 96%, 0 100%);
}

```

上面的代码只是沿着图片的底部边缘剪裁了一个基本切角来实现了一个倾斜的效果。

当然，我们也可以对一个定义了背景图片的div区块进行剪裁，也可以达到上述效果。要注意一点的是，这里要声明区块的高度才能正确显示。


```
.promo {
  -webkit-clip-path: polygon(0 0, 1600px 0, 1600px 87%, 0 100%);
  clip-path: polygon(0 0, 1600px 0, 1600px 87%, 0 100%);
}

```

下面是上面两段代码运行的实例：

<p data-height="268" data-theme-id="0" data-slug-hash="zvdXZM" data-default-tab="result" data-user="jeremyfrank" class='codepen'>See the Pen <a href='http://codepen.io/jeremyfrank/pen/zvdXZM/'>Angled Edge with CSS clip-path</a> by Jeremy Frank (<a href='http://codepen.io/jeremyfrank'>@jeremyfrank</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

不过现在浏览器对于clip-path这个属性支持还不是很好，[浏览器支持查看](http://caniuse.com/#search=clip-path)。

这里推荐几个在线的clip-path代码生成器，非常方便。

[clip-path-generator](http://cssplant.com/clip-path-generator)

[clippy](http://bennettfeely.com/clippy/)

### CSS GENERATED CONTENT

第二种方式使用伪类来实现倾斜效果。使用它可以很方便的在一些纯色背景区块中实现这种效果。


```
.quote {
  background: #41ade5;
  color: #fff;
  position: relative;
  z-index: 1;
}
 
.quote:after {
  background: inherit;
  bottom: 0;
  content: '';
  display: block;
  height: 50%;
  left: 0;
  position: absolute;
  right: 0;
  transform: skewY(-1.5deg);
  transform-origin: 100%;
  z-index: -1;
}

```

通过伪类在元素的底部改变它的旋转角度来达到倾斜的效果。

同理可得，使用元素的**before**和**after**的伪类我们可以很轻松的实现不同角度的倾斜效果。

<p data-height="268" data-theme-id="0" data-slug-hash="qOXeWL" data-default-tab="result" data-user="jeremyfrank" class='codepen'>See the Pen <a href='http://codepen.io/jeremyfrank/pen/qOXeWL/'>Angled Edges with CSS Pseudo Elements</a> by Jeremy Frank (<a href='http://codepen.io/jeremyfrank'>@jeremyfrank</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

伪类这种技术可以大胆放心的使用，浏览器支持都非常好。

### 使用SASS MIXIN快速实现各种角度的倾斜

如果要实现不同角度的倾斜效果都要手写，那还是有点痛苦的，还好现在有SASS这种预编译的CSS语言。我们可以把实现各种边缘倾斜的效果整合成一个MIXIN，这样就可以快速的实现各种倾斜效果。


```
.block {
  background: #41ade5;
  @include angle(after);
}

```

下面就来看看使用SASS实现的各种角度倾斜的效果：

<p data-height="268" data-theme-id="0" data-slug-hash="avyezR" data-default-tab="result" data-user="jeremyfrank" class='codepen'>See the Pen <a href='http://codepen.io/jeremyfrank/pen/avyezR/'>Angled Edge Pseudo Element SASS Mixin</a> by Jeremy Frank (<a href='http://codepen.io/jeremyfrank'>@jeremyfrank</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

看到没有，使用起来非常方便。








