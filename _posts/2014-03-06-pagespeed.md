---
layout: post
title: 	web页面性能之用户会花多少时间看到你网页内容
categories:
- Life
tags:
- web性能
- 翻译
---

> web开发中对于web性能这块这里是指页面加载速度即从开始加载开始到首屏呈现在用户面前花的时间是是衡量web性能一个重要的方面，对用户体验也有至关重要的影响，试想一个用户打开你的网页等了四五秒都没啥反应，后果可想而知。尤其现在移动互联网的时代，web页面加载的速度更加重要。关于测试网页加载速度现在也有很多这方面的工具，今天文章中要说的这个工具非常不错，而且提供的功能也非常酷。提供你网页加载的各个方面的数据分析，并且针对网页对于加载各个资源所花的时间自动生成了一个**Filmstrip View**，可以让你非常详细的了解网页加载过程中所发生的一切。原文地址[Page Speed: How Soon Will Visitors See Your Content?](http://www.sitepoint.com/page-speed-soon-visitors-see-content/)。

如果你在web开发中经常使用一些工具来测试你web性能，你可能会听说过或者使用过[WebPagetest.org](WebPagetest.org)这个工具。如果你没有使用过，相信经过本篇文章后你一定会喜欢上它的。

WebPagetest.org这个工具几乎包含关于测试web性能各项指标的功能，在[2010年的时候Steve Souders甚至说它是最顶级的web性能测试工具](http://www.stevesouders.com/blog/2010/03/05/webpagetest-org-top-tool/)。

在他的那篇文章中，Souders briefly提到关于WebPagetest.org一个我认为不容忽视的一个功能就是**Filmstrip view**。[ Paul Irish](https://twitter.com/paul_irish/)告诉我Filmstrip view这个功能非常棒，也是我写这篇文章的原因，这篇文章就来说说**Filmstrip view**。

### **Filmstrip view** ###

要使用这个功能也很简单，先进入到[WebPagetest.org](WebPagetest.org)这个网站然后在URL地址栏里输入你要测试网站的网址。为了测试**Filmstrip view**这个功能的使用方法，我们准备用[http://mlb.mlb.com/home](http://mlb.mlb.com/home)这个网站的首页来进行测试。

![](http://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/03/1393868079wpt-enter.jpg)

WebPagetest.org还提供很多高级个性化的设置选项，比如设置测试的地点位置，网络的连接速度等等。我们这里全部是按照默认的设置来测试，点击测试按钮**Start Test**稍稍等个几秒钟。测试完成后，我们会看到如下图所示的情况：

![](http://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/03/1393868077wpt-done.jpg)

这里提供了很多有用的数据给我们分析，但是由于我们主要是来说**Filmstrip View**这个功能，如下图所示我们点击**Filmstrip View**这个链接：

![](http://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/03/1393868069wpt-filmstrip-link.jpg)

点击之后，稍等一下你应该可以下面的情形：

![](http://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/03/1393868080wpt-filmstrip-initial.jpg)

正如你所看到的，**filmstrip**给我们提供了网页各个时间段加载的详细情况，这个过程基本上也是你网站的用户将要看到的一个加载过程。移动**filmstrip**面板的滚动条，你会看到你整个网页从加载到完全显示的一个过程。

重要的是，filmstrip的第一个视图是网页初始化渲染加载的时候的情形。即左边的一个空白的视图的情形：

![](http://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/03/1393868068wpt-filmstrip-left.jpg)

正如在上面图中所看到的， MLB.com网站的首页大约花了一秒的时间来加载呈现在用户面前。我们可以进一步详细了解在这一秒钟之内发生的详细信息，注意选择下图中所示的选项：

![](http://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/03/1393869219wpt-interval.jpg)

这里我们调整**filmstrip**生成加载过程图片的间隔时间为十分之一秒，即**Thumbnail Interval**的时间，这样我们可以更加详细的观察页面加载的信息。

![](http://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/03/1393868072wpt-interval-indicated.jpg)

比较上面这两张图可以发现网页其实在0.9秒的时候就开始渲染了。

### **分析相关数据** ###

在测试结果页面上你会看到waterfall这一栏详细的罗列出网页加载各种资源所花费的时间：

![](http://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/03/1393868076wpt-waterfall.jpg)

墨绿色的线代表页面开始渲染加载。红色的线是当你移动**filmstrip**面板的时候表示各个时间段所加载的资源。

MLB网站在0.9秒的时候开始渲染加载页面这个表现还是非常不错的。但是从**waterfall**这个视图可以看出还有值得优化的地方可以进一步提高网页的加载速度。

比如，在网页开始渲染加载的时候CSS文件和javascript文件就优先加载了。如果[把javascript从网页的head区域移动到body标签开始加载](http://developer.yahoo.com/blogs/ydn/high-performance-sites-rule-6-move-scripts-bottom-7200.html)以及[压缩](https://medium.com/coding-design/24888fbbd2e2)CSS文件，那网页的加载速度能提高0.5秒或者是更多。也可以使用**document.write**来加载脚本文件，[不过这样可能会导致一些问题](http://www.stevesouders.com/blog/2012/04/10/dont-docwrite-scripts/)。(译者注：现在一般会使用像requirejs或者是seajs这样提供异步加载的方式的js库来处理脚本加载问题)

如果你的网页花了2秒或者是2秒钟以上的时间来渲染，那你得详细分析下网页中的哪些资源加载导致了渲染时间的加长，然后针对性的优化。

### **使用WebPagetest的视频功能来体验网页的实时加载过程** ###

在**filmstrip**面板的下方你会发现一个**create video**的链接，它会把你网页整一个的加载过程以视频的方式呈现给你，你可以非常形象的观察到你网页实时的一个加载过程。

我这里也创建了一个mlb.com网站[首页加载](http://www.webpagetest.org/video/view.php?id=140303_41e3039c04fdb42d8529c974da8436c1e6cdf127)的一个视频，你可以去[这里](http://www.webpagetest.org/video/compare.php?tests=140303_HD_J1P-r%3A1-c%3A0&thumbSize=200&ival=100&end=visual)看一下。

当然，我这里仅仅是基于自己的经验对MLB.com网站首页加载做了一个简单的分析，不一定都是正确的，我也不确定上面分析的关于网站资源加载改进的建议是否合理。

当然在测试web性能的时候，我们应该在针对不同的场景，不同的时间来测试web的性能，确保同样的问题能够在不同的场景下重现。我这里只是测试了一种情形下的的测试结果可能不是很准确。但是可以看出WebPagetest确实提供了非常有用的测试功能。

在web开发中，我们都知道[页面的加载速度对于web性能和用户体验是多么的重要](http://www.impressivewebs.com/importance-of-website-performance-sources/)。WebPagetest提供的web性能测试功能对于提高我们的web性能和页面加载速度有着很好的帮助。

下面推荐一些有用的资源：

- [http://www.webpagetest.org/video/compare.php?tests=140303_HD_J1P-r%3A1-c%3A0&thumbSize=200&ival=100&end=visual](http://www.webpagetest.org/video/compare.php?tests=140303_HD_J1P-r%3A1-c%3A0&thumbSize=200&ival=100&end=visual)
- [https://sites.google.com/a/webpagetest.org/docs/using-webpagetest/metrics/speed-index](https://sites.google.com/a/webpagetest.org/docs/using-webpagetest/metrics/speed-index)
- [http://calendar.perfplanet.com/2012/deciphering-the-critical-rendering-path/](http://calendar.perfplanet.com/2012/deciphering-the-critical-rendering-path/)
