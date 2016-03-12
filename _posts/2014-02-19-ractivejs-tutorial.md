---
layout: post
title: 	ractive.js学习笔记(1)
categories:
- Life
tags:
- javascript
- ractive.js
---

现在关于js的MVC方面的框架层出不穷，比较的著名的有angularjs、emberjs、backbone等等。特别是angularjs和emberjs这两个框架在前端界那可谓是风生水起，一时风头无两。它们都提供一整套的比如路由、模板、数据双向绑定、ajax等等一整套web开发中常见的功能，就是一个重型武器。可是有时候我们仅仅只是在前端需要专注于UI层面的数据双向绑定就行了，并不需要想angularjs这样的重型武器。

ractivejs就是这样的一个小型的第三方的js库，专注于DOM的数据双向绑定、更新和事件处理。最重要的是学习曲线不是很高，没有像angularjs那样有很多比如依赖注入等概念需要理解。

今天就先来一个简单的介绍一下ractivejs是怎样工作的，也是基于官方的一个[快速入门教程](http://docs.ractivejs.org/latest/second-setup)来写的。

首先需要下载[ractivejs文件](https://raw.github.com/RactiveJS/Ractive/master/Ractive.js)。

然后新建一个demo.html文件，并且在底部引入下载好的ractivejs文件，代码如下：

    <!doctype html>
	<html lang='en-GB'>
	<head>
	    <meta charset='utf-8'>
	    <title>ractivejs start</title>
	</head>
	
	<body>
	    <h1>ractivejs start</h1>
	    <script src='ractive.js'></script>
	</body>
	</html>

然后添加一个id为container的容器，给ractivejs使用：

    <body>
    <h1>ractivejs start</h1>

    <div id='container'></div>

    <script src='ractive.js'></script>
	</body>

对于模板有很多的方法来使用模板。我们这里就采用最简单的一种方法，就是使用**text/javascript**这样的标签来存放模板文件，代码如下：

    <body>
    <h1>ractivejs start</h1>

    <div id='container'></div>

    <script id='myTemplate' type='text/ractive'>
        <p>{{greeting}}, {{recipient}}!</p>
    </script>

    <script src='ractive.js'></script>
	</body>

为了能够是ractive能够正常工作，我们还需要在底部编写如下的代码：

    <body>
    <h1>ractivejs start</h1>

    <div id='container'></div>

    <script id='myTemplate' type='text/ractive'>
        <p>{{greeting}}, {{recipient}}!</p>
    </script>

    <script src='ractive.js'></script>
	<script>
        var ractive = new Ractive({
            el: 'container',
            template: '#myTemplate',
            data: { greeting: 'Hello', recipient: 'world' }
        });
    </script>
	</body>

现在你打开浏览器你会看到浏览器为我们输出了**Hello world**的文字信息。如果你打开控制台输入下面的代码**ractive.set('recipient', 'Jim')**，浏览器的里面的文字也会实时更新。

一个简单是DOM的双向绑定就完成了，是不是非常简单。

### 注意 ###

- 你也可以通过使用模板字符串来作为模板从而代替使用ID**script**作为标记的模板。当然如果你的模板文件很大的话，你也许需要把模板单独出来一个文件使用**$ajax**的方法来加载它，如果你是使用jQuery的话。
- 如果你应用程序很庞大的话，你也可以把不同**new Ractive()**代码分成不同的模块文件。可以[配合Requirejs来使用](http://docs.ractivejs.org/latest/using-ractive-with-requirejs)。
- 在使用过程中你可以经常去官方的[API文档](http://docs.ractivejs.org/latest/api-reference)看看。

下篇文章接着来学ractivejs其它方面的特性。