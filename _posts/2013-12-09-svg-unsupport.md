---
layout: post
title: SVG基础知识入门系列(5)-浏览器兼容性
categories:
- Life
tags:
- svg
- 翻译
---

通过上面一系列的文章，我们介绍了SVG的一些基础知识，对于不支持的浏览器我们可以使用[Raphael.js](http://raphaeljs.com/)这个第三方的库来解决。在本文章中我们具体来看一看怎么使用它们来解决兼容性问题。

![](http://pic.yupoo.com/reicky_v/Dn6vb7RS/medium.jpg)

我们可以直接在网页中使用[SVG elements](http://www.w3.org/TR/SVG/struct.html)来插入SVG元素，当然对于不支持SVG的浏览器，我们要准备一个回退的功能来是它能够渲染SVG元素。

现在我们就来看一看我认为不错的两个SVG的回退的解决方案。

### 使用对象来插入SVG ###

我们前面的文章有介绍到把SVG直接插入HTML结构的方法。比如我们可以使用**&lt;object&gt;**来插入**.svg**文件，如下：

    <object data='images/apple.svg'></object>  

我们来验证一下浏览器的兼容性，我们添加了一个苹果的SVG图标在页面里。当我们用一些不支持SVG的浏览器来浏览页面的时候，我们会发现浏览器是空的。为了解决这个问题，我们可以用下面的方法插入一张位图文件来解决这个问题。

    <object data='images/apple.svg'>  
    <img src='images/apple.png'/>  
	</object>  

用这种方法，浏览器支持SVG的就会显示SVG图片，不支持的，就会显示我们插入的位图。

![](http://pic.yupoo.com/reicky_v/Dn6DyKVj/medium.jpg)

不过这样就会有一个问题，前面说过位图跟SVG相比，缺乏可伸缩性。

### Using Modernizr ###

另外一种方法是使用[Modernizr](http://modernizr.com/)。如果你不是很熟悉这个库，不要担心我们后面会写一篇关于这个库的使用方法的文章。

首先，让我们引入一些库，**Modernizr和Raphael.js**。然后，我们就可以使用Rapheal.js提供的一个工具[ReadySetRaphael.js](http://readysetraphael.com/)来把我们的svg图片转化为浏览器可支持的代码，我们把代码保存在名为**svg.js**的文件里。

首先在HTML文件里引入Modernzr.js文件，如下所示：

    <script type="text/javascript" src="scripts/modernizr.js"></script>  

至于起塔两个js文件**raphael.js**和**svg.js**文件，我们需要处理一下，**对于不支持SVG的浏览器我们才引入它们**。

使用Modernizr我们就可以用来判断浏览器是否支持特定的功能，在这里我们只需要检测浏览器是否支持SVG文件，如果不支持才引入指定的js文件：

    if (!Modernizr.inlinesvg) {  
	document.write(  
	    '<script type="text/javascript" src="scripts/raphael.js"><\/script>',   
	    '<script type="text/javascript" src="scripts/svg.js"><\/script>'  
	);  
	}  

然后，需要在HTML添加下面的这些代码：

    <svg version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" width="500px" height="280px" viewBox="0 0 500 280" enable-background="new 0 0 500 280" xml:space="preserve">  
	<path fill="#333333" d="M296.908,120.622c-8.77,6.201-13.158,13.676-13.158,22.41c0,10.458,5.425,18.479,16.262,24.076  
    c-2.908,8.435-7.122,15.764-12.65,22.009c-5.516,6.243-10.553,9.368-15.11,9.368c-2.147,0-5.075-0.718-8.794-2.133l-1.782-0.687  
    c-3.646-1.416-6.854-2.133-9.656-2.133c-2.641,0-5.535,0.555-8.679,1.665l-2.237,0.807l-2.818,1.154  
    c-2.218,0.884-4.468,1.326-6.725,1.326c-5.328,0-11.208-4.387-17.642-13.161c-9.273-12.567-13.905-26.264-13.905-41.085  
    c0-10.538,2.886-19.02,8.678-25.46c5.78-6.432,13.446-9.658,22.979-9.658c3.566,0,6.897,0.653,10,1.958l2.129,0.865l2.238,0.92  
    c1.992,0.84,3.601,1.264,4.825,1.264c1.569,0,3.316-0.364,5.231-1.094l2.929-1.151l2.19-0.804c3.483-1.262,7.34-1.896,11.555-1.896  
    C282.777,109.183,290.814,112.996,296.908,120.622z M273.238,82.575c0.108,1.344,0.167,2.378,0.167,3.102  
    c0,6.628-2.412,12.442-7.237,17.443c-4.823,5-10.438,7.494-16.837,7.494c-0.189-1.493-0.29-2.563-0.29-3.212  
    c0-5.635,2.239-10.924,6.726-15.864c4.482-4.939,9.671-7.838,15.575-8.678C271.754,82.787,272.395,82.696,273.238,82.575z"/>  
	</svg>  
  
	<div id="applelogo"></div> 

在上面的代码中，我们直接把SVG代码插入在HTML文件中以及一个**div**给Raphael使用的。非常简单，我们还可以在图片的下面插入一个文字。

	<text x="210" y="250">This is SVG</text>  

执行结果如下图：

![](http://pic.yupoo.com/reicky_v/Dn6LZJsr/medium.jpg) 

你可以使用IE7/8和Google Chrome浏览器来浏览这个例子。

[demo](http://demo.hongkiat.com/scalable-vector-graphic-browsers/index.html)和[源代码](http://demo.hongkiat.com/scalable-vector-graphic-browsers/source.zip)

### 总结 ###

上面只是简单的举了几个例子，在一些特定的情况下，可能不会生效。但是一般来说，我们使用这些方法来解决浏览器不支持SVG的问题。

OK，SVG基础知识的系列就结束了。接下来，会介绍些用SVG制作交互进阶的文章。

