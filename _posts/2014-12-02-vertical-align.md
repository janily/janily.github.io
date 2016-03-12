---
layout: post
title: 关于vertical-align，你需要知道的一些事儿
categories:
- Life
tags:
- css
- 翻译

---

> 在web前端开发中，我们经常用到**vertical-align**来对齐元素。那关于它你又知道多少呢？这篇文章我们就来唠叨唠叨**vertical-align**这个属性。原文来自[All You Need To Know About Vertical-Align](http://christopheraue.net/2014/03/05/vertical-align/)。

在web开发中，我们经常要对齐一些元素。有时候，我们会使用**float**来对齐元素；也有可能使用**position:absolute**来对齐；有时候也会使用诸如margin或者是padding这些技巧来对齐元素。

对于这些方法，我觉的都是一些旁门左道，不值得提倡。**float**也只能使元素顶部对齐并且还要清除浮动所带来对其它元素的影响。而使用绝对定位则会使元素脱离文档。使用margin和padding也会改变元素的一些属性。

对于对齐元素，我们一直忽视了**vertical-align**这个属性，这个属性才是用来对齐元素的正统所在。不过你要是用它来布局的话，那可是用错地方了。一个萝卜一个坑，它就是用来对齐的。当然，如果对它深入理解的话，那我们就可以很轻松的驾驭它，在不同环境下用来对齐元素。特别是对于不同尺寸的元素，使用它来对齐更是一个非常不错的选择。

### vertical-align的特性

不过要用好**vertical-align**这个属性也不是那么容易的，得理解它的原理才能用好它。关于**vertical-align**这个属性，网络上很多的资料对它的说明也都是浅尝则止，并没有深入的剖析它。下面我们就一步一步来剖析它的原理，为更好的驾驭它打下坚实的基础。

怎么来深入理解它呢？我们就从W3C规范开始，可以去看看这两个链接[CSS](http://www.w3.org/TR/CSS2/visudet.html#line-height),[inline](http://www.w3.org/TR/CSS2/visuren.html#inline-formatting)。

开始吧！

### vertical-align使用的条件

**vertical-align**主要是用于[行内元素](http://www.w3.org/TR/CSS2/visuren.html#inline-level)，对应css中的display属性的值包含以下三个：

* inline
* inline-block
* inline-table(在开发中不推荐使用它)

像一般的文本就是基本的行内元素。

**inline-block**元素，从名字就可以猜出：同时具有块状和行内元素属性的元素，可以设置宽高、padding、margin以及边框。

行内元素不可以设置宽（width）和高（height），但可以与其他行内元素位于同一行，行内元素内一般不可以包含块级元素。行内元素的高度一般由元素内部的字体大小决定，宽度由内容的长度控制。常见的行内元素有a, em ,strong等。

行内元素会依次排在一起，这样就形成来一个**line box**。不同尺寸的也意味着**line box**有不同的高度，如下图所示，红线之间就是一个line box：

![image](http://pic.yupoo.com/reicky_v/EgcI4agA/medium.jpg)

在每一个line box中，我们都可以使用vertical-align来对齐line box之中的元素。

### 关于行内元素的baseline和outer edges

对于要对齐的元素来说baseline是一个非常重要的属性，它才是关键所在。当然在一些情况下，元素的top line和bottom line着两条线也非常重要。说了这么多的线，头都晕啦。看了下面这张图就明白了。

![image](http://pic.yupoo.com/reicky_v/EgcN4lpl/medium.jpg)

![image](http://pic.yupoo.com/reicky_v/EgcI3Osv/medium.jpg)

看上面的三张图分别表示不同的line box。顶部和底部的红线表示行高(line height)，绿色的线表示字体的高度，蓝色的线则是基线即base line。在最左边的图中，文字的行高和字体的font-size的值是一样的。所以绿色的线和红色的线会合并到一起。在中间的图中，行高的值是font-size值的两倍。在右边，行高的值是font-size值的一半。

行内元素的顶部和底部一般和行高对齐。不过，如果行高小于字体的大小，那就会出现上面右边图中的情况。

行内元素的基线(baseline)就在文字的底部，关于更多的描述可以去[w3c看看](http://www.w3.org/TR/CSS2/visudet.html#leading)。

![image](http://pic.yupoo.com/reicky_v/EgcI4p1A/medium.jpg)

上图分别是行内元素在三中不同情况下，基线(baseline)的表现：第一种是，正常的文档流中文字基线的表现形式；第二种是使用了**overflow:hidden**属性后的表现形式；第三种是内容为空的时候，基线的表现形式。红线是margin，边框是黄色，绿色表示padding，蓝色表示内容区域。

**行内元素的基线主要与下面这些相互影响**

* 在正常的文档流的情况下，基线(baseline)就回紧跟在内容元素的下面，如左图所示。
* 不过，当使用了overflow并且值为visible以外的值的时候，基线(baseling)就会跟**margin-box**底部的线重合，如上图中中间的图所示。
* 当内容为空的时候，基线(baseling)就会跟**margin-box**底部的线重合，如上图中左边的图所示。

![image](http://pic.yupoo.com/reicky_v/EgcI4zJu/medium.jpg)

从上图可以看出，我给文字的顶部和底部分别画了两条绿色的线，还画了一条蓝色的基线(baseline)，并且给每一个文字定义了灰色的背景。

可以发现，绿色的线分别对齐了文字的顶部和底部。

**不过W3C标准中并没有定义line box基线(baseline)的位置。**

这在我们使用**vertical-align**这一属性的时候，会给我们造成些许困扰。同时也意味着，我们只需要满足基线的条件，就可以使用它来对齐元素。它是一个自由参数，我们可以随意的控制它。

在line box中基线(baseline)是不可见的，不过我们变通一下可以使它可见。就如我上图中添加一个字符**X**，如果这个字符没有应用任何的对齐属性，那它的位置默认就是基线(baseline)的位置。

像这种只有文字构成的line box我们一般称之为**text box**。text box在line box不会带有任何的对齐属性。它的上下边界是由line height来定义的。当text box和基线(baseline)关联起来的时候，它的位置就会随着基线(baseline)的位置的改变而改变。([strunt](http://www.w3.org/TR/CSS2/visudet.html#strut))。

下面我们来总结下两个重要的知识点：

* line box。由基线(baseline)、text box以及上下边界组成。
* 它是行内元素，它们都有对齐的属性，也由基线(baseline)、以及上下边界组成。

### vertical align的值

下面，我们就来看看vertical align都有哪些值，先来看一张图：

![image](http://pic.yupoo.com/reicky_v/EgcI5hG8/medium.jpg)

* **baseline:**默认。元素的基线与父元素的基线对齐
* **sub:**降低元素的基线到父元素合适的下标位置。
* **super:**升高元素的基线到父元素合适的上标位置。
* **percentage:**通过距离（相对于1line-height1值的百分大小）升高（正值）或降低（负值）元素。'0%'等同于'baseline'。
* **length:**过距离升高（正值）或降低（负值）元素。'0cm'等同于'baseline'

### vertical-align:middle

![image](http://pic.yupoo.com/reicky_v/EgyZ18IY/medium.jpg)

当值为middle的时候，这个居中对齐的点的值就是line box的基线加上X字符高度一半的这个位置。

### text-top和text-bottom

![image](http://pic.yupoo.com/reicky_v/EgyZ1BRb/medium.jpg)

**text-top:**把元素的顶端与父元素内容区域的顶端对齐。

**text-bottom:**把元素的底端与父元素内容区域的底端对齐。

### top和bottom

![image](http://pic.yupoo.com/reicky_v/Egz367hm/medium.jpg)

**top:**把对齐的子元素的顶端与line box顶端对齐。

**bottom:**把对齐的子元素的底端与line box底端对齐。

详细信息可以去这个[地址](http://www.w3.org/TR/CSS2/visudet.html#propdef-vertical-align)了解。

### vertical align实际应用

#### 居中对齐icon

比如，我这里想对齐icon和文字，这个在开发中经常见到。只需要使用**vertical-align:middle**就可以来么，我们实际来看一看：

先上代码：

	<!-- left mark-up -->
	<span class="icon middle"></span>
	Centered?

	<!-- right mark-up -->
	<span class="icon middle"></span>
	<span class="middle">Centered!</span>

	<style type="text/css">
  	.icon   { display: inline-block;
            /* size, color, etc. */ }

  	.middle { vertical-align: middle; }
	</style>
	
结果如下图所示：

![image](http://pic.yupoo.com/reicky_v/Egz6riyU/medium.jpg)

下面的图我把相关的基线也画出来，这样我们就更能理解对齐的属性了。

![image](http://pic.yupoo.com/reicky_v/Egz75YMi/medium.jpg)

我们首先来看左边这张图，看起来有点怪，因为它没有完全的居中对齐，它只是对齐了基线。

而右边这张图，我们把icon和字符都定义了vertical-align:middle的属性。这样文本的基线就会低于line box的基线从而实现了居中对齐的效果。

### 移动line box的基线(baseline)

当我们使用**vertical-align**属性的时候，我们要做注意在line box中的元素会影响到基线(baseline)的位置。比如，当元素以某种方式排列的时候，会使line box的基线(baseline)移动除了(top和bottom)。我们来看一些实际的例子：

* 如果这儿有一个和line box一样高的元素，那么**vertical-align**属性将不会对它产生影响。因为没有空间能给基线(baseline)来移动。如下图所示，在左边的图中，高一点的元素使用**text-bottom**对齐的属性。而在右边的图中，则使用了**text-top**对齐属性。你可以看到基线(baseline)随着不同的对齐属性而移动。

![image](http://pic.yupoo.com/reicky_v/EgI60UrK/medium.jpg)

代码如下所示：

	<!-- left mark-up -->
	<span class="tall-box text-bottom"></span>
	<span class="short-box"></span>

	<!-- right mark-up -->
	<span class="tall-box text-top"></span>
	<span class="short-box"></span>

	<style type="text/css">
  	.tall-box,
  	.short-box   { display: inline-block;
                /* size, color, etc. */ }

  	.text-bottom { vertical-align: text-bottom; }
  	.text-top    { vertical-align: text-top; }
	</style>
	
* 如果把**vertical-align**的值设置为**bottom**(左图)和**top**(右图)，可以发现基线(baseline)也移动了。

![image](http://pic.yupoo.com/reicky_v/EgI9jOCd/medium.jpg)

	<!-- left mark-up -->
	<span class="tall-box bottom"></span>
	<span class="short-box"></span>

	<!-- right mark-up -->
	<span class="tall-box top"></span>
	<span class="short-box"></span>

	<style type="text/css">
  	.tall-box,
  	.short-box { display: inline-block;
               /* size, color, etc. */ }

  	.bottom    { vertical-align: bottom; }
  	.top       { vertical-align: top; }
	</style>
	
我们再来看看下面三个例子：

![image](http://pic.yupoo.com/reicky_v/EgIfqc2M/medium.jpg)

	<!-- left mark-up -->
	<span class="tall-box text-bottom"></span>
	<span class="tall-box text-top"></span>

	<!-- mark-up in the middle -->
	<span class="tall-box text-bottom"></span>
	<span class="tall-box text-top"></span>
	<span class="tall-box middle"></span>

	<!-- right mark-up -->
	<span class="tall-box text-bottom"></span>
	<span class="tall-box text-top"></span>
	<span class="tall-box text-100up"></span>

	<style type="text/css">
  	.tall-box    { display: inline-block;
                 /* size, color, etc. */ }
                   
  	.middle      { vertical-align: middle; }
  	.text-top    { vertical-align: text-top; }
  	.text-bottom { vertical-align: text-bottom; }
  	.text-100up  { vertical-align: 100%; }
	</style>
	
### 一些小问题

先看看下面图，你会发现li元素的底部有小的缝隙。

![image](http://pic.yupoo.com/reicky_v/EgIlp6Gt/medium.jpg)

	<ul>
  		<li class="box"></li>
  		<li class="box"></li>
  		<li class="box"></li>
	</ul>

	<style type="text/css">
  		.box { display: inline-block;
         /* size, color, etc. */ }
	</style>
	
从图中可以看出，li元素为了留空间给文字和基线(baseline)，所以下面才会有空隙。那怎么来解决空隙问题呢？我们可以使用**vertical-align**来移动基线(baseline)就可以了，只要设置值为**middle**就可以了。

![image](http://pic.yupoo.com/reicky_v/EgIptbJ3/medium.jpg)

	<ul>
  		<li class="box middle"></li>
  		<li class="box middle"></li>
  		<li class="box middle"></li>
	</ul>

	<style type="text/css">
  	.box    { display: inline-block;
            /* size, color, etc. */ }
            
  	.middle { vertical-align: middle; }
	</style>
	
### 行内元素空袭问题

真正意义上的inline-block水平呈现的元素间，换行显示或空格分隔的情况下会有间距；使用CSS更改非inline-block水平元素为inline-block水平，也会有该问题。这种表现是符合规范的应该有的表现。

不过，这类间距有时会对我们布局，或是兼容性处理产生影响，需要去掉它，该怎么办呢？

元素间留白间距出现的原因就是标签段之间的空格，因此，去掉HTML中的空格，自然间距就木有了。考虑到代码可读性，显然连成一行的写法是不可取的，我们可以借助HTML注释来去除空格：比如下面的例子：

![image](http://pic.yupoo.com/reicky_v/EgIvGnhR/medium.jpg)

	<!-- left mark-up -->
	<div class="half">50% wide</div>
	<div class="half">50% wide... and in next line</div>

	<!-- right mark-up -->
   	<div class="half">50% wide</div><!--
	--><div class="half">50% wide</div>

	<style type="text/css">
  	.half { display: inline-block;
          width: 50%; }
	</style>
	
### 总结

OK，明白了原理后，也不是那么复杂。如果，**vertical-align**没有生效，可以从下面两个方面来检测：

* 分析line box中基线(baseline)、上下边界的位置
* 分析行内元素(inline elements)中基线(baseline)、上下边界的位置

从上面两个方面来分析就可以很好的解决问题。


















