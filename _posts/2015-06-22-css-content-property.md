---
layout: post
title: CSS 'content'属性在web开发中的应用
categories:
- Life
tags:
- css3
- 前端

---

> 作为前端开发来说，对于伪元素应该很熟悉了。平时在开发中也使用多很频繁，比如使用很多的**before**和**after**就属于伪元素。所谓伪元素，即将伪元素添加到选择器中允许在不添加额外声明的情况下为文档的某些部分添加样式。 例如，使用 ::first-line 伪元素会匹配由选择器中所指定的元素的第一行。如果你对伪元素还不是很熟悉，你可以去[这篇文章](http://www.smashingmagazine.com/2011/07/13/learning-to-use-the-before-and-after-pseudo-elements-in-css/)看看。今天主要来谈谈**content**属性，它主要是跟**before**和**after**这两个伪元素配合使用，[原文地址](http://www.sitepoint.com/understanding-css-content-property)。

**content**属性主要用来使用指定在某些元素上生成一些额外的内容，并且这个属性大部分的[浏览器都支持](http://caniuse.com/#feat=css-gencontent)。

### 基本语法

**content**支持下面这些内容：


```
p::before { content: normal|counter|attr|string|open-quote|url|initial|inherit;
}
```

当然不同的CSS的属性用法也略微不同。比如，配合**before**和**after**使用**attr()**属性，在CSS中得这样来：


```
p::after {
  content: " (" attr(title) ")";
}
```

下面，我们会针对每一个属性的使用做一个详细的阐述。

### Value:none或者normal

当**content**的值为**none**的时候，将不会生成任何内容。如果设置为**normal**的话，效果跟值为**none**是一样的。


```
p::before {
  content: normal;
}
 
p::after {
  content: none;
}
```

这种情形一般是在你想覆盖一些嵌套元素中已经定义了伪元素的情况下使用。

### Value:**string**

**string**主要是用来定义一些字符串的内容，主要是文本内容。当然，如果是使用的非拉丁字符串，需要经过编码才能生效。下面来看一个例子：


```
	<h2>Tutorial Categories</h2>
		<h2>Tutorial Categories</h2>
		<ol>
  		<li>HTML and CSS</li>
  		<li class="new">Sass &amp; Less</li>
  		<li>JavaScript</li>
		</ol>
 
	<p class="copyright">SitePoint, 2015<p>

```

然后是CSS：


```
.new::after {
  content: " New!";
  color: green;
}
 
.copyright::before {
  content: "\00a9  ";
}
```

这里，给这个列表生成了一些文本内容，以及经过编码的字符（版权那个符号）。

<p data-height="268" data-theme-id="0" data-slug-hash="pJeLGP" data-default-tab="result" data-user="SitePoint" class='codepen'>See the Pen <a href='http://codepen.io/SitePoint/pen/pJeLGP/'>Content Property with Text</a> by SitePoint (<a href='http://codepen.io/SitePoint'>@SitePoint</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

字符串内容必须使用引号包裹起来，单引号或者双引号都可以。

### Value:**url**

当你想为特定的网址显示图标的时候，**url**就派上用场了。


```
<a class="sp" href="http://www.sitepoint.com/">SitePoint</a>
```

CSS代码，为特定网址显示特定的图标：


```
.sp::before {
  content: url(http://www.sitepoint.com/favicon.ico);
}
```

<p data-height="268" data-theme-id="0" data-slug-hash="bdqvPe" data-default-tab="result" data-user="SitePoint" class='codepen'>See the Pen <a href='http://codepen.io/SitePoint/pen/bdqvPe/'>Content Property with url()</a> by SitePoint (<a href='http://codepen.io/SitePoint'>@SitePoint</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

### Value:**content()**或者**counters()**

这两个值是**content**属性稍微复杂一点的也很有趣的两个值。有两种不同写法**content()**或者**counters()**。详细的可以去[这篇文章](http://www.sitepoint.com/understanding-css-counters-and-their-use-cases/)看看。

**counter()**，所生成的文本是你指定的伪元素选择器范围内的一个计数器，它会生成格式化的阿拉伯数字。有序列表ol下的每一个li元素都会默认有数字编号，而 counter 则可以完成更加复杂的嵌套列表的序号。

而计数器自动增长的数值是由css的两个属性控制的，他们是counter-reset 和counter-increment。计数器由这些属性定义，然后用counter() 和 counters()方法获取内容属性。

counter-reset计数器重置属性可以包含一个或多个计数器的名字（例如“标识符”），后面可以跟一个可选的整数参数。通过counter-increment属性设置增长的整数值，可以作用在任何元素。他的默认值是0.负值是容许的。

counter-increment计数器增量属性是类似的，他们基本的不同是这个增加一个计数器，其默认增量是1负值是容许的。

counter-increment这个属性在默认情况下，是让计数器自增1，但也可以这样同时创建多个计数器，并赋予不同的自增减值：

详细的可以去[MDN这个地址](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Counters#Nesting_counters)看看。

先来看一个例子：


```
	<h2>Name of First Four Planets</h2>
	<p class="planets">Mercury</p>
	<p class="planets">Venus</p>
	<p class="planets">Earth</p>
	<p class="planets">Mars</p>
```

CSS代码：


```
	.planets {
  		counter-increment: planetIndex;
	}
 
	.planets::before {
  		content: counter(planetIndex) ". ";
	}
```

效果如下所示：

<p data-height="268" data-theme-id="0" data-slug-hash="qdPGvz" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/qdPGvz/'>qdPGvz</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

使用上面的代码，我们很容易为一些有条理的内容生成序列数字。下面再来看一个使用**counter**的例子：

<p data-height="268" data-theme-id="0" data-slug-hash="bdeOKJ" data-default-tab="result" data-user="SitePoint" class='codepen'>See the Pen <a href='http://codepen.io/SitePoint/pen/bdeOKJ/'>CSS Counters drag-and-drop demonstration</a> by SitePoint (<a href='http://codepen.io/SitePoint'>@SitePoint</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

### Value:**attr()**

**attr()**使用来给指定的属性使用的。如果指定的元素没有制定的属性，将会返回一个空值。

来个例子：


```
	<ul>
  		<li><a href="http://www.sitepoint.com/html-css/">HTML 			and CSS</a></li>
  		<li><a href="http://www.sitepoint.com/				javascript">JavaScript</a></li>
  		<li><a href="http://www.sitepoint.com/mobile/">Mobile</		a></li>
	</ul>
```

上面的代码中，a标签有**href**这个属性，就可以使用**attr**这个属性：


```
	a::after {
  		content: " (" attr(href) ") ";
	}
```

<p data-height="268" data-theme-id="0" data-slug-hash="gpmzYp" data-default-tab="result" data-user="SitePoint" class='codepen'>See the Pen <a href='http://codepen.io/SitePoint/pen/gpmzYp/'>Content Property using attr()</a> by SitePoint (<a href='http://codepen.io/SitePoint'>@SitePoint</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

它会直接把**href**的值取出来，显示在网页上。

### Value:**open-quote**或者是**close-quote**

当使用**open-quote**或者是**close-quote**值的时候，**content**会生成开始或者是闭合的双引号。一般使用在**q**标签上，当然使用在其它标签上也没有问题：

```
		blockquote::before {
  			content: open-quote;
		}
 
		blockquote::after {
  			content: close-quote;
		}
```

<p data-height="268" data-theme-id="0" data-slug-hash="yNMjOp" data-default-tab="result" data-user="SitePoint" class='codepen'>See the Pen <a href='http://codepen.io/SitePoint/pen/yNMjOp/'>Content Property with open/close quotes</a> by SitePoint (<a href='http://codepen.io/SitePoint'>@SitePoint</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

**close-quote**值值能使用在**::after**元素上，而**open-quote**只能使用在**::before**元素上。

### Value:**no-open-quote**或者是**no-close-quote**

**no-open-quote**是用来删除指定元素起始的双引号的，**no-close-quote**则用来删除闭合双引号的。你可能会觉得很无聊，能有什么用呢？先来看看下面这个实例：


```
		<p><q>A wise man once said: <q>Be true to yourself,
			but don't listen to those who say <q>Don't be true 			to 
		yourself.</q></q> That is good advice.</q></p>
```

从上面到代码中可以看到，这段文字嵌套了三层的引号。如果，使用**content**属性来生成引号，那如何确保嵌套正确呢？


```
		q {
  		quotes: '“' '”' '‘' '’' '“' '”';
		}
 
		q::before { 
  		content: open-quote;
		}
 
		q::after {
  		content: close-quote;
		}
```

第一行代码定义我们针对引用文本想要使用引号的类型。然后使用伪元素来生成这些引号。

<p data-height="268" data-theme-id="0" data-slug-hash="RPpyVW" data-default-tab="result" data-user="SitePoint" class='codepen'>See the Pen <a href='http://codepen.io/SitePoint/pen/RPpyVW/'>Content property with nested quotes</a> by SitePoint (<a href='http://codepen.io/SitePoint'>@SitePoint</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

但是，如果我们想要第二层的引用文本不需要但引号该怎么办呢？这个时候**no-open-quote**和**no-close-quote**就派上用场了：


```
		.noquotes::before {
  			content: no-open-quote;
		}
 
		.noquotes::after {
  			content: no-close-quote;
		}
```

我们定义了**noquotes**这个类。用它来控制引号的现实与否。这样一来，就可以自由的控制引号了。

<p data-height="268" data-theme-id="0" data-slug-hash="oXZdWK" data-default-tab="result" data-user="SitePoint" class='codepen'>See the Pen <a href='http://codepen.io/SitePoint/pen/oXZdWK/'>Content property with no-open-quote/no-close-quote</a> by SitePoint (<a href='http://codepen.io/SitePoint'>@SitePoint</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

### 总结

CSS标准不断的在完善和发展，在平时的开发中能有很多的应用。就比如今天说的这个**content**属性，如果平时不细心挖掘，我是万万想不到有这么多的可以应用的场景。

下面推荐一些关于**content**属性有用的资料：

[The content property on W3C](http://www.w3.org/TR/CSS21/generate.html#content)

[The content property on MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/content)

[The quote property on MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/quotes)













