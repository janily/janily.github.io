---
layout: post
title: 用CSS3的Flex Box来进行布局
categories:
- Life
tags:
- css
- 翻译
---

> 前面学习了一些CSS3的Flex Box的一些基础知识，那怎么把他应用到实际的项目中呢？正好在Mozilla的网站上看到一篇Flex Box应用的文章，就翻译出来了。原文地址[Application Layout with CSS3 Flexible Box Module](https://hacks.mozilla.org/2013/12/application-layout-with-css3-flexible-box-module/)。

随着[CSS3 Flexible Box Layout Module](http://www.w3.org/TR/css3-flexbox/)的出现，现在来编写一个流体自适应的布局变得非常容易。在本文中，我们就用它编写一个简单应用的布局，它会自适应屏幕大小的变化。

在这里我们将使用[HTML5中的新的语义标签](http://www.w3.org/wiki/HTML_structural_elements)来编写HTML结构。使用新增的标签不仅能使我们的代码更加符合语义的规范，也能提高我们编写代码的效率，因为我们能够直接选定特定元素而不再需要定义一些特定的class和Id来选择元素。

首先来看看我们最终完成的这个效果[demo](http://www.speich.net/articles/css-flexiblebox-layout-full.html)。

### 第一步，编写三个垂直布局区域 ###

首先在body标签内编写**&lt;header&gt;**,**&lt;main&gt;**和**&lt;footer&gt;**这三个结构。

![](http://pic.yupoo.com/reicky_v/Dp7kVVn1/medium.jpg)

代码如下：

    <!DOCTYPE html>
	<html>
	<head>
	    <title>CSS3 Application Layout</title>
	</head>
	 
	<body>
	<header></header>
	<main></main>
	<footer></footer>
	</body>
	</html>

现在就来编写CSS代码使这三个布局区域垂直排列。这就需要把body的**display**值设置为**flex**，把**flex-direction**属性的值设置为**column**。设置后，就会定义浏览器垂直方向显示body标签内的子元素。

那怎么用**flex**来控制元素的空间大小呢？你可以去这个[地址](https://developer.mozilla.org/en-US/docs/Web/CSS/flex)详细了解有关的知识。在本文中的这个例子中，我们并不希望所有的元素来自动计算其空间的大小。我们固定**&lt;header&gt;**和**&lt;footer&gt;**这两个布局区域的高度，而**&lt;main&gt;**这个布局盒子自动来填充除去**&lt;header&gt;**和**&lt;footer&gt;**两个布局盒子的空间后的所有空间。

如下所示：

    html, body {
    height: 100%;
    width: 100%;
    padding: 0;
    margin: 0;
	}
	 
	body {
	    display: flex;
	    flex-direction: column;
	}
	 
	header {
	    height: 75px;
	}
	 
	main {
	    flex: auto;
	}
	 
	footer {
	    height: 25px;
	}

[demo](http://www.speich.net/articles/css-flexiblebox-layout1.html)。

### 第二步：水平排列布局区域 ###

让我们在**&lt;main&gt;**里面添加三个元素**&lt;nav&gt;,&lt;article;&gt;**和**&lt;aside&gt;**。但是我们希望这三个元素能够按水平方向来布局。

![](http://pic.yupoo.com/reicky_v/Dp7qKAB8/medium.jpg)

    <body>
	<header></header>
	<main>
	    <nav></nav>
	    <article></article>
	    <aside></aside>
	</main>
	<footer></footer>
	</body>

要达到这样的目的，我们需要把**&lt;main&gt;**这个元素的**display**的值设置为为**flex**以及**flex-direction**的值设置为**row**。而且还把**&lt;nav&gt;**和**&lt;aside&gt;**的宽度设置为固定的宽度，而**&lt;article&gt;**元素的宽度则为自动填充。代码如下：

    main {
    display: flex;
    flex-direction: row;
    flex: auto;
	}
	 
	nav {
	    width: 150px;
	}
	 
	article {
	    flex: auto;
	}
	 
	aside {
	    width: 50px;
	}

[demo](http://www.speich.net/articles/css-flexiblebox-layout2.html)

就是这么简单。试着调整浏览器窗口大小看看，布局也能自适应浏览器窗口的大小。

### 第三步：调整 ###

这里有一个小小的问题，就是当缩小浏览器窗口的时候，如果内容很多会出现滚动条。

![](http://pic.yupoo.com/reicky_v/Dp7uuy4c/medium.jpg)

因此，我们需要给元素设置一个最小宽度。把**&lt;body&gt;**和**&lt;main&gt;**的**overflow**值设置为**hidden**，而把**&lt;nav&gt;,&lt;**和**&lt;aside&gt;**的**overflow-y**的值设置为**auto**.

    body {
	overflow: hidden;
	display: flex;
	flex-direction: column;
	}
	 
	header {
		height: 75px;
		min-height: 75px;
	}
	 
	footer {
		height: 25px;
		min-height: 25px;
	}
	 
	main {
		display: flex;
		flex-direction: row;
		flex: auto;
		border: solid grey;
		border-width: 1px 0;
		overflow: hidden;
	}
	 
	nav {
		width: 150px;
		min-width: 150px;
	}
	 
	article {
		border: solid grey;
		border-width: 0 0 0 1px;
		flex: auto;
		overflow-x: hidden;
		overflow-y: auto;
	}
	 
	aside {
		width: 50px;
		min-width: 50px;
		overflow-x: hidden;
		overflow-y: auto;
	}

注意，我这里没有添加相关的浏览器前缀的，正式代码中需要添加相关浏览器的前缀，如**-webkit**.

### 最后一步：javascript代码 ###

最后，我们添加一个可以让用户用鼠标拖拽**&lt;aside&gt;**来改变它的宽度。我们需要多添加一个**&lt;div&gt;**来作为用户触发拖拽的钩子。

    <body>
	<header></header>
	<main>
	    <nav></nav>
	    <article></article>
	    <div class="splitter"></div>
	    <aside></aside>
	</main>
	<footer></footer>
	</body>

我们把**splitter**元素的宽度设置为4px，把鼠标的样式设置为**col-resize**，这样当用户把鼠标放到这个元素上的时候，会显示一个可以拖拽的光标。

    .splitter {
    border-left: 1px solid grey;
    width: 4px;
    min-width: 4px;
    cursor: col-resize;
	}

下面是编写javascript代码，允许用户拖拽splitter来关改变元素的宽度。

    var w = window, d = document, splitter;
 
	splitter = {
	    lastX: 0,
	    leftEl: null,
	    rightEl: null,
	 
	    init: function(handler, leftEl, rightEl) {
	        var self = this;
	 
	        this.leftEl = leftEl;
	        this.rightEl = rightEl;
	 
	        handler.addEventListener('mousedown', function(evt) {
	            evt.preventDefault();    /* prevent text selection */
	 
	            self.lastX = evt.clientX;
	 
	            w.addEventListener('mousemove', self.drag);
	            w.addEventListener('mouseup', self.endDrag);
	        });
	    },
	 
	    drag: function(evt) {
	        var wL, wR, wDiff = evt.clientX - splitter.lastX;
	 
	        wL = d.defaultView.getComputedStyle(splitter.leftEl, '').getPropertyValue('width');
	        wR = d.defaultView.getComputedStyle(splitter.rightEl, '').getPropertyValue('width');
	        wL = parseInt(wL, 10) + wDiff;
	        wR = parseInt(wR, 10) - wDiff;
	        splitter.leftEl.style.width = wL + 'px';
	        splitter.rightEl.style.width = wR + 'px';
	 
	        splitter.lastX = evt.clientX;
	    },
	 
	    endDrag: function() {
	        w.removeEventListener('mousemove', splitter.drag);
	        w.removeEventListener('mouseup', splitter.endDrag);
	    }
	};
	 
	splitter.init(d.getElementsByClassName('splitter')[0], d.getElementsByTagName('article')[0], d.getElementsByTagName('aside')[0]);

需要注意的是，由于某些原因，貌似我们这个拖拽在IE11、chrome31没有生效。

