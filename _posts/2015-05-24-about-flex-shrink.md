---
layout: post
title: flexbox之flex-shrink属性使用探索
categories:
- Life
tags:
- css3
- flexbox
- 前端

---

> 在新的伸缩盒内容中有一个flex-shrink的属性，以前没怎么注意过。刚好看到一篇关于它的介绍文章，趁机好好来学习下它。原文[链接地址](http://codepen.io/noahblon/blog/practical-guide-to-flexbox-dont-forget-about-flex-shrink)。

当我开始学习flexbox的时候，我完全被它的功能所吸引。

但是，最近 在进一步的学习过程中，我发现flexbox中的flex-shrink被严重的忽略了。下面我们来看看一些实例，你就知道我为什么这样说啦。

首先来解释下**flex-shrink**这个属性下。**flex-shrink**即设置或检索弹性盒的收缩比率，根据弹性盒子元素所设置的收缩因子作为比率来收缩空间。

比如：a,b,c将按照1:1:3的比率来收缩空间

	<ul id="flex">
		<li>a</li>
		<li>b</li>
		<li>c</li>
	</ul>
	
css:

	#flex{display:-webkit-flex;display:flex;width:400px;margin:0;padding:0;list-style:none;}
	#flex li{width:200px;}
	#flex li:nth-child(1){background:#888;}
	#flex li:nth-child(2){background:#ccc;}
	#flex li:nth-child(3){-webkit-flex-shrink:3;flex-shrink:3;background:#aaa;}
	
flex-shrink的默认值为1，如果没有显示定义该属性，将会自动按照默认值1在所有因子相加之后计算比率来进行空间收缩。

本例中c显式的定义了flex-shrink，a,b没有显式定义，但将根据默认值1来计算，可以看到总共将剩余空间分成了5份，其中a占1份，b占1份，c占3分，即1:1:3。

我们可以看到父容器定义为400px，子项被定义为200px，相加之后即为600px，超出父容器200px。那么这么超出的200px需要被a,b,c消化。

按照以上定义a,b,c将按照1:1:3来分配200px，计算后即可得40px,40px,120px，换句话说，a,b,c各需要消化40px,40px,120px，那么就需要用原定义的宽度相减这个值，最后得出a为160px，b为160px，c为80px。

一个典型的应用实例是，弹性盒子元素会平均地分布在行里。如下所示：

<p data-height="268" data-theme-id="0" data-slug-hash="mJEPZV" data-default-tab="result" data-user="noahblon" class='codepen'>See the Pen <a href='http://codepen.io/noahblon/pen/mJEPZV/'>Figure 1</a> by Noah Blon (<a href='http://codepen.io/noahblon'>@noahblon</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

我们使用**display: flex**创建了一个伸缩盒子。并且使用**justify-content: space-between**来使弹性盒子元素会平均地分布在行里。一切好像都按照理想中的模样展现出来，如果我们添加更多的子元素会怎么样呢？

<p data-height="268" data-theme-id="0" data-slug-hash="GJqZbZ" data-default-tab="result" data-user="noahblon" class='codepen'>See the Pen <a href='http://codepen.io/noahblon/pen/GJqZbZ/'>Figure 2</a> by Noah Blon (<a href='http://codepen.io/noahblon'>@noahblon</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

正如，你所看到的，元素都被压缩变形了！这可不是我们想要的。

为什么会这样呢？这是因为**flex-shrink**默认的值是**1**。flex-shrink的默认值为1，如果没有显示定义该属性，将会自动按照默认值1在所有因子相加之后计算比率来进行空间收缩。

要修复它也很简单：把子元素设置**flex-shrink:0**就可以啦。并且把父元素设置为**overflow:auto**，这样内容就可以滚动啦。

<p data-height="268" data-theme-id="0" data-slug-hash="ZGOWdj" data-default-tab="result" data-user="noahblon" class='codepen'>See the Pen <a href='http://codepen.io/noahblon/pen/ZGOWdj/'>Figure 3</a> by Noah Blon (<a href='http://codepen.io/noahblon'>@noahblon</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

再来看一个稍微复杂点的实例，就是模拟本地应用的样式：

<p data-height="268" data-theme-id="0" data-slug-hash="BNjjvj" data-default-tab="result" data-user="noahblon" class='codepen'>See the Pen <a href='http://codepen.io/noahblon/pen/BNjjvj/'>Figure 4</a> by Noah Blon (<a href='http://codepen.io/noahblon'>@noahblon</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

这里的头部和底部都是固定尺寸的，并且主内容区域是占据剩余的空间。当内容超过当前视图区域的时候，内容自动滚动。

为了达到这个目的，我们设置**.app**的高度为当前视图区域的高度，还使用**flex-direction**设置布局的方式是垂直方向布局。头部和底部是固定尺寸的，内容区域占据剩余的空间。不像**flex-shrink**默认值为1，**flex-grow**的默认值为0，这就意味着子元素不会分配剩余的空间。

但是，这里有一个问题。当**.main**内容区域滚动的时候，我们想保持它的可视区域的范围不变。

<p data-height="268" data-theme-id="0" data-slug-hash="NqrYra" data-default-tab="result" data-user="noahblon" class='codepen'>See the Pen <a href='http://codepen.io/noahblon/pen/NqrYra/'>Figure 5</a> by Noah Blon (<a href='http://codepen.io/noahblon'>@noahblon</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

我们会发现空间都自动收缩啦。这里有两个原因。第一个是，伸缩盒的子元素的**flex-shrink**的默认值是1。也就是说将会自动按照默认值1在所有因子相加之后计算比率来进行空间收缩。第二个原因是，**.main**内容区域也参与到了伸缩盒的计算。因为**flex-basis**的默认值是auto(设置或检索弹性盒伸缩基准值。如果所有子元素的基准值之和大于剩余空间，则会根据每项设置的基准值，按比率伸缩剩余空间)，如果没有声明的话，意味着伸缩盒在计算的时候，会把当前的可视区域都计算进去。

修复这个也很简单，只要把头部和底部设置下**flex-shrink:0**就可以了。

<p data-height="268" data-theme-id="0" data-slug-hash="WvxzMJ" data-default-tab="result" data-user="noahblon" class='codepen'>See the Pen <a href='http://codepen.io/noahblon/pen/WvxzMJ/'>Figure 6</a> by Noah Blon (<a href='http://codepen.io/noahblon'>@noahblon</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

在使用flex进行布局的时候，有两个需要注意的地方：第一个是哪些元素是需要保持不变的；哪些是需要伸缩可变的。

更多的关于flexbox的可以去[这里](http://codepen.io/noahblon/blog/)瞧瞧。或者是[flow codepen](http://codepen.io/noahblon/)。





