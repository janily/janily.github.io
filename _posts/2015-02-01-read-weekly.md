---
layout: post
title: javascript经典实例-读书周总结
categories:
- Life
tags:
- javascript
- 读书

---

2015年制定了几个行动计划，其中读书是行动计划之一，会利用每天晚上阅读一些书籍，每周写篇阅读的总结。

最近这段时间在读<<javascript经典实例>>，今天来看看关于正则表达式的一些应用场景和解决方案。

正则表达式是被用来匹配字符串中的字符组合的模式。在JavaScript中，正则表达式也是对象。这种模式可以被用于RegExp的exec和test方法以及 String的match、replace、search和split方法。这一章介绍Javascript的正则表达式。

### 创建一个正则表达式

通过下面两种方法你可以创建一个正则表达式：

#### 使用一个正则表达式字面量，如下所示：

var re = /ab+c/;

正则表达式字面量在脚本加载后编译。若你的正则表达式是常量，使用这种方式可以获得更好的性能。

#### 调用RegExp对象的构造函数，如下所示：

var re = new RegExp("ab+c");

使用构造函数，提供了对正则表达式运行时的编译。当你知道正则表达式的模式会发生改变， 或者你事先并不了解它的模式或者是从其他地方（比如用户的输入），得到的代码这时比较适合用构造函数的方式。

### 编写一个正则表达式的模式

一个正则表达式模式是由简单的字符所构成的，比如/abc/, 或者是简单和特殊字符的组合，比如 /ab*c/ or /Chapter (\d+)\.\d */. 最后一个例子用到了括号，它在正则表达式中可以被用做是一个记忆设备。这一部分正则所匹配的字符将会被记住，在后面可以被利用。正如 Using Parenthesized Substring Matches

### 使用简单的模式

简单的模式是有你找到的直接匹配所构成的。比如，/abc/这个模式就匹配了在一个字符串中，仅仅字符'abc'同时出现并按照这个顺序。在Hi, do you know your abc's?" 和 "The latest airplane designs evolved from slabcraft."就会匹配成功。在上面的两个实例中，匹配的是子字符串‘abc’。在字符串"Grab crab"中将不会被匹配，因为它不包含任何的‘abc’子字符串。

### 使用特殊字符

当你需要搜索一个比直接匹配需要更多条件的匹配时，比如寻找一个或多个b's,或者寻找空格，那么这时模式将要包含特殊字符。比如， 模式/ab*c/ 匹配了一个单独的‘a’后面跟了零个或则多个b（ *的意思是前面一项出现了零个或者多个），且后面跟着‘c’的任何字符组合。在字符串“cbbabbbbcdebc,”中，这个模式匹配了子字符串'abbbbc'。

关于正则表达式，可以去下面这张图片的详细介绍：

![image](http://pic002.cnblogs.com/img/abeen/200906/2009060116224266.jpg)

接下来来看一些实例应用。

### 测试一个字符串是否包含另一个字符串

使用正则表达式来定义一个搜索模式，然后根据要搜索的字符串来应用该模式，使用RegExp test方法。

并且RegExp test方法接受两个参数，要测试的字符串和一个可选的修饰符。它对该正则表达式应用，如果一个匹配的话，返回true；否则，就返回false。如下代码所示：

	var cookbookString = [];

	cookbookString[0] = "janily want a book";
	cookbookString[1] = "janily want a cookbook";
	cookbookString[2] = "javascript cookbook";
	cookbookString[3] = "janily want a bookCook";

	//搜索模式

	var pattern = /cook.*book/;

	for (var i = 0; i < cookbookString.length; i++) {
  		console.log(cookbookString[i] + " " + pattern.test(cookbookString[i]));
	}
	
### 使用新字符串替换模式

用一个新的子字符串替换所有匹配子字符串。

方法是使用String对象的replace方法和一个正则表达式，如下代码所示：

	var searchString = "now is the time, this is the time";
	var re = /t\w{2}e/g   //g表示全局匹配
	var replacement = searchString.replace(re,"place");
	alert(replacement); //字符串变成 now is the place, this is the place
	
正则表达式的() [] {}有不同的意思。

() 是为了提取匹配的字符串。表达式中有几个()就有几个相应的匹配字符串。

(\s*)表示连续空格的字符串。

[]是定义匹配的字符范围。比如 [a-zA-Z0-9] 表示相应位置的字符要匹配英文字符和数字。[\s*]表示空格或者 * 号。

{}一般用来表示匹配的长度，比如 \s{3} 表示匹配三个空格，\s[1,3]表示匹配一到三个空格。

(0-9) 匹配 '0-9′ 本身。 [0-9]* 匹配数字（注意后面有 *，可以为空）[0-9]+ 匹配数字（注意后面有 +，不可以为空）{1-9} 写法错误。

[0-9]{0,9} 表示长度为 0 到 9 的数字字符串。

下面来看一个正则表达式中圆括号的应用。

### 在字符串中找到一个模式的所有实例

先来看看圆括号使用的一个实例。

例如，我们收到一个名称和姓氏的字符串，想交换名称，以便让姓氏出现在前面。

	var name = "janily chen";
	var re = /^(\w+)\s(w+)$/;
	var newname = name.replace(re,"$2,$1");

捕获圆括号不仅能够匹配字符串中的特定模式，而且稍后引用匹配的子字符串。匹配的子字符串用数字来引用，从左到右，在String replace方法中可以使用 "$1" 和 "$2"来表示。

在上面的代码中，正则表达式匹配两个单词，这两个单词用空格隔开，并且都应用来捕获圆括号，一次，使用"$1"来访问名称，使用"$2"来访问姓氏。

**String.place特殊模式**

模式  用途

$$   允许替换中有一个直接量-美元符($)
$&   插入匹配的子字符串
$`   在匹配之前插入字符串的一部分
$'   在匹配之后插入字符串的一部分
$n   在插入使用RegExp的第n次补货圆括号到值

接下来使用String.place的特殊模式在字符串中查找文本并突出显示。详细代码可以在下面相应的tab查看到。

试试在textarea中输入一些字符，然后在下面的文本框中输入想要查找的关键字，点击搜索按钮看看会发生什么？

<a class="jsbin-embed" href="http://jsbin.com/fehoqa/2/embed">test</a><script src="http://static.jsbin.com/js/embed.js"></script>

### 用正则表达式来去除空白

在新的规范中，String有一个专门去处空白的方法trim()。

	var testString = " this is blank";
	testString = testString.trim(); //空白去除了
	
不过现在很多浏览器对标准的支持还有限，在全面支持之前，我们可以使用正则表达式来去除空白。

	var testString = " this is blank";
	
	//去除开头的空白
	testString = testString.repalce(/^\s+/,"");
	//去除结尾的空白 testString = testString.repalce(/\s+$/,"");
	
我们可以把去除开头和结尾空白封装成两个函数方法，方便经常调用。

	function leftTrim(str) {
		return str = str.repalce(/^\s+/,"");
	}
	
	function rightTrim(str) {
		testString = testString.repalce(/\s+$/,"");
	}
	

	
	






