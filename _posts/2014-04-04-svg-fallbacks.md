---
layout: post
title: 在web开发中SVG的回退方案
categories:
- Life
tags:
- 前端
---

> 前面陆续的写了一些svg的学习笔记，也提到过对于不支持svg的浏览器的处理方案。今天偶然在css-tricks上看到了一篇文章讲到是关于svg回退方案的文章，对于在各种场景下到svg的回退方案有一个系统到介绍，所以就搬过来了以备查用。原文地址[svg fallbacks](http://css-tricks.com/svg-fallbacks/)。

Alexey写过一篇关于svg[回退方案的文章](http://lynn.ru/examples/svg/en.html)。使用它可以针对不支持svg的ie8以下的浏览器和Android2.3使用**png**类型到图片来代替svg图片。

Alexey的回退方案是这样的：

`<svg width="96" height="96">
  <image xlink:href="svg.svg" src="svg.png" width="96" height="96" />
</svg>`

这个技术是基于Archibald的[这篇文章](http://jakearchibald.com/2013/having-fun-with-image/)中提到的浏览器中解析**&lt;image&gt;**标签的时候跟**&lt;img&gt;**标签其实是一样的。

这个技术对于不支持svg的浏览器也能正常显示图片，如ie8也能显示png图片。

不过让我感到困惑的是在ios3和4系统中浏览器是支持使用img标签来插入或者是css方式来插入svg图片。但问题是在ios3和4中不支持使用**svg**标签的方式在html中插入svg图片。看来对于是否支持svg图片还要根据使用场景来看了。

当然我们也关心在使用回退方案的浏览器会不会按需下载毕竟这关系着页面的性能。

好消息是现代的浏览器如chrome只会下载svg图片。Android2.3只会下载png图片。

不过在IE9和IE10以及IE11(预览版)中测试发现，IE9会把svg和png都会下载下来。

![image](http://cdn.css-tricks.com/wp-content/uploads/2013/08/ie9-timeline.png)

在IE的开发者工具中，我们可以发现事实上png图片最后的状态是**aborted**即没有下载下来，但是还是会花费时间去请求，无疑对页面性能是有一定影响的。IE10中的情况也差不多。

![image](http://cdn.css-tricks.com/wp-content/uploads/2013/08/ie10-timeline.png)

Scott Jehl[建议](https://twitter.com/scottjehl/status/369178695908327424)使用[Charles Proxy](http://www.charlesproxy.com/)这个工具来检测实际的请求。

![image](http://cdn.css-tricks.com/wp-content/uploads/2013/08/charles.png)

我们可以看到在实际的请求中花费来300ms的时间，Yoav Weiss[指出](http://blog.cloudfour.com/using-charles-proxy-to-examine-ios-apps/comment-page-1/#comment-15400),如果是在头部引入脚步的话，这可能会导致一些意外的问题出现。

关于Charles Proxy可以去这个[地址](http://blog.cloudfour.com/using-charles-proxy-to-examine-ios-apps/)看看它的使用方法。

Andy Davies猜想在win7中IE11应该会解决PNG和svg使用时，png请求的问题。

下面是我的一个测试结果：

<p data-height="268" data-theme-id="0" data-slug-hash="BrFpL" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/BrFpL/'>SVG Tests</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

### 回退方案

ok，如果IE上的png请求问题以及iOS上的显示问题你可以接受的话。那下面这里推荐一些针对不支持svg浏览器的回退方案。

#### css中使用svg作为背景图片的回退方案

我们可以借助Modernizr这个库来[检测](http://modernizr.com/docs/#features-misc)浏览器对svg是否支持。所以我们可以定义一个类，当浏览器不支持svg的话我们可以使用png图片来代替。

`.my-element {
  background-image: url(image.svg);
}
.no-svg .my-element {
  background-image: url(image.png);
}`

这样也会导致一个问题，就是浏览器还是会请求两次下载两张图片。

这里推荐更好的的一个方案是：

`.my-element {
  background-image: url(fallback.png);
  background-image: url(image.svg), none;
}`

这其实是利用来css中的多重背景这一技术。如果浏览器支持多重背景这个属性，浏览器就会使用第二个声明也就是svg图片，如果不支持则使用第一个的声明也就是png图片。

#### 使用svg标签来插入svg图片

如果是使用svg标签的话，我们可以这样来使用：

`<svg width="96" height="96">
  <image xlink:href="svg.svg" src="svg.png" width="96" height="96"/>
</svg>`

David Ensinger[也提到过](http://davidensinger.com/2013/04/inline-svg-with-png-fallback/)在svg标签中使用**&lt;foreignObject&gt;**的方法来解决问题。但是问题是，它也有重复下载的问题。

我们还可以这样做：

`<svg></svg>`
`<div class="my-svg-alternate"></div>`

然后根据Modernizr来检测浏览器是否支持svg...

`.my-svg-alternate {
  display: none;
}
.no-svg .my-svg-alternate {
  display: block;
  width: 100px;
  height: 100px;
  background-image: url(image.png);
}`

### 使用&lt;object&gt;来插入svg

我们也可以是使用object的方式来插入svg。

`<object type="image/svg+xml" data="image.svg" class="logo"></object>`

`.no-svg .logo {
  display: block;
  width: 100px;
  height: 100px;
  background-image: url(image.png);
}`

如果是使用SVG图片来插入到img标签到话，Scott Jehl推荐[这样做](https://twitter.com/scottjehl/status/369190249437483008):

`<img src="image.svg" onerror="this.src=image.png">`

配合Modernizr：

`if (!Modernizr.svg) {
  $("img[src$='.svg']")
    .attr("src", fallback);
}`

代码中的fallback会自动来使用html代码中的png图片。

[SVGeezy](http://benhowdle.im/svgeezy/)这个库也是来做浏览器svg检测的。

当然在实际项目中你可以选择使用svg标签或者是object的方式配合回退方案来使用svg图片。

如果你现在还没有对svg非常熟悉，你可以先去[这篇文章](http://css-tricks.com/using-svg/)看看。













