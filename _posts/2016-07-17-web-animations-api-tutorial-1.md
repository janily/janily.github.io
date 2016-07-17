---
layout: post
title: web animations api学习系列(1)－web animations初探
categories:
- Life
tags:
- web animations
- 动画
- javascript
- 前端

---

> 平常工作中，跟动画打的交道比较多，特别是在移动端，在做动画的同时，要时刻关注页面在设备上的性能。
特别是现在安卓设备，千差万别的硬件配置，性能尤其重要，稍不注意在一些硬件不是很好的设备上，页面就会因为动画卡。性能优化手段也多种多样，今天这一系列要讲的web animations，就是浏览器提供的用来做动画的API，使用它来编写动画效果能很好的优化动画的性能。这一系列的主要是根据[danielcwilson](http://danielcwilson.com/)网站上的系列文章整理而成，[原文地址](http://danielcwilson.com/blog/2015/09/animations-conclusion/)。文章主要是根据自己学习整理而成，有删减。

### 什么是web animations

在正式开始之前，首先来了解下什么是web animations。

随着CSS3标准的日益稳定和推广，在过去五年中，web动画在CSS3以及javascript的带动下，取得了非常大的进展。现在随便打开一个页面，里面或多或少都用到了css3或者是javascript来实现的一些动画效果。但是，每一种动画的实现方式也都有它优劣。

**CSS动画**：浏览器会默认对使用了**transitions**属性的动画效果提供硬件加速来提升性能，但前提是必须在CSS中声明这个属性，而且在必要的时候，还要用javascript来动态改变属性的值。

**requestAnimationFrame**：当使用requestAnimationFrame来编写动画效果时，浏览器能好的针对它进行优化。即每次浏览器更新页面时，能获取通知并执行应用。 简单理解为，RAF能在每个16.7ms间执行一次咱们的函数，不多不少；最小化的消耗资源，RAF在页面被切换或浏览器最小化时，会暂停执行，等页面再次关注时，继续执行动画；相比 CSS 动画有更好的掌控，能合理降低CPU的使用。

缺点是：无法控制执行时间，执行时间由系统根据屏幕刷新时间决定以及浏览器兼容问题。

**setInterval**：缺点是不同浏览器的对setInterval的时间间隔都不同，另外jquery的动画借口也是使用setInterval来实现的。

**[Velocity](http://julian.com/research/velocity/)**和**[GreenSock](http://greensock.com/)**：像现在比较出名的两个用来编写动画效果的javascript库都对动画的性能进行了优化，我也使用了的确性能表现非常不错，但总是要加载一个额外的第三方库。有时候一两个简单的动画效果，也不需要用到。

通常，当浏览器原生支持一些性能上的优化；或者是实现一些通用的接口。作为开发者的我们当然是非常开心的，毕竟不需要借助一些第三方的库来实现。比如DOM选择，现在浏览器原生提供的** document.querySelector** 方法就实现了jQuery中的DOM方法。现在浏览器也提供了很多的原生方法来实现一些以前只能借助于第三方库才能实现的方法。

web animations就是这样一个方法。它致力于集合CSS3动画的性能、JavaScript的灵活、动画库的丰富等各家所长，将尽可能多的动画控制由原生浏览器实现，并添加许多CSS不具备的变量、控制以及其它的选项。

不过现在浏览器对它的支持还不是很好：

![](http://ww4.sinaimg.cn/large/0060lm7Tgw1f5wscjz9noj30z50e941r.jpg)

不过还好，我们可以引入一个第三方的[Polyfill库](https://github.com/web-animations/web-animations-js)来使不支持的浏览器支持web animations方法，在移动端上安卓5.0以上是支持的。

第一篇先到这了，简单的介绍了一些动画实现方法以及优缺点。下一篇正式来开始学习使用web animations API的使用。







