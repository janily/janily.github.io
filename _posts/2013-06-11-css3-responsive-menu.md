---
layout: post
title: 使用css打造响应式导航菜单(翻译)
categories:
- Life
tags:
- css
- 翻译
---

今天在webdesignerwall博客上看到一篇用css制作响应式导航菜单的博文，浅显易懂，就把它翻译过来了。英文不是很好，翻译是按照自己的理解的路线来进行的翻译的。原文在[这里](http://webdesignerwall.com/tutorials/css-responsive-navigation-menu)。我一般翻译这类技术文章的时候，只是翻译跟文章主题内容相关的信息，其它一些跟主题信息无关紧要就不会照搬了。

demo在[这里](http://webdesignerwall.com/demo/responsive-menu/)，可以点进去看下，缩小浏览器窗口看，就可以看出效果了。

这个响应式导航的实现是基于css的媒体查询和伪类来实现的，无需依赖javascript技术，并且还可以灵活的运用css来调整导航的位置，如居左、居中、居右。兼容性也还可以。当然ie一如既往的悲剧，还是要靠javasript来辅助在ie上的实现。不过在移动端是没有压力的。

废话不多说，先来看看我们这个响应式导航要达到的响应式效果是咋样的呢？

首先，当客户端的尺寸小于我们设定的尺寸的时候，导航在表现形式上变化为一个下拉列表的形式，一图胜千言，看图就明白是啥意思啦。

<img src="/media/images/purpose-of-responsive-menu.png" width="360" height="247"/>

图2

<img src="/media/images/purpose-of-responsive-menu-2.png" width="560" height="216"/>

现在就开始正式编码啦，先是html代码如下

<pre>
<code>
&lt;nav class="nav"&gt;
	&lt;ul&gt;
		&lt;li class="current"&gt;&lt;a href="#"&gt;Portfolio&lt;/a&gt;&lt;/li&gt;
		&lt;li&gt;&lt;a href="#"&gt;Illustration&lt;/a&gt;&lt;/li&gt;
		&lt;li&gt;&lt;&lt;a href="#"&gt;Web Design&lt;/a&gt;&lt;/li&gt;
		&lt;li&gt;&lt;a href="#"&gt;Print Media&lt;/a&gt;&lt;/li&gt;
		&lt;li&gt;&lt;a href="#"&gt;Graphic Design&lt;/a&gt;&lt;/li&gt;
	&lt;/ul&gt;
</nav>
</code>
</pre>

###css

导航的css部分简洁易懂，需要注意的是，这里并没有使用浮动去排版li标签，而是用display:inline-block；来代替了浮动，这样我们就可以用text-align来控制导航的位置了。

<pre>
<code>
/* nav */
.nav {
	position: relative;
	margin: 20px 0;
}
.nav ul {
	margin: 0;
	padding: 0;
}
.nav li {
	margin: 0 5px 10px 0;
	padding: 0;
	list-style: none;
	display: inline-block;
}
.nav a {
	padding: 3px 12px;
	text-decoration: none;
	color: #999;
	line-height: 100%;
}
.nav a:hover {
	color: #000;
}
.nav .current a {
	background: #999;
	color: #fff;
	border-radius: 5px;
}
</code>
</pre>

###控制位置(这个就不用解释啦)

<pre>
<code>
/* right nav */
.nav.right ul {
	text-align: right;
}

/* center nav */
.nav.center ul {
	text-align: center;
}
</code>
</pre>

###处理ie浏览器

由于IE9以下的浏览器不支持html5新增的标签和css3的media query这个属性，所以我们需要引入两个脚本来使之支持。分别是 css3-mediaqueries.js (或者是 respond.js) 和 html5shim.js。当然你也可以不引用html5shim.js，那就要用div代替nav标签了。

<pre>
<code>
&lt;!--[if lt IE 9]&gt;
	&lt;script src="http://css3-mediaqueries-js.googlecode.com/files/css3-mediaqueries.js"&gt;&lt;/script&gt;
	&lt;script src="http://html5shim.googlecode.com/svn/trunk/html5.js"&gt;&lt;/script&gt;
&lt;![endif]--&gt;
</code>
</pre>

###响应式

下面就是激动人心的时候，我们要用media query来实现我们的响应式导航！至于media query是啥东东，可以查看以前的文章，[这里](http://webdesignerwall.com/tutorials/css3-media-queries)和[这里](http://webdesignerwall.com/tutorials/responsive-design-in-3-steps)，都是英文的哦，中文方面的可以大漠的博客看看[这里](http://www.w3cplus.com/content/css3-media-queries)。

我们600px为我们响应式的临界点，当客户端的尺寸是在600px(包括600px)以下的时候。我们把.nav设置为相对定位，这样我们就可以通过绝对定位来设定ul的位置。当然还要隐藏所有的导航栏目也就是li标签了。第一个li标签添加一个current的类，设置为display:block。

当鼠标经过ul的时候，把所有的li标签设置为display:block。至于位置就很好设置了(ul是绝对定位的)。这里就不啰嗦啦。

<pre>
<code>
@media screen and (max-width: 600px) {
	.nav {
		position: relative;
		min-height: 40px;
	}	
	.nav ul {
		width: 180px;
		padding: 5px 0;
		position: absolute;
		top: 0;
		left: 0;
		border: solid 1px #aaa;
		background: #fff url(images/icon-menu.png) no-repeat 10px 11px;
		border-radius: 5px;
		box-shadow: 0 1px 2px rgba(0,0,0,.3);
	}
	.nav li {
		display: none; /* hide all 
		margin: 0;
	}
	.nav .current {
		display: block; 
	}
	.nav a {
		display: block;
		padding: 5px 5px 5px 32px;
		text-align: left;
	}
	.nav .current a {
		background: none;
		color: #666;
	}

	/* on nav hover */
	.nav ul:hover {
		background-image: none;
	}
	.nav ul:hover li {
		display: block;
		margin: 0 0 5px;
	}
	.nav ul:hover .current {
		background: url(images/icon-check.png) no-repeat 10px 7px;
	}

	/* right nav */
	.nav.right ul {
		left: auto;
		right: 0;
	}

	/* center nav */
	.nav.center ul {
		left: 50%;
		margin-left: -90px;
	}
	
}
</code>
</pre>

最终的效果点击[这里](http://webdesignerwall.com/demo/responsive-menu/)








