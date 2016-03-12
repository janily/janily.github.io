---
layout: post
title: 14个方便使用的jquery代码片段
categories:
- Life
tags:
- jquery
- 翻译
---

自从jquery诞生后，javascript的编写从未变得如此容易和舒适。对于一个刚开始从事web前端的开发者来说，也会编写一些jquery代码。

这里收集了14个非常有用的jquery代码片段，在需要的时候可以用到它。这里提供的仅仅是一些编写好的代码片段，你可以在需要的时候改写它。

###1.Hover On/Off

<pre><code>
$("a").hover( function () 
{ // code on hover over }, 
function () { // code on away from hover } ); 
</code></pre>

Jquery的hover方法是一个简洁的事件处理方法。你可以给鼠标停留或者离开定义具体事件，具体编写在两个函数里面。

###2.Prevent Achor Links frm Loading(阻止默认事件）

<pre><code>
$("a").on("click", function(e){
  e.preventDefault();
});
</code></pre>

很多时候，当你用用一个链接或者一个按钮，来创建javascript应用时，你不希望去触发链接的默认事件。仅仅使用当做触发一些如菜单隐藏或者ajax的回调的动态效果的钩子。通过点击事件，我们可以通过浏览器的url来操纵数据。在这种情况下，我们就可以使用&quot;e.preventDafaule();&quot;来阻止默认事件。

###3.Scroll to top(滚动到顶部）

<pre><code>
$("a[href='#top']").click(function() 
{ $("html, body").animate({ scrollTop: 0 }, "slow"); 
return false; });
</code></pre>

回到顶部这个功能，现在非常流行。特别是在一些社交性的网站上，如新浪微博。

这个代码片段非常简洁使用。而且这个片段还带有一个平滑的动画效果，而不会非常生硬的跳到顶部，会缓缓的滚动到顶部。

###4.Ajax Template

<pre><code>
$.ajax({ type: 'POST', url: 'backend.php', data: "q="+myform.serialize(), 
success: function(data){ // on success use return data here }, 
error: function(xhr, type, exception) { // if ajax fails display error alert alert("ajax error response type "+type); } });
</code></pre>

通过ajax方法获取数据是jquery一个使用频率非常高的方法。作为web开发者会经常跟ajax打交道。而通过原生的javascript来使用ajax,非常繁琐并且还有很多兼容性问题需要处理，而通过这一小段的jquery的ajax模板，可以使我们更加快速的编写健壮有力的ajax应用。

###5.Animate Template(动画）

<pre>
<code>
$('p').animate({ 
left: '+=90px', 
top: '+=150px',
opacity: 0.25 }, 
900, 'linear', function() 
{ // function code on animation complete });
</code>
</pre>

jquery的动画方法是编写页面梦幻交互展现方式的一个重型武器，css3的transitions在写一些简单的交互还是非常有用的。但是animate方法却可以同时操纵一个目标对象多个css属性。

更多的关于jquery的animate方法是用实例，可以去看看jquery ui这个ui库，链接在[这里](http://jqueryui.com/)。

###6.Toggle CSS Classes

<pre><code>
$('nav a').toggleClass('selected');
</code></pre>

通过同一个动作添加或者删除一个css类，是不是很熟悉这样的场景，没错在一些菜单或tab的交互场景里，常常需要来回的切换当前状态，如高亮或者正常状态。通常我们一般用&quot;.addClass()&quot;和&quot;.removeClass()&quot;方法来搞定，而通过&quot;toggleClass()&quot;方法，一句话就搞定了。

###7.Toggle Visibility

<pre>
<code>
$("a.register").on("click", function(e){
  $("#signup").fadeToggle(750, "linear");
});
</code>
</pre>

淡入淡出这样的效果，在一些交互场景中很常见，如弹出框。当需要应用到这个淡入淡出效果时。使用这个&quot;fadeToggle&quot;方法，可以非常快速地达到效果。

###8.Loading external content

<pre>
<code>
$("#content").load("somefile.html", 
function(response, status, xhr) 
{ // error handling if(status == "error") 
{ $("#content")
.html("An error occured: " + xhr.status + " " + xhr.statusText); } });
</code>
</pre>

有时候我们需要从外部载入一些html文件到当前的页面中。如当Ajax请求遭遇一个错误时，我们可以用来显示一些信息。

###9.Keystroke Events

<pre>
<code>
$('input').keydown(function(e) 
{ // variable e contains keystroke data // only accessible with 
.keydown() if(e.which == 11) { e.preventDefault(); } }); 
$('input').keyup(function(event) { // run other event codes here	 });
</code>
</pre>

jquery键盘监听方法可以处理很多场景下的键盘响应事件。当然还是不同情况下，键盘按键表现也不一样，需要谨慎处理。

###10.Equal Column Heights

<pre>
<code>
var maxheight = 0;
$("div.col").each(function()
{ if($(this).height() > maxheight) 
{ maxheight = $(this).height(); } }); $("div.col").height(maxheight);
</code>
</pre>

这个是不到万不得已才使用的，慎用，少用，不用。

###11.Append new html

<pre>
<code>
var sometext = "here is more HTML"; 
$("p#text1").append(sometext); // added after $("p#text1").prepend(sometext); // added before
</code>
</pre>

使用&quot;append()$&quot;方法，我们可以快速的在现有的dom结构中插入新的html代码。这个方法经常跟ajax一起使用。

###12.Setting & Getting Attributes

<pre>
<code>
var alink = $("a#user").attr("href"); // obtain href attribute value 
$("a#user").attr("href", "http://www.google.com/"); // set the href attribute to a new value 
$("a#user").attr({ alt: "the classiest search engine", href: "http://www.google.com/" }); // set more than one attribute to new values
</code>
</pre>

jquery的这个特性非常棒。你可以把&quot;.attr()&quot;方法应用到你想要放的html元素上，它就会返回这个元素的一些属性以及值，如id、class、name等属性，总之是一个非常牛逼的方法。

###13.Retrieve Content Values

<pre>
<code>
$("p").click(function () 
{ var htmlstring = $(this).html(); // obtain html string from paragraph $(this).text(htmlstring); // overwrite paragraph text with new string value }); var value1 = $('input#username').val(); // textfield input value var value2 = $('input:checkbox:checked').val(); // get the value from a checked checkbox var value3 = $('input:radio[name=bar]:checked').val(); // get the value from a set of radio buttons
</code>
</pre>           

正如你可以创建html内容到页面中，有时候你也想获取html的值。如&quot;val()&quot;方法。常常用于获取表单里文本框的值。

###14.Traversing the DOM

<pre>
<code>
$("div#home").prev("div"); // find the div previous in relation to the current div 
$("div#home").next("ul"); // find the next ul element after the current div 
$("div#home").parent(); // returns the parent container element of the current div 
$("div#home").children("p"); // returns only the paragraphs found inside the current div
</code>
</pre>

dom节点上的移动方法，对于一个有jquery使用经验的web开发者来说，会经常用的这些方法，如&quot;parent()&quot;&quot;next()&quot;等，可以很容易选择我们想要选择的元素。

这写个代码片段对于刚接触jquery的web开发者来说，还是非常有用的。当然，如果你也有一些好的代码片段，也可以拿出来与大家分享。

本文并未逐字逐句的翻译，加上自己js正在学习中，有些技术上的细节可能翻译理解不到位，还请多多指正。原文在[这里](http://blog.teamtreehouse.com/14-handy-jquery-code-snippets-for-developers)。
