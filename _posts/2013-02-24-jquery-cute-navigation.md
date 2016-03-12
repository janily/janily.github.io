---
layout: post
title: 用jquery制作可爱的文字导航
categories:
- Life
tags:
- jquery
- 翻译
---
今天在看以前收藏的文章的时候，翻到了一篇来自[tutsplus](http://net.tutsplus.com/tutorials/javascript-ajax/how-to-create-a-mootools-homepage-inspired-navigation-effect-using-jquery/)的博文，讲的是用jquery来制作一款可爱的文字导航觉的还不错，特整理上来。

先来预览下效果

<iframe style="width: 100%; height: 300px" src="http://jsfiddle.net/janily/6CEGC/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

##第一步，写html结构

这里只写出导航的主题结构，那些个完整的文档声明等就不写啦。完整代码可以查看源代码。

<pre>
<code>
&lt;div id=&quot;navigation-block&quot;&gt;
&lt;ul id="sliding-navigation"&gt;
    &lt;li class="sliding-element"&gt;&lt;h3>Navigation Title&lt;/h3&gt;&lt;/li&gt;
    &lt;li class="sliding-element"&gt;&lt;a href="#"&gt;Link 1&lt;/a&gt;&lt;/li&gt;
    &lt;li class="sliding-element"&gt;&lt;a href="#"&gt;Link 2&lt;/a&gt;&lt;/li&gt;
    &lt;li class="sliding-element"&gt;&lt;a href="#"&gt;Link 3&lt;/a&gt;&lt;/li&gt;
    &lt;li class="sliding-element"&gt;&lt;a href="#"&gt;Link 4&lt;/a&gt;&lt;/li&gt;
    &lt;li class="sliding-element"&gt;&lt;a href="#"&gt;Link 5&lt;/a&gt;&lt;/li&gt;
&lt;/ul&gt;
<div>
</code>
</pre>

##第二步，写样式

为了让导航开起来美观，我们需要添加下样式。样式如下：

<pre>
<code>
body
{
	margin: 0;
	padding: 0;
	background: #1d1d1d;
	font-family: "Lucida Grande", Verdana, sans-serif;
	font-size: 100%;
}
ul#sliding-navigation
{
	list-style: none;
	font-size: .75em;
	margin: 30px 0;
}
ul#sliding-navigation li.sliding-element h3,
ul#sliding-navigation li.sliding-element a
{
	display: block;
	width: 150px;
	padding: 5px 15px;
	margin: 0;
	margin-bottom: 5px;
}
ul#sliding-navigation li.sliding-element h3
{
	color: #fff;
	background: #333;
	border: 1px solid #1a1a1a;
	font-weight: normal;
}
ul#sliding-navigation li.sliding-element a
{
	color: #999;
	background: #222;
	border: 1px solid #1a1a1a;
	text-decoration: none;
}
ul#sliding-navigation li.sliding-element a:hover { color: #ffff66; }

</code>
</pre>

##第三步，写js实现交互效果

为了实现交互效果，我们首先定义一个函数方法（函数名就叫slide）。需要定义五个参数 (导航id, 弹出, 弹入, 时间, and 频率):参数的具体含义如下：

1.导航id:导航id指包裹导航内容的ul，以便于我们操作导航元素。

2.弹出:指鼠标移入的时候，鼠标所移入的导航栏目的所要移动的距离，是用padding来实现的。

3.弹入:指鼠标移出的时候，鼠标所移入的导航栏目的恢复原位也是导航栏目默认的左边距离，也是是用padding来实现的。

4.时间:指鼠标移入移出，导航栏目移动距离和回复原位的时间。

5.频率:指弹入弹出的频率。

所以，我们需要一个名为sliding_effect.js的js文件，代码如下:

<pre>
<code>
function slide(navigation_id, pad_out, pad_in, time, multiplier)
{
	// 首先定义一些我们需要的目标变量
	var list_elements = navigation_id + " li.sliding-element";
	var link_elements = list_elements + " a";
	// 定义动画快慢的频率变量
	var timer = 0;
	// 遍历目标元素，添加动画效果，页面刚载入的时候就出现这个效果，用左边距来实现
	$(list_elements).each(function(i)
	{
		// margin left = - ([width of element] + [total vertical padding of element])
		$(this).css("margin-left","-180px");
		// updates timer
		timer = (timer*multiplier + time);
		$(this).animate({ marginLeft: "0" }, timer);
		$(this).animate({ marginLeft: "15px" }, timer);
		$(this).animate({ marginLeft: "0" }, timer);
	});
	// 为鼠标移入添加动画效果
	$(link_elements).each(function(i)
	{
		$(this).hover(
		function()
		{
			$(this).animate({ paddingLeft: pad_out }, 150);
		},
		function()
		{
			$(this).animate({ paddingLeft: pad_in }, 150);
		});
	});
}

</code>
</pre>

接下来为了在页面的载入的时候，使我们的编写的动画效果生效，我们需要写如下代码

<pre>
<code>
	$(document).ready(function()
		{
			slide([navigation_id], [pad_out], [pad_in], [time], [multiplier]);
		});
</code>
</pre>

这个完整的js代码如下

<pre>
<code>
$(document).ready(function()
{
	slide("#sliding-navigation", 25, 15, 150, .8);
});

function slide(navigation_id, pad_out, pad_in, time, multiplier)
{
	// 首先定义一些我们需要的目标变量
	var list_elements = navigation_id + " li.sliding-element";
	var link_elements = list_elements + " a";
	// 定义动画快慢的频率变量
	var timer = 0;
	// 遍历目标元素，添加动画效果，页面刚载入的时候就出现这个效果，用左边距来实现
	$(list_elements).each(function(i)
	{
		// margin left = - ([width of element] + [total vertical padding of element])
		$(this).css("margin-left","-180px");
		// updates timer
		timer = (timer*multiplier + time);
		$(this).animate({ marginLeft: "0" }, timer);
		$(this).animate({ marginLeft: "15px" }, timer);
		$(this).animate({ marginLeft: "0" }, timer);
	});
	// 为鼠标移入添加动画效果
	$(link_elements).each(function(i)
	{
		$(this).hover(
		function()
		{
			$(this).animate({ paddingLeft: pad_out }, 150);
		},
		function()
		{
			$(this).animate({ paddingLeft: pad_in }, 150);
		});
	});
}
</code>
</pre>

打完收工，效果预览在[这里]()