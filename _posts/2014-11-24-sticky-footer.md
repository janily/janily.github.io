---
layout: post
title: 使用css制作固定底部的效果
categories:
- Life
tags:
- css

---

有时候，在web开发中，内容不够的时候，底部版权等内容需要固定在视窗的底部。要达到这种效果手段也五花八门，今天就来看看其中一种方法，也是我经常用的，今天在做一个需求的时候，就碰到了这样的场景，纪录在这里。

### html

	<div class="page-wrap">
  
  		Content!
      
	</div>

	<footer class="site-footer">
  		I'm the Sticky Footer.
	</footer>
	
### css

	* {
  		margin: 0;
	}
	html, body {
  		height: 100%;
	}
	.page-wrap {
  		min-height: 100%;
  		/* 等于底部的高度 */
  		margin-bottom: -142px; 
	}
	.page-wrap:after {
  		content: "";
  		display: block;
	}
	.site-footer, .page-wrap:after {
  		height: 142px; 
	}
	.site-footer {
  		background: orange;
	}
	
### 效果

<p data-height="268" data-theme-id="0" data-slug-hash="qEdbEm" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/qEdbEm/'>Sticky Footer</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>