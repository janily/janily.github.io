---
layout: post
title: 用css3制作简洁大方的下拉菜单
categories:
- Life
tags:
- css3
- 翻译
---

今天翻译的文章同样来自来自[designmodo](designmodo)网站的一篇css3制作下拉菜单的文章.原文在[这里](http://designmodo.com/create-css3-mega-menu/).

首先我们来预览下效果(点击result选项卡就可以看到效果了).

<iframe style="width: 100%; height: 300px" src="http://jsfiddle.net/janily/P6k8g/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

这个教程将教大家怎样利用css3的新特性来制作一款可爱简洁大方的下拉菜单.像这样的菜单在一些商业、企业类型的网站可以经常看到,而且在未来也会非常的流行,因为这类型的菜单可以非常好的组织网站的内容.而且像这样的网站在[The Bricks UI](http://designmodo.com/the-bricks-addons/)很常见.废话不多说开始吧！

###HTML结构

首先我们先写一个class为nav,然后为每一个li标签里添加超链接.如果想为添加下拉项目直接诶就可以在li标签里面写.

<pre>
<code>
&lt;ul class="nav"&gt;
    &lt;li&gt;
        &lt;a href="#"&gt;What's new&lt;/a&gt;
        &lt;div&gt;
            Mega Menu Content...
        &lt;/div&gt;
    &lt;/li&gt;
    ...
    &lt;li&gt;&lt;a href="#"&gt;All Categories&lt;/a&gt;&lt;/li&gt;
    &lt;li class="nav-search"&gt;
        &lt;form action="#"&gt;
            &lt;input type="text" placeholder="Search..."&gt;
            &lt;input type="submit" value=""&gt;
        &lt;/form&gt;
    &lt;/li&gt;
&lt;/ul&gt;
</code>
</pre>

###样式

首先为了统一在各个浏览器的表现需要重置下样式.

<pre>
<code>
.nav,
.nav a,
.nav ul,
.nav li,
.nav div,
.nav form,
.nav input {
    margin: 0;
    padding: 0;
    border: none;
    outline: none;
}
.nav a { text-decoration: none; }
.nav li { list-style: none; }
</code>
</pre>

接着我们给菜单添加下基本样式和给li标签添加浮动样式,这样它们就会排成一行.

<pre>
<code>
.nav {
    display: inline-block;
    position: relative;
    cursor: default;
    z-index: 500;
}
.nav > li {
    display: block;
    float: left;
}
</code>
</pre>

###Style the Menu Links

具体编写菜单项目的样式,如内边距,高度,位置,字体大小,背景颜色,边框等等.这里要注意我们为了是菜单下拉的表现更具平滑自然,需要添加css3的transitions.为了支持大部分的浏览器,我们还需要添加各种前缀.

<pre>
<code>
.nav > li > a {
    position: relative;
    display: block;
    z-index: 510;
    height: 54px;
    padding: 0 20px;
    line-height: 54px;
    font-family: Helvetica, Arial, sans-serif;
    font-weight: bold;
    font-size: 13px;
    color: #fcfcfc;
    text-shadow: 0 0 1px rgba(0,0,0,.35);
    background: #372f2b;
    border-left: 1px solid #4b4441;
    border-right: 1px solid #312a27;
    -webkit-transition: all .3s ease;
    -moz-transition: all .3s ease;
    -o-transition: all .3s ease;
    -ms-transition: all .3s ease;
    transition: all .3s ease;
}
</code>
</pre>

鼠标经过的我们也要添加些样式,改变下背景就可以了.当然这里有一个小小要注意的地方是菜单的第一个标签是不需要右上角和右下角的圆角的,所以我们可以用css的伪类来去除右上右下的圆角.

<pre>
<code>
.nav > li:hover > a { background: #4b4441; }
.nav > li:first-child > a {
    border-radius: 3px 0 0 3px;
    border-left: none;
}
</code>
</pre>

###定义搜索框的样式

在开始设置搜索框的样式前,我们先把form表单设为相对定位,宽度设为inherit,就会自适应它子元素的宽度.

<pre>
<code>
.nav > li.nav-search > form {
    position: relative;
    width: inherit;
    height: 54px;
    z-index: 510;
    border-left: 1px solid #4b4441;
}
</code>
</pre>

然后就具体设置input的浮动,高度,内边距等等.在这里我们首先把input的宽度设为1像素并把它的左右边距设为0.而当鼠标滑过或者是获得焦点时,就重新设置inputd的宽度和内边距.这样我们这个搜索框就有了一个非常友好的动画交互效果.

<pre>
<code>
.nav > li.nav-search input[type="text"] {
    display: block;
    float: left;
    width: 1px;
    height: 24px;
    padding: 15px 0;
    line-height: 24px;
    font-family: Helvetica, Arial, sans-serif;
    font-weight: bold;
    font-size: 13px;
    color: #999999;
    text-shadow: 0 0 1px rgba(0,0,0,.35);
    background: #372f2b;
    -webkit-transition: all .3s ease 1s;
    -moz-transition: all .3s ease 1s;
    -o-transition: all .3s ease 1s;
    -ms-transition: all .3s ease 1s;
    transition: all .3s ease 1s;
}
.nav > li.nav-search input[type="text"]:focus { color: #fcfcfc; }
.nav > li.nav-search input[type="text"]:focus,
.nav > li.nav-search:hover input[type="text"] {
    width: 110px;
    padding: 15px 20px;
    -webkit-transition: all .3s ease .1s;
    -moz-transition: all .3s ease .1s;
    -o-transition: all .3s ease .1s;
    -ms-transition: all .3s ease .1s;
    transition: all .3s ease .1s;
}
</code>
</pre>

搜索按钮的样式就简单了,添加一个背景和设置圆角,同样也需要设置transitions属性,这样才能使其动画效果更加平滑.

<pre>
<code>
.nav > li.nav-search input[type="submit"] {
    display: block;
    float: left;
    width: 20px;
    height: 54px;
    padding: 0 25px;
    cursor: pointer;
    background: #372f2b url(../img/search-icon.png) no-repeat center center;
    border-radius: 0 3px 3px 0;
    -webkit-transition: all .3s ease;
    -moz-transition: all .3s ease;
    -o-transition: all .3s ease;
    -ms-transition: all .3s ease;
    transition: all .3s ease;
}
.nav > li.nav-search input[type="submit"]:hover { background-color: #4b4441; }
</code>
</pre>

接下来就是下拉菜单部分啦,我们要设置下拉菜单的位置,宽度等样式.然后用到opacity,visibility和overflow三个属性隐藏下拉菜单.接着就是定义诸如背景颜色,圆角和transitions.为什么要这三个属性来隐藏下拉菜单而不是直接设置display:none呢?因为用display:none俩隐藏的话transition就不会起作用了.

<pre>
<code>
.nav > li > div {
    position: absolute;
    display: block;
    width: 100%;
    top: 50px;
    left: 0;
    opacity: 0;
    visibility: hidden;
    overflow: hidden;
    background: #ffffff;
    border-radius: 0 0 3px 3px;
    -webkit-transition: all .3s ease .15s;
    -moz-transition: all .3s ease .15s;
    -o-transition: all .3s ease .15s;
    -ms-transition: all .3s ease .15s;
    transition: all .3s ease .15s;
}
</code>
</pre>

定义鼠标滑过的时候显示下拉菜单

<pre>
<code>
.nav > li:hover > div {
    opacity: 1;
    visibility: visible;
    overflow: visible;
}
</code>
</pre>

下拉菜单项目具体内容不写,这个教程主要核心的就是上面的了.

首先我们来预览下效果(点击result选项卡就可以看到效果了).

<iframe style="width: 100%; height: 300px" src="http://jsfiddle.net/janily/P6k8g/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>