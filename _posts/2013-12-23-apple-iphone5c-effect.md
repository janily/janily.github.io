---
layout: post
title: iPhone5C/iPhone5S页面效果制作-页面结构分析
categories:
- Life
tags:
- 前端
- 翻译
---

> 这两篇文章翻译自[ihatetomatoes](http://ihatetomatoes.net/)网站的[Apple iPhone 5C page deconstructed](http://ihatetomatoes.net/apple-iphone-5c-page-deconstructed/)以及[One Page Scroll with animations](http://ihatetomatoes.net/one-page-scroll-with-animations/)。

几个月前苹果更新了两款手机iPhone5C和iPhone5S。让我们来看看苹果宣传iPhone5C/iPhone5S的网站页面的效果[iPhone5C/iPhone5S website](http://www.apple.com/iphone-5c/)。

一如既往，苹果产品的网站的效果非常注重细节和体验，就跟它们的实际产品一样。

### 1.顶部导航效果 ###

![](http://pic.yupoo.com/reicky_v/DphwnRrS/medium.jpg)

当你滚动页面到第二屏的时候，网站的导航会从上面滑动显示并且固定在页面的顶部。这个效果非常简单，但非常优雅。用javascript和CSS3很容易就可以做到，我们首先来看看它的HTML结构。

#### HTML ####

    <div id="product-nav-slide" class="active">
    <nav id="productheader">
        ...
    </nav>
	</div>
	 
	<div id="main" class="main mbig">
	    ...
	</div>

我们会看到导航的结构里有两个于ID为productheader的**nav**元素在HTML结构里。这好像不符合规范，我们知道ID是唯一的。（译者注：的确我在苹果这个页面的源代码里确实发现了两个同名的ID的导航元素，虽然页面不会报错，不过这对于苹果这样一个有完美情节的公司来说，确实有点出人意料。）

这个**nav#productheader**用了绝对定位来把它定位在顶部(**top:-57px**)，并且当你滚动到第二屏的时候用javascript添加了一个**class="acitve"**类。具体代码如下：

#### CSS ####

	html, #main {
	    display: block;
	    position: static;
	    padding: 0;
	    width: 100%;
	    height: 100%;
	}
	#main {
	    position: relative;
	    z-index: 10;   
	}
	/* Navigation - default */
	#product-nav-slide {
	    position: absolute;
	    width: 100%;
	    z-index: 20;
	    top: -57px;
	    line-height: 0.66;
	    -webkit-transform: translateZ(0);
	    -webkit-transition: -webkit-transform 700ms;
	    -moz-transition: -moz-transform 700ms;
	    transition: transform 700ms;
	}
	/* Navigation - active */
	#product-nav-slide.active {
	    -webkit-transition: -webkit-transform 600ms;
	    -moz-transition: -moz-transform 600ms;
	    transition: transform 600ms;
	    -webkit-transition-delay: 1010ms;
	    -moz-transition-delay: 1010ms;
	    transition-delay: 1010ms;
	    -webkit-transform: translateY(57px) translateZ(0);
	    -moz-transform: translateY(57px);
	    transform: translateY(57px);
	}

当添加**active**类后，用CSS3中的transition和transform的两个属性来制作一个平滑的动画效果，即**translateY(57px)**用来定位和**transition-delay:1010ms**用来延迟动画效果的执行。

### 2.设计第二屏的动画效果 ###

![](http://pic.yupoo.com/reicky_v/DpodUjml/medium.jpg)

当你滚动到第二屏的时候，你会发现一个侧滑的动画效果。这个是怎么做的呢？

#### HTML ####

    <section id="color" class="active">
    <div class="gallery-content">
        <img class="front" src="..." width="239" height="464">
        <img class="back" src="..." width="240" height="464">
        <img class="side" src="..." width="71" height="464">
    </div>
	</section>

这个结构简洁明了。用id为**#gallery-content**的DIV把三张图片包裹起来。如果你去看苹果网站的源代码你会发现每一张图片都有一个**data-responsive**属性，这个属性主要是用来存放图片路径的一个数组，用来针对不同分辨率大小的屏幕适配不同尺寸图片的，这里为了方便分析就把这个属性删除了。

#### CSS ####

    #color .gallery img {
    float: left;
    width: 800px;
    height: 696px;
    behavior: none;
    -webkit-transform: translateZ(0);
	}
	/* animations */
	#color .gallery-content.animate img { position:relative; z-index:1;
	    -webkit-transition:-webkit-transform .6s .9s;
	    -moz-transition:   -moz-transform .6s .9s;
	    transition:        transform .6s .9s;
	}
	#color .gallery-content.animate .back { z-index:2;
	    -webkit-transform:translate3d(-40%, 0, 0);
	    -moz-transform:translate3d(-40%, 0, 0);
	    transform:translate3d(-40%, 0, 0);
	}
	#color .gallery-content.animate .front {
	    -webkit-transform:translate3d(56.6%, 0, 0);
	    -moz-transform:translate3d(56.6%, 0, 0);
	    transform:translate3d(56.6%, 0, 0);
	}
	#color .gallery-content.animate .side {
	    -webkit-transform:translate3d(-338%, 0, 0);
	    -moz-transform:translate3d(-338%, 0, 0);
	    transform:translate3d(-338%, 0, 0);
	}
	#color.active .gallery-content.animate .front,
	#color.active .gallery-content.animate .back,
	#color.active .gallery-content.animate .side {
	    -webkit-transform:translate3d(0, 0, 0) !important;
	    -moz-transform:translate3d(0, 0, 0) !important;
	    transform:translate3d(0, 0, 0) !important;
	}

默认图片是用浮动来布局的，图片之间的距离是用百分比来控制的，这样在不同分辨率大小的屏幕的时候也能正确显示。**.front**这张图片向右移动56.6%，**.side**向左移动338%以及**.back**同样向左移动40%，注意这里是用CSS3的translate3d这个属性来进行位移的。这样方便我们后面制作动画效果。同样你也可以看到**.back**这张图片的的**z-index**在这三张图片是最大的，它会显示在另外两张图片的上面。

跟上面制作那个导航效果一样，这里我们也需要在**#color container**这个里面添加一个**active**类，用这个类来控制**translate**使图片的位置还原为原来默认的位置，同时配合** transition: transform .6s 20s**，即6毫秒的动画执行时间和9毫秒的延迟执行来制作动画效果。

### 3.第三屏的动画效果 ###

![](http://pic.yupoo.com/reicky_v/DpoeePFy/medium.jpg)

第三屏的动画效果解释起来确实有点棘手，这里尽量用通俗易懂的语言来解释下这个动画效果。

#### HTML ####

    <div class="capable-bottom">
	    <img class="capable-pink" src="..." width="231" height="440">
	    <img class="capable-yellow" src="..." width="226" height="440">
	    <img class="capable-blue" src="..." width="458" height="216">
	</div>

这三张图片都是用绝地定位来定位的，并且通过**.capable-bottom**这个类来控制具体的位置属性。

    	<!-- Features View -->
		<figure id="capable-ios-actor" class="actor-slide" style="-webkit-transform: translate3d(0px, 0%, 0px);">
		    <div class="slide-content link-down link-up" style="-webkit-transform: translate3d(0, -106px, 0);">
		        <div class="capable-capable-container">
		            <img class="capable-green" src="..." width="230" height="440">
		        </div>
		    </div>
		</figure>
		 
		<!-- iOS View -->
		<figure id="capable-ios-actor" class="actor-slide" style="-webkit-transform: translate3d(0px, 100%, 0px);">
		    <div class="slide-content link-down link-up" style="-webkit-transform: translate3d(0, -325px, 0);">
		        ...
		    </div>
		</figure>

你可能会注意到，绿色的图片其实是在第四屏的结构里的(**figure#capable-ios-actor**)。主要是在滚动的时候依靠改变** #slide-content**transform的值来控制它在两屏之间的位置的。

下面就是具体的CSS代码。

    #capable .capable-bottom {
    position: absolute;
    z-index: 1;
    top: 50%;
    left: 50%;
    display: block;
    margin-top: -288px;
    margin-left: -636px;
    width: 0;
    height: 0;
	}
	#capable-ios-actor { z-index:1; }
	#capable-ios-actor .capable-green {
	    position:absolute; z-index:1;
	    top:50%; left:50%;
	    margin-top:293px; margin-left:-531px;
	}
	 
	#capable .capable-bottom img { position:absolute; top:0px; z-index:1; }
	#capable .capable-bottom .capable-pink { top:-45px; left:106px; }
	#capable .capable-bottom .capable-yellow { top:-87px; left:-222px; }
	#capable .capable-bottom .capable-blue { top:-70px; left:448px; }

从上面的CSS可以看出，这里主要是 用绝对定位来布局的并且用到了负margin值的技术。

如果你仔细观察的话，你会看到最后一屏的滚动的时候，文字有一个淡入的动画效果。这是因为在第三屏和第四屏之间的绿色的苹果的图片有一个滚动动画效果，如果第四屏的顶部的文字一开始就是显示的话，会影响到第三屏和第四屏之间的滚动的动画效果。所以对于第四屏的文字来说采取一个延迟淡入的动画效果是一个非常好的解决方法。

### 总结 ###

总之细心观察，你会发现这个页面有很多交互小细节的处理值得我们学习。

这里强烈推荐你用**firebug**或者是**chrome Inspector**这类的开发工具来查看iphone的这个新产品的页面，研究一下它是怎么运用javascript和CSS来制作这个滚动动画效果的。

未完待续...







