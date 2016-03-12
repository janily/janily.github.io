---
layout: post
title: 制作迷人的遮罩文字效果
categories:
- Life
tags:
- css3
- svg
- 前端

---

前段时间在项目中遇到了镂空文字的效果，啥叫文字遮罩效果，简而言之就是让背景图片显示在文字中。一图胜千言，看下图久明白啦：

![](https://cdn.css-tricks.com/wp-content/uploads/2015/09/ps-outlinetext.gif)

在photoshop中，一般使用图层蒙版很容易就能实现类似的效果。那在web开发中，有哪些方法来实现这样的效果呢？得益于web技术的发展，现在要实现遮罩文字效果，也比较简单。

下面就来看看都有哪些方法来实现这样的效果。

###-webkit-background-clip: text;

这个属性现在还不是W3C支持的标准属性，需要添加**－webkit**的前缀才能生效。不过对于现在移动端来说，-webkit占据绝对的市场，可以放心的使用它。具体的浏览器支持可以去[这里](http://caniuse.com/#search=-webkit-background-clip)看看。

-webkit-background-clip 属性，指定对象的背景图像向外裁剪的区域。相对应的有以下几个值：

border-box：默认值，从border区域（不含border）开始向外裁剪背景。

padding-box：从padding区域（不含padding）开始向外裁剪背景。

content-box：从content区域开始向外裁剪背景。

text：从前景内容的形状（比如文字）作为裁剪区域向外裁剪，如此即可实现上面所说的遮罩效果。

要实现遮罩效果，还需要使用**-webkit-text-fill-color: transparent**，这个属性来配合。这条语句声明了文字的填充颜色为透明，这样就可以显示背景图片从而实现遮罩效果。

实现遮罩效果的css代码如下：


```
.clip-text-maybe {
  
  /* if we can clip, do it */
  -webkit-text-fill-color: transparent;
  -webkit-background-clip: text;

  /* what will show through the text
      ~ or ~
     what will be the background of that element */
  background: whatever;

  /* fallback text color
      ~ or ~
     the actual color of text, so it better work on the background */
  color: red;
 
}
```

利用上面的代码，我们可以实现一些有趣的文字效果：

<p data-height="268" data-theme-id="17491" data-slug-hash="crlxk" data-default-tab="result" data-user="Jintos" class='codepen'>See the Pen <a href='http://codepen.io/Jintos/pen/crlxk/'>-webkit-background-clip:text CSS effect </a> by Jintos (<a href='http://codepen.io/Jintos'>@Jintos</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

###SVG &lt;pattern&gt; fill

当然特效这种事，怎么能缺少SVG呢？

SVG中也有**&lt;text&gt;**元素也可以实现文字遮罩效果。这里需要用到SVG中的**pattern**元素。

比如：


```
<svg>
  <pattern id="pattern" patternUnits="userSpaceOnUse" width="750" height="800">
    <image width="750" height="800" xlink:href="image.jpg"></image>
  </pattern>
  <text x="0" y="80" class="headline" style="fill:url(#pattern);">background-clip: text | Polyfill</text>
</svg>
```

使用上面的代码，可以实现下面这样的效果：

<p data-height="268" data-theme-id="17491" data-slug-hash="vOwPOd" data-default-tab="result" data-user="cypark" class='codepen'>See the Pen <a href='http://codepen.io/cypark/pen/vOwPOd/'>SVG Text Clip with Gradient & GIF</a> by C.Y. Park (<a href='http://codepen.io/cypark'>@cypark</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

Lea Verou也写过这方面的文章，可以去[看看](http://lea.verou.me/2012/05/text-masking-the-standards-way/)

###Polyfill the CSS with SVG

Tim Pietrusky写了一个关于**-webkit-background-clip**的polyfill，是一个使用SVG方法作为CSS降级方案的脚本。因为相对于SVG的方法，使用CSS来实现文字遮罩效果更简单些。当**-webkit-background-clip**属性不被支持时，就使用SVG的方法来降级实现遮罩效果。

<p data-height="268" data-theme-id="17491" data-slug-hash="cnvBk" data-default-tab="result" data-user="TimPietrusky" class='codepen'>See the Pen <a href='http://codepen.io/TimPietrusky/pen/cnvBk/'>-webkit-background-clip: text Polyfill</a> by Tim Pietrusky (<a href='http://codepen.io/TimPietrusky'>@TimPietrusky</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

###mix-blend-mode: screen;

再来看看[blending](https://css-tricks.com/almanac/properties/m/mix-blend-mode/)这个非常新的属性，它可以用来实现像photoshop中类似("screen","multiply","lighten"等图层效果)。

来看下面这个例子：

<p data-height="268" data-theme-id="17491" data-slug-hash="RPdLaQ" data-default-tab="result" data-user="giana" class='codepen'>See the Pen <a href='http://codepen.io/giana/pen/RPdLaQ/'>CSS Gradient Text in Firefox</a> by Giana (<a href='http://codepen.io/giana'>@giana</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

主要内容是来自css-tricks网站，有删减。原文[地址](https://css-tricks.com/how-to-do-knockout-text/)。











