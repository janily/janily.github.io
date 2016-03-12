---
layout: post
title: 表单元素交互模式探索
categories:
- Life
tags:
- jquery
- 翻译
---

> 这篇文章是对表单元素的交互的一个探索，翻译于[tutsplus](http://webdesign.tutsplus.com/)网站的[Implementing the Float Label Form Pattern](http://webdesign.tutsplus.com/tutorials/ux-tutorials/implementing-the-float-label-form-pattern/)。

本文表单元素的交互效果主要是来源于[Matt Smith's](http://studiomds.co/)发表在dribble网站上一个表单交互的[效果图](http://dribbble.com/shots/1254439--GIF-Mobile-Form-Interaction)。本文就用HTML，CSS和javascript来实现这个交互效果。

![](http://cdn.tutsplus.com/webdesign/uploads/2013/10/form-animation-_gif_.gif)

表单设计和交互在WEB设计上一直是一个争论不休的话题。对于表单的设计的可访问性和交互一直是web设计中比较困难的一个问题。

[Matt Smith's](http://studiomds.co/)最近就表单交互设计进行了一个有益的探索[地址](http://floatlabel.com/)。

虽然是为IOS系统设计的一个交互效果，但是我们也可以用HTML、CSS和javascript来实现这样的交互效果。

本文要讲解的这个效果就是基于[Matt Smith's](http://studiomds.co/)的这个设计。我们要做的基于一个联系人的表单来制作这个效果。开始吧！

### HTML结构 ###

首先基于Matt设计这个效果，我们来规划我们的HTML结构。当然，从这个设计的效果对于我们怎么去规划和实现我们的javascript的代码也有很大的作用。HTML主要包括下面几个的元素。

- &lt;lable&gt;元素
- &lt;input&gt;元素
- &lt;textarea&gt;元素
- placeholder

![](http://pic.yupoo.com/reicky_v/DoDoXxjC/medium.jpg)

### 交互流程 ###

通过观察Matt设计的这个交互效果，我们可以先理清这个交互的一个流程。这对于我们来规划我们的HTML、CSS和javascript代码有很大的帮助。

#### 在页面加载的时候 ####

- 加载并隐藏label元素
- 隐藏高亮状态的样式
- input和textarea元素，当文本框为空，则显示占位文字；如果用户输入则隐藏占位文字

#### 焦点响应事件(focus) ####

- 如果input是没有焦点激活，则隐藏label元素。

#### 键盘keyup事件(keyup) ####

- input和textarea元素，当文本框为空，则显示占位文字；如果用户输入即获得焦点则隐藏占位文字，显示label元素

#### 失去焦点事件(blur) ####

- 如果失去焦点，则label元素不再是高亮状态的颜色。
- input和textarea元素，当文本框为空，则显示占位文字

理清了交互流程后，接下来我们可以正式开始编码了。

### 编码HTML文件 ###

先来创建一个基本的HTML骨架。

    <!doctype html>
	<html lang="en">
	<head>
	    <meta charset="utf-8">
	    <title>Contact Me</title>
	    <!-- Viewport tag to make things responsive -->
	    <meta name="viewport" content="width=device-width, initial-scale=1">
	    <!-- Link to the stylesheet we will create -->
	    <link rel="stylesheet" href="styles.css">
	    <!-- HTML5 shiv for older browsers -->
	    <!--[if lt IE 9]>
	        <script src="http://html5shiv.googlecode.com/svn/trunk/html5.js"></script>
	    <![endif]-->
	</head>
	<body>
	    <!-- Form Markup Will Go Here -->
	    <!-- Google's hosted version of jQuery -->
	    <script src="//ajax.googleapis.com/ajax/libs/jquery/1.10.2/jquery.min.js"></script>
	    <!-- Link to the javascript file we will create -->
	    <script src="scripts.js"></script>
	</body>
	</html>

然后创建表单的结构：

    <section class="container">
    <h1 class="title">Contact Me</h1>
    <form id="form" class="form" action="#" method="get">
        <ul>
            <li>
                <label for="name">Your Name:</label>
                <input type="text" placeholder="Your Name" id="name" name="name" tabindex="1"/>
            </li>
            <li>
                <label for="email">Your Email:</label>
                <input type="email" placeholder="Your Email" id="email" name="email" tabindex="2"/>
            </li>
            <li>
                <label for="message">Message:</label>
                <textarea placeholder="Message…" id="message" name="message" tabindex="3"></textarea>
            </li>
        </ul>
        <input type="submit" value="Send Message" id="submit"/>
    </form>
	</section>

这里我们用了h1这个标签来包裹“Contact Me”表示这是一个联系信息的表单。这里我们用到了HTML5新增的**type="email"**这个属性来表示电子邮件。如果浏览器不支持这个新的属性，那么就显示一个正常的文本输入框。

现在有了一个基本的结构，我们来看看在浏览器里是什么样子的。

![](http://pic.yupoo.com/reicky_v/DoEoEiAe/medium.jpg)

### 编写CSS ###

HTML编写完后，接下来就是CSS的编写了。在编写的样式的时候，需要记住一条是要保持表单的可用性和易用性。

首先创建一个**style.css**文件，然后在头部引入。由于每一个浏览器都有一些默认的样式，所以我们需要在编写样式之前，重置一下HTML元素的样式。这里我们使用的是Eric Meyer的一个CSS重置样式文件，你可以从这个[地址](http://meyerweb.com/eric/tools/css/reset/reset.css)获得这个文件。

接下来编写一些全局的基本样式：

    /* General
	==================================== */
	*, *:before, *:after {
	    -moz-box-sizing: border-box;
	    -webkit-box-sizing: border-box;
	    box-sizing: border-box;
	}
	body {
	    background: #eaedf1;
	}
	body, input, textarea {
	    font: 1em/1.5 Arial, Helvetica, sans-serif;
	}
	.container {
	    max-width: 25em;
	    margin: 2em auto;
	    width: 95%;
	}
	.title {
	    font-size: 1.5em;
	    padding: .5em 0;
	    text-align: center;
	    background: #323a45;
	    color: white;
	    border-radius: 5px 5px 0 0;
	}

在上面的代码中，你会发现我们使用了**box-sizing:border-box**这个属性来定义元素的盒子解析模型。

现在我们的表单看起来就会好很多了，这还不够，我们还需要做更多的工作来使表单更加美观。

![](http://pic.yupoo.com/reicky_v/DoEtfM6o/medium.jpg)

### 定义表单元素样式 ###

现在来具体定义表单元素的样式，这里我们是参照Matt的那个效果图来编写我们的样式。

首先把行内元素用样式定义为块级元素：

    label, input, textarea {
    display: block;
    border: 0;
	}
	input, textarea {
	    width: 100%;
	    height: 100%;
	    padding: 2.25em 1em 1em;
	    outline: 0;
	}
	textarea {
	    height: 16em;
	    resize: none;
	}

表单就会变成下面这个样式：

![](http://pic.yupoo.com/reicky_v/DoEvwRAe/medium.jpg)

然后是定义label元素的样式，按照Matt的效果图，我们需要把label定义在文本输入框的顶部。

    label {
    font-size: .8125em; /* 13/16 */
    position: absolute;
    top: 1.23em;
    left: 1.23em;
    color: #f1773b;
    opacity: 1;
	}

注意到了**opacity**这个规则么？我们将使用它配合javascript来制作label的动画效果。

接下来再浏览我们的表单跟Matt的效果图越来越像了。

![](http://pic.yupoo.com/reicky_v/DoExccii/medium.jpg)

最后我们来定义提交按钮的样式。

    input[type="submit"] {
    background: #f1773b;
    margin-bottom: 1em;
    color: white;
    border-radius: 3px;
    padding: .75em;
    -webkit-appearance: none; /* remove default browser <button> styling */
    -webkit-transition: .333s ease -webkit-transform;
    transition: .333s ease transform;
	}
	input[type="submit"]:hover {
	    -webkit-transform: scale(1.025);
	    transform: scale(1.025);
	    cursor: pointer;
	}
	input[type="submit"]:active {
	    -webkit-transform: scale(.975);
	    transform: scale(.975);
	}

这里我们使用了属性选择器来定义提交按钮的样式。我们定义按钮在**hover**、**active**不同状态的样式，虽然在老一点的浏览器不支持**transition**这个过渡属性，少了优雅的动画效果，但仍然可以正常提交数据。

![](http://pic.yupoo.com/reicky_v/DoEyGJWb/medium.jpg)

我们表单的样式基本跟Matt的效果图差不多而且还兼顾了表单的可访问性。就算没有javascript，表单依然可以跟用户完成交互。对于不支持placeholder属性的浏览器，用户仍然知道每一个表单元素所代表的的具体含义。

而且我们把**&lt;input&gt;**、**&lt;textarea&gt;**这两关于transition个元素变成了块级元素，能够保证有足够的空间用来进行输入。可访问性和易用性得到了保证。

比如下面这张图是表单在IE8浏览器下的样子：

![](http://pic.yupoo.com/reicky_v/DoECcR5l/medium.jpg)

### 使用javascript来增强表单的功能 ###

现在是该考虑使用javascript来实现Matt所设计的交互效果。我们这里主要是通过javascript配合添加删除CSS类来完成这个交互效果。

我们的javascript代码主要是基于jQuery这个库来实现，先引入jquery文件然后创建一个**scripts.js**文件并在html文件中引入，首先在**scripts.js**文件里编写下面的代码：

    $(document).ready(function(){
    // Our code here
	});

#### 检测浏览器是否支持placeholder属性 ####

这里提醒你一下，我们这个交互效果要依赖placeholder这个属性的支持才能完成的更好。[大部分现代浏览器都支持这个属性](http://caniuse.com/#feat=input-placeholder)。正因为我们的这个交互效果依赖于placeholder这个属性，所以我们需要检查浏览器是否支持这个属性，如果支持则执行我们的交互效果，不支持(主要是IE8-9)则展示一个没有交互效果的表单。这就是渐进增强的开发策略啦。

首先，我们使用jQuery提供的**$.support()**方法来检测浏览器是否支持**placeholder**属性。它会返回一个true或者是false的布尔值，支持则返回true，反之则返回false。

    // Test for placeholder support
	$.support.placeholder = (function(){
	    var i = document.createElement('input');
	    return 'placeholder' in i;
	})();

如果支持placeholder，就在页面加载的时候隐藏**&lt;label&gt;**这个元素。当然为了确保支持placeholder的浏览器才隐藏label元素，不支持placeholder属性的浏览器也能使用表单元素，我们需要改一下我们的javascript代码。

    // Hide labels by default if placeholders are supported
	if($.support.placeholder) {
	    $('.form li').each(function(){
	        $(this).addClass('js-hide-label');
	    });
	    // Code for adding/removing classes here
	}

我们给每一个li元素添加了**js-hide-label**类。这是因为默认情况下label元素是隐藏的，这个类就是用来隐藏label元素的。

为了更好地管理我们的代码，我们通过添加一个**js-**的前缀来表示这个类是通过javascript代码来添加删除的。我们是通过opacity来控制label元素的显示和隐藏的。如下所示：

    .js-hide-label label {
    opacity: 0;
    top: 1.5em
	}

**opacity**这个属性大部分的现代浏览器都支持。IE8并不支持，不过没关系，因为IE8也不支持placeholder，所以在IE8浏览器上不会运行我们的javascript代码。

### 关于transition ###

如果你仔细观察Matt的那个效果图，你会发现文本不是仅仅显示和隐藏这么简单，它在隐藏和显示状态切换之间有一个向上和向下移动的动画效果。因为我们的label元素是通过绝对定位来定义位置的，所以我们可以通过transition和定位的top属性值来实现这样的效果。

那我们就对**label**元素的top和opacity属性来添加transition属性：

    label {
    -webkit-transition: .333s ease top, .333s ease opacity;
    transition: .333s ease top, .333s ease opacity;
	}

效果如下：

![](http://cdn.tutsplus.com/webdesign/uploads/2013/10/form-hiding-label.gif)

现在我们来看看在现代浏览器下表单的样子。

![](http://pic.yupoo.com/reicky_v/DoET4gRd/medium.jpg)

### 删除高亮样式 ###

我们通过在**&lt;li&gt;**这个标签添加和删除类来控制它子元素的样式。我们这里是通过**js-hide-label**这个类来控制的label元素的样式的。现在我们需要写一个样式来删除label标签的高亮状态的颜色。

    .js-unhighlight-label label {
    color: #999
	}

### 监听事件 ###

现在我们完成了一个基本的交互表单。

但是我们还要通过监听用户的事件来触发这个交互动画。我们需要监听**blur**、**focus**和**keyup**这三个事件，如下所示：

    $('.form li').find('input, textarea').on('keyup blur focus', function(e){
    // Cache our selectors
    var $this = $(this),
        $parent = $this.parent();
    // Add or remove classes
    if (e.type == 'keyup') {
        // keyup code here
    }
    else if (e.type == 'blur') {
        // blur code here
    }
    else if (e.type == 'focus') {
        // focus code here
    }
	});

下面是一个简单的对代码的阐述:

- 我们通过**('.form li')**选择所以的**&lt;li&gt;**元素。然后通过** .find('input, textarea')**找到所有的**input**、**textarea**和元素。这里注意一下最后一个按钮的input不在我们我选择范围内。
- 我们这里缓存了我们的选择器。这对于提高性能来说非常有帮助。变量的名字用**$**开头，这样表示我们的这个变量是一个jQuery对象。
- 我们通过**e.type**来判断事件的类型来进行相关的操作。

#### keyup事件 ####

我们刚开始的时候简单的梳理了这个交互动画的逻辑，当用户开始输入文字的时候显示label元素，不是获得焦点的时候显示label。

所以我们使用了**keyup**事件，这样我们就可以检查用户是否输入文字从而进行相关操作，我们是通过**if(e.type == 'keyup'):**来检测的:

    if( $this.val() == '' ) {
    $parent.addClass('js-hide-label');
	} else {
	    $parent.removeClass('js-hide-label');
	}

我们交互逻辑如下：

- 如果文本框是空的，则添加**js-hide-label**这个类和隐藏label元素。
- 也就是说，如果文本框不是空的，则显示label元素和删除**js-hide-label**元素。

现在就可以看到当我们输入文字的时候显示label，删除文字的时候隐藏label元素。

![](http://cdn.tutsplus.com/webdesign/uploads/2013/10/form-hiding-label.gif)

#### blur事件 ####

通过我们开始的交互逻辑，我们知道当用户把焦点移入到下一个文本框的时候，我们还要控制**label**元素文字的颜色。

通过**else if (e.type == 'blur')**来判断焦点事件，代码如下：

    if( $this.val() == '' ) {
    $parent.addClass('js-hide-label');
	}
	else {
	    $parent.removeClass('js-hide-label').addClass('js-unhighlight-label');
	}

这个逻辑是这样子的：

- 当用户移出焦点的时候，如果文本框的值是空的则通过添加**js-hide-label**类隐藏label元素。
- 另外一个方面，当用户移出焦点，文本框的值不是空的时候，就删除**js-hide-label**类添加**js-unhighlight-label**类。

具体效果如下：

![](http://cdn.tutsplus.com/webdesign/uploads/2013/10/form-blur.gif)

#### focus事件 ####

最后是针对焦点事件来进行操作。通过**else if (e.type == 'focus')**根据我们上面制定的逻辑，代码如下：

    if( $this.val() !== '' ) {
    $parent.removeClass('js-unhighlight-label');
	}

流程如下：

- 如果文本框不是空的，删除父元素的**js-unhightlight-label**类。

现在当文本框获得焦点的时候，如果不是空的，label元素的颜色会发生改变，效果如下：

![](http://cdn.tutsplus.com/webdesign/uploads/2013/10/form-focus.gif)

### 添加错误提示 ###

当然，关于表单的交互方面我们还可以做的更多更好，比如表单数据合法性的判断。我们可以创建一些类来定义一些验证信息的显示。如，我们可以通过**js-error**来定义错误信息的样式。

    .js-error {
    border-color: #f13b3b !important; /* override all cases */
	}
	.js-error + li {
	    border-top-color: #f13b3b;
	}
	.js-error label {
	    color: #f13b3b;
	}

![](http://pic.yupoo.com/reicky_v/DoF9L2u1/medium.jpg)

通过本文，我们参照Matt的设计，创建了一个易于使用和渐进增强的表单。当然我们还可以做的更好。你可以在项目中根据自己的需求来增强它，使用它。

### 有用的资源 ###

- [http://floatlabel.com](http://floatlabel.com/)
- [@floatlabel](http://twitter.com/floatlabel)
- 一篇关于这个效果的文章[Float Label Pattern](http://bradfrostweb.com/blog/post/float-label-pattern/)


