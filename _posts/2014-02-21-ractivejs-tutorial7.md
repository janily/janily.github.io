---
layout: post
title: 	ractive.js学习笔记(7)——表格应用
categories:
- Life
tags:
- javascript
- ractive.js
---

这篇文章我们就来用一个排序的表格的实例来学习列表数据在ractive中应用，使用的数据谁来自[superherobd.com](http://www.superherodb.com/)。

首先来看下我们的数据：

    xmen = [
	  { name: 'Nightcrawler', realname: 'Wagner, Kurt',     power: 'Teleportation',    info: 'http://www.superherodb.com/Nightcrawler/10-107/' },
	  { name: 'Cyclops',      realname: 'Summers, Scott',   power: 'Optic blast',      info: 'http://www.superherodb.com/Cyclops/10-50/' },
	  { name: 'Rogue',        realname: 'Marie, Anna',      power: 'Absorbing powers', info: 'http://www.superherodb.com/Rogue/10-831/' },
	  { name: 'Wolverine',    realname: 'Howlett, James',   power: 'Regeneration',     info: 'http://www.superherodb.com/Wolverine/10-161/' }
	];

从数据可以看到我们需要循环遍历**xmen**这个数据对象，根据这个特点我们可以这样来写我们的模板：

![](http://pic.yupoo.com/reicky_v/DyrayTZ7/medium.jpg)

然后根据mustaches的语法来输出三个属性的值到表格中**name**，**realname**和**power**。还有链接地址**info**。

新建一个table.html文件，编写以下代码：

![](http://pic.yupoo.com/reicky_v/DyraysaR/medium.jpg)

在浏览器中运行文件，表格和数据就已经正确的显示在视图中，如下图所示：

![](http://pic.yupoo.com/reicky_v/DypTdeL1/medium.jpg)

有时候我们可能有会想在表格中显示数据的索引值。

Mustache对于这种开发需求也没有提供很好的处理方法，对此Ractive提供了一个不错的方法：

![](http://pic.yupoo.com/reicky_v/DyrayWwN/medium.jpg)

通过声明**num**这个属性索引值，我们就可以像使用变量那样来使用它。

首先我们需要在表格的头部添加一个表示索引的单元格：

    <tr>
	  <th>#</th>
	  <th>Superhero name</th>
	  <!-- etc -->
	</tr>

相对应的还需要增加显示索引的单元格：

    {{#superheroes:num}}
	  <tr>
	    <td>{{num}}</td>
	    <td><a href='{{info}}'>{{name}}</a></td>
	    <td>{{realname}}</td>
	    <td>{{power}}</td>
	  </tr>
	{{/superheroes}}

这里需要注意一下，因为在数组中，索引是从0开始的，所以我们这里可以把每一个索引值都加一。完整代码如下所示：

![](http://pic.yupoo.com/reicky_v/DyrazSTC/medium.jpg)

在浏览器中打开文件，就可以看到表格里显示了索引值，如下图所示：

![](http://pic.yupoo.com/reicky_v/DyqlxkYH/medium.jpg)

### 修改或者是更新表格数据 ###

如果你想添加一条数据到表格中。可以使用**ractive.set()**方法来添加，不过你必须先要计算已经存在索引值的长度：

    var index = ractive.get( 'superheroes' ).length;
	ractive.set( 'superheroes[' + index + ']', newSuperhero );

这样做并不是一个很好地方法。我们也可以使用**ractive.update('superheroes')**来代替它，这样就可以实时更新你的数据：

    xmen[ xmen.length ] = newSuperhero;	
	ractive.update( 'superheroes' );

> 在使用**ractive.update()**更新的时候，没有指定一个更新的路径参数的时候，Ractive.js就会更新一切数据。

不过这里还有一个更方便的方法来更新数据。ractive.js也支持使用数组对象本身的方法来操作数据比如**push、pop、shift、unshift、splice、sort和reverse**这些方法，所以我们可以使用push方法来更新数据：

    xmen.push(newSuperhero);

你可以在控制台里输入下面的代码来更新表格里的数据：

    var newSuperhero = {
	  name: 'Storm',
	  realname: 'Monroe, Ororo',
	  power: 'Controlling the weather',
	  info: 'http://www.superherodb.com/Storm/10-135/'
	};
	
	xmen.push( newSuperhero );

数据就会实时更新到视图中去。

### 数组排序 ###

表格排序也是一个常见功能。

首先在表格的头部给每一个单元格添加触发排序的事件：

    <th class='sortable' on-click='sort:name'>Superhero name</th>
	<th class='sortable' on-click='sort:realname'>Real name</th>
	<th class='sortable' on-click='sort:power'>Superpower</th>

当我们点击头部单元格的时候，视图就会触发**sort**事件。

    ractive.on('sort',function(event,column){
		alert('Sorting by' + column);
	});

在浏览器中运行文件，当你点击头部单元格的时候就会触发事件，下面就来添加排序的功能。

下面来添加一些代码来实现真正的表格排序。在ractive中当然是使用表达式来解决。

调整下模板：

![](http://pic.yupoo.com/reicky_v/DyrdLh9O/medium.jpg)

要实现排序需要编写一个排序的函数。关于排序的实现方式也有很多种，下面就是其中的一个实现方式，可以去[这个地址](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Global_Objects/Array/sort)看看关于排序实现的详细的介绍。

    function ( array, sortColumn ) {
	  array = array.slice(); // clone, so we don't modify the underlying data
	
	  return array.sort( function ( a, b ) {
	    return a[ sortColumn ] < b[ sortColumn ] ? -1 : 1;
	  });
	}

使用它就非常简单：

    ractive.on( 'sort', function ( event, column ) {
	  this.set( 'sortColumn', column );
	});

完整代码如下：

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

        <table class='superheroes'>
        <tr>
          <th>#</th>
          <th on-click='sort:name'>Superhero name</th>
          <th on-click='sort:realname'>Real name</th>
          <th on-click='sort:power'>Superpower</th>
        </tr>

        {{# sort( superheroes, sortColumn ) :num}}
          <tr>
            <td>{{ num + 1 }}</td>
            <td><a href='{{info}}'>{{name}}</a></td>
            <td>{{realname}}</td>
            <td>{{power}}</td>
          </tr>
        {{/ end of superheroes list }}
      </table>
    </script>

  	<script src='ractive.js'></script>
	<script>

        var ractive, xmen;

        // 定义数据
        xmen = [
          { name: 'Nightcrawler', realname: 'Wagner, Kurt',     power: 'Teleportation',    info: 'http://www.superherodb.com/Nightcrawler/10-107/' },
          { name: 'Cyclops',      realname: 'Summers, Scott',   power: 'Optic blast',      info: 'http://www.superherodb.com/Cyclops/10-50/' },
          { name: 'Rogue',        realname: 'Marie, Anna',      power: 'Absorbing powers', info: 'http://www.superherodb.com/Rogue/10-831/' },
          { name: 'Wolverine',    realname: 'Howlett, James',   power: 'Regeneration',     info: 'http://www.superherodb.com/Wolverine/10-161/' }
        ];
        var ractive = new Ractive({
        el: 'container',
        template: '#myTemplate',
        data: {
          superheroes: xmen,
          sort: function ( array, column ) {
            array = array.slice(); // 重新复制数组

            return array.sort( function ( a, b ) {
              return a[ column ] < b[ column ] ? -1 : 1;
            });
          },
          sortColumn: 'name'
        }
      });

      ractive.on( 'sort', function ( event, column ) {
        this.set( 'sortColumn', column );
      });
    </script>
	</body>
	</html>

> 在浏览器中运行这个文件，点击表格的头部就可以实现排序。当然你也可以指定特定的列来初始化排序，比如'sortColumn:''name'来实现。

我们还可以做多一点点事情来优化体验，比如我们可以在当前排序的的列上添加一个类来高亮当前排序的列。我们可以在模板里面使用三元操作符来完成这个功能：

    <th class='sortable {{ sortColumn === "name" ? "sorted" : "" }}' on-tap='sort' data-column='name'>Superhero name</th>

现在在浏览器运行这个文件，就实现了一个表单排序的功能。

