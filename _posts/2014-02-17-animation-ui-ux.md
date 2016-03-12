---
layout: post
title: 	UI动画和用户体验：运用之妙，存乎一心
categories:
- Life
tags:
- css
- 翻译
---

> 这篇文章来自于[alistapart](http://alistapart.com/)网站的[UI Animation and UX: A Not-So-Secret Friendship](http://alistapart.com/article/ui-animation-and-ux-a-not-so-secret-friendship)。具体翻译有删减。

对于非常注重细节的web世界来说，一些细微的动画能大大提高web的用户体验。随着CSS3的普及，在web中越来越多的使用CSS3来制作一些交互动画，本文就来说说用CSS3来制作交互动画的一些事情。

### CSS动画概述 ###

本文主要是使用CSS3中的animation和transition来制作动画效果。下面我们来简要介绍下着两个东东：

CSS中的animation(动画)和我们平时看到的动画实现的原理相似。在CSS中实现动画，需要依靠**@keyframes**关键帧来实现。并且配合一些其它参数的设置，比如动画时间(animation duration)、播放方式(animation timing funtion)等就可以制作出一个非常棒的动画效果。如下代码所示：

    .animation {
	…
	animation: bounceAround 1.1s ease-in-out infinite;
	}
	
	@keyframes bounceAround {
		0% {transform:translateY(0);}
		20% {transform:translateY(-60px) rotate(0deg);}
		25%{transform:translateY(20px) rotate(0deg);}
		35%, 55%{transform:translateY(0px) rotate(0deg);}
		60% {transform: translateY(-20px) rotate(0deg);}
		100%{transform: translateY(-20px) rotate(360deg);}
	}

CSS也支持过渡(transition)，所谓过渡就是一种动画转换过程，如渐显、渐弱、动画快慢等。制作一个过渡类型的动画效果，首先需要指定一个转换的元素的属性，还需要指定触发两个状态之间元素属性值的一个转换。这样好像有点抽象，看下面代码就明白了：

    .transition{
	…
	transition: .4s background ease-out;
	}
	
	.transition:hover {
		background:#e65445;
	}

在实际开发中，animation和transition一般都是一起使用的，所以我们下面的实例也是一起使用它们来制作动画效果。[这个例子](http://codepen.io/valhead/pen/61c28980cb3edf974e499735a05cbdb8)就是使用animation和transition一起实现的。

### UI动画实战 ###

在什么时候使用动画效果是最合适的呢？一般是在web元素状态发生变化的时候添加状态之间变化的一个动画效果。

下面我们就来看一些实际的例子：

### 路径导航(从哪儿来，到哪儿去) ###

人类大脑会对移动的物体特别敏感。在现实生活中，比如魔术师就会利用移动的物体来吸引分散观众的注意力；汽车展上，各大厂商会利用车模来吸引眼球。在web设计中，我们也可以利用它来引起用户的注意力，例如我们可以使用抽屉的效果来显示隐藏信息。如下所示：

<p data-height="268" data-theme-id="0" data-slug-hash="KcGol" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/KcGol'>Show/hide example</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

在上面这个例子中，当鼠标移入到菜单区域中的时候，会显示详细信息的子菜单。移入触发动画这个会吸引用户的注意力，提醒用户主菜单下面还有更详细的信息可以查看。

其实上面这个例子中不需要动画效果也能很好的工作。这里添加动画效果是为了创建一个更好的视觉效果，在越来越讲究设计细节的今天，这个非常重要。

### 状态之间转换的动画效果 ###

一个元素两种不同状态之间的转换动画效果在web设计中使用的非常普遍。

比如下面这个例子：

<p data-height="268" data-theme-id="0" data-slug-hash="CJHdk" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/CJHdk'>Modal Relationship Examples</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

在这个例子中，当点击文件图标的时候，会弹出一个对话框展示这个文件更多详细的信息。当你关闭对话框的时候，又会恢复原来的样子。这样的动画交互效果同样是用的非常广泛。

比如在电商类型的网站这样的效果就是用的非常普遍，通常点击缩略图的时候，会显示详细图片的信息。并且用户体验也不错。

同样你可以在Basecamp和IOS7中也有运用这样的动画效果，不过具体运动效果就不相同了。

下面我们再看一个点击或者是触摸屏上的tap的动画效果。如下所示：

<p data-height="268" data-theme-id="0" data-slug-hash="HamEF" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/HamEF'>Adding Items</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

你可以点击**add**或者是**remove**看看具体的效果，当你点击**add**按钮的时候，我们可以看到列表项目会往下移动给新的项目腾出一个新的位置，并且会高亮当前新增的项目列表，当新增完后颜色会恢复到跟现在列表的颜色一样。

当交互变得越来越复杂的时候，这种在web交互中使用提示型的动画更加重要。在web中，我们经常要操作大量的数据，实时更新数据以及其它更加复杂的任务处理。在移动互联迅猛发展的今天，我们还面对各种触摸屏上的交互开发。所有的这些都给交互动画开发提供了更加宽广的舞台。

当web设计碰到动画，动画的使用才会变得非常有趣，动画的使用往往会给用户带去意想不到的惊喜。

### UI动画tips ###

在web设计中使用良好的动画会大大提高web的用户体验。在现代web设计开发中，就就交互动画开发这块还不是很成熟一般的公司都是由ui设计师或者是前端工程师处理，正式因为交互动画还处于一个混沌的阶段，只要我们在平时的设计中稍加训练我们也可以在项目中设计很棒的交互动画。

要想正确的在恰当的时候使用恰当的交互动画的一个重要的技巧是跟用户联系紧密的状态信息传递或者是状态转换的时候使用动画来来展现信息的传递或者是状态的转换。比如**Fitbit**在它的[web应用](http://d.alistapart.com/390/ui-animation-and-ux-a-not-so-secret-friendship/fitbit_dashboard_animation.mov)和它的[客户端](http://d.alistapart.com/390/ui-animation-and-ux-a-not-so-secret-friendship/fitbit_app_animation.mov)上就是使用动画来表示用户相关数据的更新。对于那些特别注意数据的用户来说，运用动画的形式来表现用户数据的更新确实能够带来更好地用户体验。

当然交互动画的存在不能影响web功能上的完整性。有时候动画的滥用反而会影响用户体验。比如square就重新设计了它的导航，[它把导航设置为一个弹出的对话框](http://d.alistapart.com/390/ui-animation-and-ux-a-not-so-secret-friendship/square_overdone_menu.mov)。为了使用它的导航，你需要等到导航对话框打开，并且对话框打开之后具体导航栏目还有一个淡入的动画。这种过度的使用动画无疑就影响到了用户的体验。

当然这只是我的一家之言-也许你就非常喜欢这种效果。“过度使用”这些一般只是人们的主观感受，无法形成一个统一的认识。不过过度的的动画设计确实能给一些人们造成一些不适的感觉，比如有人就反应IOS7中一些动画的转场效果就给他们带了[不适的感觉](http://www.theguardian.com/technology/2013/sep/27/ios-7-motion-sickness-nausea)。虽然这样情形比较少见，特别是在一个操作系统级别的设计之中这样的情形更加难以掌握，这里有一个[WCGA动画设计指南](http://www.theguardian.com/technology/2013/sep/27/ios-7-motion-sickness-nausea)。可以去看看。

同时动画运行的时间也是区别一个好的交互动画和一个糟糕动画的一个指标。动画运行的时间是一个动画存在最直接的感受之一。比如，在CSS中**CSS-IN**表示动画执行将是由一个慢到快的过程来执行动画，跟总的动画时间无关。是一个渐现的效果，如果在一个按钮上应用这个效果，就会给用户造成一个到底有没有点击按钮的困扰。所以我建议在按钮的一些交互动画效果上尽量避免使用**ease-in**这类的动画效果。

一个很好的交互动画效果如果和整个系统不匹配的话，那么它也是失败的。比如，在一个游戏应用中[弹性菜单的效果](http://d.alistapart.com/390/ui-animation-and-ux-a-not-so-secret-friendship/dots.mov)就用的非常好，而在[苹果网站上](http://d.alistapart.com/390/ui-animation-and-ux-a-not-so-secret-friendship/apple_odd_motion.mov)应用这个效果确显得比较别扭。交互动画更应起到沟通连接的作用，如果我们在设计交互动画的时候不考虑这一点，那就得不偿失了。

一个好的交互动画应该是能很好传递设计目的以及表现产品特色，只有这样才能更进一步的设计好的交互动画。而不仅仅是为了动画而动画，把交互动画设计融入设计之中。

当在你的项目之中添加交互动画的时候，我们应该不停地使用原型设计工具来迭代设计它。使它更加完善和设计融为一体。正所谓熟能生巧。借助一些原型设计工具我们能更快设计一些交互动画的原型，比如使用After Effects，Edge Animate或者另外一些工具来设计交互动画原型能使我们更快实现我们心目中的交互动画。至于用代码来实现能够在线上运行的实际代码在设计阶段我们不用关心。我们的目标是快速设计可用的原型，在设计的同时我们可以从用户的角度来问自己下面几个问题：

- 交互动画是否能够提供有用的信息？
- 是否能够正确的响应用户？
- 在用户当时的场景下是否能响应用户？

当然在根据浏览器的能力的情况下，我们应该采用渐进增强的开发方法来设计交互动画。针对一些比较低级的浏览器我们也应该照顾到这部分浏览器用户的体验。

我们可以去[caniuse](www.caniuse.com)这个网站来了解浏览器对[animation](http://caniuse.com/#search=animations)和[transitions](http://caniuse.com/#search=transition)的支持情况。不过这里也要注意的是，支持并不意味着浏览器在表现和性能上也是一样的，我们可以去这里看看[关于一些属性在动画上性能表现](http://www.html5rocks.com/en/tutorials/speed/high-performance-animations/)。这对于我们设计更好地交互动画也有很大的帮助。

UI交互动画设计对于web设计师来说是一个很强大的工具-它能够使我们的web设计在交互上能够媲美本地应用。

实际上，我们可以做的比一些本地应用更好。特别随着CSS3和HTML5的发展，没有想不到，只有做不到！


