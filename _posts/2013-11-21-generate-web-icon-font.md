---
layout: post
title: 用webfonts制作属于自己的icon
categories:
- Life
tags:
- css
- 翻译
---

> 上一篇文章讲了关于用webfonts打造更好地用户界面以及使用方法。这篇文章我们来讲讲关于怎么样用工具和webfonts来制作我们的icon。这篇文章是来于[webdesignerdepot](http://www.webdesignerdepot.com/2012/01/how-to-make-your-own-icon-webfont/)，具体翻译有删减。

在本文中，我将结合自己以前在UI icons的设计方面的经验来说说怎么打造自己的字体图标。

从设计一个图标到用**@font-face**嵌入字体图标，以及用什么授权协议分发字体图标，我们都用开源的软件和一些在线服务来制作我们的字体图标。这不需要很多高深的知识，只是需要一点点的设计品位就可以做到。

当然这个过程远远超过了仅仅是设计一个字体图标这么简单。

在开始设计一个图标之前，我们首先要弄明白我们所要用到的图标是要达到一个什么目的。这个也值得探讨一下，我们就来探讨一下，这个之前，我们也得明白字体图标也是属于字体符号的一个范畴。

### 怎么来制作一个好的图标 ###

从广泛的意义上来说，我们学习字体符号学，有助于我们了解字体符号学具体构成一些细节。

当你考虑在你的设计中运用一些特定符号的时候，这个符号就要对你的用户很直观的传达你这个符号所表达的意义和概念，因为符号并不能像语言一样具体的能传达给用户具体的信息，而符号仅仅是依靠一些颜色、图形形状来传达信息给用户-这个我们就称为图标。不过在图标符号中我们也要考虑到不同国家不同文化的一些差异，[在中国](http://www.labbrand.com/brand-source/semiotics-vs-semiology)，红色一般象征着吉祥，而在西方红色则象征着危险。这些我们在设置图标的时候要考虑到。

![](http://pic.yupoo.com/reicky_v/DkphMkuq/medium.jpg)

**图标**并不是简简单单的表示一个图片而已。比如地图上用到的地图钉，我们在设计的时候它的形状应该跟真实存在的地图钉存在联系，反过来，我们看到这个图标的时候，应该能跟现实生活中的图标对应起来，给用户传达一个概念，这就是一个地图钉。设计其它图标的时候，也要秉持这个原则。

现在很多的一些所谓的图标并不是真正意义上的图标。比如无处不在的RSS订阅图标，它由一个圆点和两个同心圆的一段组成，不过RSS指使存在于虚拟的互联网的世界中，在现实世界中并没有真正对应实物存在，所以RSS图标更多的意义是一个抽象的表示，不过现在经过这么多年的培养，RSS图标的这种表示已经成为一个行业的标准了，也无可厚非。

![](http://pic.yupoo.com/reicky_v/Dkpqsdu4/medium.jpg)

那我们现在去设计一个图标的时候，应当遵循下面的两个标准：

- 首先要从现实存在的找到相对应的实物来设计，比如一个打印机就要对应现实生活中真实的打印机。
- 建立一个可辨识的符号系统

### 字体图标-设计的潮流 ###

现在字体图标越来越流行了，特别是对于提高UI健壮性更是一把利刃。使用字体图标不仅能使你的UI设计的简洁美观，也能增加你web的点击率。

跟用图片表示图标而言，字体图标是一种全新的设计方式。更重要的是相比图片而言，字体图标有着无可比拟的优势。（这个可以去上篇文章看一下译者注），也可以去看看我这里对于使用字体图标的一些优势的阐述的[这篇文章](http://www.heydonworks.com/using-icon-web-fonts)。这里我总结一下具体的一些优势：

- 字体图标是矢量文件，可以随意改变大小，而不影响图标质量
- 可以用CSS样式来自由定义图标颜色
- 所有的字体图标全部在一个文件中，可以节省HTTP请求
- 正如Chris指出的那样，浏览器对于字体图标的支持度非常好，IE6都支持
- 使用字体图标用样式非常容易控制诸如对齐方面的问题，因为字体图标本来就是文字
- 你还可以使用CSS3的一些新的属性来定义它的效果，如**text-shadow**和**background-clip**等
- 跨浏览器支持非常容易实现

### 存在的问题 ###

在Chris看来，使用字体图标是一个非常好的开发方法，不过现在字体图标的应用还不是很广泛。首先大部分的质量不错的字体都市妖付费的，比如[Pictos](http://pictos.drewwilson.com/)，[Fico](http://fico.lensco.be/)，Klepto，Cheetos，Ponyo和[Sailor Moon](http://keyamoon.com/icomoon/)，这些字体都是要付费的。总而言之就是有两个主要的问题：

- 你需要用钱取买字体图标
- 不过你用钱去买，那你不得不接受别人的设计来影响你的设计

![](http://pic.yupoo.com/reicky_v/DkpCJEG8/medium.jpg)

我假设在读我这篇文章的读者大部分都是web设计师，当然除了网络爬虫。我相信大部分的web设计师都不想在别人的设计的影响下来设计自己的项目。作为设计师，我们知道自己想要的图标是什么样子的，受制于人谁也不愿意。

经过一段时间的摸索，我从这个[视频](http://www.youtube.com/watch?v=_KX-e6sijGE)里面学习到了[Inkscape](http://inkscape.org/)这个SVG编辑器的使用方法。经过一些联系我能够使用Inkscape来制作TTF格式的字体，你可以去这个[地址](http://www.delicious.com/stacks/view/SC3hpq)看看。这个字体是免费使用的。

![](http://pic.yupoo.com/reicky_v/DkpGd4Of/medium.jpg)

### 使用Inkscape来制作字体图标 ###

#### 设置Inkscape ####

首先你得下载安装这个软件。你也可以使用我提供这个模板，可以从github上下载，[下载地址](https://github.com/Heydon/Community-Icon-Font/blob/master/resources/inkscape_iconfont_canvas_template.svg)。你可以在Inkscape打开这个文件，打开下面的菜单来设置：

- OBJECT → FILL AND STROKE
- OBJECT → ALIGN AND DISTRIBUTE
- TEXT → SVG FONT EDITOR

在SVG Font Editor面板中，在**Font**的下面点击**Font 1**，如下图所示：

![](http://pic.yupoo.com/reicky_v/DkpJIpRs/medium.jpg)

可以看到，文字的基线在画布的底部偏下一点点，当然这是不好的，如果你想跟别人分享你设计的字体文件，你应该设置好的文字的基线对齐画布的底部。

#### 制作第一款字体图标 ####

点击菜单栏字体选项里svg字体编辑器，然后点击添加字型按钮添加一个字体，我们首先来创建一个五角星的字体图标，我们取名为**s**：

![](http://pic.yupoo.com/reicky_v/Dkq1ywHT/medium.jpg)

定义好字体形状后，我们就可以开始制作字体本身所代表的的形状了，因为只是一个很简单的五角星，我们可以用Inkscape中提供的形状工具来画我们的五角星，这里我把圆角设为0.2来画一个五角星，如下图所示：

![](http://pic.yupoo.com/reicky_v/Dkq4WeUk/medium.jpg)

这样我们就为我们的字体定义了一个形状，没有颜色，图层和渐变。现在我们必须把我们的**图形从对象转化为路径**，可以在菜单里选择路径选项，然后点击从对象转化为路径。现在我们选中五角星，然后回到SVG字体编辑面板，选中我们先前定义好字体**s**然后点击从选区中获得曲线按钮：

![](http://pic.yupoo.com/reicky_v/Dkq7ywo5/medium.jpg)

然后你在实例文本框中输入我们定义好的**s**字体名称，我们可以在预览中看到我们定义好的五角星形状，如图：

![](http://pic.yupoo.com/reicky_v/Dkq8VKIL/medium.jpg)

#### 制作一些复杂的字体图标 ####

就这么简单，我们制作好了一个比较简单的字体图标。如果使用填充功能，编辑路径节点我们可以制作出一些更加令人惊艳的图标出来。不过这里并不打算成为一个Inkscape全面的教程，我们还有其它的事情要做，下面就说下几个inkscape设计图标的小技巧：

- 用黑色填充图标，你只需要记住一点的是我们只是需要具体的形状，至于颜色可以在CSS里自由控制
- 我们制作所有的图标必须转换成路径，对象转换成路径或者是轮廓转换成路径
- 如果我们的图标由多个形状组成，那么必须在路径菜单的选项里合并路径
- 当你的图标有多个图像组成的时候，你可以试试路径选项里**差集、并集、交集**这些功能，可以大大提高你工作效率

#### **准备开始在你网页中使用字体** ####

我们制作好图标字体文件后，接下来肯定是想马上就能在网页上用了。

#### 把SVG转换成TTF字体格式 ####

我们首先要做的是把SVG格式文件转换成通用的字体格式文件。TTF格式字体是一个优秀的字体格式。并且**@font-face**属性对它的支持也非常好。现在有很多线上的服务提供字体格式的转换服务，包括，[http://onlinefontconverter.com](http://onlinefontconverter.com/)、[http://www.fontconverter.org](http://www.fontconverter.org/)和[http://www,font2web.com](http://www.font2web.com/)。我个人比较喜欢[http://www.freefontconverter.com](http://www.freefontconverter.com/)这个网站提供的字体格式转换服务，因为它非常稳定。

![](http://pic.yupoo.com/reicky_v/DkqI6xBh/medium.jpg)

使用非常简单，上传文件然后点击转换按钮就行了。

#### 编辑字体的信息 ####

转换完后就会自动下载好相关的字体文件，这里我建议你编辑一下字体相关的信息，重命名和描述以及一些安装和分发的协议。也是保护自己的知识产权的一种方法。Windows操作系统可以使用微软的[字体编辑工具](http://www.microsoft.com/typography/property/fpedit.htm)或者是[Typograf](http://www.neuber.com/typograph/)这个工具来编辑字体的信息。

![](http://pic.yupoo.com/reicky_v/DkqLcmtq/medium.jpg)

这里注意一点的是，虽然微软提供的工具允许你添加字体作者，缪奥数和授权协议等信息，但是它不允许你编辑一些基础信息比如字体的名称等信息。如果你想编辑这些信息，你只能在转换格式前，用编辑器把SVG文件打开来编辑相关信息，我一般使用notepad++这个编辑器来编辑：

- Font Name：就在*font-family*这个属性里编辑
- Postscript Name：在*id*属性里编辑
- description 描述你可以在author，license这些属性里编辑。需要注意的一点的是这些不会在转换的时候保存，你必须单独添加这些信息

#### 嵌入字体 ####

![](http://pic.yupoo.com/reicky_v/DkqPLqNj/medium.jpg)

当你下载好转换好的字体后，我们可以把它安装在我们的电脑上，确保它没有出错，确认后，我们使用[@font-face generator](http://www.fontsquirrel.com/fontface/generator)这个工具来生成我们的字体图标文件。不过在选择输出文件的时候有几个选项我们要注意一下：

- Subsetting：这个选项是允许你选择生成文件仅用你声明的字符来生成文件，这样可以减少生成文件的体积。
- Removing kerning : 你生成的图标字体文件中总会有间距的存在，所以间距的存在是不必要的，删除字符间距也可以减少文件体积。
- WebOnly：如果你不想你的字体图标只能在web上使用，你就可以选择这个选项。

#### 分发字体 ####

如果你准备跟别人分享你的字体，选择一个授权协议是有必要的。不过我们使用的都是开源工具来制作我们的字体文件，那选择开源给他人使用也是理所应当的。

这里有一些授权协议你可以去了解一下。[GNU General Public License](http://www.opensource.org/licenses/gpl-3.0.html)这个协议不错，不过你可能要考虑下使用[SIL Open FONT license](http://scripts.sil.org/cms/scripts/page.php?site_id=nrsi&id=OFL#f28128b3)。这个协议的一个优点是允许保留字体名称，这就意味着其他设计师在使用你的字体的时候必须的保留你的名称。

在一个完整的授权协议里，你必须包含一个有关授权协议的文件以及在字体的信息里包含协议的链接，下面这张图就是一个完整的包含授权协议的字体文件。

![](http://pic.yupoo.com/reicky_v/DkqYLETH/medium.jpg)

#### CSS雪碧图 ####

为什么我们要大力的推行使用SVG格式的字体图标呢？我们可以使用SVG来制作更多的的图标元素。现在很流行用**CSS sprites**这种技术来管理组织网页上的图片文件，因为它把所有的图标都放在一个文件中，这样可以大大的减少HTTP请求。主要使用了图片定位的技术来给元素指定背景图片。这使得它也很适合用在[响应式设计中](http://coding.smashingmagazine.com/2011/01/12/guidelines-for-responsive-web-design/)。

在我的博客中我就大量的使用了SVG来制作字体图标。如下图所示：

![](http://pic.yupoo.com/reicky_v/Dkr2zv7U/medium.jpg)

使用字体图标有很多的有点，比如你可以使用CSS3来制作很多的效果，可以去这个网站看看[收集了很多用CSS3和字体图标制作优雅的效果](http://www.webfontsgallery.com/)

#### 对于屏幕阅读器需要注意的地方 ####

使用字体图标有一个问题是对于屏幕阅读器有一点影响。正常的访问网站，访问者很容易就知道图标所表达的意思，但是对于屏幕阅读器来说，屏幕阅读器并不会明白这些元素所表示的意思。对于使用字体图标，我建议考虑下Coyier的建议：[Supplementary Private Use Area](http://en.wikipedia.org/wiki/Mapping_of_Unicode_characters#Private_use_characters)用字符集的方法来使用字体图标。

### 协同合作制作字体图标 ###

我的javascript指导老师，[Rupert](http://twitter.com/#!/rupertredington)像我提出，采用社区合作的方式使用SVG来制作字体图标是一个非常有趣的事情。想想看，SVG是一种XML格式的代码文件，可读性非常高。我们可以使用Github社区来作为开发模式。

我非常认同这个想法：能够在字体图标制作上达成共识，集合社区的力量把常见的一些字体图标，制作一套关于字符图标的字符系统，这样对社区也有积极的推动意义。

![](http://pic.yupoo.com/reicky_v/Dkrfcz4l/medium.jpg)

基于这个想法，我们创建了一个github的项目，名叫[Community Icon Font](http://heydon.github.com/Community-Icon-Font/)，这个不是很复杂，这个项目有一个Inkscape的入门教程。如果你初次接触Github，你可以去它们的[帮助页面](http://help.github.com/)看看。

