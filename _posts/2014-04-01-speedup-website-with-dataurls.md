---
layout: post
title: 使用Data URLs来加速你的网站
categories:
- Life
tags:
- 前端
---

> 在影响网页加载的性能因素中，http请求是一个重要的因素。所以在前端开发中，我们会使用一些技术手段来减少http请求，比如使用图片精灵、使用css3来制作一些以前需要图片才能制作的效果、压缩css和javascript等手段来减少http请求。但是如果网站本身有很多内容图片需要加载的话，一样会大大增加http请求。今天这篇文章就来学学使用Data URLs技术来处理网站中的图片从而为网站提速。这篇文章来自[Using Data URIs to Speed Up Your Website](http://blog.teamtreehouse.com/using-data-uris-speed-website)。翻译寒暄之类的话语有删减。

随着网站越来越复杂，网站性能也越来越受到关注。现在网站开发这块也有很多的技术策略手段来提升网站的性能。比如压缩图片、合并压缩css或者是利用缓存等等技术手段来提高web的性能。

在这篇文章中，我们来学习data URLs这种技术，以及使用它来提升网站的性能。

### 什么是Data URL?

Data URL是使用64位编码的技术把一些8-bit的数据翻译成标准的ASCII字符文件。这意味着你可以把它直接嵌入到HTML和CSS文件中。当浏览器在代码中碰到data URL代码当时候，浏览器会把它转换成正常当文件。

你可以使用data URIs来表示各种不同类型当文件。不过一般都会使用它来转换图片都编码，偶尔也会用来转换一些字体。

data URI有一个固定都格式，主要是包含文件的类型和文件本身的编码。

`data:[<MIME-type>][;charset=<encoding>][;base64],<data>`

你可以在html或者是css代码中使用Data URL。如你可以使用Data URL在css中来表示背景图片：

`li { 
    background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABFCAYAAAD6pOBtAAAABmJLR0QA/wD/AP+gvaeTAAAACXBIWXMAAAsTAAALEwEAmpwYAAAAB3RJTUUH3gMbBwwfAKopzQAAEfdJREFUeNrVW3uUHFWZ...) no-repeat;
}`

当然也可以使用data URIs在html文件中插入图片**src**，如：

`<img width="64" height="69" alt="Treehouse Logo" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABFCAYAAAD6pOBtAAAABmJLR0QA/wD/AP+gvaeTAAAACXBIWXMAAAsTAAALEwEAmpwYAAAAB3RJTUUH3gMbBwwfAKopzQAAEfdJREFUeNrVW3uUHFWZ...">`

事实上data URIs都地址一般都会很长。

### 为什么要使用data URIs

使用data URIs的一个最大的好处就是能大大的减少网页的http请求。因为每一个单独的外链文件在html或者是css代码中都会创建一个新的http请求，而使用data URIs则是直接在html或者是css中嵌入文件，所以不要创建一个http请求来拉取文件。

减少http请求意味着能使网页更快的加载。因为每一个http请求浏览器都会花时间和服务器进行通讯从而增加了网页加载时间。使用data URIs则不会有这样都问题。

当然使用data URIs转换为编码的字符串文件会比使用正常编码的文件要大。这意味着需要下载大数据就会大一点，不过我们可以把它们压缩成一个文件，即一个http请求。通过一个请求下载一个文件比通过多个请求下载多个文件还是要快一点的。

使用data URIs的一个缺点是浏览器不会缓存它们。每一次加载的时候，浏览器都会去解码加载data URIs，消耗系统的资源。移动设备解码data URLs需要的时间比pc端要花端时间多一些，所以你需要在网络和解码时间上做一个权衡。如果你在网站里使用了data URLs，那你要确保在多个移动设备上来测试你网站，这样才能确定是否在移动设备上有性能问题。

### 如何创建data URLs?

了解了如何使用data URLs以及它带来利与弊，那该怎么来创建data URLs呢?下面我们就来说说创建data URLs的几种方法。

#### 使用在线工具

现在网络上提供很多在线创建data URLs的服务。我发现来一个比较好的[dataurl.net](http://dataurl.net/#dataurlmaker)。

如果你看过我以前的关于web性能方面的文章，你可能会知道我使用的[the PageSpeed module](http://blog.teamtreehouse.com/automating-web-performance-best-practices-mod_pagespeed)的这个模块。只要你把这个模块安装在你的apache或者是nginx服务器上，它会自动对web对性能做一些优化不需要你再设置。

PageSpeed模块其中的一个特性自动会把一些小尺寸的图片转换成data URLs的格式插入到你到html或者是css代码中。意味着不需要你手动去转换格式了，PageSpeed会自动帮你完成这些工作。

当然还是需要再服务端设置一些东东到，你需要在服务端到配置文件编写下面到代码：

Apache服务器需要这样来设置：

`ModPagespeedEnableFilters inline_images`

Nginx设置如下：

`pagespeed EnableFilters inline_images;`

并且把**rewrite_images**这个模块打开可以对**inline_images**做一些优化。

#### COMPASS

如果你使用[compass](http://compass-style.org/)来编写css，它提供了data URLs的转换功能。

**inline-image()**这个模块会帮你把图片转为data URLs编码。只需要把图片文件传递给**$image**这个变量就可以了。**$mime-type**是用来设置格式的。

`inline-image($image, $mime-type) `

也可以使用**inline-font-files()**这个函数来处理字体的编码转换。

`inline-font-files([$font, $format]*)`

更多的信息可以去compass的官网看看详细的用法[inline data helpers](http://compass-style.org/reference/compass/helpers/inline-data/)。

#### 在服务端创建data URLs

很多的编程语言都有提供64位编码转换的第三方的语言包。不过你需要在编码文件的字符串前面加上**data:<mime type>;base64**。

可以去[这个地址](http://en.wikipedia.org/wiki/Data_URI_scheme#Examples)看看一些例子了解使用方法。

### 浏览器支持情况

现在浏览器对data URLs的支持还不错。你只需要注意ie5到ie7是不支持的。如果你想支持老版本的ie浏览器，你可以针对老版本的浏览器创建一个样式文件使用正常的编码来解决这个问题。

具体的支持可以去这个地址看看[http://caniuse.com/datauri](http://caniuse.com/datauri)。

### 总结

在这篇文章中，我们学会了使用data URLs来减少http请求从而提高web性能的技术。

web性能对于一个web开发者来说应该是要时刻关注并且不断研究的一个领域。在带宽越来越好的今天，有时候很容易忘记这方面的关注。特别是在现在这个高速发展的移动互联网时代，更应关注web性能方面的实践和研究。

当你在开发一个web应用的时候，你应当确保你的web能给你的用户提供一个非常好的用户体验，快速响应用户的请求。

### 一些有用的资源

* [Can I use… Data URIs](http://caniuse.com/datauri)
* [DataURI Maker](http://dataurl.net/#dataurlmaker)













