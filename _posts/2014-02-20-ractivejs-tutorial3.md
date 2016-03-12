---
layout: post
title: 	ractive.js学习笔记(3)—— 数据格式
categories:
- Life
tags:
- javascript
- ractive.js
---

上篇文章我们学习了使用数据设置的方式来设置元素的属性，今天我们来看看在ractive使用[mustache语法风格](http://mustache.github.io/)的属性嵌套的特点来表示数据，那就意味着我们可以是javascript对象的形式来表示数据。

下面我们通过构建一个地图应用的例子来学习在ractive中使用对象嵌套的方式来表示数据。比如一个国家或者是城市的数据就可以使用以下的形式来表示：

    {
	  name: 'Yue Yang',
	  climate: { temperature: 'cold', rainfall: 'excessive' },
	  population: 62641000,
	  capital: { name: '岳阳市', lat: 29.372602, lon: 35.173808 }
	}

我们就可以把它放到ractive中来使用。

开始之前我们先来新建一个demo.html文件，代码如下：

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
	
	        <p>{{country.name}} is a {{country.climate.temperature}} country with {{country.climate.rainfall}} rainfall and a population of {{country.population}}.</p>
	
	        <p>The capital city is CAPITAL_NAME_HERE (<a href='https://maps.google.co.uk/maps?q=LATITUDE_HERE,LONGITUDE_HERE&z=11' target='_blank'>see map</a>).</p>
	    </script>
	
	    <script src='ractive.js'></script>
		<script>
	        var ractive = new Ractive({
	          el: output,
	          template: template,
	          data: {
			    country: {
			      name: 'The UK',
			      climate: { temperature: 'cold', rainfall: 'excessive' },
			      population: 62641000,
			      capital: { name: 'London', lat: 51.5171, lon: -0.1062 }
			    }
			  }
	        });
	
	    </script>
	</body>
	</html>

而且我们可以使用{{country.name}}这样的形式来方位数据里面的值。

打开浏览器看看，如下图所示：

![](http://pic.yupoo.com/reicky_v/DydFqWs1/medium.jpg)

下面让我们来使用**ractive.set**方法来改变经纬度，就是用中国的经纬度来看下回发生什么变化，代码如下：

    ractive.set( 'country', {
	  name: 'China',
	  climate: { temperature: 'hot', rainfall: 'limited' },
	  population: 1322620600,
	  capital: { name: 'Beijing', lat: 39.9139, lon: 116.3917 }
	});

直接在chrome的控制台里来操作如图所示：

![](http://pic.yupoo.com/reicky_v/DydK0OMA/medium.jpg)

点击链接可以看到地图：

![](http://pic.yupoo.com/reicky_v/DydK106O/medium.jpg)

在地图应用我们可能不止在地图上标记一个国家或者是地点，有时候可能需要标记很多的国家或者是地区，-难道我们要一个一个像这样来写**＆#123;＆#123;country.capital.xxx＆#125;＆#125;**而访问属性值。

当然不需要这么麻烦，在ractive中我们可以使用**＆#123;＆#123;#section＆#125;＆#125;<!-- stuff -->＆#123;＆#123;/section＆#125;＆#125;**这样的语法来编写模板，＆#123;＆#123;#param＆#125;＆#125;这个标签很强大，有if判断、forEach的功能。比如上面模板中的我们就可以使用上面的这个语法来改写，按照我们上面的数据格式的特点，我们可以创建＆#123;＆#123;#couintry＆#125;＆#125;以及在＆#123;＆#123;#country＆#125;＆#125;里面的＆#123;＆#123;capital＆#125;＆#125;这两个数据区块。

这里需要注意一下，如果我们创建了两个数据区块，那我们现在就有两个**＆#123;＆#123;name＆#125;＆#125;**变量了——一个是country的，一个是capital的。

我们可以看到如果访问capital的**＆#123;＆#123;name＆#125;＆#125;**属性，我们要经过两层的上下文的路径来访问到它，如**country.capital.name**来访问capital的name的属性。

如果capital没有name这个属性，那么Ractive会自动往上移动一层来寻找是否有这个属性，在这里是country然后直接访问**data**对象直到找到**name**这个属性为止。我们还可以使用**ractive.set()**的方法来设置属性。

	 <p>{{country.name}} is a {{country.climate.temperature}} country with {{country.climate.rainfall}} rainfall and a population of {{country.population}}.</p>

我们可以改写为：

    {{#country}}
  		<p>{{name}} is a {{climate.temperature}} country with {{climate.rainfall}} rainfall and a population of {{population}}.</p>

  	{{#capital}}
    <p>The capital city is {{name}} (<a href='https://maps.google.co.uk/maps?q={{lat}},{{lon}}&z=11' target='_blank'>see map</a>).</p>
  	{{/capital}}
  
	{{/country}}

### 更新属性值 ###

ractive还为我们提供了**ractive.update()**方法来更新数据，比如：

    var country = ractive.get( 'country' );

	country.climate.rainfall = 'very high';
	ractive.update( 'country' );

Ractive就会更新数据里面的**rainfall**这个属性的值。

当然我们也还可以使用一种更简单的方法来更新：

    ractive.set('country.climate.rainfall','very high');

这样也可以更新数据。

下篇文章我们来看看在ractive中表达式的部分。

   