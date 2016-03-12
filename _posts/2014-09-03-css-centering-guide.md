---
layout: post
title: css居中技术完全指南
categories:
- Life
tags:
- css
- 翻译
---

> 在前端开发中使用css来居中html元素是一个常见的开发场景。比如，水平或者是垂直居中一个元素以及水平垂直居中一个元素。方法也非常多，前两天正好看到一篇全面总结css居中技术的文章，原文地址[Centering in CSS: A Complete Guide](http://css-tricks.com/centering-css-complete-guide/)。

### 水平居中

#### 行内元素居中

对于像普通文本或者是链接元素，要使它居中非常容易：

	.center-children {
  	text-align: center;
	}
	
<p data-height="268" data-theme-id="0" data-slug-hash="HulzB" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/chriscoyier/pen/HulzB/'>Centering Inline Elements</a> by Chris Coyier (<a href='http://codepen.io/chriscoyier'>@chriscoyier</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

这对行内元素，行内块状元素，行内表格，行内flex等元素都有效。

#### 块状元素居中

对于已知宽度的块状元素，可以使用**margin:0 auto**这个属性来使它居中：

	.center-me {
  	margin: 0 auto;
	}
	
<p data-height="268" data-theme-id="0" data-slug-hash="eszon" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/chriscoyier/pen/eszon/'>Centering Single Block Level Element</a> by Chris Coyier (<a href='http://codepen.io/chriscoyier'>@chriscoyier</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

需要注意的是，如果对元素使用了浮动的属性，那么居中就会失效。至于原因可以去这篇文章看看[There is a trick though](http://css-tricks.com/float-center/)

#### 多个块状元素居中

如果你有两个以上的元素需要水平居中。可以使用**display**属性来声明元素表现为行内块状元素即**display:inline-block**:

<p data-height="327" data-theme-id="0" data-slug-hash="ebing" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/chriscoyier/pen/ebing/'>Centering Row of Blocks</a> by Chris Coyier (<a href='http://codepen.io/chriscoyier'>@chriscoyier</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

当然，要是元素都是知道宽度的，那就好办啦。**margin: 0 auto**

<p data-height="268" data-theme-id="0" data-slug-hash="haCGt" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/chriscoyier/pen/haCGt/'>Centering Blocks on Top of Each Other</a> by Chris Coyier (<a href='http://codepen.io/chriscoyier'>@chriscoyier</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

### 垂直居中

#### 行内元素垂直居中

使用padding来使它垂直居中：

	.link {
  		padding-top: 30px;
  		padding-bottom: 30px;
	}
	
<p data-height="268" data-theme-id="0" data-slug-hash="ldcwq" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/chriscoyier/pen/ldcwq/'>Centering text (kinda) with Padding</a> by Chris Coyier (<a href='http://codepen.io/chriscoyier'>@chriscoyier</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

如果有时候使用padding不合适的话，可以使用行高**line-height**来使元素垂直居中。

	.center-text-trick {
  		height: 100px;
  		line-height: 100px;
  		white-space: nowrap;
	}
	
<p data-height="268" data-theme-id="0" data-slug-hash="LxHmK" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/chriscoyier/pen/LxHmK/'>Centering a line with line-height</a> by Chris Coyier (<a href='http://codepen.io/chriscoyier'>@chriscoyier</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

#### 多行文字元素居中

使用padding能使多行的文字元素居中，不过也并不是随时都可以用。这时候可以使用display来使元素具有表格元素的属性即**table-cell**，然后使用**vertical－align**为middle来使元素居中，[vertical-align](http://css-tricks.com/what-is-vertical-align/)：

<p data-height="268" data-theme-id="0" data-slug-hash="ekoFx" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/chriscoyier/pen/ekoFx/'>Centering text (kinda) with Padding</a> by Chris Coyier (<a href='http://codepen.io/chriscoyier'>@chriscoyier</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

使用css3中的flex属性也很容易使元素居中。

	.flex-center-vertically {
  		display: flex;
  		justify-content: center;
  		flex-direction: column;
  		height: 400px;		
  	}
  	
<p data-height="268" data-theme-id="0" data-slug-hash="uHygv" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/chriscoyier/pen/uHygv/'>Vertical Center Multi Lines of Text with Flexbox</a> by Chris Coyier (<a href='http://codepen.io/chriscoyier'>@chriscoyier</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

要注意的是这里是相对与父元素的高度来居中的，所以这里设置来父元素的高度。

如果上面的技术都不使用，还有一种称只为**ghost element**的技术来居中，主要是使用伪元素设置元素的高度配合**vertical－middle**属性来使元素居中：

	.ghost-center {
  		position: relative;
	}
	.ghost-center::before {
  		content: " ";
  		display: inline-block;
  		height: 100%;
  		width: 1%;
  		vertical-align: middle;
	}
	.ghost-center p {
  		display: inline-block;
  		vertical-align: middle;
	}
	
<p data-height="268" data-theme-id="0" data-slug-hash="ofwgD" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/chriscoyier/pen/ofwgD/'>Ghost Centering Multi Line Text</a> by Chris Coyier (<a href='http://codepen.io/chriscoyier'>@chriscoyier</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

#### 块状元素居中

如果知道一个元素的高度，那可以这样来做：

	.parent {
  		position: relative;
	}
	.child {
  		position: absolute;
  		top: 50%;
  		height: 100px;
  		margin-top: -50px; /* account for padding and border if not using box-sizing: border-box; */
	}
	
<p data-height="371" data-theme-id="0" data-slug-hash="HiydJ" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/chriscoyier/pen/HiydJ/'>Center Block with Fixed Height</a> by Chris Coyier (<a href='http://codepen.io/chriscoyier'>@chriscoyier</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

##### 如果不知道元素的高度呢？

可以配合使用transform的属性来使它垂直居中：

	.parent {
  		position: relative;
	}
	.child {
  		position: absolute;
  		top: 50%;
  		transform: translateY(-50%);
	}
	
<p data-height="369" data-theme-id="0" data-slug-hash="lpema" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/chriscoyier/pen/lpema/'>Center Block with Unknown Height</a> by Chris Coyier (<a href='http://codepen.io/chriscoyier'>@chriscoyier</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

##### 使用flexbox

使用flexbox就更容易来使元素垂直居中了。

	.parent {
  		display: flex;
  		flex-direction: column;
  		justify-content: center;
	}
	
<p data-height="413" data-theme-id="0" data-slug-hash="FqDyi" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/chriscoyier/pen/FqDyi/'>Center Block with Unknown Height with Flexbox</a> by Chris Coyier (<a href='http://codepen.io/chriscoyier'>@chriscoyier</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

### 同时水平垂直居中

#### 知道元素的宽高

使用绝对定位就可以使元素水平垂直居中：

	.parent {
  		position: relative;
	}

	.child {
  		width: 300px;
  		height: 100px;
  		padding: 20px;

  		position: absolute;
  		top: 50%;
  		left: 50%;

  		margin: -70px 0 0 -170px;
	}
	
<p data-height="354" data-theme-id="0" data-slug-hash="JGofm" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/chriscoyier/pen/JGofm/'>Center Block with Fixed Height and Width</a> by Chris Coyier (<a href='http://codepen.io/chriscoyier'>@chriscoyier</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

#### 不知道元素的宽高

如果不知道元素的宽高，可以使用transform中的translate属性来使之居中：

	.parent {
  		position: relative;
	}
	.child {
  		position: absolute;
  		top: 50%;
  		left: 50%;
  		transform: translate(-50%, -50%);
	}
	
<p data-height="346" data-theme-id="0" data-slug-hash="lgFiq" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/chriscoyier/pen/lgFiq/'>Center Block with Unknown Height and Width</a> by Chris Coyier (<a href='http://codepen.io/chriscoyier'>@chriscoyier</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

还可以使用flexbox来使元素居中：

	.parent {
  		display: flex;
  		justify-content: center;
  		align-items: center;
	}
	
<p data-height="354" data-theme-id="0" data-slug-hash="msItD" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/chriscoyier/pen/msItD/'>Center Block with Unknown Height and Width with Flexbox</a> by Chris Coyier (<a href='http://codepen.io/chriscoyier'>@chriscoyier</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>






