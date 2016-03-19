---
layout: post
title: 使用最新的CSS属性打造响应式图片布局
categories:
- Life
tags:
- CSS
- responsive

---

> 本文来自[sitepoint](http://www.sitepoint.com/)的一篇文章[Using Modern CSS to Build a Responsive Image Grid](http://www.sitepoint.com/using-modern-css-to-build-a-responsive-image-grid),非常不错，能在很多的web开发场景中使用。

在现在的web开发中，响应式的布局总是给人非常优雅的使用体验。本文就小试牛刀的用一个简单的实例来展现响应式布局的魅力。

### 响应式布局

开始之前，让我们先假设一个在宽屏上的一个图片布局情形：

![](http://ww4.sinaimg.cn/large/0060lm7Tgw1f22p7cqzwvj30m80bdadd.jpg)

在小屏设备上，是这样子的：

![](http://ww4.sinaimg.cn/large/0060lm7Tgw1f22p7dmebmj30m80aztcv.jpg)

布局非常简单，代码如下：

	<div>
	  <a href="path-to-the-image">
	  </a>
	  <!-- other anchors here ... -->
	</div>

我们将会使用两种不同的方法来实现上面的布局，并比较它们各自不同优缺点。在开始之前，简单来描述下这个布局的两个注意的地方：

1、在小屏设备是是两列布局，在桌面设备上是四列布局。

2、每个图片之间的间隙是8px。

### 使用inline-block方法

首先使用**display:inline-block**方法来实现布局，CSS代码如下：


```
div {
  font-size: 0;
}

a {
  font-size: 16px; 
  display: inline-block;
  margin-bottom: 8px;
  width: calc(50% - 4px);
  margin-right: 8px;
}

a:nth-of-type(2n) {
  margin-right: 0;
}

@media screen and (min-width: 50em) {
  a {
    width: calc(25% - 6px);
  }

  a:nth-of-type(2n) {
    margin-right: 8px;
  }

  a:nth-of-type(4n) {
    margin-right: 0;
  }
}
```

下面来解释下代码：

众所周知，在使用**inline-block**的时候，元素与元素之间会产生[空隙](https://css-tricks.com/fighting-the-space-between-inline-block-elements/)。修复方法也多种多样，我们这里使用的方法是，设置父元素的字体的大小为0。当然，这里就需要我们重新设置子元素的字号了(不过在这个实例中，不需要这样做)。

在小屏设备上，仍然要保持8px的间距。

这里我们使用了CSS中的**calc**来设置元素的宽度：

1、每一列之间的间距总共是24px，不是32px。因为我把第四个元素的margin-right的值去掉了。

2、每一列的宽度，使用了**calc**属性来计算，即**calc(25% - 6px)**。6px的值是怎么来的呢？很简单：把间距平均分配给每一列即(24px/4 => 6)。

![](http://ww3.sinaimg.cn/large/0060lm7Tgw1f22p7ev0qhj30m80axn1f.jpg)

demo[地址](http://codepen.io/SitePoint/pen/XXEmXj)

### 使用Flexbox

上面的解决方法也不赖，但是有一些缺陷。让我们假设这样一种情形，每一列中同时包含图片和文字说明。

修改一下html结构：

	<div>
	  <a href="path-to-the-image">
	    <figure>
	      ...
	      <figcaption>Some text here</figcaption>
	    </figure>
	  </a>
	  <!-- other anchors here ... -->
	</div>

就会变成如下图所示的情形：

![](http://ww1.sinaimg.cn/large/0060lm7Tgw1f22p7edo0kj30m80bagov.jpg
)

[demo](http://codepen.io/SitePoint/pen/yeKYJN)

这样的展现可不是我们想要的，因为每一列都不一样高。而使用**Flexbox**的布局方法就不会有这个问题，使用它可以解决很多以前经常碰到的布局问题(比如，inline-block的间隙问题)。只要稍微改造下CSS代码，我们就可以使用Flexbox来打造一个完美的布局：


```
div {
  display: flex;
  flex-wrap: wrap;
}
```

搞定，再此刷新demo，这才是我们想要的。完美适配所有的屏幕：

![](http://ww2.sinaimg.cn/large/0060lm7Tgw1f22p7fb8g3j30g80gmdj0.jpg)

[demo](http://codepen.io/SitePoint/pen/bEvVqP)

这里还要说明一点的是。Flexbox可以使用**justify-content**属性来规定flex元素的对齐方式。比如**space-between**或者是**space-around**：

**space-between**两端对齐：

![](http://ww2.sinaimg.cn/large/0060lm7Tgw1f22p7g3607j30m80bh41g.jpg)

**space-around**元素间隔相等：

![](http://ww2.sinaimg.cn/large/0060lm7Tgw1f22p7hlrocj30m80bm773.jpg)

修改代码如下：


```
div {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between; /* or space-around */
} 

a {
  display: inline-block;
  margin-bottom: 8px;
  width: calc(50% - 4px);
}

@media screen and (min-width: 50em) {
  a {
    width: calc(25% - 6px);
  }
}
```

在这里，我们没有使用**margin-right**来指定元素的右边距。因为我们使用了**justify-content**属性，它会自动来分配元素之间的间距。

[demo](http://codepen.io/SitePoint/pen/RrMWjq)

### 总结

通过本文，我们使用两种方法来实现响应式布局。当然，inline-block方法也是非常有用的；不过使用Flexbox可能更加容易一点，特别是涉及到宽度计算的时候更是如此。





