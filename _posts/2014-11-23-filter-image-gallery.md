---
layout: post
title: 使用javascript实现过滤筛选效果
categories:
- Life
tags:
- javascript
- flexbox
- 翻译

---

> 原文来自[Filtered Flexbox: A Dynamic Image Gallery](http://demosthenes.info/blog/945/Filtered-Flexbox-A-Dynamic-Image-Gallery)，具体效果请看下面的demo，点击下拉框选择选项，下面的图片就会根据对应字段筛选出来。

<p data-height="379" data-theme-id="0" data-slug-hash="pzhmn" data-default-tab="result" data-user="dudleystorey" class='codepen'>See the Pen <a href='http://codepen.io/dudleystorey/pen/pzhmn/'>Filtered Flexbox: Dynamic Image Gallery</a> by Dudley Storey (<a href='http://codepen.io/dudleystorey'>@dudleystorey</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

### HTML & CSS

首先来把html和css写好。这里图片部分使用**figure**这个元素来定义。还定义了一个空的div，是用来后面创建下拉框的。html结构如下所示	:

	<div id="selectionbar"></div>
	<div id="dynaflex">
	<figure data-group="bears">
	<img src="polar-bear-closeup.jpg" alt>
	</figure>
	<figure data-group="tigers" class="cub">
	<img src="tiger-cub.jpg" alt>
	</figure>
	<figure data-group="lions" class="on-grass">
	<img src="lion-on-grass.jpg" alt>
	</figure>
	<figure data-group="tigers">
	<img src="tiger.jpg" alt>
	</figure>
	<figure data-group="bears">
	<img src="polar-bear-swimming.jpg" alt>
	</figure>
	<figure data-group="lions">
	<img src="lion-and-lioness.jpg" alt>
	</figure>
	</div>

每一张图片使用data这个属性来定义它所属的分类。并且使用flex来布局，具体flex的介绍可以去我以前写的[文章](http://janily.gitcafe.com/life/2013/08/02/flexbox(1))了解当然你也可以使用完以前写一个脚本文件来达到类似瀑布流的布局效果。

	#dynaflex { display: flex; font-size: 0; flex-flow: row wrap; }
	#dynaflex figure { margin: 0; min-width: 201px; flex: 0.67; transition: .5s; }
	#dynaflex figure img { width: 100%; height: auto; }
	#dynaflex figure.cub { flex: 0.568; min-width: 170.49px; }
	#dynaflex figure.on-grass { flex: 1; min-width: 300px; }
	#dynaflex figure.diminish { min-width: 0; flex: 0; }
	
为了再筛选的时候有一个自然的过渡动画效果，我们使用transition这个属性配合diminish来制作动画效果。

### Javascript

首先创建一个空的**categories**数组，用来存放**figure**元素中的**data－group**属性的值。然后使用**filter**方法去重，保证数组的元素都是唯一的值，并且使用**reverse**方法重新排列数组，得到unshift["lions","tigers","bears"]，然后使用**unshift**方法往数组中添加**all**这个值。代码如下：

	var container = document.getElementById("dynaflex"),
	array = container.getElementsByTagName("figure"),
	selectionBar = document.getElementById("selectionbar"),
	categories = [];
	for (var i = 0; i < array.length ;i++) {
	categories[i] = array[i].dataset.group;
	}
	var unique = categories.filter(function(item, i, ar){ return ar.indexOf(item) === i; });
	unique.reverse();
	unique.unshift("all");
	
### 创建下拉菜单

下面是来创建下来筛选菜单，代码如下：

	var dropdown = document.createElement("select");
	dropdown.id = "categories";
	var dropdownLabel = document.createElement("label");
	dropdownLabel.for = dropdown.id;
	dropdownLabel.innerHTML = "Show : ";
	unique.forEach(buildDropDown);
	selectionBar.appendChild(dropdownLabel);
	selectionBar.appendChild(dropdown);
	
我们这使用了ECMAScript5中的**forEach**方法来调用**buildDropDown**这个函数来创建下拉菜单：

	function buildDropDown(name,index,array) {
	var opt = document.createElement("option");
	opt.value = name;
	opt.innerHTML = name;
	dropdown.appendChild(opt);
	}

这个方法就是用来创建下拉菜单选项的，下面是来操作菜单的交互响应代码的编写：

	dropdown.onchange = function() {
	if (this.value == "all") {
	for (var i = 0; i < array.length; ++i) {
	array[i].classList.remove("diminish");
	}
	} else {
	var hide = document.querySelectorAll('#dynaflex figure:not([data-group="'+this.value	+'"])');
	for (var i = 0; i < hide.length; ++i) {
	hide[i].classList.add("diminish");
	}
	var show = document.querySelectorAll('#dynaflex figure[data-group="'+this.value+'"]');
	for (var i = 0; i < show.length; ++i) {
	show[i].classList.remove("diminish");
	}
	}
	}
	
OK，一个简单的过滤系统功能就完成了，不过在移动端Safari上有点问题，这个我相信后面会得到解决，现在这个效果不依赖任何的框架和插件就完成了，great。