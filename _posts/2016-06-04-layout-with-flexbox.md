---
layout: post
title: 使用flexbox，网页布局从未如此简单
categories:
- Life
tags:
- flexbox
- CSS3
- 前端

---

> 在[tutsplus](http://webdesign.tutsplus.com/)网站看到一片不错的关于使用flexbox进行网页布局方面的文章，还不错，翻译过来，对开阔下思维还是不错的。[原文地址](http://webdesign.tutsplus.com/tutorials/how-to-build-a-news-website-layout-with-flexbox--cms-26611)。

在这篇文章中我们会使用flexbox来实现如下图所示的网页布局。

![](https://cms-assets.tutsplus.com/uploads/users/30/posts/26611/final_image/preview.png)

在本文中，我们关注的是如何利用flexbox这一新特性来实现像[The Guardian](http://www.theguardian.com/)网站一样的布局。

使用flexbox来实现，是因为它有下面这几点非常强大的特性：

**能轻而易举的实现响应式布局**

**能轻松的实现等高布局**

**能随意的增添内容，而保证布局不乱**

说了这么多，下面我们来一步一步实现这个布局。

### 1、实现两列布局

在CSS中要实现列的布局，一直以来都是一个颇有挑战的领域。在很长的一段时间里，只能通过浮动或者是表格来实现列的布局，而且还有各种各样的问题。

flexbox这一新属性的出现，就能轻而易举的实现各种各样的布局，使用flexbox来进行布局，主要有下面几点优势：

**代码整洁**：只需要声明下*display:flex*

不需要清除浮动来解决内容塌陷的问题

使布局的html结构更具语义化

**响应式**：只需要用几行代码就能解决在各种不同屏幕下的展示问题

首先来实现一个简单的两列布局。


```
<div class="columns">
  <div class="column main-column">
    2/3 column
  </div>
  <div class="column">
    1/3 column
  </div>
</div>
```


上面的代码中主要是两部分：

父容器**columns**

两个子容器**column**，一个子容器又一个额外的类**main-column**后面会用它来控制宽度


```
.columns {
  display: flex;
}
 
.column {
  flex: 1;
}
 
.main-column {
  flex: 2;
}
```

在上面的代码中，**main-column**类的容器的flex值是**2**，这样它占的宽度就会比另一个容易要宽。

如下所示：

<p data-height="300" data-theme-id="17491" data-slug-hash="gMbpQM" data-default-tab="result" data-user="tutsplus" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/tutsplus/pen/gMbpQM/">How to Build a News Website Layout with Flexbox i</a> by Envato Tuts+ (<a href="http://codepen.io/tutsplus">@tutsplus</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

### 2、声明每一列为flex容器

因为在每一列中会垂直排列不同的内容，所以需要**column**容器也是具有flex属性的容器。主要是要达到下面两个目的：

**column**容器里面的内容垂直排列

内容自动填充**column**容器里面的空间


```
.column {
  display: flex;
  flex-direction: column; /* 内容垂直方向排列 */
}
 
.article {
  flex: 1; /* 自动填充剩余空间 */
}
```

**flex-direction:column**和**flex:1**两个属性主要是为了确保内容能垂直排列并且两列始终保持等高。

<p data-height="300" data-theme-id="17491" data-slug-hash="PzwqXG" data-default-tab="result" data-user="tutsplus" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/tutsplus/pen/PzwqXG/">How to Build a News Website Layout with Flexbox ii</a> by Envato Tuts+ (<a href="http://codepen.io/tutsplus">@tutsplus</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

### 3、声明每一列的内容为flex容器

现在，就需要为内容添砖加瓦了，每一个内容(articles)里面都包含下面这些内容：

标题

一段文字

一个关于文章作者时间等内容的信息工具栏

一张响应式的图片

我们将使用flex来实现这些内容的布局，如下图所示：

![](https://cms-assets.tutsplus.com/uploads/users/30/posts/26611/image/card.png)

下面是主要的html和css代码：


```
<a class="article first-article">
  <figure class="article-image">
    <img src="">
  </figure>
  <div class="article-body">
    <h2 class="article-title">
      <!-- title -->
    </h2>
    <p class="article-content">
      <!-- content -->
    </p>
    <footer class="article-info">
      <!-- information -->
    </footer>
  </div>
</a>
```


```
.article {
  display: flex;
  flex-direction: column;
  flex-basis: auto; /* 定义项目的空间为项目的本来大笑  */
}
 
.article-body {
  display: flex;
  flex: 1;
  flex-direction: column;
}
 
.article-content {
  flex: 1; /* 这个值将会使内容填满剩余的空间，从而使工具信息栏永远居于地步 */
}
```

而**flex-direction: column;**则是用来垂直排列内容的。

声明**article-content**的**flex:1**则是保证了**article-content**填满剩余的空间，从而使**article-info**居于底部，跟列的高度无关。

<p data-height="363" data-theme-id="17491" data-slug-hash="RRNWNR" data-default-tab="result" data-user="tutsplus" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/tutsplus/pen/RRNWNR/">How to Build a News Website Layout with Flexbox iii</a> by Envato Tuts+ (<a href="http://codepen.io/tutsplus">@tutsplus</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

### 4、多加几列看看

在左边的一栏中，尝试着多加些内容看看。


```
<div class="columns">
  <div class="column nested-column">
    <a class="article">
      <!-- Article content -->
    </a>
  </div>
 
  <div class="column">
    <a class="article">
      <!-- Article content -->
    </a>
    <a class="article">
      <!-- Article content -->
    </a>
    <a class="article">
      <!-- Article content -->
    </a>
  </div>
</div>
```

如果想让第一个包含子内容的第一列更宽一点，我们可以声明一个类，添加点样式就可以轻松做到：


```
.nested-column {
  flex: 2;
}
```

结果如下所示：

<p data-height="379" data-theme-id="17491" data-slug-hash="wWBKaq" data-default-tab="result" data-user="tutsplus" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/tutsplus/pen/wWBKaq/">How to Build a News Website Layout with Flexbox iv</a> by Envato Tuts+ (<a href="http://codepen.io/tutsplus">@tutsplus</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

### 5、第一篇文章水平布局

第一篇文章占据非常显眼的地方，为了充分利用空间，我们可以试着把它变为水平布局的形势。


```
.first-article {
  flex-direction: row;
}
 
.first-article .article-body {
  flex: 1;
}
 
.first-article .article-image {
  height: 300px;
  order: 2;
  padding-top: 0;
  width: 400px;
}
```

**order**属性在这里出现的非常及时，我们可以用它来控制html元素的显示顺序。**article-image**在实际的html中是排在**article-body**前面的，但是通过**order**属性就可以使它显示在后面。

<p data-height="300" data-theme-id="17491" data-slug-hash="VjYvve" data-default-tab="result" data-user="tutsplus" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/tutsplus/pen/VjYvve/">How to Build a News Website Layout with Flexbox v</a> by Envato Tuts+ (<a href="http://codepen.io/tutsplus">@tutsplus</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

### 6、响应式布局

看起来好像是没有什么问题了，不过要是能自适应各种屏幕那就更完美了。

flexbox有一个重要的特性就是，只需要去掉**display:flex**的声明从而使具有flex属性的容器不再具有flex属性。

这样一来，我们可以选择一个断点来触发flex的响应，如下代码所示：


```

@media screen and (min-width: 800px) {
  .columns,
  .column {
    display: flex;
  }
}
```

就是这么简单，在小屏设备上，所有的内容将会垂直排列。只有在大于800px的设备上才会以两列布局的形势呈现。

### 7、更完美一点点

为了是布局在大屏幕设备上能更好的呈现给用户，再添加一点点css代码：


```

@media screen and (min-width: 1000px) {
  .first-article {
    flex-direction: row;
  }
 
  .first-article .article-body {
    flex: 1;
  }
 
  .first-article .article-image {
    height: 300px;
    order: 2;
    padding-top: 0;
    width: 400px;
  }
 
  .main-column {
    flex: 3;
  }
 
  .nested-column {
    flex: 2;
  }
}
```

在超过1000px的设备上，第一篇文章的图片是在右边，而文字是在左边的。**main-column**的宽度是超过75%的。如下所示，建议在大屏幕上来看看效果：

<p data-height="300" data-theme-id="17491" data-slug-hash="Wxbvdp" data-default-tab="result" data-user="tutsplus" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/tutsplus/pen/Wxbvdp/">How to Build a News Website Layout with Flexbox</a> by Envato Tuts+ (<a href="http://codepen.io/tutsplus">@tutsplus</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

### 总结

通过上面这个实例，我们并不需要了解flexbox所有的知识才能开始使用它。just do it!







