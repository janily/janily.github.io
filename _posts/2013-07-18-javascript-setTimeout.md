---
layout: post
title: javascript之setTimeout延时应用
categories:
- Life
tags:
- javascript
---

在网页制作里，经常会有这样的需求：鼠标滑入显示某内容，滑出后隐藏内容。

今天在项目中，就遇到了这样一个需求。就把我解决问题的经过记下来了。

首先来举个简单例子，比如导航条中子菜单的制作。

<pre>
<code>
  &lt;a href="#" class="add">aaa&lt;/a>
  &lt;ul class="addContent"&gt;
    &lt;li&gt;bb&lt;/li&gt;
    &lt;li&gt;cc&lt;/li&gt;
    &lt;li&gt;dd&lt;/li&gt;
  &lt;/ul&gt;
</code>
</pre>

现在的需求是这样的，当鼠标滑入类名为add的节点的时候，类名为.addContent的节点就出现，js代码如下

<pre>
<code>
$(function(){
  $('.add').hover(function(){
    $('.addContent').toggle();
  });
});
</code>
</pre>

初看起来，似乎没什么问题。但其实真是的需求是这样的，就是当鼠标移入从.add节点离开进入.addContent节点的时候，由于.addContent节点没在.add节点中，则就出现一种情况，就是你永远点击不到.addContent中的菜单。

可以看下demo

<a class="jsbin-embed" href="http://jsbin.com/axuxiq/4/embed?live">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

在鼠标滑到.add上时，.addContent菜单出现，而当你滑动鼠标去点击.addContent，这时因为鼠标滑出.add，mouseleave 事件被触发，.addContent 被隐藏，也就无法点击。

那有什么办法解决这个问题呢？这里，需要用到JavaScript的setTimeout定时器函数了。

我们的需求可以重新描述如下：当鼠标滑入add，并且某个时间间隔内没有滑入addContent，addContent自动消失，如果鼠标滑出.add并且在某个时间间隔内滑入.addContent，则.addContent继续存在。

下面就来看下实际的js代码

<pre>
<code>
$(function(){
    var t;
    var $addMenu = $('.add');
    var $dMenu = $('.addContent');
    $addMenu.hover(function(){
        clearTimeout(t);
        $dMenu.show();
    },
    function(){
        t = setTimeout(function () {
            $dMenu.hide();
        }, 1000);
    }
    );
    $dMenu.hover(function(){
       clearTimeout(t);
    },
    function(){
        t = setTimeout(function () {
            $dMenu.hide();
        }, 100);
    });
});
</code>
</pre>

这样就可以解决我们的问题了，demo如下

<a class="jsbin-embed" href="http://jsbin.com/axuxiq/5/embed?live">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>



