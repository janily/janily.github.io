---
layout: post
title: 响应式开发之视网膜图片方案
categories:
- Life
tags:
- 响应式
- css
---

随着各种高清视网膜屏幕的出现，现在web设计也需要考虑各种高清屏幕的显示效果，同样前端在代码实现的时候也需要根据屏幕的不同来输出不同分辨率的图片。前面我翻译过一篇响应式图片的文章[这里](http://janily.gitcafe.com/life/2013/09/17/responsive-image/)可以去看看。今天我们再来看看另外一种关于响应式图片的解决方案。

一般为了使我们的网页能够适配视网膜屏幕上的高分辨率，一般需要准备两套不同尺寸的图片来应对，一套在普通屏幕上显示；一套在高清屏幕上显示。可以去[这个地址](http://en.wikipedia.org/wiki/List_of_displays_by_pixel_density)看看现在市面上主流设备的屏幕分辨率。

当然w3c标准也意识到了这个问题，也推出了一个[HTML的解决方案](http://www.w3.org/TR/html-picture-element)，不过浏览器对它的支持还很有限。在浏览器全面实现这个方案之前，我们不得不借助于其它的方法来实现响应式图片，如CSS3的媒体查询等。不过使用javascript也有一个很简单的方案。如下：

    <img src="myimage.jpg" data-2x="myimage@2x.jpg">
	<img src="photo_800.png" data-2x="photo_1600.png">

上面的HTML的img标签，我添加了一个**data-2x**的属性，这个属性就是用来引用为高清屏准备的图片的，当然得借助javascript来实现：

    if(window.devicePixelRatio >= 1.2){
    var images = document.getElementsByTagName('img');
    for(var i=0;i < images.length;i++){
        var attr = images[i].getAttribute('data-2x');
        if(attr){
            images[i].src = attr;
        }
    }
	}

上面的javascript代码只有在高清屏幕的设备上才会运行。它会检测页面中的所有**img**标签，然后取得所有img标签的**data-2x**的属性的值代替**src**里面的值，这样就实现了在高清屏幕上显示高分辨率图片，很简单吧。你要做的仅仅是在**img**标签里面多添加一个**data-2x**的属性就可以了。

当然上面的方案也有它的局限性，就是它只针对页面中**img**标签引入的图片有效。如果是使用CSS的**background-image**的话，该怎么办呢？也很简单：

    <div data-2x="background_big.jpg" style="background-image: url(background.jpg);"></div>

我们上面的HTML中的**div**是使用CSS来定义图片的，同样也添加了一个**data-2x**的属性，也需要配合javascript来实现。

    if(window.devicePixelRatio >= 1.2){
    var images = document.getElementsByTagName('*');
    for(var i=0;i < images.length;i++){
        var attr = images[i].getAttribute('data-2x');
        if(attr){
            images[i].style.cssText += 'background-image: url('+attr+')';
        }
    }
	}

这段代码和上面的类似，不过这回只是用来替换**background-image**的图片。把它合并为一个方案不失为一个正确的选择。

    <img src="myimage.jpg" data-2x="myimage@2x.jpg">
	<img src="photo_800.png" data-2x="photo_1600.png">
	
	<div data-2x="background_big.jpg" style="background-image: url(background.jpg);"></div>

假设我们现在在web页面中用到了**img**和**background-image**两种图片方案，javascript代码如下：

    if(window.devicePixelRatio >= 1.2){
	    var elems = document.getElementsByTagName('*');
	    for(var i=0;i < elems.length;i++){
	        var attr = elems[i].getAttribute('data-2x');
	        if(attr && elems[i].tagName == 'IMG'){
	            elems[i].src = attr;
	        } else if(attr){
	            elems[i].style.cssText += 'background-image: url('+attr+')';
	        }
	    }
	}

这段代码首先会检测页面中所有的元素然后筛选出所有的**data-2x**的属性。然后检测元素是否是**img**标签，是则更新**src**的属性；否则更新**background-image**的属性值。

如果是使用jQuery的话，代码可以简洁一些：

    if(window.devicePixelRatio >= 1.2){
	    $("[data-2x]").each(function(){
	        if(this.tagName == "IMG"){
	            $(this).attr("src",$(this).attr("data-2x"));
	        } else {
	            $(this).css({"background-image":"url("+$(this).attr("data-2x")+")"});
	        }
	    });
	}

当然要是图片正确的在页面上显示尺寸，还需要在CSS方面定义一些样式。如**img**的**width（宽）**，当**img**标签加载高清图片的时候图片的尺寸会比原先的尺寸大一倍，比如图片的正常尺寸是400px，那么高清图片的尺寸则是800px，这样图片就可能会打破页面的布局。所以我们需要在CSS中定义图片的宽高属性，不过我们一般不需要这样做。
我们只需要定义**img**的宽为100%就行了即：

    img {
    max-width: 100%;
	}

设置完以后，图片会根据浏览器窗口的大小来自动调整自身大小。现在web开发中这已经成为一个标准的解决方案。

如果是使用**background-image**定义的图片的话，我们可以设置**background-size**的值为cover。这样图片会自适应容器的大小来调整自身的大小。

    [data-2x] {
    background-size: cover;
	}

打完收工，上面说的这种方案还是蛮简单的，可以在项目中使用。

参考文章地址[原文地址](http://drew.roon.io/retina-images-that-respond)。
    