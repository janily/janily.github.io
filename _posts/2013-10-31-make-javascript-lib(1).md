---
layout: post
title: javascript学习打造javascript框架系列-概述
categories:
- Life
tags:
- javascript
---

> 从事前端一年多了，而前端中javascript才是一把利剑。关于javascript方面，也在看《javascript高级程序设计》这本书，正所谓光说不练假把式，javascript中包含的知识点实在是太多了，要不停的总结，才能提升自己。而写一个javascript的库正好可以检验自己对javascript的掌握程度到底如何，也能查漏补缺，当然现在有了jQuery这么优秀的类库在那，也不是要重复造轮子，只是为了检验自己以及更进一步对javascript的理解。这个系列参考了[DailyJS](http://dailyjs.com/)网站连载的的一个javascript系列，地址在[这里](http://jsbin.com/AduboCI/2)以及这里的[jQuery源码解读系列](http://www.cnblogs.com/nuysoft/archive/2011/11/14/2248023.html)还有[Prototype.js](http://prototypejs.org/)这个库。

将从以下几个方面来写这个库

- 首先整体先组织好库的结构
- 运用函数式编程的方法来写我们的库
- 选择器部分
- 事件部分
- Ajax
- 动画
- 模块加载
- 支持插件

其实这些跟jQuery的功能差不多，其中选择器部分可能花费的功夫多一点。

在这过程中，我们也会进一步理解对于现代WEB应用程序来说，Javascript开发包含以下特点：

- 处理浏览器的兼容性
- 简洁、易用的API设计
- 基准测试以及性能优化
- 良好的代码组织
- 使用GitHub开发，进行版本管理

好吧，这篇是基本上是扯淡的，下一篇才正式开餐，开餐前，得为我们的库像个名字，啥名字好呢？Alien就出现在我的脑海中，好吧就叫Alien吧，Alien是啥，相信不用我解释了吧，不要说大名鼎鼎的Alien你不知道，这里贴个[地址](http://zh.wikipedia.org/wiki/%E7%95%B0%E5%BD%A2_(%E9%9B%BB%E5%BD%B1))