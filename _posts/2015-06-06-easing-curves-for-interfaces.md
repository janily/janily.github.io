---
layout: post
title: 打造丝丝润滑的动画体验
categories:
- Life
tags:
- css3
- animation
- 贝塞尔曲线
- 前端

---

> 要创造高质量动画效果，曲线是必备的杀器。那么在动画效果设计中，要怎么来运用曲线打造丝丝润滑的交互动画体验呢？今天这就来聊聊这个。[原文地址](https://medium.com/@ryan_brownhill/crafting-easing-curves-for-user-interfaces-34f39e1b4a43)。

曲线是用来定义动画运动的速度与时间的。在CSS3它还有一个别名，叫贝塞尔曲线。比如CSS3动画中默认的ease-in，ease-out或者是ease-in-out就是贝塞尔曲线。

###曲线基本知识

曲线有两根轴，X轴和Y轴。在动画中这两根轴分别是：X轴表示动画，Y轴表示动画的时间。

![image](https://d262ilb51hltx0.cloudfront.net/max/800/1*yrj0VOEk_rciKIDglyvF-A.png)

在实际开发中，就是表示控制动画在完成过程中每一帧所需要的时间。

### Timing和Spacing

Timing表示完成整个动画所需要的时间。Spacing在这里表示整个动画过程每一帧所需要的时间。比如在下面这张图中，就演示了曲线与动画之间的关系。

<video loop="" video="" autoplay="" class="graf-image" data-image-id="1*fpHbdiO48eNmQzAZG8S2zQ.gif" data-width="762" data-height="366"><source src="https://d262ilb51hltx0.cloudfront.net/max/1600/1*fpHbdiO48eNmQzAZG8S2zQ.ogv" type="video/ogg"><source src="https://d262ilb51hltx0.cloudfront.net/max/1600/1*fpHbdiO48eNmQzAZG8S2zQ.mp4" type="video/mp4">Your browser does not support the video tag.</video>

在线性的曲线中，动画的每一段所占用的时间都是一样的，所以，我们看到整个动画都是匀速进行的。实际[代码地址](http://codepen.io/ryanbrownhill/pen/EjVdeY)。

####Ease In曲线

<video loop="" video="" autoplay="" class="graf-image" data-image-id="1*7LJjBNRmXRhfAQV2kC2Z8g.gif" data-width="740" data-height="352"><source src="https://d262ilb51hltx0.cloudfront.net/max/1600/1*7LJjBNRmXRhfAQV2kC2Z8g.ogv" type="video/ogg"><source src="https://d262ilb51hltx0.cloudfront.net/max/1600/1*7LJjBNRmXRhfAQV2kC2Z8g.mp4" type="video/mp4">Your browser does not support the video tag.</video>

上面是Ease In曲线的时间与空间的一个百分比。可以看到动画刚开始的时候，相对于整个动画过程中占用了相对多的空间，即刚开始的时候动画运动的慢一点，到后面运动的快一点。[代码地址](http://codepen.io/ryanbrownhill/pen/VLvEre)。

####Ease Out曲线

<video loop="" video="" autoplay="" class="graf-image" data-image-id="1*u2F7k1-MldDAVaR3HS456w.gif" data-width="724" data-height="362"><source src="https://d262ilb51hltx0.cloudfront.net/max/1600/1*u2F7k1-MldDAVaR3HS456w.ogv" type="video/ogg"><source src="https://d262ilb51hltx0.cloudfront.net/max/1600/1*u2F7k1-MldDAVaR3HS456w.mp4" type="video/mp4">Your browser does not support the video tag.</video>

上面是Ease Out的曲线示意。可以看到以快速开始，以慢速结束的过渡效果。[代码地址](http://codepen.io/ryanbrownhill/pen/MwaPBZ)。

###设计曲线

对于开发者来说，什么时候该设计和使用曲线？是一个困扰的问题。我觉得，这里要根据整个应用的场景来决定。一个曲线并不能使用所有的动画场景。

最重要的是，我们设计的动画要符合真实的物理运动规律。

现实生活对于设计动画效果具有重要的参考作用。比如，在真实的世界中，物体的运动不会一直保持匀速，并且能立即停止－比如linear曲线。在真实的世界中，物体的运动往往是伴随着加速和减速来运动的。比如，迪斯尼著名的[12个动画原则](https://vimeo.com/93206523)，就是基于真实世界中物体运动规律而得来的。

<iframe scrolling="no" frameborder="0" id="player" src="https://player.vimeo.com/video/93206523?api=1&amp;referrer=https%3A%2F%2Fmedium.com%2Fmedia%2Fa5136e7cc9261a75e0fad063493487cf%3FmaxWidth%3D700" allowfullscreen="true"></iframe>

在设计曲线的时候需要记住一点的是，曲线往垂直方向，表示物体运动的越快；曲线往水平方向，表示物体就运动的越慢。基于这个原理，我们就可以设计各种不同的曲线。

<video loop="" video="" autoplay="" class="graf-image" data-image-id="1*AtW9LyqTeYScAwCShyoFxw.gif" data-width="1128" data-height="392"><source src="https://d262ilb51hltx0.cloudfront.net/max/1600/1*AtW9LyqTeYScAwCShyoFxw.ogv" type="video/ogg"><source src="https://d262ilb51hltx0.cloudfront.net/max/1600/1*AtW9LyqTeYScAwCShyoFxw.mp4" type="video/mp4">Your browser does not support the video tag.</video>

[代码地址](http://codepen.io/ryanbrownhill/pen/mJeQyq?editors=110)

在曲线设计中，还可以把动画的某一帧一拆为二。这样就可以创造出缓动的运动效果。

<video loop="" video="" autoplay="" class="graf-image" data-image-id="1*ENr717Pm2gm6ps4AvH39lQ.gif" data-width="788" data-height="758"><source src="https://d262ilb51hltx0.cloudfront.net/max/1600/1*ENr717Pm2gm6ps4AvH39lQ.ogv" type="video/ogg"><source src="https://d262ilb51hltx0.cloudfront.net/max/1600/1*ENr717Pm2gm6ps4AvH39lQ.mp4" type="video/mp4">Your browser does not support the video tag.</video>

[代码地址](http://codepen.io/ryanbrownhill/pen/zGrNwv?editors=110)

网络上有很多的工具来设计曲线。

 - [cubic-bezier.com](http://cubic-bezier.com/#.17,.67,.83,.67)
 - [Cesear](http://matthewlein.com/ceaser/)
 - [Easings.net](http://easings.net/)

###开发者眼中的曲线

在开发中，曲线即规定过渡效果的速度曲线。也经常叫做贝塞尔曲线。

![image](https://d262ilb51hltx0.cloudfront.net/max/1654/1*vWeVRPeCyo8Ul6G7CPibLA.png)

在很多的编程语言中，会默认定义一些曲线。在CSS3中也内置了一些常见的动画曲线：

* ease-in = cubic-bezier(.42, 0, 1, 1)
* ease-out = cubic-bezier(0, 0, .58, 1)
* ease-in-out = cubic-bezier(.42, 0, .58, 1)

####在CSS中定义曲线

在CSS中，我们可以在animation中使用animation-timing-function这个指定动画曲线。

<video loop="" video="" autoplay="" class="graf-image" data-image-id="1*VhiXDe5IlAQ9ZKwDDloOzg.gif" data-width="538" data-height="532"><source src="https://d262ilb51hltx0.cloudfront.net/max/1600/1*VhiXDe5IlAQ9ZKwDDloOzg.ogv" type="video/ogg"><source src="https://d262ilb51hltx0.cloudfront.net/max/1600/1*VhiXDe5IlAQ9ZKwDDloOzg.mp4" type="video/mp4">Your browser does not support the video tag.</video>

	.object-class {
 		animation-name: animation-rocks;
 		animation-timing-function: cubic-bezier(1,.01,.91,.46);
	}	
	
[代码地址](http://codepen.io/ryanbrownhill/pen/JdYmqG)

如果想更细致的控制动画效果，还可以针对每一帧来指定曲线。

<video loop="" video="" autoplay="" class="graf-image" data-image-id="1*pIfuxLTA9waZJ4MVvBoHOg.gif" data-width="526" data-height="518"><source src="https://d262ilb51hltx0.cloudfront.net/max/1600/1*pIfuxLTA9waZJ4MVvBoHOg.ogv" type="video/ogg"><source src="https://d262ilb51hltx0.cloudfront.net/max/1600/1*pIfuxLTA9waZJ4MVvBoHOg.mp4" type="video/mp4">Your browser does not support the video tag.</video>

	@keyframes animation-name {
  			0% {
    animation-timing-function: cubic-bezier(1,.01,.91,.46);
  	}
  		25% {
    animation-timing-function: linear;
  	}
  	50% {
    animation-timing-function: cubic-bezier(0,.02,0,1.01);
  	}
  	75% {
    animation-timing-function: linear;
  	}
  	100% {
    animation-timing-function: linear;
  	}
	}
	
[代码地址](http://codepen.io/ryanbrownhill/pen/JdYejX)

### 延迟动画效果

我们还可以使用**animation-delay**来设计延迟运行动画。比如下面使用了SASS和Compass来定义延迟运行动画。

<video loop="" video="" autoplay="" class="graf-image" data-image-id="1*wz068HDsQ3byZZ1cHCJsfw.gif" data-width="450" data-height="305"><source src="https://d262ilb51hltx0.cloudfront.net/max/1600/1*wz068HDsQ3byZZ1cHCJsfw.ogv" type="video/ogg"><source src="https://d262ilb51hltx0.cloudfront.net/max/1600/1*wz068HDsQ3byZZ1cHCJsfw.mp4" type="video/mp4">Your browser does not support the video tag.</video>







