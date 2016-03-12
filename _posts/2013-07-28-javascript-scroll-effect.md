---
layout: post
title: javascript页面滚动动画效果
categories:
- Life
tags:
- javascript
---

这个教程是在codrops网站上发现的，首先来看下具体效果是怎么样的，[demo](http://tympanus.net/Blueprints/OnScrollEffectLayout/).

codrops这个网站真心不错，里面有好多前端方面的教程，对于我来说，最重要的是能从这么些教程里面学到很多，特别是对于javascript方面。提高了很多。

废话不多说啦，原教程的地址在[这里](http://tympanus.net/codrops/2013/07/18/on-scroll-effect-layout/),我这里主要是来分析下它里面的javascript代码，具体的css和html就不详细说了。

实现这样的效果，HTML结构得讲讲

<pre>
<code>
&lt;div id="cbp-so-scroller" class="cbp-so-scroller"&gt;
 
    &lt;section class="cbp-so-section"&gt;
        &lt;article class="cbp-so-side cbp-so-side-left"&gt;
            &lt;h2>Lemon drops&lt;/h2&gt;
            &lt;p>Fruitcake toffee jujubes. Topping biscuit sesame snaps jelly caramels jujubes tiramisu fruitcake. Marzipan tart lemon drops chocolate sesame snaps jelly beans.&lt;/p&gt;
        &lt;/article>
        &lt;figure class="cbp-so-side cbp-so-side-right"&gt;
            &lt;img src="images/1.png" alt="img01"&gt;
        &lt;/figure&gt;
    &lt;/section&gt;
 
	 &lt;section class="cbp-so-section"&gt;
        &lt;article class="cbp-so-side cbp-so-side-left"&gt;
            &lt;h2>Lemon drops&lt;/h2&gt;
            &lt;p>Fruitcake toffee jujubes. Topping biscuit sesame snaps jelly caramels jujubes tiramisu fruitcake. Marzipan tart lemon drops chocolate sesame snaps jelly beans.&lt;/p&gt;
        &lt;/article>
        &lt;figure class="cbp-so-side cbp-so-side-right"&gt;
            &lt;img src="images/1.png" alt="img01"&gt;
        &lt;/figure&gt;
    &lt;/section&gt; 

    &lt;section&gt;
        <!-- ... -->
    &lt;/section&gt;
	...
     
&lt;/div&gt;
</code>
</pre>

看完上面代码就会发现一个特点section都有一个名为cbp-so-section的类，而我们需要页面滚动的动画也是通过运动类名为cbp-so-section的盒子来实现的。

在原文中有说道，由于在移动设备上由于对滚动支持上有些问题，所以这个效果在移动设备不会有动画效果。可以去看看[这篇文章](http://tjvantoll.com/2012/08/19/onscroll-event-issues-on-mobile-browsers/)。

css方面就不说拉，动画效果主要是用到css3的transform和transition来实现的，css3比css2确实是一个质的飞跃，使web视觉表现上就像马车和汽车的对比一样产生了质的飞跃。

说到这里，对于广大在中国奋战的前端来说，是说不尽的心酸和流不尽的泪啊。

回到文章中来，接着就javascript部分啦，这部分得详细说说，不是有这么一句话么，站在巨人的肩膀山让我们看的更远。通过观看别人写代码的方法来提高自己的代码质量和编写代码的能力。

<pre>
<code>
;( function( window ) {
     
    'use strict';
 
    var docElem = window.document.documentElement;
 
    function getViewportH() {
        var client = docElem['clientHeight'],
            inner = window['innerHeight'];
         
        if( client < inner )
            return inner;
        else
            return client;
    }
 
    function scrollY() {
        return window.pageYOffset || docElem.scrollTop;
    }
 
    // http://stackoverflow.com/a/5598797/989439
    function getOffset( el ) {
        var offsetTop = 0, offsetLeft = 0;
        do {
            if ( !isNaN( el.offsetTop ) ) {
                offsetTop += el.offsetTop;
            }
            if ( !isNaN( el.offsetLeft ) ) {
                offsetLeft += el.offsetLeft;
            }
        } while( el = el.offsetParent )
 
        return {
            top : offsetTop,
            left : offsetLeft
        }
    }
 
    function inViewport( el, h ) {
        var elH = el.offsetHeight,
            scrolled = scrollY(),
            viewed = scrolled + getViewportH(),
            elTop = getOffset(el).top,
            elBottom = elTop + elH,
            // if 0, the element is considered in the viewport as soon as it enters.
            // if 1, the element is considered in the viewport only when it's fully inside
            // value in percentage (1 >= h >= 0)
            h = h || 0;
 
        return (elTop + elH * h) <= viewed && (elBottom) >= scrolled;
    }
 
    function extend( a, b ) {
        for( var key in b ) { 
            if( b.hasOwnProperty( key ) ) {
                a[key] = b[key];
            }
        }
        return a;
    }
 
    function cbpScroller( el, options ) {   
        this.el = el;
        this.options = extend( this.defaults, options );
        this._init();
    }
 
    cbpScroller.prototype = {
        defaults : {
            // The viewportFactor defines how much of the appearing item has to be visible in order to trigger the animation
            // if we'd use a value of 0, this would mean that it would add the animation class as soon as the item is in the viewport. 
            // If we were to use the value of 1, the animation would only be triggered when we see all of the item in the viewport (100% of it)
            viewportFactor : 0.2
        },
        _init : function() {
            if( Modernizr.touch ) return;
            this.sections = Array.prototype.slice.call( this.el.querySelectorAll( '.cbp-so-section' ) );
            this.didScroll = false;
 
            var self = this;
            // the sections already shown...
            this.sections.forEach( function( el, i ) {
                if( !inViewport( el ) ) {
                    classie.add( el, 'cbp-so-init' );
                }
            } );
 
            var scrollHandler = function() {
                    if( !self.didScroll ) {
                        self.didScroll = true;
                        setTimeout( function() { self._scrollPage(); }, 60 );
                    }
                },
                resizeHandler = function() {
                    function delayed() {
                        self._scrollPage();
                        self.resizeTimeout = null;
                    }
                    if ( self.resizeTimeout ) {
                        clearTimeout( self.resizeTimeout );
                    }
                    self.resizeTimeout = setTimeout( delayed, 200 );
                };
 
            window.addEventListener( 'scroll', scrollHandler, false );
            window.addEventListener( 'resize', resizeHandler, false );
        },
        _scrollPage : function() {
            var self = this;
 
            this.sections.forEach( function( el, i ) {
                if( inViewport( el, self.options.viewportFactor ) ) {
                    classie.add( el, 'cbp-so-animate' );
                }
                else {
                    // this add class init if it doesn't have it. This will ensure that the items initially in the viewport will also animate on scroll
                    classie.add( el, 'cbp-so-init' );
                     
                    classie.remove( el, 'cbp-so-animate' );
                }
            });
            this.didScroll = false;
        }
    }
 
    // add to global namespace
    window.cbpScroller = cbpScroller;
 
} )( window );
</code>
</pre>

可以看到我们的代码被包裹这样的结构里

<pre>
<code>
( function (window) { ... })(window);
</code>
</pre>

这里是创建了一个匿名函数和立即执行的函数表达式，就会产生一个命名空间。这样你就不用担心与别人的代码产生冲突和变量冲突了。不会污染全局的命名空间了。而通过window参数导出要暴露给外部的函数。

由于我们这里没有使用jquery，所以一些常见的dom操作需要封装成函数，以便于使用。

<pre>
<code>
 var docElem = window.document.documentElement;
 
    function getViewportH() {
        var client = docElem['clientHeight'],
            inner = window['innerHeight'];
         
        if( client < inner )
            return inner;
        else
            return client;
    }
 
    function scrollY() {
        return window.pageYOffset || docElem.scrollTop;
    }
 
    // http://stackoverflow.com/a/5598797/989439
    function getOffset( el ) {
        var offsetTop = 0, offsetLeft = 0;
        do {
            if ( !isNaN( el.offsetTop ) ) {
                offsetTop += el.offsetTop;
            }
            if ( !isNaN( el.offsetLeft ) ) {
                offsetLeft += el.offsetLeft;
            }
        } while( el = el.offsetParent )
 
        return {
            top : offsetTop,
            left : offsetLeft
        }
    }
 
    function inViewport( el, h ) {
        var elH = el.offsetHeight,
            scrolled = scrollY(),
            viewed = scrolled + getViewportH(),
            elTop = getOffset(el).top,
            elBottom = elTop + elH,
            // if 0, the element is considered in the viewport as soon as it enters.
            // if 1, the element is considered in the viewport only when it's fully inside
            // value in percentage (1 >= h >= 0)
            h = h || 0;
 
        return (elTop + elH * h) <= viewed && (elBottom) >= scrolled;
    }
 
    function extend( a, b ) {
        for( var key in b ) { 
            if( b.hasOwnProperty( key ) ) {
                a[key] = b[key];
            }
        }
        return a;
    }
</code>
</pre>

上面几个函数的作用是

1.getViewportH是用来确定浏览器窗口的尺寸（浏览器的视口，不包括工具栏和滚动条）。由于不同浏览器用来获取尺寸的属性不同所以在这个方法里做了下判断。

对于Internet Explorer、Chrome、Firefox、Opera 以及 Safari用innerHeight - 浏览器窗口的内部高度而对于 Internet Explorer 8、7、6、5：就要用clientHeight来获取了。

2.scrollY方法使用来获取滚动距离的，同样针对浏览器做了兼容。

3.getOffset方法是用来获得元素offsetLeft和offsetTop的值的。

4.inViewport方法是用来判断当前试图区域是否在可视区域里，这个方法在页面滚动动画中的作用就是，当盒子滚动到当前可视的区域内的时候，就执行动画效果，不在可视区域内的盒子就不执行动画。

5.extend方法对于写过jquery插件的来说，就不陌生了这里extend方法也是模仿了jquery中的extend方法。主要是用来是扩展现有的对象，将两个或更多对象的内容合并到第一个对象。如果后面的参数和前面的参数存在相同的名称，那么后面的会覆盖前面的参数值。因为我们这个是写成了一个插件的形式，所以我们要用到extend方法来暴露一些参数来给使用这来修改插件的参数。

好了分析完一些基础功能函数后，就来写具体控制页面滚动的方法了，对了提醒一下，这里一些想常见的类的操作，如addClass、removeClass等方法使用到了classie这个方法工具集来实现的。

<pre>
<code>
function cbpScroller( el, options ) {   
        this.el = el;
        this.options = extend( this.defaults, options );
        this._init();
}
 
cbpScroller.prototype = {
    defaults : {
        // The viewportFactor defines how much of the appearing item has to be visible in order to trigger the animation
        // if we'd use a value of 0, this would mean that it would add the animation class as soon as the item is in the viewport. 
        // If we were to use the value of 1, the animation would only be triggered when we see all of the item in the viewport (100% of it)
        viewportFactor : 0.2
    },
    _init : function() {
        if( Modernizr.touch ) return;
        this.sections = Array.prototype.slice.call( this.el.querySelectorAll( '.cbp-so-section' ) );
        this.didScroll = false;

        var self = this;
        // the sections already shown...
        this.sections.forEach( function( el, i ) {
            if( !inViewport( el ) ) {
                classie.add( el, 'cbp-so-init' );
            }
        } );

        var scrollHandler = function() {
                if( !self.didScroll ) {
                    self.didScroll = true;
                    setTimeout( function() { self._scrollPage(); }, 60 );
                }
            },
            resizeHandler = function() {
                function delayed() {
                    self._scrollPage();
                    self.resizeTimeout = null;
                }
                if ( self.resizeTimeout ) {
                    clearTimeout( self.resizeTimeout );
                }
                self.resizeTimeout = setTimeout( delayed, 200 );
            };

        window.addEventListener( 'scroll', scrollHandler, false );
        window.addEventListener( 'resize', resizeHandler, false );
    },
    _scrollPage : function() {
        var self = this;

        this.sections.forEach( function( el, i ) {
            if( inViewport( el, self.options.viewportFactor ) ) {
                classie.add( el, 'cbp-so-animate' );
            }
            else {
                // this add class init if it doesn't have it. This will ensure that the items initially in the viewport will also animate on scroll
                classie.add( el, 'cbp-so-init' );
                 
                classie.remove( el, 'cbp-so-animate' );
            }
        });
        this.didScroll = false;
    }
}
</code>
</pre>

写了一个名为cbpScroller的函数，我们主要是通过构造函数和原型链的方式来编写我们的代码。这也是javascript界中流行的编写方式，好扩展。

<pre>
<code>
function cbpScroller( el, options ) {   
        this.el = el;
        this.options = extend( this.defaults, options );
        this._init();
}
</code>
</pre>

这里主要是传入一些参数，以及默认插件可配置的一些参数，具体的代码在cbpScroller的prototype中，下面来分析下具体是怎么来实现的

<pre>
<code>
cbpScroller.prototype = {
        defaults : {
            // The viewportFactor defines how much of the appearing item has to be visible in order to trigger the animation
            // if we'd use a value of 0, this would mean that it would add the animation class as soon as the item is in the viewport. 
            // If we were to use the value of 1, the animation would only be triggered when we see all of the item in the viewport (100% of it)
            viewportFactor : 0.2
        },
        _init : function() {
            if( Modernizr.touch ) return;
            this.sections = Array.prototype.slice.call( this.el.querySelectorAll( '.cbp-so-section' ) );
            this.didScroll = false;
 
            var self = this;
            // the sections already shown...
            this.sections.forEach( function( el, i ) {
                if( !inViewport( el ) ) {
                    classie.add( el, 'cbp-so-init' );
                }
            } );
 
            var scrollHandler = function() {
                    if( !self.didScroll ) {
                        self.didScroll = true;
                        setTimeout( function() { self._scrollPage(); }, 60 );
                    }
                },
                resizeHandler = function() {
                    function delayed() {
                        self._scrollPage();
                        self.resizeTimeout = null;
                    }
                    if ( self.resizeTimeout ) {
                        clearTimeout( self.resizeTimeout );
                    }
                    self.resizeTimeout = setTimeout( delayed, 200 );
                };
 
            window.addEventListener( 'scroll', scrollHandler, false );
            window.addEventListener( 'resize', resizeHandler, false );
        },
        _scrollPage : function() {
            var self = this;
 
            this.sections.forEach( function( el, i ) {
                if( inViewport( el, self.options.viewportFactor ) ) {
                    classie.add( el, 'cbp-so-animate' );
                }
                else {
                    // this add class init if it doesn't have it. This will ensure that the items initially in the viewport will also animate on scroll
                    classie.add( el, 'cbp-so-init' );
                     
                    classie.remove( el, 'cbp-so-animate' );
                }
            });
            this.didScroll = false;
        }
    }
</code>
</pre>

在cbpScroller的prototype中首先是配置参数，我们这里主要有一个参数viewportFactor，这个参数的作用是，是用来定义当我们设定的盒子区域在进入当前的可视区域多大的范围内就触发动画效果，值为9的时候，是已进入就执行动画；值为1的时候，是完全进入可视区域才执行动画。这两种值规定的范围都不是很好，所以我们给了一个默认值0.2是一个比较理想的范围。至于范围后面的代码会清楚的告诉你。

首先是_init方法，做一些初始化的工作的，用到了Modernizr这个库来判断是否支持touch（触摸）事件。以及选出了我们用来执行动画的dom结构区域类名为cbp-so-section的区块。一个用来设定滚动的开关this.didScroll = false，默认为false。

还给所有有cbp-so-section区块加上了名为cbp-so-init的类，这个类主要的作用是用来配合做动画效果的。

第二个scrollHandler的方法，主要是用来控制滚动事件和缩放浏览器窗口的。当滚动的页面的时候，加个了计时器，当滚动后60毫秒才执行我们的滚动方法。当缩放浏览器出发resize时，会执行resizeHandler方法。

第三个_scrollPage方法，就是具体的执行页面滚动的时候会触发的方法了。主要的作用是，当我们规定的盒子区域就如可视区域时，我们就给当前的盒子区域，也就是类名为cbp-so-section的区域，加上一个cbp-so-animate的类，这个类就是用css3来实现动画效果的。

javascript部分就分析完了。

路漫漫其修远兮，吾将上下而求索！







