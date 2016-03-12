---
layout: post
title: 高性能web动画性能编写－FLIP原则
categories:
- Life
tags:
- javascript
- css3
- 性能

---

> 一直以来对于提高css3动画性能，特别是在移动端动画性能，一直是前端工程师们关心的话题。今天刚好看到一篇是Google工程师的文章和演讲视频，主要内容也是关于高性能动画编写的原则(FLIP)，就根据他的文章和视频整理了一下，就有了这一篇博文。具体地址和视频可以点击这个[链接](http://aerotwist.com/blog/flip-your-animations/)去看看。

关于web动画，最好帧数在60fps，这样动画才会比较顺畅自然。这个广大的工程师们应该都很清楚。但是要达到这个目标也不是那么容易，因为它跟很多因素有关，要做一个流畅的动画要花点的功夫多。这里推荐使用一个FLIP原则来编写你的动画效果。

什么是FLIP原则呢？

FLIP是这四个单词的缩写即，First,Last,Invert,Play。

下面来具体解释下这四个单词的意思：

* First：元素的初始状态。
* Last：元素经过运动之后的状态。
* Invert：就是指元素在First和Last状态之间的一个过程，比如宽度、高度或者是透明度的变化。再比如，一个元素在从First到Last状态之间要向下移动90px，你可能会使用transformY(-90px)来达到目的。
* Play：指元素最终到达last状态时，需要把First和Last状态之间的Invert状态的一些变化属性删除。

大概FLIP就介绍道这里啦，下面先来些代码：

	// 获取元素初始状态的一些信息如位置、宽高
	var first = el.getBoundingClientRect();

	// 设置最终状态的控制的类
	el.classList.add('totes-at-the-end');

	// 再来获取在最终状态元素的一些信息如位置、宽高
	var last = el.getBoundingClientRect();

	// 这里可以来计算在元素first和last状态之间的一些信息
	var invert = first.top - last.top;

	// 使用transform来把invert状态的值表现出来
	el.style.transform = 'translateY(' + invert + 'px)';

	// 使用requestAnimationFrame来控制我们的动画效果
	requestAnimationFrame(function() {

  	    el.classList.add('animate-on-transforms');

  	    el.style.transform = '';
	});

	// Capture the end with transitionend
	el.addEventListener('transitionend', tidyUpAnimations);
	
当然，如果你对最新的[Web Animations API](http://w3c.github.io/web-animations/)比较熟悉多话，我们可以改进下上面的代码：

	// 获取元素初始状态的一些信息如位置、宽高
	var first = el.getBoundingClientRect();

	// 设置最终状态的控制的类
	el.classList.add('totes-at-the-end');

	// 再来获取在最终状态元素的一些信息如位置、宽高
	var last = el.getBoundingClientRect();

	// 使用transform来把invert状态的值表现出来
	var invert = first.top - last.top;
	
	var player = el.animate([
  		{ transform: 'translateY(' + invert + 'px)' },
  		{ transform: 'translateY(0)' }
	], {
  		duration: 300,
  		easing: 'cubic-bezier(0,0,0.32,1)',
	});

	// 
	player.addEventListener('finish', tidyUpAnimations);

当然，上面的代码要配合着[Web Animations API polyfill](https://github.com/web-animations/web-animations-js)来使用，地址里面有详细的介绍。

如果你想对**FLIP**有更加深刻的认识，可以去看看[这里的代码](https://github.com/GoogleChrome/devsummit/blob/master/src/static/scripts/components/card.js#L263-296)。

### 能带来什么？

说了这么多，那FLIP原则到底有什么优势呢？

用一些动画效果给用户的输入以及时对反馈确实能大大提高用户到体验。所以在Chrome Dev Summit的网站上，当用户点击卡片的时候，添加来一个卡片放大的动画效果。而且，因为网站是响应式的，要适应不同的设备。所以我们不知道元素开始和结束的确切位置。

当然，我们这么做多原因是，当用户触发你的网站交互动画的时候，其实是有一个100ms的延迟的，不过用户是感觉不到的。我们要做的就是，把动画效果做到60fps，这样就能使用户感觉动画的流畅，而不是延迟卡顿。

![image](http://aerotwist.com/static/blog/flip-your-animations/window.jpg)

在这里要提到的是**getBoundingClientRect**，这样能确保动画运行的更快更好。至于原因可以去看看[这篇文章](http://aerotwist.com/blog/pixels-are-expensive/)。

### 一些需要注意的

如果你要使用FLIP的原理来做动画效果，那么下面这些要注意一下：

* **不要超过100ms的窗口期**。千万要切记这个时间点。如果你超过啦这个时间点，那么你的动画将不会有任何响应，具体可以使用chrome的开发者工具来查看响应时间。
* **精心来规划你的动画效果**。想象一下，如果你本来有一个透明度和transform的动画效果，然后记决定要加入另外一个动画效果，那么它就需要额外的计算资源来承载。这样就会阻断其它动画效果的运行。这里就需要确保在空闲的时候来进行计算资源来计算或者是在上面说的窗口期的时间内来完成计算。总之是不能影响到其它动画效果的执行。
* **内容失真变形**。 当你使用放大缩小来操作一些元素的时候，可能会引起该元素的失真变形。

### 总结

使用FLIP原则来编写动画效果还是不错的，它能很好的利用javascript喝css之间的配合来制作高性能的动画效果。可以轻松的使用 Web Animations API 或者是 JavaScript 来控制动画效果。最主要的目的还是尽可能减少计算资源的开销（一般是指opacity和transform带来资源消耗），给用户最好的体验。




