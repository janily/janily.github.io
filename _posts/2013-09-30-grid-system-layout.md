---
layout: post
title: 用SASS打造自适应网格布局系统
categories:
- Life
tags:
- css
- sass
- 翻译
---

本文中用到的源代码[下载地址](http://mos.creativebloq.com/tutorials/2013/css-237.zip)。

> 这篇文章翻译自creativebloq网站的[Create flexible grids using Sass](http://www.creativebloq.com/web-design/create-flexible-grids-using-sass-9134524)。行文风格有少量改编。

如果你是一个网页设计师，或者是做跟网页设计有关的工作，那你在开始设计一个网站布局之前，你可能先要设计一个网格系统，这个过程可能有一点点痛苦，但是相对于网格系统对整个网站的视觉整体的一致性来说，设计一个好的网格系统还是值得的。

试想一下，当你设计一个响应式网站的时候，你会面对这样一个问题，不同布局情况下，网页布局元素的宽度问题。你难道想去计算每一种布局情况下，元素的宽度么？你需要的是一个自适应的网格系统来自适应调整元素的宽度。

现在一般都会用到一些网格系统的解决方案。比如很流行的960网格系统，但是随之而来也要接受一些在框架中本来就有的问题。

比如，它会带来一些冗余的代码，当你要调整你的布局的时候，它还是需要你去计算宽度，它还会添加一些没有语义的代码在你的HTML代码中。

![](http://pic.yupoo.com/reicky_v/DcsYTVRW/medium.jpg)

当然这并不意味着你不需要一个网格系统来辅助你的网页设计，本文就是提示你，怎么来打造一个符合自己需求的网格系统。

像上面我们提到的一些网格系统存在的一些缺点，从框架的适应性来，是可以接受得。但是如果我们来做的话，应该可以做的更好。这里我们会用到SASS这个CSS预编译语言来开发网格系统，能大大的提高我们得开发效率，并且也能够编译为CSS语言，也不需要添加无语义的标签到我们的HTML文件中。

一个网格系统应该能够面对各种不同的布局情况，最重要的是还能够保持系统简单可用。

在网页中媒体的尺寸和文字的行高，都会影响元素之间的距离。

当然要打造一个完美的网格系统并不是本文的目的，但是我这里可以提供一些关于网格系统的参考资料，来给你参考 。

这里推荐这两个工具[Modular Grid Pattern](http://modulargrid.org/)或者是[Gridulator](http://gridulator.com/)。

我个人推荐Gridulator这个工具，它能够根据你输入的宽度和网格列数自动帮你计算出网格列的宽度和边距。一个缺点就是它不能用百分比的方式来创建一个流体布局。不过这好像也是这类工具共同的一个缺点，在本文中，我们将尽量的解决这些问题。

![](http://pic.yupoo.com/reicky_v/DctSt1Fm/medium.jpg)

再本文中，我们将使用[Elliot Jay Stocks](http://elliotjaystocks.com/blog/a-better-photoshop-grid-for-responsive-web-design/)提供的网格系统为模板来开发我们得网格系统，因为它能够很好的处理百分比的问题。这个模板总宽度为1000px，分为六列每列宽度为150px,每列之间的间距是20px。我们将使用 [Ethan Marcotte](http://alistapart.com/article/fluidgrids) 提供的一个公式来处理百分比问题。

    target / context = result

按照这个公式，如下：

	150 / 1000 = 0.15

这样就可以得到我们的宽度是15%，边距是2%。

现在你应该明白了使用Elliot Jay Stocks提供的网格系统为模板来开发我们的网格系统的原因啦，因为这样我们就不用纠结于宽度的计算问题，从而把经历放到其它一些重要的概念上。

接下来，就是编写HTML结构。我们将以上面说的理论为基础来写一个实例。打开我们下载好的源代码里的01-markup这个目录里的index.html  文件，可以看到我们得HTML文件是以[HTML5 Boilerplate](http://html5boilerplate.com/)这个项目提供的模板为基础的。当然，在实际的生产环境中，我们需要根据自己的需求来删减一些代码。

看了代码后，你会发现这里也有一些没有语义的代码。但是，从我们演示的这个例子看，这些类名实际上是有语义的。

![](http://pic.yupoo.com/reicky_v/DcuaeIzi/medium.jpg)

像现在一些比较好的网格系统的背后都有一个简洁、健壮的HTML结构。

在开始SASS编写前，需要RUBY语言环境。为了更好的发挥SASS的特点，我们也需要一个两好的SASS文件结构。当然最后编译的时候，会统一编译到一个CSS文件中。下面就是SASS的文件结构。

	@import 'variables'; // 全局需要用到的一些变量
	@import 'mixins'; // 一些公共的模块
	@import 'typography'; // 基础得文字样式
	@import 'grid'; // 网格系统的方法
	@import 'global'; // 这是我们实际编写SASS的文件

当然，我们还需要引入一个重置样式。

现在打开下载好的源代码里的03-base-styles这个目录里的文件css/sass/_variables.scss，这里存放了网格系统将要用到的一些变量。我们这里使用了单行的注释风格 //，这样在编译后的文件中，注释就不会被编译了。

如果你打开css/sass/_mixins.scss 这个文件，你会看到这里的模块在网格系统里都非常有用。

基于 [Paul Irish's advocacy](http://www.paulirish.com/2012/box-sizing-border-box-ftw/) 的建议，我们在网格系统中使用 box-sizing: border-box;这个属性来定义元素盒模型得渲染方式，这样对于我们的网格系统是非常有用的，当然这个属性不支持IE7以下得浏览器，不过可以用脚本解决兼容性问题。

打开css/sass/_typography.scss.这个文件，这里面定义了一些基本的文字样式，这里用的是多行的注释风格，它会编译到最终的CSS文件中。下图就是我们最终的一个文件结构：

![](http://pic.yupoo.com/reicky_v/DcuouWN1/medium.jpg)

这个文件结构一看就知道各个文件的具体意思。

一些基础设置完成后，接下来就开始编写我们的网格系统的方法了，打开04-grid-functions这个目录里的文件css/sass/_grid.scss 里面声明了网格系统方法要用到的一些变量，如下：

	$max-width: 1000px; // 设置页面的宽度
	$column-width: 15%; // 设置每一列的宽度
	$gutter-width: 2%; // 设置边距
	$maximum-columns: 6; // 设置最大可以存在的列数

下面我们将创建网格系统响应式列宽和边距的方法，这个模块主要是来自SASS这个[Bourbon](http://bourbon.io/)库中提供的模块方法。当然为了更好的匹配我们的网格系统，我们修改了方法的名字。

	@function columns($columns, $container-columns: $maximum-columns) {
	  $width: $columns * $column-width + ($columns - 1) * $gutter-width;
	  $container-width: $container-columns * $column-width + ($containercolumns - 1) * $gutter-width;
	  @return percentage($width / $container-width);
	}
	@function gutter($container-columns: $maximum-columns, $gutter:
	$gutter-width) {
	  $container-width: $container-columns * $column-width + ($containercolumns - 1) * $gutter-width;
	  @return percentage($gutter / $container-width);
	}

用上面的方法，我们就可以设置元素的宽度和边距，下面来一个实例：

	div.parent {
	  width: columns(3);
	  margin-right: gutter;
	  div.child {
	    width: columns(1, 3);
	    margin-right: gutter(3);
	  }
	}

column function 方法至少需要一个参数才能正常工作，这个参数就是我们想要设置列的数量的值，设置好后，这个方法将会自动计算元素的宽和边距。如果我们的元素是在另外一个列的里面，那么我们可以传第二个参数给这个方法。这个时候元素的宽度是以父级元素宽度为基础的，所以我们需要以父元素的宽度为基础来调整子元素的宽度的百分比。我们把父元素的宽度也就是列得数量传入第二个参数，我们的网格系统的SASS方法将会自动来计算调整子元素宽度的百分比。

![](http://pic.yupoo.com/reicky_v/DcuNh7Kc/medium.jpg)

现在一个基本的布局样式已经搭建好了。接下来，就可以以这个为基础来使用网格系统来创建灵活的布局样式。

控制边距的方法需要一个参数，当一个布局元素需要边距的时候，它就非常有用了，只需要传递一个参数给它，它就会自动根据父元素的宽度计算调整好边距。

现在我们的网格系统可以灵活的处理元素的宽度和边距了，但是为了能使我们的网格系统能更好的工作，我们还得添加一些样式。

一个典型的例子是，在网格系统中，一般都需要一个名为row 的类，来包裹一个布局区块。

![](http://pic.yupoo.com/reicky_v/DcuUikFU/medium.jpg)

现在，我们的网格系统基本上能够正常使用了。当然，我们还需要完善下网格系统，可以使它能适用于更多的情况。

这里我们将创建一个模块 ，用来包裹布局元素。当然这个模块并没有指定给特定的类才能使用，只要是用来控制布局元素的都可以用这个模块。这个模块主要包括控制布局区域的宽度、删除内边距、清除浮动这几个功能。

	@mixin row {
	  width: 100%;
	  max-width: $max-width;
	  margin: 0 auto;
	  @include clearfix;
	  @include nesting;
	}

在我们上面的模块中包含了nesting这个模块，主要是用来控制这个模块的第一个子元素浮动以及边距用的，我们这里还改变了它的盒模型为borderbox。这样大大提高了这个模块的可用性。

	@mixin nesting {
	  padding: 0;
	  & > div {
	    float: left;
	    margin-right: gutter();
	    @include border-box;
	  }
	}

在一个网页布局中，留白是一个影响网页美感的一个重要因素。谁也不愿意面对一个密密麻麻的，拥挤的网站。所以我们的网格系统也应该支持留白这样的布局。所以我们这里创建一个offset模块来实现留白这样的布局需求。它需要两个参数，一个是控制元素的偏移方向，一个是控制偏移的距离。这里也会涉及一些计算，具体的代码如下：

	@function offset-columns($columns) {
	  $margin: $columns * $column-width + $columns * $gutter-width;
	  @return $margin;
	}
	@mixin offset($from-direction, $columns) {
	  @if $from-direction == left {
	    float: left;
	    margin-left: offset-columns($columns);
	  }
	  @if $from-direction == right {
	    float: right;
	    margin-right: offset-columns($columns);
	  }
	}

到这里我们还有一个问题需要处理，就是最后一个元素的边距。在我们的网格系统中，我们给每个布局元素添加了边距，但是在一个布局中，常常会碰到最后一个元素的边距问题，需要删除最后一个元素的边距，这样才不至于因为最后一个元素的边距而破坏整个布局。当然现在有很多解决方案，但是没有一个是完美的解决方案。

第一种方案是，用 :last-child 这个伪类选择器来解决。但是IE8以及IE8以下的浏览器不支持伪类选择器。这个方案还不适合大规模应用。

第二种方案是，用元素的左边距来代替右边距，删除第一个元素的左边距也可以达到目的。并且兼容性也不错，但是不要高兴的太早，我们需要的方案是不需要去删除一个特定布局中的第一个或者是最后一个元素的外边距，需要的是在任何情况下都适用的方案。也不需要为了删除边距，而改变我们的布局结构。
 
还有一种方案也是用 :nth-child()这个选择器来做，同样也会有兼容性问题。当然我们后面的解决方案跟这些伪类选择器比起来，没有这么优雅。

![](http://pic.yupoo.com/reicky_v/Dcv887nz/medium.jpg)

从上面的图中可以看到，百分比跟具体的数字比起来，并不是很精确。而且有些浏览器的计算方式也有差异。但是用浮动的话，可以避免这个问题。

很多网格系统都有last或者是end这样的类名来解决元素的边距问题。当然从解决问题的可用性的角度来说，这样的方法还不错，但是它也会添加额外的类在你的文件中。如果用SASS中的模块来做的话，就没有这样的问题了，我们可以在我们布局中的任何元素中用它来解决边距问题。虽然没有 :nth-child()的方案这么优雅，但也不错，是不。

	@mixin last {
	  margin-right: 0;
	  float: right;
	}

我们解决的方法是，浮动最后一个元素并去掉外边距。这样就能避免上面说到的百分比问题。

现在，我们的网格系统基本上设置好了，当然在正式开始前，我们还是要设置一些基本样式。

	body { padding: 0 1em; margin: 0; }
	.container { width: 100%; max-width: $max-width; margin: 0 auto; padding: 0; }

上面我们设置了body的内边距，这样在一些小的设备上能更好的展现我们的网站。你可能注意到container这个类的样式和row这个模块的样式差不多是一样的，这是因为不想和用到row这个模块的子元素的样式冲突。而且container这个类，在网页中只会用到一次。

下面，我们先设置我们布局元素的样式：

	.two-columns,
	.six-columns,
	.varying-columns,
	.nested-columns,
	.more-nested-columns,
	.offset-columns {
	  @include row;
	}

设置后，就可以针对每一个元素来写样式了。运用我们网格系统中提供的方法，可以打造出伸缩自如的网格布局。这里就不贴代码了，可以去看下载下来的代码中这个目录 05-final 中css/sass/_ global.css 这个文件中 查看具体代码。最终效果如下图所示：

![](http://pic.yupoo.com/reicky_v/DcvpSpXF/medium.jpg)

用我们这个网格系统不仅能制作出灵活的网格布局，最重要的是简单易用。

当然在最终的生产环境中，我们的SASS文件结构还有改进的余地，在本文中，我们的文件结构是按具体的用途来组织的。

最后来回顾下我们网格系统开发之旅。我们首先是设计了一个网格系统和用百分比的形式来表示宽度。然后我们写了一个简洁的HTML结构来作为我们项目开始的骨架。我们用SASS这种CSS预编译的语言，设计我们需要的方法来。最后用网格系统开发了一个简单的网格布局。

简而言之，我们打造的这一个网格系统可以正式用在项目中，当然一些变量的值可能需要调整。还在等什么，马上开始用网格系统来开发我们的项目吧！
