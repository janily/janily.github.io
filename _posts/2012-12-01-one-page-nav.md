---
layout: post
title: 用jquery实现单页面滚动导航
categories:
- Life
tags:
- web前端
- jquery
- javascript
- 翻译
---

在现在很多的web设计中，单页面应用的运用越来越多，而在单页面的设计中的导航的实现也是多种多样，可以看看这个[链接](http://www.jay-han.com/2012/03/20/8-plugins-tutorials-to-create-unique-presentational-scrolling-website/)，今天，我们就用jquery来创建一个非常简单的页面滚动导航的网站。

开始前可以先看看这个[效果](http://jsfiddle.net/janily/tUFtR/)。

##html结构

先写好一个基本的带导航的结构。
<pre><code>
&lt;nav&gt;
  &lt;ul&gt;
    &lt;li&gt;
	   &lt;a href="#paragraph1"&gt;Paragraph 1 &lt;/a&gt;
	&lt;/li&gt; 
	&lt;li&gt;
	   &lt;a href="#paragraph2"&gt;Paragraph 2 &lt;/a&gt;
	&lt;/li&gt; 
	&lt;li&gt;
	   &lt;a href="#paragraph3"&gt;Paragraph 3 &lt;/a&gt;
	&lt;/li&gt;
	&lt;li>
       &lt;a href="#paragraph4"&gt;Paragraph 4 &lt; &lt;/a&gt;
	&lt;/li&gt;
	&lt;li&gt;
	   &lt;a href="#paragraph5"&gt;Paragraph 5 &lt;/a&gt;
	&lt;/li&gt;
  &lt;/ul&gt;
&lt;/nav&gt;
</code></pre>

用nav标签写了一个基本导航结构，这其中可以看到a标签中的锚点都是链接到一个有特定的ID的内容区域。这样做的好处是，当用户的浏览器环境禁用js或者js失效的时候，导航仍然能正常工作；用jquery我们可以取到&quot;href&quot;这一属性的值，方便相关操作。

下面是内容的html结构。
<pre><code>
&lt;div id=&quot;content&quot;&gt;
	&lt;p id=&quot;paragraph1&quot;&gt;
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec luctus, dui nec porttitor rhoncus, magna mi adipiscing nisl, at laoreet nisl metus at leo. Maecenas bibendum, nibh ac blandit pharetra, eros tortor pretium lorem, ac posuere lorem leo imperdiet quam. Cras vitae eros libero, vel commodo sapien. Cras mauris dui, vulputate vel adipiscing pellentesque, mattis et ligula. Sed massa mauris, luctus sed feugiat at, bibendum vitae magna. Aenean pellentesque, purus non placerat fermentum, nibh ante posuere leo, eget vehicula nibh elit in elit. Proin aliquet aliquet odio vel auctor. Proin sed augue velit. Phasellus metus mauris, scelerisque vitae tincidunt eget, dignissim nec mi. Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Vestibulum eleifend, nisi et congue placerat, purus sem auctor magna, et cursus ante lectus a nulla. Vestibulum tempor, ipsum id congue accumsan, velit mauris rhoncus urna, et accumsan velit orci ac ligula. Vestibulum lobortis facilisis mauris, ut varius mi suscipit in. Donec lobortis diam id nisl tincidunt eget adipiscing mauris vestibulum.
&lt;/p&gt;
....
</code></pre>

这个布局非常简单，就不把代码全部贴上来啦，具体可以看demo的源代码。需要注意的是每个段落的id要与上面导航的id对应起来。

##css

写css前记得重置下css

<pre><code>
body {
font-family: arial;
font-size: 15px;
line-height: 25px;
color: #515151;
background: #fdfdfd;
}
 
#content {
width: 500px;
margin: 0 auto;
padding-top: 40px;
height: 2000px;
}
 
#content p {
margin-bottom: 25px;
}
nav {
width: 100%;
position: fixed;
height: 40px;
background: white;
border: 1px solid #CACACA;
border-top: none;
-webkit-box-shadow: 0px 0px 3px 1px #ebebeb;
-moz-box-shadow: 0px 0px 3px 1px #ebebeb;
box-shadow: 0px 0px 3px 1px #ebebeb;
}
 
nav ul {
width: 750px;
margin: 0 auto;
}
 
nav ul li{
float: left;
width: 150px;
text-align: center;
}
 
nav ul li a {
line-height: 40px;
font-size: 16px;
text-decoration: none;
color: #515151;
border-bottom: 1px dotted #515151;
}
 
nav ul li a:hover{
color: #000;
}
</code></pre>

导航用fixed这一定位属性，ie6下就不支持了~~~可以用css表达式来解决这一问题，可以看下[这个链接](http://www.alloyteam.com/2012/06/five-lines-of-code-perfect-solution-flashing-problem-from-ie6-to-the-chrome-browser-the-position-fixed/)。

##Jquery

主要的原理就是点击导航链接的时候，滚动相对应的内容区域。

首先当然是上jquery标准的代码。

<pre><code>
$(document).ready(function(){
 
});
</code></pre>

我们先来写最核心的滚动的代码的函数，名字就叫scrollToDiv。函数有两个参数，一个是要滚动的元素；一个是导航的高度。
<pre><code>
function scrollToDiv(element,navheight){
 
}
</code></pre>

接下来，我们需要定义三个变量。

<pre><code>
function scrollToDiv(element,navheight){
    var offset = element.offset();
    var offsetTop = offset.top;
    var totalScroll = offsetTop-navheight;
}
</code></pre>

这个&quot;offset&quot;是用来获取元素位置信息的，它会返回一个包含toip和left属性的对象。&offsetTop&quot;变量是用来获取元素顶部的距离，&quot;totalScroll&quot;使用来计算浏览器应该要滚动的距离。而第二个导航高度这个参数是用来计算浏览器滚动距离的。

我们用jquery的动画的函数animate来实现滚动的效果。这样可以实现一个很平滑自然的滚动的效果。

<pre><code>
$('body,html').animate({
    scrollTop: totalScroll
}, 500);
</code></pre>

到现在，我们完成了滚动的核心代码。
<pre><code>
function scrollToDiv(element,navheight){
    var offset = element.offset();
    var offsetTop = offset.top;
    var totalScroll = offsetTop-navheight;
     
    $('body,html').animate({
            scrollTop: totalScroll
    }, 500);
}
</code></pre>

现在就可以来写导航的点击事件啦~~！

<pre><code>
$('nav ul li a').click(function(){
 
    return false;
});
</code></pre>

在得到点击元素后，我们还必须得到元素&quot;attrf&quot;属性的值。

<pre><code>
var el = $(this).attr('href');
var elWrapped = $(el);
</code></pre>

这个el变量使用存储&quot;attr&quot;属性的值的。而elWrapped是用jquery的方法来存储变量el的值的，这样我们才能用jquery来操作它。如，

<pre><code>
$('#paragraph1').offset();
</code></pre>

完整的点击函数如下:
<pre><code>
$('nav ul li a').click(function(){
    var el = $(this).attr('href');
    var elWrapped = $(el);
     
    scrollToDiv(elWrapped,40);
     
    return false;
});
</code></pre>

这样我们就全部完成一个的单页面滚动导航的页面。再看看[demo](http://jsfiddle.net/janily/tUFtR/)。

原文在[这里](http://speckyboy.com/2012/03/07/scroll-to-internal-link-with-jquery/)。
