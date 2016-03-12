---
layout: post
title: javascript学习打造javascript框架系列-alien库的面向对象
categories:
- Life
tags:
- javascript
---

上一篇文章说了下关于javascript继承和面向对象方面的话题。主要是关于基于原型继承方面的话题。最后创建了Alien库的一个关于面向对象的一个方法库*alien.oo.js*文件，这篇文章主要是来说说文件中具体的代码。

我们来详细看一下**Creation**这个方法中具体的代码。

    create: function() {
    var methods = null,
        parent  = undefined,
        klass   = function() {
          this.initialize.apply(this, arguments);
        };

这个方法主要是用来创建类的。其中*initialize*，这个方法是在我们用*new*关键字实例化一个类的时候，就会执行*initialize*方法。*initialize*只不过是个变量，代表一个方法，用途是类的构造函数。当使用这个构造函数来构造对象的时候，会让构造出来的这个对象的initialize变量执行apply()方法。

下面来说说apply()这个方法，提到它必然也要提到call()方法。

call()和apply()的功能。功能基本一样，function().call(object,{},{}……)或者function().apply (object,[……])的功能就是对象object调用这里的funciton()，不同之处是call参数从第二个开始都是传递给funciton的，可以依次罗列用“，”隔开。而apply只有两个参数，第二个是一个数组，其中存储了所有传递给function的参数。 

下面我们来看看怎么运用我们的*create*方法，进而解释下this.initialize.apply(this, arguments);这代码的具体含义。

    // Class使用方法如下 
	var A = Class.create(); 
	A. prototype={ 
	initialize:function(v){ 
	this .value=v; 
	} 
	showValue:function(){ 
	alert(this.value); 
	} 
	} 
	var a = new A(‘helloWord!'); 
	a. showValue();//弹出对话框helloWord！ 

那上面的**this.initialize.apply(this, arguments);**是什么意思呢？

这里的第一个this，是指用new调用构造函数之后生成的对象，也就是前面的a，那么第二个this也当然应该是指同一个对象。那这句话就是this（也就是a）调用initialize方法，参数是arguments对象（参数的数组对象），所以在构造函数执行的时候，对象a就会去执行initialize方法来初始化，这样就和单词“initialize”的意思对上了。 

那么执行initialize方法的参数怎么传递进去的呢？ 

这里就要说说*Arguments*了

看下面这一段代码：

    function test(){ 
	alert(typeof arguments); 
	for(var i=0; i<arguments.length; i++){ 
	alert(arguments[i]); 
	} 
	} 
	test("1","2","3"); 
	test("a","b"); 

执行后alert(typeof arguments);会显示object，说明arguments是对象。然后会依次打出1、2、3。说明arguments就是调用函数的实参数组。 

arguments 就是create返回的构造函数的实参数组，那么在var a = new A(‘helloWord!'); 的时候‘helloWord!'就是实参数组（虽然只有一个字符串），传递给方法apply，然后在调用initialize 的时候作为参数传递给初始化函数initialize。 

### Mixin、extended和super ###

接下来我们看看Mixin、extended和super这三个方法。

主要是用于对象继承扩展功能用的。

我们首先来看看*Mixin*方法。

    mixin: function(klass, methods) {
      if (typeof methods.include !== 'undefined') {
        if (typeof methods.include === 'function') {
          oo.extend(klass.prototype, methods.include.prototype);
        } else {
          for (var i = 0; i < methods.include.length; i++) {
            oo.extend(klass.prototype, methods.include[i].prototype);
          }
        }
      }
    }

主要是判断看传入methods的include是否定义定义了是函数就扩展入klass，否则就作为数组处理，将数组内的定义扩展入klass。

*extended*方法是用于来处理对象的继承的。

    extend: function(destination, source) {
      for (var property in source)
        destination[property] = source[property];
      return destination;
    }

这个方法主要是是通过for in把父类的原型成员逐一赋给子类的原型。从而实现继承。

接下来是$super这个参数的用法。

    $super: function(parentClass, instance, method, args) {
      return parentClass[method].apply(instance, args);
    }

$super参数是替换成父类的同名方法，这样在子类调用$super()时，将调用父类的同名方法。这个方法是参考了*Prototype.js*库中的一个写法。

这个关键字可以作为要扩展方法的第一个参数出现，一但要扩展的方法出现了这个关键字，那么这个方法内部就可以通过$super来取得类上与之同名的方法的引用。比如：

    //声明Person类，并定义初始化方法 
	var Person = Class.create({ 
	initialize: function(name) { 
	this.name = name; 
	}, 
	say: function(message) { 
	return this.name + ': ' + message; 
	} 
	}); 
	
	var Pirate = Class.create(Person, { 
	// redefine the speak method 
	//注意这里的$super用法
	say: function($super, message) { 
	return $super(message) + ', yarr!'; 
	} 
	}); 
	
	var john = new Pirate('Long John'); 
	john.say('ahoy matey'); 
	// -> "Long John: ahoy matey, yarr!" 

通过两篇文章，我们实现扩展了javascript在面向对象和继承方面的功能。



