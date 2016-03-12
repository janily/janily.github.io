---
layout: post
title: 浅析网页开发中的交互动画的开发方法
categories:
- Life
tags:
- javascript
- 翻译
- css
---

随着web技术的发展，特别是最近几年来，动画元素在web开发中运用的越来越多，现在如果网站中没用点交互动画，都不好意思拿出来啦！最近在项目中也用到了很多的交互动画，当然实现方法也是多种多样，无外乎javascript和css这两种基本的方法来实现，现在就来用一些实例来分析下网页开发中的交互动画的开发方法。原文在[这里](http://flippinawesome.org/2013/08/12/introduction-to-animating-in-html/?utm_source=buffer&utm_campaign=Buffer&utm_content=buffer2db08&utm_medium=facebook)。行文上可能不会按原文逐字逐句翻译哦！

###javascript方法

作者有很多年的动画开发经验，在以前的动画开发中你需要准备一些动画需要的图片，然后用程序把这些图片按照一定的顺序运动起来，这样就会得到我们想要的动画效果。同样的方法在网页中的动画开发中的也可以用到，比如在5秒中把一个DOM元素的宽度以每秒增加20px的频率来改变它的宽度。

要达到这样的效果，一种颇为流行的方法是用javascript。比如你可以创建这样一个函数，在一定的时间里，不断循环地改变DOM元素的属性。比如下面的代码就是用来不断循环来改变元素的宽度的。

<pre>
<code>
var width = 100;
var to = 200;
var loop = function() {
    if(++width < to) {
        element.style.width = width + "px";
        setTimeout(loop, 1);
    }
}
loop();
</code>
</pre>

效果如下

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/9AUzv/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

在jquery中，用animate这个方法同样也可以做到。

像上面这种通过javascript是一种在web动画开发中比较流行的做法。这样做的好处是，你能够控制整个动画的发生的过程，你也能够非常容易地在动画过程中加入自己想做的其它一些事情，比如在运动到20%的时候，让它变换下运动的速度等诸如此类的事情。

####运动形式

当然，如果动画只是固定的做些匀速运动是不够的。这就是我们为什么要用一些运动函数来管理动画，使动画的元素在运动的时候更加有趣的原因了。在Flash的世界中 Robert Penner大大创造了一些非常有名的运动方法。影响非常大，在javascript或者jquery中的控制动画的运动方法都来源于Robert Penner的理论。

那么我们如何在动画中来使用各种运动方法呢？我们首先通过一个例子来说明一下。比如我们想改变一个DIV的宽度，从0到10px依次累加10次。如果我们使用匀速的方法来做的话，是这样的。

<pre>
<code>
step 1 - 1px
step 2 - 2px
step 3 - 3px
step 4 - 4px
step 5 - 5px
step 6 - 6px
step 7 - 7px
step 8 - 8px
step 9 - 9px
step 10 - 10px
</code>
</pre>

看了之后，是不是觉的很简单？但是，如果我们变化的值不是整数呢，比如从232px到306px并且在14次循环了完成这个变化，那我们就要写一个方法来实现了。就像下面写的这样。

<pre>
<code>
var calcAnimation = function(from, to, steps) {
    var diff = Math.abs(to - from);
    var res = [];
    var valuePerStep = diff / steps;
    from += valuePerStep;
    for(var i=0; i&lt;steps; i++) {
        res.push(from + (i * valuePerStep));
    }
    return res;
}
</code>
</pre>

这个方法需要三个参数，分别是初始值，以及目标值和循环的次数。会返回一个包含了计算得出的每一次循环所得出的运动数值的数组。比如

<pre>
<code>
calcAnimation(0, 10, 10) // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
calcAnimation(0, 20, 5) // [4, 8, 12, 16, 20]
calcAnimation(-10, 10, 10) // [-8, -6, -4, -2, 0, 2, 4, 6, 8, 10]
</code>
</pre>

上面的这个方法可以用下面的改进的一个方法来实现

<pre>
<code>
var animation = {from: 100, to: 200, steps: 50};
var values = calcAnimation(animation.from, animation.to, animation.steps);
var currentIndex = 0;
var loop = function() {
    if(currentIndex < values.length) {
        element.style.width = values[currentIndex] + "px";
        currentIndex += 1;
        setTimeout(loop, 1);
    }
}
loop();
</code>
</pre>

上面的这个calcAnimation方法是用来做匀速运动的。但是当我们把初始值和目标值之间的运动值在每一次循环中随机变化下，变速运动就是这样发生了。我们通过下面一个方法来实现，下面方法中的变速公式就是来源于上面提到的Flash开发界中的大神Robert Penner。

<pre>
<code>
var calcAnimationOutElastic = function(from, to, steps) {
    var InOutElastic = function(t, b, c, d, a, p) {
        if (t==0) return b;  if ((t/=d)==1) return b+c;  if (!p) p=d*.3;
        if (!a || a < Math.abs(c)) { a=c; var s = p/4; }
        else s = p/(2*Math.PI) * Math.asin (c/a);
        return (a*Math.pow(2,-10*t) * Math.sin( (t*d-s)*(2*Math.PI)/p ) + c + b);
    }
    var res = [];
    for(var i=0; i&lt;steps; i++) {
        res.push(InOutElastic(i, from, to, steps, 0, 0));
    }
    return res;
}
</code>
</pre>

我们来看下运用这个变速运动后，会产生什么神奇的效果呢？

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/mq5KP/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

也许看了上面的calcAnimationOutElastic公式后，你一头雾水，这啥东西，像看天书一样。当然你如果想成为像Robert Penner一样的大神，那你得好好修炼，知其然，知其所以然。如果只是想看到钉子的时候，能拿出一把锤子，不理解也没关系，你只需要知道这玩意返回的值让你能创建变速动画就行了。

###CSS

说完javascript的方法，接着来说说在最新的css3中是怎么样来做动画的。

其实CSS3动画和javascript运动的原理差不多，在同等的基准条件下，CSS3动画还要比javascript做的动画的性能要快点。CSS3创建动画可以通过两种方法：transitions和animations。

CSS transition 需要设置两个属性来达到动画效果transition-property 和 transition-duration表示要运动的属性和运动的时间。

<pre>
<code>
transition-property: background;
transition-duration: 500ms;
</code>
</pre>

由于现在W3C还没形成标准规范，所以在各个浏览器下要加上前缀才能使CSS3动画生效，这里为了简便就没加上各浏览器前缀了。

下面来一个CSS3动画的简单的例子，就是当你鼠标移入按钮的时候平滑地改变其背景颜色。

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/6Tjmx/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

如果你有很多的属性需要动画效果，你可以用逗号把需要运动的属性隔开。

当然CSS3动画不仅可以制作动画还可以制作想FLASH一样的逐帧动画，而不仅仅是控制开始和结尾而已的动画效果。下面就来看一个简单的例子，在不同的时段改变背景的颜色。CSS3中通过keyframes来制作帧动画的效果，具体可以去看相关的资料，这里就不详细说了，如下

<pre>
<code>
@keyframes 'bg-animation' {
    0% { background: #C9C9C9; }
    50% { background: #61BE50; }
    100% { background: #C9C9C9; }
}
.star:hover {
    animation-name: 'bg-animation';
    animation-duration: 2s;
    animation-iteration-count: infinite;
}
</code>
</pre>

完整的效果看demo一目了然啦

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/qCm8f/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

当然，当你选择用CSS3的动画来代替javascript制作动画效果的时候，CSS3也有一些无法回避劣势。

* 你不能够控制动画开始与结束的事件
* 语法也不是很友好（这好像不是劣势吧）
* 同步运行多个动画效果有那么点点难度

###锤子和钉子

不同情况，我们就要用不同的方法来制作动画效果，不能给你把大锤，看见钉子就锤吧，有些小小的钉子，本来一个小石头就可以搞定，你硬是要用把大锤来锤，当然也可以，可是也未免大材小用，杀鸡用牛刀了。

在原文作者的观点来看，一些简单动画效果，比如鼠标滑入滑出或者是一些淡入淡出的稍微复杂点的动画效果可以用CSS3来搞定。从性能上来考虑也是如此，如果我们仅仅是通过javascript来制作动画效果，但是可能性能上可能稍微差那么点点。尤其是在移动设备上更是如此。

####怎么配合javascript去制造一个动画

一般情况下，你可以通过创建一个类来制造你想要的动画效果。只要把这个类加到你想要创建动画效果的元素上，或者是你在想结束动画效果的时候移除这个类，这样你就可以轻松的控制动画效果了。通过以下代码你就可以控制整个动画过程了。

<pre>
<code>
var element = document.querySelector(".star");
element.addEventListener("mouseover", function() {
    if(element.className.indexOf('star-hover') === -1) {
        element.className += ' star-hover';
    }
});
element.addEventListener("mouseout", function() {
    element.className = element.className.replace(/ star-hover/g, '');
});
</code>
</pre>

下面这个例子就是用上面的代码制作的

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/B57SD/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

####见缝插针（捕获动画，创建动画序列）

下面我们来创建一个稍微复杂点的动画，连续执行两个不同的动画，跟回调函数效果差不多，简而言之，就是我们创建两个或两个以上的动画，在不同时间执行不同的动画。这里会用到transitionend这个时间，transitionend事件发生于过渡完成时。如果过渡在完成之前被取消，则将不会触发此事件。当然为了兼容各个浏览器的transitionend事件，我们这里用javascr来处理各个浏览器的transitionend事件，Modernizr也用了类似的方法来处理。

<pre>
<code>
function whichTransitionEvent(el){
    var t;
    var transitions = {
      'transition':'transitionend',
      'OTransition':'oTransitionEnd',
      'MozTransition':'transitionend',
      'WebkitTransition':'webkitTransitionEnd'
    }

    for(t in transitions){
        if( el.style[t] !== undefined ){
            return transitions[t];
        }
    }
}
var transitionEnd = whichTransitionEvent(element);
element.addEventListener(transitionEnd, function(event) {
    if(element.className.indexOf('star-expand') === -1) {
        element.className += ' star-expand';
    }
});
</code>
</pre>

用上面的代码，我们做下面那个简单的动画，当你点击按钮的时候，你会发现按钮的颜色会改变接着是按钮的宽度也发生了变化。这是因为我们创建了一个动画序列，两个动画类交替添加在元素上，从而使元素有了两个动画效果。

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/S9R9L/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

需要记住的是，这里的事件处理函数只接受一个包含动画信息的对象，比如一些属性的改变等等。

####诗情画意的动画类库

这里强烈推荐用Animate.css这个动画方法库来制作动画效果，当然是基于CSS3的。它包含了一些非常棒的动画效果，你再也不用担心因为去制作一个动画效果还要去学习相关的数学知识。

我们来看一个简单的例子

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/p7jfe/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

在上面的例子中我们用到了Animate.css中rotateInDownLeft和bounceOutRight这两个类来创建不同的动画效果，由于在Animate.css中的动画效果都是通过animations来创建的，相应的我们也要修改下js中控制动画方法的函数

<pre>
<code>
function whichAnimationEvent(el){
    var a;
    var animations = {
      'animation': 'animationend',
      'OAnimation': 'oAnimationEnd',
      'MozAnimation': 'animationend',
      'WebkitAnimation': 'webkitAnimationEnd'
    }
    for(a in animations){
        if( el.style[a] !== undefined ){
            return animations[a];
        }
    }
}
</code>
</pre>

在Animate.css有很多非常棒的动画效果，你可以去它的官网看看，地址在[这里](http://daneden.me/animate/)。

jquery中，你也可以这样来用animate.css 

<pre>
<code>
$('#yourElement').addClass('animated bounceOutLeft');
</code>
</pre>

####Animate.js

这段时间在开发中，我尝试着精良避开在不必要的时候用像Jquery这样的类库，比如我只是要选择一个元素，改变一下它的类等诸如此类的工作。除了Jquery以外，也有很多其它的javascript解决方案。

最近，我在两个项目中用到了animate.css这个库来制作动画效果。由于要用到javascript来控制动画，所以我就写了一个简单javascript方法库以便更好的控制animate.css来制作我想要的动画效果。地址在[这里](https://github.com/krasimir/animate.css/tree/master/js)。

#####初始化Animate.js

首先得在html文件中引入animate.js，接着做下初始化的工作

<pre>
<code>
var el = document.querySelector(".your-element-class");
var controller = Animate(el);
</code>
</pre>

接着我们就可以用它来控制animate.css来制作动画效果啦，例如

<pre>
<code>
controller
.add('animated')
.add('bounceOutLeft')
</code>
</pre>

add方法使用来给元素添加类的，这个方法也接受一个回调函数，当动画完成的时候还可以干一些其它的事情，比如

<pre>
<code>
controller.add("flipInY", function() {
    alert("flipInY finishes");
});
</code>
</pre>

既然有添加，那也得有删除是么，删除方法是remove（用过jquery的很熟悉这种方式吧）

<pre>
<code>
controller.remove('bounceOutLeft');
</code>
</pre>

当你用out这个方法的时候，元素将会在动画结束的时候隐藏起来。如果你想动画结束的时候，元素回到最初始的状态，那用remove方法就可以了。

所有的方法都会把自己作为一个对象返回，这就意味着可以像jquery一样来链式调用了，比如

<pre>
<code>
controller
.add("rotateOutUpRight")
.end("rotateOutUpRight", function() {
    alert("rotateOutUpRight");
});
</code>
</pre>

你可能会像Jquery中那样直接嵌套来使用链式方法编写代码，但是为了代码的可读性，最好是配合end方法来使用。它接收上一个动画的类名以便知道这个动画结束了。

在开发中，我经常需要在一个动画结束的时候，开始一个新的动画。这个时候我们就可以用end方法了。我们可以把我们要制作新的动画的名称作为第二个参数传给end方法。像下面这样

<pre>
<code>
controller
.add("flipInY")
.end("flipInY", 'rotateInDownLeft')
.end("rotateInDownLeft", 'bounceOutDown');
</code>
</pre>

#####删除所有的动画

有时候，在一个新的动画运动前我们可能需要删除所有的动画效果。就需要用到removeAll()这个方法了。

<pre>
<code>
controller.removeAll();
</code>
</pre>

#####运行一个动画列队

<pre>
<code>
controller.sequence('flip', 'flipInX', 'flipOutX', 'flipOutY', 'fadeIn', 'fadeInUp');
</code>
</pre>

一目了然，就不解释啦。只需要记住新的动画开始运行前，旧的动画的类就会被删除。

这里需要注意的是，每一个方法都是在javascript中的上下文环境中执行的。所以this关键字也是指向你当前环境中的对象，比如

<pre>
<code>
Animate(el).add("flipInY", function() {
    this.removeAll().add("fadeInUp");
});
</code>
</pre>

下面来个简单的例子，来演示下怎么用animate.js和animate.css来创建动画效果。

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/R2tzB/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

###总结一下

这篇文章主要是提供了一些关于html、css、javascript这三者之间实现一些动画的思路。如果你想学习跟多关于animate.css，你可以去animate.css学习下。

文章的原地址是http://krasimirtsonev.com/blog/article/Introduction-to-animations-in-HTML-css3-transitions-keyframes。

翻译文章并没有逐字逐句，可以去原文详细的看看。






