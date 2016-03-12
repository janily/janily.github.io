---
layout: post
title: 响应式设计：用html5和css3创建一个流体布局
categories:
- Life
tags:
- css
- 翻译
---

> 这段时间在做公司官网，是固定布局的，我想把它做成自适应式的布局也就是流体布局。按照我一贯的做法，首先会用google搜搜有关这方面的资料，正所谓站在前人的肩膀上，能让我们看的更远。虽然现在到处都是在吹捧响应式设计、响应式开发。而真正理解响应式开发不见得有多少，在我看来在还没有弄懂何为响应式设计、响应式开发的情况下，采用流体布局也许是一个不错的选择，流体布局能很好地适应各种设备，不至于在一些小屏幕的设备上，网站的表现惨不忍睹。今天的这篇文章是2012年的一篇老文，看了下对于理解和应用流体布局还是有一很大帮助的，原文地址是[reate fluid layouts with HTML5 and CSS3](http://www.creativebloq.com/css3/create-fluid-layouts-html5-and-css3-9122768)。具体翻译的时候，有删减。

![](http://pic.yupoo.com/reicky_v/DshTj3T5/medium.jpg)

开始之前可以去下载已经做好的[源代码](http://mos.creativebloq.com/tutorials/2012/netmag-create-fluid-layouts-with-html5-and-css3.zip)。

这篇文章主要是来自Ben Frain的《Responsive Web Design with HTML5 and CSS3》这本书中一个章节，[已经出版了](http://www.packtpub.com/)。

在20世界90年代互联网刚开启的阶段，网站基本上还是处于table的天下。很多时候，都是用百分比的形式来表示屏幕的宽度的。因为那时候的屏幕的尺寸和浏览器也不像现在这样百花齐放。所以一个网站的布局非常容易把控。

在那个时代基本上没人会关心，网站在不同的设备上的表现。然而，随着css技术的出现和发展，网站的布局越来越和现实中的印刷行业的布局和表现靠拢。于是基于像素单位的固定布局统治了web的布局，一直延续至今。

正所谓物极必反，现在按百分比的布局又重新回来了。特别是现在各种终端的出现，流体布局反而比固定布局更能适应现在的web世界。

### 本文会给你带来什么 ###

- 为什么流体布局(百分比)是响应式设计一个不可缺少的一部分
- 从基于像素的布局转换为基于百分比的布局
- 用em代替以px为单位的字体设计
- 了解和处理元素的上下文

### 1.固定布局在可见的未来还不会消失 ###

正如在前面讲到的，在以表格布局的年代，都是采用固定布局的，通常一个网站会要求是950-1000px的宽度，如果采用百分比来进行布局，你会发现在不同屏幕上，网站的宽度是不一样的。你的上级主管会跟你说，怎么跟设计出来的宽度不一样啊，赶紧搞成一样的啊。这个时候采用以像素为单位的固定布局就有了用武之地，在哪都一样。

即使是现在，有了媒体查询这样的技术来根据不同设备来改变布局，但是实际上仍然是采取固定像素来布局的。然而，在现在这个各种尺寸设备层出不穷的时代，我们依然要面对各种不同的设备来调整我们的web布局。

### 2.为什么基于百分比的流体布局对于响应式设计来说是必不可少一个部分 ###

虽然媒体查询非常强大，但同样还是 有一些限制。任何固定设计，用基于媒体查询来进行适配不同设备的时候，也只是针对不同设备的宽度来设置元素的布局，并没有一个可控的规律来操作。

相反，如果我们创建一个灵活的采取百分比的流体布局，那我们的网站就可以同时面对各种设备来自适应调整，而不是用媒体查询来针对每一种设备来调整。这个时候流体布局的优势就发挥出来了。

### 3.流体布局和媒体查询 ###

Ethan Marcotte写了[ definitive article ](http://www.alistapart.com/articles/responsive-web-design/)这篇文章关于响应式设计的，虽然他使用的流体布局和媒体查询是早已经存在的技术，不过他里面关于响应式设计的思想在今天看来依然是可以借鉴的。对于从事web设计的工作者来说，他的这篇文章提供了两个关于web设计的新思路：一个是基于流体布局来设计web网站，另外一个就是使用媒体查询。把它们和响应式设计结合起来，才能充分发挥响应式设计的价值。

### 4.从固定布局到流体布局 ###

在可预见的未来，固定布局不会消失。当我们开始制作一个网站的时候，我们用photoshop把设计稿打开，然后确定网站的宽度，各种元素的尺寸、元素与元素之间的margin以及图片的尺寸，然后在CSS中用像素来表示它们。那我们该如何把固定的尺寸转换为百分比的形式呢？

### 5.记住一个公式 ###

Mr. Marcotte在他的[Handcrafted CSS](http://handcraftedcss.com/)这本书中提供了一个简单的公式用来把固定的单位转换为百分比的单位：

    target ÷ context = result

当我们准备采用响应式来设计网站的时候，这个公式会常伴我们左右。实践是检验理论的标准，光说不练假把式，就让我们通过一个实际的网站[ And The Winner Isn't...](http://www.andthewinnerisnt.com/index.html)来把它从固定布局转换为流体布局。

### 6.拥抱流体布局 ###

首先我们来看一下这个网站的一个基本的HTML结构：

      <div id="wrapper">
         <!-- the header and navigation -->
         <div id="header">
           <div id="navigation">
             <ul>
               <li><a href="#">navigation1</a></li>
               <li><a href="#">navigation2</a></li>
             </ul>
           </div>
         </div>
         <!-- the sidebar -->
         <div id="sidebar">
           <p>here is the sidebar</p>
         </div>
         <!-- the content -->
         <div id="content">
           <p>here is the content</p>
         </div>
         <!-- the footer -->
         <div id="footer">
           <p>Here is the footer</p>
         </div>
    </div>

这里需要注意的是我们在CSS中把网站的主要部分(头部、导航、边栏以及内容区域)设置了固定的宽度。如下所示：

     #wrapper {
      margin-right: auto;
      margin-left: auto;
      width: 960px;
    }
    #header {
      margin-right: 10px;
      margin-left: 10px;
      width: 940px;
    }
    #navigation {
      padding-bottom: 25px;
      margin-top: 26px;
      margin-left: -10px;
      padding-right: 10px;
      padding-left: 10px;
      width: 940px;
    }
    #navigation ul li {
      display: inline-block;
    }
    #content {
      margin-top: 58px;
      margin-right: 10px;
      float: right;
      width: 698px;
    }
    #sidebar {
      border-right-color: #e8e8e8;
      border-right-style: solid;
      border-right-width: 2px;
      margin-top: 58px;
      padding-right: 10px;
      margin-right: 10px;
      margin-left: 10px;
      float: left;
      width: 220px;
    }
    #footer {
      float: left;
      margin-top: 20px;
      margin-right: 10px;
      margin-left: 10px;
      clear: both;
      width: 940px;
    }

在上面的代码中可以发现我们所有的单位都是用像素表示的。现在就让我们用**target ÷ context = result**这个公式来把像素转换为百分比。

在我们的HTML结构中，我们使用ID为**#wrapper**的DIV来包裹我们整个网站的内容。你可以在上面的CSS看到我们把它的宽度设置为960px。那该怎么样把它转换为百分比呢？

### 7.根据上下文元素之间的的关系来设置单位 ###

在设置**content, sidebar, footer**这些元素宽度的时候，我们首先需要设置这些元素共同的父级元素的宽度，因为它们的宽度的值会根据父元素的宽度来设置。那我们先来设置**#wrapper**这个元素的单位，因为它是网页内容的共同的父元素。根据公式可得**#wrapper**的宽度为96%，如下所示：

     #wrapper {
         margin-right: auto;
         margin-left: auto;
         width: 96%; /* Holding outermost DIV */
    }

在浏览器中的表现如下所示：

![](http://pic.yupoo.com/reicky_v/Dsl8hupJ/medium.jpg)

设置好后，可以发现在浏览器中表现的非常好。

从固定布局转变为流体布局确实比固定布局复杂一些。接下来我们看看头部该怎样用百分比的形式来设置宽度。按照我们的公式** target ÷ context = result **，**#header**这个DIV在 **#wrapper**的里面，并且它的宽度是940px，所以按照公式我们可以得出它的宽度为** 97.9166667%**，css修改如下：

    #header {
      margin-right: 10px;
      margin-left: 10px;
      width: 97.9166667%; /* 940 divided by 960 */
    }

同样我们也可以计算出**#navigation**和**#footer**的宽度。

接下来是**#content**和**#sidebar**这两个DIV，**#content**的宽度为698px,依葫芦画瓢我们可以得到**#content**的宽度为**72.7083333%**。

sidebar的宽度是220px，但是还有2px的边框在里面。所以我们需要减去2px的边框的宽度，这样我们才能得到sidebar内容的真实宽度，所以sidebar的宽度是218px，按照公式计算得到宽度为**22.7083333%**。

最后把我们上面计算出来的宽度对应原先的CSS修改如下：

      #wrapper {
      margin-right: auto;
      margin-left: auto;
      width: 96%; /* Holding outermost DIV */
    }
    #header {
      margin-right: 10px;
      margin-left: 10px;
      width: 97.9166667%; /* 940 divided by 960 */
    }
    #navigation {
      padding-bottom: 25px;
      margin-top: 26px;
      margin-left: -10px;
      padding-right: 10px;
      padding-left: 10px;
      width: 72.7083333%; /* 698 divided by 960 */
    }
    #navigation ul li {
      display: inline-block;
    }
    #content {
      margin-top: 58px;
      margin-right: 10px;
      float: right;
      width: 72.7083333%; /* 698 divided by 960 */
    }
    #sidebar {
      border-right-color: #e8e8e8; border-right-style: solid; border-right-width: 2px; margin-top: 58px;
      margin-right: 10px;
      margin-left: 10px;
      float: left;
      width: 22.7083333%; /* 218 divided by 960 */
    }
    #footer {
      float: left;
      margin-top: 20px;
      margin-right: 10px;
      margin-left: 10px;
      clear: both;
      width: 97.9166667%; /* 940 divided by 960 */
    }

下面这个截图是在Firefox浏览器视图的宽度为1000px的情形下的表现形式：

![](http://pic.yupoo.com/reicky_v/Dslhlr7O/medium.jpg)

### 8.数字之外 ###

现在让我们花些时间来考虑调整后布局其它方面的问题。在响应式设计的时候有一个争论的地方的是，认为在CSS中编写像**550724638%**这样的数据是非常愚蠢的。你可能会认为他们太敏感了。而支持此写法的人认为只有这样才能更加精准。只有这样浏览器才能更加精准的显示表现它们。

除此之外，你可能也听说过关于[黄金比例](http://en.wikipedia.org/wiki/Golden_%20ratio)在设计中的运用。黄金比例中的比值为** 1:1.61803398874989**，它也没有采用四舍五入的方法来表示，因为这个数字对于黄金比例来是非常重要的，并不是数字整不整洁的关系。如果连黄金比例也能容忍这样长的小数点，那在网页布局中又有何不可呢？

### 9.转换其它元素 ###

布局方面的元素处理之后，现在该处理布局以外其它元素了，如margin和padding的值，依然按照公式来计算margin和padding的值都是10px，依据公式计算可得其值为**1.0416667**(10 ÷ 960)。

现在一切看上去，各个元素表现的好像没啥问题。不过，导航区域好像有一点问题，当我把浏览器窗口缩小的时候，导航会折成两行，如下所示：

![](http://pic.yupoo.com/reicky_v/Dslnalhc/medium.jpg)

此外，当我放大浏览器窗口的时候，元素之间的margin值没有按比例增加。回到CSS来找找原因：

      #navigation {
      padding-bottom: 25px;
      margin-top: 26px;
      margin-left: -1.0416667%; /* 10 divided by 960 */
      padding-right: 1.0416667%; /* 10 divided by 960 */
      padding-left: 1.0416667%; /* 10 divided by 960 */
      width: 97.9166667%; /* 940 divided by 960 */
      background-repeat: repeat-x;
      background-image: url(../img/atwiNavBg.png);
      border-bottom-color: #bfbfbf;
      border-bottom-style: double; border-bottom-width: 4px;
    }
    #navigation ul li {
      display: inline-block;
    }
    #navigation ul li a {
      height: 42px;
      line-height: 42px;
      margin-right: 25px;
      text-decoration: none;
      text-transform: uppercase;
      font-family: Arial, "Lucida Grande", Verdana, sans-serif;
      font-size: 27px;
      color: black;
    }

在**#navigation ul li a**这条CSS规则中，margin的单位还是固定的像素25px。我们需要把它转换为百分比的形式。**#navigation**的宽度是940px，按照公式可得margin值为**2.6595745%**。css修改如下：

    #navigation ul li a {
      height: 42px;
      line-height: 42px;
      margin-right: 2.6595745%; /* 25 divided by 940 */ text-decoration: none;
      text-transform: uppercase;
      font-family: Arial, "Lucida Grande", Verdana, sans-serif;
      font-size: 27px;
      color: black;
    }

修改之后，用浏览器再来测试一下..

![](http://pic.yupoo.com/reicky_v/DslnIDXb/medium.jpg)

修复了导航的问题，貌似又有其它问题来了，从上面图可以看到导航链接之间没有保持一个适当的间距，看起来就是一大坨，可读性很差。

### 10.把握全局，注意上下文 ###

回到我们的公式**target ÷ context = result**，我们就会发现问题的所在：

      <div id="navigation">
      <ul>
        <li><a href="#">Why?</a></li>
        <li><a href="#">Synopsis</a></li>
        <li><a href="#">Stills/Photos</a></li>
        <li><a href="#">Videos/clips</a></li>
        <li><a href="#">Quotes</a></li>
        <li><a href="#">Quiz</a></li>
      </ul>
    </div>

你可以发现，**&lt;a href="#"&gt;**是包裹在**&lt;li&gt;**里面的。那我们在设置margin的时候，就要考虑这两者之间的关系。而在CSS中我们会发现**li**标签没有设置宽度：

	#navigation ul li { display: inline-block; }

这才是问题原因所在，解决方法也有很多。我们可以给**li**标签设置一个宽度，无论是固定像素还是百分比，但是这样就丧失了灵活性。

我们可以把**li**标签的属性用样式改为内联：

    #navigation ul li {
      display: inline;
    }

这样可以使导航元素水平的排列显示，而是用**inline-block**在IE的早期版本中是有问题的。不过我更喜欢用**inline-block**，这样才能更好地控制元素的各个属性，最终我还是用**inline-block**来(也许可以使用hack来修复在ie6-7中的问题)并且把margin从**a**元素里面移到**li**元素上，css修改如下：

    #navigation ul li {
      display: inline-block;
      margin-right: 2.6595745%; /* 25 divided by 940 */
    }
    #navigation ul li a {
      height: 42px;
      line-height: 42px;
      text-decoration: none;
      text-transform: uppercase;
      font-family: Arial, "Lucida Grande", Verdana, sans-serif;
      font-size: 27px;
      color: black;
    }

再来用浏览器测试一下，就正常了：

![](http://pic.yupoo.com/reicky_v/DslxPqe0/medium.jpg)

但是依然还有些问题，如在浏览器的窗口小于768px的时候，导航还是会折行显示，这个时候媒体查询就该出场了，可以去看看我书里面的第二张关于这个的阐述。在修复导航问题之前，我准备把文字的单位从像素改为**em**。

### 11.使用em来表示字体的大小 ###

在以前，web设计师一般用ems来表示字体的大小，因为当文字的单位是像素的时候当时的浏览器IE不支持文字放大缩小。不过现在的现代浏览器一般都支持文字的放大缩小，无论你是用像素或者是ems来表示字体的大小，那为什么用em来表示字体的大小呢？

em主要是根据元素上下文来确定的。如果在**body**中设置字体的时候用把字体的大小设置为100%，那么body的子元素的字体都会以body为基础来确定大小。这样做的好处是，如果客户需要改变字体的大小，我们只需要在body中设置字体的大小，其它元素的字体就会自动按比例改变其大小。

仍然使用**target ÷ context = result **这个公式来计算，把px改为em。一般现代浏览器会把字体的大小设置为16px，除非在CSS中规定字体大小。因此，我们就以16px为基础来转换字体的单位：

    font-size: 100%;
    font-size: 16px;
    font-size: 1em;

正如上面所示，我们就可以这个为基准来设置字体的大小：

    #logo {
      display: block;
      padding-top: 75px;
      color: #0d0c0c;
      text-transform: uppercase;
      font-family: Arial, "Lucida Grande", Verdana, sans-serif; font-size: 48px;
    }
    #logo span {
      color: #dfdada;
    }

**48/16=3**，所以CSS修改如下：

      #logo {
      display: block;
      padding-top: 75px;
      color: #0d0c0c;
      text-transform: uppercase;
      font-family: Arial, "Lucida Grande", Verdana, sans-serif; font-size: 3em; /* 48 divided by 16 = 3*/
    }

按照这个方法来设置其它元素的字体大小，需要明确一点的是要根据元素之间的关系来去确定具体的数值，比如：

    <h1>Every year <span>when I watch the Oscars I'm annoyed...</span></h1>

CSS修改如下：

     #content h1 {
      font-family: Arial, Helvetica, Verdana, sans-serif; text-transform: uppercase;
      font-size: 4.3125em; } /* 69 divided by 16 */
      #content h1 span {
      display: block;
      line-height: 1.052631579em; /* 40 divided by 38 */ color: #757474;
      font-size: .550724638em; /* 38 divided by 69 */
    }

可以看到我们这里的**span**元素字体的大小是根据它的父元素**h1**的字体的大小来确定的。其它的设置行高也一样。

### 进一步探索 ###

现在，我们整个网站转换为了流体布局能够适应不同大小的屏幕，不过图片没有自适应，这个可以去写的书里找解决方法。

《Responsive Web Design with HTML5 and CSS3》已经出版了，可以去[这里购买](http://www.packtpub.com/responsive-web-design-with-html-5-and-css3/book)。




