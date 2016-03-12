---
layout: post
title: 移动设备的手势事件介绍与运用
categories:
- Life
tags:
- javascript
- 翻译
---

上一篇文章，学习了一些关于在移动设备上手势设计的一些知识，这次来具体学习下，移动设备上的手势事件和具体的应用。

# 响应式设计 #

关于响应式设计你也许听了很多。特别是针对各种移动设备的设计更是响应式设计中重要部分。那在移动设备上怎么来提高哦你网站的用户体验呢？今天我们就来学习下移动设备上怎么来提高用户体验的方法。

# 触摸 #

关于事件，平时在web中很常见，比如点击、鼠标滑过等，以及在触摸屏上的触摸事件。那我们怎么来定义触摸事件呢。一般触摸屏都会响应以下三个触摸事件：

- TOCHSTART(触摸)
- TOUCHEND(触摸结束)
- TOUCHMOVE(触摸并移动手指)

## 三个事件所代表的具体的含义 ##

那么这三个事件具体包含了哪些事件信息呢？在应用触摸事件前，我们得检查下浏览器是否支持触摸事件：

    <script type="text/javascript">
		function thatFunction(e) {
		    document.write(&quot(开价);hello!&quot;);
		}
		 
		addEventListener('touchstart', thatFunction, false);
	</script>

这样，我们就简单写了一个触摸事件，当你触摸屏幕的时候，会执行thatFunction这个函数，打印出hello。so cool！

## 收集事件信息 ##

当然仅仅是这三个事件，并不能让我们做出丰富的交互体验，那怎么利用手机的触摸来做一些更加酷的交互体验呢？其实利用手机的手势事件来得到一些手指的触摸信息，就能让我们做到，触摸事件中还包含以下一些信息：

- TOUCHES(触摸时初始化手指的位置)
- ChangTouches(手指在屏幕上移动的位置)
- targetTouches(触摸事件的目标元素)

上面的三个事件会返回一些关于手指的位置信息，来让你操纵你想要的效果，主要是包含以下一些信息

- screenX:事件目标在屏幕中X轴的位置
- screenY:事件目标在屏幕中y轴的位置
- clientX:事件目标在当前视窗中x轴的位置，不包括滚动条的距离
- clientY:事件目标在当前视窗中y轴的位置
- pageX:事件目标在当前视窗中x轴的位置，包括滚动条的距离
- pageY:事件目标在当前视窗中y轴的位置，包括滚动条的距离
- identifier:标示符
- target:触碰的目标元素

收集这些信息


    function someFunction(e) {
 
    var touch = e.touches[0]; // 获得触摸时手指的位置
    var scrX = touch.pageX; // 获得当前视窗中x轴的位置
 
}

容易吧？需要记住一点，当用户手指移动时要用到changedTouches事件。利用刚刚写好的方法，就可以做一个简单的demo

    // Add to Javascript
	addEventListener('touchstart', someFunction, false);

仅仅知道有这么一回事就够了么？重要的是我们要利用它来在移动设备上做一些特别的事情，所以我们可以用脚本来做一些手势应用。接下来就让我们开始手势应用之旅吧。

# 手势应用之旅 #

接下来，我们写一个简单的手势应用。如果你有像智能手机、ipad等诸如此类的移动设备可以用来测试下。接下来我们将创建一个响应手指在屏幕上滑动的手势应用的div实例，一个手指触摸是,div的颜色是蓝色，两个手指触摸是紫色，三个是红色。接下来就是代码部分，可以看看注释部分就明白具体代码的意思了。

    addEventListener('touchmove', function(e) { // 添加触摸事件.
     
    e.preventDefault(); // 阻止触摸时候触发的滚动事件.
 
    var touch = e.touches[0]; // 定义触摸事件变量.
         
    var posY = touch.pageY - 25; // 获得触摸时X轴位置
    var posX = touch.pageX - 25; // 获得触摸时Y轴位置
     
    if(e.touches.length == 1) { // 如果是一个手指触摸
     
        // 在触摸的位置创建一个div
        // -----------------------------------------
         
        var blue = document.createElement('div');
        blue.setAttribute('class', 'blue');
         
        blue.style.top = posY+'px';
        blue.style.left = posX+'px';
         
        document.body.appendChild(blue); 
         
    }
     
    if(e.touches.length == 2) { //  如果是两个手指触摸
     
        var purple = document.createElement('div');
        purple.setAttribute('class', 'purple');
         
        purple.style.top = posY+'px';
        purple.style.left = posX+'px';
         
        document.body.appendChild(purple);
     
    }
     
    if(e.touches.length == 3) { // 如果是三个手指触摸
     
        var red = document.createElement('div');
        red.setAttribute('class', 'red');
         
        red.style.top = posY+'px';
        red.style.left = posX+'px';
         
        document.body.appendChild(red);
     
    }
     
	}, false);


这里要提醒一下的是，记得要在html头部加上viewport申明

    <meta name="viewport" content="initial-scale = 1.0,maximum-scale = 1.0" >

需要一些简单的css

    body {
    padding: 0;
    margin: 0;
	}
	 
	 
	.blue {
	    width: 50px;
	    height: 50px;
	    position: absolute;
	    background: blue;
	    border-radius: 200px;
	}
	 
	.purple {
	    width: 50px;
	    height: 50px;
	    position: absolute;
	    background: purple;
	    border-radius: 200px;
	}
	 
	.red {
	    width: 50px;
	    height: 50px;
	    position: absolute;
	    background: red;
	    border-radius: 200px;
	}

ok,一个简单的手势就创建完成了，可以在移动设备上测试一下，通过手势我们可以创建更加丰富的交互体验。

这篇文章是翻译的，并没有逐字逐句，语言上差不多是意译的，所以行文上跟原文有点差别，原文在[这里](http://www.inserthtml.com/2012/02/creating-interactive-mobile-experience/)。