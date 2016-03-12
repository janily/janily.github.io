---
layout: post
title: flexbox实战之旅(1) 
categories:
- Life
tags:
- css3
- flexbox
- 前端

---

> 订阅了[davidwalsh](http://davidwalsh.name/)上的flexbox系列的学习文章，今天是第一篇[Getting Dicey With Flexbox](http://davidwalsh.name/flexbox-dice)看了下，非常有实战意义，就搬过来了。顺便也加深对flexbox的理解。

你能在短时间之内构建一个复杂的CSS布局么？要是还用以前的来技术，可能有点难度。如果使用W3C标准中flexbox来进行布局的话，无论是登高、水平垂直居中等布局小菜一碟，分分钟的事情。

在很大一部分的开发者的观念中，可能觉的现在使用flexbox还不是时候。是时候该更新自己的观念啦，现在差不多93%的用户使用的都是现代浏览器，特别是移动端，更是如此。比对**video**的支持还要好。

在这篇文章中，我将会通过一个骰子的实例来介绍flexbox的基本使用方法。当然，我会以W3C最新的标准语法来编写代码，至于浏览器前缀等这里就不管啦。

###骰子第一面

骰子一共有六个面。每个面的点数都不相同，第一个面到点数是1，并且水平垂直居中。

首先是html结构。

	<div class="first-face">
  		<span class="pip"></span>
	</div>
	
为了使样式有一个基本的骰子的形状，编写了一些基本的样式。最终效果如下图所示：

![image](http://davidwalsh.name/demo/dicey-flexbox-images/face-1-1.png)

样式上，首先声明它是一个flexbox的容器：

	.first-face {
  		display: flex;
	}
	
![image](http://davidwalsh.name/demo/dicey-flexbox-images/face-1-1.png)

基本的样子是有了。不过，还有一个问题就是这个点数没有水平垂直居中。

![image](http://davidwalsh.name/demo/dicey-flexbox-images/face-1-1-axes.png)

从上面到图可以看到，有两根轴，分别是水平方向和垂直方向的。

**justify-content**属性是用来定义水平方向轴的对齐的。比如，我们想让内容沿水平方向的轴居中对齐，可以指定它的值为center就可以啦。

	.first-face {
  		display: flex;
  		justify-content: center;
	}
	
![image](http://davidwalsh.name/demo/dicey-flexbox-images/face-1-2.png)

简单的一句话，就让内容乖乖的居中对齐啦。

而**align-items**是用来指定垂直方向的对齐的，因为要水平垂直居中对齐，所以也指定它的值为center就可以啦。

	.first-face {
  		display: flex;
  		justify-content: center;
  		align-items: center;
	}
	
![image](http://davidwalsh.name/demo/dicey-flexbox-images/face-1-3.png)

看到没有，就是这么简单，这么酷！

###骰子第二面

骰子的第二面是两个点数，一个点是在左上角，另外一个是在右下角。这对于flexbox来说再简单不过了！

兵马未动，粮草先行。先上html：

	<div class="second-face">
  		<span class="pip"></span>
  		<span class="pip"></span>
	</div>
	
	.second-face {
  		display: flex;
	}
	
![image](http://davidwalsh.name/demo/dicey-flexbox-images/face-2-1.png)

现在要做的是让两个点分别居左对齐和居右对齐。只需要**just-content**的值为**space-between**就可以了。

space-between表示弹性盒子元素会平均地分布在行里。如果最左边的剩余空间是负数，或该行只有一个子元素，则该值等效于'flex-start'。在其它情况下，第一个元素的边界与行的主起始位置的边界对齐，同时最后一个元素的边界与行的主结束位置的边距对齐，而剩余的伸缩盒项目则平均分布，并确保两两之间的空白空间相等。

	.second-face {
  		display: flex;
  		justify-content: space-between;
	}
	
![image](http://davidwalsh.name/demo/dicey-flexbox-images/face-2-2.png)

现在的点数还没有按照期待中的那样摆放。这里就不能使用**align-items**的属性来对齐了，因为它会影响到点数彼此之间的对齐。幸运的是，flexbox有一个**align-self**的属性。

**align-self**属性是用来设置或检索弹性盒子元素自身在侧轴（纵轴）方向上的对齐方式。当值为**flex-end**的时候，表示弹性盒子元素的侧轴（纵轴）起始位置的边界紧靠住该行的侧轴结束边界。

	.second-face {
  		display: flex;
  		justify-content: space-between;
	}

	.second-face .pip:nth-of-type(2) {
  		align-self: flex-end;
	}
	
![image](http://davidwalsh.name/demo/dicey-flexbox-images/face-2-3.png)

###第四个面

来看看第四个面吧。这个面有四个点，并且是两两分别居左居右对齐。

这里有两点需要注意，一点是flexbox的内容可以是水平或者垂直摆放；另一点是flexbox可以彼此嵌套。

这个面需要改变下html的组织方式：

	<div class="fourth-face">
  		<div class="column">
    		<span class="pip"></span>
    		<span class="pip"></span>
  		</div>
  		<div class="column">
    		<span class="pip"></span>
    		<span class="pip"></span>
  		</div>
	</div>
	
![image](http://davidwalsh.name/demo/dicey-flexbox-images/face-4-1.png)

首先像前面做的一样把四个点分别居左居右对齐。

	.fourth-face {
  		display: flex;
  		justify-content: space-between;
	}
	
![image](http://davidwalsh.name/demo/dicey-flexbox-images/face-4-2.png)

然后置或检索伸缩盒对象的子元素在父容器中的位置为纵向排列。

	.fourth-face {
  		display: flex;
  		justify-content: space-between;
	}

	.fourth-face .column {
  		display: flex;
  		flex-direction: column;
	}
	
![image](http://davidwalsh.name/demo/dicey-flexbox-images/face-4-2.png)

看起来好像没什么变化，但实际上已经是纵向排列的。上面我直接使用来flexbox互相嵌套。

然后就是设置字元素的对齐方式，即**justify-content**属性。

	.fourth-face {
  		display: flex;
  		justify-content: space-between;
		
	}

	.fourth-face .column {
  		display: flex;
 		flex-direction: column;
  		justify-content: space-between;
	}
	
![image](http://davidwalsh.name/demo/dicey-flexbox-images/face-4-3.png)

###第三、第五、第六面

其它几面依样画葫芦，跟上面的原理差不多。

如下所示：

<p data-height="268" data-theme-id="0" data-slug-hash="KpzzGo" data-default-tab="result" data-user="LandonSchropp" class='codepen'>See the Pen <a href='http://codepen.io/LandonSchropp/pen/KpzzGo/'>Flexbox Dice</a> by Landon Schropp (<a href='http://codepen.io/LandonSchropp'>@LandonSchropp</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

未完待续！

