---
layout: post
title: AngularJS学习初探(3)-内容渲染(ng-repeat)
categories:
- Life
tags:
- javascript
---
接着上篇文章。

数据绑定了，配合作用域 $scope 和 ng 的数据双向绑定机制， ng 的模板系统就变得比较牛逼了。

为了一窥究竟，我们就来做这样一个数据到模板双向绑定的实例。这之中也用到angularjs中表单控件。

首先我们要在我们把在上一篇中控制器里面代码升级一下，如下

<pre>
<code>
app.controller("MainController", function($scope){
$scope.selectedPerson = 0;
$scope.selectedGenre = null;
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
});
</code>
</pre>

在上面的代码中我们申明了selectedPerson变量是用来存放已经从people变量中选择的name这一的。selectedGenre变量是用来存放对应name这一字段相关类型的值的。

当然相应的我们的html也要升级一下，如下,只写一下升级代码的部分，完整的在下面demo中可以看到

<pre>
<code>
	...
	&lt;select ng-model='selectedPerson' ng-options='obj.name for obj in people'&gt;&lt;/select&gt;
		&lt;select ng-model='selectedGenre'&gt;
			&lt;option ng-repeat='label in people[selectedPerson.id].music'&gt;{{label}}&lt;/option&gt;
	&lt;/select&gt;
	...
</code>
</pre>

在上面的代码中我们用到了ng-options这个标签，这是一个比较牛B的控件。它里面的一个叫做 ng-options 的属性用于数据呈现。

对于给定列表时的使用。

最简单的使用方法， x for x in list ：,就如我们代码中用到一样。

我们上面中的ng-options定义了从people变量中把name这个字段的值拿出来作为select中option的值。

我们还用到了ng-repeat来渲染输出我们模板中的数据，ng-repeat它会迭代循环输出我们在模板中已经定义好的数据，非常方便使用。它可以应用于任何的html元素，非常牛逼的一个东东。

在我们的代码中ng-repeat会为我们把在people中的musice这一类型里面数据循环遍历出来，作为select中的option值显现出来。

说了这么一通，头都晕了。还是直接看代码吧，一目了然。

<a class="jsbin-embed" href="http://jsbin.com/upasiy/2/embed?live">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

分成一个一个小的单元来学一个东西，感觉还不错，下回接着聊angularjs~~!


