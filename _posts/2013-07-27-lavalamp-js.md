---
layout: post
title: javascript导航lavalamp效果制作
categories:
- Life
tags:
- javascript
---

今天在逛淘宝UED博客的时候，注意到了它的导航的效果做的不错，就是当导航任何一个非激活的分类被 hover 时，当前激活的分类背景就会动态地从之窜到被 hover 的分类上面，用语言难以表达清楚这个效果，看下面这张图马上就明白了，这效果确实还不错，英文中叫这种效果为lavalamp,中文不知道咋翻译好。

<img src="http://pic.yupoo.com/reicky_v/D2DozyWA/GJcIh.gif" width="297" height="362" />

看了上面这个效果后，是不是也磨刀霍霍向猪羊，也来整一个，简约美观国际化，高端大气上档次啊。今天就来整一个，巧的是网上也有这方面的教材，参考了部分教程，也整了一个，是基于jquery的，下面就开始了。原来的教程地址是[这里](http://net.tutsplus.com/tutorials/html-css-techniques/how-to-build-a-lava-lamp-style-navigation-menu/)。

###HTML

首先先把结构写好

<pre>
<code> 
...  
&lt;div id="container"&gt;
  
    &lt;ul id="nav"&gt;  
        &lt;li id="selected"&gt;&lt;a href="#"&gt;首页&lt;/a&gt;&lt;/li&gt;  
        &lt;li&gt;&lt;a href="#"&gt;关于&lt;/a&gt;&lt;/li&gt;  
        &lt;li&gt;&lt;a href="#"&gt;博客&lt;/a&gt;&lt;/li&gt; 
        &lt;li&gt;&lt;a href="#"&gt;更多&lt;/a&gt;&lt;/li&gt;  
        &lt;li&gt;&lt;a href="#"&gt;联系&lt;&gt;a&gt;&lt;/li&gt;
    &lt;/ul&gt;  
  
&lt;/div&gt;  
  
&lt;script src="http://ajax.googleapis.com/ajax/libs/jquery/1.4/jquery.min.js" type="text/javascript"&lt;&lt;/script&gt;
&lt;script src="http://ajax.googleapis.com/ajax/libs/jquery/1.4/path/to/jquery.easing.js" type="text/javascript"&lt;&lt;/script&gt;      
...    
</code>
</pre>

这里提醒下，用到jquery.easing这个插件来做相关的动画效果。

结构写完了，接下来就是js部分啦，我们会基于jquery来写一个叫做lavanav的插件。下面就是js部分

###javascript代码

写基于jquery的插件，一般都有一个插件代码结构，我们先把结构写出来

<pre>
<code>
(function($){

	$.fn.lava = function(options) { 

		options = $.extend({  
	    overlap : 20,  
	    speed : 500,  
	    reset : 1500,  
	    color : '#0b2b61',  
	    easing : 'easeOutExpo'  
		}, options); 

		return this.each(function() {  
  			
			...
			
		});        
  
	};  

})(jQuery);
</code>
</pre>

在上面的代码结构中，我们首先定义了一些默认的参数。当然我们也可以在调用插件的时候，自定义参数，如

<pre>
<code>
	$('#nav').lava({  
	   speed : 2000,  
	   easing : 'easeOutElastic'    
	});  
</code>
</pre>

这两个参数就会覆盖我们在插件里面定义好对应参数的默认值。

接下来，我们来继续完善插件具体的功能函数方法。

<pre>
<code>
return this.each(function() {  		
	var nav = $(this),  
    currentPageItem = $('#selected', nav),  
    blob,  
    reset;  		
});   
</code>
</pre>

上面的代码中，会循环遍历我们我们传入到插件中元素，我们还在顶部声明初始化了一些变量，后面会用到。

nav变量是指向我们传入dom的一个jquery对象。

currentPageItem是我们指定的默认被选中的导航栏目，通过第二个参数来指定当前的执行环境。

blob变量是用于鼠标滑过时高亮用的。

reset变量是用定时器的。下面会详细用到这几个变量。

下面我们先来定义blob变量

<pre>
<code>
$('&lt;li id="blob"&gt;&lt;/li&gt;').css({  
    width : currentPageItem.outerWidth(),  
    height : currentPageItem.outerHeight() + options.overlap(重叠),  
    left : currentPageItem.position().left,  
    top : currentPageItem.position().top - options.overlap / 2,  
    backgroundColor : options.color  
}).appendTo(this);
</code>
</pre>

我们创建了一个id为blob的li元素，并且用javascript来计算样式，因为这些css属性是动态变化的，所以只能用javascript来检查相关的css属性的值。

width就是当前元素的宽度是包含padding和borer的。

height就是当前元素的高度度是包含padding和borer的。

left是指当前鼠标经过元素相对于currentPageItem这个元素的值。

top是指blob变量的top值。

backgroundColor是设置blob变量的背景颜色。

现在该初始化blob变量了

<pre>
<code>
blob = $('#blob', nav);  
</code>
</pre>

###鼠标滑动处理

现在来编写鼠标滑过元素时的事件(包括滑入和滑出)

<pre>
<code>
$('li:not(#blob)', nav).hover(function() {  
    // mouse over  
    clearTimeout(reset);  
    blob.animate(  
        {  
            left : $(this).position().left,  
            width : $(this).width()  
        },  
        {  
            duration : options.speed,  
            easing : options.easing,  
            queue : false  
        }  
    );  
}, function() {  
    // mouse out      
    reset = setTimeout(function() {  
        blob.animate({  
            width : currentPageItem.outerWidth(),  
            left : currentPageItem.position().left  
        }, options.speed)  
    }, options.reset);  
      
});  
</code>
</pre>

首先选择id不是blob的li元素，然后添加hover事件包括鼠标滑入滑出。

接着就为blob编写动画处理方法，首先是鼠标滑入的方法，鼠标滑入的时候先计算blob元素的left值和宽度，还要设置下动画的事件和运动方式。这样blob元素就能相应的滑动到当前鼠标滑入的元素上面。

鼠标滑出的时候，设定了一个名为reset的定时器，当鼠标滑出的时候，blob元素会回到我们设定的第一个元素。

至此一个基本的lavalamp效果的插件就写完了，一些样式什么的就不搬上来了。

我们可以这样来调用我们的插件

<pre>
<code>
$('#nav').lava({  
	   speed : 2000,  
	   easing : 'easeOutElastic'    
}); 
</code>
</pre>

下面就看下做成以后的效果了，如下图，推荐下LICEcap这个录制屏幕的软件，我就是用这个录制效果的，它会自动保存为gif图，非常不错。


<img src="http://pic.yupoo.com/reicky_v/D2DZ8nv5/medium.jpg" width="297" height="362" />



