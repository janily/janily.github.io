---
layout: post
title: IE6,IE7下设置body{overflow:hidden;}失效Bug
categories:
- Life
tags:
- css
---

这段时间依然是在赶公司的项目,在做一个频道页面的时候,这里碰到一个IE6,IE7下设置body{overflow:hidden;}失效Bug,后来在google搜索网上有没有类似问题的解决方案的时候,在淘宝也是一前端的博客里找到这个问题的原因.特转载过来.

原文地址是[这里](http://www.veryued.org/2011/05/%E8%AF%91%EF%BC%8B%E6%95%B4%E7%90%86ie6ie7%E4%B8%8B%E8%AE%BE%E7%BD%AEbodyoverflowhidden%E5%A4%B1%E6%95%88bug/).

### 解决方法 ###

参考文章：http://haslayout.net/css/Document-Scrollbars-Overflow-Inconsistency

此项其实并不是Bug，只是各浏览器默认付值不同造成的，其他明智的浏览器还好，这个bug只出现在IE6 IE7下

问题重现：

<p>There are no scrollbars on this page in sane browsers</p>
html, body, p {
    margin: 0;
    padding: 0;
}

body {
    overflow: hidden;
}

p {
    width: 5000px;
    height: 5000px;
}

IE6 IE7下不生效（IE6下横向纵向滚动条都在 IE7下纵向滚动条还在）

原因：

明智的浏览器(ex. chrome and firefox)会初始付值给html{overflow:visible;}

IE6 初始付值html{overflow-x:auto;overflow-y:scroll;}

IE7 初始付值html{overflow-x:visible;overflow-y:scroll;}

只有dom根结点（也就是html根节点）设置html{overflow:visible;}的时候，浏览器才会将body元素中的overflow值应用到视图区。

举个例子说：

设置了body{overflow:hidden}，还会出现滚动条，不过这个滚动条不是body的，是html的
只有你设置html{overflow:visible;} body{overflow的值}才能传递到html{}中去
这样html的值就变成了{overflow:hidden}，ok没有滚动条了

这样就很明了啦，并不是bug，而是浏览器初始值不同产生的问题。

解决办法：

html, body, p {
    margin: 0;
    padding: 0;
}

html {
    overflow: visible;
}

body {
    overflow: hidden;
}

p {
    width: 5000px;
    height: 5000px;
}

此项可以整理到自己的reset文件中去