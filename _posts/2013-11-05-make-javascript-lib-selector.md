---
layout: post
title: javascript学习打造javascript框架系列-选择器
categories:
- Life
tags:
- javascript
---

今天来谈谈选择器部分，首先还是来看一下流行第三方库的选择器实现的特点。

对于一个库来说，选择器的API的浏览器的兼容性是非常重要的。

无论是用 XPath或者CSS风格的选择器，浏览器的实现各不一样。

首先我们来看看用javascript原生语法是如何来选择元素的：

    document.all    
	document.getElementById('navigation');

看到这长长的名字后，我们是不是可以缩短它呢？我们看看Prototype是如何来做的，只需*$()*一个简短的语法我们就可以办到：

    function $(element) {
	  if (arguments.length > 1) {
	    for (var i = 0, elements = [], length = arguments.length; i < length; i++)
	      elements.push($(arguments[i]));
	    return elements;
	  }
	  if (Object.isString(element))
	    element = document.getElementById(element);
	  return Element.extend(element);
	}

这比*document.getElementById*简单多了，也用容易记住。

像诸如*getElementsByClassName*, *getElementsByName*这些长长的选择器等，还有其它的一些DOM相关的方法，不易书写又难记。

现代浏览器也支持[ querySelector and querySelectorAll](http://www.w3.org/TR/selectors-api/)新的选择器的特征，当然在老版本的IE上就不支持了。

正是由于浏览器对于DOM选择器的支持不但兼容性问题一大堆而且使用起来也不是很友好。所以现在第三方的库中，都有实现扩展选择器这一部分，增强浏览器的兼容性，扩展一些有用的功能。

正如此，产生了很多选择器的引擎，比如鼎鼎大名的*Sizzle*：

    // Check to see if the browser returns elements by name when
	// querying by getElementById (and provide a workaround)
	(function(){
	  // We're going to inject a fake input element with a specified name
	  var form = document.createElement("div"),
	  id = "script" + (new Date()).getTime();
	  form.innerHTML = "<a name='" + id + "'/>";
	   
	  // Inject it into the root element, check its status, and remove it quickly
	  var root = document.documentElement;
	  root.insertBefore( form, root.firstChild );

由于不是是在原生上扩展的一些方法，那么对性能还是有很高的要求的。

对于这个，*Sizzle*是用浏览器能力来检测的，当浏览器支持原生功能的时候，就用原生的功能比如*querySelectorAll*。不支持则采用自己实现的方法来实现。

另外一个，尽量利用缓存来提高性能。*Sizzle*和*Prototype*都使用了缓存功能。

另外一个来检测性能的途径是，善用各种工具来检测。比如[slickspeed](http://github.com/kamicane/slickspeed)或者是[Woosh](http://github.com/jakearchibald/Woosh/)。这些工具都非常有用，对于测试结果你还是仔细斟酌。像对于Prototy和MoolTools这样的全功能的库来说，性能上跟只专注于DOM选择器Sizzle这样的库肯定是要慢点。

像Sizzle这样的只专注于Dom选择器的库还有很多，比如[Sly](http://github.com/digitarald/sly)以及[Peppy](http://github.com/jdonaghue/Peppy)。

一般来说，选择器的API风格设计有两种风格。一种是用一个函数来返回相匹配的元素，但是它们要包装在一个特殊的包装类中。而且这个包装器可以用来执行复杂的DOM操作以及链式调用。比如jQuery,Dojo和Glow就是用这样一种方法来设计它们的API的。

jQuery提供了一个*$()*包装器，能够用来选择任何的DOM元素节点，还可以实现链式操作。

Dojo也有*dojo.query*方法，会返回一个像[ dojo.NodeList](http://www.dojotoolkit.org/reference-guide/dojo/NodeList.html#dojo-nodelist)数组一样的东东。Glow也差不多。

第二种方法是，选择元素根据用法的不同，有时候返回的可能就是数组，有时候又是元素对象。

比如Prototype有*$()*可以实现*getElementById*,而*$$()*又可以实现CSS或者是XPath风格的选择器。Prototype用它自己的方法来扩展元素的功能。MoolTools也差不多。

我们的Alien库秉持简单易用的原则适用Glow风格的API设计风格。比如：

    alien.dom.find('.class')                  // 选择反回匹配的元素
	.find('a')                                 // 选择a链接
	.css({ 'background-color': '#aabbcc' })    // 设置元素的CSS属性

为了是我们的选择器保持足够的简单，我们的DOM选择器有以下特征：

- 只支持CSS风格的选择
- 支持少部分的伪类选择器
- 尽可能使用原生的方法
- 缓存
- 修复浏览器的兼容问题

在分析代码前，有必要先来了解下现在W3C标准中，关于CSS选择器的规范。主要包括下面这些：

- *E* 元素选择器，匹配任何元素
- *E F* 后代选择器
- *.class* 类选择器
- *E.class* 匹配指定包含类的元素
- *#id* id选择器
- *E#id* 匹配指定包含id的元素 

那么浏览器在解析CSS样式的时候，是遵循什么样的规则的呢？可以看看这篇文章[Writing Efficient CSS](https://developer.mozilla.org/en/Writing_Efficient_CSS)。

浏览器解析样式的时候，主要是包含下面这四种类型：

- ID选择器
- 类选择器
- 元素标签
- 一般类型

根据上面的分析，我们可以这样来构建我们的选择器的方法：

    findMap = {
    'id': function(root, selector) {
    },

    'name and id': function(root, selector) {
    },

    'name': function(root, selector) {
    },

    'class': function(root, selector) {
    },

    'name and class': function(root, selector) {
    }
  	};

### 词法分析 ###

词法分析简而言之就是把传入的字符串分别归类。比如我们的选择器，我们就要实现以下的词法分析：

- 去掉字符串前后的空格
- 把得到的字符串转化成我们查询器可以识别的字符
- 然后通过字符串，执行我们的DOM命令

比如我们可以这样做：

    function Token(identity, finder) {
	  this.identity = identity;
	  this.finder   = finder;
	}
	
	Token.prototype.toString = function() {
	  return 'identity: ' + this.identity + ', finder: ' + this.finder;
	};

*finder*参数是我们在*findMap*里定义的选择器的类型，而*identity*是基本规则。

### 匹配元素 ###

想[Sly](http://github.com/digitarald/sly)和[Sizzle](http://sizzlejs.com/)这两个选择器引擎都是用正则表达式来实现字符串匹配的，一般称之为*字符串扫描*。

在javascript中用正则表达式能够实现强大的功能，特别是对于字符串匹配这类工作来说，更是如此。但是正则表达式也有一个缺陷，就是可读性比较差。所以在我们的DOM选择器的构建中，不打算用正则表达式这一方法。

大多数编程语言都有一个基于词汇的词法分析器。它们的词法分析就是靠这个词法分析器来实现的。

现在一般的编程语言都会提供一个词法分析器，但是我们面对的是每天都会产生大量数据的互联网世界。词法分析器也会与时俱进，比如在一个Ruby Html和XML提供解析器[nokogiri](http://nokogiri.org/)中我们就可以看到很多的词法解析。

词法解析器强大的功能在于它为程序员和解析器之间提供了一个抽象层的接口。依赖于这些接口我们可以不用关心内部实现细节，只要调用相关接口就行了。

下面就让我们用创建一个简单的词法解析器。这个词法解析器是以[ CSS grammer specification](http://www.w3.org/TR/CSS2/grammar.html)为基础的。

如下：

    macros = {
	  'nl':        '\n|\r\n|\r|\f',
	  'nonascii':  '[^\0-\177]',
	  'unicode':   '\\[0-9A-Fa-f]{1,6}(\r\n|[\s\n\r\t\f])?',
	  'escape':    '#{unicode}|\\[^\n\r\f0-9A-Fa-f]',
	  'nmchar':    '[_A-Za-z0-9-]|#{nonascii}|#{escape}',
	  'nmstart':   '[_A-Za-z]|#{nonascii}|#{escape}',
	  'ident':     '[-@]?(#{nmstart})(#{nmchar})*',
	  'name':      '(#{nmchar})+'
	};
	
	rules = {
	  'id and name':    '(#{ident}##{ident})',
	  'id':             '(##{ident})',
	  'class':          '(\\.#{ident})',
	  'name and class': '(#{ident}\\.#{ident})',
	  'element':        '(#{ident})',
	  'pseudo class':   '(:#{ident})'
	};

上面主要是包括以下内容：

- 把规则包裹到*macros*中
- 规则rules是以macros为基础的
- 用反斜杠转义
- 主要的规则是用正则表达式来做的

当然这里的词法解析器也是像Sizzle那样用正则表达式来做的。这样做的的好处是可以清楚地明白词法解析器和DOM匹配之间的关系。

当一个选择器初始化后，它会传入词法解析器中进行解析匹配。如下：

    while (match = r.exec(this.selector)) {
	  finder = null;
	
	  if (match[10]) {
	    finder = 'id';
	  } else if (match[1]) {
	    finder = 'name and id';
	  } else if (match[29]) {
	    finder = 'name';
	  } else if (match[15]) {
	    finder = 'class';
	  } else if (match[20]) {
	    finder = 'name and class';
	  }
	  this.tokens.push(new Token(match[0], finder));
	}

接下来我们继续分析如通过*Searcher*这个类来搜索DOM节点。

*Searcher*类通过分词器*Tokenizer*分词的结果来搜索匹配DOM节点。这里我们来看看Firefox浏览器是如何执行CSS的，我们的搜索类也是通过这个原理来搜索DOM节点的。

在CSS中，匹配CSS规则是这样的，从右到左即浏览器首先通过关键选择器过滤出所有符合规则的元素集合，然后再通过其他规则进行过滤。

*Searcher*这个类，会通过把搜索到节点和数组中规则进行实例化。数组中的最后一项是关键选择器。每一个数组都有一个类型值和选择类型，什么样的类型就决定用什么样的类型选择器，比如下面就是选择ID元素和CLASS元素的方法，如下：

    find = {
	  byId: function(root, id) {
	    return [root.getElementById(id)];
	  },
	
	  byNodeName: function(root, tagName) {
	    var i, results = [], nodes = root.getElementsByTagName(tagName);
	    for (i = 0; i < nodes.length; i++) {
	      results.push(nodes[i]);
	    }
	    return results;
	  },
	
	  byClassName: function(root, className) {
	    var i, results = [], nodes = root.getElementsByTagName('*');
	    for (i = 0; i < nodes.length; i++) {
	      if (nodes[i].className.match('\\b' + className + '\\b')) {
	        results.push(nodes[i]);
	      }
	    }
	    return results;
	  }
	};
	
	findMap = {
	  'id': function(root, selector) {
	    selector = selector.split('#')[1];
	    return find.byId(root, selector);
	  },
	
	  'name and id': function(root, selector) {
	    var matches = selector.split('#'), name, id;
	    name = matches[0];
	    id = matches[1];
	    return filter.byAttr(find.byId(root, id), 'nodeName', name.toUpperCase());
	  }
	
	  // ...
	};

上面的代码中，当浏览器支持原生的*getElementsByClassName*方法的时候，就用*getElementsByClassName*方法来搜索类；不支持的时候，就用我们自己的*byClassName*方法。

*Searcher*类通过find方法实现了一个对选择器引擎的抽象接口。

如下：

    Searcher.prototype.find = function(token) {
	  if (!findMap[token.finder]) {
	    throw new InvalidFinder('Invalid finder: ' + token.finder); 
	  }
	  return findMap[token.finder](this.root, token.identity); 
	};

当规则不存在的时候，会抛出一个异常。核心的方法是*matchesToken*，这个方法会检测一个节点是否符合规则：

    Searcher.prototype.matchesToken = function(element, token) {
	  if (!matchMap[token.finder]) {
	    throw new InvalidFinder('Invalid matcher: ' + token.finder); 
	  }
	  return matchMap[token.finder](element, token.identity);
	};

在matchesAllRules方法里，每一个规则(token)都会和指定节点列表进行匹配，while循环用来迭代整个节点树。

    while ((ancestor = ancestor.parentNode) && token) {
	  if (this.matchesToken(ancestor, token)) {
	    token = tokens.pop();
	  }
	}

这样查询DOM元素就非常简单了：

    Searcher.prototype.parse = function() {
	  // Find all elements with the key selector
	  var i, element, elements = this.find(this.key_selector), results = [];
	
	  // Traverse upwards from each element to see if it matches all of the rules
	  for (i = 0; i < elements.length; i++) {
	    element = elements[i];
	    if (this.matchesAllRules(element)) {
	      results.push(element);
	    }
	  }
	  return results;
	};

匹配查找的时候，将会议关键选择器为起点来查找。然后再访问它们的祖先节点，看是否匹配所有的规则。

当然这样查找DOM元素，性能上可能有点慢，但是可读性更好。

### 封装API ###

现在就要把我们实现的查询DOM方法封装成简单可用的接口。像下面这样：

    dom.tokenize = function(selector) {
	  var tokenizer = new Tokenizer(selector);
	  return tokenizer;
	};
	
	dom.get = function(selector) {
	  var tokens = dom.tokenize(selector).tokens,
	      searcher = new Searcher(document, tokens);
	  return searcher.parse();
	};

我们就可以通过调用dom.get进行查询，你提供一个选择器，然后返回一个元素数组。