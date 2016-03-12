---
layout: post
title: 用jquery制作关灯效果
categories:
- Life
tags:
- jquery
---

关灯效果现在在图片浏览和视频网站应用很普遍。简而言之，关灯就是使网站全部变黑，把想要突出的部分更加突出的显示给访问者。今天在项目中的一个图片的内页中需要用到这个效果，就是当点击关灯后，网页全黑而图片则突出显示给用户。

我是jquery来实现，记录下。先来预览下效果。[链接](http://jsfiddle.net/janily/MPr5d/embedded/result/)。

<iframe style="width: 100%; height: 300px" src="http://jsfiddle.net/janily/MPr5d/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

这个示例非常简单，实际项目中可能情况就复杂点，但是基本思路应该都是这样的。

首先我们需要一个黑色半透明的背景来实现的关灯效果，当然这里是指除了关灯两个字外，实际情况可能复杂点。这个层需要覆盖在页面所有元素之上，所以我们需要用到绝对定位和大一点z-index来达到效果。下面来写一些基本的html和css。

html 

<pre>
<code>
	&lt;a href="javacript:;"id="turnOff"&gt;&lt;/a&gt;
	&lt;div id="shadow"&gt;&lt;/div&gt;
</code>
</pre>

css

<pre>
<code>
#turnOff {
    color:#f00;
    font-size:16px;
}
#shadow {
	background:#000;
	position:absolute;
	left:0;
	top:0;
	width:100%;
	z-index:100;
	opacity:0.80;
	filter: alpha(opacity = 80);
	zoom:1;
}
.active {
	position:relative;
	z-index: 8888;
}
</code>
</pre>

这里我们没有在css里面定义遮罩层的高度，因为如果设置高度为100%的话，有可能网页内容比较高或者比较短，这样的话遮罩层可能遮罩不了整个网页。

这个时候我们可以jquery动态来计算，如下

<pre>
<code>
$("#shadow").css("height", $(document).height()).hide();
</code>
</pre>

这段代码的意思是获取这个文档窗口的高度，然后直接赋给shadow这个层，刚载入的时候是隐藏遮罩层的。

我们需要当点击id为turnOff的标签的时候，显示遮罩层,在此点击的时候关闭遮罩层。可以这样写：

<pre>
<code>
 $("#lightSwitcher").click(function(){
        $("#shadow").toggle();
});
</code>
</pre>

jquery中的toggle方法可以用来切换一个元素的可见性。所以我们上面用toggle方法来切换遮罩层的可见性。

当然这里也会有一个小问题，就是当遮罩层显示后由于是绝对定位而且z-index很大，这样也会覆盖关灯的链接上，而不能点击。所以在切换遮罩层的时候，需要增大关灯链接的z-index属性要大于遮罩层的z-index，这样才能实现关灯开灯的效果。我们这里用了一个active类，显示遮罩层的时候，把这个类加到关灯链接上。如下

<pre>
<code>
$(#shadow).css("height",$(document).height()).hide();
$(#turnOff").click(function(){
	$("#shadow").toggle();
	if($("#shadow")is(":hidden")){
		$(this).html("关灯");
		$(this).removeClass("active");
		}
	else {
		$
		$(this).html("开灯");
		$(this).addClass("active")
		}；
	});
</code>
</pre>

全部完成，可以跟据项目特点完善的更好一些。本文参考互联网上的诸多资料，在此不一一感谢了。



