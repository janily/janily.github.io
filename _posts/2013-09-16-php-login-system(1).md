---
layout: post
title: 用PHP创建一个简单注册登陆系统（1）
categories:
- Life
tags:
- php
---



古有云：凡事预则立不预则废！做一个东西的时候，先要确定需求，你想要的是什么，具体有什么功能等等，这个在开始写代码前，就应该确定。

就像我们现在要写的这个登陆注册系统，在开始编码前，我们应该确定我们这个登录注册的功能具体的需求是什么，现在就开始来确定下我们这个登录注册系统的功能点，如下列表所示：

- 首先是登陆页面（具体的字段是，用户名，密码，忘记密码，登陆和注册按钮）。
- 注册页面（具体的字段是，用户名，密码，确认密码，登陆和注册按钮，忘记密码）。
- 忘记密码（邮箱，发送按钮）。
- 退出功能
- 激活账户功能

就上面功能点，我做了三个简单的线框图，退出功能、激活账户功能这两个功能页面只是两个简单的链接，就没做原型了。

下面分别是，登陆、注册、忘记密码三个页面的功能页面的线框图：

![](http://pic.yupoo.com/reicky_v/Dao1mUIM/QZnrM.png)

![](http://pic.yupoo.com/reicky_v/Dao1n3c9/HQwzx.png)

我们现在就可以按照我们这个线框图，来制作我们的页面，这里我用的bootstrap3这个前端框架来快速制作出我们的页面。

首先是我们的登陆页面，在制作页面前，得把bootstrap文件下下来，我们按照bootstrap框架的先把页面的一个基本骨架搭起来，如下

        <!DOCTYPE html>
		<html>
		  <head>
		    <title>Bootstrap 101 Template</title>
		    <meta name="viewport" content="width=device-width, initial-scale=1.0">
		    <!-- Bootstrap -->
		    <link rel="stylesheet" href="assets/css/bootstrap.min.css">
			<link rel="stylesheet" href="assets/css/sigin.css">
		  </head>
		  <body>
		    
			...放置内容代码区域...
		
		  </body>
		</html>

这里我新建了一个sigin.css文件，内容如下：

    body {
	  padding-top: 40px;
	  padding-bottom: 40px;
	  background-color: #eee;
	}
	
	.form-signin {
	  max-width: 330px;
	  padding: 15px;
	  margin: 0 auto;
	}
	.form-signin .form-signin-heading,
	.form-signin .checkbox {
	  margin-bottom: 10px;
	}
	.form-signin .checkbox {
	  font-weight: normal;
	}
	.form-signin .form-control {
	  position: relative;
	  font-size: 16px;
	  height: auto;
	  padding: 10px;
	  -webkit-box-sizing: border-box;
	     -moz-box-sizing: border-box;
	          box-sizing: border-box;
	}
	.form-signin .form-control:focus {
	  z-index: 2;
	}
	.form-signin input[type="text"] {
	  margin-bottom: -1px;
	  border-bottom-left-radius: 0;
	  border-bottom-right-radius: 0;
	}
	.form-signin input[type="password"] {
	  margin-bottom: 10px;
	  border-top-left-radius: 0;
	  border-top-right-radius: 0;
	}
	.mt10 {
	  margin-top: 10px;
	}

搭好基本页面的框架后，下面我先来具体制作登陆、注册和忘记密码页面，首先是登陆页面,代码如下：

     <div class="container">
	      <form class="form-signin">
	        <h2 class="form-signin-heading">登录</h2>
	        <input type="text" class="form-control" placeholder="用户名" autofocus="">
	        <input type="password" class="form-control" placeholder="密码">
	        <label class="checkbox">
	          <input type="checkbox" value="remember-me">记住我
	        </label>
	        <p>
	        	<button class="btn btn-primary" type="submit">登录</button>
	        	<button class="btn btn-default" type="submit">注册</button>
	        </p>
	        <p>
	        	 <a href="#">忘记密码</a>
	        </p>
	      </form>
   	 </div>

这样就把一个登陆页面做出来了，下面依样画葫芦，把注册页面和忘记密码页面做出来：

注册页面：

    <div class="container">
	      <form class="form-signin">
	        <h2 class="form-signin-heading">注册</h2>
	        <input type="text" name="username" class="form-control" placeholder="用户名" autofocus="">
	        <input type="password" name="password" class="form-control" placeholder="密码">
			<input type="password" name="passconf" class="form-control" placeholder="确认密码">
			<input type="text" name="email" class="form-control" placeholder="电子邮件">
	        <p>
	        	<button class="btn btn-primary" type="submit">注册</button>
	        	<button class="btn btn-default" type="submit">登陆</button>
	        </p>
	        <p>
	        	 <a href="#">忘记密码</a>
	        </p>
	      </form>
   	 </div>

忘记密码页面：

    <div class="container">
	      <form class="form-signin">
	        <h2 class="form-signin-heading">忘记密码</h2>
			<input type="text" name="email" class="form-control" placeholder="电子邮件">
	        <p>
	        	<button class="btn btn-primary" type="submit">发送</button>
	        </p>
	      </form>
   	 </div>

页面建好后，如下图所示：

登陆页面：

![](http://pic.yupoo.com/reicky_v/DaoqzIQx/3ObE4.jpg)

注册页面：

![](http://pic.yupoo.com/reicky_v/DaoqRtNd/ta65a.jpg)

忘记密码页面：

![](http://pic.yupoo.com/reicky_v/DaoqRDm2/medium.jpg)

页面写完后，就要开始写PHP部分代码了。这个留在下篇文章来写。
    