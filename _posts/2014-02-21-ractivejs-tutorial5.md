---
layout: post
title: 	ractive.js学习笔记(5)——事件处理
categories:
- Life
tags:
- javascript
- ractive.js
---

事件是web交互中的一个基石。在开发中绑定事件的几乎是不可缺少的一部分：

    element.addEventListener('click',hander)
	或者是
	$('#button').on('click',hander)

上面的代码相信在web开发中随处可见。而在Ractive.js一般这样来处理事件：

    <button on-click='active')Active!</button>

或者是像下面这样：

    ractive.on('active',funtion(event){

		alert('Activating!');
	});

非常方便，你不需要再像以前那样要依靠**id**或者是**class**来寻找DOM元素然后再进行事件绑定。新建一个event.html文件输入以下代码：

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
	        <h2>City profile</h2>
	
	        <button on-click='activate'>Activate!</button>
	    </script>
	
	    <script src='ractive.js'></script>
		<script>
	        var ractive = new Ractive({
	          el: 'container',
	          template: '#myTemplate'
	        });
	
	        ractive.on( 'activate', function ( event ) {
	          alert( 'Activating!' );
	        });
	    </script>
	</body>
	</html>

在浏览器中运行这个文件点击按钮，就会看到弹出一个对话框证明绑定的事件成功运行了，如下图所示：

![](http://pic.yupoo.com/reicky_v/DymHIcLO/medium.jpg)

如果你使用开发者工具来查看元素，你会发现解析出来的模板中的按钮是没有**on-click**这个属性的。当Ractive.js解析模板的时候，它会自动处理像**on-**这样开头的属性，下面我们来进一步学习在ractive中的事件处理。

### 多个事件处理 ###

有时候在开发中可能要一并处理多个事件，这在ractive中也很容易办到，只需要在上面的事件绑定的代码中修改一下代码：

    ractive.on({
	  activate: function () {
	    alert( 'Activating!' );
	  },
	  deactivate: function () {
	    alert( 'Deactivating!' );
	  }
	});

这样就可以处理多个事件了。

### 解除事件 ###

解除事件也有很多方法，如果你使用jQuery开发的话，你对下面的代码应该很熟悉：

    ractive.on('select',selectHandler);
	ractive.off('select',selectHandler);

如果你定义了相关的事件的处理函数，就可以使用上面的代码来灵活处理事件。如果你使用了匿名函数来处理事件，你也可以使用下面的代码：

    ractive.off('select')  会解除所有跟select相关的事件
	ractive.off()  会解除任何事件

或者你也可以像下面这样来进行事件处理：

	var listener = ractive.on('select',selectHandler);

	var otherlisteners = ractive.on({
		active:function() { alert('Activating!'); },
		deactivate:function() {alert('Deactivating!');}
	});

	//解除事件
	listener.cancel();
	otherListeners.cancel();

下面我们来更新下我们的event.html文件，代码如下：

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
        <h2>City profile</h2>

        <button on-click='activate'>Activate!</button>
        <button on-click='deactivate'>Deactivate!</button>
        <button on-click='silence'>Silence!</button>
    </script>

    <script src='ractive.js'></script>
	<script>
        var ractive = new Ractive({
          el: 'container',
          template: '#myTemplate'
        });

        listener = ractive.on({
          activate: function () {
            alert( 'Activating!' );
          },
          deactivate: function () {
            alert( 'Deactivating!' );
          },
          silence: function () {
            alert( 'No more alerts!' );
            listener.cancel();
          }
        });
    </script>
	</body>
	</html>

> 这里还需要注意一下的是**ractive.teardowm**这个方法。它会解除跟**ractive.on()**相关的所有的事件。会直接删除视图中的DOM结构。

### 自定义事件 ###

在ractive.js中使用自定义的事件跟使用常规的事件是一样的，如：

    <button on-tap='activate'>Activate!</button>

我们这里就是使用自定义的**on-tap**事件。

> 在Ractive中自定义事件是以插件的形式存在的，可以去[这个地址](https://github.com/RactiveJS/Ractive/wiki/Plugins)看看ractive提供的一些插件，当然你也可以创建自己的事件——可以使用Ractive的一个[模板文件来创建](https://github.com/RactiveJS/Ractive/wiki/Plugin-APIs)。

在移动设别上使用**click**事件不是一个很好的选择，因为在移动设备上的交互方式跟PC上的交互方式是迥然不同的，使用click会引起很多问题，对用户体验的影响也是很大的。

而且在移动设备上，一般都是使用触摸的方式来处理交互的，使用**click**事件的话还有一个300毫秒的延迟现象。

而**tap**事件则能很好的解决click带来的问题。试着在模板中把click改为tap。

    <button on-tap='activate'>Activate!</button>
	<button on-tap='deactivate'>Deactivate!</button>
	<button on-tap='silence'>Silence!</button>

### 事件委托 ###

在遇到大量事件处理的时候，一般都会选择事件委托来处理。 事件委托就是在一个页面上使用一个事件来管理多种类型的事件。这并不是一个新的想法，但对于把握性能来说却很重要。

事件委托是事件冒泡的一个应用，可以减少绑定元素的个数，也不必担心子节点被替换后可能需要进行重新的事件绑定。因为事件的捕获和后续代码的执行已经完全委托给了其父节点。如果页面中含有大量元素需要绑定事件，这样做会减少事件绑定数量，为浏览器减负，无疑会提高页面性能。

在web开发中，事件委托是一个非常好的开发实践。不过在Ractive.js开发中事件委托不是必需的。在ractive中我们只需要为每一个元素创建一个事件代理，那么后面同样的元素都会自动绑定事件处理方法。

这比事件委托更加有效率。因为事件委托意味着需要在DOM树中执行大量的检测从而找到需要绑定事件处理的元素，这会花掉很多的资源。

具体的的例子可以去官方的这个[地址看看](http://learn.ractivejs.org/event-proxies/5/)。






    



