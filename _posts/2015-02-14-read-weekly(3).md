---
layout: post
title: javascript经典实例-读书周总结(3)
categories:
- Life
tags:
- javascript
- 读书

---

今天来总结下在看<<js经典实例>>一书中数组这一节中的一些内容。

在js中数组可以使用对象的方法或者是直接量的方法来初始化：

	var arrObject = new Array("val1","val2"); //对象
	var arrObeject = ["val1","val2"]; //直接量
	
一个数组，不管是对象还是直接量，都可以包含不同类型的数据：

	var arrObject = new Array("val1",34,true);
	var arrObject = new ["val1",34,true];
	
可以打印出一个数组，js引擎将会自动把数组转换为一个字符串。

可以直接访问数组元素，使用方括号包括它们的索引久行了。也可以直接使用索引值来设置数组元素，如果数组元素不存在的话，就会自动创建它。

如：

	var arrObject = new Array();
	arrObject[0] = "cat";
	alert(arrObject[0]); //打印出cat
	
在js中的数组的索引值是从0开始的，这意味着第一个元素的索引是从0开始，最后一个元素是数组的长度减去1:

	var frmAnimals = new Array("cat","dog","horse","pig");
	alert(frmAnimals[0]);
	alert(frmAnimals[3]);  //pig
	
来看看第一个数组常见的操作，循环遍历数组：

	var mammals = new Array("cat","dog","human","whale","seal");
	var animalString = "";
	
	for (var i = 0; i < mamals.length; i++) {
		animalString += mamals[i] + "";
	}
	
	alert(animalString);
	
有时候，你想访问一个特定的元素。可以使用while循环来达到这个目的：

	var numArray = (1,4,55,123,444,555);
	
	var i = 0;
	
	while(numArray[i] < 100) {
		alert(numArray[i++]);
	}
	
### 创建多维数组

具体代码看下面：

<a class="jsbin-embed" href="http://jsbin.com/cavupu/1/embed?js,console">test</a><script src="http://static.jsbin.com/js/embed.js"></script>

### 从数组中创建一个字符串

可以使用Array内建的join方法，把数组元素中加入一个字符串中。

如：

	var fruitArray = ["apple","bannan","lime"];
	
	var resultString = fruitArray.join('-');
	
### 排序数组

使用js中的sort方法来对数组进行排序：

	var fruitArray = ["apple","cc","bannan","lime"];
	console.log(fruit.sort()); //返回apple banana cc lime
	
如果没有提供任何比较函数的话，sort()方法会按照字母顺序排排序数组。为了便于排序，在排序之前，所有的数据类型都会转换为其对等的字符串：

	var numArray = [4,13,2,31,5];
	
	console.log(number.sort()) //返回 13，2，31，4，5
	
可以看到虽然是按照词法的顺序进行排序的，但我们要进行的是真正的数字排序，可以使用一个定制的sort函数：

	function compareNumber (a,b) {
		return a - b;
	}
	
	var numArray = [4,13,2,31,5];
	
	console.log(numberArray.sort(compareNumber)) //打印出2、4、5、13、31
	
这是按照升序来进行排序的，如果想要按照相反的顺序进行排序，可以先使用sort方法排序元素，然后使用reverse方法反转数组成员的顺序：

	var numArray = [4,5,2,1,3];
	numberArray.sort();
	numberArray.reverse() // 5,4,3,2,1
	
### 按照一定的顺序来存储和访问数组

要按照接受值的顺序来存储和访问值，得创建一个先进先出(FIFO)的队列。使用javascript array对象的push方法，像队列添加项，并且用shift来获取项。

shift方法从数组前面提取元素，并将其从数组中删除，返回该元素。而且数组计数会自减。

如果想以相反的顺序来访问值，先访问最近存储的值，即后进先出的栈。可以使用pop方法。

具体的代码如下所示：

<a class="jsbin-embed" href="http://jsbin.com/gaxeci/1/embed?js,console">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

### 创建一个新数组作为已有数组的子集

如果想要从已有到数组来创建一个新数组。如果该数组是对象，需要保持两个数组同步。

可以使用slice的方法，根据给定的范围内的元素来创建一个新数组：

<a class="jsbin-embed" href="http://jsbin.com/xipenu/1/embed?js,console">test</a><script src="http://static.jsbin.com/js/embed.js"></script>

slice方法是从另一个数组中的一段连续的元素来创建一个新数组的一种简单方法。参数是要复制元素序列的开始索引值和结束索引值。任何一个索引的负值，表示该slice将从数组的末尾开始。

### 在数组中搜索和删除

如果想在一个数组中搜索特定值，找到的话，获取该数组元素的索引值。

可以使用新的indexOf和lastIndexOf这个两个方法：

	var animals = new Array("dog","cat","elephant","lion");
	
	console.log(animals.indexOf("elephant")); //打印出3
	
如果没有找到值会返回－1。

配合splice方法，可以找到并删除或者是替换数组元素：

<a class="jsbin-embed" href="http://jsbin.com/xiciz/1/embed?js,console">test</a><script src="http://static.jsbin.com/js/embed.js"></script>

splice方法接受三个参数。第一个参数是必需的，它是进行拼接出的索引。另外两个参数是可选的，即删除到元素的数目及其一个替代元素。如果索引是负的，将从数组末尾开始，而不是从数组开头进行。

### 合并数组

将一个多维数组合并成一个单维数组。

可以使用到Array对象中的concat()方法。

如下所示：

<a class="jsbin-embed" href="http://jsbin.com/zovefa/1/embed?js,console">test</a><script src="http://static.jsbin.com/js/embed.js"></script>

### 对每个数组元素应用一个函数

如果想使用一个函数来检查一个数组值，满足给定的条件，就替换它。

这个时候可以使用新的ForEach方法，来针对每一个数组元素绑定一个回调函数：

来看下面的实例：

<a class="jsbin-embed" href="http://jsbin.com/nedeqo/1/embed?js,console">test</a><script src="http://static.jsbin.com/js/embed.js"></script>

ForEach方法接受一个参数，这个参数是一个函数。该函数自身有三个参数：数组元素，元素的索引和数组。所以这三个参数都用于该函数中，即*replaceElement*中。

首先，测试该元素的值，看它是否与一个给定的字符串ab匹配。如果匹配，使用数组元素的索引，用指定的字符串来修改该数组元素的值。

### 对数组中的每个元素执行一个函数并返回一个新数组

在新的js中，提供了一个map方法。map方法允许我们绑定一个回调函数，以应用于每一个数组元素。与ForEach不同，map方法的结果是一个新的数组，而不是修改原始数组。因此使用ForEach方法不必返回一个值，使用map方法必须返回一个值。

map方法有三个参数：当前的数组元素，该数组元素的索引以及数组。不过IE8对这两个方法都不支持。

来看一个实例：

<a class="jsbin-embed" href="http://jsbin.com/suhiza/1/embed?js,console">test</a><script src="http://static.jsbin.com/js/embed.js"></script>

### filter方法来过滤数组

过滤一个数组，并且把结果赋给一个新的数组。

可以使用Array对象中的filter方法：

实例如下：

<a class="jsbin-embed" href="http://jsbin.com/cutif/1/embed?js,console">test</a><script src="http://static.jsbin.com/js/embed.js"></script>

上面就是数组一些常见应用场景及解决方案。




	


