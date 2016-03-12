---
layout: post
title: 复杂网站导航的响应式设计
categories:
- Life
tags:
- css
- 响应式
- 翻译
---


> 这篇文章翻译自[smashingmagazine](http://www.smashingmagazine.com/)网站的[Responsive Navigation On Complex Websites](http://mobile.smashingmagazine.com/2013/09/11/responsive-navigation-on-complex-websites/)。行文风格有改编。

一个良好的，简洁的网站导航对于一个网站的体验来说是至关重要的。在过去的几个月里，我参与两个复杂网站的导航菜单的开发。随着一个网站的内容变的越来越复杂，相应的网站的导航的层次也会变得越来越复杂，更重要的还要使网站的导航在一些小屏幕设备上保持简单可用，这对于一个网站的维护也会变得愈发艰难。

为了描述在大型网站上面怎样来打造一个响应式导航系统，我将通过我实际参与的两个项目的导航系统的构建，来说明一个响应式导航应该怎么从布局等方面一步步探索，最后构建一个响应式的导航系统。

### 伦敦的一个卫生单位的响应式导航系统（Middlesex-London Health Unit） ###

#### 开发过程 ####

一个项目成功的关键是，要随时保持跟客户的沟通，让客户参与到项目的开发中来。在开始一个项目之前，我们应该学习如何开始一个项目，在ResIM，我们是这样做的，首先开始对一个网站的目标用户进行研究，然后再通过信息架构，线框图的构建，线框的可用性测试来进行项目开发的。

我们最新的一个项目是一个公共卫生组织的项目，项目地址在这里[ Middlesex-London Health Unit](http://www.healthunit.com/) (MLHU)，项目开始后，我们首先进行对网站目标用户进行分析，分析一些关于卫生组织的网站。但是考虑到MLHU网站有1300多个页面，我们需要确保最重要内容能够优先显示给用户。

作为一个专业的设计工作室，我们进行了广泛的受众研究。我们采取的方法是，研究用户在网站上完成各种不同目的需要多长时间，然后把这个过程录像下来。当然这种方法并不是很完美，因为没有经验的用户到一个新的网站是有一定学习的成本的。但是它仍然能够提供一些有价值的内容。

当一个网站有一个良好的结构和网站地图，就可以很好地运用这个方法了，但是我们决定先不要这样做。MLHU这个组织有很多委员会的一些机构也表示需要在它们的网站上必须能够倾听到每一个用户的声音。

我们开始跟委员会的每一位委员商讨，网站的哪些部分对于用户是最重要的。听完每个成员对于哪些信息需要显示在网站上的原因后，我们开始建立一个关于网站的原型结构。我们注意到网站很多不同的内容区域，需要很好地组织它们的结构层次，以便用户能够更快的通过导航就能到达目的地。我们决定把相同类型的内容放到一个大的栏目下；最终我们把网站的导航分为四个大的栏目。

至此，导航的设计已经成功了一半，但是我们也意识到网站除了这四个部分外，其它也有些信息是用户关注的。所以我们设计了一个二级的下来菜单使用户能够访问有关网站的其它信息。建立好导航的结构后，我们就可以创建一个线框图来进行测试，模拟用户来操作网站的各个流程。

通过观察用户可以发现，用户能够使用我们的导航能够快速的找到他们想要的。这个结果是通过观察真实用户的行为得到的。下面这图是MLHU网站的网站地图：

![](http://pic.yupoo.com/reicky_v/DeE6PQ9a/medium.jpg)

现在我们就可以开始进行适应移动设备的导航的视觉设计了。同一个网站可以适应多种不同的设备。

为了使网站适应不同的设备，特别是移动设备。我们一般以平板电脑为一个基准点，来调试我们的网站在桌面设备和一些移动设备上的布局表现。根据我们对MLHU网站的用户分析，发现有一部分的用户的年龄比较大，我们决定不使用[ hamburger-style](http://dribbble.s3.amazonaws.com/users/5973/screenshots/880056/lexicon-nuggets-2-ui-hamburger.png)风格的导航。

感觉到如果使用**hamburger-style**风格的导航，可能会是一些用户使用起来比较困难，最后选用一个文字垂直布局的导航方案。这样的方案对于有多级子目录的导航来说尤其有用，而且导航更加易用。如下图所示：

![](http://pic.yupoo.com/reicky_v/DeEHACee/medium.jpg)

#### 编码实现 ####

刚开始的时候，我们还担心是否能够在保持移动设备导航和桌面导航相同结构的情况下，仅仅使用CSS来就可以使导航自适应不同设备，而不用改结构。通常的情况下，都用**hamburger-style**风格的导航来解决这样的问题。但是，由于我们的网站覆盖的用户的年龄比较广，使用文字类型的导航对用户可能更加友好一点。

如果我们想用同一个结构，仅仅使用CSS来调整导航的布局适应不同的设备，用CSS3的媒体查询很容易就可以办到。

    @media only screen and (max-width: 600px) {
	nav li {
		width: 100%;
		box-sizing: border-box;
	}

	nav a {
		background: rgba(0,0,0,0.5);
		padding: 10px 12px;
		text-align: left;
		margin: 0;
		box-sizing: border-box;
	}
	}

![](http://pic.yupoo.com/reicky_v/DeEMMDs8/medium.jpg)

我们可以用javascript来解决导航的交互行为，这里使用了[hoverIntent plugin](http://cherne.net/brian/resources/jquery.hoverIntent.html)这个插件，当然我得要它在移动设备也能正常运行。我们的解决方法是，检测设备的屏幕的尺寸，如果是小屏幕，我们就用手风琴的交互方案来代替淡入淡出的交互。手风琴的交互更适合移动设备，淡入淡出在移动设备上表现可不是很好。代码如下：

    function navIn() { 
	[...]
	} else if($(window).width() < 600) {  /* Here's where we check for the screen size and do a slide instead. */
		$(this).children('ul').slideDown();
		$(this).addClass('hovering');
	} 
	[...]
	}

### Seneca College（项目名） ###

#### 开发过程 ####

第二个实例是为一个学校设计学校的官方网站[ Seneca College](http://www.senecacollege.ca/)。这里就不能用像MLHU这个项目的方法来设计响应式导航了。我们不再需要想MLHU项目一样来分析它的受众了，因为Seneca College学校的网站目标用户很明确，就是学生。跟MLHU项目相似，我们先使用线框图工具设计在不同设备上，导航的设计布局表现方案。

由于网站的用户主要是学生这一年轻的群体，在移动设备上，我使用**hamburger-style**风格的导航方案。**hamburger-style**风格的导航在移动应用越来越越流行，采用它还可以省出很多的空间来放置网站的LOGO以及其它一些需要放到头部的元素。Seneca College学校的网站包含了很多的子页面需要显示，如果把这些子页面的链接全部放置头部的话，可能就过于拥挤了。

我的解决方法是，在每一个子页面的顶部放置子页面的导航文字链接。通过二级导航能够是用户快速定位到想要到达的子页面，而不用滚动的很长的一段距离去点击导航。随着网站的导航系统变得越来越复杂，把导航放到内容的前面有时候是必要的。

如果你不确定使用什么解决方案，不妨做一些用户的调查研究，这样就可以比较容易确定采取什么样的解决方案了。

![](http://pic.yupoo.com/reicky_v/DeF6r8NK/medium.jpg)

#### 编码执行 ####

Seneca学校的网站跟MLHU项目的还是有很大的不同的。由于是使用**hamburger-style**风格的导航，那在一般的桌面屏幕的时候，我们需要隐藏因为**hamburger-style**而存在的HTML结构，代码如下：

    <div id="mobileNav">
	<a id="searchIcon" href="#"><img src="http://media.smashingmagazine.com/images/searchIcon.png" width="24px" height="24px" alt="" /></a>
	<a id="mobileNavIcon" href="#"><img src="http://media.smashingmagazine.com/images/mobileNav.png" width="38px" height="20px" alt="" /></a>
	</div>

![](http://pic.yupoo.com/reicky_v/DeK2z6EO/medium.jpg)

    #mobileNav {
	display: none;
	}

我们用媒体查询很容就可以改变导航的样式：

    @media only screen and (max-width: 600px) {
	#mobileNav {
		display:block;
	}
	}

以此同时，我们重新设计的导航，可以自由地在不同设备上使用不同的导航布局。在移动设备上，我采用的是一种手风琴折叠式的导航设计，具体效果就像[ PageSlide](http://srobbin.com/jquery-plugins/pageslide/)，实现这样的导航方式的技术多种多样，你可以采用你自己最擅长的技术来实现。

    @media only screen and (max-width: 600px) {
	nav {
		display: none;
		width: 100%;
		background: #000;
		margin-left: -20px;
		padding-right: 40px;
	}

	nav li {
		display: block;
		margin: 20px 0 20px 20px;
		width: 100%;
	}

	nav a {
		color: #FFF;
		margin: 0;
		width: 100%;
	}
	}

我们最后是用了一点点js代码，做了这样一个效果，代码如下：

    $('#mobileNavIcon').click(function () {
	$('nav').slideToggle();
	return false;
	});

如图所示：

![](http://pic.yupoo.com/reicky_v/DeKct2Fi/medium.jpg)

当然，我们最终执行的设计方案，主要的决策来源是来自于用户的测试和提升用户体验。虽然在当前的移动应用中，** hamburger-style**风格的导航成为了一个不成文的标准，但是也不是适用全部情况。

### 其它一些技术（Other Techniques） ###

我一直在寻找一些新的解决方案来解决复杂导航的问题。下面的这些解决方法可以参考下，还是不错的。

#### PAGESLIDE ####

我非常喜欢像[ PageSlide](http://srobbin.com/jquery-plugins/pageslide/)这样的解决方案。这样的侧拉的菜单的用户体验可能更好一点。

#### RESPONSIVE MULTI-LEVEL MENU ####

[Responsive Multi-Level Menu](http://tympanus.net/Development/ResponsiveMultiLevelMenu/index.html)这个解决方案也不错。它也是一个下拉类型的菜单，还能够很好的支持移动设备的触摸屏。它能使用户能够很好的快速达到所要达到的目的地。

一个项目的解决方案最终还是要以目标用户为基础的。如果你一开始没有参与项目的设计工作，那你至少得参与项目的响应式部分的工作。总的来说，一个项目的最终目标是以你的产品的用户的为基础，要使用户用你的产品用的爽。

当然，接下来做的是，要使桌面上的导航与移动设备上的导航怎么来平滑过渡。使用javascript来创建一个动画效果？或者是从方向上来过渡？当然，最终的目的是用最少的代码，来达到效果。

当然，不要对一个复杂的导航系统感到恐惧。总能找到一种用户体验友好简单的解决方案。

这里推荐一些有用的资源：

- [ PageSlide](http://srobbin.com/jquery-plugins/pageslide/)一个jQuery插件
- [Responsive Multi-Level Menu](http://tympanus.net/Development/ResponsiveMultiLevelMenu/index.html)用来制作一个支持触摸多级的导航的教程文章
- [Convert a Menu to a Dropdown for Small Screens](http://css-tricks.com/convert-menu-to-dropdown/)也是一片讨论在小屏幕上导航的解决方案的文章
- [Responsive Navigation Patterns](http://bradfrostweb.com/blog/web/responsive-nav-patterns/)一些移动导航示范的收集



