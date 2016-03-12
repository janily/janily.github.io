---
layout: post
title: jquery插件实战-弹出框（1）
categories:
- Life
tags:
- jquery
---

学习jquery也有一段时间啦，在项目中用到jquery的地方大部分是一些交互方面的，如弹出框、幻灯片、选项卡等。几乎每个网站都有这些交互效果，这个时候jquery强大的插件功能就闪亮登场啦，我们可以把一些经常用的交互效果的功能封装成一个jquery插件，这样就可以大大提高我们的生产效率哦。

接下来就奉上我jquery插件的处女作，是一个弹出框的的插件，第一次写，有什么不完善的考虑不周的欢迎拍砖。

###一、功能讲解
1、鼠标点击按钮激活弹出窗
2、弹出窗弹出的时候会有一个半透明的遮罩遮住网页内容，突显弹出窗
3、弹出窗要水平垂直居中并且当有滚动条或者窗口缩小放大时候也要水平垂直居中。

写插件，就像做菜一样，首先得备好必备的材料，备好料后，就可以快速把菜搞出来。

写插件的时候，我们可以先把插件分为几个功能点，把功能点写好，再把功能点结合起来封装为插件。

按照上面的功能分析，首先来编写水平垂直居中这个功能。先写一个简单模型和css。

##html
<pre><code>
&ltdiv class="center"&gt;		
	&le;/div&gt;
</code></pre>

##css
<pre><code>
.center 
{
width:200px;
height:100px;
background: green;
}
</code></pre>

##jquery

用css中要把一个层水平垂直居中，首先要知道固定的宽高，给元素进行绝对定位，并设置left,top值为“50%”（此处最好使用绝对定位，如果只是单单水平居中，此处可以换成相对定位）；最后设置margin-top和margin-left的负值，而且其值分别为元素高度和宽度的一半 。

<pre>
<code>
center {
width: 200px;/*元素的宽度*/
height:200px;/*元素的高度*/
position: absolute;
left: 50%;/*配合margin-left的负值实现水平居中*/
margin-left: -100px;/*值的大小等于元素宽度的一半*/
top:50%;/*配合margin-top的负值实现垂直居中*/
margin-top: -100px;/*值的大小等于元素高度的一半*/
}
</code>
</pre>

但是我们通常要自适应宽高，这个时候css的方法就不适应了，js就派上用场啦，先上代码，后面简要解析下。
<pre>
<code>
$(document).ready(function(){
	$(window).resize(function(){
		$('.center').css({
			position:'absolute',
			left: ($(window).width() - $('.center').outerWidth())/2,
			top: ($(window).height() - $('.center').outerHeight())/2
		});
	});
	// 最初运行函数
	$(window).resize();
	});
</code>
</pre>

上面这一段的功能是把需要居中的元素设置为绝对定位，然后设置left和top值，left和top值主要是根据当前文档的宽高减去居中元素的宽高然后除以2，这样就可以居中了，当然在一般情况下的居中是没有问题的，但是当浏览器出现滚动条的时候就不适用了，那改怎么办呢？待下回分解。





