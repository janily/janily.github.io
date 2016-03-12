---
layout: post
title: 图片优化高级技巧
categories:
- Life
tags:
- 前端
- 翻译
---

> 图片是影响web性能的一个重要因素，所以现在在web开发中图片优化技巧也是百花齐放。压缩图片就是常用的一招，不过有时候如果我们花一点点时间在从psd输出图片的时候就优化它们的体积，那么后面压缩就可以进一步提高图片的压缩率。这篇文章就来介绍几种优化图片体积的几个高级技巧，原文地址是[Advanced Image Optimization Tricks](http://sixrevisions.com/web-development/advanced-image-optimization/)。

### 1.高斯模糊优化JPEG图片体积 ###

高斯模糊能够对图片的一些细节进行柔化处理。在图片处理中，高斯模糊通常用于提高其质量或者是打造一个特别的视觉效果。

不过，你仅仅对图片进行一个很小的高斯模糊处理，它不会对图片质量有什么大的影响但可以降低图片的体积。我们来一个例子。

![](http://pic.yupoo.com/reicky_v/DpTnydv9/medium.jpg)

这张图片的体积是60.9kb。

然后我们用photoshop打开这张图片，对这张图片应用滤镜中的高斯模糊，如下图所示：

![](http://pic.yupoo.com/reicky_v/DpTpPDdt/medium.jpg)

我们可以调节模糊的半径来调整效果，调整到可以接受的范围就可以啦，如下图：

![](http://pic.yupoo.com/reicky_v/DpTsuNtM/medium.jpg)

应用之后我们在保存图片，优化后的图片的体积是58.7KB-比原始图片体积降低了3.6%。(译者注:我用上面的图片做了个实验确实可以减少图片的体积。)

### 2.色调分离法(Image Posterization) ###

色调分离允许我们降低图片的体积而不会对图片的质量造成很大的损害。[色调分离](http://en.wikipedia.org/wiki/Posterization)是指色调分离是指一幅图像原本是由紧紧相邻的渐变色阶构成，被数种突然的颜色转变所代替。这个确实有点专业这里是中文维基百科关于[色调分离](http://zh.wikipedia.org/zh-cn/%E8%89%B2%E8%B0%83%E5%88%86%E7%A6%BB)的解释，可以去看看。

还是来一个实例吧。

在这个例子中，我们使用下面这张PNG图片，[地址](http://sixrevisions.com/freebies/icons/wooden-social-free-icon-set/)。

![](http://pic.yupoo.com/reicky_v/DpTCXlA4/medium.jpg)

用photoshop打开它然后进行色调分离。

![](http://pic.yupoo.com/reicky_v/DpU1jqI8/medium.jpg)

在photoshop的菜单栏中，代开图像选项中的调整中的色调分离选项。会弹出一个调整色阶的对话框然后调整其数值。调整图片质量到可以接受的范围。

![](http://pic.yupoo.com/reicky_v/DpU4aQFf/medium.jpg)

在这个例子中调整到76，再低就会影响到图片质量。调整好后保存图片，下面就是保存后的图片:

![](http://pic.yupoo.com/reicky_v/DpU44eoN/medium.jpg)

保存后图片体积降低了30%以上。

#### 推荐阅读一些关于图片优化的文章 ####

- [Clever PNG Optimization Techniques](http://www.smashingmagazine.com/2009/07/15/clever-png-optimization-techniques/)这篇文章详细介绍了关于色调分离的方法来优化图片。
- [Most Effective Method to Reduce and Optimize PNG Images](http://www.queness.com/post/2507/most-effective-method-to-reduce-and-optimize-png-images)这篇文章详细的教你一步一步用photoshop中色调分离来优化图片。
- [Image Posterization ](http://www.cambridgeincolour.com/tutorials/posterization.htm)这篇文章也是讲关于色调分离原理的。

### Pixel-fitting(这个用中文真不知道咋翻译) ###

[Pixel-fitting](http://dcurt.is/pixel-fitting)技术对于确保高质量的把矢量图形准换成光栅图形来说非常重要。

对于像logo或者是一些小的图标这种不是经过摄像来创作的图片，最好是用矢量图形的格式来设计，这样就能适应不同分辨率的设备以及任意改变其尺寸的大小而不失真。

不过，当把矢量图形转换成一些位图格式的时候比如**JPEG**和**PNG**。当用像photoshop之类的图片处理工具来转换的时候，它在处理图片边缘的时候会重叠像素，这样就会造成图片边缘模糊的情况。

如下图所示在转换后的图片边缘是有多余的像素存在的，而用了Pixel-fitting技术的图片则不会有这个问题。

![](http://pic.yupoo.com/reicky_v/DpUoLoNK/medium.jpg)

那Pixel-fitting是啥呢？其实也很简单，用图片编辑器photoshop尽量的放大矢量图片能够清晰的看到图片的边缘，然后把你想要把矢量图片保存为位图图片的区域用钢笔工具连接起来然后再保存为位图格式，过程如下所示：

![](http://pic.yupoo.com/reicky_v/DpUupXfM/medium.jpg)

看这张图你就明白Pixel-fitting是啥东东了。不过Pixel-fitting只能是针对规则的形状的图形，如果是曲线图形那就不能用这种方法了。

推荐阅读

- [Pixel-fitting](http://dcurt.is/pixel-fitting)是关于Pixel-fitting一个详细的讨论。
- [Pixel Hinting Vectors in Photoshop ](http://methodandcraft.com/videos/pixel-hinting-vectors-in-photoshop)是一个Pixel-fitting技术的视频教程。
- [How to Achieve Pixel Perfection in Photoshop ](http://blog.teamtreehouse.com/quick-tip-how-to-achieve-pixel-perfection-in-photoshop)也是一个Pixel-fitting技术的视频教程。

### 8-pixel网格优化JPEG图片 ###

这个技巧我也是从 Smashing Magazine的一篇文章[Clever JPEG Optimization Techniques](http://www.smashingmagazine.com/2009/07/01/clever-jpeg-optimization-techniques/)看到的，这篇文章里同样也介绍了一些优化JPEG图片的技巧。

看下面这一张图片：

![](http://pic.yupoo.com/reicky_v/DpUCsjkL/medium.jpg)

这是一个黑白格的图片，在保存为JPEG的时候选择质量很低的话，就会出现模糊的锯齿。这种锯齿我们经常见到，但神奇的是右下角的那个正方形的边缘非常完美，这是因为JPEG储存图片的时候会把图片存为一系列8×8像素的块。

所以优化技巧就是，如果图片包含轮廓清晰的元素，那么就把元素挪到8×8的参考线上。

具体在photoshop里操作是，在PS里设置网格的尺寸的方法是编辑>首选项>参考线、网格和切片>网格>网格线间隔设置为8的倍数比如32，然后子网格设置为4（32/8=4）即可。显示网格的方法是视图>显示>网格。就可以用8-pixel的方法来优化图片了。

推荐阅读

- [JPEG optimization. Part 1 ](http://www.artlebedev.com/tools/technogrette/etc/jpeg-1/)这是一个8-pixel优化图片的一个教程。

### 选择性压缩 ###

一般我们压缩JPEG图片的时候，是压缩整个图片。

而选择性压缩，则是指针对图片不同区域来压缩图片。比如，针对一张图片比较重要的区域我们可能想采取较低的压缩率来压缩图片，而图片的其它区域比如图片的背景或者是一些没多少细节的区域，我们会采取比较高的压缩率来压缩它。

来看一个实例，选择性压缩可能要用到fireworks这个软件。下面这张图片是质量为80的JPEG格式图片，体积是54.0KB。

![](http://pic.yupoo.com/reicky_v/DpUHkh2M/medium.jpg)

看上面这张图片，我们把蓝色的背景和周围的黑线采用的高的压缩比例来压缩，而中间的部分因为有很多的细节可以把压缩的比例调低一点。

在fireworks中，我们可以把我们需要保护的区域用蒙版蒙起来。蒙版区域我们采用80的压缩比例来压缩，而其它区域则用60的压缩比例来压缩。

我们可以使用套索工具(我一般使用路径工具)来选择我们想要保护的区域。如下图所示：

![](http://pic.yupoo.com/reicky_v/DpZMOV7M/medium.jpg)

选择好区域后，我们在fireworks的菜单栏的修改命令里选择将所选保存为JPEG蒙版的命令，如下图所示：

![](http://pic.yupoo.com/reicky_v/DpZOM2dl/medium.jpg)

选择好后，我们选择保护的选取就会有一个蒙版的效果，如图所示：

![](http://pic.yupoo.com/reicky_v/DpZPG37A/medium.jpg)

然后在优化面板，有品质和选择性品质两个选项。我们可以把品质选项的值调整为60，而选择性品质的值调整为80。(如果你在firework中没看到优化面板，你可以在菜单栏的的窗口命令里选择优化面板它就会出现了)

![](http://pic.yupoo.com/reicky_v/DpZS2wkY/medium.jpg)

然后选择文件保存为JPEG格式的命令，保存完后你会发现压缩后的体积比原始图降低了很多，而且图片细节的地方跟原始图片几乎分辨不出来有啥分别。

选择性压缩JPEG图片是一项非常耗时的事情。如果你有很多的图片要处理就不是很适合。如果，你真的非常在意在保持质量的前提下优化图片，那选择性压缩JPEG图片也是一个不错的方法。

推荐阅读:

- [Selectively compressing areas of an image ](http://www.adobe.com/support/fireworks/workpixels/selective_jpeg/selective_jpeg02.html)，这篇文章是一片关于用fireworks优化图片的教程，是adobe官方出的比较老了。

这篇文章介绍了一些图片优化的方法。其实在开发中，我们一般用photoshop提供的保存为web格式的图片的命令，也能很好的自动帮我们压缩图片。

但是，如果你想更多的控制图片优化的一些细节和体积的话，你可以尝试使用本文介绍的几个技巧。当然这些工作是要花费一些时间的，所以在实际开发中，我们一般用[ Kraken.io](https://kraken.io/web-interface)和[ Smush.it](http://www.smushit.com/ysmush.it/)这类的自动化的工具来压缩我们的图片，这类自动化工具能够在保持图片质量前提下尽可能的压缩图片的体积。不过有时候上面这些方法还是有用武之地的。


