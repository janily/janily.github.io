---
layout: post
title: 打造一个平滑滚动的导航菜单
categories:
- Life
tags:
- javascript
---

> 这篇文章主要是来自[onextrapixel](http://www.onextrapixel.com/)网站的[Create a Smooth Jump-To Sub Navigation Menu in One JS Call](http://www.onextrapixel.com/2013/10/17/create-a-smooth-jump-to-sub-navigation-menu-in-one-js-call/)。具体内容有删减，添加了一些自己的理解。

有时候，写作者并不能在一个很简单的一段话中，把自己想要表达的观点传达给他的读者。有可能要写很长的文字才能表达清楚，这时候对于他的读者来说，面对大容量的文字，可能会产生抵触的心理。特别是现在快餐式消费盛行的互联网时代，读者很容易被其它的东西分散注意力。

为了降低这种情况的发生，我想到用jQuery写一个导航插件，用户可以很方便的跳转到自己感兴趣的部分开始阅读。想知道我怎么做的么？那就继续往下看，一探究竟。

**注意**这个插件在IE9以上，Chrome，firefox,safari浏览器上能很好的运行。

![](http://pic.yupoo.com/reicky_v/DfQmr4Hs/medium.jpg)

下面是demo地址和代码下载地址

[Demo](http://www.onextrapixel.com/examples/smooth-jump-to-sub-navigation-menu-in-one-js-call/) [源代码](http://www.onextrapixel.com/examples/smooth-jump-to-sub-navigation-menu-in-one-js-call/smooth-jump-to-sub-navigation-menu-in-one-js-call.zip)

### Jump-To Sub Navigation Menu使用教程 ###

#### 运行原理 ####

这个插件只做一件事情，就是让你无痛的整合你的内容。这个插件会自动搜索你的文档内容，然后根据文章的标题自动生成一个导航目录。当用户滚动文章的时候，导航会固定在一个位置，并且会在导航上高亮文章目前所知的区域。如下图所示：

![](http://pic.yupoo.com/reicky_v/DfQr4Tfa/medium.jpg)

#### 怎么使用插件 ####

首先你得引入jQuery文件和jquery.jumpto.js 和 immersive-slider.css这三个文件，可以去插件的[地址](https://github.com/peachananr/jumpto)看看的详细设置。全部设置完后，按照下面的结构来组织你的文章的内容。

**HTML**

    <div class="page_container">
	  <div class="jumpto-block">
	    <h2>Header 1</h2>
	    <h3>SubHeader 1</h3>
	    ....
	    <h3>SubHeader 2</h3>
	    ....
	  </div>
	  <div class="jumpto-block">  
	    <h2>Header 2</h2>
	    <h3>SubHeader 1</h3>
	    ...
	  </div>
	</div>

上面的这个结构全部被*page_container*这个div包裹，并且每一篇文章的一个大的区域，都要用一个类名为*junmp-block*的DIV包裹，这样插件才能正确理解每一个区域的标题和子标题。这里的*h2*是用来放以及标题的，*h3*是用来放二级标题的。

结构写好后，只要简单地设置下js就可以了：

    $(".page_container").jumpto({
	  firstLevel: "> h2",
	  secondLevel: false,
	  innerWrapper: ".jumpto-block",
	  offset: 400,
	  animate: 1000,
	  navContainer: false,
	  anchorTopPadding: 20,
	  showTitle: "Jump To",
	  closeButton: true
	});


上面具体的代码的意思是，用H2标签设置一级标题，并且指定*jump-block*这个DIV为区域包裹容器。而且这个生成的导航，当用户滚动文章距离顶部400px的时候，会固定在页面上不再滚动，当点击导航里菜单的时候，会平滑的滚动对应的区域。

下面来详细说说这个插件具体的一些配置的意思：

- firstLevel:这个设置是定义你一级标题所用的标签。默认的设置是*h2*，指定一级标题都得用*h2*标签来定义。

- secondLevel:插件也支持多级标题。*secondLevel*这个设置选项就是用来设置二级标题。比如我们可以指定*h3*标签来指定二级标题用H3标签定义，默认这个选项是关闭的。

- innerWrapper：用来设置包裹区域包裹容器的选择器的类名，默认的是*jump-block*。

- offset：定义导航滚动多少距离就固定位置的，默认是400px。

- animate：这个是用来定义滚动的动画效果的，比如*slow*或者是*fast*。设置为*false*可以关闭动画效果。

- navContainer:如果你想自定义改变导航的位置，比如（DEMO3那样），这个设置选项就可以派上用场了。默认这个设置是关闭的。

- showTitle：这个是来设置导航菜单的title的，默认是*Jump To*。

- anchorTopPadding 这个是用来设置当导航菜单被点击的时候，设置顶部padding值用的。也可以用来控制顶部和你页面头部的距离，默认是20PX。

- closeButton：你也可以控制关闭按钮在导航菜单的显示和隐藏，默认是显示的，你也设置为false隐藏关闭按钮。

![](http://pic.yupoo.com/reicky_v/DfQVhTi5/medium.jpg)

差不多就到这里了。当然你也可以自定义导航的显示样式，在*jumpto.css*这个文件中改就可以了。你也可以看看demo中我是怎么通过改变样式，使导航从侧边的位置固定在顶部。




