---
layout: post
title: 响应式图片解决方案初探
categories:
- Life
tags:
- css
- javascript
- php
---

响应式设计的出发点是，移动优先、内容优先。因为如此，我们更应该关心内容在各种设备上的阅读性和显示效果。

我们都希望能在任何时间，任何设备上都能清楚的，高效的传递信息给用户。事实上这是一个常见的误解；移动优先，并不是要我们把一个网站的全部信息一股脑的都搬到移动设备上来呈现。移动优先，是要求我们把网站对用户最主要的信息呈现给用户，而其它辅助性信息可以不显示或者是不显示在第一屏这么重要的位置。

而在响应式设计中，响应式图片也是一个热点讨论的技术，关于响应式图片技术也各种各样，现就响应式图片技术做一个综合的梳理，正所谓站在前人的肩膀上，让我们看的更远！

现在流行的响应式图片技术主要是分为三种，分别为CSS、javascript和服务端这三种解决方案。现逐一梳理。首先是CSS的解决方案。

### CSS ###

最简单的CSS解决图片自适应方案，莫过于在CSS中设置图片的最大宽度为100%就可以了，如下：

    img {
    	max-width: 100%;
	}

在大多数情况下，这句简单的代码足可以应付。设置好这句CSS后，图片就会根据父级元素的大小来自动调节自身的大小适应设备的宽度。设置图片的最大宽度为100%，也能确保图片在自适应过程中不降低图片的质量。如果你想支持IE6-8这类浏览器，你还需要加一句*width:100%*，因为它们不支持*max-width:100%*。

当然，这种方案不是完美的，当你想在*Retina*这种高分辨率屏幕上也能正常的显示高质量的图片的时候，你原本只是150*150的图片，现在你得准备300*300的图片，这样才能在高分辨率屏幕上显示完美。你可能会这样做：

    img.sitelogo {
    	max-width: 150px;
	}

不幸的是，它没有像我们想的那样来同比例显示图片。想让图片自适应大小，我们不应该限制图片本身的大小，而是应该限制图片被包裹容器的大小。例如，我们包裹在图片外面的容易的类名是*branding*。那得写如下所示的代码：

    .branding {
    	max-width: 150px;
	}

这样我们的图片就可以自适应了，无论是在*Retina*还是在其它屏幕上。

到目前为止，好像这方案很完美的样子，实则不然，试想一下，上面的响应式图片方案，是让所有的设别都用同一个图片文件。这在一些小的按钮图片或者是logo可能没多大问题，但是你想一下，当我们的响应式图片还要面对像*Retina*高分辨率的视网膜屏幕的时候，那就意味着我们的图片是非常大的，如果让所有的设备都用这样一种高质量图片，而实际上一般的屏幕也显示不了这种高质量图片的细节。

想象一下，我们真的需要发送一张990 × 300像素的图片给一个普通屏幕的手机设备来显示吗？你可能会说，现在一般都是用Wi-Fi，这点流量不算什么啊。但是他们也可能在没有Wi-Fi的条件下，浏览你的网站，这样可想而知，要加载这样一张大的图片要花多少时间。要知道，加载速度慢，就可能让你失去一个客户。

#### CSS3媒体查询（media queries） ####

CSS3中新出现的媒体查询，也可以很好地帮我们来响解决应式图片问题，而且不会出现我们上面说到的问题。只不过需要我们准备不同分辨率的图片。下面我们就看看怎么运用CSS3的媒体查询（media queries）来做响应式图片解决方案。

比如像针对苹果 MacBook Pro 这样的*Retina*屏幕的时候，我们可以这样写：

	@media only screen and (min-width: 1440px) {
		/* styles for MacBook Pro-sized screens and larger */
	}

我们就可以把我们准备的高清图片写在上面的代码块了，这样在高分屏幕的时候，CSS就会智能判断调用高清图片。

它这里判断的是根据浏览器*viewport*属性来判断屏幕分辨率的。

当然1440px是在浏览器全屏时候的大小，有可能当前浏览器的大小可能比1440px小一点，为了更好地应付这种情况，我们可能需要调整下我们的样式：

    @media only screen and (min-width: 1200px) {
	/* styles for wide screens */
	}

CSS3的媒体查询由两个部分构成，第一部分是*only screen*，这部分声明表明，里面的样式不适用打印或者非标准屏幕等设备。第二部分是*min-width: 1200px*，指当屏幕大于这个值的时候，才应用这里面的样式。

比如，针对一般的智能手机设备，噩梦可以这样写：

    @media only screen and (max-width: 320px) {
    	/* styles for narrow screens */
    }

用*min-width*和*max-width*，在确定用户设备宽度的情况下非常有用。但是，对于一个*Retina*视网膜屏幕，这可能就不是很好用了。因为不同浏览器对于*Retina*屏幕的像素显示单位有不同的语法表示，为了识别高清屏幕，我们可以用下面这样的语法来表示：

    @media
	only screen and (-webkit-min-device-pixel-ratio: 2),
	only screen and (min--moz-device-pixel-ratio: 2),
	only screen and (-moz-min-device-pixel-ratio: 2),
	only screen and (-o-min-device-pixel-ratio: 2/1),
	only screen and (min-device-pixel-ratio: 2),
	only screen and (min-resolution: 192dpi),
	only screen and (min-resolution: 2dppx) { 
		/* styles for Retina-type displays */
	}

这样，每一个浏览器都能支持*dppx*这样的单位来识别高清屏了。

下面，我们利用CSS背景图片的技术，来一个简单的响应式图片解决方案.代码如下：

    <!DOCTYPE html>
	<html lang="en">
	<head>
		<meta charset="utf-8">
		<title></title>
		<style>
		#image { 
			background-image: url(largeimage.jpg); 
		}
		@media only screen and (max-width: 320px) {
			#image { 
				background-image: url(smallimage.jpg); 
			}
		}
		</style>
	</head>
	<body>
		<div id="image"></div>
	</body>
	</html>

那我们该怎样来打造我们的响应式图片解决方案呢？或许我们可以参考下[contfont.net](http://contfont.net/)这个网站的方案，

当然，在打造响应式图片前，我们要尽可能优化压缩我们的图片，这样才能加载的更快，如果你的图片是JPEG的时候，我们可以在导出的时候选中连续这个选项，这样我们图片在网页中使用时候，首先会加载一张低质量的图片，直至图片全部加载完，才显示高质量的图片。

在编写响应式图片方案的时候，首先得确定最小分辨率的图片，比如[contfont.net](http://contfont.net/)网站中，最小的分辨率是320px,来对应不是视网膜屏幕的之类的智能手机设备，比如iphone等。在[contfont.net](http://contfont.net/)网站中，320px分辨率下的图片的尺寸是290 × 183像素。想 [contfont.net](http://contfont.net/)的响应式图片全部类型如下：

![](http://pic.yupoo.com/reicky_v/DawuN7iv/medium.jpg)

我们也可以参考这样的形式来打造我们响应式图片的解决方案。

那么就可以用媒体查询来使用我们响应式图片了，摘自[contfont.net](http://contfont.net/)网站的样式，如下：

    /* default screen, non-retina */
	.hero #cafe { background-image: url("../img/candc970.jpg"); }
	
	@media only screen and (max-width: 320px) {
	    /* Small screen, non-retina */
	    .hero #cafe { background-image: url("../img/candc290.jpg"); }
	}
	@media
	only screen and (min-resolution: 2dppx) and (max-width: 320px) {
	    /* Small screen, retina */
	    .hero #cafe { background-image: url("../img/candc290@2x.jpg"); }
	}
	@media only screen and (min-width: 321px) and (max-width: 538px) {
	    /* Medium screen, non-retina */
	    .hero #cafe { background-image: url("../img/candc538.jpg"); }
	}
	@media
	only screen and (min-resolution: 2dppx) and (min-width: 321px) and (max-width: 538px) {
	    /* Medium screen, retina */
	    .hero #cafe { background-image: url("../img/candc538@2x.jpg"); }
	}
	@media
	only screen and (min-resolution: 2dppx) and (min-width: 539px) {
	    /* Large screen, retina */
	    .hero #cafe { background-image: url("../img/candc970@2x.jpg"); }
	}

CSS目前来说，就上面两种方案比较常用，实用性也表较强。

当然，在最新的以webkit为内核的"safari6"和“chrome21“的浏览器中，支持支持CSS4的background-image新规范草案image-set。通过Webkit内核的浏览器私有属性“-webkit”，image-set为Web前端人员提供了一种解决高分辨率图像的显示，用来解决苹果公司提出的Retian屏幕显示图片的技术问题。简而言之：这个属性用来支持Web前端人员解决不同分辨率下图片的显示，特别的（Retina屏幕）。比如：

    selector {
	  background-image: url(no-image-set.png);
	  background: image-set(url(foo-lowres.png) 1x,url(foo-highres.png) 2x) center;
	}

类似于不同的文本，图像也会显示成不同的：

不支持image-set：在不支持image-set的浏览器下，他会支持background-image图像，也就是说不支持image-set的浏览器下，他们解析background-image中的背景图像；

支持image-set：如果你的浏览器支持image-sete，而且是普通显屏下，此时浏览器会选择image-set中的@1x背景图像；

Retina屏幕下的image-set：如果你的浏览器支持image-set，而且是在Retina屏幕下，此时浏览器会选择image-set中的@2x背景图像。

image-set目前是一个全新的属性，没有几个浏览器支持，但这是一种新技术，我们需要用起来，只有用的人多了，将来才有机会写入标准的规范中，正所谓“世上本无路，走的人多了，路就出来了”。

HTML中也有响应式解决方案，像Intention.js这个库，提供一个轻量级的和明确的方式，帮助你动态重组 HTML，成为响应式的方式。操作方法都放在了元素自己里面，所以灵活的布局看起来就似乎不会那么的抽象和凌乱。

您可以轻松地增加布局的选择和灵活性，缩短开发时间和减少样式表覆盖媒体查询驱动的必要性。你可以轻松地添加自己的上下文或者创建自己的自定义组。

可以去它的网站看看，[这里](http://intentionjs.com/)。

下面接着介绍下javascript提供的解决方案。

### javascript响应式图片解决方案 ###

javascript现在也有一些解决方案，都是通过判断分辨率大小来加载图片的。

#### HISRC ####

[HiSRC](https://github.com/teleject/hisrc)是一个基于jQuery的插件，它可以通过设置不同分辨率来加载不同的图片，它会基于网速和是否是视网膜高清屏来显示图片。

![](http://pic.yupoo.com/reicky_v/DawIfhzd/medium.jpg)

使用前，要加载jQuery和HiSRC脚本文件。

在HTML中，先要设置不同分辨率，不同图片的的路径。要条件一个类，这样好让HiSRC脚本能正常的工作。然后添加一些特别的属性：data-1x 和 data-2x这样来指定不同分辨率和不同的图片，如下：

    <img src="http://placekitten.com/200/100" data-1x="http://placekitten.com/400/200" data-2x="http://placekitten.com/800/400" class="hisrc" />

然后，在DOM加载完后，调用hisrc提供的方法就可以了：

    $(document).ready(function(){
	  $(".hisrc").hisrc();
	});

这里，首先会加载*http://placekitten.com/200/100*这个图片。然后hisrc脚本会判断设备屏幕的尺寸，如果是高清屏就会加载*data-2x="http://placekitten.com/800/400" *这张图片，如果不是高清屏机会加载*@1x*这张图片。

这里，你可能会注意到*http://placekitten.com/200/100*这张图片总是会加载，不管脚本是否根据分辨率不同加载对应的图片。当谈，它只是在判断你的网络环境为3G环境的情况下，会加载两次图片。如果不是3G的情况下，它只会加载默认的第一张图片。

但是由于依赖jQuery，所以当你不打算用jQuery的时候，它就无用武之地了。

类似的还有Picturefill这个脚本文件，也提供了一种解决方案，它不依赖jQuery,但是依赖*picturefill.js*这个脚本文件，并且它还对结构有一定的要求，就想CSS3中的媒体查询一样，对格式有特定的要求，而且还提供当不支持javascript的回退方案。

这里有一个简单的例子，如下：

    <span data-picture data-alt="Descriptive alt tag">
	    <span data-src="images/myimage_sm.jpg"></span>
	    <span data-src="images/myimage_lg.jpg" data-media="(min-width: 600px)"></span>
	
	    <!--[if (lt IE 10) & (!IEMobile)]>
	    <span data-src="images/myimage_sm.jpg"></span>
	    <![endif]-->
	
	    <!-- Fallback content for non-JS browsers. -->
	    <noscript>
	        <img src="images/myimage_sm.jpg" alt="Descriptive alt tag" />
	    </noscript>
	</span>

上面的代码，很容易明白。设置不同分辨率加载不同的图片。按照它的这个规范，我们就可以很容易制作响应式图片解决方案了。

当然由于Picturefill需要大量额外的标签，可能不是一个很好地选择。更重要的是它没有像上面的HISRC有检测网络环境的机制。

当然上面两种的js方案，都需要在客户端来执行，而且还要增加额外的标签，这无疑会增加客户端的负担和页面的加载速度，特别现在中国的这种网络环境下，用户对于流量和加载速度是很敏感的。

### PHP服务端处理响应式图片 ###

如果，我们在服务端就处理好响应式图片，直接根据用户的客户端的分辨率情况，来加载不同的图片，这样在同等的条件下就可以大大提高网页的加载速度了，需要在服务端用到 Adaptive Images这个类库，基于Apache 2 服务器, PHP 5.x 和 GD库。

具体使用方法，可以去它的官方网站看看，有详细的使用教程，[官网](http://adaptive-images.com/)。这里再推荐一篇关于这个的使用的文章[这里](http://www.webdesignerdepot.com/2012/07/adaptive-images-solving-the-responsive-image-problem/)。

**Adaptive Images**的一个优势是，你不需要在你的HTML添加任何的额外的标签，只需要正常的引入图片就可以了。当在服务端设置好后，PHP会根据用户设备的大小自动生成不同大小的图片，返回给客户端。而且，它还提供了很多的设置，比如生成图片的分辨率的种类，图片质量高低等等。

当然，它也有一些弱点：

- 由于它是基于PHP和Apache的，如果你的服务器环境不是PHP的话，那就可能用不上了。
- 由于它是直接通过拦截通过服务器的图片请求来生成图片的，如果你的图片是通过第三方的分发网络的话，就用不上了。
- 它不检测用户的网络环境
- 它不能解决一些需要艺术处理的图片，因为它只是在原图的基础上重新调节图片的大小

OK，你可能会注意到，上面的解决方案中，不是需要javascript，就是需要一些特别的设置。你可能想要的方案是不需要任何设置，接着往下看：

### SENCHA.IO SRC ###

[SENCHA.IO SRC](http://www.sencha.com/products/io/) 是一个第三方的提供响应式图片的解决方案的服务。你不需要在你的服务器设置任何东西，只要把你的图片路径的前面指向Sencha的服务器上就可以啦，它会自动处理好图片响应式部分的工作。

比如，在设置前，我们的图片设置是这样的：

    <img src="http://www.your-domain.com/path/to/image.jpg" alt="My large image" />

用SENCHA的服务，只需要在你图片的路径前指向SENCHA.IO服务器就行了，如下：

    <img src="http://src.sencha.io/http://www.your-domain.com/path/to/image.jpg" alt="My large image" />

默认情况下，SENCHA会通过user-agent来判断用户的设备，从而来响应式的设计图片。比如一般的智能手机会设置图片的宽度为320，而像一般的平板类的则设置图片的宽度为768px等等。

当然，你也可以通过Sencha.io服务器设置返回指定尺寸的图片。Sencha.io是免费提供这项服务的，当然也有一些风险，由于它是免费的第三方服务，有可能存在超出你控制范围内的风险，比如当机等风险，这个是需要权衡的。

### RESRC.IT ###

[ReSRC.it](http://www.resrc.it/)跟上面的Sencha.io差不多类似，也是一个第三方的响应式图片解决方案。但是它可以提供图片滤镜和网络宽带环境监测而不依靠user-agent检测。（好像被墙了）

在开始使用前，你需要注册ReSrc.it的账号。

然后，在你的html中引入它们的js文件，当然它们也支持异步脚本加载方案，用来提高性能。

    <script src="//use.resrc.it"></script>

引入之后，就可以像下面这样来加载你的图片文件了，在图片路径前，加上ReSrc.it的路径以及还需要添加一个resrc类，如下所示：

    <img src="http://trial.resrc.it/http://www.your-domain.com/path/to/image.jpg" alt="My large image" class="resrc" />

ReSRC.it还允许你指定一些参数来处理图片，比如翻转、剪裁、旋转等等。比如下面的这个例子，表示水平翻转图片，设置宽度为300，压缩JPEG的图片质量为80：

    <img src="http://trial.resrc.it/r=h/s=w300/o=80/http://www.your-site.co/image.jpg" alt="My large image" class="resrc" />

### 测试、测试、测试 ###

最后还是要通过不断的测试，才能确定选用哪一种解决方案。下面有一些测试的工具，可以参考下：

#### YSLOW ####

这个是久负盛名的雅虎提供的测试工具。它基于雅虎提出的web性能优化的23调规则，来测试你的web性能，并且给出一些优化的建议。在chrome、firefox上都有扩展提供。

#### WEBPAGETEST ####

[WebPageTest](http://webpagetest.org/)是一个在线工具由GOOGEL提供的。你只要输入你的网址，指定测试用的浏览器。它还提供了很多选项设置，测试web的各个部分的性能，比如javascript、HTTP响应等等关乎web性能的代码测试。它会把测试结果制作成表格或者是图片截图给你。

上面就是有关响应式图片解决方案的一个简单的梳理，以备用到时按需选择。

这篇文章参考下面这些文章：

[http://speckyboy.com/2012/11/20/responsive-image/](http://speckyboy.com/2012/11/20/responsive-image/)。

[http://heygrady.com/blog/2012/05/25/responsive-images-without-javascript/](http://heygrady.com/blog/2012/05/25/responsive-images-without-javascript/)。

[http://mobile.smashingmagazine.com/2013/07/22/simple-responsive-images-with-css-backgrounds/](http://mobile.smashingmagazine.com/2013/07/22/simple-responsive-images-with-css-backgrounds/)。

[http://mobile.smashingmagazine.com/2013/07/08/choosing-a-responsive-image-solution/](http://mobile.smashingmagazine.com/2013/07/08/choosing-a-responsive-image-solution/)。

[http://csswizardry.com/2011/07/responsive-images-right-now/](http://csswizardry.com/2011/07/responsive-images-right-now/)。

[http://www.sitepoint.com/css3-responsive-centered-image/](http://www.sitepoint.com/css3-responsive-centered-image/)。




 
