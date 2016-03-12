---
layout: post
title: javascript事件委托应用
categories:
- Life
tags:
- javascript
---
最近在项目中的一个功能编写中，有用到事件委托，说实话以前还真没怎么注意到javascript中事件委托这个东东对性能还有代码的可维护性方面的提升作用很大。

这次在项目中由于要给元素添加事件，而这些元素又是动态变化的，我最开始的做法是一个个遍历子节点，为每个元素都添加监听函数。而且因为这些子节点又是动态变化的，又不得不重新为这些动态变化的元素添加监听函数。

我写了一个简单的demo，这是没有优化前的代码，可以先看看代码中的js部分中的事件的绑定

<a class="jsbin-embed" href="http://jsbin.com/upewap/8/embed?live">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

其中javascript中的事件绑定部分如下

<pre>
<code>
 obj.create.on('click',function(){
    var opt = $('#jName');
    var _html = $('&lt;li&gt;'+opt.val()+'&lt;/li&gt;');
    var _val = _html.text();
    obj.placeholder.text(_val);
    obj.dd.toggleClass('active');
    $('.dropdown').append(_html);
    _html.on('click',function(){
    var opt = $(this);
    obj.val = opt.text();
    obj.placeholder.text(obj.val);
    obj.dd.toggleClass('active');
    });
});

obj.opts.on('click',function(){
    var opt = $(this);
    obj.val = opt.text();
    obj.placeholder.text(obj.val);
    obj.dd.toggleClass('active');
});
</code>
</pre>

可以看到，我为了给li元素添加点击事件，而且这个li元素还是动态变化的，我首先遍历已有的li元素，给它们绑定点击事件，由于li元素是动态变化的，我又重新给新增的元素绑定了点击事件。

可以看到为了给li元素绑定处理事件，我连续两次重复绑定了两次事件处理函数，这还是好的，如果在一个复杂的Web应用程序中，对所有可单击的元素都采用这种形式，那么结果就会有数不清的代码用于添加事件处理程序。代码冗余不说，最重要的是影响性能和代码维护性。

在js中每个函数都是对象，都会占用内存；内存中对象越多，性能越差。必须事先指定所有事件处理程序而导致的DOM访问次数，会延迟整个页面的交互就绪事件。

解决事件处理程序过多的问题的解决方案就是事件委托。事件委托，是一种优化DOM元素事件绑定的技巧，利用事件冒泡的原理，通过绑定事件到父元素，检查event触发元素的target，最终执行相应的事件函数处理，总而言之，事件委托是一个解决内存和性能的技巧。

为了不一个个遍历子节点绑定事件处理程序，把事件处理程序只绑定在包含这些子元素的父元素上，点击子节点，由于事件冒泡，绑定在父元素上的事件处理程序被处理，所以用户看到的效果跟遍历节点绑定事件处理程序的效果一样，而事件目标还是被点击的字节点。

而且如果使用遍历字节点去绑定事件处理程序，如果删除子节点，那些绑定在子节点上的事件处理程序还是没有移除的，必须得手动移除，如：element.onclick=null，而采用事件委托也没有这烦恼。

使用事件委托，不仅能提高性能，而且加入绑定事件处理程序的父元素中指向事件源的子元素还是有之前的事件的。一般，因为遍历是在新添加元素之前完成的，所以当我们新添加的元素进去后，事件处理程序是不可能绑定进去的。

而事件委托没有这个烦恼，因为他处理程序在父元素上，所以事件委托解决了提高性能和给未来节点添加预期事件处理程序两个问题 ，可谓一箭双雕。

ok,说了一大通事件委托的原理和好处，接下来我们就用事件委托来改下上面代码中的事件处理部分。

在jquery中，从jQuery 1.7开始，.delegate()已经被.on()方法取代。但是，对于早期版本，它仍然是使用事件代理（委派）最有效的方式。事件绑定和代理（委派）的更多信息可以查看jquery中.on()方法的详细说明。在一般情况下，这两种方法是等效的。

那我们就基于jquery来改造上面代码中的事件绑定，详细的代码结构可以在demo中看到，这里就不写了。

因为要给li元素绑定事件处理，而且这个li元素还是动态变化的，我们可以考虑给li元素的父级ul绑定事件处理程序。这样就不用一个个遍历子节点绑定事件处理程序，利用事件冒泡的原理，通过绑定事件到父元素，检查event触发元素的target，最终执行相应的事件函数处理。代码量也减少了，性能也得到了提升。何乐而不为呢~~！

<pre>
<code>
 obj.create.on('click',function(){
        var _opt = $('#jName');
        var _html = $('<li>'+_opt.val()+'</li>');
        var _val = _html.text();
        obj.placeholder.text(_val);
        obj.dd.toggleClass('active');
        $('.dropdown').append(_html);
});

obj.opts.on('click','li',function(){
        var _opt = $(this);
        var _val = _opt.text();
        obj.placeholder.text(_val);
        obj.dd.toggleClass('active');       
});

</code>
</pre>

最终改完代码后demo如下

<a class="jsbin-embed" href="http://jsbin.com/upewap/7/embed?live">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>


