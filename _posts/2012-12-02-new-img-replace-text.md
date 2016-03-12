---
layout: post
title: 以图换字新方法
categories:
- Life
tags:
- css
- web前端
- 翻译
---

在前端制作中有时候会碰到一些特殊字体效果，由于用常规的字体无法实现，一般就直接用图片代替，就是所谓的以图换字。现在最流行的用法就是-9999px文字缩进法，如以下代码：
<pre><code>
  element 
{  
    background: url("myimage") 0 0 no-repeat;  
    text-indent: -9999px;  
}    
</code></pre>

由于文字缩进超出文档范围，所以文字就不会显示，从而达到以图换字的目的，简单高效。

现在又出现了一种新的以图换字技术，是Scott Kellum在Zeldman.com网站发现的，代码如下:

<pre><code>
#replace  
{  
    text-indent: 100%;  
    white-space: nowrap;  
    overflow: hidden;  
}  
</code></pre>

上面代码主要是百分百缩进文字，溢出隐藏不换行，以达到以图换字的目的。我还没有发现这种技术的负面作用，你发现了吗？发现了话还请指出来。

翻译过程中，有什么不妥之处，还请指正。原文在[这里](http://www.sitepoint.com/new-css-image-replacement-technique/)。

