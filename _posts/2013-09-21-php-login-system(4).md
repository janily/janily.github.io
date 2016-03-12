---
layout: post
title: 用PHP创建一个简单注册登陆系统（4）
categories:
- Life
tags:
- php
---

接着上篇，接下来是实现忘记密码，找回密码这个功能。具体是功能点是把用户的密码发送到用户的邮箱里。这部分的代码跟注册功能的代码大同小异，最终代码如下：

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
	if(isset($_POST['forget'])){
		
		//把用户提交的邮件存到定义好的变量中
		$email = protect($_POST['email']);

		//检测是否填写了邮箱
		if(!$email){
			//提示
			echo "<center>您还没有填写邮箱地址!</center>";
		}else{
			
			//验证邮箱的规则
			$checkemail = "/^[a-z0-9]+([_\\.-][a-z0-9]+)*@([a-z0-9]+([\.-][a-z0-9]+)*)+\\.[a-z]{2,}$/i";

			//验证用户填写的是否符合规则
            if(!preg_match($checkemail, $email)){
            	//提示
                echo "<center>邮箱格式不正确，邮箱的格式可以参照 name@server.tld这样的格式!</center>";
            }else{
            	
            	//把用户提交的邮箱地址跟数据库里的邮箱地址对比
            	$res = mysql_query("SELECT * FROM `users` WHERE `email` = '".$email."'");
            	$num = mysql_num_rows($res);

            	//如果数据库没有
            	if($num == 0){
            		//提示
					echo "<center>您输入的邮箱还没有注册，请您填写您注册时候的邮箱地址!</center>";
				}else{
				
					//返回根据从结果集取得的行生成的关联数组，如果没有更多行，则返回 false。
					$row = mysql_fetch_assoc($res);

					//把密码发送到用户的邮箱里
					mail($email, '忘记密码', "这是您的密码: ".$row['password']."\n\nPlease try not too lose it again!", 'From: noreply@yourwebsitehere.co.uk');

					//display success message
					echo "<center>您的密码已经发送到您的邮箱，请及时查收!</center>";
				}
			}
		}
	}

	?>
   <div class="container">
      <form class="form-signin">
        <h2 class="form-signin-heading">忘记密码</h2>
		<input type="text" name="email" class="form-control" placeholder="电子邮件">
        <p class="mt10">
        	<button class="btn btn-primary" type="submit" name="forget">发送</button>
        </p>
      </form>
	 </div>
	 </body>
	</html>

代码里面的功能，注释很详细了，这里就不细说了。

下面的一个功能是，用户权限的一个功能，具体就是有些页面，只有用户登录才能查看，没登录的情况下，提示用户登录才能查看页面，具体代码如下：

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
 <?php
 
		//检测用户是否登陆
		if(strcmp($_SESSION['uid']," ") == 0){
			//如果没登录
			echo "<center>必须登陆，才能查看这个页面!</center>";
		}else{
 
			//更新当前的时间戳
			$time = date('U')+50;
			$update = mysql_query("UPDATE `users` SET `online` = '".$time."' WHERE `id` = '".$_SESSION['uid']."'");
			?>
			<div id="border">
				<table cellpadding="2" cellspacing="0" border="0" width="100%">
					<tr>
						<td><b>用户名:</b></td>
						<td>
						<?php
 
						//根据时间戳去取出当前在线的用户
						$res = mysql_query("SELECT * FROM `users` WHERE `online` > '".date('U')."'");
 
						//循环
						while($row = mysql_fetch_assoc($res)){
							//输出用户名
							echo $row['username']." - ";
						}
 
						?>
						</td>
					</tr>
					<tr>
						<td colspan="2" align="center"><a href="logout.php">退出</a></td>
					</tr>
				</table>
			</div>
			<?php
		}
 
		?>
	 </body>
	</html>

接下来是退出功能，这个功能和简单，只要把当前用户的session销毁掉就可以了。代码如下：

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
 
		//检测用户是否登陆
		if(strcmp($_SESSION['uid']," ") == 0){
			//如果没登录
			echo "<center>您还没登陆!</center>";
		}else{
			
			//根据SESSION把当前登录的用户名取出来
			mysql_query("UPDATE `users` SET `online` = '".date('U')."' WHERE `id` = '".$_SESSION['uid']."'");
 
			//销毁用户的SESSION
			session_destroy();
 
			echo "<center>您已经成功退出!</center>";
		}
 
		?>
	 </body>
	</html>

到这里，一个简单的登录注册就完成，经过这么一个功能的编写，对怎么运用PHP开发web功能总算是有了一个比较好的理解。继续加油~~！