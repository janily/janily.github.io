---
layout: post
title: html5新API-Web Notifications的使用方法
categories:
- Life
tags:
- html5
- 翻译
---

> 这篇文章翻译自inserthtml网站的[Why (and How) you Should Probably Use Web Notifications](http://www.inserthtml.com/2013/10/notification-api/)。行文风格有少量改编。

本文中用到的源代码[下载地址](http://inserthtml.com/downloads/inserthtml.com.notifications.zip)。

![](http://pic.yupoo.com/reicky_v/De1szcFm/medium.jpg)

Web通知(Web notifications)是允许通过浏览器来向用户推送消息提醒的一个新的API。在Chrome29里，已经很完整的支持这个新的API。使用它可以使我们开发web应用的时候，又有一得力凶器。

![](http://pic.yupoo.com/reicky_v/De1ubLCC/medium.jpg)

### Web通知开发web应用意义何在（Why should I Use It?） ###

Chrome和Firefox都已经支持web通知这个API了。Opera应该在不久之后也会支持，当然不仅于此，使用web通知开发weby应用还有以下一些意义：

- 这类消息推送将会在web开发中应用的越来越普遍
- 这样可以增加网站对用户的粘性。当推送消息的时候，会弹出一个通知菜单，这样当用户点击消息的时候，他可能又会回到你的网站。
- 配合javascript，我们完全可以按照自己的需求开发自己消息通知中心。
- web通知是独立与浏览器来推送消息的。这意味着我们可以在不同的设备上更好地利用消息推送。比如在一些小屏幕的移动设备上，推送消息就会占用本来就不大的屏幕的空间，这个时候就可以使用*web通知*了。

### 怎样使用web通知呢？（How do I Use Notifications?） ###

这个API的使用不是很难。而且Web Notifications API还提供了一些非常有用的属性，可以开发出很强大的消息应用。下面就来一个简单的示例，演示一下web通知的用法。

在使用web通知前，首先得使用*new Notification*创建一个通知对象，如下所示：

    window.addEventListener('load', function() {
	
	function theNotification() {
		
	   	var n = new Notification("Hi!",  {
	   		icon: 'icon.jpg', 
	   		tag: 'note', 
	   		body: 'Notification content...'
	    });
	    	
	}

在上面的代码中我们创建了一个新的通知对象*new Notification("Hi", {})*。其中*Hi*是通知对象的标题。然后我们通过通知对象提供的一些属性设置了相关通知属性，主要是以下5个属性：

- *dir* 设置消息的排列方向，可取值为“auto”(自动), “ltr”(left to right), “rtl”(right to left)
- *icon*用来设置在消息中显示图片的
- *tag*为消息添加标签。如果设置此属性，当有新消息提醒时，标签相同的消息，后一个消息框会替换先前一个，不会出现多重消息提示框。
- *body*用来设置消息具体的内容的
- *lang*用来设置你的编码语言的比如中文，具体这个地址[ BCP 47 language code](http://people.w3.org/rishida/utils/subtags/index.php?list=4&submit=list)。

设置好之后，接下来需要做的事情就是，我们需要从用户那里得到授权才能给用户推送消息。下面就是具体的代码。

    // 选中按钮
	var button = document.querySelector('#button');
	
	// 绑定点击事件
	button.addEventListener('click', function () {
	
		// 表示用户同意消息提醒
		if (Notification && Notification.permission === "granted") {
			theNotification();
		}
	
		// 用户没有拒绝消息推送提醒的情况
		else if (Notification && Notification.permission !== "denied") {
			
			// 判断用户的授权情况
			Notification.requestPermission(function (status) {
				
				// Change based on user's decision
				if (Notification.permission !== status) {
					Notification.permission = status;
				}
				
				// 如果用户允许消息推送
				if (status === "granted") {
					theNotification();
				}
		
				else {
					// 用户浏览器不支持消息提醒
				}
				
			});
		
		}
	
		else {
			// 用户浏览器不支持消息提醒
		}
		
	});

这里先介绍下通知这个API的具体实现机制，桌面提醒功能是由window对象下的webkitNotifications来实现的，通过window.webkitNotifications将返回一个NotificationCenter对象。这个对象没有属性，但是却关联着四个方法：1.requestPermission()这个方法用于向用户请求获得消息提醒的权限，调用这个方法将产生如下效果（下图），分别对应着3中状态：“granted”（状态值：0）表示用户同意消息提醒；“default”（状态值：1）表示默认状态，用户既未拒绝，也未同意；“denied”（状态值：1）表示用户拒绝消息提醒。只有在状态值为0的时候才能够允许消息提醒，这个值保存在一个内部变量中，并且是只读的，通过checkPermission()方法可以提取到这个状态值。

如果用户不允许消息推送，我们也得准备一个回退方案，就是用HTML和CSS编写一个弹窗来推送消息，从而在用户不允许消息推送的情况下，也可以推送消息给用户。


### 一些问题[Perceived Problems] ###

我是很支持在web开发中使用web通知这个API，但是有时候也会给用户带来一些困惑。现在虽然是互联网的时代，但是我们也不能随心所欲滥用web通知这个API。这也是需要用户授权的原因所在。然而，web通知的授权选项过于晦涩，有可能会被一些用户带来困惑。比如Firefox的授权选项，给出很多选项给用户选择，把一个简单的问题搞得非常复杂，从而使用户无所适从。如下图：

![](http://pic.yupoo.com/reicky_v/De2RzAlS/medium.jpg)

Web通知这个API的这个用户授权的这个特征，也会带来一些问题。比如如果你的浏览器告诉你授权浏览器消息推送，可能带来一些安全隐患，并且是以弹窗的形式告知你，你会怎么想？

面对这样的提醒，用户可能会拒绝授权消息推送，这样的话，用户就可能会把当前的页面直接关闭。这可不我们想要的。一种解决方法是，当寻求用户授权的时候，新开一个空白页面来询问用户是否同意消息推送，如果同意则返回web应用页面。这样体验上可能或好很多。

在Chrome浏览器中，是通过两个按钮来询问用户是否同意授权的。简单明了通过同意或者是不同意两个选项来询问用户的授权，这样可以避免给用户造成困扰。

下面就是一个简单的完整的消息通知的代码示例：

#### HTML ####

    <p>This site uses notifications! Click 'Notifications!' 
	below and if you want them and click 'ALLOW' or 'ALWAYS 
	SHOW' when your browser asks you, that way we can provide 
	you information in a more intuitive way. If you don't 
	want them, click below and then select 'DENY' or 'BLOCK'</p>
	
	<button id="button">Notifications!</button>

#### JS ####

    window.addEventListener('load', function() {
	// 选择DOM
	var button = document.querySelector('#button');
	
	// 绑定事件
	button.addEventListener('click', function() {
		
		// 判断浏览器是否支持web通知
		if(Notification) {
			
			// 如果用户允许
			if(Notification.permission !== "denied") {
				
				// Request permission
				Notification.requestPermission(function (status) {
				
					if (Notification.permission !== status) {
						Notification.permission = status;
					}
					
				});
				
			}
			
		}
			
	});
	
	// 当用户允许消息推送或者是拒绝的时候，重定向到你想返回的页面的地址
	setInterval(function() {
		
		if(Notification.permission === "granted" || Notification.permission === "denied") {
			
			// Put the URL you want to redirect to here.
			document.location.href = 'redirect URL';
			
		}
		
	}, 500);
	});

这好像看起来有点点麻烦？我我却不这样认为。我认为它能够把使用户迷惑困扰的那部分隐藏起来，这样留给用户的是简单明了的选项，这样体验上会更好一点。

### 回退方案（Fallbacks） ###

当然，我们也需要提供一个回退方案，以备用户浏览器不支持web通知的时候，也能够享受到web通知带来的便利。在上面的提供的下载的代码中，提供了一个回退的解决方案，可以去看看具体的代码。我认为，回退方案是必要的，而且通过CSS和javascript很容易实现，何乐而不为呢？

### 浏览器支持情况 ###

现在浏览器对web通知这个API支持的越来越好，Firefox和Chrome这两个浏览器就能很好的支持web通知，目前还不知道IE11是否支持web通知，具体浏览器支持的详细情况可以去这里查看，[caniuse](http://caniuse.com/#search=Web%20Notifications)。但有一点确定的是，现在可以用web通知这个API来开发令人惊叹的web应用啦~~！



