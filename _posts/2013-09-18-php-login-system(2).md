---
layout: post
title: 用PHP创建一个简单注册登陆系统（2）
categories:
- Life
tags:
- php
---

在前面的第一章节中，我们基本把前端的页面搭建起来了，下面就开始写后端PHP部分的代码了。

我们首先写登陆这块的功能，具体的功能如下列表所示：

- 用户名和密码是必填字段
- 用户输入的名字必须是已经注册的
- 如果用户名是已经注册的，那还得匹配密码是否正确
- 上面的都通过，那就提示登陆成功

上面的都通过，那就提示登陆成功。OK，当然我们得建立一个数据库来存储用户数据，我们这里使用的是和PHP黄金搭档的MYSQL数据库，来建立数据库。首先我们来规划下，我们数据库中的一些字段吧，这里我用这个工具[http://drawmyba.se/](http://drawmyba.se/)简单的画了一个数据库的字段结构图，如下：

<iframe src="http://drawmyba.se/v/fd7128c2338a4842ee79e89fb59ec9db01226e53" frameborder="no" style="width: 100%;max-width:700px;height: 410px;margin: auto; display: block;"></iframe>

你可以用mysql新建个loginTut的表，然后在这个表中运行下面的SQL语句：

    CREATE TABLE IF NOT EXISTS `users` (
	`id` int(11) NOT NULL auto_increment,
	`username` varchar(32) NOT NULL,
	`password` varchar(32) NOT NULL,
	`online` int(20) NOT NULL default '0',
	`email` varchar(100) NOT NULL,
	`active` int(1) NOT NULL default '0',
	`rtime` int(20) NOT NULL default '0',
	PRIMARY KEY (`id`)
	) ENGINE=MyISAM DEFAULT CHARSET=utf8;


现在我们就可以往数据库存储用户信息了，我们先添加一个测试账户，运行下面的SQL语句：

    INSERT INTO `users` (`id`, `username`, `password`, `online`, `email`, `active`, `rtime`) VALUES
	(1, ‘testing’, ‘testing’, 0, ‘fake@noemail.co.uk’, 0, 0);

现在我们就有了一个用户名为testing，密码为testing的测试账户了，所以我们现在就可以来写PHP代码来处理登陆功能了。

### PHP ###

在写PHP前，为了安全着想，我们需要过滤下字符串，我们把它写到一个functions.php的文件里。代码如下：

    <?php
 
		function protect($string){
		$string = trim(strip_tags(addslashes($string)));
		return $string;
	}
 
	?>

这段代码会过滤掉一些 HTML 和 PHP 标记以及字符串的空格，具体可以PHP官方手册看看对应的函数的详细说明。

现在，我们把上一节的登陆页面改为PHP文件，PHP可以和HTML代码混编的，下面的代码就是添加了PHP代码后的登陆页面，你可以看到有了很多的PHP代码，如下：

    <head>
  	<meta charset="utf-8">
    <title>Bootstrap 101 Template</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <!-- Bootstrap -->
    <link rel="stylesheet" href="assets/css/bootstrap.min.css">
    <link rel="stylesheet" href="assets/css/sigin.css">
	  </head>
	  <body>
	  	<?php
 
	//用户提交表单执行下面代码

	
	if(isset($_POST['login'])){
		// var_dump($_POST);
		// exit();
		
		//过滤字符串
		$username = protect($_POST['username']);
		$password = protect($_POST['password']);

		//检测用户名和密码两个字段是否已经输入完全
		if(!$username || !$password){
			//如果没输入完全，就提示用户
			echo "<center>必须输入用户名和密码!</center>";
		}else{
			//如果全部输入了，那就继续执行

			//从数据库中取出所有的用户名的字段与用户输入的用户名进行匹配
			$res = mysql_query("SELECT * FROM `users` WHERE `username` = '".$username."'");
			$num = mysql_num_rows($res);

			//如果没有匹配到
			if($num == 0){
				//提示用户用户名不存在
				echo "<center>用户名不存在</center>";
			}else{
				//如果存在

				//则把用户名和密码字段一起在数据库中进行比较
				$res = mysql_query("SELECT * FROM `users` WHERE `username` = '".$username."' AND `password` = '".$password."'");
				$num = mysql_num_rows($res);

				//如果匹配不成功
				if($num == 0){
					//则提示用密码错误
					echo "<center>密码错误</center>";
				}else{
					//如果匹配成功
					//返回根据从结果集取得的行生成的关联数组
					$row = mysql_fetch_assoc($res);

					//检测用户是否是已经激活的
					if($row['active'] != 1){
						//没激活，就提示用户还没激活账户
						echo "<center>你还没激活你的账户</center>";
					}else{
						//如果已经激活了的

						//检测是否已经登陆
						$_SESSION['uid'] = $row['id'];
						//提示
						echo "<center>你已经成功登陆</center>";

						//更新时间戳
						$time = date('U')+50;
						mysql_query("UPDATE `users` SET `online` = '".$time."' WHERE `id` = '".$_SESSION['uid']."'");

						//重定向在线页面
						header('Location: usersOnline.php');
					}
				}
			}
		}
	}

	?>
   <div class="container">
      <form action="login.php" method="post" class="form-signin">
        <h2 class="form-signin-heading">登录</h2>
        <input type="text" class="form-control" placeholder="用户名" name="username" autofocus="">
        <input type="password" class="form-control" name="password" placeholder="密码">
        <label class="checkbox">
          <input type="checkbox" value="remember-me">记住我
        </label>
        <p>
        	<button class="btn btn-primary" type="submit" name="login">登录</button>
        	<button class="btn btn-default" type="submit" name="reg">注册</button>
        </p>
        <p>
        	 <a href="#">忘记密码</a>
        </p>
      </form>
	 	</div>
	  </body>
	</html>
	<?
		ob_end_flush();
	?>

代码就不详细说明了，具体已经注释的很清楚了。现在我们就可以用我们的测试账号来测试我们写的登陆的代码是否正确了。下面是我测试的一个截图，我把用户名输入为test，按照我们代码里写的，应该会返回*用户名不存在*，如下：

![](http://pic.yupoo.com/reicky_v/DaGBWH7f/medium.jpg)

OK，登陆这部分没什么问题了，下一篇开始写注册功能。
    