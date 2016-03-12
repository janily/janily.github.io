---
layout: post
title: 你现在就可以开始使用的CSS3中12个令人尖叫的特性
categories:
- Life
tags:
- css
---


> 这篇文章主要是来自[tutorialzinel](http://tutorialzine.com/)网站的[12 Awesome CSS3 Features That You Can Finally Start Using](http://tutorialzine.com/2013/10/12-awesome-css3-features-you-can-finally-use/)。具体内容有删减，添加了一些自己的理解。


如果你像我一样，当看到令人印象深刻的用CSS3做精彩的实例时，你可能也会迫不及待把它运用到你的网站中。当然，可能只有少部分的浏览器支持（但是这里一般不会出现IE浏览器），所以最终，你可能会等到所有浏览器支持的时候，才使用它。不过，幸运的是，现在有一些非常棒的CSS3特性在浏览器上已经支持的很好了，现在就可以在网站开始使用这些特性了。

不过需要注意的是，这些特性在IE9以下的浏览器都是不支持的。如果你的网站用户中用这些浏览器的用户占了大多数，那就要考虑下平稳降级了。但是对于其它使用现代浏览器的用户来说，就可以放心使用了。

### 1、CSS Animations and Transitions ###

CSS动画，现在大部分浏览器已经支持了，包括IE10以后的版本都支持。有两种方法来创建CSS动画。第一种方法非常容易，用*transition*属性来改变元素的属性。用transition属性，你可以创建一些鼠标滑过或者是点击的动画效果，或者用javascript来模拟CSS动画，下面有一个实例，用鼠标滑过地球上的时候，这个火箭就会像地球靠拢。

第二种方法是，直接定义CSS动画方法，这就可能有点复杂了，这里要用到动画属性里面提供的关键帧这个东东，*@keyframe*。它允许你重复一个动作而不依靠javascript。下面是个实例，具体你可以看看源代码：

<a class="jsbin-embed" href="http://jsbin.com/EHUSAto/1/embed?output">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

这里就需要去学习CSS动画方面的知识了，我推荐些有用的资料可以去学习下[ this Mozilla Developer Network (MDN) article](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Using_CSS_animations)。可以去这个网站查询浏览器的兼容性，[this compatibility table](http://caniuse.com/#feat=css-animation)。

### 2. Calculating Values With calc() ###

另外一个很棒的CSS的新特性是*calc()*这个方法。这个方法可以使CSS进行一些简单的的计算。比如一些需要计算元素的尺寸的场景中，这个方法就非常方便。更酷的是，你可以计算不同单位的组合，比如百分比和像素。这使我们过去使用的一些诡异的布局技巧，相形见绌，显得非常过时。而且在IE9以及IE9以上的浏览器也能工作的很好，不过需要加上特定的前缀。

<a class="jsbin-embed" href="http://jsbin.com/AhIMoLi/1/embed?output">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

更多关于*calc()*方法的使用可以去[这里](https://developer.mozilla.org/en-US/docs/Web/CSS/calc)看看。[这里](http://caniuse.com/#feat=calc)查询各个浏览器的兼容性。

### 3. Advanced Selectors ###

现在，你如果还大量用ID来定义元素的样式，你肯能就要改进下了。在CSS2和CSS3规范中引进了很多非常强大的选择器，使你更加方便的定义样式，而不需要定义很多ID之类的。

这些选择大部分浏览器都支持，有一些得IE9以上的浏览器才能支持。下面这个例子就用了很多的选择器，你可以点击查看源代码。

<a class="jsbin-embed" href="http://jsbin.com/AsaWaze/1/embed?output">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

#### 4. Generated Content and Counters ####

生成内容对于开发者来说是一个强大的工具，在CSS中，我们可以使用伪元素*::before*和*::after*来生成内容。这样可以使用很少的HTML就能生成以前需要很多的HTML标签才能实现的布局效果。这对于需要制作阴影和一些其它的视觉效果的场景，非常有用。当然，最后你还能避免一些无语义的标签，何乐而不为呢。

CSS3还提供了一些提供计算功能的伪类选择器，增强了CSS的计算功能。它们还可以读取特定元素中的内容，可下面的这个例子，就清楚了。

<a class="jsbin-embed" href="http://jsbin.com/uYaMEpO/1/embed?output">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

可以去[这里](http://caniuse.com/#feat=css-gencontent)查看下生成内容的支持程度。

### 5. Gradients ###

渐变这个属性是一个强大的工具，可以使我们不需要图片就可以创造非常好的颜色渐变的效果。而且在视网膜屏幕上也能运行的很好。渐变支持线性渐变和径向渐变，也支持平铺。渐变在语法上也在持续发生了一些改变，现在可以不使用前缀，就能在大部分浏览器上使用。

<a class="jsbin-embed" href="http://jsbin.com/uyUVeQe/1/embed?output">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

这里有渐变的详细教程[地址](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Using_CSS_gradients)，可以去[这里](http://caniuse.com/#feat=css-gradients)查看兼容性。

### 6. Webfonts ###

你能想象在以前的web中，我们只能使用很少的字体的情形么？难以置信，今时不同往日，现在有诸如[ Google Fonts](http://www.google.com/fonts/)和[ typekit](https://typekit.com/)这些提供字体的线上服务，直接在我们的页面中就可以调用它们提供的字体。也有想[ fontawesome](http://fortawesome.github.io/Font-Awesome/)提供字体图标的服务的。提供了很多的矢量图标。要用到* @font-face*这个属性。然后你就可以用*font/font-family *来使用这些图标了。

<a class="jsbin-embed" href="http://jsbin.com/EQubedOr/1/embed?output">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

由于浏览器安全方面的限制，要在服务器的环境中才能使用webfonts。可以去这个[地址](http://demo.tutorialzine.com/2013/10/css3-features-you-can-finally-use/06-webfonts.html)看看。

而且在IE6这样古老的浏览器中，webfonts也能很好的工作。

### 7. Box Sizing ###

对于刚接触CSS人来说，盒子模型是一个令人头疼的东西。盒模型的宽高受元素的边框，边距，内容宽度等因素的影响。如果没注意这些的因素的影响，那么很容易就会破坏布局，现在使用*box-sizing*这个规则就可以很容易解决这个问题，你只要把它的值设置为*border-box*，看下面这个例子就很容易知道了：

<a class="jsbin-embed" href="http://jsbin.com/OBEvAqa/1/embed?output">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

[这里](https://developer.mozilla.org/en-US/docs/Web/CSS/box-sizing)可以学到跟多的box-sizing知识，或者可以去[这里](http://caniuse.com/#feat=css3-boxsizing)看看兼容性。

### 8. Border Images ###

*Border Images*属性可以使用图片来定义边框。这里可以使用一个雪碧图来定义边框，对应四条边的不同部分。可以去[这里](http://demo.tutorialzine.com/2013/10/css3-features-you-can-finally-use/assets/img/border.png)看图片的具体样子。

<a class="jsbin-embed" href="http://jsbin.com/acujIrU/1/embed?output">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

可以去这些地址深入了解下边框图片这个属性,[ MDN page ](https://developer.mozilla.org/en-US/docs/Web/CSS/border-image)和[ this article](http://css-tricks.com/understanding-border-image/)，Border Images支持[大部分的浏览器和IE11](http://caniuse.com/#feat=border-image)。

### 9. Media Queries ###

如果你认真严肃的对待网页设计的话，那媒体查询是绝对不能避开的。媒体查询已经存在很长的时间了，最近因为移动互联网发展，才被人们重新重视起来。这之前，我们会为不同的设备单独设计不同的网站，不过配合媒体查询，我们可以只建立一个网站适应不容的设备。

媒体查询非常容易使用，你只需要在你的样式表中使用*@media *这个规则就可以了，比如，我们缩放[这个页面](http://demo.tutorialzine.com/2013/10/css3-features-you-can-finally-use/09-media-queries.html)。我已经把它写在下面的例子中，你可以试着删除*@media*看看会发生什么。

<a class="jsbin-embed" href="http://jsbin.com/uWeMayI/1/embed?output">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

### 10. Multiple Backgrounds ###

用多重背景属性，可以创建很多的有趣效果。可以定义不同的背景在一个元素航。每一张背景图片可以独立的移动和运动。就像下面这个例子（你可以把鼠标移到图片上去看看），所有的背景可以在CSS规则中用逗号分隔开。

<a class="jsbin-embed" href="http://jsbin.com/eWUBOsI/1/embed?output">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

去这里，可以了解跟多关于多重背景的知识，[这里](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Using_multiple_backgrounds)。浏览器的支持度非常不错（[看这里](http://caniuse.com/#feat=multibackgrounds)）。

### 11. CSS Columns ###

基于列的布局在CSS中难以实现是众所周知的。这里涉及到在服务端要处理为不同的元素，分割文字。这是不必要的，而且把问题复杂话了。不过，在CSS3中，这一切再不是难题：

<a class="jsbin-embed" href="http://jsbin.com/idOrAWa/1/embed?output">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

同样可以去这些地址了解跟多的知识。[支持度](http://caniuse.com/#feat=multicolumn)。当然不同的浏览器对于这个属性还是有些地方有兼容性问题，可以去[这里](http://zomigi.com/blog/deal-breaker-problems-with-css3-multi-columns/)看看不同浏览器的表现形式。

### 12. CSS 3D Transforms ###

在CSS3中，再没有比3D变换给人留下深刻的印象啦！尽管现在，3D变换在浏览器中不能很好得到支持，但是3D变换提供的一些强大的功能还是能够给开发者和设计者设计带给用户非常棒的用户体验。

下面就看下实例：

<a class="jsbin-embed" href="http://jsbin.com/iCaNekU/1/embed?output">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

这里的代码是模仿[这个教程](http://tutorialzine.com/2012/02/apple-like-login-form/)里的代码，我也推荐你读一下这篇文章，如果想学习关于CSS 3D变换这方面的知识，可以去看下[这篇详细的教程](http://desandro.github.io/3dtransforms/)，如果你不打算支持IE，那就不需要考虑[兼容性问题了](http://caniuse.com/#feat=transforms3d)。

### 其它一些特性 ###

CSS3还有一些其它非常棒的特性或者是新的改进。比如现在你可以不要再加在一些属性前添加前缀来兼容了，比如*border-radius* 和 *box-shadow*。你现在可以使用*data-uri*属性来定义背景图片。*opacity*也非常好用，*background-size*也很有用。

还有一些令人期待的属性*flexbox*、*@supports*、[filters](http://tutorialzine.com/2012/06/quick-tip-css-filters/)和*CSS masks*，虽然现在还在制定中，但是是值得期待的。