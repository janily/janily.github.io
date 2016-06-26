---
layout: post
title: 前端眼中的响应式web开发一些实用的tips
categories:
- Life
tags:
- 响应式
- 前端

---

> 这篇是接上篇的，主要是来聊聊如何利用技术来实现响应式开发。当然主要是指页面UI开放，即如何使用技术手段来使web页面适配不同的设备。

本文主要是结合自己的一些实践来讲讲在开发web中，页面构建中特别是移动端的页面中使用到的一些技术tips。由于平时工作主要是移动端的开发，所以下面的这些tips也主要是针对移动端的。

### meta标签设置

首先是在网页代码头部添加**viewport**meta标签。
```
<meta name="viewport" content="width=device-width, initial-scale=1" />
```

是网页默认的宽度和高度，上面这行代码的意思是，网页宽度默认等于屏幕宽度（width=device-width），原始缩放比例（initial-scale=1）为1.0，即网页初始大小占屏幕面积的100%。


### 媒体查询

所谓媒体查询就是根据不同的屏幕分辨率，选择应用不同的CSS规则。比如：


```
@media screen and (max-width: 320px) {
　　　　.column { 　　　　　　float: none; 　　　　　　width:auto; 　　　　}
　　　　.sidebar { 　　　　　　display:none; 　　　　}
　　}
```

就是说：如果屏幕宽度小于320px，则column块取消浮动（float:none）、宽度自动调节（width:auto），sidebar块不显示（display:none）。

### 单位选择

由于现在移动端设备的屏幕越来越丰富，所以移动端适配也是一个很让人头疼的事情，你不能让一个同样大小的元素在不同的设备上都显示一样大小。比如：iphone6和iphone4下大小肯定是应该不一样的，否则用户体验很差。

比如字体的大小，如果在iPhone5上是14px，那么到了iPhone6 plus等宽屏上，就未免显得有点小；还有按钮，如果使用px写死宽度，那么在大屏手机上体验也不是很好。

这里就涉及到单位的选择了。在移动端，目前好使的单位莫过于**rem**了。

rem是一个半相对单位，它相对的是html(或body)元素的font-size值，例如有html { font-size: 10px; }，则1rem = 10px。

**rem数值计算**

如果利用rem来设置css的值，一般要通过一层计算才行，比如如果要设置一个长宽为100px的div，那么就需要计算出100px对应的rem值是 100 / 16 =6.25rem。

**rem基准值计算**

一般的做法，都是根据我们拿到的视觉高的宽度来设置的，因为我们在写样式的时候必须要先以一个确定的屏幕来作为参考，这个就由我们拿到的视觉稿来定。现在在我所做的项目中一般都是以iPhone6的尺寸的设计稿，即宽度是750的设计稿。

所以，我们可以借助于一点点的js来根据设备不同的宽度来动态设置html的**font-size**的值。这段js也是在网上看到的。


```

!(function(win, doc){
    function setFontSize() {
        // 获取window 宽度
        var winWidth =  window.innerWidth;
       
        // 375宽度以上进行限制 需要css进行配合
        var size = (winWidth / 375) * 100;
        doc.documentElement.style.fontSize = (size < 100 ? size : 100) + 'px' ;
    }
 
    var evt = 'onorientationchange' in win ? 'orientationchange' : 'resize';
    
    var timer = null;
 
    win.addEventListener(evt, function () {
        clearTimeout(timer);
 
        timer = setTimeout(setFontSize, 300);
    }, false);
 
    // 初始化
    setFontSize();
 
}(window, document));


```

建议把这段js放在头部来加载。

计算出基准值后，就可以使用它来设置元素的尺寸、margin或者是padding值了。

比如在设计稿的尺寸是750的，要把一个元素宽为40px转换为rem，改怎么做呢？

根据上面的js计算，我们的rem的基准值是100px，直接使用40除以100就可以了，即0.4rem。

当html元素的font-size是根据设备宽度自适应时，使用rem的页面也就会有自适应的特性。

并且它的支持度非常好，可以放心大胆的使用。

![](http://i.imgur.com/oblImmX.png?1)



#### 字体大小问题

很多的开发者想到的rem应用场景是自然是用来作为字体的单位，但是经过实际项目开发的一些实践，发现如果使用rem来设置字体的大小，误差太大，且不能满足任何屏幕下字体大小相同。

而且经常会出现一些奇怪的问题，所以对于字体还是建议使用媒体查询来设置字体的大小。

#### 布局问题

在rem刚兴起的时候，用的最多的就是布局了。使用它来布局，可以很好的适用不同屏幕。不过随着浏览器对**flex**支持越来越好，现在觉得rem布局已经可以放弃了。flex布局已经很好用了，早已有之的百分比布局等稍用点心思也并不难。

一般在开发中在诸如margin、padding或者是一些按钮的宽高会使用rem。

### 布局

上面说了，现在布局可以放心的使用flex来布局。特别是在移动端，可以完美的解决自适应的问题。

为此整理了一个一些常见的使用flexbox来布局的小的CSS集合。

可以去这里看看[flex常用使用方法](http://janily.github.io/mystique/)


### 图片自适应

#### img自适应

如果项目中使用的图片是**img**标签的话，可以直接在样式中使用下面这句，图片就乖乖的会按比例自动缩放：

```
max-width:100%;
```

#### 背景图片自适应

背景图片的，可以使用**background-size:cover或contain**来达到自适应的目的。





