---
layout: post
title: 	使用AJAX和jQuery加速你的网站，提高web的用户体验
categories:
- Life
tags:
- javascript
- jquery
---

> 这篇文章来自[webdesignerdepot](http://www.webdesignerdepot.com/)网站的[How to supercharge your site’s speed with AJAX and jQuery](http://www.webdesignerdepot.com/2014/02/how-to-supercharge-your-sites-speed-with-ajax-and-jquery/)，具体翻译有删减。

在本文中，我们将会学习使用不同的方法来提高网站的用户体验。主要针对的是网站中的静态内容部分。

我们来假设这样一个场景，在很多的一些企业类型的网站中，大部分的内容都是静态的，一般点击不同的导航就会跳转到新的页面然后加载新的内容。我们所要做的就是当用户点击导航的不跳转页面，只是内容区域部分的内容，简而言之就是不要刷新页面。

我们这里使用两种不同方法来实现这种效果，一种是使用jQuery；另外一种是使用PHP和AJAX来实现。当然它们的优缺点也不尽相同。首先我们来看看最终的[demo](http://netdna.webdesignerdepot.com/uploads7/how-to-supercharge-your-sites-speed-with-ajax-and-jquery/demo1/)。

### jQuery实现效果 ###

首先我们来编写页面的代码。HTML页面非常简单，但有几个地方值得注意。就是在我们的链接中的**href**指向我们定义的ID，如下代码所示：

    <body>
	<header>
	    <h1>Speed Up Static Sites with jQuery</h1>
	    <nav>
	        <ul>
	            <li><a href="#page1" class="active" id="page1-link">Page 1</a></li>
	            <li><a href="#page2" id="page2-link">Page 2</a></li>
	            <li><a href="#page3" id="page3-link">Page 3</a></li>
	            <li><a href="#page4" id="page4-link">Page 4</a></li>
	        </ul>
	    </nav>
	</header>
	<div id="main-content">
	    <section id="page1">
	        <h2>First Page Title</h2>
	        <p>First page content.</p>
	    </section>
	    <section id="page2">
	        <h2>Look, no page load!</h2>
	        <p>Second page content.</p>
	    </section>
	    <section id="page3">
	        <h2>Ooh fade!</h2>
	        <p>Third page content.</p>
	    </section>
	    <section id="page4">
	        <h2>Fourth Page Title</h2>
	        <p>Fourth page content.</p>
	    </section>
	</div> <!-- end #main-content -->
	<script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js"></script>
	<script type="text/javascript" src="custom.js"></script>
	</body>

从上面的代码中，我们可以看到我们的链接都指向对应的内容区域。比如连接到**Page2**的链接是这样写的**a href="#page2"**。我们的内容全部是在是**#main-content**这个结构里面，每一个内容都用**section**标签包裹起来。从上面的代码中可以知道，我们引入了jQuery文件和一个新建的custom.js文件。

在开始编码之前，我们需要把其它的内容隐藏起来，如下所示：

    #page2, #page3, #page4 {
		display: none;
	}

### javascript代码 ###

现在就正式来编写我们的jQuery代码。打开custom.js文件，当用户点击导航链接的时候，我们需要显示相对应的内容并且隐藏其它内容，代码如下所示：

    $(function() {
    $('header nav a').click(function() {
        var $linkClicked = $(this).attr('href');
        document.location.hash = $linkClicked;
        if (!$(this).hasClass("active")) {
            $("header nav a").removeClass("active");
            $(this).addClass("active");
            $('#main-content section').hide();
            $($linkClicked).fadeIn();
            return false;
        }
        else {
            return false;
        }
    });
    var hash = window.location.hash;
    hash = hash.replace(/^#/, '');
    switch (hash) {
        case 'page2' :
            $("#" + hash + "-link").trigger("click");
            break;
        case 'page3' :
            $("#" + hash + "-link").trigger("click");
            break;
        case 'page4' :
            $("#" + hash + "-link").trigger("click");
            break;
    }
	});

上面代码主要分两部分，首先来说说第一部分。当点击导航链接的时候，我们把链接的地址存储在**$linkClicked**这个变量中，并且还把这个名字更新到浏览器的地址里。然后我们就通过一个循环来判断用户的点击是否是当前的链接，如果是则不做任何操作；如果是其它的链接则删除当前链接的高亮状态并隐藏对应内容从而显示点击链接的内容。这部分非常简单。

第二部分是是来检测url的，这里用到了javascript中的location对象，而location.hash则可以用来获取或设置页面的标签值。比如http://domain/#admin的location.hash="#admin"。利用这个属性值可以做一个非常有意义的事情。

例如上面的代码中，我们就根据浏览器中的hash值并且模拟点击事件来加载对应的页面。这就是第一种方法，全部内容都在一个页面里面。并且不需要重新加载页面。下面我们来看看另一种方法。

### AJAX和PHP ###

使用AJAX和PHP，我们就不需要把所有的内容放到一个页面里面去，依靠AJAX方式按需加载内容。下面这张图片是我们需要准备文件以及组织结构：

![](http://pic.yupoo.com/reicky_v/DxWQdNZL/medium.jpg)

从上面的文件可以看到，我们的index文件变成以php结尾的php文件以及一个用来处理加载内容的load.php文件，并且把不同的内容的HTML文件全部放在一个pages的文件夹中。要运行php文件，我们需要架设一个服务器环境，我们可以使用像针对mac平台[MAMP](http://mamp.info/)或者是windows平台的[WAMP Server](http://www.wampserver.com/en/)这两个软件来快速搭建服务器环境。这样PHP才能正常运行。

注意index.php文件不再加载全部内，只是在原先内容显示的区域使用include引入了静态的页面，如下所示：

    <body>
	<header>
    <h1>AJAX a Static Site</h1>
    <nav>
        <ul>
            <li><a href="#page1" class="active" id="page1-link">Page 1</a></li>
            <li><a href="#page2" id="page2-link">Page 2</a></li>
            <li><a href="#page3" id="page3-link">Page 3</a></li>
            <li><a href="#page4" id="page4-link">Page 4</a></li>
        </ul>
    </nav>
	</header>
	<div id="main-content">
	<?php include('pages/page1.html'); ?>
	</div> <!-- end #main-content -->
	<script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js"></script>
	<script type="text/javascript" src="custom.js"></script>
	</body>

使用include我们引入我想要显示的任何内容。

###在javascript中使用$.ajax ###

现在让我们来编写js文件，我们使用AJAX的方式来根据用户点击的链接来显示相对应的内容。代码如下：

    $(function() {
    $('header nav a').click(function() {
        var $linkClicked = $(this).attr('href');
        document.location.hash = $linkClicked;
        var $pageRoot = $linkClicked.replace('#page', '');
        if (!$(this).hasClass("active")) {
            $("header nav a").removeClass("active");
            $(this).addClass("active");
            $.ajax({
                type: "POST",
                url: "load.php",
                data: 'page='+$pageRoot,
                dataType: "html",
                success: function(msg){
                if(parseInt(msg)!=0)
                {
                    $('#main-content').html(msg);
                    $('#main-content section').hide().fadeIn();
                }
            }
        });
    }
    else {
        event.preventDefault();
    }
	});

相比较前面的js，第二部分判断浏览器地址hash值的部分是一样的，只是改变了第一部分。让我们来来看看上面的代码具体是实现什么样的功能。首先是**$pageRoot**变量，这个变量是用来存储hash值中#page后面的数字的(这里使用了正则表达式来取出hash值中的数字)。然后使用了一个循环判断并且使用AJAX方法来发送信息给服务端，并且在内容显示使用jQuery中的fade()方法，这样不至于显示内容过于生硬。

#### load.php ####

load.php文件就简单了，就是根据接收从客户端发来信息来加载显示内容，我们在客户端会根据用户点击的链接的发送数字到服务端，而load.php文件则根据接收到的数字输出对应的html文件。

    <?php
	if(!$_POST['page']) die("0");
	$page = (int)$_POST['page'];
	if(file_exists('pages/page'.$page.'.html'))
	echo file_get_contents('pages/page'.$page.'.html');
	else echo 'There is no such page!';
	?>

上面介绍了两种方法来提升网站的用户体验。我们可以在项目中按需选择。

jQuery效果的地址[这里](http://netdna.webdesignerdepot.com/uploads7/how-to-supercharge-your-sites-speed-with-ajax-and-jquery/demo1/)，	PHP地址[这里](http://netdna.webdesignerdepot.com/uploads7/how-to-supercharge-your-sites-speed-with-ajax-and-jquery/demo2/)或者是[下载源代码](http://netdna.webdesignerdepot.com/uploads7/how-to-supercharge-your-sites-speed-with-ajax-and-jquery/download.zip)。

