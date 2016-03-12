---
layout: post
title: 使用jQuery来处理多个AJAX和JSON请求以及回调函数
categories:
- Life
tags:
- jQuery
---

在一些常见的web开发中，有时候我们需要同时加载一些脚本和json数据。现在使用jQuery的[jQuery.getScript](http://davidwalsh.name/loading-scripts-jquery)和jQuery.getJSON这两个方法就很容易处理这样的开发需求。

还有在开发网站的过程中，我们也会经常遇到某些耗时很长的javascript操作。其中，既有异步的操作（比如ajax读取服务器数据），也有同步的操作（比如遍历一个大型数组），它们都不是立即能得到结果的。

通常的做法是，为它们指定回调函数（callback）。即事先规定，一旦它们运行结束，应该调用哪些函数。使用jQuery中的jQuery.when()方法很容易做到这些。

比如下面的：

	$.when(
	$.getScript('/media/js/wiki-min.js?build=21eb633'), 
	$.getJSON('https://developer.mozilla.org/en-US/demos/feeds/json/featured/')
	).then(function(a, b) {  // or ".done"
		
		// 回调函数，当资源加载完后执行回调函数
	
	})

上面的代码的意思是当资源加载完后，然后执行**done**或者是**then**回调函数。

如果我们想添加一个传统的AJAX XHR对象到when方法里面，那该怎么做呢：

    $.when(
	$.getScript('/media/js/wiki-min.js?build=21eb633'), 
	$.getJSON('https://developer.mozilla.org/en-US/demos/feeds/json/featured/'), 
	$.get('/')
	).then(function(a, b, c) { 
		console.log(a, b, c); 
	});

在实际开发中如果能很好的利用when方法对于提高开发效率还是很有帮助的。

参考文章[jQuery: Multiple AJAX and JSON Requests, One Callback](http://davidwalsh.name/jquery-ajax-callback)