---
layout: post
title: javascript学习打造javascript框架系列-事件
categories:
- Life
tags:
- javascript
---

在javascript整个语言核心中，可以说事件是一个基石，无时无刻都在与事件打交道，你能想象在web世界中没有事件交互响应的情况吗？事件和javascript形影不离，可以说只要用到javascript，事件也一定会出现，比如：

    <a href="/" onclick="alert('Hello World!')">

像上面这种写在行内的时间处理代码很常见。

也可以像下面这样写：

    element.onclick = function() { alert('Hello World!'); };

由于事件应用非常频繁，我们也可以把它封装成一个函数：

    function handler(event) {
	if (!event) var event = window.event;
	}

*window.event*是为了兼容浏览器的事件处理，像大多数的框架对于事件都会做兼容性的处理，比如jQuery是这样做的：

    handle: function( event ) {
	  var all, handlers, namespaces, namespace_sort = [], namespace_re, events, args = jQuery.makeArray( arguments );
	  event = args[0] = jQuery.event.fix( event || window.event );

有时候，我们可能也需要阻止一些默认事件的触发。可以这样做：

    element.onclick = function() { alert('Hello World!'); return false; };

在javascript事件中，还有事件冒泡和事件捕获两种需要注意的地方。

#### 事件冒泡 ####

在一个对象上触发某类事件（比如单击onclick事件），如果此对象定义了此事件的处理程序，那么此事件就会调用这个处理程序，如果没有定义此事件处理程序或者事件返回true，那么这个事件会向这个对象的父级对象传播，从里到外，直至它被处理（父级对象所有同类事件都将被激活），或者它到达了对象层次的最顶层，即document对象（有些浏览器是window）。

当然我们也可以阻止事件冒泡，由于不同浏览器对于事件冒泡的处理不同，我们也需要做兼容处理，如下：

     function stopBubble(e)  
	    {  
	        if (e && e.stopPropagation)  
	            e.stopPropagation()  
	        else 
	            window.event.cancelBubble=true 
	    }  

在IE中使用*cancelBubble*来阻止，其它浏览器用stopPropagation来阻止。

捕获阶段是一个和冒泡阶段完全相反的过程，即事件由祖先元素向子元素传播，和一个石子儿从水面向水底下沉一样，要说明的是在 IE，opera浏览器中，是不存在这个阶段的。从各浏览器提供的注册事件监听的方法中可见一斑，例如适用于ie,opera的attachEvent， 有两个参数，attachEvent(”on”+type,fn)，而适用于所谓标准浏览器的addEventListener则有三个参 数，addEventListener(type,fn,boolean)，前面两个参数不用解释，第三个参数boolean，就是决定注册事件发生在捕 获阶段还是冒泡阶段。

在大部分的框架中，都是用上面的方法来处理阻止事件冒泡处理的。

当然，如果事件的处理就是这么简单地话，那完全可以不用框架来处理事件了。

那我们的alien框架在事件处理部分要达到什么样的目的呢？

- 重新命名事件的名称，比如点击事件*onClick*改为*click*
- 简化事件的绑定与解除
- 兼容各个浏览器的事件处理
- 增强一些事件处理的功能
- 修复像	IE浏览器内存泄露问题

在jQuery中的事件处理是遵循W3C标准来处理事件的，可以去这个地址看看[jQuery’s event handling](http://api.jquery.com/category/events/event-object/)。

在jQuery中，事件非常容易使用，比如：

    $('a').click(function(event) {
	  alert('A link has been clicked');
	});

- 如果你要解除一个事件，那么可以用* $('a').unbind('click');*
- 阻止一个元素的默认事件用* $('a').unbind('click');*
- 阻止事件冒泡用* $('a').unbind('click');*
- 可以用* $('a').trigger('click');*根据绑定到匹配元素的给定的事件类型执行所有的处理程序和行为。

下面我们来看看其它一些框架对事件是怎样处理的。

### Prototype ###

[Prototype’s event handling](http://api.prototypejs.org/dom/event/)是这样处理事件的：

    $('id').observe('click', function(event) {
	  var element = Event.element(event);
	});

- 可以通过*Event.element(event) *或 *event.element();*来访问绑定事件的元素
- 可以用*Event.stop()*来阻止事件
- 用* Event.stopObserving(element, eventName, handler);*可以解除事件绑定
- *Event.fire(element)*根据绑定到匹配元素的给定的事件类型执行所有的处理程序和行为

### Glow ###

在[ glow.events](http://www.bbc.co.uk/glow/docs/1.7/api/glow.events.shtml)中是通过*addListener*来处理事件的：

    glow.events.addListener('a', 'click', function(event) {
	  alert('Hello World!');
	});

- 用*event.source*可以访问事件触发的元素
- 可以返回一个*false*值来阻止默认事件
- 用*glow.events.removeListener(handler)*解除事件
- * glow.events.fire()*用于绑定到匹配元素的给定的事件类型执行所有的处理程序和行为

### Dojo ###

在Dojo中是这样处理事件的：

    dojo.connect(dojo.byId('a#hello'), 'onclick', function(event) {
	  alert('Hello World!');
	});

- *event.target*指向事件处理目标元素
- *event.preventDefault()*阻止事件默认事件
- * event.stopPropagation()*阻止事件冒泡
- *dojo.disconnect()*解除事件

看完一些框架的事件处理之后，jQuery的设计尤其令人印象深刻。jQuery能横扫DOM世界可不是盖的。

接下来就是来打造我们的Alien框架的事件处理的部分了，主要有以下几个特点：


- 重新命名事件的名称，比如点击事件*onClick*改为*click*
- 简化事件的绑定与解除
- 兼容各个浏览器的事件处理
- 增强一些事件处理的功能
- 修复像	IE浏览器内存泄露问题

首先来设计我们Alien库的阻止默认事件的API。

我们这里是参考jQuery来设计的，因为jQuery的实现方法是遵循W3C标准来设计的，不用担心浏览器兼容性的问题。

主要是支持以下几种方法：

*event.stop()*阻止默认事件和冒泡
*event.preventDefault*阻止默认事件
*event.preventPropagation*阻止事件的冒泡

针对IE浏览器，我们还要做一下兼容性处理：

    function stop(event) {
	  event.preventDefault(event);
	  event.stopPropagation(event);
	}
	
	function fix(event, element) {
	  if (!event) var event = window.event;
	
	  event.stop = function() { stop(event); };
	
	  if (typeof event.target === 'undefined')
	    event.target = event.srcElement || element;
	
	  if (!event.preventDefault)
	    event.preventDefault = function() { event.returnValue = false; };
	
	  if (!event.stopPropagation)
	    event.stopPropagation = function() { event.cancelBubble = true; };
	
	  return event;
	}

大多数的框架在处理浏览器的兼容性的问题的时候都采取了不同的方法，尤其是关于鼠标事件和键盘事件。

jQuery解决了以下的一些问题：

- Safari处理文本节点
- 关于*event.pageX/y*的值
- 纠正*event.wich*和*event.metaKey*,主要是利用event.which方法获取键盘输入值的代码

Prototype是这样解决的：

    var _isButton;
	if (Prototype.Browser.IE) {
	  // IE doesn't map left/right/middle the same way.
	  var buttonMap = { 0: 1, 1: 4, 2: 2 };
	  _isButton = function(event, code) {
	    return event.button === buttonMap[code];
	  };
	} else if (Prototype.Browser.WebKit) {
	  // In Safari we have to account for when the user holds down
	  // the "meta" key.
	  _isButton = function(event, code) {
	    switch (code) {
	      case 0: return event.which == 1 && !event.metaKey;
	      case 1: return event.which == 1 && event.metaKey;
	      default: return false;
	    }
	  };
	} else {
	  _isButton = function(event, code) {
	    return event.which ? (event.which === code + 1) : (event.button === code);
	  };
	}

具体的代码文件可以去github地址看，[alien.js](https://github.com/janily/Alien)。


