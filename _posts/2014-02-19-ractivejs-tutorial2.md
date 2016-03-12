---
layout: post
title: 	ractive.js学习笔记(2)——动态属性
categories:
- Life
tags:
- javascript
- ractive.js
---

> 下面会陆续把在Ractivejs网站的互动教程[Learn Ractive.js](http://learn.ractivejs.org/)学习的过程记录下来，加强自己的理解以及随时翻阅。

在上一篇文章中，我们大概预览了一下ractive是怎么样工作的，ractive对DOM操作的确非常方便。

这一片文章我们先来看看对于动态改变元素的属性在ractive中应该怎么样做？

我们来看看下面这个例子，新建一个demo2.html文件，代码如下：

    <!doctype html>
		<html lang='en-GB'>
		<head>
		    <meta charset='utf-8'>
		    <title>ractivejs start</title>
		</head>
		
		<body>
		    <h1>ractivejs start</h1>
		
		    <div id='container'></div>
		
		    <script id='myTemplate' type='text/ractive'>
		        <p style='color: {{color}}; font-size: {{size}}em; font-family: {{font}};'>
		          {{greeting}} {{recipient}}!
		        </p>
		    </script>
		
		    <script src='ractive.js'></script>
			<script>
		        var ractive = new Ractive({
		            el: 'container',
		            template: '#myTemplate',
		            data: {
		                greeting: 'Hello',
		                recipient: 'world',
		                color: 'purple',
		                size: 2,
		                font: 'Arial'
		            }
		        });
		    </script>
		</body>
		</html>

在上面的代码中，可以发现我们把元素的样式相关属性的值全部用变量表示的，然后再使用ractive来赋值。我们打开浏览器可以看到输出紫色的**Hello world**的文字，如下图所示：

![](http://pic.yupoo.com/reicky_v/Dy6Ht6jG/medium.jpg)

当然我们还可以使用**ractive.set()**来改变相关的属性：

    ractive.set({
	  color: 'red',
	  size: 3,
	  font: 'Comic Sans MS'
	});

你可以在浏览器中的控制台中输入(我使用的是chrome浏览器)，我们就可以看到它会实时把改变更新到视图中去，如下图所示：

![](http://pic.yupoo.com/reicky_v/Dy6IyEvf/medium.jpg)

**注意**

虽然我们同时改变了元素的三个属性，但是在Ractive看来也只是更新了一次DOM，而不是三次。

上面我们知道了使用Ractive怎样去改变属性值，下面我们再来看看使用Ractive和事件监听来制作一个稍微有动态效果有趣的实例。

在上面的模板中添加下面的代码：

    <button id='count'>Times this button has been clicked: {{counter}}</button>

我们主要要实现的是监听按钮的点击事件，点击按钮一次就计算一次点击累计点击的次数从0一直累加，记得在data中添加counter这个数值，代码如下所示：

    var ractive = new Ractive({
            el: 'container',
            template: '#myTemplate',
            data: {
                greeting: 'Hello',
                recipient: 'world',
                color: 'purple',
                size: 2,
                font: 'Arial',
                counter: 0
            }
        });

        document.getElementById( 'count' ).addEventListener( 'click', function () {
          ractive.set( 'counter', ractive.get( 'counter' ) + 1 );
        });

然后刷新浏览器。点击按钮会看到按钮里的数值随着点击次数的增加，也一直累加计算我们总共点击按钮的次数。如下图所示：

![](http://pic.yupoo.com/reicky_v/Dy70u73i/medium.jpg)

> 在以前如果遇到这样的需求，编码就复杂多了。需要不停的创建和删除按钮来更新其状态，所以你还需要不停的绑定和删除事件。当然你可以使用使用事件委托来处理频繁的事件处理。或者是手动的来操作DOM--想想就够你受的了，所以现在各种类型的MV*M框架都会具备双向绑定这个功能。

这篇先到这里，下篇接着学习。





