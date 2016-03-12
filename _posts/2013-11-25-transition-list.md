---
layout: post
title: CSS3配合javascript来制作动画交互效果
categories:
- Life
tags:
- css
- 翻译
---

> 这篇文章来自[css-tricks](http://css-tricks.com/transitional-interfaces-coded/)的[Transitional Interfaces, Coded](http://css-tricks.com/transitional-interfaces-coded/)，主要是讲了用CSS3和javascript来制作一些列表的交互效果。具体内容有删减。

Pasquale D’Silva在他的博客里写了一篇关于交互动画方面的文章[Transitional Interfaces](https://medium.com/design-ux/926eb80d64e3)。这篇文章非常不错，我们可以运用CSS3中的Transition和animations这两个属性制作出非常流畅的交互动画。使用它们可以给你的创意插上翅膀，制作出令人印象深刻的交互动画。

比如，在一个列表中插入一个新的列，你是想让它很生硬的插入到列表中去呢，还是添加一些动画效果来实现这个插入效果好呢？我相信你看了下面这些示例的交互动画后，你会选择正确的方法。

<p data-height="268" data-theme-id="0" data-slug-hash="JdALD" data-user="janily" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/JdALD'>Transitional Interfaces</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a></p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

其实这些交互效果并没有用到啥高深的技术，仅仅是用到了CSS3的一些属性再配合javascript来实现的。

下面就简要的分析下这里的效果里面的添加一个列表的一个实现原理，首先在设置新添加的列的高度为0，**max-height:0;**，然后再配和[@keyframes](http://css-tricks.com/snippets/css/keyframe-animation-syntax/)使用**scale**制作一个动画效果，把高度设置为50px，当然这个高度是根据你自己列表的高度来定的。

具体@keyframes的用法可以去W3C官方的手册来看，代码如下：

    .new-item {
	  max-height: 0;
	  opacity: 0;
	  transform: scale(0);
	  animation: growHeight 0.2s ease forwards;
	}
	@keyframes growHeight {
	  to { 
	    max-height: 50px;
	    opacity: 1;
	    transform: scale(1);
	  }
	}

我们还可以组合动画做出更多的交互效果，如下代码所示：

    .new-item {
	  max-height: 0;
	  opacity: 0;
	  transform: translateX(-300px);
	  animation: 
	    openSpace 0.2s ease forwards,
	    moveIn 0.3s 0.2s ease forwards;
	}
	@keyframes openSpace {
	  to { 
	    max-height: 50px;
	  }
	}
	@keyframes moveIn {
	  to { 
	    opacity: 1;
	    transform: translateX(0);
	  }
	}

动画属性可以用两个** keyframe**组合在一起，当然这两个动画是按顺序执行的。先执行完第一个，再执行第二个。



