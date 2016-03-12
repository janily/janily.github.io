---
layout: post
title: 使用css3动画属性中的steps()来制作动画效果
categories:
- Life
tags:
- css3

---

在css3 animation中有一个steps()的属性，它可以实现分步过渡来执行动画效果。它可以传入两个参数，第一个是一个大于0的整数，他是将间隔动画等分成指定数目的小间隔动画，然后根据第二个参数来决定显示效果。第二个参数设置后其实和step-start，step-end同义，在分成的小间隔动画中判断显示效果。可以看出：steps(1, start) 等于step-start，steps(1,end)等于step-end。

那它在制作动画中有什么具体的使用场景呢？

下面我们分别来看下它能给我们带来哪些神奇的效果。

### 打字效果(type effect)

<p data-height="268" data-theme-id="0" data-slug-hash="fglsI" data-default-tab="result" data-user="janily" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/fglsI/'>fglsI</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

从上面的代码可以看到这个效果主要是通过改变边线的宽度来实现的。上面有30个字符，为来让字符逐个现实我们指定分30步来执行动画效果即**steps(30)**。并且设置字体为等宽字体即**monospace**，这样确保每个字符的宽度是一样的。

### 雪碧图动画效果(Sprite sheet animation) 

<p data-height="261" data-theme-id="0" data-slug-hash="tukwj" data-default-tab="result" data-user="simurai" class='codepen'>See the Pen <a href='http://codepen.io/simurai/pen/tukwj/'>Steps Animation</a> by simurai (<a href='http://codepen.io/simurai'>@simurai</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

原理也是利用steps()这个属性来做的。

可以看到雪碧图共有10张图片，所以我们分成10步来执行动画效果。这样就制作出来以前只能靠javascript才能制作的雪碧图动画效果。

充分发挥你的想像力，我们还可以制作出更加复杂的雪碧图动画效果：

<p data-height="353" data-theme-id="0" data-slug-hash="rCost" data-default-tab="result" data-user="rachelnabors" class='codepen'>See the Pen <a href='http://codepen.io/rachelnabors/pen/rCost/'>Complete CSS3 + HTML5 music video</a> by Rachel Nabors (<a href='http://codepen.io/rachelnabors'>@rachelnabors</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>


参考文章：

[Pure CSS3 typing animation with steps()](http://lea.verou.me/2011/09/pure-css3-typing-animation-with-steps/)

[Sprite sheet animation with steps()](http://simurai.com/blog/2012/12/03/step-animation/)

[Flashless Animation](http://24ways.org/2012/flashless-animation/)
