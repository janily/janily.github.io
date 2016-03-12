---
layout: post
title: AngularJS学习初探(1) 
categories:
- Life
tags:
- javascript
---

AngularJS是Google开发的纯客户端JavaScript技术的WEB框架，用于扩展、增强HTML功能，它专为构建强大的WEB应用而设计。之前也接触过backbone等前端框架，但看了AngularJS之后就深深地被它迷上啦。关于它的详细资料，google一下有很多的介绍。而且还是google出品哦~~！

下面就来跟 Angularjs亲密接触一下。你就发现就可以用它快速的创造强大的web应用。

###亲密接触Angularjs

在开始用Angularjs我们需要一段简单的html结构，在头部引入Angularjs，代码如下

<pre>
<code>
&lt;!DOCTYPE html&gt;
&lt;head>
     &lt;title>Learning AngularJS&lt;/title&gt;
	 &lt;script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.0.7/angular.min.js" type="text/javascript"&gt;&lt;/script&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;/body&gt;
&lt;/html&gt;
</code>
</pre>

####页面设置

在开始使用Angularjs写web应用前，我们需要在页面上设置一些东西，才可以在页面中用Angularjs来开发web应用，如以下代码

<pre>
<code>
	...
	&lt;div id='content' ng-app='MyTutorialApp'&gt;&lt;/div&gt;
	...
</code>
</pre>

可以发现代码中添加了这么一行ng-app='MyTutorialApp'，这一行表示什么呢？ng-app指令标记了AngularJS脚本的作用域，ng-app表示AngularJS脚本仅在该&lt;div&gt;中运行。

是时候给这些网页来点动态特性了——用AngularJS！我们这里为后面要加入的控制器添加了一个测试。

一个应用的代码架构有很多种。对于AngularJS应用，我们鼓励使用模型-视图-控制器（MVC）模式解耦代码和分离关注点。考虑到这一点，我们用AngularJS来为我们的应用添加一些模型、视图和控制器。

首先我们来创建一个名为MainController的控制器的文件，并且在我们的作用域上声明使用它，代码如下

<pre>
<code>
	...
	&lt;div id='content' ng-app='MyTutorialApp' ng-controller='MainController'&gt;

	&lt;/div&gt;
	...
</code>
</pre>

控制器的代码为

<pre>
</code>
app.controller("MainController", function($scope){
		
});
</code>
</pre>

我们还要创建一个名为app.js的文件，代码如下
<pre>
<code>
var app = angular.module('MyTutorialApp',[]);
</code>
</pre>

上述代码表示我们创建了一个名为MyTutorialApp的应用，并把这个应用赋给了app的一个变量，并且定义了MyTutorialApp的module,给后面的程序使用。

好了，整个为angular开发的配置完成了，整个的代码如下

<pre>
<code>
&lt;!DOCTYPE html&gt;
&lt;head>
     &lt;title>Learning AngularJS&lt;/title&gt;
	 &lt;script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.0.7/angular.min.js" type="text/javascript"&gt;&lt;/script&gt;
	&lt;script src="app.js" type="text/javascript"&gt;&lt;/script&gt;
	&lt;script src="maincontroller.js" type="text/javascript"&gt;&lt;/script&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;/body&gt;
&lt;/html&gt;
</code>
</pre>

###理解作用域

scope是一个指向应用model的object。它也是expression的执行上下文。scope被放置于一个类似应用的DOM结构的层次结构中。scope可以监测（watch,$watch）expression和传播事件。

scope是在应用controller与view之间的纽带。在scope中设置表达式。表达式让directive能够得知属性的变化，使得directive将更新后的值渲染到DOM中。这个directive不知道怎么翻译好~~！

下面还是直接上例子，更直观的理解下scope

我们先在控制器里面声明一个$scopr变量，代码如下

<pre>
<code>
app.controller("MainController", function($scope){
		$scope.understand = "hello world! ";
});
</code>
</pre>

下面我们就可以在我们的html页面中来使用$scope变量了

<pre>
<code>
	...
	&lt;div id='content' ng-app='MyTutorialApp' ng-controller='MainController'&gt;
			{{understand}}
	&lt;/div&gt;
	...
</html>
</code>
</pre>

我们来看一下根据上面的原理，建立的一个简单的demo

<a class="jsbin-embed" href="http://jsbin.com/utalac/1/embed?live">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

看完这个简单的demo后，是不是感觉到angularjs的魅力啦！话说要写技术博客还真是有点吃力啊，特别是对于我这种野路子的更是如此啊，今天就到这拉，下篇继续介绍angularjs~~！




