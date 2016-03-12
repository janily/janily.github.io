---
layout: post
title: 运用Emmet高效编写CSS代码
categories:
- Life
tags:
- css
- 翻译
---

> 相信现在在前端界没有几个人不知道书写CSS和HTML的神器Emmet（即以前的zen coding），自从有了它，写HTML和CSS那叫一个爽，无法用言语表达，谁用谁知道，今天就来介绍用Emmet怎么来高效编写CSS。本翻译自[tutsplus](http://dev.tutsplus.com/)网站的[Turbo-Charge Your CSS With Emmet](http://dev.tutsplus.com/articles/writing-turbo-charged-css-with-emmet--webdesign-16511?utm_source=CSS-Weekly&utm_campaign=Issue-90&utm_medium=email)。

在很多的文章和教程中一般都介绍用Emmet怎么来高效编写HTML代码，至于介绍编写CSS的文章相对来说很少啦。今天就来介绍一下用Emmet来高效编写CSS代码。开始吧！

### 啥是Emmet ###

Emmet是一个集合了html/xml/css代码的扩展，现在正在考虑在未来的版本中添加代码片段的特性，可以去这个[地址](http://emmet.io/download/)下载对应编辑器的Emmet的插件并安装好它。

使用Emmet时，只要输入对应的缩写然后触发快捷键就可以了。在Sublime Text编辑器中快捷键是**TAB**，它会根据你的文件的类型来使用相对应的代码缩写。

### 为啥要使用Emmet来编写代码 ###

Emmet使用了一些富有语义的缩写来简化代码的编写。根据你的熟练程度，能显著提高你编写代码的速度和效率。更重要的是使编写代码也能变得非常优雅。愉悦了开发者，此处省略一百字....。

> Emmet即以前的Zen coding是一个web开发的得力工具，它能显著提高你编写HTML和CSS的效率。

它不仅节省你写代码的时间，也能使编写代码变得非常有趣。仅仅是输入几个简单的字符，它就能自动填充符合标准格式的代码。

在使用Emmet编写代码的过程中，还能够使我更好地记住代码。缩略词更容易记忆，比如**text-transform:**这个只要编写**tt**就够了；而**text-align:justify;**，只需要输入**taj**就行了。是不是像变魔法一样。在Emmet中，你只需要记住相关属性的缩略语就够了。至于符号和空格就更不用担心了，Emmet会帮你自动生成符号和空格。

### Emmet和CSS ###

首先我们来看看一些基本的CSS缩写词，来看看它是怎么来提高我们编写代码的效率的。

#### 属性 ####

CSS是有属性名和值构成的，比如font-size，margin，padding等等。

![](http://pic.yupoo.com/reicky_v/DoZWomDc/medium.jpg)

Emmet把CSS中属性全部定义成了一些缩略词。所以**border-bottom**使用**bdb**表示的，**border-top**则用**bdt**来表示的。比如上图中的**font-size**使用**fz**来表示的。

输入缩略词后，只要触发快捷键(我定义的快捷键是TAB)，然后Emmet就会输出对应CSS属性的完整值，如下图所示：

![](http://cdn.tutsplus.com/webdesign/uploads/2013/10/emmet-css-property.gif)

#### 值 ####

现在我们明白了在Emmet中怎么用缩略词来编写CSS的属性了，输入属性后自然是输入属性的值了。我们也可以用缩写的形式来输入属性值。比如**fz18**，会输出**font-size：18px**完整的CSS代码。你不需要输入"px"因为Emmet会自动输入。如果一些属性没有单位的比如**font-weight**，Emmet也会自动识别不会添加单位。

![](http://cdn.tutsplus.com/webdesign/uploads/2013/10/emmet-css-value.gif)

#### 单位 ####

在CSS中不只有px这一种单位。Emmet也支持像**em**,**rem**,**%**,和**px**这样的单位。每一种单位在Emmet中都有对应的缩略词：

- px 是默认的单位
- p 表示 %
- e 表示 em
- r 表示 rem
- x 表示 ex

要使用单位，只需要在你的CSS值的后面加上对应单位的缩略词就够了。如下所示：

![](http://cdn.tutsplus.com/webdesign/uploads/2013/10/emmet-css-unit.gif)

#### 复合单位 ####

一些属性，比如**margin**，允许编写多个带单位的值。在Emmet中，只需要用横杠**-**表示就可以了，看下面这个示例就知道具体的意思了。

![](http://cdn.tutsplus.com/webdesign/uploads/2013/10/emmet-css-multiple-units.gif)

#### 颜色 ####

Emmet中，颜色的属性的值是以**#**为开头的。不同的数字组合会输出不同的颜色代码，我们来看看一些例子：

- 1 表示 111111
- e0 表示 e0e0e0
- fc0 表示 ffcc00

我们来看看下面这个实例来演示一下：

![](http://cdn.tutsplus.com/webdesign/uploads/2013/10/emmet-css-colors.gif)

#### ！important ####

**important**这个属性值也用的非常频繁，Emmet使用**!**这个符号来代替：

![](http://cdn.tutsplus.com/webdesign/uploads/2013/10/emmet-css-important.gif)

#### 复合属性 ####

到现在为之，我们应该明白了Emmet一些基本的使用方法，现在来点更深入的东东。就像用它来编写HTML代码一样，我们也可以使用**+**这个符号。

我们可以用**+**符号来编写多个CSS属性和值的缩略词，然后触发快捷键，接下来就是见证神奇的时候。

如下所示：

![](http://cdn.tutsplus.com/webdesign/uploads/2013/10/emmet-css-multiple-properties.gif)

最后我们看一个结合我们上面介绍的知识的一个CSS代码编写实例。当然有时候可能会不是很准确，不过它确实能提高我们编写代码的效率。这里把我的编写过程制作了一个gif格式的图片，就像变魔法一样神奇，不信往下看：

![](http://cdn.tutsplus.com/webdesign/uploads/2013/10/emmet-css-example.gif)

### 总结 ###

Emmet是一个非常强大的工具。是一个高效输入代码的工具，可以达到事半功倍的效果。并且支持大部分的代码编辑器。

当然，任何一个新的实物都有一定的学习成本和学习曲线，但是Emmet并不需要你花很多的时间就能使用它，何乐而不为呢？你可以定期的的去Emmet官方文档看看新增的特性，Emmet也制作了一个快速入门的[快速指南](http://docs.emmet.io/cheat-sheet/)。“Write Less，do more”。

一些有用资源链接：

- [Emmet CSS Documentation](http://docs.emmet.io/css-abbreviations/)
- [Emmet Cheat Sheet](http://docs.emmet.io/cheat-sheet/)
- [Build Bootstrap in Minutes Using Emmet](http://webdesign.tutsplus.com/tutorials/applications/build-bootstrap-in-minutes-using-emmet/)
- [Smashing Magazine Emmet Article](http://coding.smashingmagazine.com/2013/03/26/goodbye-zen-coding-hello-emmet/)

