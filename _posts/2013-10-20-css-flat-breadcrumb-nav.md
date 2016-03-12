---
layout: post
title: 用CSS创建一个扁平化设计风格的面包屑导航
categories:
- Life
tags:
- css
---

> 这篇文章主要是来自[line25](http://line25.com/)网站的[How To Create Flat Style Breadcrumb Links with CSS](http://line25.com/tutorials/how-to-create-flat-style-breadcrumb-links-with-css)。具体内容有删减，添加了一些自己的理解。

随着CSS和CSS3技术的发展，以前很多的只能用背景图片才能实现的效果，现在用CSS也能实现。在本文中，我们将完全用CSS技术来实现一个扁平化设计风格的的面包屑导航。如下图所示：

![](http://pic.yupoo.com/reicky_v/DfymMrS8/medium.jpg)

如上图所示，在以前我们一般用背景图片来实现这样的效果，但是用CSS中的边框属性我们也可以创建这样的效果。

首先是HTML结构：

    <div id="crumbs">
	<ul>
		<li><a href="#">Breadcrumb</a></li>
	</ul>
	</div>

我们主要用一个无序列表来作为我们面包屑导航的结构，每一个*<li>*标签包裹一个链接。

通过编写样式效果如下图所示：

![](http://pic.yupoo.com/reicky_v/DfyuyQCe/medium.jpg)

下面是CSS代码：

    #crumbs ul li a {
	display: block;
	float: left;
	height: 50px;
	background: #3498db;
	text-align: center;
	padding: 30px 40px 0 40px;
	position: relative;
	margin: 0 10px 0 0; 
	
	font-size: 20px;
	text-decoration: none;
	color: #fff;
	}

上面的代码最终的效果是，定义每一个链接的样式。主要是文字居中和按钮的背景颜色以及按钮的宽高。还有把链接的定位属性设置为了相对定位，这样后面再定义元素绝对定位的时候，就会以它为父级元素来定位。

主要是用到了css中的伪元素配合边框属性来实现三角箭头的效果，如下代码所示：

    #crumbs ul li a:after {
	content: "";  
	border-top: 40px solid red;
	border-bottom: 40px solid red;
	border-left: 40px solid blue;
	position: absolute; right: -40px; top: 0;
	}

在a标签的后面插入一个内容为空的的元素，配合元素的边框设置，就很容易实现三角的效果啦，如下图所示：

![](http://pic.yupoo.com/reicky_v/DfyAKbqu/medium.jpg)

以前这样的效果只能靠背景图片来实现。现在用伪元素就可以很容易的实现了。用边框来实现三角形这种技术已经很流行了，再回过头看我们上面的代码，如果要实现我们文章开头图片所示的那样的效果改怎么做呢？很简单，我们只要把其中两个边框的颜色设置为透明就可以实现。然后再用绝对定位把位置定好，就大功告成啦。

代码如下：

    border-top: 40px solid transparent;
	border-bottom: 40px solid transparent;
	border-left: 40px solid #3498db;

效果如图所示：

![](http://pic.yupoo.com/reicky_v/DfyG5Xkw/medium.jpg)

接下来要实现下图所示的效果就简单了，同样是用伪元素来实现，只不过我们这次是用*:before*这个伪元素来实现，代码如下：

    #crumbs ul li a:before {
	content: "";  
	border-top: 40px solid transparent;
	border-bottom: 40px solid transparent;
	border-left: 40px solid #d4f2ff;
	position: absolute; left: 0; top: 0;
	}

![](http://pic.yupoo.com/reicky_v/DfyKnHV9/medium.jpg)

明白了原理后，就可以实现文章开头所说的效果了，首先是结构：

    <div id="crumbs">
	<ul>
		<li><a href="#1">One</a></li>
		<li><a href="#2">Two</a></li>
		<li><a href="#3">Three</a></li>
		<li><a href="#4">Four</a></li>
		<li><a href="#5">Five</a></li>
	</ul>
	</div>

加上三角形的样式后是这样的：

![](http://pic.yupoo.com/reicky_v/DfyM9YEm/medium.jpg)

添加一些简单的样式后，一个面包屑导航效果差不多就出来啦！

    #crumbs ul li:first-child a {
	border-top-left-radius: 10px; border-bottom-left-radius: 10px;
	}
	#crumbs ul li:first-child a:before {
		display: none; 
	}
	
	#crumbs ul li:last-child a {
		padding-right: 80px;
		border-top-right-radius: 10px; border-bottom-right-radius: 10px;
	}
	#crumbs ul li:last-child a:after {
		display: none; 
	}

![](http://pic.yupoo.com/reicky_v/DfyNnqsV/medium.jpg)

这里我们用到了css中两个伪类选择器*:first-child*和*:last-child*来删除导航的第一个和最后一个元素的三角形，然后用*border-radius*定义了圆角，还定义了鼠标滑过的效果，如下图所示：

![](http://pic.yupoo.com/reicky_v/DfyOZocK/medium.jpg)

    #crumbs ul li a:hover {
	background: #fa5ba5;
	}
	#crumbs ul li a:hover:after {
		border-left-color: #fa5ba5;
	}

最后一个扁平化设计风格的面包屑导航就完成了。

可以去这个[地址](http://line25.com/wp-content/uploads/2013/breadcrumbs/demo/demo.html)看实际的效果。



