---
layout: post
title: 滑动在图片说明中的运用(由Figure和Figcaption说起)
categories:
- Life
tags:
- css3
- javascript
---

###由Figure 和 Figcaption说起

在过去的web开发中，图片和和图片说明是一个常见的web元素，遇到这样的场景的时候，一般是用没有语义的DIV或者是span这类标签配合img来写这样的结构，现在就不用这样做了，在html5的规范中的Figure和Figcaption的标签就是专门为这样的场景而生的。

在w3c中的规范是这样描述它的。大概意思是：&lt;figure&gt; 标签规定独立的流内容（图像、图表、照片、代码等等）。&lt;figcaption&gt; 标签定义 figure 元素的标题（caption）。"figcaption" 元素应该被置于 "figure" 元素的第一个或最后一个子元素的位置。

在这里，我们就可以用使用这些新的元素来创建一个简单的优雅的图片说明文字滑动的交互效果。用强大的css3和javascript都可以做到。开始吧！

开始之前，来看看实际的效果是怎么样的。

<iframe width="100%" height="300" src="http://jsfiddle.net/jkeuf/3/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

###html结构

我们用html5新的Figure和Figcaption标签，来组织我们的结构，如下

<pre>
<code>
&lt;figure&gt;
	&lt;img alt="" src="http://hhhhold.com/400x300/jpg?21189" /&gt;
	&lt;figcaption&gt;
		为之，则难者易矣；不为，则易者难矣！
	&lt;/figcaption&gt;
&lt;/figure&gt;
</code>
</pre>

这里顺便说一下，我一般用hhhhold提供的图片来做demo的占位图片，很好用！

###css

由于我们做的交互效果图片说明也就是Figcaption默认是隐藏的，而且图片说明的显示方式是从底部慢慢滑动出来的的，我们用绝对定位来实现这个效果，所以figure元素需要一个相对定位的属性。完整的css代码如下

<pre>
<code>
figure { 
  display: block; 
  position: relative; 
  overflow: hidden; 
}

figcaption { 
  position: absolute; 
  background: rgba(0,0,0,0.75); 
  color: white; 
  padding: 10px 20px; 
  bottom: -30%;
  opacity: 0;
  -webkit-transition: all 0.6s ease;
  -moz-transition:    all 0.6s ease;
  -o-transition:      all 0.6s ease;
}

figure:hover figcaption {
  opacity: 1;
  bottom: 0;
}
</code>
</pre>

到这里，就完成了我们这个符合语义又简单的交互效果，当然这得在现代浏览器下才得以实现，而在一些不是那么现代的浏览器下，就悲剧了，像IE浏览器。

特别是在中国的这种浏览器环境下，你懂的，我们一般用javascript来解决这样的交互效果。下面就用jquery来实现下

###javascript实现

html代码和css不变，只是把用css3实现交互效果的部分用javascript来实现，是用jquery来实现的，首先得引入jquery库了。还有html5shim这个东东，是为IE9以下浏览器准备的，用来兼容html5新标签的。

<pre>
<code>
&lt;head&gt;
&lt;script src="resources/jquery.js"&gt;&lt;/script&gt;
&lt;!--[if lt IE 9]&gt;
	&lt;script src="http://html5shim.googlecode.com/svn/trunk/html5.js"&gt;&lt;/script&gt;
&lt;![endif]--&gt;
&lt;/head&gt;
</code>
</pre>

接下来就是js部分了，很简单没什么多说的，主要是用到jquery的动画这个函数来实现的，代码如下

<pre>
<code>
$(document).ready(function(){
	$('figcaption').css('bottom','-50');
	$('figure').hover(function(){
	$(this).find('figcaption').stop().animate({'bottom':'0'}, 200);
	},function(){
	$(this).find('figcaption').stop().animate({'bottom':'-50'}, 200);
	});
}); 
</code>
</pre>

<iframe width="100%" height="300" src="http://jsfiddle.net/jkeuf/3/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

很简单吧！有了css3这么强大的利器，我门可以做很多优雅的交互效果。















