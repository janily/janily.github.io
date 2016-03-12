---
layout: post
title: 用jquery实现postion:sticky效果
categories:
- Life
tags:
- jquery
---

用户的屏幕越来越大，而页面太宽的话会不宜阅读，所以绝大部分网站的主体宽度和之前相比没有太大的变化，于是浏览器中就有越来越多的空白区域，所以你可能注意到很多网站开始在滚动的时候让一部分内容保持可见，比如，侧边栏的部分区域。position:sticky为此而生。

虽然现在在css3中有position:sticky这一个全新的属性,但W3C也是刚刚开始才讨论它,还没形成一个标准,浏览器的实现也只是在chrome开发版中才支持,但是现在又有这样的需求,这个时候就该javascript出场了.我现在用的jquery来实现它的.

下面先上基本的html结构

<pre>
<code>
	&lt;div class="wrapper"&gt;
	&lt;div class="header"&gt;
		导航
	&lt;div&gt;
	&lt;div class="nav"&gt;
		需要固定位置的导航
	&lt;/div&gt;
	&lt;div class="content"&gt;
		&lt;p&gt;
		天下事有难易乎？为之，则难者亦易矣；不为，则易者亦难矣。人之为学有难易乎？学之，则难者亦易矣；不学，则易者亦难矣。
		&lt;/p&gt;
	&lt;/div&gt;
&lt/div&gt;
</code>
</pre>

我们需要固定位置的是nav这一块,当然我们首先给它一样式,这是实现固定位置的关键类,我们用了position:fixed这一属性来固定位置,具体怎么实现呢?接着往下看.

<pre>
<code>
	.sticky {  
    position: fixed;  
    width: 100%;  
    left: 0;  
    top: 0;  
    z-index: 100;  
    border-top: 0;  
}  
</code>
</pre>

看起来是啥样子的呢,我在jsfiddle这一提供代码调试的线上工具里做了一个demo,如下

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/MgmnZ/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

现在可以看到在没有添加代码前nav这块,在滚动的时候没有固定在顶部,这里我们要用javascript来实现,具体思路是这样的,当滚动的时候,滚动的距离大于nav自身到顶部的距离,就给nav加上sticky这个,从而把nav固定在顶部,大概思路就是这样的,下面直接上js代码了,是基于jquery的

<pre>
<code>
$(document).ready(function() {
	var stickyNavTop = $('.nav').offset().top;  //获取nav距离顶部的距离
	
	var stickyNav = function(){
	var scrollTop = $(window).scrollTop();  //获取滚动的距离
	     
	if (scrollTop > stickyNavTop) {        //判断当滚动距离大于nav距离顶部的时候添加sticky类,负责删除sticky类
	    $('.nav').addClass('sticky');
	} else {
	    $('.nav').removeClass('sticky'); 
	}
	};
	
	stickyNav();
	
	$(window).scroll(function() {
		stickyNav();
	});
	});
</code>
</pre>

上面代码很简单,简单的注释了下.可以看下最终的效果.

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/MgmnZ/1/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>


