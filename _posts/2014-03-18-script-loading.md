---
layout: post
title: 使用labjs来加载脚本
categories:
- Life
tags:
- javascript
---

> 现在web开发中javascript和HMTL以及CSS呈三足鼎立之势，互相依赖，而且随着技术的发展javascript在web中占据这越来越重要的位置。而且现在越来越多的网站和应用正在依赖着复杂的Javascript资源系统，在web开发中，因为浏览器赋予了script标签一些不利的性能特征-脚本阻塞，这意味着JavaScript脚本在加载和执行的时候，页面上的其他脚本（包括其他JavaScript脚本）和行为（如页面渲染）将会停止。所以，如果你有10个script标签，他们会依照顺序依次加载执行，这就减慢了整个页面的加载速度。在开发中，我们一般会把脚本放在页面的底部加载来降低脚本加载对页面加载速度的影响。那还有其它的解决方法么？

当然有现在也出现了很多的脚本加载解决方案，流行的有requirejs以及国内的seajs，不过它们出现的主要目的是为了解决javascript模块化编程而出现的，顺带也提供了脚本的加载的解决方案，对于大型的web应用程序来说requirejs和seajs都是一个非常好的选择。不过如果我们只是需要一个脚本加载器的话，使用requirejs和seajs未免大材小用正所谓杀鸡焉用牛刀。

labjs就是专门为脚本加载而生的。它的一个设计目标就是代替传统的**script**标签来加载脚本。下面我们就来学习labjs的使用方法。

首先是下载labjs文件，下载地址[labjs](http://labjs.com/)或者是它的[github](http://github.com/getify/LABjs)。

我们来看看传统的脚本加载的方式：

    <script src="jquery.js"></script>
	<script src="lightface/Source/LightFace.js"></script>
	<script src="lightface/Source/LightFace.Request.js"></script>
	<script src="lightface/Source/LightFace.Static.js"></script>
	<script src="Color.js"></script>

像在现在的web中加载5、6个脚本是家常便饭的事。由于脚本阻塞这个特性，脚本加载的数量对web性能有着重要的影响。labjs就是奔着提高脚本加载速度这个目的来的。

当然我们需要在页面中引入labjs文件：

    <script src="LAB.js"></script>

$LAB对象是LABjs内部对象中最重要的一个，通过它调用.script()会产生一条LABjs链，调用wait()会锁住这些链和初始$LAB的引用。如下所示:

    $LAB.script('jquery.js');

wait()方法可以用来处理脚本之间的加载的顺序，比如在web开发中我们会使用一些第三方的jQuery的插件，这些插件依赖jQuery，所以我们可以使用**wait()**方法来指定只有先加载完jQuery文件的时候才执行jQuery插件文件，如下所示：

    $LAB
	.script('jquery.js').wait()
	.script('jquery-dependent-script.js');

wait(...)函数同样可以接收参数，如果一个函数作为参数传进去了，那么这个函数将会被看作内联script标签中的代码。然后将这个内联脚本片段插入到了位于.wait()之前需要立即执行的脚本后。

### LABjs设置选项 ###

一些高级的使用方法可以通过配置$LAB.setOptions来设置。如:

    $LAB.setOptions({ AlwaysPreserveOrder:true });

下面我们来看看具体的一些设置选项的含义。

- AlwaysPreserveOrder: AlwaysPreserveOrder一个布尔值(默认值为false),控制是否一个隐式空wait()调用假定每个脚本加载后,基本上所有的脚本在链条部队执行串行顺序(加载并联,默认情况下,不受此设置)。
- BasePath 设置本地脚本的基本路径

基本的使用方法就是这样的了，更多的信息可以去[官方的文档](http://labjs.com/documentation.php)看看。

对于一些小型的web程序来说，使用labjs来管理脚本加载不失为一个好选择方案。

