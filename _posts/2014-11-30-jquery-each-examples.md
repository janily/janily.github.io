---
layout: post
title: jQuery.each()方法使用实例
categories:
- Life
tags:
- javascript
- jquery

---

> 一篇老文，主要是因为今天在写数组遍历的时候，用到了jquery中的jQuery.each()方法。刚好在网上看到一篇讲jquery中的jQuery.each()方法使用的实例，觉的还不错。简单的翻译了下，原文地址[5 jQuery.each() Function Examples](http://www.sitepoint.com/jquery-each-examples/)。

jQuery.each()方法是jquery中经常用到并且非常重要的方法，下面就把常见的几个用到$.each()方法场景以实例的形势列举出来。

### jQuery.each()方法的基本知识

jQuery.each方法是一个通用的迭代函数，它可以用来无缝迭代对象和数组。数组和类似数组的对象通过一个长度属性（如一个函数的参数对象）来迭代数字索引，从0到length - 1。其他对象通过其属性名进行迭代。

### $.each()语法

	//循环遍历dom元素
	$("div").each(function(index,value){
		console.log('div' + index + ':' + $(this).attr('id));
	});
	
	//输出每一个div元素的ID
	
	//数组
	
	var arr = ["one","two","three","four","five"];
	
	jQuery.each(arr,function(index,value){
		console.log(this);
		return(this != "three"); //在遍历到three后，会终止
		
	});
	//输出 one two three
	
	//对象
	
	var obj = {one:1,two:2,three:3,four:4,five:5};
	
	jQuery.each(obj,function(i,val){
		console.log(val);
	});
	
	//输出 1 2 3 4 5
	
更多关于jQuery.each()这个方法的使用可以去看看这篇文章[jquery-create-excerpts-text-elements/](http://www.jquery4u.com/snippets/jquery-create-excerpts-text-elements/)。

### 1、jQuery.each() 使用基本实例

	$('a').each(fucntion(index,value){
		console.log($(this).attr('href'));
	})
	//输出页面上每个链接的链接地址
	
	$('a').each(function(index,value){
		var ihref = $(this).attr('href');
		
		if(ihref.indexOf("http") >= 0)
		{
			console.log(ihref + '<br />');
		}
	})
	
	//输出页面中有外链的链接地址
	
比如，页面上的友情链接就属于这种链接：

	<a href="http://www.jquery4u.com">JQUERY4U</a>
	<a href="http://www.phpscripts4u.com">PHP4U</a>
	<a href="http://www.blogoola.com">BLOGOOLA</a>
	
上面的程序就会输出：

	http://jquery4u.com
 
 	http://www.phpscripts4u.com
 
 	http://www.blogoola.com
 	
### 2、jQuery.each()方法在数组中的使用

	var numberArray = [0,1,2,3,4,5];
	
	jQuery.each(numberArray,function(index,value){
		console.log(index + ':' +value);
	});
	
	//输出 0:0 1:1 2:2 3:3 4:4 5:5
	
### 3、jQuery.each()方法在JSON中的使用

	(function($){
		var json = [
			{"red": "#f00"},
			{"green":"#0f0"},
			{"blue":"#00f"}
		];
		
		$.each(json,function(){
			$.each(this,function(name,value){
				//do stuff
				
				console.log(name + '=' +value);
			});
		});
		//输出 red=#f00 green=#0f0 blue=#00f
	})(jQuery);
	
更多jQuery.each()方法在JSON中使用的例子可以去看看这篇文章[10 Example JSON Files](http://www.jquery4u.com/json/10-example-json-files/)。

### 4、jQuery.each()方法在类中使用

下面这个例子是通过使用jQuery.each()方法来遍历出特定类的dom元素并进行相关的操作。

如果有什么疑惑的话，可以先去看看这个例子[jsfiddle shows you the object types returned by $.each()](http://jsfiddle.net/ryDed/)。

	<div class="productDescription">Red</div>
	<div class="productDescription">Orange</div>
	<div class="productDescription">Green</div>
	
	$.each($('.productDescription'), function(index, value) { 
    console.log(index + ':' + value); 
	});
	//输出: 1:Red 2:Orange 3:Green
	
index和value参数不是必需的，这里只是为了方便理解。可以直接像下面这样写：

	$.each($('.productDescription'),fucntion(){
		console.log($(this).html());
	});
	
	//输出  Red Orange Green
	
### 5、jQuery.each()方法延迟执行实例

[点击我](http://www.sitepoint.com/jquery-each-examples/#)

点击上面的链接后，会发现菜单下面的大标题会变成黄色，是怎么做到的呢？

	$('#5demo).on('click',function(e){
		$('li').each(function(i){
			$(this).css('background-color','orange');
			$(this).delay(i*200).fadeOut(1500);
		});
	});
	
这里要提醒一点的是:$.each()和$(selector).each()完全是两个不同方法。

$.each()与 $(selector).each() 不同, 后者专用于jquery对象的遍历, 前者可用于遍历任何的集合(无论是数组或对象),如果是数组,回调函数每次传入数组的索引和对应的值(值亦可以通过this 关键字获取,但javascript总会包装this 值作为一个对象—尽管是一个字符串或是一个数字),方法会返回被遍历对象的第一参数。

关于这两个方法的区别可以去这里看看[demo](http://www.jquery4u.com/function-demos/index.php?function=each#)。




