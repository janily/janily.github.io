---
layout: post
title: css能做的远不止于此
categories:
- Life
tags:
- css3

---

> css3的出现，大大增强了web的表现力。以前在web开发中一旦涉及一些交互表现，就要javascript出马才能搞定。不过css3却改变了这一现状，一些以前需要靠javascript才能实现的效果，现在使用css3也能实现。下面我们就来使用css3来实现一些交互表现效果，仅仅抛砖引玉，其实css3能做的远不止于此。原文链接[You Can Do That With CSS?](http://www.sitepoint.com/you-can-do-that-with-css/)。

### 一个表单交互效果

开始之前可以去看看之前的一篇文章[No JS Mini Demos](http://www.scottohara.me/article/mini-demos.html)，这篇文章演示了不依靠javascript而完成的两个简单的表单的交互效果。

为了更能直接体现css制作交互效果的能力，下面制作了一个表单的交互效果。试着点击文本框，就会看到优美的效果。

<p data-height="376" data-theme-id="0" data-slug-hash="zGyLt" data-default-tab="result" data-user="SitePoint" class='codepen'>See the Pen <a href='http://codepen.io/SitePoint/pen/zGyLt/'>Display Form Label on Focus with CSS</a> by SitePoint (<a href='http://codepen.io/SitePoint'>@SitePoint</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

当点击文本框的时候，框中的提示文字会平滑的移动到文本框的上面。

那是怎么做到的呢？先来看看html结构：

	<div class="form-row">
  		<input type="email" class="input-text" id="name2" placeholder="Your Email" />
  		<label class="label-helper" for="name2">Your Email</label>
	</div>
	
注意到**label**这个标签么？这个文本的移动效果主要是靠这个标签来实现的。

下面是主要的css代码：

	.form-row {
  		position: relative;
	}
 
	.input-text {
  		background-color: #fff;
  		border: 1px solid #ccc;
  		margin-bottom: 8px;
  		padding: 8px 4px;
  		position: relative;
  		width: 100%;
  		z-index: 3;
	}
 
	.label-helper {
  		bottom: 0;
  		left: 0;
  		opacity: 0;
  		position: absolute;
  		transition: .2s bottom, .2s opacity;
  		z-index: 1;
	}
	
可以看到刚开始的时候，类名为**label－helper**的文字是隐藏的。

而当文本框获得焦点的时候，我们可以使用css来改变文本的位置：

	.input-text:focus + .label-helper,
	.input-text:invalid + .label-helper {
  		bottom: 95%; /* magic number, boo */
  		font-family: arial;
  		font-size: 14px;
  		line-height: 1;
  		opacity: 1;
  		padding: 4px;
	}
 
	.input-text:invalid {
  		border-left: 10px solid #f00;
	}
 
	.input-text:invalid + .label-helper:after {
  		color: #f00;
  		content: "X";
  		font-family: arial;
  		font-size: 14px;
  		line-height: 1;
  		padding-left: 12px;
	}
	
需要注意的是**:focus**这个属性以及相邻选择器**+**的使用。

当文本框获得焦点的时候，改变文本的位置和透明度。我们还稍微扩展了一下，当文本框中文字不符合指定的规则的时候，会显示一个**X**的符号来提醒用户。

### 单页面布局页面切换效果

在Codrops中看到来这个效果[FULLSCREEN LAYOUT WITH PAGE TRANSITIONS](http://tympanus.net/codrops/2013/04/23/fullscreen-layout-with-page-transitions/)，不过它是依靠javascript来实现的，下面我们就用css来实现这个效果。

<p data-height="492" data-theme-id="0" data-slug-hash="fuqLr" data-default-tab="result" data-user="SitePoint" class='codepen'>See the Pen <a href='http://codepen.io/SitePoint/pen/fuqLr/'>Page Layout transitions with Pure CSS</a> by SitePoint (<a href='http://codepen.io/SitePoint'>@SitePoint</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

下面是html结构：

	<input type="radio" class="radio" name="pages" id="exit" checked />
 
	<div class="page demo2">
 
  	<input type="radio" class="radio" name="pages" id="page_1" />
    <section class="section-container section-one">
    <label for="exit" class="check-label exit-label">
      X
    </label>
    <label for="page_1" class="page-label check-label">
      <span>Page X</span>
    </label>
    <header class="section-header">
      <div class="section-content">
        <h1>
          Title of Section X
        </h1>
      </div>
    </header>
    <div class="section-info">
      <div class="section-content">
        <p>
          ...
        </p>
      </div>
    </div>
   </section>
  
	</div>
	
注意到来**&lt;input type="radio"&gt;**这个元素么？我们将使用它们来触发页面的转换效果。

主要是使用了radio元素的**:checked**属性，配合**input**元素。当radio被选中的时候，使用相邻选择器来使用css操作对应的**section**元素的相关属性达到页面切换的效果。下面是radio元素的css代码：

	.radio,
	.exit-label {
  		display: none;
  		height: 0;
  		visibility: hidden;
	}
 
	.exit-label {
  		border: 1px solid #333;
  		color: #444;
  		display: inline-block;
  		padding: 4px 12px;
  		text-decoration: none;
	}
 
	.page-label {
  		height: 100%;
  		left: 0;
  		position: absolute;
  		text-align: center;
  		top: 0;
  		width: 100%;
	}
 
	.page-label span {
  		position: relative;
  		top: 50%;
	}
	
**.page-label**和**.page-label span**这两个元素都是用来触发页面切换效果用的。

	.section-container {
  		border: 0px solid;
  		height: 50%;
  		overflow: hidden;
  		position: fixed;
  		transform-origin: center center;
  		transition: 0.4s all;
  		width: 50%;
  		z-index: 1;
	}
 
	.section-content {
  		margin: auto;
  		max-width: 800px;
  		opacity: 0;
  		padding: 20px;
  		transition: 0.66s all;
  		visibility: hidden;
	}
 
	.section-one {
  		transform: translate(0%, 0%);
	}
 
	.section-two {
  		transform: translate(0%, 100%);
	}
 
	.section-three {
  		transform: translate(100%, 0%);
	}
 
	.section-four {
  		transform: translate(100%, 100%);
	}
	
这里定位我们是用来**transform:translate**来代替绝对定位的top,left,bottom,right来实现的。因为transform的性能要比使用定位好一些[Why Moving Elements With Translate() Is Better Than Pos:abs Top/left](http://www.paulirish.com/2012/why-moving-elements-with-translate-is-better-than-posabs-topleft/)。

	:checked + .section-container {
  		height: 100%;
  		overflow: auto;
  		transform: translate(0%, 0%);
  		width: 100%;
  		z-index: 5;
	}
 
	:checked + .section-container .exit-label {
  		display: inline-block;
  		visibility: visible;
	}
 
	:checked + .section-container .page-label {
  		display: none;
	}
 
	:checked + .section-container .section-content {
  		opacity: 1;
  		visibility: visible;
	}
 
	#exit:checked + .page .section-container {
  		border: none;
  		opacity: 1;
	}
 
	.page :not(:checked) + .section-container {
  		border: 40px solid;
	}
	
这部分代码就是我们实现页面切换效果的关键代码啦。

* **.section-container**是每一个页面外框，当触发按钮的时候，它会填满整个窗口。
* **exit**按钮是用来关闭页面的。

下面我们再来看看几个页面的切换效果。

<p data-height="517" data-theme-id="0" data-slug-hash="mFrAj" data-default-tab="result" data-user="SitePoint" class='codepen'>See the Pen <a href='http://codepen.io/SitePoint/pen/mFrAj/'>Alternative Layout Transitions with Pure CSS</a> by SitePoint (<a href='http://codepen.io/SitePoint'>@SitePoint</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

<p data-height="495" data-theme-id="0" data-slug-hash="wqnxg" data-default-tab="result" data-user="SitePoint" class='codepen'>See the Pen <a href='http://codepen.io/SitePoint/pen/wqnxg/'>Pure CSS Slide Nav</a> by SitePoint (<a href='http://codepen.io/SitePoint'>@SitePoint</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

上面这些例子虽然优美，但是问题还是有的。主要是可访问性上的问题。

当用户使用鼠标或者是触摸的方式来访问的时候，没什么问题。然而，当用户想用键盘或者是其它辅助性的工具来访问上面这些例子的时候，我们就不得不额外的添加一些标记以及javascript来实现效果让用户顺利访问。

下面就让我们来改造下第一个页面切换的例子，让它的可访问性更好一些。

### 使用:target来实现页面切换

我们使用**a**标签来代替上面的radio和label元素。使用**ID**和**:target**这两个属性来实现效果：

<p data-height="491" data-theme-id="0" data-slug-hash="oBzrp" data-default-tab="result" data-user="SitePoint" class='codepen'>See the Pen <a href='http://codepen.io/SitePoint/pen/oBzrp/'>Layout Changes with :target</a> by SitePoint (<a href='http://codepen.io/SitePoint'>@SitePoint</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

主要结构如下：

	<section class="section-container section-one" id="page_1">
  		<a href="#exit" class="btn btn-exit">
    		X
  		</a>
  		<a href="#page_1" class="btn btn-page">
    		<span>Page 1</span>
  		</a>
  		<header class="section-header">
    		<div class="section-content">
      			<h1>
        		Title of Section X
      			</h1>
    		</div>
  		</header>
  		<div class="section-info">
    		<div class="section-content">
      			<p>
        			...
      			</p>
    	</div>
  		</div>
		</section>
 
		<!-- this is again repeated for id="page_2", etc -->

使用a标签来代替上面的radio元素，并且把页面的ID分配给a标签的**href**属性。这样无论是语义话还是可访问性都比使用radio元素好了不少。

当然css也要相对应的调整：

	.btn-exit {
  		display: none;
  		visibility: hidden;
	}
 
	#page_1:target .btn-exit,
	#page_2:target .btn-exit,
	#page_3:target .btn-exit,
	#page_4:target .btn-exit {
  		border: 1px solid #333;
  		display: inline-block;
  		padding: 4px 12px;
  		text-decoration: none;
  		visibility: visible;
	}
 
	.btn-page {
  		height: 100%;
  		left: 0;
  		position: absolute;
  		text-align: center;
  		top: 0;
  		width: 100%;
	}
 
	.btn-page span {
  		position: relative;
  		top: 50%;
	}
 
	#page_1:target,
	#page_2:target,
	#page_3:target,
	#page_4:target {
  		transform: translate(0%, 0%);
  		width: 100%;
  		height: 100%;
  		z-index: 5;
  		overflow: auto;
	}
 
	#page_1:target .btn-exit,
	#page_2:target .btn-exit,
	#page_3:target .btn-exit,
	#page_4:target .btn-exit {
  		display: inline-block;
  		visibility: visible;
	}
 
	#page_1:target .btn-page,
	#page_2:target .btn-page,
	#page_3:target .btn-page,
	#page_4:target .btn-page {
  		display: none;
	}
 
	#page_1:target .section-content,
	#page_2:target .section-content,
	#page_3:target .section-content,
	#page_4:target .section-content {
  		border: 0;
  		opacity: 1;
  		visibility: visible;
	}
 
	#exit:target .page .section-container {
  		border: none;
  		opacity: 1;
	}
	
我们使用了**:target**属性来代替radio元素的**for**属性来实现页面的切换效果。

target的定义和用法：URL 带有后面跟有锚名称 #，指向文档内某个具体的元素。这个被链接的元素就是目标元素(target element)。
:target 选择器可用于选取当前活动的目标元素。

### 总结

自从css3推出以来，css成为了在web开发中一个强有力的工具。能实现以前只能靠javascript才能实现的效果。

当然上面的这些例子只是用来激发你的灵感的。在实际的开发中考虑到浏览器对标准的实现的情况各不相同，还是推荐使用javascript来实现这些效果。

源代码下载地址[source](http://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/08/1407313147no-js-source.zip)。


