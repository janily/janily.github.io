---
layout: post
title: CSS的秘密花园 ——— 读《CSS SECRETS》
categories:
- Life
tags:
- 读书
- CSS3
- 前端

---

> 前段时间终于读完了《CSS SECRETS》这本书，这本书时在kindle上读完的，可以去[图灵](http://www.ituring.com.cn/book/1695)购买电子版。书的内容不错，实战性非常强。囊括了很多实战技巧，并且应用场景也非常广。看完后，有一种阔然开朗的感觉。作者也是大名鼎鼎的[Lea Verou](http://lea.verou.me/)，相信关注前端特别是CSS这块的，应该都看过她写的文章，总之这本书值得一看。下面是看书过程中一些摘录，都是一些开发中使用技巧。

![](http://ww3.sinaimg.cn/large/0060lm7Tgw1f3o8ipwhs9j30be0dwwf8.jpg)

###四边形

现在在网页开发中，像多边形的设计元素运用的越来越多，比如平行四边形或者是菱形等等。在CSS3以前，一般都会直接使用图片代替，这样大大的降低了灵活性，而且开发成本也很高。

![](http://ww1.sinaimg.cn/large/0060lm7Tgw1f3o8z4eonuj30go0fsad0.jpg)

CSS3推出以后，实现这样的效果就很简单了，只要使用CSS3种的**transform**中的**skew**方法就可以轻松实现。当然在实际应用过程中还是有些问题的，如果是直接在摇变形的元素上使用**skew**属性的话，会遇到下面的问题：

<p data-height="300" data-theme-id="17491" data-slug-hash="NNEwYO" data-default-tab="css,result" data-user="janily" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/janily/pen/NNEwYO/">NNEwYO</a> by janily (<a href="http://codepen.io/janily">@janily</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

会发现，内容一起变形了，这肯定不是我们期望的。那有没有方法只让容器的形状倾斜，而内容保持不变呢？

**嵌套元素方案**

我们可以对内容再应用一次反向的**skew**，从而抵消容器的变形效果，最终就会得到我们所期望的结果。不过要使用这样的方案就不得不使用一层额外的html结构来包裹内容，如下所示：

<p data-height="300" data-theme-id="17491" data-slug-hash="ZWmaqz" data-default-tab="css,result" data-user="janily" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/janily/pen/ZWmaqz/">ZWmaqz</a> by janily (<a href="http://codepen.io/janily">@janily</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

**伪元素方案**

那还有没有其它的方法呢？当然有，这种方法就是把所有的样式应用到伪元素上，然后再对伪元素进行变形。因为内容不在伪元素里，所以内容并不会受到变形的影响。下面就来看看这个方法的具体实现。

为了伪元素能保持良好的灵活性，可以自使用内容的尺寸，一个简单的方法是把宿主元素应用相对定位的属性，而伪元素则应用绝对定位，并且四个方向的偏移量都设置为0，这样伪元素就可以完美的适应内容的尺寸了。代码如下所示：


```.circle {
     postion:relative;
}

.circle::before {
     content:"";
     position:absolute;
     top:0;
     right:0;
     bottom:0;
     left:0;
     z-index:-1;
     background:#58a;
     transform:skewX(-45deg);
}
```

当然为了伪元素不覆盖在宿主元素之上，还要把伪元素的**z-index**的值设置为**－1**。

<p data-height="300" data-theme-id="17491" data-slug-hash="GZwOed" data-default-tab="css,result" data-user="janily" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/janily/pen/GZwOed/">GZwOed</a> by janily (<a href="http://codepen.io/janily">@janily</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

完美实现了效果，并且还不需要额外的添加html结构。这个方法还是用其它的任何形式的变形，比如菱形等。

###环形文字排版

在网页开发中，环形文字不是一个常见的需求，不过有时候也会遇到要让一个短句沿着圆形路径进行排列的。在CSS中，还没有任何一个特性能实现这样的效果。那么有没有不依赖图片能实现这样效果的方法呢？

有一些js脚步方案能实现这样的效果，但仅仅是为了实现这样一个效果，不但要引入臃肿的脚步，还会增加页面的DOM的数量。这样有点得不偿失。

尽管还没有更好的CSS方案，但其实可以使用一点SVG来轻松的解决这个问题。因为SVG原生支持以任意路径排列文字，而圆形也只不过是一种特殊的路径而已。

在svg中，让文本按照路径排列，需要用到一个**textPath**元素来包裹文字，再把它装进一个text元素中。然后在**textPath**中用ID属性来引用**path**中定义好的路径。

假设我们要实现一个按圆形路径进行排列文字的效果，首先需要在html元素中添加一个内联的SVG元素，并用路径来定义我们想要的路径：


```<svg viewBox="0 0 100 100">
<path d="M0,50 a50,50 0 1,1 0,1z" id="circle"></path>
<text>
<textPath xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#circle">
	人法地 地法天 天法道 道法自然
</textPath>
</text>
</svg>
```

就是这么简单，就实现了我们想要的效果：

<p data-height="393" data-theme-id="17491" data-slug-hash="grqMEx" data-default-tab="css,result" data-user="janily" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/janily/pen/grqMEx/">grqMEx</a> by janily (<a href="http://codepen.io/janily">@janily</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

###扩大点击区域

在移动终端特别是智能手机，人们与机器的交互主要是通过触摸来完成。而在触摸的交互中，点击的交互几乎是大部分的应用主要的交互形势，这就涉及到可点击区域的问题。

在人机交互领域有一个著名[Fitts](http://simonwallner.at/ext/fitts/)法则，即人类移动到某个目标所需要的时间有目标距离与目标宽度之比所构成的对数函数，按这个公式我们可以推倒出，目标越大，越容易到达。

所以在移动开发中，对于那些较小的难以点中的控件来说，将其可点击区域向外扩张往往就可以带来可用性的提升。随着触屏的不断普及，这一点变得愈发重要。没有人愿意面对一个难以点中的狭小的按钮来按很多次。

在开发中借助CSS就可以做到提升按钮的可用性。这里又要用到前面使用到的伪元素，基于伪元素的方法极为灵活，基本可以把热区设置为任何想要的尺寸、位置或者是形状，甚至可以脱离元素的原有位置。

<p data-height="377" data-theme-id="17491" data-slug-hash="WwPGOP" data-default-tab="css,result" data-user="janily" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/janily/pen/WwPGOP/">WwPGOP</a> by janily (<a href="http://codepen.io/janily">@janily</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

上面就是一些简单的读书摘要，在原书里还有更多的实战技巧，想知道更多的实战技巧直接买书读。







