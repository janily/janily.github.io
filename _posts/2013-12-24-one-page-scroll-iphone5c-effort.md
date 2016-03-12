---
layout: post
title: iPhone5C/iPhone5S页面效果制作-效果制作
categories:
- Life
tags:
- 前端
- 翻译
---

上篇文章分析了苹果iPhone5S/5C页面的动画效果的实现原理，正好几周前[Pete R](http://www.thepetedesign.com/)介绍了它根据苹果这个页面的效果写了一个插件[One Page Scroll](http://www.thepetedesign.com/demos/onepage_scroll_demo.html)。

这篇文章就根据我们上篇文章分析的原理结合这个插件来实现以下苹果iPhone5S/5C页面的动画效果。

[demo](http://ihatetomatoes.net/demos/one-page-scroll-animations/)

### 页面加载动画 ###

虽然苹果的页面没有页面加载的这个动画效果，但是我们可以在自己的项目中使用javascript和CSS3来制作加载动画效果，对于提升用户体验还是有很大帮助的。

### HTML ###

    <body class="loading">
    <section>
        ...
    </section>
	</body>

### CSS ###

    .loading {
    background: url('../img/ico_loading.gif') no-repeat center center;
	}
	section {
	    opacity: 0;
	    -webkit-transition:opacity .6s;
	    -moz-transition:opacity .6s;
	    -o-transition:opacity .6s;
	    transition: opacity .6s;
	}
	.loaded section {
	    opacity: 1;
	}

### jQuery ###

    $(window).load(function() {
    // start up after 2sec no matter what
    window.setTimeout(function(){
        $('body').removeClass("loading").addClass('loaded');
    }, 2000);
	});

上面的代码很容易理解，我们通过控制body上的类延迟两秒钟来显示页面。

### 注意 ###

如果你打算把上面的代码应用到你的项目中，你需要注意添加一个针对没有加载js的情况下的回退方案。使用**no-js**这个类来针对没有加载js的情况下编写样式。

### 第二屏的动画效果 ###

![](http://pic.yupoo.com/reicky_v/DpodUjml/medium.jpg)

第二屏的动画效果非常不错，深得我心。我看到很多的网站有用到这样的效果。效果实现起来也非常简单，需要把图片用**translate**属性把图片的位置定好，触发动画效果的时候，只需要**translate3d**的值改为0就可以了。

    #revealAnim .images-container img {
    -webkit-transition: -webkit-transform .6s .9s;
    -moz-transition: -moz-transform .6s .9s;
    transition: transform .6s .9s;
	}
	#revealAnim .images-container .back { z-index:2;
	    -webkit-transform:translate3d(-40%, 0, 0);
	       -moz-transform:translate3d(-40%, 0, 0);
	            transform:translate3d(-40%, 0, 0);
	}
	#revealAnim .images-container .front {
	    -webkit-transform:translate3d(61.6%, 0, 0);
	       -moz-transform:translate3d(61.6%, 0, 0);
	            transform:translate3d(61.6%, 0, 0);
	}
	#revealAnim .images-container .side {
	    -webkit-transform:translate3d(-338%, 0, 0);
	       -moz-transform:translate3d(-338%, 0, 0);
	            transform:translate3d(-338%, 0, 0);
	}
	.viewing-page-2 #revealAnim .images-container img {
	    -webkit-transform: translate3d(0, 0, 0) !important;
	    -moz-transform: translate3d(0, 0, 0) !important;
	    transform: translate3d(0, 0, 0) !important;
	}

正如你在上面的代码中看到的一样，我们这里用了CSS3的transition的属性来制作一个平滑过渡的动画效果，而且延迟9毫秒来执行这个运动效果。

### 第三屏的动画效果 ###

![](http://pic.yupoo.com/reicky_v/DpoeePFy/medium.jpg)

第三屏的滚动动画效果是将一个元素从另一个区域移动到当前所在的区域，这样的动画效果非常棒。

在上篇文章就这个也分析了下实现原理，这里就不再阐述了。这个元素的移动主要是把第四屏里的绿色的苹果手机的移动到第三屏里面来。接着往下看。

### 第四屏动画效果 ###

![](http://pic.yupoo.com/reicky_v/DppZ1hgt/medium.jpg)

### HTML ###

    <section id="betweenSlidesAnimEnd">
    <div class="fake-content">
        <div class="images-container">
            <img class="green" src="img/img_green.png" />
        </div>
    </div>
    <div class="section-content">
        <div class="copy-container">
            ...
    </div>
    <div class="images-container">
        <img class="front2" src="img/img_iphone.png" />
    </div>
    </div>
	</section>

除了绿色的苹果手机的图片外，其它两张图片是放在**.images-container**这个区域，而绿色苹果手机是放在**.fake-content**这个区域里。因为绿色苹果手机这张图片是需要运动的，所以我们把它单独放在一个区域里。

One Page Scroll这个插件是通过添加类来判断浏览器当前所在的区域。我们使用**.view-page-3**这个类来改变**.fake-content**的位置把它移动到第三屏的合适的位置。如下：

    .viewing-page-3 .fake-content {
    -webkit-transform: translate3d(0, -100%, 0) !important;
    -moz-transform: translate3d(0, -100%, 0) !important;
    transform: translate3d(0, -100%, 0) !important;
	}
	.viewing-page-3 .fake-content .green {
	    -webkit-transform: translate3d(0, 160px, 0) !important;
	    -moz-transform: translate3d(0, 160px, 0) !important;
	    transform: translate3d(0, 160px, 0) !important;
	}

上面的代码就是定义在滚动到第三屏的时候，把绿色苹果手机这张图片移动对应的位置，当滚动到第四屏的时候，把它的位置恢复到默认的位置。

    .viewing-page-4 .fake-content,
	.viewing-page-4 .fake-content .green {
	    -webkit-transform: translate3d(0, 0, 0) !important;
	    -moz-transform: translate3d(0, 0, 0) !important;
	    transform: translate3d(0, 0, 0) !important;
	}

这里我们用**translate3d**这个属性把图片的位置改为默认的位置。

[demo](http://ihatetomatoes.net/demos/one-page-scroll-animations/)和[源代码下载](http://ihatetomatoes.net/demos/one-page-scroll-animations.zip)

### 总结 ###

这篇文章结合上篇文章，我们就很轻松的实现了苹果手机产品页面的效果。主要是用到了CSS3中的**transition**和**translate**这两个属性配合javascript来实现的。

当然，运用我们无穷的想象力我们还可以做的更多，做出更多的优雅的动画效果。特别是CSS3的出现，那酸爽谁用谁知道。




