---
layout: post
title: 如何更好地书写高性能的jQuery代码
categories:
- Life
tags:
- jquery
- 翻译
---

> jQuery现在作为一个在前端的一个事实上的javascript的标准库类的地位，应该是毋庸置疑的了。从事前端的人不懂jQuery，亲，你从火星来的么。不过正因为jQuery入门门槛低，看这官方的API文档，就可以立马开工写出一些常见的交互效果，要造成jQuery的代码质量参差不齐，这应该也是很多前端界大牛鄙视写jQuery一个原因吧。今天刚好看到一篇关于如何书写高质量的jQuery代码的文章，特翻译过来，学习学习。顺便也可以检查下自己书写jQuery代码的质量。这篇文章来自于[flippinawesome](http://flippinawesome.org/)网站的[Writing Better jQuery Code](http://flippinawesome.org/2013/11/25/writing-better-jquery-code/?utm_source=javascriptweekly&utm_medium=email)。

现在有很多的讨论关于jQuery和原生javascript性能方面的文章。不过，今天这篇文章，我打算谈谈如何改进我们书写jQuery代码的方式，从而来提高我们代码的质量。更好地代码意味着提高我们网站的性能以及加载速度和更好地用户体验。

首先，我们要明白的一点的是jQuery代码也是javascript代码，这里推荐去读一读这两篇文章[Javascript best practices for begining](http://net.tutsplus.com/tutorials/JavaScript-ajax/24-JavaScript-best-practices-for-beginners/)和[ writing high quality JavaScript](http://net.tutsplus.com/tutorials/JavaScript-ajax/the-essentials-of-writing-high-quality-JavaScript/)。

OK,如果你使用jQuery开发，那么下面这些开发实践可以大大提高你的代码质量。

### 缓存(Var Caching) ###

操作DOM是前端开发中必不可少的的一道工序，但也是一项非常昂贵的性能开销。所以当我们要操作DOM的时候，缓存是一个很好的开发实践。

    // 不好的写法

	h = $('#element').height();
	$('#element').css('height',h-20);
	
	// 好的写法
	
	$element = $('#element');
	h = $element.height();
	$element.css('height',h-20);

### 避免全局变量(Avoid Globals) ###

无论使用jQuery还是原生javascript，在定义变量的时候，最好是在使用它的范围内，来定义它。尽量避免全局变量。

    // 不好的写法

	$element = $('#element');
	h = $element.height();
	$element.css('height',h-20);
	
	// 好的写法
	
	var $element = $('#element');
	var h = $element.height();
	$element.css('height',h-20);

### 使用匈牙利命名规范(Use Hungarian Notation) ###

在使用jQuery开发的时候，我们最好在变量前面加上美元符号($)。这样我们就能够很好的知道这是一个jQuery对象。

    // 不好的写法

	var first = $('#first');
	var second = $('#second');
	var value = $first.val();
	
	// // 好的写法 - $ 符号表明我们可以用jQuery来操作它
	
	var $first = $('#first');
	var $second = $('#second'),
	var value = $first.val();

### 使用单变量模式来声明变量(Use Var Chaining (Single Var Pattern)) ###

比起每个变量都使用var来声明，你可以只用一个var关键字来声明一组变量。建议把未分配值的变量放在末尾。

    var 
	  $first = $('#first'),
	  $second = $('#second'),
	  value = $first.val(),
	  k = 3,
	  cookiestring = 'SOMECOOKIESPLEASE',
	  i,
	  j,
	  myArray = {};

### 用on来绑定事件(Prefer ‘On’) ###

最新版本的jQuery已经改变了想**click()**这类事件的绑定方式。现在推荐使用**on('click')**来绑定事件。在低一点的版本中使用**bind()**来绑定事件的。在jQuery1.7版本推荐用**on()**来绑定事件。从现在开始，你可以放心大胆的使用**on()**来绑定事件。

    // 坏的方式

	$first.click(function(){
		$first.css('border','1px solid red');
		$first.css('color','blue');
	});
	
	$first.hover(function(){
		$first.css('border','1px solid red');
	})
	
	// 好的方式
	$first.on('click',function(){
		$first.css('border','1px solid red');
		$first.css('color','blue');
	})
	
	$first.on('hover',function(){
		$first.css('border','1px solid red');
	})

### 尽量的合并你的javascript代码(Condense JavaScript) ###

在一般情况下，尽可能的简写你的代码。

    // 坏的方式

	$first.click(function(){
		$first.css('border','1px solid red');
		$first.css('color','blue');
	});
	
	// 好的方式
	
	$first.on('click',function(){
		$first.css({
			'border':'1px solid red',
			'color':'blue'
		});
	});

### 使用链式方法来组织你的代码(Use Chaining) ###

jQuery提供了一个非常的链式方法，如下：

	// 坏的方式
	
	$second.html(value);
	$second.on('click',function(){
		alert('hello everybody');
	});
	$second.fadeIn('slow');
	$second.animate({height:'120px'},500);
	
	// 好的方式
	
	$second.html(value);
	$second.on('click',function(){
		alert('hello everybody');
	}).fadeIn('slow').animate({height:'120px'},500);

### 保持代码的可读性 ###

当你使用链式和合并的方法来书写你的代码的时候，你的代码的可读性可能会大打折扣。所以在写代码的时候尽量使用一些良好的格式来提高你代码的可读性。

    //坏的方式

	$second.html(value);
	$second.on('click',function(){
		alert('hello everybody');
	}).fadeIn('slow').animate({height:'120px'},500);
	
	//好的方式
	
	$second.html(value);
	$second
		.on('click',function(){ alert('hello everybody');})
		.fadeIn('slow')
		.animate({height:'120px'},500);

### 使用一些逻辑运算符来简化你的代码(Prefer Short-circuiting) ###

[Short-curcuit evaluation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_Operators#Short-Circuit_Evaluation)是指运用一些逻辑操作运算符如**&&(和)**和**||(或者)**这些运算符。

    //坏的方式

	function initVar($myVar) {
		if(!$myVar) {
			$myVar = $('#selector');
		}
	}
	
	//好的方式
	
	function initVar($myVar) {
		$myVar = $myVar || $('#selector');

	}

### 使用简写方式 ###

充分利用一些简写方法也是简化你的代码的一种方法。

    // 坏的方法

	if(collection.length > 0){..}
	
	// 好的方法
	
	if(collection.length){..}

### 当你重复操作一些DOM元素的时候，使用Detach方法 ###

当需要移走一个元素，不久又将该元素插入DOM时，这种方法很有用。

    // 坏的方法

	var 
		$container = $("#container"),
		$containerLi = $("#container li"),
		$element = null;
	
	$element = $containerLi.first(); 
	//... a lot of complicated things
	
	// 好的方法
	
	var 
		$container = $("#container"),
		$containerLi = $container.find("li"),
		$element = null;
	
	$element = $containerLi.first().detach(); 
	//...a lot of complicated things
	
	$container.append($element);

### 需要知道的一些技巧 ###

当你要使用jQuery的一些方法的时候，最好是去查看一下其[官方文档](http://api.jquery.com/)，使用其推荐的方式来使用jQuery。比如：

    // 坏的方法

	$('#id').data(key,value);
	
	// 好的方法
	
	$.data('#id',key,value);

### 基于缓存的方式来查找元素 ###

在javascript中查找DOM元素是一项非常昂贵的操作。最好是把我们要查找的元素的父元素缓存起来。

    // 坏的方法

	var 
		$container = $('#container'),
		$containerLi = $('#container li'),
		$containerLiSpan = $('#container li span');
	
	// 好的方法
	
	var 
		$container = $('#container '),
		$containerLi = $container.find('li'),
		$containerLiSpan= $containerLi.find('span');

### 避免通用选择器(Avoid Universal Selectors) ###

使用通用选择器来选择元素不是一个值得推荐的方法，速度非常慢。

### 避免使用隐式的通用选择器 ###

有时候我们选择一些元素的时候，也有可能用到了一些隐式的通用选择器

    // 坏的方法

	$('.someclass :radio'); 
	
	// 好的方法
	
	$('.someclass input:radio');

### 优化选择器 ###

比如，使用一个ID选择器已经是唯一的了，没必要再添加一个其它的选择器来指定它。

    // 坏的方法

	$('div#myid'); 
	$('div#footer a.myLink');
	
	// 好的方法
	$('#myid');
	$('#footer .myLink');

    // 坏的方法

	$('#outer #inner'); 
	
	// 好的方法
	
	$('#inner');

### 使用最新版本的jQuery ###

一般来说，最新版本的jQuery代码：会优化其代码体积和执行速度。所以我们在使用jQuery开发的时候，一般推荐使用最新版本的代码，需要注意一点的是2.0版本的jQuery代码已经不支持IE6-8版本的浏览器了。

### 不要使用已经废弃的一些方法 ###

在使用新版本的jQuery代码的时候，要关注已经[废除的一些方法](http://api.jquery.com/category/deprecated/)，避免使用已经废弃的一些方法。

    // 废弃的方法 live

	$('#stuff').live('click', function() {
	  console.log('hooray');
	});
	
	// 推荐使用的方法
	$('#stuff').on('click', function() {
	  console.log('hooray');
	});

### 用CDN来加载jQuery代码 ###

google提供了用CDN来加载jQuery代码。地址如下[ http://code.jQuery.com/jQuery-latest.min.js]( http://code.jQuery.com/jQuery-latest.min.js)

### 万变不离其宗 ###

正如前面说过的那样，jQuery也是javascript代码，这就意味着我们同样可以使用原生的javascript来做jQuery能做的事情。使用原生的javascript代码可能会使你的代码量增加和难以维护，但是同样也意味着你的代码运行着更快。记住一点，没有一个框架的代码能够比原生的代码运行的更快和轻量。

你可以去这个地址看一看原生javascript方法和jQuery方法的一个比较[native equivalent of jQuery functions](http://www.leebrimelow.com/native-methods-jQuery/)。

推荐你去看一些这篇文章[increasing jQuery performance](http://net.tutsplus.com/tutorials/JavaScript-ajax/10-ways-to-instantly-increase-your-jQuery-performance/)这篇文章，也是关于优化jQuery代码的。

使用jQuery只是一个可选项，不是必选项。你需要考虑的是你需要简化你的哪些操作，DOM操作？AJAX?模板？CSS动画?或者是一个选择器引擎？有时候你可以使用一些[微型的javascript框架](http://microjs.com/)或者是一个[定制版的jQuery](http://net.tutsplus.com/tutorials/JavaScript-ajax/how-to-build-your-own-custom-jQuery/)。


