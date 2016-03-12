---
layout: post
title: jquery插件实战-弹出框（2）
categories:
- Life
tags:
- jquery
---

上篇文章中大致把弹出框的核心功能梳理并用代码实现出来了，现在就该轮到怎么把它封装为一个jquery插件，废话不多说...开动吧！

开动前，可以先看看[demo](http://jsfiddle.net/janily/64Gef/)。

开动前，我们需要先把jquery插件的一个大体的代码框架先搭起来，这样后面的工作就像盖房子一样，地基搭好了，添砖加瓦慢慢把房子盖起来。

jquery的插件一般都有一个基础模板，如下:
<pre>
<code>
(function($){
    $.fn.插件名= function(settings){
                //默认参数
        var defaultSettings = {
 
        }
       
        /* 合并默认参数和用户自定义参数 */
        settings = $.extend(defaultSettings,settings);
       
        return this.each(function(){
                      //代码
        });
       
    }
   
})(jQuery);
</code>
</pre>
至于模板的一些详细说明,[这里有详细的说明](http://www.36ria.com/2768)。

###准备工作

在写功能前，我们还需要准备好一些基础性的样式，为后面的功能做准备，代码如下:

css
<pre>
<code>
#lean_overlay {
    position:fixed;top:0;left:0;height:100%;width:100%;background:#000\9;filter:alpha(opacity=40)\9;background:rgba(0,0,0,0.6);z-index:1001;
    display: none;
    _position:absolute;
    _left:expression(eval(document.documentElement.scrollLeft));top:expression(eval(document.documentElement.scrollTop));
}
*html,*html body{background-image:url(about:blank);background-attachment:fixed;}  
#test {
    width: 600px;
    padding: 30px; 
    display:none;
    background: #FFF;
    border: 1px solid #333;
    border-radius: 5px; -moz-border-radius: 5px; -webkit-border-radius: 5px;
}
</code>
</pre>

html
<pre>
<code>
&lt;a id="go" rel="Modal" name="test" href="#test"&gt;Basic&lt;/a&gt;
&lt;div id="test" &gt;
	&lt;p&gt;积土成山，风雨兴焉；积水成渊，蛟龙生焉；积善成德，而神明自得，圣心备焉。故不积跬步，无以至千里；不积小流，无以成江海。骐骥一跃，不能十步；驽马十驾，功在不舍。锲而舍之，朽木不折；锲而不舍，金石可镂。蚓无爪牙之利，筋骨之强，上食埃土，下饮黄泉，用心一也。蟹六跪而二螯，非蛇鳝之穴无可寄托者，用心躁也。&lt;/p&gt;	
&lt;/div&gt;
</code>
</pre>

上面的一个是为半透明遮罩层准备的样式，用到fixed这一属性，这样就可以在有滚动的时候，可以让半透明背景遮盖当前用户的可是区域，而对于ie6着用到了expression表达式来修复，这样考虑是弹出框只是网站一个辅助性的功能，并不是一直作为网站表现层出现在用户面前的，所以就用到了表达式来解决ie6.还有一个是弹出框的样式。

###1、设置参数

<pre>
<code>
var var defaults = {overlay: 0.5,closeButton: null};
    var overlay = $("<div id='lean_overlay'></div>");
    $("body").append(overlay);
    options = $.extend(defaults, options);
</code>
</pre>

我们这个提供了一两个参数，用来做关闭按钮的开关。null表示不显示关闭按钮，当为&quot;.modal_close&quot;时则表示打开关闭按钮。一个是改变遮罩透明度。ie不支持，但是上面样式做了处理。以及把遮罩层添加到文档里面。

###2、弹出功能编写

<pre>
<code>
return this.each(function() {
var o = options;
var modal_id = $(this).attr("href");
var toTop = $(document).scrollTop(),
	winHeight = $(window).height(),
	winWidth = $(window).width(),
	height = $(modal_id).height(),
	width = $(modal_id).width();
$("#lean_overlay").click(function() {
    close_modal(modal_id)
});
$(o.closeButton).click(function() {
    close_modal(modal_id)
});
function close_modal(modal_id) {
    $("#lean_overlay").fadeOut(200);
    $(modal_id).css({"display": "none"})
}
$("#lean_overlay").css({"display": "block"});//触发弹出层
$("#lean_overlay").fadeTo(200, o.overlay);//弹出层
</code>
</pre>

我们首先获得一些基本参数，触发弹出框的id,滚动距离、窗口宽高以及弹出框的宽高。写了两个关闭弹出层的点击事件，就是当点击遮罩层或者是关闭按钮的时候，就关闭弹出层。

###3、弹出窗位置编写

<pre>
<code>
$(modal_id).css({
	'display':'block',
	'position':'absolute',
	'margin-left': -(winWidth > width ? parseInt(width/2) : 0) + 'px',						
	'top': (winHeight > height ? parseInt((winHeight-height)/2) + toTop : toTop)+ 'px',
	'left': (winWidth > width ? 50 : 0) + '%'
});
</code>
</pre>

这段关于位置的在上一篇文章中有详细说明。

###4、自适应弹出层位置

主要是两种情况下，需要自适应位置，改变文档窗口大小和有滚动条的时候，这里我们用到jquery中的两个事件，scroll用来处理滚动、resize用来处理文档窗口大小改变事件。如下，当这两个事件发生时，重新计算位置来定位。

总觉的我这里写的不是很好，代码有点冗余。又不知从何下手，看来js还要多多加强啊！

<pre>
<code>
$(window).resize(function() {
	var toTop = $(document).scrollTop(),
	winHeight = $(window).height(),
	winWidth = $(window).width(),
	height = $(modal_id).outerHeight(),
	width = $(modal_id).outerWidth();
 $(modal_id).css({
	'position':'absolute',
	'margin-left': -(winWidth > width ? parseInt(width/2) : 0) + 'px',						
	'top': (winHeight > height ? parseInt((winHeight-height)/2) + toTop : toTop)+ 'px',
	'left': (winWidth > width ? 50 : 0) + '%'
  });
});

$(window).scroll(function() {
	var toTop = $(document).scrollTop(),
	winHeight = $(window).height(),
	winWidth = $(window).width(),
	height = $(modal_id).height(),
	width = $(modal_id).width();
 $(modal_id).css({						
 	'position':'absolute',
	'margin-left': -(winWidth > width ? parseInt(width/2) : 0) + 'px',						
	'top': (winHeight > height ? parseInt((winHeight-height)/2) + toTop : toTop)+ 'px',
	'left': (winWidth > width ? 50 : 0) + '%'
  });
</code>
</pre>

好了，打完收工。

具体可以用$("#go").Modal();和$("a[rel*=Modal]").Modal();这两种方式来调用。

请看[弹出框的demo](http://jsfiddle.net/janily/64Gef/)。这是我第一次编写插件，肯定有很多不足之处，还请多多指正。