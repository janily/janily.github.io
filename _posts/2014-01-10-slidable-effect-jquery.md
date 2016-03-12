---
layout: post
title: 用jQuery实现一个网格滑动效果
categories:
- Life
tags:
- jQuery
- 翻译
---

> 原文地址[How to create a slidable grid with jQuery](http://www.webdesignerdepot.com/2014/01/how-to-create-a-slidable-grid-with-jquery/)。

这个网格滑动效果主要是用来在有限的空间里显示更多的信息的一种交互方式。主要的交互方式是点击或者是鼠标滑过格子的时候，显示额外的信息。

在编写这个效果的过程中，我们会从HMTL结构、样式以及用**web font**字体图标这三个方面来进行打造。当然也需要辅助少量的jQuery代码。

首先来看一看这个我们将要开发的效果的实际运行效果，如下所示：

<iframe width="782" height="560" src="http://netdna.webdesignerdepot.com/uploads7/how-to-create-a-slidable-grid-with-jquery/"></iframe>

### HTML ###

这个例子的布局非常简单，接着往下看你就知道了。我们这个效果是以网格的形式来实现布局效果的，首先网格区域我们得赋予它相对定位的属性，然后我们会在单个网格的里面编写两个盒子区域，正常情况下有一个盒子是不可见的，只有当鼠标点击当前盒子区域或者是鼠标滑过盒子区域，另外一个盒子区域才会显示，而当前盒子区域则会隐藏起来。而两个盒子区域显示隐藏状态的切换就是用绝对定位改变元素的定位来实现的。说了一大堆可能有一点抽象，下面来进入编码阶段，就容易理解了。

首先是HTML代码：

    <div id="services" class="cf">
        <section class="service">
              <div class="service-icon"><span class="icon-web"></span><br/>Web Design</div>
              <div class="service-description"><p>Something descriptive about web design. And what you offer as a service.</p></div>
       </section>
       <section class="service">
              <div class="service-icon"><span class="icon-graphic"></span><br/>Graphic Design</div>
              <div class="service-description"><p> ... </p></div>
       </section>
       <section class="service">
              <div class="service-icon"><span class="icon-logo"></span><br/>Logo Design</div>
              <div class="service-description"><p> ... </p></div>
       </section>
       <section class="service">
              <div class="service-icon"><span class="icon-dev"></span><br/>Web Development</div>
              <div class="service-description"><p> ... </p></div>
       </section>
       <section class="service">
              <div class="service-icon"><span class="icon-3d"></span><br/>3D Design</div>
              <div class="service-description"><p> ... </p></div>
       </section>
       <section class="service">
              <div class="service-icon"><span class="icon-illustration"></span><br/>Illustration</div>
              <div class="service-description"><p> ... </p></div>
       </section>
	</div> <!-- END #services -->

看到了吧，这个布局非常简单。我们这里定义了六个网格区域，并且网格用ID为**services**的div包裹起来，由于我们的网格会用浮动来布局，所以这里还用了**cf**这个类来清除浮动。正如我们上面有讲到的每一个网格里面都有两个DIV，一个是包含一个用字体图标定义的图标和标题，另外一个DIV是包含了详细信息。

我们来看看没有CSS和javascript的情况下，我们布局的样子：

![](http://pic.yupoo.com/reicky_v/Ds2Es6At/medium.jpg)

接下来是定义样式以及基于jQuery来编写js来实现交互效果。

### CSS ###

我们把CSS分成三部分来做，主要是配合jQuery实现交互效果，用字体来实现图标以及美化布局。首先是第一步：

    #services .service {
       width: 33%;
       float: left;
       padding: 0.5em;
       min-height: 200px;
       overflow: hidden;
       position: relative;
       border: 1px solid #eee;
	}
	
	@media screen and (max-width: 600px) {
	       #services .service {
	              width: 50%;
	       }
	}
	
	@media screen and (max-width: 320px) {
	       #services .service {
	              width: 100%;
	       }
	}
	
	#services .service .service-icon, #services .service .service-description {
	       position: absolute;
	       width: 100%;
	       height: 100%;
	       top: 0;
	       left: 0;
	       background: #fff;
	       padding: 50px 0;
	       color: #222;
	}
	
	#services .service .service-description {
	       left: 100%;
	       background: #249EC2;
	       color: white;
	       padding: 50px;
	}
	
	#services .service .service-description:hover { cursor: pointer; }

首先我们是用百分比的形式来定义网格的宽度以及网格的最小高度，这样网格的布局就更加容易控制。并且是用浮动来实现网格布局的，下面是实现效果核心的样式，及把**overflow**的值设置为**hidden**，这样设置后溢出的内容会隐藏起来。并且我们用到了媒体查询针对不同分辨率的屏幕做了一个响应式的处理，即在主流的桌面电脑上是显示三个网格的；而在小于600px的屏幕上一行则显示两个网格；在小于320px的屏幕上则显示一个网格。

接下来就是处理网格内部两个DIV盒子的显示与隐藏的样式了，这里关键的地方就是使用绝对定位来控制两个	DIV的位置从而达到显示与隐藏的目的。从上面的代码可以看出我们是通过控制元素的left的值来控制元素的显示与隐藏的。当把元素的left值设置为0的时候，则元素是显示在当前盒子区域的；当把元素的left值设置为100%的时候，元素的位置因为超出了本来的区域就会隐藏起来。明白了这个原理，我们就可以使用jQuery来改变元素的left值达到我们上面的交互效果。

### 字体图标 ###

下一步是使用**@font-face**来制作我们的小图标。我们可以在线上的一些开源的字体图标库寻找我们需要的图标，这里我使用的是[Fontastic](http://fontastic.me/)。我们可以选择我们想要的图标然后打包下载下来就可以使用了。如果你想自定义一些规则，比如类名或者是字体名，也可以做到，如下所示：

![](http://pic.yupoo.com/reicky_v/Ds2PBkGD/medium.jpg)

上面就是我自定义的类名和字体名称，定义好后会提供一个**fonts**的文件包给我们下载。把下下来的文件夹放到你的CSS文件夹里。然后在样式文件中编写代码来使用我们下载好的字体文件来定义图标，如下所示：

    @font-face {
       font-family: "slidable-grid";
       src:url("fonts/slidable-grid.eot");
       src:url("fonts/slidable-grid.eot?#iefix") format("embedded-opentype"),
              url("fonts/slidable-grid.woff") format("woff"),
              url("fonts/slidable-grid.ttf") format("truetype"),
              url("fonts/slidable-grid.svg#slidable-grid") format("svg");
       font-weight: normal;
       font-style: normal;
	}
	
	[class^="icon-"]:before,
	[class*=" icon-"]:before {
	       font-family: "slidable-grid" !important;
	       font-style: normal !important;
	       font-weight: normal !important;
	       font-variant: normal !important;
	       text-transform: none !important;
	       speak: none;
	       font-size: 4em;
	       line-height: 1;
	       -webkit-font-smoothing: antialiased;
	       -moz-osx-font-smoothing: grayscale;
	}
	
	.icon-web:before {
	       content: "a";
	}
	
	.icon-graphic:before {
	       content: "b";
	}
	
	.icon-logo:before {
	       content: "c";
	}
	
	.icon-dev:before {
	       content: "d";
	}
	
	.icon-3d:before {
	       content: "e";
	}
	
	.icon-illustration:before {
	       content: "f";
	}

### 美化布局样式 ###

最后一步是编写样式来美化我们的布局，即用样式来定义我们想要呈现的效果，如下所示：

    @import url(reset.css);

	* { -moz-box-sizing: border-box; -webkit-box-sizing: border-box; box-sizing: border-box; }
	
	.cf:before, .cf:after { content: " "; /* 1 */ display: table; /* 2 */ }
	.cf:after { clear: both; }
	.cf { *zoom: 1; }
	
	body {
	       font-family: 'Exo 2', sans-serif; /* Google Font http://google.com/fonts */
	       text-align: center;
	       color: #999;
	       background: #444;
	       -webkit-font-smoothing: antialiased;
	}
	
	#services {
	       max-width: 850px;
	       margin: 0 auto;
	}
	
	#services .service .service-icon:hover {
	       cursor: pointer;
	       color: #249EC2;
	}
	
	#services .service .service-icon span {
	       display: block;
	       -webkit-transition: all 0.1s linear;
	       -moz-transition: all 0.1s linear;
	       transition: all 0.1s linear;
	}
	
	#services .service .service-icon:hover span {
	       position: relative;
	       bottom: 5px;
	}

### jQuery代码 ###

最后是编写jQuery代码来完成交互效果。我们这里主要监听用户的点击或者是鼠标的hover事件，来改变元素的left值从而完成交互效果。代码如下所示：

    $(document).ready(function() {
       $('.service').click(function() {
              var $this = $(this);
              if ($this.hasClass("open")) {
                     $this.find('.service-icon').animate({left: "0"});
                     $this.find('.service-description').animate({left: "100%"});
                     $this.removeClass("open");
              }
              else {
                     $this.find('.service-icon').animate({left: "-100%"});
                     $this.find('.service-description').animate({left: "0"});
                     $this.addClass("open");
              }
       });
	});

在监听用户点击我们会做一个判断，判断元素是否有open这个类，其实这个类的作用就是用来实现在点击的时候循环显示隐藏的动画效果。如下图所示：

![](http://pic.yupoo.com/reicky_v/Ds2YOkLD/medium.jpg)

从上面的图中我们就会明白，点击的时候如果**service**有open这个类，我们就会把有图标的这个DIV的盒子的left值改为0，把第二个DIV盒子的left值改为100%，而且会删除open这个类，这样当我们重复点击**service**元素的时候，就会重复执行这个过程，从而实现隐藏显示的交互效果。

同样我们最终例子还添加了一个hover的效果，原理差不多这里就不再阐述了。

