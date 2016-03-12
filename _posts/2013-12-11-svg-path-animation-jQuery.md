---
layout: post
title: 用jQuery和Illustrator来制作SVG动画
categories:
- Life
tags:
- svg
- 翻译
---

> 通过上面有系列的SVG基础入门文章学习后，基本上掌握了SVG的知识。那用SVG我们可以来做什么呢？以及可以做出什么样的效果呢？接下来会翻译一些我认为比较好的关于SVG方面的文章来探索一些SVG在web开发中的应用，特别是一些动画方面的应用。本文来自treehouse这个提供优秀WEB教程网站的一篇文章[SVG Path Animation with jQuery and Illustrator](http://blog.teamtreehouse.com/svg-path-animation-with-jquery-and-illustrator)。具体翻译内容有删减。

![](http://pic.yupoo.com/reicky_v/DnpL5g2B/medium.jpg)

时至今日，层出不穷的新技术为我们打造web应用提供了非常丰富的工具。矢量图片即svg就是其中的一种。SVG图片在过去应用不是很广泛，因为浏览器对它的支持还不是很全面，你可以去这个网站看看各个浏览器对SVG支持,[CaniUse.com](http://caniuse.com/#cats=SVG)。

在本文中，我们使用[Cam O‘Connell](http://www.camoconnell.com/)创建的[Lazy Line Painter](http://lazylinepainter.info/)这个jQuery插件来制作一个路径动画。Cam是一个前端开发工程师，并且对开发web交互和插件有着非常高的热情。首先我们来看看我们将要制作动画的一个效果和源代码。

[DEMO](http://treehouse-blog.s3.amazonaws.com/SVG%20Animations/initializr/index.html)和[源代码地址](http://treehouse-blog.s3.amazonaws.com/SVG%20Animations/initializr.zip)

教程开始之前，你需要一些关于HTML、CSS和jQuery的知识。如果你是一个菜鸟，没关系。这个教程没有太多的学习曲线。只要跟着我们的步伐就可以了。

我们来看一下本教程的一些步骤：

- 设置一个基本的HTML结构
- 下载需要的插件
- 用矢量编辑软件生成SVG文件
- 把SVG文件用Lazy Line Converter转换成SVG代码
- 把生成的代码放到新建好的main.js文件中
- 添加一些简单的样式

### 1设置HTML结构 ###

所谓兵马未动粮草先行，开始之前，我们需要搭建好一个基本的HTML环境。这里推荐用[Initializr](http://initializr.com/)这套HTML5模板来搭建我们的HTML环境：

![](http://pic.yupoo.com/reicky_v/DnpS2oDe/medium.jpg)

这个模板有以下特点：

- 是基于H5BP (HTML5 Boiler Plate)这个为基础的一套开发实践
- 没有模板(按需定制)
- 可以选择压缩相关文件，降低体积
- 针对IE生成特定的类
- 支持Chrome Frame

定制好我们需要的一个HTML环境后，我们就可以把生成的相关的文件下载下来。解压出来就可以使用了。

### 2下载相关插件 ###

HTML环境搭建好后，由于在initializr已经包含了jQuery文件，所以我们只需要下载相关的插件文件，下载好[Lazy Line Painter github repository](https://github.com/camoconnell/lazy-line-painter/archive/master.zip)这个文件就可以了，已经包含了lazy line painter插件文件。

![](http://pic.yupoo.com/reicky_v/DnpVv407/medium.jpg)

下载后解压文件。我们只需要**jquery.lazylinepainter-1.1.min.js**以及在**example -> js -> vendor ->**目录里的**raphael-min.js**这两个文件，把它们放到我们自己的initializr文件夹里的js文件夹里。

![](http://pic.yupoo.com/reicky_v/DnpXvSrM/medium.jpg)

下面需要把这两个文件在我们的*index.html*文件中引入，我们可以在文件的底部引入。我使用[Sublime Text2](http://www.sublimetext.com/)作为我的文件编辑器，在我们引入**main.js**之前引入这两个js文件。

    <script src="js/jquery.lazylinepainter-1.1.min.js"></script>
	<script src="js/raphael-min.js"></script>

在*&lt;body&gt;*标签中，我们会看到如下内容：

    <p>Hello world! This is HTML5 Boilerplate.</p>

我们用下面的这个结构把它包裹起来：

    <div id="icons"></div>

### 3用IIIustrator制作SVG文件 ###

在开始用IIIustrator制作SVG文件之前，让我们先来来了解下Lazy Line Painter这个插件的一些需要注意的点：

- 用IIIustrator来创建你的矢量路径
- 确保它们没有被填充
- 不要一个全闭合的路径-路径开始也得有结束
- 调整好画板来创作我们的图形
- 导出为SVG文件
- 文件的画布尺寸必须在1000*1000以下，体积保持在40kb一下

在本文中，我使用钢笔工具创作了几个简单的图标。在源文件中为**icons.ai**。

![](http://pic.yupoo.com/reicky_v/Dnq3UTV8/medium.jpg)

用lazylinepainter这个插件，你可以自如的控制边框的颜色和宽度。

用Adobe IIIustrator软件打开icons.ai这个文件。

然后把文件保存为SVG格式的文件，保存在initializr文件夹中的img文件夹中。只需要在保存的时候，在保存对话框的底部的格式中选择为SVG格式就可以了。IIIustrator软件会询问你一些保存SVG的选项。我们只需要按默认的设置保存就可以。接下来就要使用lazy line painter插件提供的SVG Lazy Line Converter来转换代码就可以了。

### 4使用SVG Lazy Line Converter来转换SVG格式的图片 ###

这一步，非常简单。确保你已经打开[Lazy Line Painter](http://lazylinepainter.info/)网站，滚动到页面底部会看到SVG to Lazy Line Converter这个字眼。把准备好的**icons.svg**文件拖拽到里面就可以了，然后就可以静静的等待神奇的事情即将发生。

![](http://pic.yupoo.com/reicky_v/Dnq8h3lj/medium.jpg)

我们只需要把生成的代码拷贝到我们的main.js文件里。选择点击**Copy Data to Clipboard**这个按钮。

### 5拷贝代码到main.js文件 ###

我们把代码拷贝到main.js文件里。在代码的底部，你会看到**strokeWidth**和**strokeColor**这两个变量。我们可以改变这两个变量的值。试着把strokeWidth的值改为*10*，把strokeColor的值改为*#262213*.

我们也可以改变动画的执行时间，默认的是**600毫秒**。我们改为**200毫秒**，

### 6添加CSS ###

为了使我们的例子能够显示的更加友好，我添加了一点CSS：

    /* ==========================================================================
   	Author's custom styles
   	========================================================================== */

	body { 
	    background:#F3B71C; 
	}
	
	#icons {
	    position: fixed;
	    top:50%;
	    left:50%;
	    margin: -300px 0 0 -400px;
	}

保存在**main.css**文件里，这样我们一个简单的SVG动画就做好了。

一些有用的资源

- [源代码](http://treehouse-blog.s3.amazonaws.com/SVG%20Animations/initializr.zip)
- [DEMO](http://treehouse-blog.s3.amazonaws.com/SVG%20Animations/initializr/index.html)
- [jQuery](http://jquery.com/)
- [Lazy Line Painter](http://lazylinepainter.info/)
- [initializr](http://initializr.com/)
- [Sublime Text2](http://www.sublimetext.com/)

