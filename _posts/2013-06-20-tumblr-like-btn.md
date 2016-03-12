---
layout: post
title: 用css3和javascript制作Tumblr风格喜欢按钮交互效果
categories:
- Life
tags:
- css3
- javascript
---

[sixux.com](www.sixux.com)这个专门收集展示各种非常不错的网站交互效果的网站，专门把交互细节录制为视频展示在网站上。非常不错。今天在上面看到一个展示Tumblr网站喜欢按钮的交互展示，非常不错。一时心痒就把它趴下来研究了一番，然后自己写了一遍，好记性不如烂笔头。记下我的实现过程,这里css部分是直接用了tumblr的，自己稍微改了下。javascript是我自己搞的。先看下这个具体交互效果是啥样的。

<video controls="" autoplay="true" name="media"><source src="https://vines.s3.amazonaws.com/v/videos/D3BA2FBE-F25D-4966-BD87-9ECD5756689C-8357-000009918C259448_13d63e58003.1.2.mp4?versionId=DAza19KmJaJBfsy44DVFZirRqKZe8zws" type="video/mp4"></video>

这个交互效果的实现的关键点是css3动画和transform配合来实现动画效果，当然首先要熟悉css3中animation和transform这两个东东了，详细就不介绍了，推荐去看看张鑫旭博客中的这方面的两篇文章，有详细的介绍，链接在这里[CSS3 Transitions, Transforms和Animation使用简介与应用展示](http://www.zhangxinxu.com/wordpress/2010/11/css3-transitions-transforms%E5%92%8Canimation%E4%BD%BF%E7%94%A8%E7%AE%80%E4%BB%8B%E4%B8%8E%E5%BA%94%E7%94%A8%E5%B1%95%E7%A4%BA/)。这里主要介绍我实现这个效果的过程。

首先来看下demo

<iframe width="100%" height="300" src="http://jsfiddle.net/janily/PYHva/15/embedded/result,js,html,css" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

###html结构

<pre>
<code>
&lt;ol&gt;
&lt;li style="margin-top:100px;"&gt;
    &lt;div class="post_full"&gt;
        &lt;div class="post_control like" title="Like"&gt;&lt;/div&gt;
   &lt;/div&gt;
&lt;/li&gt;
&lt;/ol&gt;
</code>
</pre>

###css

接下来就是css部分啦，在说之前，首先来分析下，要实现这个交互效果的思路。

这个交互效果关键是在，当点击心形图片的时候怎么让图片变大而且是慢慢上升最后消失，并且上升的过程中伴随着摇摆的动画；第二个是第二次点击的时候，怎么让灰色的图片从上往下掉并且在往下掉的过程中是有撕开动画的，相信看了上面这张图的时候，就容易想到怎么做了。

首先来看下第一个点击效果的代码部分，这里说一下，这个交互效果的动画全部是由css来完成的，javascript只是控制动画结构生成起始与结束状态的部分。

首先我们用javascript来生成这个动画是所需要的结构

<pre>
<code>
function heart() {
var n = $('&lt;div class="post_animated_heart poof"&gt;&lt;span class="heart_left"&gt;&lt;/span&gt&lt;span class="heart_right"&gt;&lt;/span&gt;&lt;/div&gt;');  //生成动画用到的节点
$(".post_control").append(n);  //把它插入到类名为post_control这个节点中去
setTimeout(function() {   //定义一个定时器，用来控制动画节点的存在状态
        n.fadeOut(200, function() {    //动画节点是慢慢消失直至用remove把它删掉
            n.remove()
        })
}, 300); 
};
</code>
</pre>

这里我们定义了一个heart函数，用来管理动画节点的开始与结束。代码中的作用有注释。

接下来是实现交互效果动画的部分，用css3来实现，首先是第一次点击的时候，心形在上升的过程中有个逐渐变大的过程，这里用css3中transform的scale来实现的，代码如下

<pre>
<code>
@-webkit-keyframes post_poof {
    0% {
        opacity: 1;
        -webkit-transform: scale(0.5);
    }

    25% {
        opacity: 1;
        -webkit-transform: scale(1);
    }

    100% {
        opacity: 0;
        -webkit-transform: scale(1.1);
    }
}
@keyframes post_poof {
    0% {
        opacity: 1;
        transform: scale(0.5);
    }

    25% {
        opacity: 1;
        transform: scale(1);
    }

    100% {
        opacity: 0;
        transform: scale(1.1);
    }
}
</code>
</pre>

这里一大坨，由于现在各个浏览器还是各自为政，还要加上私有前缀，我这里只是写了webkit内核和W3C标准的部分，animation这东东主要是靠keyframes这个东东来实现的，啥是keyframes，就跟flash中的关键帧差不多，这里代码的意思是开始的时候透明度为1和0.5倍的放大，到25%这个帧的时候放大1倍，透明度还是为1，到结束帧的时候也就是100%的时候，透明度为0，放大为1.1倍。后面的代码基本上都可以这样的来理解。

接下来就是实现心形的上升和摇摆的动画的啦，首先看代码部分

<pre>
<code>
@keyframes like_poof {
    0% {
        opacity: 1;
        transform: rotate(0deg);
    }

    25% {
        opacity: 1;
        transform: rotate(-20deg);
    }

    75% {
        transform: rotate(20deg);
    }

    100% {
        margin-top: -80px;
        opacity: 0;
        transform: rotate(0deg);
    }
}
</code>
</pre>

这里用到的知识点是css3中transform中的rotate来实现摇摆动画以及margin-top来实现上升的动画。rotate()是来旋转物体的，像上面的的rotate(-20deg);是表示向左倾斜20度，那rotate(20deg);就表示向右倾斜20度，这样就实现了左右摇摆的动画。至于上升就更容易理解啦，直接在动画结束的时候把margin-top设置为-80就可以实现向上升起的动画了。

接下来就是第二次点击心形交互效果的实现了，就是取消喜欢的时候，心形往下掉，并且是一个撕裂的过程，这里再回到我们生成动画节点的结构上面来说明下。

<pre>
<code>
function heart() {
var n = $('<div class="post_animated_heart poof"><span class="heart_left"></span><span class="heart_right"></span></div>');  //生成动画用到的节点
....
};

</code>
</pre>

我们在heart函数中生成的结构中有heart_left和heart_right这个节点，这是我们实现这个撕裂动画要用到的，还是通过图片来分析，非常的简明易懂。

<img src="http://pic.yupoo.com/reicky/CWYBn0uu/medium.jpg" width="62" height="200"/>

我们把撕裂的心形分成了两半，这样我们通过heart_left和heart_right这两个节点的背景图片定位配合css3动画就可以实现效果了，首先来看下代码

<pre>
<code>
	@keyframes unlike_heartbreak_left {
    0% {
        margin-top: -36px;
        opacity: 1;
        transform: rotate(0deg);
    }

    30% {
        margin-top: -36px;
        opacity: 1;
    }

    80% {
        margin-top: 0;
        opacity: 0;
        transform: rotate(-15deg);
    }

    100% {
        margin-top: 0;
        opacity: 0;
        transform: rotate(-15deg);
    }
}
@keyframes unlike_heartbreak_right {
    0% {
        margin-top: -36px;
        opacity: 1;
        transform: rotate(0deg);
    }

    30% {
        margin-top: -36px;
        opacity: 1;
    }

    80% {
        margin-top: 0;
        opacity: 0;
        transform: rotate(15deg);
    }

    100% {
        margin-top: 0;
        opacity: 0;
        transform: rotate(15deg);
    }
}
</code>
</pre>

动画我们也分成了两部分，分别是心形左边和心形右边的部分，包含心形下坠和心形撕裂两部分动画。心形下坠主要是通过逐渐改变心形的margin-top值来实现的；而心形撕裂主要是通过rotate()来改变心形的角度来实现的。通过改变透明度实现心形的渐隐渐现的效果。

好了动画部分的代码加来就是来把动画效果添加到要实现效果的节点上去了，这里我们通过为类名为post_control这个节点来实现我们点击的动画的效果，还是要通过我们的js代码来回顾下，我们为实现动画效果为post_control添加了哪些类

<pre>
<code>
$(function(){
 $(".post_control").toggle(function(){
    $(this).addClass("liked").removeClass("unlike");
    heart();
    },
        function(){
        $(this).addClass("unlike").removeClass("liked");
        heart();
    }
);
});
</code>
</pre>

我用到了两个点击事件，一个是单击的时候添加liked类，来表示喜欢；第二次单击的时候是取消喜欢，添加了unlike类。我们用到了jquery中的toggle方法来切换这两个事件，我们的动画效果也是通过这个来实现的，下面我们看看怎么样通过css来执行我们的动画效果。这里主要是来说明下css是如何来执行动画的部分代码，其它css布局等部分就不说明了。

<pre>
<code>
.post_full .poof {
	position: absolute;
	top: 50%;
	left: 50%;
	z-index: 100;
	display: block;
	margin: -27px 0 0 -27px;
	width: 54px;
	height: 54px;
	background: url('http://photo.yupoo.com/reicky/CWYBn0uu/medish.jpg') no-repeat;
	-webkit-transform-origin: 50% 50%;
	-moz-transform-origin: 50% 50%;
	-o-transform-origin: 50% 50%;
	transform-origin: 50% 50%;
	-webkit-animation: post_poof .6s ease-out;
	-moz-animation: post_poof .6s ease-out;
	-ms-animation: post_poof .6s ease-out;
	-o-animation: post_poof .6s ease-out;
	animation: post_poof .6s ease-out;
	-ms-transform-origin: 50% 50%;
}
.post_full .post_animated_heart {
	background: url('http://photo.yupoo.com/reicky/CWYBn0uu/medish.jpg') no-repeat 0 -66px;
	-webkit-animation: like_poof .6s ease-out;
	-moz-animation: like_poof .6s ease-out;
	animation: like_poof .6s ease-out;
}

.post_full .post_animated_heart span {
	display: none;
}
.post_full .unlike .post_animated_heart span {
	display:block;
}
.post_full .unlike .post_animated_heart {
	background: transparent;
	-webkit-animation: none;
	-moz-animation: none;
	-ms-animation: none;
	-o-animation: none;
	animation: none;
}

.post_full .unlike .post_animated_heart span {
	position: absolute;
	top: 0;
	left: 50%;
	display: block;
	margin-left: 3px;
	height: 100%;
	background: url('http://photo.yupoo.com/reicky/CWYBn0uu/medish.jpg') no-repeat;
}

.post_full .unlike .post_animated_heart span.heart_left {
	margin-left: -29px;
	width: 29px;
    height:48px;
	background-position: 0 -6px;
	-webkit-transform-origin: 26px 54px;
	-moz-transform-origin: 26px 54px;
	-o-transform-origin: 26px 54px;
	transform-origin: 26px 54px;
	-ms-transform-origin: 26px 54px;
	-webkit-animation: unlike_heartbreak_left .6s ease-out;
	-moz-animation: unlike_heartbreak_left .6s ease-out;
	-ms-animation: unlike_heartbreak_left .6s ease-out;
	-o-animation: unlike_heartbreak_left .6s ease-out;
	animation: unlike_heartbreak_left .6s ease-out;
}

.post_full .unlike .post_animated_heart span.heart_right {
	width: 29px;
    height:48px;
	background-position: -29px -6px;
	-webkit-transform-origin: 0 54px;
	-moz-transform-origin: 0 54px;
	-o-transform-origin: 0 54px;
	transform-origin: 0 54px;
	-ms-transform-origin: 0 54px;
	-webkit-animation: unlike_heartbreak_right .6s ease-out;
	-moz-animation: unlike_heartbreak_right .6s ease-out;
	-ms-animation: unlike_heartbreak_right .6s ease-out;
	-o-animation: unlike_heartbreak_right .6s ease-out;
	animation: unlike_heartbreak_right .6s ease-out;
}
</code>
</pre>

当第一次点击的时候，我们生成有poof和post_animated_heart这两个类的节点，我们调用了post_poof和like_poof这两个动画，来实现心形上升的动画。这里注意下这里

<pre>
<code>
.post_full .post_animated_heart span {
	display: none;
}
.post_full .unlike .post_animated_heart span {
	display:block;
}
</code>
</pre>

因为在post_animated_heart这个节点下的span是用来实现心形撕裂效果的，所以开始的时候我们把它隐藏掉了，当第二次点击的时候，添加了个unlike类，通过这个类调用了unlike_heartbreak_right和unlike_heartbreak_left两个动画来实现心形撕裂的效果，好了基本上，这个效果的实现就是这样的。


















