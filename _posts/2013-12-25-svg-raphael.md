---
layout: post
title: Raphaeljs使用指南
categories:
- Life
tags:
- svg
- Rapheal
- 翻译
---

> 前面有一系列的文章介绍了关于SVG的一些基础知识。这篇文章我们来介绍一下用js操纵SVG方面的知识，一些简单的图形当然不需要用到js。但在实际的开发中，不可能仅仅是插入一个svg图片这么简单，比如现在很多web交互中就涉及到和SVG交互或者是数据方面的交互这个时候javascript就派上用场了，现在也有很多关于SVG方面的js库，我们这里介绍的是目前用的比较广泛Raphaeljs这个库，它处理浏览器兼容方面的问题。原文是[An Introduction to the Raphael JS Library](http://net.tutsplus.com/tutorials/javascript-ajax/an-introduction-to-the-raphael-js-library/)。这篇文章是基于raphael1.0版本的，现在raphael已经更新到2.1版本，不过一些基本的用法和API没有什么变化这个就不用担心了。

[Raphael JS ](http://raphaeljs.com/)是一个轻量级的库，能够让你用javascript来在浏览器中绘制矢量图形。在本文中，我将会介绍raphael一些基本的知识和用法，最后完成实例的制作。

### 1.准备工作 ###

首先需要下载Raphael的源文件[这儿](http://raphaeljs.com/)。官网提供了压缩和完整两个版本下载。我建议在开发的时候使用完整版本，上线的时候使用压缩版本。

下载完后，新建一个HTML文件并在头部引入raphaeljs文件，以及我们自己要编写的js文件，然后在body标签里面新建一个id为**canvas——container**的div，这就是我们用来绘制图形的画布。

    <html>  
    <head>  
        <title>Raphael Play</title>  
        <script type="text/javascript" src="path/to/raphael.js"></script>  
        <script type="text/javascript" src="path/to/our_script.js"></script>  
        <style type="text/css">  
            #canvas_container {  
                width: 500px;  
                border: 1px solid #aaa;  
            }  
        </style>  
    </head>  
    <body>  
        <div id="canvas_container"></div>  
    </body>  
	</html>  

注意这里的raphael最好是引入官方的最新版本的文件，如果使用的版本是老版本最好去官方文档看看详细的升级日志。

### 2.创建画布 ###

当我们使用raphael来绘制矢量图形的时候，我们需要一个画布来绘制。我们使用Raphael提供Raphael()这个对象来创建我们的画布，并把这个画布赋予**paper**这个变量。针对这个画布，raphael也提供了很多的设置选项给我们来设置画布，比如画布的宽、高以及位置等属性。

    var paper = new Raphael(x, y, width, height); //option (a)  
	var paper = new Raphael(element, width, height); //option (b)  

我更喜欢使用(b)的方法来设置画布。接下来我们就创建一个画布，宽高都为500px，画布元素为上面创建的**canvas_container**元素：

    window.onload = function() {  
    var paper = new Raphael(document.getElementById('canvas_container'), 500, 500);  
	}  

创建好画布后，我们就可以在这个画布上随心所欲绘制图形了。

### 3.绘制图形 ###

现在就让我们来绘制一些简单的图形吧。首先在画布上默认图形的位置是x=0,y=0即在画布的左上角。这意味着我们可以指定x,y的值来指定图形的位置，这个位置就是相对画布的左上角来定位的。

首先我们来绘制一个圆，在js文件里面编写一下js代码：

    window.onload = function() {  
    var paper = new Raphael(document.getElementById('canvas_container'), 500, 500);  
    var circle = paper.circle(100, 100, 80);  
	}  
     
这段代码将会在x=100,y=100这个坐标点绘制一个半径为80px的圆。我们可以绘制任意数量的圆，js写一个循环就可以了如下所示：

    for(var i = 0; i < 5; i+=1) {  
    var multiplier = i*5;  
    paper.circle(250 + (2*multiplier), 100 + multiplier, 50 - multiplier);  
	}  

下一步，我们来画一个矩形。我们可以使用**rect()**这个方法来绘制矩形，它需要指定几个参数:位置的坐标点x、y和矩形的宽和高。

    var rectangle = paper.rect(200, 200, 250, 100);  

最后我们来绘制一个椭圆x轴的半径是100，y轴的半径是50，坐标点是x=200，y=400。js代码如下:

    window.onload = function() {  
    var paper = new Raphael(document.getElementById('canvas_container'), 500, 500);  
    var circle = paper.circle(100, 100, 80);  
    for(var i = 0; i < 5; i+=1) {  
        var multiplier = i*5;  
        paper.circle(250 + (2*multiplier), 100 + multiplier, 50 - multiplier)  
    }  
    var rectangle = paper.rect(200, 200, 250, 100);  
    var ellipse = paper.ellipse(200, 400, 100, 50);  
  
	}  

执行的效果如下：

<p data-height="268" data-theme-id="0" data-slug-hash="mDhvA" data-user="janily" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/mDhvA'>mDhvA</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a></p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

### 4.绘制路径 ###

内置的一些图形的绘制方法在绘制一些图形的时候确实很方便但是缺乏灵活性，而**path**就能使我们绘制图形的更加灵活。当使用路径的时候，我们可以这样想象，路径就相当于给了一支钢笔能够任意在画布上绘制图形，并且路径的初始位置也是画布的左上角。

比如，我们把路径移动到画布的中心。就需要我们在x轴和y轴移动250px。如下图所示：

![](http://pic.yupoo.com/reicky_v/Dps9fAQy/medium.jpg)

这就是所谓的路径啦。

路径是执行一系列的命令数字和值。比如我们要把路径移动到x=250和y=250的位置就应该使用下面的命令：

    "M 250 250"

'M'表示移动的意思，后面跟的数字表示移动位置的坐标点x和y。

现在我们把路径的初始的位置移动到指定的位置，让我们以这个位置为起点来绘制一条线段,绘制线段的命令是"L"：

    "M 250 250 l 0 -50"

这个命令表示向上画一条50px的线段。就用这个简单的命令来画一个俄罗斯方块的形状：

    "M 250 250 l 0 -50 l -50 0 l 0 -50 l -50 0 l 0 50 l -50 0 l 0 50 z" 

'z'命令表示闭合路径，对应我们前面开始的M的命令。

接下来就是调用Raphael提供的**path()**来绘制路径，编写下面的代码：

    window.onload = function() {  
    var paper = new Raphael(document.getElementById('canvas_container'), 500, 500);  
    var tetronimo = paper.path("M 250 250 l 0 -50 l -50 0 l 0 -50 l -50 0 l 0 50 l -50 0 l 0 50 z");  
	}  

结果如下：

<p data-height="268" data-theme-id="0" data-slug-hash="Diupd" data-user="janily" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/Diupd'>Diupd</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a></p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

路径如果配合曲线来使用的话那还是有点复杂的，可以去这个地址看看关于路径一个详细的介绍[SVG Path specification](http://www.w3.org/TR/SVG/paths.html#PathData)。

### 5.样式 ###

绘制好图形后，我们需要定义一些样式使它看起来更加舒服些。同样我们可以使用**attr()**方法。

attr()方法就像我们使用jQuery中的css方法一样接受属性和值这两个参数来编写样式。我们把路径存储在变量**tetronimo**中，然后就可以直接使用attr()方法来操纵这个变量，如下所示:

    window.onload = function() {  
    var paper = new Raphael(document.getElementById('canvas_container'), 500, 500);  
    var tetronimo = paper.path("M 250 250 l 0 -50 l -50 0 l 0 -50 l -50 0 l 0 50 l -50 0 l 0 50 z");  
  
    tetronimo.attr({fill: '#9cf', stroke: '#ddd', 'stroke-width': 5});  
	}  

执行结果如下：

<p data-height="268" data-theme-id="0" data-slug-hash="ufemz" data-user="janily" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/ufemz'>ufemz</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a></p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

我们还可以改变方块的颜色和方向，改下代码：

    window.onload = function() {  
    var paper = new Raphael(document.getElementById('canvas_container'), 500, 500);  
    var tetronimo = paper.path("M 250 250 l 0 -50 l -50 0 l 0 -50 l -50 0 l 0 50 l -50 0 l 0 50 z");  
  
    tetronimo.attr(  
        {  
            gradient: '90-#526c7a-#64a0c1',  
            stroke: '#3b4449',  
            'stroke-width': 10,  
            'stroke-linejoin': 'round' 
        }  
    ); 
	  tetronimo.transform("r90");
	}  

(注意，原文是用rotation来改变方向的，但是现在好像失效了，我在W3C的官方的SVG没查到这个属性，在raphael的文档里查到是用transform来改变方向的)。

执行结果如下：

<p data-height="268" data-theme-id="0" data-slug-hash="gpqtj" data-user="janily" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/gpqtj'>gpqtj</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a></p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

这里推荐多去Raphael官方的文档看看相关的方法和使用细节。

### 6.动画 ###

Raphael还提供了**animate()**这个方法来制作动画效果，非常棒。而且允许我们用jQuery的方式来编写我们的动画序列。

先小试牛刀一下。先来360度旋转方块玩玩。代码如下：

    
执行效果如下：

<p data-height="268" data-theme-id="0" data-slug-hash="AvwFi" data-user="janily" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/AvwFi'>AvwFi</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a></p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

我们也可以把一个回调函数作为一个参数传给动画方法。也就是可以在动画执行完后再执回调函数。来看一个实例，上面我们的方块旋转完后可以再执行一个动画回调函数重置方块的位置。

代码如下：

    tetronimo.animate({"transform": "r 360", 'stroke-width': 1}, 2000, 'bounce', function() {
    /* callback after original animation finishes */
    this.animate({
        "transform": "r -90",
        "stroke": "#3b4449",
       	'stroke-width': "10"
    }, 1000);
		});
    
	});

执行结果如下：

<p data-height="268" data-theme-id="0" data-slug-hash="twFra" data-user="janily" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/twFra'>twFra</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a></p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

#### 6.路径动画 ####

在以前Flash开发中，经常看到一些路径绘制的动画。不过，通过Raphael的animate()方法我们也可以做到这些。首先我们绘制另外一个方块，如下所示：

    "M 250 250 l 0 -50 l -50 0 l 0 -50 l -100 0 l 0 50 l 50 0 l 0 50 z"  

如下图所示：

![](http://pic.yupoo.com/reicky_v/DpxwLU4z/medium.jpg)

同样我们在上面例子的基础上，来添加一个回调函数：

	tetronimo.attr(
    {
        stroke: 'none',
        fill: 'blue'
    });

	tetronimo.animate({ path: "M 250 250 l 0 -50 l -50 0 l 0 -50 l -100 0 l 0 50 l 50 0 l 0 50 z"}, 
		5000, 'elastic');

你可以明显看到我们这个方块的变化，效果如下：

<p data-height="268" data-theme-id="0" data-slug-hash="fLxah" data-user="janily" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/fLxah'>fLxah</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a></p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

### 7.访问DOM节点 ###

如果你想想访问DOM节点一样来访问我们矢量图形的节点，也很容易做到。这也是矢量图形的一大优势，我们可以给任意的节点添加事件。

我们先画一个圆。

    window.onload = function() {  
        var paper = new Raphael(document.getElementById('canvas_container'), 500, 500);  
  
        var circ = paper.circle(250, 250, 40);  
        circ.attr({fill: '#000', stroke: 'none'});  
	} 

然后添加一些文字以及设置它的一些样式。

	var text = paper.text(250, 250, 'Bye Bye Circle!')  
	text.attr({opacity: 0, 'font-size': 12}).toBack();  

我这里首先用透明度为0把文字隐藏起来。注意到这里我们用了一个**toBack()** 的方法，这个方法是把元素置于画布中其它元素之下(其实就相当于我们在CSS中z-index)。

现在，我们给我们画的圆添加一个鼠标经过事件，把鼠标设置为'pointer'。

    circ.node.onmouseover = function() {  
    this.style.cursor = 'pointer';  
	}

我们来看看实际在网页是怎么样的结构。如下所示：

    <circle cx="250.5" cy="250.5" r="40" fill="#000000" stroke="none" style="fill: #000000; stroke: none; cursor: pointer">  
	</circle>  

现在来给原型添加点击事件：

    circ.node.onclick = function() {  
    text.animate({opacity: 1}, 2000);  
    circ.animate({opacity: 0}, 2000, function() {  
        this.remove();  
    });  
	}  

当圆形被点击的时候，文字会显示出来。透明度从0增加到1。当动画执行完后，我们也可以添加一个回调函数来删除这个圆。如下所示：

<p data-height="268" data-theme-id="0" data-slug-hash="jhcCi" data-user="janily" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/jhcCi'>jhcCi</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a></p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

### 8.制作一个完整的实例 ###

上面学习了一些基本的知识，下面我们来综合所学来制作一个小实例。实例是这样子的，你输入1-5之间的数字，然后点击生成按钮，Raphael将会生成对应数字的圆形如下图所示：

![](http://pic.yupoo.com/reicky_v/DpxUmbi3/medium.jpg)

先来看看我们的js代码：

    window.onload = function() {  
    var paper = new Raphael(document.getElementById('canvas_container'), 500, 500);  
    var circ = paper.circle(250, 250, 20).attr({fill: '#000'});  
    var mood_text = paper.text(250, 250, 'My\nMood').attr({fill: '#fff'});  
	} 

上面的代码会创建一个半径为20的圆形以及一些文字，文字用\n换行符把文字换成两行。

下面是创建一些对应数字的信息。

    moods = ['Rubbish', 'Not Good', 'OK', 'Smily', 'Positively Manic'];  
	colors = ['#cc0000', '#a97e22', '#9f9136', '#7c9a2d', '#3a9a2d'];  
	  
	//pick a mood between 1 and 5, 1 being rubbish and 5 being positively manic  
	var my_mood = 1;  

我们把具体的信息放在**colors**和**moods**这两个数组里。接下来创建一个**show_mood**的函数，当触发这个函数后，它会根据具体的数字来生成相应的文字和颜色。

	function show_mood() {
			var my_mood = $("input#moodNumber").val();
	    for(var i = 0; i < my_mood; i+=1) {
	    console.log(i);
	    console.log(colors[my_mood - 1]);
	        (function(i) {
	            setTimeout(function() {
	                paper.circle(250, 250, 20).attr({
	                    "stroke": "none",
	                    "fill": colors[my_mood - 1]
	                }).animate({cx:250, cy: 250 - 42 * (i+1), r:20 }, 2000, "bounce" ).toBack();
	            }, 50*i);
	        })(i);
	    }
	    
	    paper.text(250, 300, moods[my_mood - 1]).attr({fill: colors[my_mood - 1]});
	    
	    mood_text.node.onclick = function() {
	        return false;
	    }
	    
	    circ.node.onclick = function() {
	        return false;
	    } 

在**show_mood**函数里，首先循环my_mood数组。在for循环里面是一个立即执行函数表达式。这样我们就可以在每一个循环阶段来访问变量**i**,在立即执行函数表达式里面，我们创建一个用来绘制圆形的定时器，每隔50×i秒在指定的位置绘制圆形。并且在y轴的方向每绘制一个圆形就往上平移 250 - 42×(i+1)个单位。注意这里的颜色和文字是根据my_mood的值来决定的。

可以在下面的文本框中输入1-5之间的数字，然后点击getMood按钮就可以看到效果了。

<p data-height="268" data-theme-id="0" data-slug-hash="yoFfr" data-user="janily" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/janily/pen/yoFfr'>yoFfr</a> by janily (<a href='http://codepen.io/janily'>@janily</a>) on <a href='http://codepen.io'>CodePen</a></p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

到此基本上把Raphaeljs框架的一些基本介绍的差不多了。现在我们就可以深入研究Raphael，然后发挥我们的想象力运用Raphaeljs来创造各种各样的图形和交互动画了。



          

