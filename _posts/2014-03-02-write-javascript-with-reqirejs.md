---
layout: post
title: 	使用reqirejs来模块化编写你的javascript代码
categories:
- Life
tags:
- javascript
- requirejs
---

> 现在web开发中，模块化编程越来越流行。正所谓，没有无缘无故的爱，也没有无缘无故的恨。不管是团队合作，还是代码质量的提升，都能感受到模块化编程带来的好处。今天我们就来学习下在javascript编程中使用requirejs来实现js模块编程的使用方法。这篇文章来自sitepoint的网站[Understanding RequireJS for Effective JavaScript Module Loading](http://www.sitepoint.com/understanding-requirejs-for-effective-javascript-module-loading/)，具体翻译有删减。

模块化编程即是把大型应用程序的代码分成不同的模块来管理代码的一种编程思想。基于模块化编程思想来编写代码能够使我们的代码组织清晰明了和便于团队合作开发。不过在编写模块化代码的时候，开发者不得不花精力来关注和管理模块之间的依赖关系。而RequireJS就是一个非常流行的用来管理模块之间依赖关系的一个库。本文就来学习使用RequireJS来进行模块化编程。

### **从加载js文件说起** ###

通常在一个稍微大型的web应用程序中一般都会加载若干的js文件。一般浏览器会按照**script**标签的顺序一个一个来加载js文件。而且js文件之间都有依赖关系。比如一个常见的情景，就是我们使用jQuery插件的时候，这些插件都需要依赖jQuery文件才能正常运行。下面我们来看一个简单的实例。假设我们有三个javascript文件。

第一个**purchase.js**文件：

    function purchaseProduct(){
	  console.log("Function : purchaseProduct");
	 
	  var credits = getCredits();
	  if(credits > 0){
	    reserveProduct();
	    return true;
	  }
	  return false;
	}

第二个product.js文件：

    function reserveProduct(){
	  console.log("Function : reserveProduct");
	 
	  return true;
	}

第三个credits.js文件：

    function getCredits(){
	  console.log("Function : getCredits");
	 
	  var credits = "100";
	  return credits;
	}

在上面这个例子中，如果我们要执行购买一个产品的程序。首先我们要检查用户是否有足够的积分去购买产品。检查通过后，才能预订购买产品，最后在**main.js**文件中再来执行**purchaseProduct()**产品。

    var result = purchaseProduct();

### **由此引发的问题** ###

在这个例子中，**purchase.js**文件依赖于**credits.js**和**product.js**这两个文件。因此，在执行**purchaseProduct()**之前需要加载上面两个js文件。如果我们像下面这样来加载js文件会引发怎么样的问题呢？

    <script src="products.js"></script>
	<script src="purchase.js"></script>
	<script src="main.js"></script>
	<script src="credits.js"></script>

这里在还没有加载**credits.js**文件之前就执行了**purchaseProduct()**方法。这样结果就会报错，这个例子还好只是加载了三个javascript文件。如果在大型的web应用程序中，后果可想而知。结果如下图所示：

![](http://pic.yupoo.com/reicky_v/DzJuzL3i/42f2r.png)

### **RequireJS** ###

RequireJS是一个流行的并且支持主流浏览器的模块化管理和加载以来关系的javascript库。在RequireJS中，我们可以把代码分成很多单个的模块来进行管理，按需加载。此外我们还可以设置文件之间的依赖关系。

首先来下载[RequireJS](http://requirejs.org/docs/download.html)文件。然后把下载好的文件复制到项目目录中。我们的项目结构可以按照下面这张图片来设置。

![](http://pic.yupoo.com/reicky_v/DzJx4EKi/IJuQ0.png)

所有的js文件都放在**scripts**文件夹里面，**main.js**文件使用初始化和编写其它程序逻辑代码的。让我们来看看在HTML文件中是怎么样来加载js文件的。

    <script data-main="scripts/main" src="scripts/require.js"></script>

我们只需要在HTML文件中引入RequireJS文件。你可能会想那其它文件是怎么样来加载的呢？在script标签中我们定义了一个**data-main**的属性，这个属性就是用来指定需要加载初始化应用程序文件的。我们这里指定的是**main.js**这个文件。RequireJS就会执行**main.js**文件并且搜索需要加载的其它的js文件和分析它们之间的依赖关系。当然在我们上面的这个例子中所有的javascript文件都放在一个文件夹中。你也可以按照自己的需求来组织自己的代码。现在来看看在**main.js**文件中的代码：

    require(["purchase"],function(purchase){
	  purchase.purchaseProduct();
	});

在RequireJS中，代码都需要放在**require()**或者是**define()**这两个函数中。第一个参数是需要依赖的文件。在上面这个例子中，初始化代码**purchaseProduct()**这个方法需要依赖**purchase.js**这个文件。注意我们这里省略了文件的扩展名。因为RequireJS文件只认**.js**结尾的文件。

第二个参数是一个匿名函数，主要用来处理和依赖文件有关的程序代码。在我们这个例子中，我们只需要依赖一个文件。当然如果碰到多个依赖文件的情况，我们可以这样来写：

    require(["a","b","c"],function(a,b,c){
	});

### **使用Requirejs来编写应用程序** ###

在这一个部分我们通过一个实例来学习在具体的项目中，如何使用RequireJS来组织编写我们的代码。我们已经编写好了**main.js**的代码，来看看其它文件。

**purchase.js**文件：

    define(["credits","products"], function(credits,products) {
 
	  console.log("Function : purchaseProduct");
	 
	  return {
	    purchaseProduct: function() {
	 
	      var credit = credits.getCredits();
	      if(credit > 0){
	        products.reserveProduct();
	        return true;
	      }
	      return false;
	    }
	  }
	});

首先，我们声明了purchase方法需要依赖的文件为credits和products。在**return**这个代码里，我们定义了每一个模块的函数方法。这里我们调用的了**getCredits()**和**reserveProduct()**方法并把它传递给相关的对象。**products.js**和**credits.js**文件的代码都差不多，如下所示：

**products.js**文件：

    define(function(products) {
	  return {
	    reserveProduct: function() {
	      console.log("Function : reserveProduct");
	 
	      return true;
	    }
	  }
	});

**credits.js**文件：

    define(function() {
	  console.log("Function : getCredits");
	 
	  return {
	    getCredits: function() {
	      var credits = "100";
	      return credits;
	    }
	  }
	});

这两个文件是独立的模块——意味着它们不依赖其它文件。这里需要注意的一点的是使用**define()**代替了**require()**方法，下面就来说说这两者之间的一些事儿。

### **require()**和**define()** ###

在上面没有使用requirejs的例子中，如果加载文件的顺序有错误浏览器就会提示相关的错误信息。现在我们在使用了requirejs的例子中来删除**credits.js**然后运行浏览器看看。我们会看到下面的提示信息：

![](http://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2012/12/requirejs-03.png)

在使用RequireJS中，如果有依赖模块文件没有加载成功，它不会执行任何代码。因为RequireJS会等待所有的模块文件加载完才会执行主程序代码。如果出现模块加载失败的情形，我们的程序也不会在数据上发生崩溃。

### **管理和加载文件** ###

RequireJS是使用了AMD规范来加载文件的。每个模块文件都是通过异步的方式来加载的。所以我们不能确保依赖文件能够按照正确顺序来加载文件。所以，RequireJS允许我们使用**shim**来设置和定义文件的加载顺序。那怎么使用呢？

    requirejs.config({
	  shim: {
	    'source1': ['dependency1','dependency2'],
	    'source2': ['source1']
	  }
	});

RequireJS允许我们通过**config()**方法来进行相关的配置定义。比如我们可以通过**shim**这个参数来定义依赖文件的加载顺序。可以去[官方的文档](http://requirejs.org/docs/api.html#config)看详细的使用方法。

    define(["dependency1","dependency2","source1","source2"], function() {
 
	);

一般如果使用上面的方法来加载依赖文件。这里**source2**依赖于**source1**，如果**source1**文件加载完毕，**source2**文件就会认为所有的依赖文件都加载完了。不过**dependency1**和**dependency2**有可能还没有加载完。而使用**shim**来配置文件的加载顺序，就不需要担心这个问题了。

OK，我希望本文能够帮助你对于RequireJS有一个好的开始。虽然看起来好像非常简单，不过对于一个大型的web程序来说，使用RequireJS来编写代码确实能够带来非常多的好处。当然这篇文章只是一个简单的介绍，并没有覆盖RequireJS全部的特征，不过你可以去它的[官网](http://requirejs.org/)查看更多的信息。












