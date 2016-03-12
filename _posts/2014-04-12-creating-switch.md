---
layout: post
title: 使用javascript和local storage来制作切换开关，提高用户体验
categories:
- Life
tags:
- javascript
- 翻译
---

允许用户能够自定义访问网站的方式是提高网站用户体验的一个方法。比如自定义网站的字体的尺寸、选择网站的语言地等等。对于开发者来说有两个地方要处理。第一是允许用户自定义一些网站的表现形式；第二个是要记住用户的选择，当他们下次再来访问的时候不需要再一次选择。这篇文章就来说说怎么样创建一个开关来自定义以及使用HTML5的**local storage**来记住用户的选择。

### 准备工作 ###

首先来创建一个基本的HTML文档和一个简单的样式。这里也创建了一个下拉框可以允许用户来选择自己喜欢的样式来访问网站。如下所示：

<iframe width="100%" height="300" src="http://jsfiddle.net/hibbard_eu/QBW79/embedded/result,js,html,css/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

### 事件绑定 ###

接下来就是下拉框中的事件绑定啦。需要实时监听用户的选择并作出反应。通过[W3C推荐的事件监听标准](http://www.w3.org/TR/2000/REC-DOM-Level-2-Events-20001113/events.html)，我们可以这样来绑定事件：

    element.addEventListener(<event-name>, <callback>, <use-capture>);

上面参数的意义是：

- event-name：表示要绑定事件的类型
- callba：表示当监听到事件触发时，要执行的具体的功能
- use-capture：这个布尔值参数是true，表示在捕获节点调用事件处理程序；如果是false，表四在冒泡节点调用事件处理程序

不过，IE8以及IE8以前的浏览器并不支持这个事件绑定模型，要使用**attachEvent()**这个方法来绑定事件。兼不兼容老版本的IE浏览器，这取决于你的目标用户的浏览器的环境。你还需要权衡一下兼容浏览器的成本和你具体的开发成本是否匹配来做出这个决定。当然我们也可以封装一个简单的事件处理函数来处理兼容性问题。下面的代码是用来监听**select**元素的**change**事件：

<iframe width="100%" height="300" src="http://jsfiddle.net/hibbard_eu/QBW79/1/embedded/js,result/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

在**switchStyles()**这个方法中，需要注意的是**this**是指向触发事件元素本身的，也就是**select()**元素。因此**this.option**则指向**option**元素，以及**this.options[this.selectedIndex]**会传递当前选中的**option**元素。通过**option**的**value**我们可以获得选中的值。

我们这里使用**console.log()**代替**alert()**来查看和测试脚本输出的值，因为使用**alert()**会阻塞UI的渲染。在这个例子中使用**alert()**方法还好，不过在一些复杂场景下，如循环遍历等场景下，使用**alert()**方法就可能显得非常不友好了。如果你对控制台不是很熟悉的话，你可以去[这篇文章](http://webmasters.stackexchange.com/questions/8525/how-to-open-the-javascript-console-in-different-browsers)看看详细的介绍，如果你想进一步了解关于javascript的debugging的话可以去[这篇文章](http://hibbard.eu/simple-javascript-debugging-with-chromes-console/)看看。

### 改变样式 ###

现在需要添加一些样式来响应用户的不同的选择。添加如下样式：

    /* High contrast */
	body.high-contrast{
	    background-color: DarkBlue;
	    color: yellow
	}
	 
	body.high-contrast h1{
	    text-shadow: none;
	}
	 
	/* Print */
	body.print h1{
	    text-shadow: none;
	}
	 
	body.print img{
	    display: none;
	}

当用户选择下拉框中不同的选项的时候，需要添加不同的类到body元素上：

    function switchStyles() {
	  var selectedOption = this.options[this.selectedIndex],
	      className = selectedOption.value;
	 
	  document.body.className = className;
	}

OK，一个简单的切换开关功能就完成了，你可以选择下拉框中不同的值看看会发生什么。如下所示：

<iframe width="100%" height="300" src="http://jsfiddle.net/hibbard_eu/QBW79/2/embedded/result,js,html,css" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

当然对于一个简单的样式这样也可以，但是如果要使用外链的方式来使用样式的话，我们可能需要改改我们的javascript代码了，代码如下所示：

    function switchStyles() {
	  var linkTag = document.getElementsByTagName('link')[0],
	      currentStylesheet = linkTag.href.replace(/^.*[\\\/]/, ''),
	      newStylesheet = this.options[this.selectedIndex].value + '.css';
	 
	  linkTag.href = linkTag.href.replace(currentStylesheet, newStylesheet);
	}

### 记住用户的选择 ###

接下来就是要处理记住用户的选择。不过这里有一个问题就是，当页面重新刷新载入的时候，页面又会回到初始的样式，并不会记住先前的选择。使用客户端浏览器本身的的数据存储器来记住用户的选择不失为一个好的解决方案。

至于客户端的数据存储方案，现在也有很多的选择。可以去看看[这篇文章](http://www.sitepoint.com/html5-browser-storage-past-present-future/)。我们这里会使用[local storage](http://www.sitepoint.com/an-overview-of-the-web-storage-api/)来保存用户的属性。local storage[大部分浏览器都支持](http://caniuse.com/namevalue-storage)，它允许我们保存5mb的数据、而且，不像cookies，保存在local storage中的数据不会转移到服务端。如果你想支持一些比较老旧的浏览器，[这里](https://github.com/Modernizr/Modernizr/wiki/HTML5-Cross-Browser-Polyfills#web-storage-localstorage-and-sessionstorage)也提供了兼容方案。当然需要注意的是在一些浏览器中，如IE8，并不支持**file://**这个规则。

下面是local storage一个语法示例：

    // is localStorage available?
	if (typeof window.localStorage != 'undefined') {
	    // store
	    localStorage.setItem('hello', 'Hello World!');
	  
	    // retrieve
	    console.log(localStorage.getItem('hello'));
	  
	    // delete
	    localStorage.removeItem('hello');
	}

回到我们的需求，我们需要实时保存下拉框中当前选中的值：

    localStorage.setItem('bodyClassName',className);

当然还需要检测页面在重新加载时的body的class，并把用户选择的值添加给body元素。首先我们需要得到当前body元素的class的值：

    // this will be null if not present
	var storedClassName = localStorage.getItem('bodyClassName');

如果得到当前的值，我们就可以循环遍历**select**元素的option的值，并比较存储在数据库中的值和当前select的值。如果匹配到值就可以把匹配到的值设置为下拉框的默认值。

    if (storedClassName) {
	  for(var i = 0; i < styleSwitcher.options.length; i++){
	    if (styleSwitcher.options[i].value === storedClassName){
	      styleSwitcher.selectedIndex = i;
	    }
	  }
	}

完整代码如下所示：

<iframe width="100%" height="300" src="http://jsfiddle.net/hibbard_eu/QBW79/3/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

现在我们可以选择一个样式并刷新页面，我们可以看到下拉框的默认值是我们上一次选择的样式的值。不过页面的样式并没有匹配到下拉框中的样式值。啥原因呢？

这是因为刷新页面的时候我们的**select**并没有执行绑定的事件。所以我们需要在页面加载的时候自动触发事件，下面的trigger方法就是用来自动触发事件的：

    function trigger(action, el) {
	  if (document.createEvent) {
	    var event = document.createEvent('HTMLEvents');
	 
	    event.initEvent(action, true, false);
	    el.dispatchEvent(event);
	  } else {
	    el.fireEvent('on' + action);
	  }    
	}

通过这个方法可以自动触发事件。这个方法会检测浏览器是否支持**document.createEvent()**这个方法来创建新的事件对象(大部分的遵循标准浏览器都支持这个方法)。如果支持，则使用**dispathEvent()**来触发事件。当然像IE老版本的浏览器则是使用**fireEvent()**来触发事件。关于这方面的可以去看看[这篇文章](http://www.sitepoint.com/javascript-custom-events/)。

最后我们通过下面这行代码来使用trigger方法来触发事件：

    trigger('change',styleWitcher);

最终代码如下：

<iframe width="100%" height="300" src="http://jsfiddle.net/hibbard_eu/QBW79/4/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

现在你去重新刷新页面，就会看到浏览器会正确的记住我们的选择。

通过这篇文章，我们学会允许用户选择自定义页面的表现形式。也学会了使用**local storage**来记住用户的选择，来提高网站的用户体验。正确的使用这个技术可以为我们的网站增色不少，特别是**local storage**。

[原文地址](http://www.sitepoint.com/creating-simple-style-switcher/)



