---
layout: post
title: AngularJS学习初探(2)-数据绑定
categories:
- Life
tags:
- javascript
---
这篇是接着上篇文章来的。

Angularjs的数据绑定是一个非常强大的功能，具体应用比如：两个输入框，我在任意一个输入框中的修改都会同步影响到另一个。这就要需要用到angularjs中的数据绑定功能，主要是通过ng-model来完成的， ng-model 是把两个方向的绑定都做了。它不光显示出变量的值，也把显示上的数值变化反映给了变量。这个在实现上就简单多了，只是绑定 change 事件，然后做一些赋值操作即可。不过 ng 里，还要区分对待不同的控件。

为了更好的理解这个概念，我们就来做这样一个数据到模板双向绑定的实例。

首先我们要在我们的控制器里面先声明一个变量，后面绑定会用到，代码如下

<pre>
<code>
	app.controller("MainController", function($scope){
		$scope.inputValue = "";
	});
</code>
</pre>

当然在html中也要做些对应的修改工作，代码如下,主要是基于上一篇的html代码来修改的，就写下主要的部分。

<pre>
<code>
	&lt;div id='content' ng-app='MyTutorialApp' ng-controller='MainController'&gt;
		&lt;input type='text' ng-model='inputValue' /&gt;
		{{inputValue}}
	&lt;/div&gt;
</code>
</pre>

在上面的代码中我们用到了ng-model这个标签，绑定了inputValue这个变量。绑定之后，就可以实现数据的绑定了，这样我们在input里面输入的值，它不光显示出变量的值，也把显示上的数值变化反映给了变量。

demo如下，你可以在文本框随便输入一段文字试试看，文本框里面的值会及时更新到dom中

<a class="jsbin-embed" href="http://jsbin.com/upasiy/1/embed?live">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

今天先到这拉，下篇接着聊angularjs~~！


