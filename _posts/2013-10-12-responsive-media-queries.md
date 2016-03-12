---
layout: post
title: 基于媒体查询开发响应式网站
categories:
- Life
tags:
- css
- 响应式
- 翻译
---


> 这篇文章翻译自[sixrevisions](http://sixrevisions.com/)网站的[Design-Based Media Queries](http://sixrevisions.com/web_design/design-based-media-queries/)。行文风格有改编。


为了使我们的网站能够在不同设备正常访问，我们使用CSS3中的媒体查询技术，来快速实现了一个响应式的网站，可以使它能够在iphone等智能设备上能够正常的显示。

但是随着移动互联网的发展，特别是移动设备市场占有率越来越高，是时候重新思考响应式在网站设计中的重要性了，可以去看看这篇文章[common responsive design breakpoints ](http://css-tricks.com/snippets/css/media-queries-for-standard-devices/)，我们先前的方案并不是很好，我们不得不重新来思考调整我们网站在响应式开发中的设备响应断点的设置。

在本文中，我会来说说基于媒体查询怎样来开发响应式网站。这种开发方法对于未来的网站开发大有裨益。

### 为什么我们需要重新来思考基于媒体查询的网站开发方式（Why Our Media Queries Need to Change） ###

目前使用媒体查询的方式基本上都是根据设备的尺寸来使用的。

响应式开发刚兴起的时候，那时候移动设备也仅仅限于苹果和安卓这两种设备。

所以，那时候的媒体查询的使用方法都很简单，如下所示：

    /* iPhone and other smartphones (portrait) */
	@media screen and (max-device-width: 320px) {
	
	}
	
	/* iPhone and other smartphones (landscape) */
	@media screen and (max-device-width: 480px) {
	
	}

随着技术的发展，移动智能设备市场也是日新月异，比如高清视网膜屏幕的兴起以及屏幕大小和分辨率也是参差不齐。所以，媒体查询的技术也得与时俱进来适应技术的发展。

为了适应新的设备，现在的媒体查询的使用是时候做出改变了，如下所示：

    /* iPhone and other smartphones (纵向模式) */
	@media screen and (max-device-width: 320px) {
	
	}
	
	/* iPhone and other smartphones (横向模式) */
	@media screen and (max-device-width: 480px) {
	
	}
	
	/* iPad and other tablets (纵向模式) */
	@media screen and (max-device-width: 768px) {
	
	}
	
	/* iPad and other tablets (横向模式) */
	@media screen and (max-device-width: 1024px) {
	
	}
	
	/* iPhone 5 (纵向模式) */
	@media screen and (max-device-width: 568px) and (orientation: portrait) {
	
	}
	
	/* iPhone 5 (横向模式) */
	@media screen and (max-device-width: 568px) and (orientation: landscape) {
	
	}

现在由于三星很流行，所以针对它，我们的媒体查询还是要调整一下，如下：

    /* Samsung Galaxy (portrait) */
	@media screen and (max-width: 385px) {
	
	}
	
	/* Samsung Galaxy (landscape) */
	@media screen and (max-width: 690px) {
	
	}
	
	/* iPhone and other smartphones (portrait) */
	@media screen and (max-device-width: 320px) {
	
	}
	
	/* iPhone and other smartphones (landscape) */
	@media screen and (max-device-width: 480px) {
	
	}
	
	/* iPad and other tablets (portrait) */
	@media screen and (max-device-width: 768px) {
	
	}
	
	/* iPad and other tablets (landscape) */
	@media screen and (max-device-width: 1024px) {
	
	}
	
	/* iPhone 5 (portrait) */
	@media screen and (max-device-width: 568px) and (orientation: portrait) {
	
	}
	
	/* iPhone 5 (landscape) */
	@media screen and (max-device-width: 568px) and (orientation: landscape) {
	
	}

那Kindle或者是iPad mini呢？

不要担心，这里有一个关于各种设备的媒体查询，里面就有Kindle和iPad mini的媒体查询以及其它的设备的媒体查询的列表[a list of media queries](http://code-tricks.com/css-media-queries-for-common-devices/)。

上面所说的一些关于媒体查询的调整，说明了如果不好好的理解媒体查询还改进媒体查询，那我们来开发响应式网站的时候就会遇到很多麻烦的地方。

在响应式网站开发中，我们应该坚持一个原则：不要把PC桌面的网站和移动网站割裂开来，它们应该是一个整体。

根据具体设备的尺寸来作为我们媒体查询开发的一个基本点，不仅仅是一个移动网站，也能够在iphone、Android、Blackberry这类智能设备上也能很好的浏览。

基于媒体查询的响应式网站的开发方法，主要是因为现在各种移动设备的厂商都是各自为战，没有一个统一的标准，现在的智能设备的市场，跟现在的浏览器的市场一样，各自为战。

对于响应式开发来说，并不是Android的设备有多少种，可以看看这个链接[ list of Android screen sizes](http://stackoverflow.com/questions/7587854/is-there-a-list-of-screen-resolutions-for-all-android-based-phones-and-tablets)。而是我们应该怎样来设置媒体查询的断点，同样这里也提供一个关于断点问题的链接[ popular responsive design breakpoints are right now ](http://stackoverflow.com/questions/16443380/common-css-media-queries-break-points)，我们也许还会有另外一个疑问：

设置短点后，我们又该做些什么呢？

如果我们的网页设计是面向未来的，更重要的是无论现在还是未来，网页都是不可或缺的。那我们的媒体查询需要根据我们的设计为基础，而不是根据移动设备来为基础。

### 为什么需要重新调整旧的断点设置(Why the Old Breakpoints Aren’t Working) ###

让我们来通过一个真实的案例，来说明以设备为基础来设置媒体查询的断点是不可行的。

为了使我们公司的网站[Compass Design](http://compass-design.co.uk/)能够响应各种不同设备，我们仅仅使用了三个媒体查询的断点设置，如下:

    /* iPad (portrait) */
	@media screen and (max-width: 760px) {
	
	
	}
	
	/* Smartphones (landscape) */
	@media screen and (max-width: 480px) {
	
	}
	
	/* Smartphones (portrait) */
	@media screen and (max-width: 320px) {
	
	}

这是几年前的代码了，当大多数的人都在使用iphone4（320x480px）或者是ipad(760x1024px)，这类设别来访问公司网站的时候，没有什么问题。

用桌面浏览的时候，一个标准的布局，一个水平方向的导航，左右两栏的布局，如下图所示：

![](http://pic.yupoo.com/reicky_v/DetIhRCr/medium.jpg)

当用平板这一类的设备访问的时候，布局大体也没怎么变化，只是导航为了适应屏幕而稍微改变了下：

![](http://pic.yupoo.com/reicky_v/DetIGJEW/medium.jpg)

当用智能手机访问的时候，布局就改变了，导航移到了页面的底部：

![](http://pic.yupoo.com/reicky_v/DetKfgtA/medium.jpg)

### 与时俱进 ###

今时不同往日，随着移动设备市场的发展，人们访问互联网的设备也越来越多样化。

比如：

- iPhone 5在横向模式的时候的宽度为 568px
- 一些Android设备的尺寸更是百花齐放，比如三星的Galaxy的尺寸是380x685px
- 平板设备的尺寸也有不同，现在一般为7寸或者是10寸

让我们用iphone5来访问网站看看。因为iphone5的尺寸在横向模式的时候比480px要大，这个时候使用的布局模式是平板电脑上的布局模式。有问题啦，这个时候导航看起来就非常的拥挤。如图：

![](http://pic.yupoo.com/reicky_v/DetOjLDy/medium.jpg)

我这里只是为了说明，如果仅仅以设备来作为我们响应式开发的基础的话，是不可行的。尤其是面对未来的设备，这样做也是不可取的。

那我们改怎么做呢，改怎么样来设置我们响应式开发的断点呢？

### 基于媒体查询的设计 ###

现在在我们公司网站的断点设置主要是以两个原则为准：

- 内容需要自适应任何的设备，这样才能保证内容在任何设备下可读，易读。
- 在移动设备上以及支持触摸屏的设备上：导航的点击区域需要足够大以保证用户很容易就能够点击。

在桌面电脑的布局是：1005px宽，水平方向的导航，如下：

![](http://pic.yupoo.com/reicky_v/DetIhRCr/medium.jpg)

以这个为基准，我们就可以设置一个断点：

    /* Breakpoint #1 for medium-sized screens */
	@media screen and (max-width: 1005px) {
	
	}

你可以在我的样式表里看到，基于上面的两个原则，我删除大部分针对移动设备的样式。

接下来，是在745px这种情况下的布局，截图如下：

![](http://pic.yupoo.com/reicky_v/DetSOJRZ/medium.jpg)

看图可知，这种情况下，导航区域变的很拥挤，不易点击，所以可以在这里设置一个断点，来调整下布局，如下：

    /* Breakpoint #2 for small-sized screens */
	@media screen and (max-width: 745px) {
	
	}

![](http://pic.yupoo.com/reicky_v/DetTWvPn/medium.jpg)

现在，我们网站的断点设置完全与设备无关，也能够适应任何设备的访问。

这种基于内容的媒体查询的开发方法，只需要关注一点的是内容与导航的可读性和易用性。

再强调一下：当我们来设计一个响应式网站的时候，记住我们需要关注的以内容为基础的设计，而不是以设备为基础。

### 未来网站设计的展望（Moving Towards a Universal Web） ###

基于媒体查询开发响应式网站以一个再简单不过的开发方法。在一个网站项目中，我们一般通过调整浏览器窗口的大小来检测我们网站是否能够正确的响应不同尺寸情况，这种方法也是能够接受的。

无论未来的各种厂商制如Apple、Samsung、Sony以及其它一些厂商会生产出怎么样的设备，我们还是可以通过使用标准的媒体查询来适应它。媒体查询不会因为未来的发展或者是设备的发展而过时。

当然，如果我们重新审视一下互联网这一伟大发明的本质的所在，继续来创建一个与设备无关、与浏览器无关，与公司无关的网络世界，我们就可以远离媒体查询啦。




