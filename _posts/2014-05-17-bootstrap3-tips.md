---
layout: post
title: Bootstrap 3使用技巧指南
categories:
- Life
tags:
- css
- 翻译
---

> Bootstrap是一个能快速提高前端开发效率到前端开发框架。在web开发中，我们会碰到经常写一些重复的css和javascript的代码，如果我们使用bootstrap来作为开发的基础框架，那可以为我们节省大量的开发时间，可以把时间真正花费在核心的事情上。这篇文章我们就来学习下Bootstrap最新版本Bootstrap 3的一些使用技巧。原文地址[Bootstrap 3 Tips and Tricks You Might Not Know](http://scotch.io/bar-talk/bootstrap-3-tips-and-tricks-you-might-not-know)，具体翻译有删减，主要是一些寒暄之类的言语没有翻译。

### 1、如何开启Bootstrap 3中鼠标滑过显示下拉菜单的功能

在Bootstrap中，导航菜单这一块还有很多可以完善的地方。当然在一般的开发中，它还是足够好用的。不过当我们需要一个鼠标滑过触发下拉菜单当需求的时候，就需要我们来自定义来，因为bootstrap的下拉菜单是不支持鼠标滑过来触发的。

在一些项目中，可能需要支持鼠标滑过触发下拉菜单的需求。在Bootstrap3中是不支持的只支持鼠标点击来触发下拉菜单，我们来看看默认bootstrap的下拉菜单：

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/BGG4d/1/embedded/result,js,html,css/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

可以发现默认只支持鼠标的点击事件。

要它支持鼠标滑过事件也很简单，只需要添加下面的代码就可以了：

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/Sv9hf/1/embedded/result,js,html,css/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

这样这个下拉菜单就支持鼠标滑过事件了。那我们可能需要当用户点击菜单当时候，跳转到对应到链接地址比如我们这里的地址是**reddit.com**，我们也只需要添加几行代码就可以做到：

	$('.dropdown-toggle').click(function() {
    var location = $(this).attr('href');
    window.location.href = location;
    return false;
	});
	
现在经过调整后，我们的下拉菜单最终效果如下所示：

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/48AAN/embedded/result,js,html,css/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

在常见的开发中，默认的下拉菜单足够应付大部分的应用场景。不过，最近一段时间[侧拉滑动菜单](http://scotch.io/tutorials/off-canvas-menus-with-css3-transitions-and-transforms)也应用的很广泛，非常建议你去尝试一下。

### 2、在使用流体布局的时候，不要忘了使用 container－fluid这个类

在刚开始使用bootstrap3的时候，当我使用流体布局(即100%)的时候我从来不会使用**container－fluid**这个类。这样有一个小问题，因为bootstrap中**row**这个类左右都有一个－15px外边距，具体表现可以看看下面都示例，当然你也可以去[官方文档](http://codepen.io/ncerminara/full/omChv/)看详细的介绍。

<p data-height="268" data-theme-id="0" data-slug-hash="omChv" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/ncerminara/pen/omChv/'>Bootstrap 3 Container Examples and Common Misuse</a> by Nicholas Cerminara (<a href='http://codepen.io/ncerminara'>@ncerminara</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

### 3、媒体查询断点设置

bootstrap为我们提供了一些常见设备的媒体查询的代码：

	/*==================================================
	=            Bootstrap 3 Media Queries             =
	==================================================*/

	/*==========  Mobile First Method  ==========*/

	/* Custom, iPhone Retina */ 
	@media only screen and (min-width : 320px) {
    
	}

	/* Extra Small Devices, Phones */ 
	@media only screen and (min-width : 480px) {

	}

	/* Small Devices, Tablets */
	@media only screen and (min-width : 768px) {

	}

	/* Medium Devices, Desktops */
	@media only screen and (min-width : 992px) {

	}

	/* Large Devices, Wide Screens */
	@media only screen and (min-width : 1200px) {

	}


	/*==========  Non-Mobile First Method  ==========*/

	/* Large Devices, Wide Screens */
	@media only screen and (max-width : 1200px) {

	}

	/* Medium Devices, Desktops */
	@media only screen and (max-width : 992px) {

	}

	/* Small Devices, Tablets */
	@media only screen and (max-width : 768px) {

	}

	/* Extra Small Devices, Phones */ 
	@media only screen and (max-width : 480px) {

	}

	/* Custom, iPhone Retina */ 
	@media only screen and (max-width : 320px) {
    
	}
	
你也可以去[这里](http://scotch.io/quick-tips)看看一些有用的技巧，或者是这里也可以看看[关于媒体查询断点设置](http://scotch.io/quick-tips/css3-quick-tips/default-sizes-for-twitter-bootstraps-media-queries)的详细介绍。如果你使用像**sass**或者是**less**一类的css编译语言的话，那就可以使用像**small**、**medium**、**large**等变量来指代具体的断点非常方便。

### 4、面向多设备的网格系统

bootstrap提供的网格系统非常棒，它能够适应不同的设备自动调整布局。如果你开发的项目是面向不同终端设备的话，那使用bootstrap的网格系统能达到事半功倍的效果。比如：

	<div class="container-fluid">
    <div class="row">
        <div class="col-md-3">
            <i class="fa fa-heart"></i>
        </div>
        <div class="col-md-3">
            <i class="fa fa-heart"></i>
        </div>
        <div class="col-md-3">
            <i class="fa fa-heart"></i>
        </div>
        <div class="col-md-3">
            <i class="fa fa-heart"></i>
        </div>
    </div>
	</div>
	
在桌面设备上是这样的：

![image](http://pic.yupoo.com/reicky_v/DLlp5hFm/medium.jpg)

在一些小的设备上(比如平板、智能手机或者是768px以下设备上)：

![image](http://pic.yupoo.com/reicky_v/DLlp5rzy/K1dZQ.png)

当然，你也可以使用一点点小小的技巧来改变在小设备上的布局：

	<div class="container-fluid">
    <div class="row">
        <div class="col-md-3 col-sm-6">
            <i class="fa fa-heart"></i>
        </div>
        <div class="col-md-3 col-sm-6">
            <i class="fa fa-heart"></i>
        </div>
                <div class="col-md-3 col-sm-6">
            <i class="fa fa-heart"></i>
        </div>
        <div class="col-md-3 col-sm-6">
            <i class="fa fa-heart"></i>
        </div>
    </div>
	</div>
	
这样，在桌面设备上仍然不会产生变化。但是在小设备上，布局就会发生变化如下图所示：

![image](http://pic.yupoo.com/reicky_v/DLlp3Lnb/11Fxha.png)

### 5、嵌套列

为了使用内置的栅格将内容嵌套，通过添加一个新的.row和一系列.col-md-*列到已经存在的.col-md-*列内即可实现。嵌套row所包含的列加起来应该等于12。比如下面的代码就使用来嵌套列：

	<div class="container-fluid">
     <div class="row">
         <div class="col-md-6">
             <div class="row">
                 <div class="col-md-8">
                     <div class="row">
                         <div class="col-sm-3">
                             <span>1</span>
                         </div>
                         <div class="col-sm-3">
                             <span>2</Span>
                         </div>
                         <div class="col-sm-3">
                             <span>3</span>
                         </div>
                     </div>
                 </div>
                 <div class="col-md-4">
                     <div class="row">
                         <div class="col-md-8">
                             <span>1</span>
                         </div>
                         <div class="col-md-4">
                             <span>2</span>
                         </div>
                     </div>
                 </div>
             </div>
         </div>
         <div class="col-md-6">
             <div class="row">
                 <div class="col-xs-2">
                     <span>1</span>
                 </div>
                 <div class="col-xs-2">
                     <span>2</span>
                 </div>
                 <div class="col-xs-2">
                     <span>3</span>
                 </div>
                 <div class="col-xs-2">
                     <span>4</span>
                 </div>
                 <div class="col-xs-2">
                     <span>5</span>
                 </div>
                 <div class="col-xs-2">
                     <span>6</span>
                 </div>
             </div>
         </div>
     </div>
 	</div>
 	
详细的信息可以去官方的[文档地址](http://getbootstrap.com/css/#grid-nesting)看看。

### 6、列排序

通过使用.col-md-push-* 和 .col-md-pull-*就可以很容易的改变列的顺序。而不再需要我们来使用浮动来改变列的排序。

	<!--
    Desktop:
        [1 2 3] [1 2 3 4 5 6 7 8 9]

    Mobile:
        [1 2 3]
        [1 2 3 4 5 6 7 8 9]
	-->
 	<div class="row">
     <div class="col-md-9 col-md-push-3">1 2 3 4 5 6 7 8 9 </div>
     <div class="col-md-3 col-md-pull-9">1 2 3 </div>
 	</div>

 	<!--
    Desktop:
        [ 2nd col ] [ 1st col ]

    Mobile:
        [ 2nd col ]
        [ 1st col ]
	-->
 	<div class="row">
     <div class="col-md-6 col-md-push-6">1st col</div>
     <div class="col-md-6 col-md-pull-6">2nd col</div>
 	</div>
 	
### 7、Lead类的使用

有时候，我们需要突出显示某一段落。通过添加.lead可以让段落突出显示。如下所示：

	<p class="lead">This is a lead paragraph. Lead paragraphs can really tie content of a page together! It attracts your eye!</p>
	<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed lobortis est eget tristique vestibulum. Fusce et pulvinar erat, id viverra sem. Vivamus porttitor, sapien nec mattis fermentum, sem augue ornare erat, vitae interdum ante sapien id libero. Vestibulum luctus augue pretium purus posuere.</p>                 
	<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed lobortis est eget tristique vestibulum. Fusce et pulvinar erat, id viverra sem. Vivamus porttitor, sapien nec mattis fermentum, sem augue ornare erat, vitae interdum ante sapien id libero. Vestibulum luctus augue pretium purus posuere.</p>                 
	<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed lobortis est eget tristique vestibulum. Fusce et pulvinar erat, id viverra sem. Vivamus porttitor, sapien nec mattis fermentum, sem augue ornare erat, vitae interdum ante sapien id libero. Vestibulum luctus augue pretium purus posuere.</p>
	
![image](http://pic.yupoo.com/reicky_v/DLlw971X/medium.jpg)

### 8、引用样式

对于**blockquote**标签，bootstrap有一个默认的样式设置。如下所示：

	<blockquote>
    <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Integer posuere erat a ante.</p>
	</blockquote>

	<blockquote class="blockquote-reverse">
    <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Integer posuere erat a ante.</p>
	</blockquote>
	
![image](http://pic.yupoo.com/reicky_v/DLlzFUE0/medium.jpg)

### 9、列表

如果你是一个前端开发的老手，你可能会使用**display:inline－block**这样的样式来使列表呈水平方向来排列。这样你就不需要使用**float**来布局，如果要使文字居中的话，使用**text－align:center**就可以了。

如果你再列表上添加一个**list-inline**的类，那么**li**标签默认的padding就会被删除同时**li**元素会以**inline－block**的形式来呈现，并且默认的**list－style**的样式也会被清除掉。如下图所示：

![image](http://pic.yupoo.com/reicky_v/DLm1MsA3/medium.jpg)

### 10、label

在表单中，我们经常会给属性为text的input元素设定一个placeholder的占位文本，不过更好的一个开发实践是能为input元素指定一个隐藏的label标签，这样屏幕阅读器也能识别input元素的内容。如下所示：

	<label for="id-of-input" class="sr-only">My Input</label>
	<input type="text" name="myinput" placeholder="My Input" id="id-of-input">
	
运行结果如下图所示：

![image](http://pic.yupoo.com/reicky_v/DLmCJ5x4/medium.jpg)

### 11、响应式图片

如果你正在开发一个响应式网站，那么对于图片的话，你只需要把图片的宽度设置为100%，高度设置为自动就行了。图片会自动根据设备的尺寸来调整自己的大小。而在bootstrap中只需要加上**img-responsive**这个类就行类。

### 12、自定义图片形状的类

bootstrap提供了三个来定义图片形状的类，如**img-rounded**、**img-circle**以及**img-thumbnail**。非常方便，如下图所示：

![image](http://pic.yupoo.com/reicky_v/DLmFK7TM/medium.jpg)

![image](http://pic.yupoo.com/reicky_v/DLmFItfy/medium.jpg)

### 总结

现实情况中，有很多的开发者很排斥使用现成的开发框架。这个也非常容易理解，不过bootstrap3确实是一个非常好用的开发框架，不但能提高你的开发效率，也能使你的代码更加简洁。

给自己一个选择的机会。bootstrap是一个非常强大的开发框架，绝对是居家开发必备良品。






