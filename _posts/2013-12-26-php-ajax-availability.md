---
layout: post
title: 用php和jquery制作一个ajax用户验证功能
categories:
- Life
tags:
- php
- jquery
---

> 收藏夹里面这个网站[9lessons](http://www.9lessons.info/)一直静静的躺着那里，这段时间公司业务不是很忙正好自己也在学习PHP方面的知识，而这个网站上的教程大部分都是PHP和javascript的非常不错，有很多很常见的web功能的教程，是学习PHP和ajax的好地方。结合实例学习web开发正是我喜欢的方式，今天先来一篇关于PHP和ajax进行用户验证的教程，[原文地址](http://www.9lessons.info/2009/12/live-availability-checking-with-jquery.html)。主要是结合自己的理解来翻译教程。

使用jQuery作为javascript的底层库，它封装了web开发中常见的功能，如Ajax等。这个简单的的验证功能是验证用户输入的数据是否在数据库中已经存在，如果存在则提示用户用户名已经存在。像这样的验证功能在web开发中非常常见。

如下图所示：

![](http://pic.yupoo.com/reicky_v/DpBETDpv/medium.jpg)

[源代码下载地址](http://demos.9lessons.info/url.php?url=http://www.box.net/shared/pb5s3oems1)

开始之前，我们先要在数据库中建一个labs库，然后建一个user表，这个表包含两个字段一个**user_id**和**user_name**,然后插入一条数据来做验证用。

下载好文件后，我们首先来看一下**user_check.html**文件里的代码。

**$('#username').change(function(){} - username**这句的代码的意思是为输入框绑定一个事件，一个元素的值改变的时候将触发change事件。而且是专门用于**input**元素的。因为我们给输入框定义了一个di为username，所以我们用jQuery就很容易选择输入框输入的值，即$("#username").val("id")。我们首先是检测输入框里的值的长度是否大于3，来提示用户的输入是否合法。如下代码所示：

    <script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/1.3.0/jquery.min.js"></script>
	<script type="text/javascript">
	$(document).ready(function()
	{
	
	$("#username").change(function() 
	{ 
	var username = $("#username").val();
	var msgbox = $("#status");
	
	if(username.length > 3)
	{
	$("#status").html('<img src="loader.gif">&nbsp;Checking availability.');
	
	$.ajax({ 
	type: "POST", 
	url: "check_ajax.php", 
	data: "username="+ username, 
	success: function(msg){ 
	$("#status").ajaxComplete(function(event, request){ 
	
	if(msg == 'OK')
	{ 
	// 这里是不同的样式来提示用户输入的数据是否正确，当然你也可以删除，不过这样用户提样就大大降低了哦
	$("#username").removeClass("red"); // remove red color
	$("#username").addClass("green"); // add green color
	msgbox.html('<img src="yes.png"> <font color="Green"> Available </font>');
	} 
	else 
	{ 
	// 这里是不同的样式来提示用户输入的数据是否正确，当然你也可以删除，不过这样用户提样就大大降低了哦
	$("#username").removeClass("green"); // remove green color
	$("#username").addClass("red"); // add red  color
	msgbox.html(msg);
	} 
	});
	} 
	}); 
	
	}
	else
	{
	// 这里是不同的样式来提示用户输入的数据是否正确，当然你也可以删除，不过这样用户提样就大大降低了哦
	$("#username").addClass("red"); // add red color
	$("#status").html('<font color="#cc0000">Enter valid User Name</font>');
	}
	return false;
	});
	});
	</script>
	<input type="text" name="username" id="username" />
	<span id="status"></span>

可以看到上面的代码中，我们用Ajax的方法来跟服务器进行通讯，把用户名跟服务器数据库中的数据进行对比判断。接下来就是服务端的**check_ajax.php**的文件，我们的Ajax就是跟它进行通讯。也就几句简单的PHP代码。

	<?php
	
	include('db.php');
	
	
	if(isSet($_POST['username']))
	{
	$username = $_POST['username'];
	$username = mysql_real_escape_string($username);
	$sql_check = mysql_query("SELECT user_id FROM user WHERE username='$username'");
	
	if(mysql_num_rows($sql_check))
	{
	echo '<font color="#cc0000"><STRONG>'.$username.'</STRONG> 已经存在，请修改用户名.</font>';
	}
	else
	{
	echo 'OK';
	}
	
	}
	
	?>

首先是引入数据库操作的PHP文件，源代码里有这个就不详细阐述了。然后检测用户发送来请求是否有username这个字段，如果有，则以发送过来的这个字段为条件在数据中进行查询，如果在数据中有这个值，则说明数据中已经存在这个用户名就输出名字提示用户这个用户名已经存在。

CSS就不贴出来啦，很简单的在源文件里面都可以看到。







