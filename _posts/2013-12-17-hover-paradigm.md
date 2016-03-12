---
layout: post
title: 鼠标响应模式探索
categories:
- Life
tags:
- css
- 翻译
---

> 虽然现在各种人机交互技术层出不穷，但是鼠标作为人机交互的一个必不可少的工具今时今日还是有不可替代的作用。同样在PC互联网的世界里，也要依靠鼠标来完成各种各样的交互行为，特别是在网页交互中，依靠鼠标，我们可以放飞我们的想象力，用代码打造各种优雅的交互行为。尤其是随着CCS3的出现，更是为交互行为锦上添花。本文就鼠标响应范式进行了探索，遂翻译之。本文翻译自[24ways](http://24ways.org/)网站的[The Responsive Hover Paradigm](http://24ways.org/2013/the-responsive-hover-paradigm/),具体内容有删减。

CSS新增的transition和animations属性给网页开发设计师提供了有力的设计工具。基于新的CSS技术我们完全可以打造以前只有依靠FLASH才能制作的各种酷炫的交互动画。

结合**:hover**这个伪类(为了行文方便，后面有用**鼠标滑过**来表示hover,敬请留意。)，我们可以利用鼠标hover的这个行为在网站上添加很多有趣的交互动画。那利用这个技术，我们能做些什么呢？

### 动还是不动，交互还是在那里 ###

在web开发社区，我们经常听到一些争论。有些认为内容才是第一位的；有些认为在现在移动互联网时代，开发应该采取的是移动优先的策略；也有一些认为正是由于大量装饰性的元素和图片的滥用，导致现在网站的加载速度并没有因为宽带的提高变得快，反而是变的越来越慢。对于上面的这些争论我都100%赞同。但是，我相信在内容为王的时代，我们还是可以做一些事情，可以为web这个复杂的系统，做一些更加有趣的东西。让web世界变得更加美丽以及提供优雅的交互行为。

当然，在移动手机上如果仅仅是单纯加载一个没有样式的HTML文件，那加载速度当然是刚刚的，这就像一个人没有穿衣服，你就随便在大街上溜达么。在以前的原始社会没有啥关系，可是在21世纪的今天，这还是有点不妥吧。再比如，如果你去一个图书馆看书，如果每一本书的封面都是一模一样的，这找起来还是有一点难度的吧？

对于用户来说，看到一个设计漂亮的网站肯定要比一个设计很粗糙的网站来的舒服吧。在很多的一些高校网站，设置一些互动的交互选项都是吸引学生一个重要途径。同样，对于一些商业网站，设计一些优雅的交互，对于提升你的销售也有重要的作用。

根据内容和你网站的用户的特点，能够帮助你设计出更加友好的交互体验，鼠标响应就是交互设计中一个重要的途径。

### 鼠标响应 ###

我们可以利用CSS属性提供的一些方法来设计出有趣的交互动画。我们可以利用CSS提供的关键帧这一个特性来实现以前只有FLASH才能实现的一些效果。不过，稍加不注意的话，我们可能会陷入滥用CSS动画的陷阱。当然，我并不认为强制改变用户的习惯去实现我们想得到的一些交互是一个好的做法。

比如，利用鼠标滑过来完成一些交互行为是一个用户已经习以为常的习惯。并不需要花很多时间来教育用户，怎么样用鼠标来来完成交互。如果我们确定一个链接元素，要对鼠标滑入滑出添加一些交互行为，但是也不要滥用，添加一些画蛇添足的动画，这样会触怒用户，那就得不偿失啦。

### 对于移动设备，触摸设备又该如何响应用户呢？ ###

对于PC端来说，可以利用鼠标来实现人机交互。那么对于移动触摸设备，该怎么做呢？当然，一些移动设备比如三星的Galaxy S4支持hover事件，但是其它大多数设备是不支持的。除了移动设备，我们也要考虑到一些桌面设备也支持触摸事件。对于判断用触摸还是hover这个的确有点难。一个选择是，我们面向触摸来设计，为hover提供一个渐进增强的支持。
顺便可以去这里看看[fuck yeah hovers](http://fuckyeahhovers.tumblr.com/)，这里收集一些滑入滑出的一些交互动画。现在让我来考考你，鼠标的四种状态对应到触摸屏幕上是哪四种类型的触摸事件呢。

### 1.文字响应鼠标悬停事件 ###

利用鼠标改变文字颜色特别是对于链接来说是一个常见的交互设计。[best accessibility](http://www.google.com/url?q=http%3A%2F%2Fwww.w3.org%2FTR%2F2008%2FNOTE-WCAG20-TECHS-20081211%2FG183&sa=D&sntz=1&usg=AFQjCNFlNJd-aNWNcnXfi3gPKplvDraeBA),为了提高web的可访问性，达到无障碍化访问的目的。链接文字，默认有四种状态以及下划线来显示这是一个链接文字，我们不需要去特别提醒用户这是一个链接。我们可以给hover事件添加一个过渡事件，代码如下：

    a {
	color: #6dd4b1;
	transition: color 0.25s linear;  
	}
	
	a:hover, a:focus  {
		color: #357099;
	}

当然现在还是需要添加特定的前缀来兼容其它的浏览器。

下面是最终的执行结果，触摸和hover事件都支持。

<p data-height="268" data-theme-id="0" data-slug-hash="yBJzl" data-user="Jenn" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/Jenn/pen/yBJzl'>Most Basic Link Transition</a> by Jenn Lukas (<a href='http://codepen.io/Jenn'>@Jenn</a>) on <a href='http://codepen.io'>CodePen</a></p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

![](http://pic.yupoo.com/reicky_v/DomsUHVX/medium.jpg)

### 2.利用Hover事件来制作优雅的动画视觉背景 ###

我们可以进一步来探索利用Hover事件制作优雅的动画视觉背景，可以使我们的web更加漂亮和优雅。

首先，我们来看一些在这方面做的比较好的网站。进入到[CSS Off](http://www.unmatchedstyle.com/cssoff/)这个网站，滚动到嘉宾介绍的那块，试着把鼠标移入到人的头像上，可以发现会显示人的真实照片。点击进入详情页面了解更多的信息。

再来看几个网站，[Delaware Valley College](http://www.delval.edu/)以及[ Frontend Dev page](https://diy.org/skills/frontenddev)。试着在这两个网站的页面里的元素上，用鼠标来滑入滑出可以看看有哪些交互动画。

下面我们再来看看 Cowork Chicago这个网站以前的交互动画，如下：

![](http://pic.yupoo.com/reicky_v/DomM8mQT/medium.jpg)

详情可以去这个地址看看[vimeo](https://vimeo.com/)。由于这个视频众所周知的原因看不到了，原文有些内容是以这个视频来讲解的，这里就不细说了，具体可以去原文看看。

这里有一个简单的代码：

    .join-buttons .daily, .join-buttons .monthly { 
    height: 260px; z-index: 0; margin-top: 30px;
	transition: height .2s linear,margin .2s linear;
	}
	
	.join-buttons .daily:hover, .join-buttons .monthly:hover { 
		height: 280px; margin-top: 20px; 
	}
	
	li.button:hover { 
	    z-index: 20; 
	}

### 3.图片交互效果 ###

在web交互设计中，经常会设计这样一种效果，当鼠标划入图片的时候，显示一些说明性的文字，这样可以让用户知道这张图片背后的一些详细信息。效果如下：

<p data-height="268" data-theme-id="0" data-slug-hash="tElFK" data-user="Jenn" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/Jenn/pen/tElFK'>Transitioning Max Height (no text)</a> by Jenn Lukas (<a href='http://codepen.io/Jenn'>@Jenn</a>) on <a href='http://codepen.io'>CodePen</a></p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

这样的设计可能存在一个缺陷，就是它无法引导用户去发现图片背后的信息。比如如果有一张图片是链接我的twitter页面上，但是按上面这种设计，图片没有任何暗示它是链接到我的twitter页面上的。如果你在图片上放置一个@jennlukas这个文字，这样用户就明白这是关于一个twitter信息的提示，用户滑过图片也能发现详细的信息，咋看起来这跟上面的设计没多大差别，但是这能使我们的用户体验更加友好，何乐而不为呢？改完后的效果如下所示：

<p data-height="268" data-theme-id="0" data-slug-hash="zvkLy" data-user="Jenn" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/Jenn/pen/zvkLy'>Transitioning Max Height</a> by Jenn Lukas (<a href='http://codepen.io/Jenn'>@Jenn</a>) on <a href='http://codepen.io'>CodePen</a></p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

[Esquire site](http://www.esquire.co.uk/)这个网站就运用了这样的设计，正常只显示标题信息，鼠标滑入的时候就显示一些说明的信息。[ Dining at Altitude](http://diningataltitude.com/vail-restaurants/)网站则采用了一种相反的设计，正常的时候显示文字信息，鼠标滑入的时候则隐藏文字信息。这也是值得推荐的设计思路。

### 4.下拉菜单设计 ###

一般web开发中的下拉菜单都是通过鼠标来触发的，对于使用触摸设备的用户来说就有一个问题了，由于触摸设备不支持hover事件，这样触摸设备上这个下拉菜单对于用户体验来说无异于一场灾难。一个解决方法是，确保你的下拉菜单的第一级菜单选项都能链接到网站的对应的页面，而不是第一级菜单仅仅只是一个空的链接只是用来触发子菜单用的，这样就可以确保即使设备不支持hover事件，用户也能够通过一级菜单选项进入到对应的页面。这样用户也可以进入到子菜单的页面。如果导航在你的网站是用户访问次数最多的，这样的方法是值得推荐的。

### 检测浏览器是否支持hover或者是touch(触摸)事件 ###

浏览器检测在现在的web开发中是经常使用的，这样我们就可以根据浏览器的功能使用渐进增强的策略来开发我们的网站。win8上的IE10使用了[aria-haspopup](http://msdn.microsoft.com/en-us/library/ie/jj152135(v=vs.85).aspx)这个属性在触摸设备上来模拟hover事件，这个这里就不深入讨论，可能超出了本文的范围。这里有一些讨论Modernizr的问题，[ false positives](https://github.com/Modernizr/Modernizr/issues/548)。W3c在[这个规范草稿](http://dev.w3.org/csswg/mediaqueries4/#hover)中已经在媒体查询这个属性中添加hover检测特性，但是目前浏览器还不支持。不过有一些设备hover和touch事件都支持，那还要用hover来制作交互效果么？难道让用户来决定，当用户用鼠标浏览你的网站然后决定用触摸设备来浏览网站的时候，就可以切换的触摸模式浏览你的网站。反之亦然。当然这可能是一个极端的例子，对于你设计一个成功的网站没什么影响。

在开发实践中，我一般使用javascript来检测用户使用鼠标还是用触摸的方式来浏览网站。当是用触摸设备来浏览网站的时候，就会把文字全部显示，如果是桌面设备则隐藏文字，鼠标滑入的时候显示文字。具体代码结果可以在下面看到。

<p data-height="268" data-theme-id="0" data-slug-hash="ALHEf" data-user="Jenn" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/Jenn/pen/ALHEf'>Start Touch, then Detect Hover Devices with mousemove and touchstart</a> by Jenn Lukas (<a href='http://codepen.io/Jenn'>@Jenn</a>) on <a href='http://codepen.io'>CodePen</a></p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

这种做法有一个缺陷是，文字其实是始终显示的，只是用透明度把它隐藏起来。在文档加载的时候，文字还是会被加载进来，不过用户是察觉不到的。还有一个缺陷是，当用户在一个支持触摸不支持hover的设备上，用鼠标来浏览网站然后再用触摸的手势来浏览你的网站，那就会应用鼠标的样式直到页面重新加载。当然这在我做的项目中都是可以接受的，可能在其它的项目中就不适应了。

### 给用户提供选择 ###

我一直在考虑如何来确定用户是使用的鼠标还是触摸设备，更不用说是键盘或者是数位板以及像少数派报告里那样的屏幕。我们可以使用[:focus](http://fuckyeahhovers.tumblr.com/post/53291253894/fuck-yeah-keyboard-focus)这个属性来检测用户是否使用键盘，但是还是其它的问题不能解决。

![](http://pic.yupoo.com/reicky_v/DoocB7dl/medium.jpg)

还记得我们不能用浏览器来放大文字，而只能依靠一些按钮来放大缩小文字吗的情形？一个解决方案是我们可以利用用户的请求来判断加载不同的样式，从而显示不同大小的文字。我们也可以利用cookie来记住用户的选择。比如下面这个例子，我们给用户一个选择，touch或者是hover。

<p data-height="268" data-theme-id="0" data-slug-hash="cwuJf" data-user="Jenn" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/Jenn/pen/cwuJf'>Hover/Touch Switcher</a> by Jenn Lukas (<a href='http://codepen.io/Jenn'>@Jenn</a>) on <a href='http://codepen.io'>CodePen</a></p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

我们再来看看[Delta Cycle](http://deltacycle.com/bike-storage)这个网站，它们就针对移动设备和桌面电脑，对商品做了两种不同交互处理。在桌面设备上，我们看到商品默认只显示商品图片和价格，只有在鼠标划入商品图片的时候，才会显示商品名称和购买的按钮。而如果在移动设备上，由于在小的屏幕上，我们就应该改变在桌面上的交互处理方式了，具体修改如下，你可以点击**touch**和**hover**按钮来切换在两种不同模式下的交互和布局情况。

<p data-height="268" data-theme-id="0" data-slug-hash="Jdnko" data-user="Jenn" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/Jenn/pen/Jdnko'>List/Grid Views for Hover or Touch</a> by Jenn Lukas (<a href='http://codepen.io/Jenn'>@Jenn</a>) on <a href='http://codepen.io'>CodePen</a></p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

像这种针对不同设备采取不同的布局和交互，在电商类的网站很常见。

### 总结 ###

当然对于鼠标的交互模式没有一个放之四海而皆准的方案。更重要的要根据你的内容来设计你的交互方式。如果你是设计一个关于路线指南或者是健康类的应用，就应该尽量减少一些针对鼠标交互的设计，面向触摸交互设计则是一个特别好的选择。如果你是设计一个教育类型的网站，为了吸引更多的流量和注册，亦或者是要营造一个身临其境的出售派[pies]( http://emporiumpies.com/pies)的网站,则可以在鼠标的交互上多下点功夫。虽然，网站的内容是我们应该首要关心的，但是也别忘记了一个好的设计和交互也非常重要，它能够给用户带来好的体验，这也至关重要。






