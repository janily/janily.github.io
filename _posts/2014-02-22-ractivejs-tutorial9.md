---
layout: post
title: 	ractive.js学习笔记(9)——双向绑定
categories:
- Life
tags:
- javascript
- ractive.js
---

这篇文章我们来学习Ractive中的关于双向绑定功能的部分。

先来看一个简单的实例，新建一个binding.html文件，输入以下代码：

![](http://pic.yupoo.com/reicky_v/DyxHtHki/medium.jpg)

在浏览器中运行文件，尝试着在文本框中输入文字，就会看到下面的文字会实时更新到我们输入的文字，如下图所示：

![](http://pic.yupoo.com/reicky_v/DyxJ8Lqd/epaUg.gif)

> 不要看这样一个简单的效果，其实ractive在内部为我们处理很多的工作，比如**input**这个元素，绑定了**change**事件和**blur**事件——这样当input元素里的值发生变动的时候能监听并作处理。如果你只想在**change**和**blur**事件的时候来更新模板文件，可以通过**lazy:true**这个设置选项来实现。如果你不想双向绑定，也可以通过设置**twoway:false**来实现。

当然在实际的开发中，没有这么简单。在开发中做的更多的是跟当数据发生变化时需要处理变化的这样的需求。我们可以使用**ractive.observe()**:

    ractive.observe( 'name', function ( newValue, oldValue ) {
	  app.user.name = newValue; 
	});

### 复选框和单选框应用 ###

新建一个checkbox.html文件，输入下面的代码：

![](http://pic.yupoo.com/reicky_v/DyxWNM7l/medium.jpg)

在浏览器中运行这个文件，点选复选框会看到下面这张图的效果：

![](http://pic.yupoo.com/reicky_v/DyxXG0Hn/BAs7.gif)

如果是单选按钮，我们可以这样做：

在模板中输入下面的代码：

![](http://pic.yupoo.com/reicky_v/Dyy3hsr1/Drr4D.jpg)

从上面图中的代码可以看到我们把单选框的**name**属性的值设置为**color**变量，当单选框被选中的时候单选框的**value**属性的值就会赋给name属性中的color的值。在上面的代码中初始值是**red**，因为value值为red的单选框是默认选中的。

> 在checkbox这个元素中，是用**checked='false'**和**checked=true**来表示未选中和选中状态的。我们可以使用**element.checked=true**这样方法来改变checkbox的状态。

而在ractive中我们可以使用下面的代码来表示checkbox的状态，如果是选中状态则**checked**的值是**true**，反之则为**false**。

![](http://pic.yupoo.com/reicky_v/DyyGqISS/medium.jpg)

新建一个checkbox.html文件，输入下图所示的代码：

![](http://pic.yupoo.com/reicky_v/DyyHnnMI/medium.jpg)

在浏览器中运行，会得到下面这样的效果：

![](http://pic.yupoo.com/reicky_v/DyyIAzy8/medium.jpg)

你也可以在控制台里运行下面的代码来改变checkbox状态：

    ractive.set( 'checked', true );
	ractive.set( 'color', 'green' );

### 下拉框的应用 ###

下拉框在web开发中也是一个常见的功能。

新建一个select.html文件输入下面的代码：

![](http://pic.yupoo.com/reicky_v/DyyMPDCT/HIRHx.jpg)

然后在浏览器中运行该文件就会得到下面的效果，选中下拉框的选项的时候，文字会实时更新：

![](http://pic.yupoo.com/reicky_v/DyyObb86/13gSMb.gif)
    
这里注意一下在模板中我们可以发现图中红圈圈标明的代码：

![](http://pic.yupoo.com/reicky_v/DyyPTgoy/hXYsR.jpg)

> 两个花括号中间一点，这个也是起到迭代循环的作用，从当前指定的数组中循环迭代。你也可以使用this代替点。由于下拉框需要指定一个默认的选项值，所以在js中我们指定第一个颜色为初始值。

### 多重选择 ###

在一些web开发需求中，我们会需要开发在下拉框中能够选择多个值这样的需求，我们可以使用Ractive提供的多重选择的这个属性来实现，非常方便。

![](http://pic.yupoo.com/reicky_v/Dyz9aAQ7/pXzIR.jpg)

新建一个select2.html文件，如下所示：

![](http://pic.yupoo.com/reicky_v/Dyz1h4cC/2rWV5.jpg)

就可以选择多个值啦。

![](http://pic.yupoo.com/reicky_v/Dyz2hTTP/lZQ26.gif)

关于双向绑定的就先到这里，下篇继续。