---
layout: post
title: 更好地语义化书写你的HTML代码：你现在就应该开始使用的8个HTML标签
categories:
- Life
tags:
- html
- 翻译
---

> 在如今的web开发中，语义化被提到的越来越越多。但是什么是语义化呢？语义化又为什么这么重要呢？今天我们就来谈谈在平时开发中，语义化的一些tips。这篇文章来自[davidwalsh](http://davidwalsh.name/)的[8 HTML Elements You’re Not Using (and Should Be)](http://davidwalsh.name/8-html-elements)。

在如今的web开发中，语义化被提到的越来越越多。但是什么是语义化呢？语义化又为什么这么重要呢？

所谓语义化HTML就是要传达文档的一个具体含义。它不仅仅是指文档的外在形式，更重要的是要文档的具体含义。当然这个对于机器而言的。一个好的语义化的文档能够帮助人们和机器更好地理解你文档所传达的具体含义。

对于屏幕阅读器来说，语义化的文档结构能够使它更好地理解和读懂文档。对于SEO也更加友好。对于现代的web浏览器来说语义化文档也能提高它的工作效率。当然语义化文档还能减少代码的冗余，使你的代码更加容易阅读。

总而言之，语义化是大势所趋，但是在web开发中，怎么进行语义化开发呢？一个好的方法是，在我们平时编写HTML文件的时候，用正确合适的标签来表达合适的内容。那就让我们来看看怎么来开始我们的语义化吧。

### &lt;q&gt; ###

这个标签跟*blockquote*类似，也是用来表示引用文本。

为什么使用quotation标签呢？引用标签当然并不意味着就是引用。有时候，引用也用于强调、讽刺的意思或者其它一些需要需要确认名称的东东。在这些情境中，使用引用完全是没有问题的。但是，如果仅仅是引用某些人说过的话，那么**&ltq&gt;**标签无疑更具有语义。

### &lt;i&gt;和&lt;b&gt; ###

在以前的思维中，i和b标签一般是用来定义斜体的字体和加粗字体的。不过当web开发中，越来越流行结构、行为、表现相分离的开发理念，那使用i和b标签就有点违背这个理念的，因为这两个标签混合了表现和结构。它们被**em****strong**这两个标签所代替，这两个标签就是用来表示强调的语义行为。

在HTML5标准中，i和b标签被赋予了新的语义。**i**标签用于表示单独的或者是语气的一个语义。这个非常有用；**b**标签用于表示跟正常文本区分开来，但是没有任何的语义。那它跟span标签有什么区别呢？区别就是&lt;b&gt;标签没有任何语义。

### &lt;abbr&gt; ###

abbr标签是用于缩写词的。这个对于一些缩写词的表示是非常方便的。abbr标签有一个title属性，是用来设置缩写词的具体的正确的书写。比如：

    <abbr title="laugh out loud">lol</abbr>
	<abbr title="I don't know">idk</abbr>
	<abbr title="got to go">g2g</abbr>
	<abbr title="talk to you later">ttyl</abbr>

### &lt;time&gt; ###

让我们来看一看一个经典的关于本地时间的问题。在美国，2013年10月5日是用10/05/13这种格式来表示的。在应该则是用05/10/13表示的，在荷兰，则是用05-10-13来表示的。在俄罗斯则是用05.10.13来表示的。在时间上的表示每一个地区或者是国家的表示方法都不同。对于机器来说，去理解各个不同的时间格式是很困难的。

**time**标签就是用来表示机器可读的时间格式的。像上面的时间可以这样来表示**<time datetime="2013-10-05">10/05/13</time>**。HTML解析器就会解析我们想要表达的精确的具体时间，而跟它用什么格式来表示没有关系。<time>标签页允许我们用24小时制的格式来表示时间**<time datetime="2013-10-05 22:00">10/05/13 at 10 PM</time>**。

### &lt;mark&gt; ###

我们经常会做这样的事情:

    <p>
	  Three hundred pages of boring, useless text. <span class="highlight">The one thing you need to know and will never be able to find again if you don't highlight it.</span> More boring stuff…
	</p>

现在我们不需要这样做了。HTML5标准推荐用<mark>标签，来表示要突出高亮的文本。

### &lt;input&gt; ###

像&nbsp;input&nbsp;标签。我们经常会用到**&nbsp;input type="text"&nbsp;, &nbsp;input type="submit"&nbsp;**和**&nbsp;input type="hidden"&nbsp;**这三种类型的。但是在最新的HTML5的标准中？大大的扩展了input标签的功能，主要新增一下几种类型：

- email
- tel
- number
- ranger
- date
- time
- search
- color

新增的这些属性非常有用，但是在使用它们的时候，要确保浏览器是否都支持它，或者是采取降级的方法来使用它。

### &lt;menu&gt; ###

一般我们在写一个菜单的HTML的时候，我们会这样写：

    <ul class="menu-toolbar">
	  <li class="new">New</li>
	  <li class="open">Open</li>
	  <li class="save">Save</li>
	  <li class="quit">Quit</li>
	</ul>

现在我们可以用最新的HTML5中**&lt;menu&gt;**标签来表示菜单了，还可以设置具体的属性如**popup**和**toolbar**。

    <menu type="toolbar">
	  <li class="new">New</li>
	  <li class="open">Open</li>
	  <li class="save">Save</li>
	  <li class="quit">Quit</li>
	</menu>

### Tips ###

很多的前端开发者喜欢在HTML中用*&nbsp*来表示空格，但是很多人并不知道这个字符串的真实意思。你比如在两个单词之间，如果没有空格，那它们会连在一起。比如下面各种各样的单位：

- Units:12m/s
- Time:8 PM
- Proper nouns:Dairy Queen

当然，那我们也可以使用**&#8209;**这个连字符来分隔字符。

HTML关于语义化的这块的各种标准也在快速的发展。把上面的这些标签运用到你的HTML中，能够使你的代码更加简洁。当然如果想知道更多的关于语义化方面的知识，可以去这些地址详细了解[ Periodic Table of the Elements](http://joshduck.com/periodic-table.html)，[Mozilla Developer Network documentation](https://developer.mozilla.org/en-US/docs/Web/HTML?menu),也可以去W3C官方地址看看[W3C's HTML specification](http://www.w3.org/TR/html5/)。



