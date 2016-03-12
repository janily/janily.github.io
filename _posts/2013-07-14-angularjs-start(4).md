---
layout: post
title: AngularJS学习初探(4)-内容过滤（filter）和事件绑定
categories:
- Life
tags:
- javascript
---

这篇来聊聊angularjs中的过滤器和事件绑定。这里说的过滤器，是用于对数据的格式化，或者筛选的函数。它们可以直接在模板中通过一种语法使用。对于常用功能来说，是很方便的一种机制。

###内容过滤（filter）
 
主要来讲讲filter这个在angular中很棒的一个内容过滤特性。filter 是一个过滤内容的标签。如果参数是一个字符串，则列表成员中的任意属性值中有这个字符串，即为满足条件（忽略大小写）。

同样我们以例子来说明这个过滤的使用方法。

我们先来升级一下我们的html代码。

<pre>
<code>
	...
	&lt;div id='content' ng-app='MyTutorialApp' ng-controller='MainController'&gt;
		&lt;input type='text' ng-model='searchText' /&gt;
		&lt;ul&gt;
			&lt;li ng-repeat='person in people | filter:searchText'>#{{person.id}} {{person.name}}&lt;/li&gt;
		&lt;/ul&gt;
	&lt;/div&gt;
	...
</code>
</pre>

在上面的代码中我们，我们新增了一个文本输入框，并申明了一个名为searchText的model来负责数据的绑定和渲染工作。我们还用ng-repeat把people变量中的ID和name字段值循环遍历出来放在一个ul列表中。

其实到这里，这个demo就可以工作了。你可以试着在文本框里输入文本框下面中的名字或者ID看看，filter就会筛选符合输入文本框参数的字符串。

demo如下，你可以试着在文本框里输入文本框下面中的名字或者ID看看

<a class="jsbin-embed" href="http://jsbin.com/icadum/1/embed?live">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

###事件绑定

事件绑定是模板指令中很好用的一部分。我们可以把相关事件的处理函数直接写在 DOM 中，这样做的最大好处就是可以从 DOM 结构上看出业务处理的形式，你知道当你点击这个节点时哪个函数被执行了。

angular中支持的事件绑定和jquery中的事件绑定的用法差不多，详细的可以去angular的文档中去看看。

对于事件对象本身，在函数调用时可以直接使用 $event 进行传递：

<pre>
<code>
	&lt;p ng-click="click($event)"&gt;点击&lt;/p&gt;
  	&lt;p ng-click="click($event.target)"&gt;点击&lt;/p&gt;
</code>
</pre>

仍然通过实例来了解下事件绑定是怎样工作的。

首先我们在上面的html基础上来升级下我们的html代码

<pre>
<code>
	&lt;ul&gt;
	...
	&lt;/ul&gt;
	&lt;input type='text' ng-model='newPerson' /&gt;
	&lt;button ng-click='addNew()'&gt;Add&lt;/button&gt;
</code>
</pre>

我们增加了一个文本框并声明了model为newPerson和一个绑定了addNew()函数的单击事件的函数。

同样我们还要升级下我们控制器部分的代码

<pre>
<code>
	app.controller("MainController", function($scope){
  	$scope.people = [
		{
			id: 0,
			name: 'Leon',
			music: [
				'Rock',
				'Metal',
				'Dubstep',
				'Electro'
			]
		},
		{
			id: 1,
			name: 'Chris',
			music: [
				'Indie',
				'Drumstep',
				'Dubstep',
				'Electro'
			]
		},
		{
			id: 2,
			name: 'Harry',
			music: [
				'Rock',
				'Metal',
				'Thrash Metal',
				'Heavy Metal'
			]
		},
		{
			id: 3,
			name: 'Allyce',
			music: [
				'Pop',
				'RnB',
				'Hip Hop'
			]
		}
	];
	$scope.newPerson = null;
	$scope.addNew = function() {
		if ($scope.newPerson != null && $scope.newPerson != "") {
			$scope.people.push({
				id: $scope.people.length,
				name: $scope.newPerson,
				music: []
			});
		}
	}
});
</code>
</pre>

上面的代码中我们声明了一个newPerson的变量，和addNew这函数。首先我们检测newPerson这个变量是否为空，当不为空的时候，我们就把文本框里面的值增加到people变量中。

非常简单，我们就把事件和数据都绑定到了dom中，当然在angular中，我们还可以绑定像诸如ng-change, ng-checked, ng-select等等事件到dom上。详细的可以angular的官方文档中区瞧瞧。

我们来看下最终的效果

<a class="jsbin-embed" href="http://jsbin.com/ocupez/1/embed?live">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

好了基本上angular一些主要的特性介绍的差不多了，下篇我们来一个实际的应用的例子，来具体了解下angular做出一个完整的web app是如何来做的。当然angular远不是止我这几篇文章能说的明白的。还是要去angular的官方网站多看看。

最后说明一下我这个初探系列中例子主要是用了[这篇](http://www.revillwebdesign.com/angularjs-tutorial/)文章中例子。大伙也可以去看看这篇文章也是关于angular的，不过是英文的哦。















