---
layout: post
title: 	ractive.js学习笔记(10)——HTML模板文件管理
categories:
- Life
tags:
- javascript
- ractive.js
---

在前端开发中使用模板一个好处是很方便对HTML文件进行管理，比如模板文件。在一般的web开发中，有很多相同的重复的HTML文件，而Ractive为我们提供了模板管理的**partials**功能来进行对HTML文件的管理。

下面我们就以一个todo的web应用来学习怎么使用**partials**进行模板文件管理。

首先在js代码中编写下面的代码：

![](http://pic.yupoo.com/reicky_v/DyPpvJCL/Ow0QF.jpg)

然后在模板文件中使用**&gt;**符号来使用我们定义好的模板文件：

![](http://pic.yupoo.com/reicky_v/DyPrXSVS/uED6R.jpg)

定义好之后，我们就可以在定义**TodoList**的这个程序中使用它：

![](http://pic.yupoo.com/reicky_v/DyQ7yg1T/PwO9R.jpg)

完整代码可以去官方的这个[地址](http://learn.ractivejs.org/partials/1/)查看。

有时模板字符串比较少的时候，把模板放在js文件里没什么大的影响，但是当你模板文件比较大的时候从代码的可维护性和简洁性都不是很友好，当我们使用AJAX来加载模板数据的时候，我们可能更倾向下面这两种方法来管理模板文件。

第一种方法是，在页面中使用**script**标签把模板包裹起来，如下所示：

![](http://pic.yupoo.com/reicky_v/DyQhc1CI/s1Vdx.jpg)

> 在上图中的代码中使用**id**来指定模板，而且**type**属性为**text/ractive**，不是**text/javascript**。

除了上面的方法之外我们还可以使用下面这种方法来管理模板，使用注释的方式：

![](http://pic.yupoo.com/reicky_v/DyQoels4/sULua.jpg)

这篇文章只是对在ractivejs中怎么管理模板文件做了简单的说明，更加详细的可以去[官方网站](http://learn.ractivejs.org/partials/2/)详细了解。


