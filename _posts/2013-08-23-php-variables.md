---
layout: post
title: php学习（变量、字符串、循环）
categories:
- Life
tags:
- php
- 翻译
---

做前端也一年多了，一直以来都想学习下一门后端语言，也买了本php的书，断断续续看了一些，没有系统地学，时间花了不少到头来一场空。这段时间也因为自己有一些想法，希望能自己独立地做出一个web应用，重新拾起了PHP，也一把年纪啦，趁着自己现在年轻，再不做点自己想做的事情，以后怕越发的艰难了。

好记性不如烂笔头，正好在sitepoint这个网站有一系列的php方面的教程，就顺便重新来梳理下PHP方面的知识记录下来。这中间也会边梳理一些基础知识，边做一些实例。翻译的话是采取意译，不会逐字逐句来翻译了。

今天就来梳理下PHP中的变量、字符串和循环这三个知识点。

# 变量 #

在php中变量是用来指向已经定义好的值的。正如其名所示，变量所定义的值是随时可以通过程序来改变的。变量是像诸如PHP这类动态编程语言所具有的一个特征，像HTML这类静态的语言就不具有变量这个特征了。

例如，我们要保存用户的名字和喜欢的颜色，但是每个用户都有不同的名字和喜欢的颜色，这个时候我们就要用变量来保存用户的名字和喜欢的颜色啦。

![](http://pic.yupoo.com/reicky_v/D6Isij2f/rg0DT.png)

变量的创建方法有很多种，但是一般我们用声明的方式来创建变量。比如我们创建两个变量$name 和 $color，用来保存用户和颜色，这样就可以用代码来保存用户的名字和颜色了。

    <?php
	echo "Hello, $name. Your favorite color is $color.";

这段代码将会输出

    Hello, John. Your favorite color is blue.

现在明白了变量是怎样来保存值和输出，非常容易就可以做到。

## 创建变量 ##

在PHP中，变量的创建非常容易，当然也要遵守一些约定的规则

- 必须以美元符 $ 来定义
- 变量必须以字母或者是下划线开头
- 变量的名称可以结合字母、数字、下划线来命名

比如$customerName是允许的，因为遵守了上面三条规则

$123customer就是不允许创建变量的，因为没有遵守第二条规则，变量必须以字母或者下划线开头。

当然，命名的时候名字最好是有意义的。便于理解可维护，比如命名一个用户的名字可以用$userName，当然你也可以用$abc123，来表示用户名字。不要告诉我你愿意这样来命名你的变量。

## 设置变量 ##

知道怎么创建变量，接下来看看怎么给变量赋值，如下代码

    <?php
	$customerName = "Fred";
	$customerID;
	$customerID = 346646;
	$customerName = $customerID;

我们首先定义一个$customerName的变量，并且给给它设置了一个值“Fred”。这样我们就创建了一个变量并赋值了。当我们使用$customerName的时候，PHP就会知道它的值是Fred。

变量之所以为变量，就是因为可以随时改变变量的值。就像上面代码那样我们，又为customerID指定了值 346646 。再用customerID的时候，就指向346646了而不是Fred。会覆盖Fred这个值。

当然需要注意的是，变量也分为不同的类型，“Fred”是字符串类型，而346646是数字类型。php的变量主要分为五种类型，如下所示

    <?php
	$total = 0;                              // 整型
	$total = "Year to Date";                 // 字符串
	$total = true;                           // 布尔值
	$total = 100.25;                         // 浮点类型
	$total = array(250, 300, 325, 475);      // 数组

变量的一些基本知识就是这么简单，下面我们来看一个简单的例子，来进一步学习变量的知识

    <?php
	$firstNumber = 4;
	$secondNumber = 6;
	$result = $firstNumber + $secondNumber;

上面代码表示把$firstNumber和$secondNumber的值相加起来赋给$result这个变量。+是一个运算符，这个以会讲到的。结果是会输出10.

## 显示输出变量值 ##

从上面也可以看到，PHP中用echo来输出显示变量的值。当然也可以用print来输出。

    <?php
	echo $customerName;

当然有时候可能需要把变量的值和一些文本连接起来，在PHP中也是非常容易办到的，如下所示

    <?php
	echo "Customer name = " . $customerName;

.号是用来连接字符串的，它会把文字和变量的值一起连接起来。

当然你也可以用插值的方法来代替.号连接字符串。插值的方法是，当有变量的时候，它会把变量的值显示出来。反之亦然。这样在程序上可以更具有可读性。

    <?php
	echo "Customer name = $customerName";

当然，PHP不会解析单引号里面的内容，这样我们也可用它来做一些其它的事情

    <?php
	echo '$customerName has the value: ' . $customerName;

变脸的一些基础知识就是这些了，可以去官方文档去看看 [PHP documentation](http://www.php.net/language.variables.php)。

# 字符串 #

PHP有强大字符串处理函数，可以让你处理字符串游刃有余。当然这里不可能介绍所有的字符串函数，所以我们要学会去PHP中的文档中去找我们所需要的方法函数。特别是对于我这种新手来说，更是如此。这里只是介绍一些常用的字符串函数。知道了这些函数，一些常见的字符串处理就很容应对了。

## 实例介绍 ##

PHP提供了一些函数，可以用来在不需要编辑就来处理字符串。这有什么用呢？比如，你可能需要强制字符串的第一个字母大写，或者需要把字符串强制显示为大写。又或者你想要在同等的情况下比较两个字符串等等诸如此类的情况， 在PHP中都可以找到对应的函数。下面我们就来介绍一些。

### 小写转大写 ###

比如你想要把字符串的字母全部转为大写，就可以用strtoupper()这个函数，如下所示

    <?php
	$str = "Like a puppet on a string.";
	$cased = strtoupper($str);
	// 结果将会显示: LIKE A PUPPET ON A STRING.
	echo $cased;

只会针对字母转换，数字和非字母不会转换。

既然有大写转换，当然有小写转换。strtolower()函数会把字母转为小写

    <?php
	$str = "LIKE A PUPPET ON A STRING.";
	$cased = strtolower($str);
	// 转为小写: like a puppet on a string.
	echo $cased;

当然，有时候你想只是把每一个单词的第一个字母转换为大写，比如用户名啥的。就可以用ucwords()这个方法

    <?php
	$str = "a knot";
	$cased = ucwords($str);
	// Displays: A Knot
	echo $cased;

当然也有可能，在一句话中，想把第一个字母转为大写或者是小写，你可以用lcfirst()和ucfirst() 方法。ucfirst() 就可能会经常用到。有时在一句话中，经常就要把第一个字母转为大写。如下

    <?php
	$str = "how long is a piece of string?";
	$cased = ucfirst($str);
	//第一个字母会大写: How long is a piece of string?
	echo $cased;

### 去点空格 ###

去掉字符串的空格这在web开发中很常见，比如字符串的开头和结尾的空格就需要去掉，trim()方法就可以用来去掉开头和结尾的空格。空格也会占用空间的。

    <?php
	$str = "  A piece of string?  ";
	// 显示22个字符: string(22) " A piece of string? "
	var_dump($str);
	 
	$trimmed = trim($str);
	// 去掉空格后显示18个字符: string(18) "A piece of string?"
	var_dump($trimmed);

当然trim()还可以指定范围来匹配去掉空格，看一下下面的代码

    <?php
	$str = "A piece of string?";
	$trimmed = trim($str, "A?");
	// 会过滤掉A?这两个字符: string(16) " piece of string"
	var_dump($trimmed);

trim()指定参数的时候，可要小心点了，如果你不小心在参数之间打了一个空格，它也会过滤掉，比如以下的代码

    <?php
	$str = "A piece of string?";
	$trimmed = trim($str, "A ?");
	// A ？中间多了一个空格，也会过滤掉: string(15) "piece of string"
	var_dump($trimmed);

trim()函数只会去除开头和结尾的空格，当A过滤掉的时候，A后面的空格就变成了开头的空格了，也会去掉。

跟trim()函数相关的函数还有ltrim() 和 rtrim()函数，对应的是去掉开头和结尾的空格。

### 字符串的长度 ###

有时候我们经常需要知道字符串的长度，例如当处理表单数据的时候，我们需要知道用户输入字符串的长度一边做出判断，就可以用到strlen()方法。

    <?php
	$str = "How long is a piece of string?";
	$size = strlen($str);
	// Displays: 30
	echo $size;

### 返回字符串指定参数的字串 ###

有时候，我们需要从一个特定的字符串中返回指定的字符，这时候就可以用到substr()方法。

使用substr()方法我们需要指定一个参数来指定我们想要返回的字符。得是一个整数。它会从0开始计算位置直到符合我们指定的参数，再从我们指定的位置返回字符。如下

    <?php
	$str = "How to cut a string down to size";
	$snip = substr($str, 13);
	//会从13这个位置返回字符: string down to size
	echo $snip;

当然你也可以使用负数，它会从字符串的结尾处返回指定参数个数的字符

    <?php
	$str = "How to cut a string down to size";
	$snip = substr($str, -7);
	//Displays: to size
	echo $snip;

substr()方法还接收第三个参数，来指定返回字符串的长度。比如

    <?php
	$str = "How to cut a string down to size";
	$snip = substr($str, 13, 6);
	//6指定返回字符串的长度: string
	echo $snip;

如果你只是要在一个字符串中找特定的字符，那么你可以用strpos()方法。strpos()方法是查找字符串首次出现的位置，当你不确定字符在字符串中的位置的时候，你可以指定一个偏移量，来缩小查找范围。比如

    <?php
	$str = "How to cut a string down to size";
	$snip = substr($str, strpos($str, "string"), 6);
	//从字符串6的位置开始查找: string
	echo $snip;

### 替换方法 ###

最后让我们来看看字符串中的替换方法。替换方法是str_replace()。它会用你指定字符替换你想要替换的字符。当你想替换掉特定字符的时候，你就可以用这个方法，如下

    <?php
	$oldstr = "The cat was black";
	$newstr = str_replace("black", "white", $oldstr);
	// Displays: The cat was white
	echo $newstr;

当然你也可以用数组的形式，来替换一组的内容

    <?php
	$oldstr  = "The flag was red white and blue.";
	$america = array("red", "white", "blue");
	$germany = array("black", "red", "yellow");
	$newstr = str_replace($america, $germany, $oldstr);
	// Displays: The flag was black red and yellow.
	echo $newstr;

str_replace()方法是区分大小写的，这一点需要注意下。当然你如果想不区分大小写，你也可以用str_ireplace()方法来代替str_replace()方法。

# 循环语句 #

电脑相比人脑来说，一个明显的优势是可以重复并且高效地执行为它设定的任务。你可以用一些循环语句来重复执行一些常见的数据操作，而不用每次都写一些重复的代码来执行它。在PHP中有：for，while,do-while和foreach这些循环语句。下面会依次介绍它们具体的用法。

### for循环 ###

让我们想象一下，我们要写一个1到12的乘法表，我们可能会这样来写，分别从1到12跟12乘一遍，如下

    <?php
	echo "1 × 12 = " . (1 * 12) . "<br>";
	echo "2 × 12 = " . (2 * 12) . "<br>";
	echo "3 × 12 = " . (3 * 12) . "<br>";
	...

这样依次写12便同样的代码，数字少还好，如果是50*12呢？写50遍，不仅效率低重要的是浪费时间，是一种非常笨的方法。

如果用for循环语句的话，那就简单多啦。

    <?php
	for ($i = 1; $i <= 12; $i++) {
	    echo $i . " × 12 = " . ($i * 12) . "<br>";
	}

这个简单的例子就展示了循环语句是多么地高效。仅仅三行代码。

for循环有三个参数，分别代表的意思如下所示

- $i = 1第一个参数是初始化变量$i的值，它会用来做循环的计数的值。即用来设定循环的次数。
- $i <= 12第二个参数是用来设定循环停止条件的
- $i++ 第三个参数是用来设定循环具体执行的方式，比如这里是递增

从可维护的角度来看，如果你想改变乘法表，你只要改变下第二个参数就行了，比如你想乘到50，你就可以这样改

    <?php
	for ($i = 1; $i <= 50; $i++) {
	    echo $i . " × 12 = " . ($i * 12) . "<br>";
	}

非常简洁，比你写50行代码不强多了。

如果想改变一下这个乘法表，以递减的方式来展现呢？即12*12，12*10这样。也很简单，只需要简单地改下循环具体执行方式就可以了

    <?php
	for ($i = 12; $i > 0; $i--) {
	    echo $i . " × 12 = " . ($i * 12) . "<br>";
	}

这里把$的值设置为12以递减的方式循环，直到$的值小到大于0的时候，停止循环。

### while 和 do-while 循环 ###

for循环非常适合于用数字就可以简单表示任务情形的情况，但是当你并不是用数字判断来执行你的循环的时候，可能for就不适用了，这个时候do-while就派上用场了。

do-while 循环和 while 循环非常相似，区别在于表达式的值是在每次循环结束时检查而不是开始时。和一般的 while 循环主要的区别是 do-while 的循环语句保证会执行一次（表达式的真值在每次循环结束后检查），然而在一般的 while 循环中就不一定了（表达式真值在循环开始时检查，如果一开始就为 FALSE 则整个循环立即终止）。

例如下面这段代码

    <?php
	$file = "employees.csv";
	$employeeFile = fopen($file, "r");
	do {
	    $data = fgets($employeeFile);
	    echo $data . "<br>"; 
	}
	while (!feof($employeeFile));

上面的代码中，先会执行do里面的语句，然后才会判断条件。用while写，就如下面所示，先会判断执行条件，再循环

    <?php
	$file = "employees.csv";
	$employeeFile = fopen($file, "r");
	while (!feof($employeeFile)) {
	    $data = fgets($employeeFile);
	    echo $data . "<br>"; 
	}

###  foreach循环 ###

在for循环一节中，我们写过一个简单的乘法表。这里我们来看一下另外一种情形，数组(以后会详细写一下)。比如一年中有12个月份，就可以看做是一个数组

    <?php
	$months = array("January", "February", "March", ... "Dec");
	for ($i = 0; $i < 12; $i++) {
	    echo $months[$i] . "<br>";
	}

为了得到数组里的值，你需要这些值的键值来作为索引。我们先用for循环来取出数组的值，我们这里用到$i这个变量来作为数组的索引值，然后跟据这些索引值遍历出对应的值。

    <?php
	$months = array("January", "February", "March", ... "Dec");
	for ($i = 0; $i < 12; $i++) {
	    echo $months[$i] . "<br>";
	}

当然用for来取出数组的值没问题，但是foreach方法天生就是为数组而准备的方法。它不需要提前需要知道数组有多元素等才能取出数组的值。

它只要两个参数，一个是数组的变量和数组里面元素的值就可以了，非常容易就取出数组元素的值。如下

    <?php
	foreach ($months as $value) {
	    echo $value . "<br>";
	}

这比for循环更简单。不需要像for一样计算数组的长度。跟数组的长度没关系，就算数组有变化，你也不需要改代码。

也可以用另外一种方法来取数组里面的值

    <?php
	foreach ($months as $key => $mon) {
	    echo $key . " - " . $mon . "<br>";
	}

这儿$key =>这个表达式，就可以访问到数组元素的键值了。

### break 和 continue 关键字###

循环顾名思义就是一直不断的循环执行下去，当然你如果想在一个特定的情况下退出终止循环也是可以的。就要用到break这个关键字，我们还是拿do-while循环里面的例子来说一下break具体的用法。

    <?php
	$file = "employee.sql";
	$employeeFile = fopen($file, "r");
	do {
	    $data = fgets($employeeFile);
	    if (strlen($data) == 0) {
	        break; 
	    }
	    else {
	        echo $data; 
	    }
	}
	while (!feof($employeeFile));

这里如果检测到数据位空的时候，就会停止执行命令。如果想继续循环就用continue关键字。

### 总结一下 ###

这篇文章相继简要梳理了一下php中变量、字符串、循环这个三个的一些基本知识点，更加详细的还是要看一下手册。下篇接着聊PHP。

这篇文章参考了以下文章

[PHP Variables](http://www.sitepoint.com/variables/)。
[PHP String Handling Functions](http://www.sitepoint.com/string-handling-functions/)。
[Learning Loops](http://www.sitepoint.com/loops/)。
以及[PHP官方手册]()




