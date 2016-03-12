---
layout: post
title: 用compass制作雪碧图
categories:
- Life
tags:
- sass
- compass
---

现在CSS预处理器越来流行啦，像LESS、Sass、Stylus这三个应该是最流行的css预处理器啦。

首先来简单介绍下什么是 CSS 预处理器，CSS 预处理器是一种语言用来为 CSS 增加一些编程的的特性，无需考虑浏览器的兼容性问题，例如你可以在 CSS 中使用变量、简单的程序逻辑、函数等等在编程语言中的一些基本技巧，可以让你的 CSS 更加简洁，适应性更强，代码更直观等诸多好处。

而今天我们要说的SASS是一种基于ruby编写的CSS预处理器，配合Compass这个工具使用，写CSS从未如此畅快，谁用谁知道。当然这里不会介绍SASS的基础知识，SASS相关的知识可以去大漠的网站看看，有一系列的非常详细的有关sass的详细教程，地址在[这里](http://www.w3cplus.com/sassguide/syntax.html)。

Compass是一个基于SASS功能强大的CSS工具集，功能很多，今天主要说的是CSS Sprite的功能，有了Compass,做雪碧图再也不是麻烦事了。你的CSS开发效率也会跟着上一个台阶。

当然使用之前，你还得配置下环境，由于SASS是基于ruby的，所以要安装ruby环境，安装完ruby后，还要使用gem安装compass，如下

    gem install compass

这里可能要使用代理来安装了，你懂得的。反正我是用代理才安装成功。

### Sprite基本用法 ###

在compass中，你需要把制作雪碧图的图片放到一个单独的文件夹中，比如icon-text文件夹，然后在SASS文件中写入

    @import "image/icon-text/*.png";
	@include all-icon-text-sprites;

@include all-icon-text-sprites;，这一语句会使compass在合成图片后，为每个图片生成一个.sprites--的CSS类。

就以我最近在一个项目中的图片为例吧，我把需要制作雪碧图的图片放在一个叫icon-text文件夹中，如下图

![](http://pic.yupoo.com/reicky_v/D867099W/medium.jpg)

在SCSS文件中写入如下代码，就可以自动生成雪碧图和为每个图片生成一个.sprites--的CSS类

    @import "icon-text/*.png";
	@include all-icon-text-sprites;

生成最终结果如下图所示

![](http://pic.yupoo.com/reicky_v/D8693CMy/YeLjD.jpg)

如果你想要在自己设置的类中设置background-position的话，需就得把@include all-icon-text-sprites;这句删掉，要在代码中加入下面这句

    @include icon-text-sprite(home);//括号里的名字就是图片的名字

例如

![](http://pic.yupoo.com/reicky_v/D86bcuD1/aDx8E.jpg)

compass就会自动帮我把图片位置定好，在编译后的css文件中是这样的

![](http://pic.yupoo.com/reicky_v/D86bTtuJ/medium.jpg)

有了compass，制作雪碧图轻而易举。当然compass提供了很多的选项，来满足我们制作雪碧图，接着看

### 指定容器的尺寸 ###

有时候，我们的图片可能每一个的尺寸不一样，这时候我们可能就需要给图片指定大小了。

compass也为我们准备了这样的功能，只需要用到 <map>-sprite-height(image-name) 和 <map>-sprite-width(image-name) 这两个方法，我们就可以为的图片的大小。比如

    @import "icon/*.png";
	$box-padding: 5px;
	$height: icon-sprite-height(some_icon);
	$width: icon-sprite-width(some_icon);
	
	.somediv {
	  height:$height + $box-padding;
	  width:$width + $box-padding;
	}

上面的代码中就会输出我们指定图片的大小。

当然，compass还有其它很多的功能，总之，用了它后写css就非常爽了。以后再慢慢介绍。

    

    







    