---
layout: post
title: 为网站的加载添加一个进度条
categories:
- Life
tags:
- javascript
- 翻译
---

随着移动互联网的发展，现在的网站开始朝着基于Web网站的系统和应用，向广大的最终用户发布一组复杂的内容和功能。也就是现在所谓的WebApp。也就是把原来跑在本地的客户端应用搬到互联网上，直接提供给给用户上网使用。最近google旗下的一些网站，有在页面加载的时候应用到进度条这个功能，比如Youtube就用到了。

在这篇文章里，我们将使用NProgress这个jQuery这个插件来给网页添加一个加载进度条。

###  NProgress插件 ###

NProgress是一个jQuery进度条插件，效果就像YouTube网站上的进度条一样。它主要是通过一个全局对象-NProgress来使用它提供的一些方法来控制进度条。这里提供一个简单的demo效果，来快速了解一下NProgress的使用

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/aA2t9/embedded/result,js,html,css" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

NProgress插件的作者建议你在*$(document).ready()*的时候调用*NProgress.start()*方法，当*$(window).load()*的时候调用NProgress.done()。这样就可以给网页添加一个加载进度条了。这样可能并有真正的显示你网站真是的加载进度（如果你要显示网页真实的资源的加载进度，你得监控你网站上所有资源的加载），大部分用户是不会关心的。

现在你应该知道怎么用NProgress了。让我们做一个稍微复杂点的例子来应用NProgress-一个加载图片的进度条。这个进度条会随着当前加载图片的数量做出加载的响应。

### 图片画廊 ###

首先我们写一个简单的HTML结构，包含一个DIV和一个加载的按钮：

    <!DOCTYPE html>
	<html>
	
	    <head>
	        <meta charset="utf-8"/>
	        <title>Quick Tip: Add a Progress Bar to Your Site</title>
	
	        <link href="http://fonts.googleapis.com/css?family=PT+Sans+Narrow:700" rel="stylesheet" />
	
	        <!-- The Stylesheets -->
	        <link href="assets/nprogress/nprogress.css" rel="stylesheet" />
	        <link href="assets/css/style.css" rel="stylesheet" />
	
	    </head>
	
	    <body>
	
	        <h1>Gallery Progress Bar</h1>
	
	        <div id="main"></div>
	
	        <a href="#" id="loadMore">Load More</a>
	
	        <script src="http://cdnjs.cloudflare.com/ajax/libs/jquery/1.10.2/jquery.min.js"></script>
	        <script src="assets/nprogress/nprogress.js"></script>
	        <script src="assets/js/script.js"></script>
	
	    </body>
	</html>

html中需要引入nprogress插件和jquery文件，在头部引入了Google Webfonts和一个样式文件。运行效果如下图所示：

![](http://pic.yupoo.com/reicky_v/Da31OpNN/medium.jpg)

接下来是jQuery代码的编写。这里我用到jQuery中的deferred对象的方法来显示图片。因为我们需要加快图片的加载速度，而不是等到图片全部加载完后才显示图片。这篇文章不会对deferred对象详细介绍，不过你可以去阅读这几篇文章，[文章一](http://www.erichynds.com/blog/using-deferreds-in-jquery)，[文章二](http://eng.wealthfront.com/2012/12/jquerydeferred-is-most-important-client.html)。[文章三](http://domenic.me/2012/10/14/youre-missing-the-point-of-promises/)。简单的来说，deferred对象就是jQuery的回调函数解决方案，非常强大。

下面是javascript代码部分，有非常详细的注释：

    (function($){

    // photos数组使用来存放图片的，也可以用AJAX的方法从服务器上啦取图片

    var photos = [
        'assets/photos/1.jpg',	'assets/photos/2.jpg',
        'assets/photos/3.jpg',	'assets/photos/4.jpg',
        // more photos here
    ];

    $(document).ready(function(){		

        // 定义变量

        var page = 0,
            loaded = 0,
            perpage = 10,
            main = $('#main'),
            expected = perpage,
            loadMore = $('#loadMore');

        // 监听图片加载事件

        main.on('image-loaded', function(){

            // 当图片加载的时候，启动NProgress

            loaded++;

            // NProgress.set用来设置NProgress加载的百分比
            NProgress.set(loaded/expected);

            if(page*perpage >= photos.length){

                // 如果图片全部加载完后，删除加载更多这个按钮

                loadMore.remove();
            }
        });

        //当点击加载按钮的时候，加载图片。加载图片的数量由perpage变量控制

		//加载按钮点击事件

        loadMore.click(function(e){

            e.preventDefault();

            loaded = 0;
            expected = 0;

            // 我们通过Deferred对象中的执行状态，来执行加载图片
			//jQuery规定，deferred对象有三种执行状态----未完成，已完成和已失败。
			//如果执行状态是"已完成"（resolved）,deferred对象立刻调用done()方法指定的回调函数；
            // 所以，这里当页面开始载入的时候，就立即加载图片
            var deferred = $.Deferred().resolve();

            // 从数组中取出图片，然后显示图片。
            // 有可能图片的数量在最后一页的时候，显示的数量可能少于perpage规定的数量

            $.each(photos.slice(page*perpage, page*perpage + perpage), function(){

                // 通过上面定义的deferred，来调用showImage方法加载图片。

                deferred = main.showImage(this, deferred);

                expected++;
            });

            // 启动NProgress加载进度条动画
            NProgress.start();

            page++;
        });

        loadMore.click();
    });

    // 创建一个showImage的插件，用来加载图片
    // 需要两个参数
    //	* src - 图片的路径
    //	* deferred - jQuery deferred 对象, 用来调用showImage
    // 
    //当图片加载完后，返回deferred对象

    $.fn.showImage = function(src, deferred){

        var elem = $(this);

        // 返回deferred对象

        var result = $.Deferred();

        // 创建一个div节点用来包裹图片

        var holder = $('<div class="photo" />').appendTo(elem);

        // 定义图片这个节点变量

        var img = $('<img>');

        img.load(function(){

            // 图片加载的时候，用deferred对象中.always()方法
			//这个方法也是用来指定回调函数的，它的作用是，
			//不管调用的是deferred.resolve()还是deferred.reject()，最后总是执行。

            deferred.always(function(){

                // 触发image-loaded事件
                elem.trigger('image-loaded');

                //把图片添加到div中，并添加一个动画效果

                img.hide().appendTo(holder).delay(100).fadeIn('fast', function(){

                    // 延迟图片加载，当下一个图片加载的时候
					//下一张图片加载的时候，又会调用这个方法

                    result.resolve()
                });
            });

        });

        img.attr('src', src);

        // 最后返回 deferred 对象
        return result;
    } 

	})(jQuery);

进度条会利用回调来监听默认的图片加载事件， showImage可以自由的处理图片的加载和显示。

可以去[这里](http://demo.tutorialzine.com/2013/09/quick-tip-progress-bar/)看看效果。

在保证技术要点表达准确的前提下，行文风格有少量改编。原文[这里](http://tutorialzine.com/2013/09/quick-tip-progress-bar/#comment-87710)。

