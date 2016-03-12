---
layout: post
title: 	ractive.js学习笔记(8)——编写ractivejs的扩展
categories:
- Life
tags:
- javascript
- ractive.js
---

在正式开始介绍编写ractivejs扩展前，先来看看mustaches另外一个语法的小技巧。

一般来说，我们使用模板就是用来显示数据的。不过有时候我们可能也需要在视图中插入HTML结构并且正确渲染出来，mustache为我们提供一个三个花括号的语法。

这样我们就可以插入一个HTML结构到文档中。比如下面的代码：

![](http://pic.yupoo.com/reicky_v/DyvYzL8Q/medium.jpg)

在浏览器中运行文件，可以看到如下图所示的效果：

![](http://pic.yupoo.com/reicky_v/DyvZl0fS/medium.jpg)

可以看到差别了吧。用双花括号表示的内容只是单纯的显示变量里面的内容，而用三个花括号表示的它还会渲染里面的HTML标签。不过需要注意一点的是使用三个花括号的时候，当数据内容发生变化的时候，里面的DOM节点会被删除重新渲染。所以在ractivejs中尽可能的不要使用mustaches三个花括号的语法。

### ractive扩展 ###

如果你使用过Backbone编写过应用程序，那你应该对于编写backbone的扩展应该也很熟悉。

下面我们就来学习下**Ractive.extend**并使用它来编写一个图片轮换的小应用。跟前面[事件那一片文章](http://learn.ractivejs.org/event-proxies/5)里的图片应用差不多。

先来进行模板的设置，首先需要获得图片的路径：

![](http://pic.yupoo.com/reicky_v/Dyw8jc0V/medium.jpg)

这里是使用了图片背景的CSS技术来显示图片，没有使用**img**标签来插入图片，因为使用背景图片外面可以使用**background-size**这个属性来灵活的处理图片的尺寸，不过IE8一下的浏览器不支持这个属性。

然后是图片描述部分的模板：

![](http://pic.yupoo.com/reicky_v/Dyw9EBLR/medium.jpg)

最后是添加事件处理：

![](http://pic.yupoo.com/reicky_v/DywaiodF/medium.jpg)

详细的代码可以去这个[地址看看](http://learn.ractivejs.org/extending-ractive/1/)。

外面可以把这个图片轮换写成ractive的一个扩展，以后再碰到类似的需求可以直接套用。跟jQuery插件一样。

在模板中编写以下代码：

![](http://pic.yupoo.com/reicky_v/DywuGKGN/medium.jpg)

最后js代码如下：

![](http://pic.yupoo.com/reicky_v/DyxuyR84/medium.jpg)

以后我们就可以这样来使用我们的这个ractive扩展：

    slideshow.goto( 3 );

当然，我们还可以根据不同的需求来完善这个扩展，比如切换的动画效果、缩略图以及针对移动设备添加手势支持等等。

Ractive.js有很强的的定制性。你可以灵活的来设计一些组件，而且模板处理也非常容易因为ractive的模板文件其实也就是HTML文件，而且在视图中处理逻辑非常简单。

