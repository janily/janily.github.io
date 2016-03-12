---
layout: post
title: 	ractive.js学习笔记(3)——表达式
categories:
- Life
tags:
- javascript
- ractive.js
---

在上篇文章中的例子中有一个问题就是来表示人口的时候-使用的是一个很大的数字62641000来表示的，这样在体验上不是很好。

我们可以使用更简单的方式来表示这类型的数字，比如**62.6千万**这样体验上就好很多了。我们可以使用表达式来处理它：

    {{ format(population) }}

这需要定义一个函数来处理数字，如下所示：

    function (num) {
		  if ( num > 1000000000 ) return ( num / 1000000000 ).toFixed( 1 ) + ' billion';
		  if ( num > 1000000 ) return ( num / 1000000 ).toFixed( 1 ) + ' million';
		  if ( num > 1000 ) return ( Math.floor( num / 1000 ) ) + ',' + ( num % 1000 );
		  return num;
	}

新建一个espression.html文件，代码如下：

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

        <p>The population of {{country}} is {{ format(population) }}.</p>
    </script>

    <script src='ractive.js'></script>
	<script>
        var ractive = new Ractive({
        el: 'container',
        template: '#myTemplate',
        data: {
          country: 'the UK',
          population: 62641000,
          format: function ( num ) {
            if ( num > 1000000000 ) return ( num / 1000000000 ).toFixed( 1 ) + ' billion';
            if ( num > 1000000 ) return ( num / 1000000 ).toFixed( 1 ) + ' million';
            if ( num > 1000 ) return ( Math.floor( num / 1000 ) ) + ',' + ( num % 1000 );
            return num;
          }
        }
      });
    </script>
	</body>
	</html>

打开浏览器会发现浏览器会输出：The population of the UK is 62.6 million.这样看起来是不是好很多。

我们可以在控制台里改为中国的人口统计看看，代码如下：

    ractive.set({
	  country: 'China',
	  population: 1344130000
	});

> 注意一下这里的表达式是Ractive.js特有的。

在web开发中特别是在电商类型的web开发中，经常会有数学计算的开发需求，当然在Ractive中处理这样的计算功能也很简单，下面我们就来看看。比如在电商类型中经常要计算商品的总价格，我们这个商品的名为**item**，有一个数量属性**quantity**以及价格**price**，我们就可以这样来计算：

   ![](http://pic.yupoo.com/reicky_v/DygUnoMQ/medium.jpg)

同样这里也需要建立一个函数来处理货币的转换，这里是处理英镑的一个函数代码如下：

    function ( num ) {
	  if ( num < 1 ) return ( 100 * num ) + 'p';
	  return '£' + num.toFixed( 2 );
	}

OK,我们来新建一个stuff.html的文件，编写以下代码：

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

        <table>
          <tr>
            <th>Price per {{item}}</th>
            <th>Quantity</th>
            <th>Total</th>
          </tr>

          <tr>
            <td>{{ format(price) }}</td>
            <td>{{quantity}}</td>
            <td>{{ format( price * quantity ) }}</td>
          </tr>
        </table>

    <script src='ractive.js'></script>
	<script>
        var ractive = new Ractive({
        el: 'container',
        template: '#myTemplate',
        data: {
          item: 'pint of milk',
          price: 0.49,
          quantity: 5,
          format: function ( num ) {
            if ( num < 1 ) return ( 100 * num ) + 'p';
            return '£' + num.toFixed( 2 );
          }
        }
      });
    </script>
	</body>
	</html>

现在打开浏览器就会自动帮我们计算相关的数值，如下图所示：

![](http://pic.yupoo.com/reicky_v/DyfRNnG8/medium.jpg)

到这里你可能会想这是怎么完成的呢。当解析模板的时候，在表达式中的属性如**price**、**quantity**以及**format**在定义的时候都发生了些什么。在渲染的时候，只要确认了这些属性的引用关系，每一个属性的依赖关系就注册为一个表达式，然后来创建相关的计算表达式。(ractive将会尽可能的重复利用这些公式，而不是重新去计算。)

当其中依赖关系的属性的值发生变化时，表达式会重新计算。如果结果发生改变，就会自动更新到DOM中。

更多的关于表达式的内容可以去ractive的[wiki](https://github.com/RactiveJS/Ractive/wiki/Expressions)看看。

### 表达式在属性中的应用 ###

在这个例子中我们创建一个关于颜色混合器计算的实例来看看表达式在属性中应用。先来看看具体的表现形式：

![](http://pic.yupoo.com/reicky_v/Dyg4jCLN/medium.jpg)

首先，我们来新建一个color.html的文件。我们用下面这样的结构来表示每一种颜色的的使用数量，代码如下：

![](http://pic.yupoo.com/reicky_v/DygUnjAL/medium.jpg)

依次类推建编写三个这样类型的结构。可以从上面的图中发现最后结果一栏是按照上面三种颜色的值来计算得出值的。我们使用**Math.round**这个方法来进行取整：

![](http://pic.yupoo.com/reicky_v/DygUnOpv/medium.jpg)

完整的代码如下：

    <!doctype html>
	<html lang='en-GB'>
	<head>
    <meta charset='utf-8'>
    <title>ractivejs start</title>
    <style type="text/css">
        .color-mixer div {
            width: 100%;
            max-width: 100%;
            height: 100%;
          }
          table {
            border-spacing: 0;
            width: 100%;
          }
        .color-mixer {
            height: 100%;
        }
        th,td {
          border-bottom: 1px solid #eee;
          padding: .5em 1em;
          text-align: left;
        }
    </style>
	</head>
	<body>
    <h1>ractivejs start</h1>

    <div id='container'></div>

    <script id='myTemplate' type='text/ractive'>
        <h2>City profile</h2>

        <table class='color-mixer'>
          <tr>
            <th>Colour</th>
            <th>Amount</th>
          </tr>
          <tr>
            <td>red:</td>
            <td><div style='background-color: red;width: {{ red * 100 }}%;'></div></td>
          </tr>
          <tr>
            <td>green:</td>
            <td><div style='background-color: green;width: {{ green * 100 }}%;'></div></td>
          </tr>
          <tr>
            <td>blue:</td>
            <td><div style='background-color: blue;width: {{ blue * 100 }}%;'></div></td>
          </tr>
          <tr>
            <td><strong>result:</strong></td>
            <td><div style='background-color: rgb({{ Math.round( red   * 255 ) }},{{ Math.round( green * 255 ) }},{{ Math.round( blue  * 255 ) }})'></div></td>
          </tr>
        </table>
	  </script>
    <script src='ractive.js'></script>
    <script>
        var ractive = new Ractive({
        el: 'container',
        template: '#myTemplate',
        data: {
          red: 0.45,
          green: 0.61,
          blue: 0.2
        }
      });
    </script>
	</body>
	</html>

打开浏览器就会看到结果啦。

上面的代码中我们在表达式中用到了javascript中Math对象中round()方法，在javascript中还有很多对象都可以在表达式中使用(如Date、Array、encodeURL、parseInt、JSON等)。详细的可以去[官方的文档](https://github.com/RactiveJS/Ractive/wiki/Expressions#supported-global-objects)看看。

表达式说简单也不简单，复杂也不复杂。如果只是在视图中用用的话(仅仅是输出一些数据)，不包括一些复杂的数学运算以及函数的运算，那表达式还是很简单的。






