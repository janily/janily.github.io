---
layout: post
title: 用PHP创建一个简单注册登陆系统（3）
categories:
- Life
tags:
- php
---

在前面的第一章节中，我们完成了登录验证这一部分的功能代码。接下里就是注册功能的代码的实现。这一部分的代码跟登陆验证的代码原理差不多，这里要验证的是，用户注册的账号是否已经存在以及邮箱是否合法和密码强度是否符合规范等规则。如果验证全部通过，则把相关数据写入数据库，然后提示用户注册成功。具体代码如下所示：

    <?php
	//检测用户是否登陆
	session_start();
	 
	//连接数据库
	$con = mysql_connect('localhost', 'root', '') or die(mysql_error());
	$db = mysql_select_db('logintut', $con) or die(mysql_error());
	 
	//引入我们写好的functions.php文件
	include "./functions/functions.php";
	 
	?>
	<!DOCTYPE html>
	<html>
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
	
			if(isset($_POST['reg'])){
	 
				//过滤字符串并保存到定义好的变量中
				$username = protect($_POST['username']);
				$password = protect($_POST['password']);
				$passconf = protect($_POST['passconf']);
				$email = protect($_POST['email']);
	 
				//检测用户名和密码、确认密码、邮箱四个个字段是否已经输入完全
				if(!$username || !$password || !$passconf || !$email){
					//如果没输入完全，就提示用户
					echo "<center>必须输入用户名和密码邮箱!</center>";
				}else{
					//如果全部输入了，那就继续执行
	 
					//检测用户名的长度是否大于32个字符和小于3个字符
					if(strlen($username) > 32 || strlen($username) < 3){
						//如果符合上面条件则提示
						echo "<center>用户名的长度必须小于32大于3个字符</center>";
					}else{
						//如果通过则继续执行
	 
						//从数据库中取出所有的用户名的字段与用户输入的用户名进行匹配
						$res = mysql_query("SELECT * FROM `users` WHERE `username` = '".$username."'");
						$num = mysql_num_rows($res);
	 
						//如果匹配
						if($num == 1){
							//提示
							echo  "<center>这个用户名已经存在，请换个账号注册</center>";
						}else{
							//通过
	 
							//检测密码的长度
							if(strlen($password) < 5 || strlen($password) > 32){
								//没通过
								echo "<center>你的密码太短了，得在5至32个字符串长度之间</center>";
							}else{
								//通过
	 
								//让用户重复输入密码，并与第一次输入的匹配
								if($password != $passconf){
									//如果不匹配
									echo "<center>两次密码输入不一致，请重新输入!</center>";
								}else{
									//继续验证邮箱是否合法
					
									$checkemail = "/^[a-z0-9]+([_\\.-][a-z0-9]+)*@([a-z0-9]+([\.-][a-z0-9]+)*)+\\.[a-z]{2,}$/i";
	 
									//验证匹配
						            if(!preg_match($checkemail, $email)){
						            	//不符合规范
						                echo "<center>你输入的邮箱不符合规范，必须像name@163.com这样的格式!</center>";
						            }else{
						            	//验证邮箱是否存在
						            	$res1 = mysql_query("SELECT * FROM `users` WHERE `email` = '".$email."'");
						            	$num1 = mysql_num_rows($res1);
	 
						            	//存在邮箱
						            	if($num1 == 1){
											echo "<center>邮箱已经存在，请换一个邮箱注册</center>";
										}else{
											//如果都通过，则写入数据库更新时间
	 
							            	$registerTime = date('U');
	 
							            	//创建一个激活码
							            	$code = md5($username).$registerTime;
	 
							            	//写入数据库
											$res2 = mysql_query("INSERT INTO `users` (`username`, `password`, `email`, `rtime`) VALUES('".$username."','".$password."','".$email."','".$registerTime."')");
	 
											//把激活码发到用户的注册邮箱
											mail($email, $INFO['chatName'].' registration confirmation', "谢谢您的注册 ".$username.",\n\n这是一个激活链接. 如果链接不可点击，请把这个地址复制到浏览器的地址栏里，访问就可以激活您的账号.\n\nhttp://www.yourwebsitehere.co.uk/activate.php?code=".$code, 'From: noreply@youwebsitehere.co.uk');
	 
											//提示
											echo "<center>恭喜您，已经注册成功，请激活您的账号!</center>";
										}
									}
								}
							}
						}
					}
				}
			}
	 
			?>
	   <div class="container">
	      <form action="reg.php" method="post" class="form-signin">
	        <h2 class="form-signin-heading">注册</h2>
	        <input type="text" name="username" class="form-control" placeholder="用户名" autofocus="">
	        <input type="password" name="password" class="form-control" placeholder="密码">
			<input type="password" name="passconf" class="form-control" placeholder="确认密码">
			<input type="text" name="email" class="form-control" placeholder="电子邮件">
	        <p class="mt10">
	        	<button class="btn btn-default" type="submit" name="reg">注册</button>
	        	<button class="btn btn-primary" type="submit" name="login">登录</button>
	        </p>
	        <p>
	        	 <a href="#">忘记密码</a>
	        </p>
	      </form>
		 </div>
	  </body>
	</html>

上面的代码就实现了注册功能，当然也出现了几个新的函数，比如strlen()方法，就是返回给定的字符串 string 的长度。preg_match是执行一个正则表达式匹配，mail() 方法是用来发送邮件，其它的功能在注释都已经说明了。

下面，你可以测试下注册功能是否可用，注册完后，你会看到一个激活账号的提示（后面在写激活部分的功能），你可以去数据看一下，看看刚才注册的账号是否已经保存到数据库中。active 这个字段的值是0，表明还没有激活账号。

下面看看我的测试截图，我注册了一个账号，可以看到我已经注册成功，页面显示如下：

![](http://pic.yupoo.com/reicky_v/DaYWBVA8/medium.jpg)

接下来我们就来实现激活账号的功能。这个一面非常简单，但确实非常重要的，对于安全来说。代码如下（active.php）:

    <?php
	//检测用户是否登陆
	session_start();
	 
	//连接数据库
	$con = mysql_connect('localhost', 'root', '') or die(mysql_error());
	$db = mysql_select_db('logintut', $con) or die(mysql_error());
	 
	//引入我们写好的functions.php文件
	include "./functions/functions.php";
	 
	?>
	<html>
	<head>
		 <title>Bootstrap 101 Template</title>
	    <meta name="viewport" content="width=device-width, initial-scale=1.0">
	    <!-- Bootstrap -->
	    <link rel="stylesheet" href="assets/css/bootstrap.min.css">
	    <link rel="stylesheet" href="assets/css/sigin.css">
	</head>
	<body>
		<?php
 
		echo md5('other');
		//声明一个受保护类型，用于本类和继承类调用的变量$code（激活码）
		$code = protected($_GET['code']);
 
		//检测激活码
		if(!$code){
			//如果不存在
			echo "<center>激活码出现了一个错误!</center>";
		}else{
		
			//从数据中把还没有激活的账号取出来
			$res = mysql_query("SELECT * FROM `users` WHERE `active` = '0'");
 
			//遍历每一个没激活的账号
			while($row = mysql_fetch_assoc($res)){
				//如果激活码匹配，则把匹配到账号的状态设置为1，激活账号
				if($code == md5($row['username']).$row['rtime']){
					//if it does then activate there account and display success message
					$res1 = mysql_query("UPDATE `users` SET `active` = '1' WHERE `id` = '".$row['id']."'");
					echo "<center>您的账号已经激活!</center>";
				}
			}
		}
 
		?>
		</div>
	</body>
	</html>

相关代码的功能可以看下注释。

下面来总结下，通过注册和登陆的功能我们学到的知识点，如下：

#### 前端 ####

- 第一个是前端方面的，我们用到了bootstrap3这个前端框架，这个可以让我们快速的做出一个网站的UI界面。

#### PHP ####

- mysql_connect() – 用于连接数据库。
- mysql_select_db() - 用于选择数据库中的表。
- mysql_query() – 执行SQL语句，比如增删查改。
- trim() – 去掉字符串首位的空格。
- strip_tags() – 去掉html或者php中的一些特殊符号标记。
- addslashes() – 使用反斜线引用字符串，返回字符串，该字符串为了数据库查询语句等的需要在某些字符前加上了反斜线。这些字符是单引号（'）、双引号（"）、反斜线（\）与 NUL（NULL 字符）。
- strlen() – 返回指定字符串的长度。
- preg_match() – 执行一个正则表达式匹配。
- mail() – 发送邮件（这里需要在服务器相关设置，具体可google或baidu）。
- md5() – md5加密。

现在主体的注册登陆功能已经完成，还剩两个辅助功能，忘记密码和用户在线两个功能，下一篇再来实现。


    