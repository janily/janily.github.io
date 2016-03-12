---
layout: post
title: javascript学习打造javascript框架系列-函数式编程
categories:
- Life
tags:
- javascript
---

### 函数式编程 ###

啥为函数式编程，下面这段话是引用了，阮老师博客的[一篇文章](http://www.ruanyifeng.com/blog/2012/04/functional_programming.html)。

[函数式编程（functional programming）](http://en.wikipedia.org/wiki/Functional_programming)开始获得越来越多的关注。

但是，"函数式编程"看上去比较难，缺乏通俗的入门教程，各种介绍文章都充斥着数学符号和专用术语，让人读了如坠云雾。就连最基本的问题"什么是函数式编程"，网上都搜不到易懂的回答。

不仅最古老的函数式语言Lisp重获青春，而且新的函数式语言层出不穷，比如Erlang、clojure、Scala、F#等等。目前最当红的Python、Ruby、Javascript，对函数式编程的支持都很强，就连老牌的面向对象的Java、面向过程的PHP，都忙不迭地加入对匿名函数的支持。越来越多的迹象表明，函数式编程已经不再是学术界的最爱，开始大踏步地在业界投入实用。

简单说，"函数式编程"是一种"编程范式"（programming paradigm），也就是如何编写程序的方法论。

它属于"结构化编程"的一种，主要思想是把运算过程尽量写成一系列嵌套的函数调用。举例来说，现在有这样一个数学表达式：

    　(1 + 2) * 3 - 4

传统的过程式编程，可能这样写：

    var a = 1 + 2;
　　 var b = a * 3;
　　 var c = b - 4;

函数式编程要求使用函数，我们可以把运算过程定义为不同的函数，然后写成下面这样：

    var result = subtract(multiply(add(1,2), 3), 4);

这就是函数式编程。

Javascript第三方的库大部分都提供了*each*方法来处理迭代循环这方面的功能。

比如一般我们在javascript中是这样处理的：

    Array.prototype.each = function(callback) {
	  for (var i = 0; i < this.length; i++) {
	    callback(this[i]);
	  }
	}
	
	[1, 2, 3].each(function(number) {
	  print(number);
	});

实际上，在Javascript中本身就有* Array.forEach*，*Array.prototype.forEach*，*for (var i in objectWithIterator) *等等这些方法来处理迭代循环的需求。那为什么第三方的库还要提供处理迭代循环的方法呢？比如你看看在[jQuery.each](http://api.jquery.com/jQuery.each/)和[each in Prototype](http://www.prototypejs.org/api/enumerable/each)，是因为各个浏览器对这些方法有兼容性问题。

我们来看看[underscore](http://github.com/documentcloud/underscore)是怎么做的：

    / The cornerstone, an each implementation.  
	// Handles objects implementing forEach, arrays, and raw objects.  
	// Delegates to JavaScript 1.6's native forEach if available.  
	var each = _.forEach = function(obj, iterator, context) {  
	  try {  
	    if (nativeForEach && obj.forEach === nativeForEach) {  
	      obj.forEach(iterator, context);  
	    } else if (_.isNumber(obj.length)) {  
	      for (var i = 0, l = obj.length; i < l; i++) iterator.call(context, obj[i], i, obj);  
	    } else {  
	      for (var key in obj) {  
	        if (hasOwnProperty.call(obj, key)) iterator.call(context, obj[key], key, obj);  
	      }  
	    }  
	  } catch(e) {  
	    if (e != breaker) throw e;  
	  }  
	  return obj;  
	};  

上述方法使用了js本身的一些特性来实现迭代，而不是继承类似Enumerable这样的类来实现迭代。

### API设计 ###

API设计对于函数式编程是一个重要特征。Prototype利用修改*object*和*Array*的原型来实现迭代循环的。虽然这样做使用起来非常简单，但是如果要跟其它库一起使用的话，不是非常容易的。所以这样做不是非常妥当。

而[underscore](http://github.com/documentcloud/underscore)对于迭代有一个非常清晰的命名空间设计，你可以使用函数式编程的方法来使用它，也可以使用面向对象的方法来使用它。

我们alien库的迭代也参考了[underscore](http://github.com/documentcloud/underscore)的实际方法来实现，使用方法如下：

    alien.enumerable.each([1, 2, 3], function(number) { number + 1; });
	alien.enumerable.map([1, 2, 3], function(number) { return number + 1; });

	// 2, 3, 4

上面说完了迭代的方法的设计，下面再来说说其它一些常见方法的实现。

### 过滤 ###

过滤功能能够使我们过滤掉不符合条件的一些元素。比如：

    alien.enumerable.filter([1, 2, 3, 4, 5, 6], function(n) { return n % 2 == 0; });
	// 2,4,6

我们这里的过滤会执行下面这一个过程：

- 首先检查有没有原生就支持的过滤功能，如果没有就是用我们自己写的过滤功能。
- 是用*alien.prolate.each*。
- 还可以把对象过滤到多维数组里。

### 元素检查 ###

元素检查跟过滤是不一样的，因为在ECMAScript标准中并没有提供这个方法。

它非常容易使用，就是检测一个数组里面有没有符合条件的元素，如下：

    alien.enumerable.detect(['bob', 'sam', 'bill'], function(name) { return name === 'bob'; });
	// bob

这个方法也是使用到了*each*方法。具体可以看代码：

    detect: function(enumerable, callback, context) {
        var result;
        global.enumerable.each(enumerable, function(value, index, list) {
          if (callback.call(context, value, index, list)) {
            result = value;
            throw global.enumerable.Break;
          }
        });
        return result;
      }

下面这一个功能是链式调用功能。

### 链式调用 ###

为了使我们的alien库更有好的面对开发者，支持链式调用是必要的：

    turing.enumerable.chain([1, 2, 3, 4]).filter(function(n) { return n % 2 == 0; }).map(function(n) { return n * 10; }).values();

链式调用就是调用对象的方法后返回到该对象，严格来讲它并不属于语法，而只是一种语法技巧。如果你使用过jQuery就知道链式调用的确非常方便，而且很容易上瘾。比如：

    .chain([1, 2, 3, 4])                         // 从数组开始
	.filter(function(n) { return n % 2 == 0; })  // 过滤掉数组中的奇数
	.map(function(n) { return n * 10; })         // 把剩下的数字每一个乘以10
	.values();                                   // 计算结果

实现链式的原理主要是：

- 保存一个临时变量
- 然后把我们要处理的数据映射到临时变量
- 运行完某个方法，返回this用来调用后面的方法
- 所有方法调用完成后返回结果

    // store temporary values in this.results
	alien.prolate.Chainer = turing.Class({
	  initialize: function(values) {
	    this.results = values;
	  },
	
	  values: function() {
	    return this.results;
	  }
	});
	
	// Map selected methods by wrapping them in a closure that returns this each time
	alien.prolate.each(['map', 'detect', 'filter'], function(methodName) {
	  var method = turing.enumerable[methodName];
	  alien.prolate.Chainer.prototype[methodName] = function() {
	    var args = Array.prototype.slice.call(arguments);
	    args.unshift(this.results);
	    this.results = method.apply(this, args);
	    return this;
	  }
	});

OK，通过本文现在我们可以知道怎么去处理一下问题了：

- 怎么去检查原生是否有我们需要的方法
- 如果没有可用*each*方法来实现
- 使用闭包的特性来实现链式的调用
- 使用一个命名空间来设计API

当然，我们完全按照自己的需求来扩展更多的方法，你可以去[underscore](http://github.com/documentcloud/underscore)看看，它都扩展了哪些方法。

具体的代码可以去这里看，[alien.prolate.js](https://github.com/janily/Alien/blob/master/alien.prolate.js)。

