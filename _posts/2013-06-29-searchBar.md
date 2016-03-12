---
layout: post
title: 用css3和javascript制作可收缩搜索框
categories:
- Life
tags:
- css3
- javascript
---

今天这篇文章来源于Codrops,是关于如何创建一个移动和响应搜索栏的文章。觉得不错就拿过来了，学习学习。原文在[这里](http://tympanus.net/codrops/2013/06/26/expanding-search-bar-deconstructed/)。

首先来看看具体的这个搜索框是啥样子的，如图所示：

<img src="http://pic.yupoo.com/reicky/CYud31Au/medish.jpg" width="580" height="315"/>

搜索框默认的的样子是一个按钮，当点击的时候搜索框才出现具体的输入框。

不罗嗦啦，下面就直接来做吧！

###HTML结构

<pre>
<code>
&lt;div id="sb-search" class="sb-search"&gt;
    &lt;form&gt;
       &lt;input class="sb-search-input" placeholder="Enter your search term..." type="search" value="" name="search" id="search"&gt;
        &lt;input class="sb-search-submit" type="submit" value=""&gt;
        &lt;span class="sb-icon-search"&gt;&lt;/span&gt;
    &lt;/form&gt;
&lt;/div&gt;
</code>
</pre>

结构写完了，接下来就是css了

###CSS

这里说明下，我这里搜索按钮的图片用搜索这两个文字代替，而不是想原文那样用css3的font-face的文字来制作图片了。

首先，按照我们的构思，这个搜索框默认只是按钮可见，而输入框是隐藏的。只有当我们点击按钮的时候，输入框才显示。这个自然就想到了overflow这一属性了。

首先我们要定义搜索框的宽度，那该怎么来定义了？首先我们想一下，默认的时候只显示按钮，点击的时候才显示输入框。至少要有一个最小宽度，用来显示按钮的，而对于搜索框呢？我们可以先把整个搜索框的宽度定义为0%，当点击的时候把它的宽度设置为100%，这样就可以达到我们想要的效果了。

当然为了在显示输入框宽度的时候，有一个自然的过度效果，我们要用到transition这一属性，以及 -webkit-backface-visibility: hidden这一属性，是为了防止在IOS设备上input获得焦点的时候闪屏的出现。

最终的代码是这样的

<pre>
<code>
.sb-search {
    position: relative;
    margin-top: 10px;
    width: 0%;
    min-width: 60px;
    height: 60px;
    float: right;
    overflow: hidden;
 
    -webkit-transition: width 0.3s;
    -moz-transition: width 0.3s;
    transition: width 0.3s;
 
    -webkit-backface-visibility: hidden;
}
</code>
</pre>

现在该隐藏的隐藏，改出现的也出现啦。接下来就是定义输入框了的样式了。

首先我们要定义输入框的宽度，这样当它的父级元素宽度变化时，它才能跟着适应父级元素的宽度而变化。用padding确保文字居中。其它的字体、颜色设置就不详细解释了。看代码就一目了然了。

<pre>
<code>
.sb-search-input {
    position: absolute;
    top: 0;
    right: 0;
    border: none;
    outline: none;
    background: #fff;
    width: 100%;
    height: 60px;
    margin: 0;
    z-index: 10;
    padding: 20px 65px 20px 20px;
    font-family: inherit;
    font-size: 20px;
    color: #2c3e50;
}
 
input[type="search"].sb-search-input {
    -webkit-appearance: none;
    -webkit-border-radius: 0px;
}
</code>
</pre>

我们还去掉了webkit内核浏览器为input设置的一些默认样式。

我们在html中用placeholder这个占位符的属性，当然我们也可以定义占位符文字的样式。

<pre>
<code>
.sb-search-input::-webkit-input-placeholder {
    color: #efb480;
}
 
.sb-search-input:-moz-placeholder {
    color: #efb480;
}
 
.sb-search-input::-moz-placeholder {
    color: #efb480;
}
 
.sb-search-input:-ms-input-placeholder {
    color: #efb480;
}
</code>
</pre>

接下来，是按钮的部分，当然原文是用font-face来制作图片的，我这里为了方便就不用字体了，用文字来表示，如果用文字的话那结构就可以更简洁写，当然font-face确实是一个很神奇的东东，可以制作很多我们原先只能用图片才能达到的效果，这个得另说了。

<pre>
<code>
.sb-icon-search,
.sb-search-submit  {
    width: 60px;
    height: 60px;
    display: block;
    position: absolute;
    right: 0;
    top: 0;
    padding: 0;
    margin: 0;
    line-height: 60px;
    text-align: center;
    cursor: pointer;
}
</code>
</pre>

为了使按钮和提交input在同一个地方，我们要用绝对定位和一样的宽高属性。如上代码所示。

当然我们为了在点击按钮图标的时候同时出发提交按钮的点击，所以我们设置提交按钮的层级为-1和颜色为透明。这样我们就能让按钮显示在提交按钮的上面，同时达到出发提交按钮的点击效果。

<pre>
<code>
.sb-search-submit {
    background: #fff; /* IE needs this */
    -ms-filter: "progid:DXImageTransform.Microsoft.Alpha(Opacity=0)"; /* IE 8 */
    filter: alpha(opacity=0); /* IE 5-7 */
    opacity: 0;
    color: transparent;
    border: none;
    outline: none;
    z-index: -1;
}
</code>
</pre>

这里你可能就要问了，为什么不直接设置背景色为透明的呢？是因为在ie下这样设置，按钮就不能点击了。所以我们就设置背景颜色为白色以及透明度为0来达到透明的效果。

我们的按钮是用文字来代替的，直接设置value就可以了，但是为了又好的扩展性，比如用图片代替文字等等。我们这里用了伪类来生成我们需要的内容。

<pre>
<code>
.sb-icon-search {
    color: #fff;
    background: #e67e22;
    z-index: 90;
    font-size: 22px;
    font-family: 'icomoon';
    speak: none;
    font-style: normal;
    font-weight: normal;
    font-variant: normal;
    text-transform: none;
    -webkit-font-smoothing: antialiased;
}
 
.sb-icon-search:before {
    content: "搜索";
}
</code>
</pre>

好了，到这里。样式定义的差不多了。由于我们这个是配合javascript来实现的效果，就要考虑到当用户禁用js或者是其它js失效的情况下，我们就要设置输入框默认是显示的，只有js生效的情况下，输入框才隐藏。

<pre>
<code>
.sb-search.sb-search-open,
.no-js .sb-search {
    width: 100%;
}
</code>
</pre>

还要设置下，当输入框显示的时候，按钮的样。以及提交按钮的层级属性，这样才能触发它的点击。

<pre>
<code>
.sb-search.sb-search-open .sb-icon-search,
.no-js .sb-search .sb-icon-search {
    background: #da6d0d;
    color: #fff;
    z-index: 11;
}
.sb-search.sb-search-open .sb-search-submit,
.no-js .sb-search .sb-search-submit {
    z-index: 90;
}
</code>
</pre>

###JAVASCRIPT

接下来就是javascript部分了，我们定义了sb-search-open这个类，来控制输入框的显示和隐藏。配合javascript就是当我们点击的时候，来回添加和删除sb-search-open这个类。这样就能实现显示隐藏输入框的效果了。

当然这里要注意的一点的是要阻止冒泡事件，就是当我们点击提交按钮的时候，我们不能让它的点击事件传递给它的父级元素。

为了使我们的代码更好的维护和模块化，我们采用一种很简单的方法来组织我们的代码，就是立即执行函数表达式。

这里说明下，这个例子中用到classie这个小类库，因为没有使用jquery，也可以使javascript支持像添加calss、删除class以及检测是否有class这些方法，这个类库的地址在[这里](https://github.com/desandro/classie)。

<pre>
<code>
;( function( window ) {
     
    function UISearch( el, options ) {  
        this.el = el;
        this.inputEl = el.querySelector( 'form > input.sb-search-input' );
        this._initEvents();
    }
 
    UISearch.prototype = {
        _initEvents : function() {
            var self = this,
                initSearchFn = function( ev ) {
                    if( !classie.has( self.el, 'sb-search-open' ) ) { // open it
                        ev.preventDefault();
                        self.open();
                    }
                    else if( classie.has( self.el, 'sb-search-open' ) && /^\s*$/.test( self.inputEl.value ) ) { // close it
                        self.close();
                    }
                }
 
            this.el.addEventListener( 'click', initSearchFn );
            this.inputEl.addEventListener( 'click', function( ev ) { ev.stopPropagation(); });
        },
        open : function() {
            classie.add( this.el, 'sb-search-open' );
        },
        close : function() {
            classie.remove( this.el, 'sb-search-open' );
        }
    }
 
    // add to global namespace
    window.UISearch = UISearch;
 
} )( window );
</code>
</pre>

在上面的代码中，我们写了open和close两个方法来控制当我们点击按钮的时候添加sb-search-open这个类，使输入框显示和隐藏。当然我们还要添加当我们点击除了搜索框以外其它地方的时候，也要关闭输入框的事件。我们把它放到open方法中。

<pre>
<code>
;( function( window ) {
		...
        open : function() {
            var self = this;
            classie.add( this.el, 'sb-search-open' );
            // close the search input if body is clicked
            var bodyFn = function( ev ) {
                self.close();
                this.removeEventListener( 'click', bodyFn );
            };
            document.addEventListener( 'click', bodyFn );
        },
		....
 
} )( window );
</code>
</pre>

还有我们想搜索框显示的时候，就自动获得焦点，以及在移动设备上阻止当input获得焦点的时候键盘弹出这种情形。还有当隐藏输入框的时候，移除焦点，这样是为了防止一些移动设别在input隐藏的时候，出现光标还在闪烁的情况。

<pre>
<code>
;( function( window ) {
 
    // http://stackoverflow.com/a/11381730/989439 检测移动设备
    function mobilecheck() {
        var check = false;
        (function(a){if(/(android|ipad|playbook|silk|bb\d+|meego).+mobile|avantgo|bada\/|blackberry|blazer|compal|elaine|fennec|hiptop|iemobile|ip(hone|od)|iris|kindle|lge |maemo|midp|mmp|netfront|opera m(ob|in)i|palm( os)?|phone|p(ixi|re)\/|plucker|pocket|psp|series(4|6)0|symbian|treo|up\.(browser|link)|vodafone|wap|windows (ce|phone)|xda|xiino/i.test(a)||/1207|6310|6590|3gso|4thp|50[1-6]i|770s|802s|a wa|abac|ac(er|oo|s\-)|ai(ko|rn)|al(av|ca|co)|amoi|an(ex|ny|yw)|aptu|ar(ch|go)|as(te|us)|attw|au(di|\-m|r |s )|avan|be(ck|ll|nq)|bi(lb|rd)|bl(ac|az)|br(e|v)w|bumb|bw\-(n|u)|c55\/|capi|ccwa|cdm\-|cell|chtm|cldc|cmd\-|co(mp|nd)|craw|da(it|ll|ng)|dbte|dc\-s|devi|dica|dmob|do(c|p)o|ds(12|\-d)|el(49|ai)|em(l2|ul)|er(ic|k0)|esl8|ez([4-7]0|os|wa|ze)|fetc|fly(\-|_)|g1 u|g560|gene|gf\-5|g\-mo|go(\.w|od)|gr(ad|un)|haie|hcit|hd\-(m|p|t)|hei\-|hi(pt|ta)|hp( i|ip)|hs\-c|ht(c(\-| |_|a|g|p|s|t)|tp)|hu(aw|tc)|i\-(20|go|ma)|i230|iac( |\-|\/)|ibro|idea|ig01|ikom|im1k|inno|ipaq|iris|ja(t|v)a|jbro|jemu|jigs|kddi|keji|kgt( |\/)|klon|kpt |kwc\-|kyo(c|k)|le(no|xi)|lg( g|\/(k|l|u)|50|54|\-[a-w])|libw|lynx|m1\-w|m3ga|m50\/|ma(te|ui|xo)|mc(01|21|ca)|m\-cr|me(rc|ri)|mi(o8|oa|ts)|mmef|mo(01|02|bi|de|do|t(\-| |o|v)|zz)|mt(50|p1|v )|mwbp|mywa|n10[0-2]|n20[2-3]|n30(0|2)|n50(0|2|5)|n7(0(0|1)|10)|ne((c|m)\-|on|tf|wf|wg|wt)|nok(6|i)|nzph|o2im|op(ti|wv)|oran|owg1|p800|pan(a|d|t)|pdxg|pg(13|\-([1-8]|c))|phil|pire|pl(ay|uc)|pn\-2|po(ck|rt|se)|prox|psio|pt\-g|qa\-a|qc(07|12|21|32|60|\-[2-7]|i\-)|qtek|r380|r600|raks|rim9|ro(ve|zo)|s55\/|sa(ge|ma|mm|ms|ny|va)|sc(01|h\-|oo|p\-)|sdk\/|se(c(\-|0|1)|47|mc|nd|ri)|sgh\-|shar|sie(\-|m)|sk\-0|sl(45|id)|sm(al|ar|b3|it|t5)|so(ft|ny)|sp(01|h\-|v\-|v )|sy(01|mb)|t2(18|50)|t6(00|10|18)|ta(gt|lk)|tcl\-|tdg\-|tel(i|m)|tim\-|t\-mo|to(pl|sh)|ts(70|m\-|m3|m5)|tx\-9|up(\.b|g1|si)|utst|v400|v750|veri|vi(rg|te)|vk(40|5[0-3]|\-v)|vm40|voda|vulc|vx(52|53|60|61|70|80|81|83|85|98)|w3c(\-| )|webc|whit|wi(g |nc|nw)|wmlb|wonu|x700|yas\-|your|zeto|zte\-/i.test(a.substr(0,4)))check = true})(navigator.userAgent||navigator.vendor||window.opera);
        return check;
    }
 
    // http://www.jonathantneal.com/blog/polyfills-and-prototypes/  格式化字符串
    !String.prototype.trim && (String.prototype.trim = function() {
        return this.replace(/^\s+|\s+$/g, '');
    });
     
    function UISearch( el, options ) {  
        this.el = el;
        this.inputEl = el.querySelector( 'form > input.sb-search-input' );
        this._initEvents();
    }
 
    UISearch.prototype = {
        _initEvents : function() {
            var self = this,
                initSearchFn = function( ev ) {
                    ev.stopPropagation();
                    // trim its value
                    self.inputEl.value = self.inputEl.value.trim();
                     
                    if( !classie.has( self.el, 'sb-search-open' ) ) { // open it
                        ev.preventDefault();
                        self.open();
                    }
                    else if( classie.has( self.el, 'sb-search-open' ) && /^\s*$/.test( self.inputEl.value ) ) { // close it
                        self.close();
                    }
                }
 
            this.el.addEventListener( 'click', initSearchFn );
            this.inputEl.addEventListener( 'click', function( ev ) { ev.stopPropagation(); });
        },
        open : function() {
            var self = this;
            classie.add( this.el, 'sb-search-open' );
            // focus the input
            if( !mobilecheck() ) {
                this.inputEl.focus();
            }
            // close the search input if body is clicked
            var bodyFn = function( ev ) {
                self.close();
                this.removeEventListener( 'click', bodyFn );
            };
            document.addEventListener( 'click', bodyFn );
        },
        close : function() {
            this.inputEl.blur();
            classie.remove( this.el, 'sb-search-open' );
        }
    }
 
    // add to global namespace
    window.UISearch = UISearch;
 
} )( window );
</code>
</pre>

为了适应移动设备上的情况，我们需要支持移动设备的触摸事件。

<pre>
<code>
;( function( window ) {
     
    // http://stackoverflow.com/a/11381730/989439
    function mobilecheck() {
        var check = false;
        (function(a){if(/(android|ipad|playbook|silk|bb\d+|meego).+mobile|avantgo|bada\/|blackberry|blazer|compal|elaine|fennec|hiptop|iemobile|ip(hone|od)|iris|kindle|lge |maemo|midp|mmp|netfront|opera m(ob|in)i|palm( os)?|phone|p(ixi|re)\/|plucker|pocket|psp|series(4|6)0|symbian|treo|up\.(browser|link)|vodafone|wap|windows (ce|phone)|xda|xiino/i.test(a)||/1207|6310|6590|3gso|4thp|50[1-6]i|770s|802s|a wa|abac|ac(er|oo|s\-)|ai(ko|rn)|al(av|ca|co)|amoi|an(ex|ny|yw)|aptu|ar(ch|go)|as(te|us)|attw|au(di|\-m|r |s )|avan|be(ck|ll|nq)|bi(lb|rd)|bl(ac|az)|br(e|v)w|bumb|bw\-(n|u)|c55\/|capi|ccwa|cdm\-|cell|chtm|cldc|cmd\-|co(mp|nd)|craw|da(it|ll|ng)|dbte|dc\-s|devi|dica|dmob|do(c|p)o|ds(12|\-d)|el(49|ai)|em(l2|ul)|er(ic|k0)|esl8|ez([4-7]0|os|wa|ze)|fetc|fly(\-|_)|g1 u|g560|gene|gf\-5|g\-mo|go(\.w|od)|gr(ad|un)|haie|hcit|hd\-(m|p|t)|hei\-|hi(pt|ta)|hp( i|ip)|hs\-c|ht(c(\-| |_|a|g|p|s|t)|tp)|hu(aw|tc)|i\-(20|go|ma)|i230|iac( |\-|\/)|ibro|idea|ig01|ikom|im1k|inno|ipaq|iris|ja(t|v)a|jbro|jemu|jigs|kddi|keji|kgt( |\/)|klon|kpt |kwc\-|kyo(c|k)|le(no|xi)|lg( g|\/(k|l|u)|50|54|\-[a-w])|libw|lynx|m1\-w|m3ga|m50\/|ma(te|ui|xo)|mc(01|21|ca)|m\-cr|me(rc|ri)|mi(o8|oa|ts)|mmef|mo(01|02|bi|de|do|t(\-| |o|v)|zz)|mt(50|p1|v )|mwbp|mywa|n10[0-2]|n20[2-3]|n30(0|2)|n50(0|2|5)|n7(0(0|1)|10)|ne((c|m)\-|on|tf|wf|wg|wt)|nok(6|i)|nzph|o2im|op(ti|wv)|oran|owg1|p800|pan(a|d|t)|pdxg|pg(13|\-([1-8]|c))|phil|pire|pl(ay|uc)|pn\-2|po(ck|rt|se)|prox|psio|pt\-g|qa\-a|qc(07|12|21|32|60|\-[2-7]|i\-)|qtek|r380|r600|raks|rim9|ro(ve|zo)|s55\/|sa(ge|ma|mm|ms|ny|va)|sc(01|h\-|oo|p\-)|sdk\/|se(c(\-|0|1)|47|mc|nd|ri)|sgh\-|shar|sie(\-|m)|sk\-0|sl(45|id)|sm(al|ar|b3|it|t5)|so(ft|ny)|sp(01|h\-|v\-|v )|sy(01|mb)|t2(18|50)|t6(00|10|18)|ta(gt|lk)|tcl\-|tdg\-|tel(i|m)|tim\-|t\-mo|to(pl|sh)|ts(70|m\-|m3|m5)|tx\-9|up(\.b|g1|si)|utst|v400|v750|veri|vi(rg|te)|vk(40|5[0-3]|\-v)|vm40|voda|vulc|vx(52|53|60|61|70|80|81|83|85|98)|w3c(\-| )|webc|whit|wi(g |nc|nw)|wmlb|wonu|x700|yas\-|your|zeto|zte\-/i.test(a.substr(0,4)))check = true})(navigator.userAgent||navigator.vendor||window.opera);
        return check;
    }
     
    // http://www.jonathantneal.com/blog/polyfills-and-prototypes/
    !String.prototype.trim && (String.prototype.trim = function() {
        return this.replace(/^\s+|\s+$/g, '');
    });
 
    function UISearch( el, options ) {  
        this.el = el;
        this.inputEl = el.querySelector( 'form > input.sb-search-input' );
        this._initEvents();
    }
 
    UISearch.prototype = {
        _initEvents : function() {
            var self = this,
                initSearchFn = function( ev ) {
                    ev.stopPropagation();
                    // trim its value
                    self.inputEl.value = self.inputEl.value.trim();
                     
                    if( !classie.has( self.el, 'sb-search-open' ) ) { // open it
                        ev.preventDefault();
                        self.open();
                    }
                    else if( classie.has( self.el, 'sb-search-open' ) && /^\s*$/.test( self.inputEl.value ) ) { // close it
                        ev.preventDefault();
                        self.close();
                    }
                }
 
            this.el.addEventListener( 'click', initSearchFn );
            this.el.addEventListener( 'touchstart', initSearchFn );
            this.inputEl.addEventListener( 'click', function( ev ) { ev.stopPropagation(); });
            this.inputEl.addEventListener( 'touchstart', function( ev ) { ev.stopPropagation(); } );
        },
        open : function() {
            var self = this;
            classie.add( this.el, 'sb-search-open' );
            // focus the input
            if( !mobilecheck() ) {
                this.inputEl.focus();
            }
            // close the search input if body is clicked
            var bodyFn = function( ev ) {
                self.close();
                this.removeEventListener( 'click', bodyFn );
                this.removeEventListener( 'touchstart', bodyFn );
            };
            document.addEventListener( 'click', bodyFn );
            document.addEventListener( 'touchstart', bodyFn );
        },
        close : function() {
            this.inputEl.blur();
            classie.remove( this.el, 'sb-search-open' );
        }
    }
 
    // add to global namespace
    window.UISearch = UISearch;
 
} )( window );
</code>
</pre>

当然不是所有的浏览器都支持 addEventListener 和 removeEventListener这两个绑定和移除事件的方法，我们这里用到的是 Jonathan Neal’s 写的[EventListener]这个封装好的方法，最终的javascript代码如下。

<pre>
<code>
;( function( window ) {
	
	'use strict';
	
	// EventListener | @jon_neal | //github.com/jonathantneal/EventListener
	!window.addEventListener && window.Element && (function () {
	   function addToPrototype(name, method) {
		  Window.prototype[name] = HTMLDocument.prototype[name] = Element.prototype[name] = method;
	   }
	 
	   var registry = [];
	 
	   addToPrototype("addEventListener", function (type, listener) {
		  var target = this;
	 
		  registry.unshift({
			 __listener: function (event) {
				event.currentTarget = target;
				event.pageX = event.clientX + document.documentElement.scrollLeft;
				event.pageY = event.clientY + document.documentElement.scrollTop;
				event.preventDefault = function () { event.returnValue = false };
				event.relatedTarget = event.fromElement || null;
				event.stopPropagation = function () { event.cancelBubble = true };
				event.relatedTarget = event.fromElement || null;
				event.target = event.srcElement || target;
				event.timeStamp = +new Date;
	 
				listener.call(target, event);
			 },
			 listener: listener,
			 target: target,
			 type: type
		  });
	 
		  this.attachEvent("on" + type, registry[0].__listener);
	   });
	 
	   addToPrototype("removeEventListener", function (type, listener) {
		  for (var index = 0, length = registry.length; index < length; ++index) {
			 if (registry[index].target == this && registry[index].type == type && registry[index].listener == listener) {
				return this.detachEvent("on" + type, registry.splice(index, 1)[0].__listener);
			 }
		  }
	   });
	 
	   addToPrototype("dispatchEvent", function (eventObject) {
		  try {
			 return this.fireEvent("on" + eventObject.type, eventObject);
		  } catch (error) {
			 for (var index = 0, length = registry.length; index < length; ++index) {
				if (registry[index].target == this && registry[index].type == eventObject.type) {
				   registry[index].call(this, eventObject);
				}
			 }
		  }
	   });
	})();

	// http://stackoverflow.com/a/11381730/989439
	function mobilecheck() {
		var check = false;
		(function(a){if(/(android|ipad|playbook|silk|bb\d+|meego).+mobile|avantgo|bada\/|blackberry|blazer|compal|elaine|fennec|hiptop|iemobile|ip(hone|od)|iris|kindle|lge |maemo|midp|mmp|netfront|opera m(ob|in)i|palm( os)?|phone|p(ixi|re)\/|plucker|pocket|psp|series(4|6)0|symbian|treo|up\.(browser|link)|vodafone|wap|windows (ce|phone)|xda|xiino/i.test(a)||/1207|6310|6590|3gso|4thp|50[1-6]i|770s|802s|a wa|abac|ac(er|oo|s\-)|ai(ko|rn)|al(av|ca|co)|amoi|an(ex|ny|yw)|aptu|ar(ch|go)|as(te|us)|attw|au(di|\-m|r |s )|avan|be(ck|ll|nq)|bi(lb|rd)|bl(ac|az)|br(e|v)w|bumb|bw\-(n|u)|c55\/|capi|ccwa|cdm\-|cell|chtm|cldc|cmd\-|co(mp|nd)|craw|da(it|ll|ng)|dbte|dc\-s|devi|dica|dmob|do(c|p)o|ds(12|\-d)|el(49|ai)|em(l2|ul)|er(ic|k0)|esl8|ez([4-7]0|os|wa|ze)|fetc|fly(\-|_)|g1 u|g560|gene|gf\-5|g\-mo|go(\.w|od)|gr(ad|un)|haie|hcit|hd\-(m|p|t)|hei\-|hi(pt|ta)|hp( i|ip)|hs\-c|ht(c(\-| |_|a|g|p|s|t)|tp)|hu(aw|tc)|i\-(20|go|ma)|i230|iac( |\-|\/)|ibro|idea|ig01|ikom|im1k|inno|ipaq|iris|ja(t|v)a|jbro|jemu|jigs|kddi|keji|kgt( |\/)|klon|kpt |kwc\-|kyo(c|k)|le(no|xi)|lg( g|\/(k|l|u)|50|54|\-[a-w])|libw|lynx|m1\-w|m3ga|m50\/|ma(te|ui|xo)|mc(01|21|ca)|m\-cr|me(rc|ri)|mi(o8|oa|ts)|mmef|mo(01|02|bi|de|do|t(\-| |o|v)|zz)|mt(50|p1|v )|mwbp|mywa|n10[0-2]|n20[2-3]|n30(0|2)|n50(0|2|5)|n7(0(0|1)|10)|ne((c|m)\-|on|tf|wf|wg|wt)|nok(6|i)|nzph|o2im|op(ti|wv)|oran|owg1|p800|pan(a|d|t)|pdxg|pg(13|\-([1-8]|c))|phil|pire|pl(ay|uc)|pn\-2|po(ck|rt|se)|prox|psio|pt\-g|qa\-a|qc(07|12|21|32|60|\-[2-7]|i\-)|qtek|r380|r600|raks|rim9|ro(ve|zo)|s55\/|sa(ge|ma|mm|ms|ny|va)|sc(01|h\-|oo|p\-)|sdk\/|se(c(\-|0|1)|47|mc|nd|ri)|sgh\-|shar|sie(\-|m)|sk\-0|sl(45|id)|sm(al|ar|b3|it|t5)|so(ft|ny)|sp(01|h\-|v\-|v )|sy(01|mb)|t2(18|50)|t6(00|10|18)|ta(gt|lk)|tcl\-|tdg\-|tel(i|m)|tim\-|t\-mo|to(pl|sh)|ts(70|m\-|m3|m5)|tx\-9|up(\.b|g1|si)|utst|v400|v750|veri|vi(rg|te)|vk(40|5[0-3]|\-v)|vm40|voda|vulc|vx(52|53|60|61|70|80|81|83|85|98)|w3c(\-| )|webc|whit|wi(g |nc|nw)|wmlb|wonu|x700|yas\-|your|zeto|zte\-/i.test(a.substr(0,4)))check = true})(navigator.userAgent||navigator.vendor||window.opera);
		return check;
	}
	
	// http://www.jonathantneal.com/blog/polyfills-and-prototypes/
	!String.prototype.trim && (String.prototype.trim = function() {
		return this.replace(/^\s+|\s+$/g, '');
	});

	function UISearch( el, options ) {	
		this.el = el;
		this.inputEl = el.querySelector( 'form > input.sb-search-input' );
		this._initEvents();
	}

	UISearch.prototype = {
		_initEvents : function() {
			var self = this,
				initSearchFn = function( ev ) {
					ev.stopPropagation();
					// trim its value
					self.inputEl.value = self.inputEl.value.trim();
					
					if( !classie.has( self.el, 'sb-search-open' ) ) { // open it
						ev.preventDefault();
						self.open();
					}
					else if( classie.has( self.el, 'sb-search-open' ) && /^\s*$/.test( self.inputEl.value ) ) { // close it
						ev.preventDefault();
						self.close();
					}
				}

			this.el.addEventListener( 'click', initSearchFn );
			this.el.addEventListener( 'touchstart', initSearchFn );
			this.inputEl.addEventListener( 'click', function( ev ) { ev.stopPropagation(); });
			this.inputEl.addEventListener( 'touchstart', function( ev ) { ev.stopPropagation(); } );
		},
		open : function() {
			var self = this;
			classie.add( this.el, 'sb-search-open' );
			// focus the input
			if( !mobilecheck() ) {
				this.inputEl.focus();
			}
			// close the search input if body is clicked
			var bodyFn = function( ev ) {
				self.close();
				this.removeEventListener( 'click', bodyFn );
				this.removeEventListener( 'touchstart', bodyFn );
			};
			document.addEventListener( 'click', bodyFn );
			document.addEventListener( 'touchstart', bodyFn );
		},
		close : function() {
			this.inputEl.blur();
			classie.remove( this.el, 'sb-search-open' );
		}
	}

	// add to global namespace
	window.UISearch = UISearch;

} )( window );
</code>
</pre>

运行效果可以去这[看看](http://tympanus.net/Tutorials/ExpandingSearchBar/)。

