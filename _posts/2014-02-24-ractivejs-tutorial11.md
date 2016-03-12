---
layout: post
title: 	ractive.js学习笔记(11)——transitions
categories:
- Life
tags:
- javascript
- ractive.js
---

在ractive中提供了两种方法来处理UI层面的动画，一种是animate以及另外的transition。本文中我们就来学学在ractive中使用transition来制作动画效果。

    <div intro='fade'>this div will fade into view</div>

只要在模板中想上面代码中那样声明要使用的动画效果就可以了。在ractive中，都是以插件的形式来管理transition效果的，可以根据自己的需求下载对应的插件。ractive的插件全部在[在这个网页](http://docs.ractivejs.org/latest/plugins)里面可以找到。也可以按照自己的需求来编写ractive的插件。

当然现在浏览器对于transition的支持还不是很全面，尤其是IE，IE9和IE9一下的浏览器都不支持transition形式的动画效果。所以在使用的时候要考虑下这个问题。

来一个实例，新建一个transition.html文件，输入下面的代码：

![](http://pic.yupoo.com/reicky_v/DyQJ8zTf/GJzOV.jpg)

可以注意到代码中我还引入了相关的ractive的transition插件，这样指定的动画效果才能正常工作。如下图所示：

![](http://pic.yupoo.com/reicky_v/DyQLatoE/O4eXa.gif)

上面的是指定的入场的动画方式，我们还可以指定离场的动画方式跟上面的使用方法差不多，只不过是**intro**改为**outro**的方式，如下所示：

    <div intro='fade' outro='fly' class='large button' on-tap='show:2'>Click me!</div>

运行代码，你就会发现不同了。完整代码如下所示：

![](http://pic.yupoo.com/reicky_v/DyQVSX0g/QCgfW.jpg)

在上面的图中注意一下红圈部分的代码。这是因为在动画中，当一个元素从DOM节点中被删除的时候新元素开始渲染的时候，我们需要模拟**remove**事件来删除DOM节点，这样才能保证动画效果的完整性。

在代码中我们是在事件处理中用一个回调函数来解决的。

    ractive.on({
	  show: function ( event, which ) {
	    this.set( 'visible', null, function () {
	      this.set( 'visible', which );
	    });
	  }
	});

> 在ractive中，我们可以使用**ractive.set()、ractive.update()**和**ractive.teardown()**方法来做一些初始化的工作。

在开发动画效果中还可以指定动画运行的一些参数：

    <div intro='fade:{duration:2000}' outro='fly' class='large button' on-tap='show:2'>Click me!</div>

在上面的代码中，我们指定动画运行时间的参数为**duration:2000**，即表示动画需要运行2秒钟。

你也可以像下面这样来指定：

    <div intro='fade:2000' outro='fly' class='large button' on-tap='show:2'>Click me!</div>

你也可以使用如**fast(200毫秒)**或者是**slow(600毫秒)**来指定动画运行的时间，就想jQuery一样。

详细的关于ractive中插件的信息可以去官方的网站的[这个页面](http://docs.ractivejs.org/latest/plugins)看看。





