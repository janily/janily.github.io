---
layout: post
title: 基于手势应用的侧滑导航
categories:
- Life
tags:
- javascript
- 翻译
---

一直以来，在各种移动设备上创建一个有好的体验的导航菜单还是比较麻烦，当然是相对于PC上而言。在接下来的教程里，我们将结合手势应用来提供一个解决方案，具体是向右滑动的时候划出菜单；向左滑动的时候，隐藏菜单。具体用的技术是jquery和css3。

![](http://pic.yupoo.com/reicky_v/D6yd9V95/medium.jpg)

# 解决方案 #

虽然移动设备的屏幕比较小，但是也有几个导航选项。在PC上，仅仅把导航项目罗列出来就可以了。当然除了一些比较复杂导航系统。

![](http://pic.yupoo.com/reicky_v/D6yeK41z/medium.jpg)

像下面图中所示，在移动设备上把导航菜单做成一种下拉的形式，越来越流行了。

![](http://pic.yupoo.com/reicky_v/D6yeK5F9/medium.jpg)

这样的表现形式，当然有它的好处。

- 能够大大地减少导航占用屏幕的空间以及简化导航。
- 这种形式的导航支持能够解决大部分导航的问题
- 需要几显示（当用户需要导航时才显示，这样可以有效地利用屏幕空间，特别是对于移动设备来说，尤其重要。）
- 目前看来这样的方案不失为一个非常棒的解决方案

# 编码实现 #

首先来看看我们的结构，非常简洁，用JQUERY很容易就能实现侧滑的效果。

    
	<div id="main-container" class="tk-chaparral-pro">
	<div id="sub-container">
		<div class="top-bar">
			
			<div class="menu-icon">
				<div class="bar"> </div>
				<div class="bar"> </div>
				<div class="bar"> </div>
			</div>
			
			<div class="travel">
				<a href="http://www.inserthtml.com/2013/05/mobile-menu/">&laquo; Back to the Article</a>
			</div>
			
		</div>
		<div id="content">
			...
		</div>
	</div>	
	</div>
	
	<div class="slide-in">
		<ul class="tk-museo-sans">
			<li>HOME</li>
			<li>FORUM</li>
			<li>TUTORIALS</li>
			<li>RESOURCES</li>
		</ul>
	</div>

类名为slide-in的div就是我们要控制滑动的导航菜单。当用户点击按钮的时候，滑出菜单；第二次点击的时候，隐藏菜单。我们可以用一些手势来做这个滑入滑出的效果，用jQuery很容易做到。例如向左向右滑动的手势。

先写一些样式，一些需要的注意的我已经注释了。

    body {
    margin: 0;
    font-size: 62.5%;
    padding: 0;
	}
	 
	.top-bar {
	    background-color: #f46149;
	    padding: 10px;
	}
	 
	#main-container {
	    width: 100%;
	    overflow: scroll; /* 触发滚动条 */
	    left: 0px;
	    -webkit-overflow-scrolling: touch; /* 设置触摸也可以滚动 */
	    position: fixed; 
	    -webkit-transition: left 0.2s ease-in;
	    transition: left 0.2s ease-in;
	}
	 
	#sub-container {
	    position: relative;
	}
	 
	#content {
	    font-size: 1.5em;
	    box-sizing: border-box;
	    text-align: justify;    
	    box-sizing: border-box;
	    -moz-box-sizing: border-box;
	}
	 
	.menu-icon {
	    padding: 5px 7px 9px 5px;
	    border-radius: 5px;
	    background: rgba(0,0,0,0.2);
	    cursor: pointer;
	    display: inline-block;
	    width: 40px;
	    height: 30px;
	    float: left;
	}
	 
	.menu-icon .bar {
	    background: white;
	    border-radius: 5px;
	    width: 40px;
	    height: 5px;
	    margin: 5px 0 0 0;
	}
	 
	.slide-in {
    background-color: #2d4b5a;
    width: 270px;
    position: absolute;
    box-shadow: inset -20px 0 30px rgba(0,0,0,0.2);
    top: 0;
    left: -270px;
    -webkit-transition: left 0.2s ease-in;
    transition: left 0.2s ease-in;
	}
	 
	.slide-in ul {
	    list-style: none;
	    padding: 0;
	    margin: 0;
	}
	 
	.slide-in ul li {
	    padding: 10px;
	    font-weight: bold;
	    font-size: 2em;
	    border-bottom: 2px solid #3d6071;
	    color: #c4d6e0;
	}
	 
	.slide-in ul li:hover {
	    background-color: #3d6071;
	    color: #fff;
	}
	 
	.slide-in.on {
	    left: 0px !important;
	}
	 
	#main-container.on {
	    left: 270px !important;
	}

用jQuery我们能够控制一些基本的触摸事件。当然如果用户的屏幕不支持触摸呢？不用担心，我们有备用计划来解决这个情况。首先我们得定义下滑动菜单的高度自适应用户的设备屏幕的大小。还有，我们得考虑下，当用户改变浏览窗口大小的时候，滑动菜单也要自适应。这个就得用下javascript来解决了。

    $(document).ready(function(e) {

    function sideBarHeight() { 
     
        var docHeight = $(document).height();
        var winHeight = $(window).height();
         
        $('.slide-in').height(winHeight);
        $('#main-container').height(winHeight);
        $('#sub-container').height($('#sub-container').height());
    } 
     
    // 当页面载入或者是改变大小的时候运行sideBarHeight()函数
    $(window).resize(function() {
        sideBarHeight();
    }); sideBarHeight();

我们需要用到Hammer这个库来制作我们的手势应用。它把一些常见的手势封装起来，用起来非常方便。先在html文件里引入它，我们将用到向左滑动和向右滑动两个手势即swipeleft和swiperight。当触发手势的时候，我们添加一些类来响应手势。

     var outIn = 'in';
 
	Hammer(document.getElementById('main-container')).on('swiperight', function(e) {
	        $('.slide-in').toggleClass('on');       
	        $('#main-container').toggleClass('on');
	        outIn = 'out';
	         
	});
	 
	Hammer(document.getElementById('main-container')).on('swipeleft', function(e) {
	        $('.slide-in').toggleClass('on');   
	        $('#main-container').toggleClass('on');
	        outIn = 'in';
	});
	     
	 
	function runAnimation() {
	 
	    if(outIn == 'out') {
	         
	        $('.slide-in').toggleClass('on');
	        $('#main-container').toggleClass('on'); 
	        outIn = 'in';
	         
	    } else if(outIn == 'in') {
	     
	        $('.slide-in').toggleClass('on');   
	        $('#main-container').toggleClass('on'); 
	        outIn = 'out';
	         
	    }
	 
	}
	
我们可以利用手指在X轴的位置来检测用户移动手指的方向，从而控制菜单的滑入滑出。

接着还要给按钮添加点击事件。这就是我们上面说的对于屏幕不能触摸的情况下的备用计划。

    function runAnimation() {
 
    if(outIn == 'out') {
         
        $('.slide-in').toggleClass('on');
        $('#main-container').toggleClass('on'); 
        outIn = 'in';
         
    } else if(outIn == 'in') {
     
        $('.slide-in').toggleClass('on');   
        $('#main-container').toggleClass('on'); 
        outIn = 'out';
         
    }
 
	}
	 
	$('.menu-icon')[0].addEventListener('touchend', function(e) {
	    $('.slide-in').toggleClass('on');       
	    $('#main-container').toggleClass('on');
	});
	 
	$('.menu-icon').click(function() {
	    $('.slide-in').toggleClass('on');       
	    $('#main-container').toggleClass('on');
	});


一个简单地支持手势地侧滑菜单就完成了。在你的下一个项目中可以开箱即用。

### 注意 ###

这里用CSS3动画来制作动画效果，这样不仅让动画更自然，而且还可以节省不少性能。

最终的效果看下面，试着点击按钮或者是滑动你的鼠标看看

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/MC6vz/embedded/result,js,html,css" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

整理翻译，原文在[这里](http://www.inserthtml.com/2013/05/mobile-menu/)。

