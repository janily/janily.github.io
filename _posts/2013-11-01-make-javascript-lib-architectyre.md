---
layout: post
title: javascript学习打造javascript框架系列-对象、继承
categories:
- Life
tags:
- javascript
---

第一篇扯了下蛋，这篇正式开始我们的库的编码工作，别忘了我们库的名字是Alien。

### 一些参考资料 ###

正所谓站在巨人的肩膀上，能让我们看的更远。我们需要参考一下前辈们的代码，下面是一个参考库的一个列表：

- [jQuery](http://jquery.com/)
- [Prototype](http://prototypejs.org/)
- [MooTools](http://mootools.net/)
- [BBC Glow](http://www.bbc.co.uk/glow/)

### 编码风格 ###

如果你打算把你的这个库开源或者是跟别人一起使用你的库用于开发。那保持一个代码编写风格是非常重要的。

在我们的这个库中，我们约定使用以下的编码风格。

- 变量和函数方法名称：在这个库中为了使我们的变量和函数可读性更强和容易理解，我们尽量把名字起的详细，这样可能就有点冗长了，不过这是值得的。
- 简单精炼的注释
- 代码缩进采取缩进两个空格
- 为了应对压缩代码的情况，语句结尾写上分号
- 用JsLint来保证代码的质量
- 坚持测试
- 用GitHub来做版本管理

### 框架结构 ###

#### 初始化 ####

大部份框架在载入的时候，都会执行一个初始化。像MooTools和Prototype使用的初始化方法是一样的，而jQuery和Glow则是使用的另一种方法，如下所示：

    var MooTools = {
	  'version': '1.2.5dev',
	  'build': '%build%'
	};
	
	var Prototype = {
	  Version: '<%= PROTOTYPE_VERSION %>',
	  ...
	}
	
	(function( window, undefined ) {
	  var jQuery = function( selector, context ) {
	      // The jQuery object is actually just the init constructor 'enhanced'
	      return new jQuery.fn.init( selector, context );
	    },
	    ...
	    jquery: "@VERSION",
	    ...
	  }
	
	  // Expose jQuery to the global object
	  window.jQuery = window.$ = jQuery;
	})(window);

jQuery和Glow都是使用的一个匿名函数，下面具体来说一下它的作用。

- 这是一个自调用匿名函数。什么东东呢？在第一个括号内，创建一个匿名函数；第二个括号，立即执行。
- 为什么要创建这样一个“自调用匿名函数”呢？通过定义一个匿名函数，创建了一个“私有”的命名空间，该命名空间的变量和方法，不会破坏全局的命名空间。这点非常有用也是一个JS框架必须支持的功能，jQuery被应用在成千上万的JavaScript程序中，必须确保jQuery创建的变量不能和导入他的程序所使用的变量发生冲突。
-  匿名函数从语法上叫函数直接量。
-  为什么要传入window呢？通过传入window变量，使得window由全局变量变为局部变量，当在jQuery代码块中访问window时，不需要将作用域链回退到顶层作用域，这样可以更快的访问window；这还不是关键所在，更重要的是，将window作为参数传入，可以在压缩代码时进行优化。
-   为什么要在在参数列表中增加undefined呢？在自调用匿名函数 的作用域内，确保undefined是真的未定义。因为undefined能够被重写，赋予新的值。
- 注意到源码最后的分号了吗？分号是可选的，但省略分号并不是一个好的编程习惯；为了更好的兼容性和健壮性，请在每行代码后加上分号并养成习惯。

### 模块和插件 ###

jQuery，MooTools和Glow现在都支持模块化加载，按需调用。我们的这个库当然也得支持，我们把我们的文件命名如下：

- alien.core.js
- alien.function.js

我们将创建一个*alien*的变量暴露给全局的作用域，这样我们就可以定义模块为函数或者是对象。

### 正式开始编码 ###

我们这里的代码风格参考了jQuery的写法，先在*alien.core.js*文件里写上如下代码：

    (function(global) {
	  var alien = {
	    VERSION: '0.0.1',
	    lesson: 'Part 1: Library Architecture'
	  };
	
	  if (global.alien) {
	    throw new Error('alien has already been defined');
	  } else {
	    global.alien = alien;
	  }
	})(typeof window === 'undefined' ? this : window);

### 类、继承、扩展 ###

在javascript中是没有类这一特性。当然在一些第三方的库中扩展了javascript的功能，提供了类这一特性，但并不是所有的库提供这一功能。关于javascript中的继承和类的，[ Douglas Crockford ](http://javascript.crockford.com/)大牛的两篇文章一定得读一读，地址在这里[Classical Inheritance in JavaScript ](http://javascript.crockford.com/inheritance.html)和[ Prototypal Inheritance in JavaScript ](http://javascript.crockford.com/prototypal.html)。

在面向对象程序设计中，类是一个核心的特性，基本上所有的支持面向的对象的语言中都是基于类来实现的。而在javascript中，却没有提供类这一实现面向对象的重要特性。

那为什么有些第三方的库会扩展javascript提供面向对象编程的功能呢？这就取决于这个库的作者了。有些人是有其它开发语言的编程经验的，从而会模仿实现他们所喜欢语言的面向对象的功能。比如Prototype受Ruby的影响，用基于类的特性来组织代码。

在本文中，我们讲对基于原型继承和面向对象编程进行一个简单的介绍，并且扩展javascript来实现类的特性和面向对象编程的功能。这也是我们alien.js的一个功能。

### 对象和类 vs 原型 类 ###

一切事物皆为对象，在一些编程语言把所有的都视为一个对象。这就意味着数字是一个对象，字符串是一个对象，实例化一个类是一个对象。关于类和对象之间的关系可是剪不断，理还乱，别是一番滋味在心头！有些语言直接就是把类当做一个对象来看待的，使用一些基本的对象模型来实现类。我们只要记住一点的是，面向对象编程不是面向类编程。

难道javascript真的需要实现一个类的特性吗？如果你是一个Java或者是Ruby开发者，你可能会发现在javascript的语法中没有*class*这个关键字，当然，如果我们真的需要的话，也是可以实现的。

### 原型继承实现javascript的面向对象 ###

首先我们来看一段代码，如下：

    function Vector(x, y) {
	  this.x = x;
	  this.y = y;
	}
	
	Vector.prototype.toString = function() {
	  return 'x: ' + this.x + ', y: ' + this.y;
	}
	
	v = new Vector(1, 2);
	// x: 1, y: 2 

如果你还没适应javascript面向对象的编写方法，那么上面的代码可能看起来怪怪的。这里我们定义了一个名为*Vector*的函数，然后实例化了一个*Vector*。New后面的标识符是对象的constructor，任意一个javascript类都可以这样创建这样一个实例。例如上面，的代码创建了一个*Vector*类，然后实例化一个*Vector*对象。但是这里的*this*是指向我们实例化后的新对象也就是*v*。

这里的*prototype*是javascript语言的一个特性，"原型"。简单的说，所有js对象都有一个私有的原型属性(_proto_)。这个”私有”意味着这个属性不可见或者只有对象本身可以访问。当对对象执行查找某个属性的时候，它首先查找实例里的是否存在这个属性，如果没有，就在对象的”私有”原型上查找。原型可以有他们自己的私有原型，所以查询可以沿着原型链继续执行，直到找到属性或者当到达空的私有原型里，依然没有找到。

而且当你在原型里新添加一个方法的时候，就相当于给*vector*新增了一个方法，有没有感觉到很神奇！

    Vector.prototype.add = function(vector) {
	  this.x += vector.x;
	  this.y += vector.y;
	  return this;
	}
	
	v.add(new Vector(5, 5));
	// x: 6, y: 7

### 基于原型的继承 ###

首先在javascript中，关于继承并没有一个标准的模式。那如果我们新定义一个*Point*类，并且要继承*vector*的一些属性方法，就可以像下面这么做：

    function Point(x, y, colour) {
	  Vector.apply(this, arguments);
	  this.colour = colour;
	}
	
	Point.prototype = new Vector;
	Point.prototype.constructor = Point;
	
	p = new Point(1, 2, 'red');
	p.colour;
	// red
	p.x;
	// 1

这里使用了*apply*这个关键字，这样*point*会指向*vector*的构造函数。apply() 方法有两个参数，用作this的对象和要传递给函数的参数的数组。

当我们创建一个新的对象的时候，我们可以从*object*继承一些方法。比如*toString*和*hasOwnProperty*：

    p.hasOwnProperty('colour');
	// true

### 原型和类 ###

关于原型继承也有很多的模式。正因为这样，我们可以很灵活的运用它来处理我们的继承。当然最重要的是我们的API要足够的简单和易用，可读性强。

接下来我们看看基于类和原型的继承模式

我们来看看**Prototype**是怎么来处理继承的：

    Vector = Class.create({
	  initialize: function(x, y) {
	    this.x = x;
	    this.y = y;
	  },
	
	  toString: function() {
	    return 'x: ' + this.x + ', y: ' + this.y;
	  }
	});
	
	Point = Class.create(Vector, {
	  initialize: function($super, x, y, colour) {
	    $super(x, y);
	    this.colour = colour;
	  }
	});

当然，我们需要简化一下这个版本，我们需要做到下面几点：

- 通过复制它们来扩展类的功能和方法
- creation这个方法使用*apply*和*prototype.constructor*来运行构造函数
- 能够确定一个父类是否是继承


### 扩展 ###

在javascript中对对象扩展是一个常用的功能。举一个例子，由于javascript 没有重载（overload），而且函数的参数类型是没有定义的，所以很多时候我们都传入一个对象来作为参数已方便控制。通常在函数里面给了参数对象的默认值，这个时候就需要通过extend来把传入的参数覆盖进默认参数。

那在javascript中实现这一个功能其实也很简单。就想下面这样：

    for (var property in source)
  	destination[property] = source[property];

### Alien中继承 ###

*create*方法将创建一个新的类。当然它也需要一些参数设置来实现继承，就想上面的那个例子一样。

    // This would be defined in our "oo" namespace
	create: function(methods) {
	  var klass = function() { this.initialize.apply(this, arguments); };
	
	  // Copy the passed in methods
	  extend(klass.prototype, methods);
	
	  // Set the constructor
	  klass.prototype.constructor = klass;
	
	  // If there's no initialize method, set an empty one
	  if (!klass.prototype.initialize)
	    klass.prototype.initialize = function(){};
	
	  return klass;
	}

最终的代码在我会传到github上面，可以去这个[地址](https://github.com/janily/Alien/blob/master/alien.oo.js)看详细的代码。


