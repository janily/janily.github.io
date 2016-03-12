---
layout: post
title: 用laravel4来编写邮箱验证激活账号的功能
categories:
- Life
tags:
- laravel
- php
---

> 这篇文章是前面[larvavel4开发web app(2)：用户验证系统学习](http://janily.gitcafe.com/life/2013/11/16/laravel-auth/)的一个补充，开始之前建议先看看前面的那篇文章。因为这篇文章的代码是以前面文章代码为基础来编写。还用到了laravel的邮件功能。在之前的文章都有介绍。

我们前面那篇文章已经建好了一个用户的表，为了能够实现邮箱验证激活的功能需要在用户表中新添加三个字段分别为**email**、**status**和**activation**。**status**字段是用来标示用户的账号是否激活，默认值是0标示未激活，如果值是1的话就表示账号已经激活了。**activation**字段是用来存放账号激活码的。如下图所示：

![](http://pic.yupoo.com/reicky_v/DquGd2Pj/medium.jpg)

数据库调整好后，接下来需要调整下登录的视图模板添加一个email字段，代码如下：

![](http://pic.yupoo.com/reicky_v/DquRTHoa/medium.jpg)

接下来在**UsersController**里的postCreate方法里的验证代码调整如下：

      if ($validator->passes()) {

		   $user = new User;
		   $user->username = Input::get('username');
		   $user->password = Hash::make(Input::get('password'));
		   $user->email = Input::get('email');
		   $user->activation = Hash::make(Input::get('email').time());
		   $user->save();
		   $data = array(
		   		'username' => Input::get('username'), 
		   		'activation' => $user->activation,

		   	);
		    Mail::send('emails.hello', $data, function($message){
      		$message->to(Input::get('email'), Input::get('username'))->subject('Welcome to the Laravel 4 Auth App!');
   			});
		 
		   return Redirect::to('users/login')->with('message', 'Thanks for registering!,请登录您邮箱激活您的账号。');
		} else {
		   return Redirect::to('users/register')->with('message', 'The following errors occurred')->withErrors($validator)->withInput();
		}

这里就是在先前的代码上加了邮件发送和保存**email**和**activation**字段数据这两个功能。其中**activation**的值我们是把email和时间戳混合在一起并用Hash的方法加密。然后再发送邮件到用户注册的邮箱里。
    
laravel中，邮件的模板是放在视图目录下emails文件夹下的，邮件的模板的代码如下:

	<!doctype html>
	<html>
	    <head>
	        <meta charset="utf-8">
	        <meta name="description" content="">
	        <meta name="viewport" content="width=device-width, initial-scale=1">
	        <title>mail</title>
	    </head>
	    <body>
	        <div style="font-size:28px;color:green;">
	        	<h1>Hi, {{ $username }}!</h1>
	 
	            <p>We'd like to personally welcome you to the Laravel 4 Authentication Application. Thank you for registering!</p>
	            <a href="http://localhost:8000/users/activation?activation={{ $activation }}">http://localhost:8000/users/activation?activation={{$activation}}</a>
	        </div>
	    </body>
	</html>

在邮件模板中我们主要是把一个激活的链接发送给用户。这里要注意的一点的是，需要把**activation**的值传给链接，这样我们才能验证用户并激活账号。

最后就是验证功能的编写，在**UsersController**控制器里添加一个**getActivation**的方法，代码如下：

    public function getActivation() {
		if(!empty($_GET['activation']) && isset($_GET['activation']))
		{
			$code=mysql_real_escape_string($_GET['activation']);
			$user_count = User::where('activation',$code)->count();
			if($user_count > 0)
			{

				$count=DB::table('users')->where('activation',$code)->where('status','0')->count();
				if($count == 1)
				{
				    $db_res = DB::table('users')->where('activation',$code)->update(array('status' => 1));
				    if($db_res == 1){
				    	return Redirect::to('users/login')->with('message','您的账号已经激活');
					}
			    }
			    else
			    {
					return Redirect::to('users/login')->with('message', '您的账号已经激活无需再次激活!');
			    }

		    }
		    else
		    {
				return Redirect::to('users/register')->with('message', '您的账号存在异常!');
		    }
		}
	}

首先我们要判断用户激活的链接是否有activation这个值。还记得我们上面的激活链接么**http://localhost:8000/users/activation?activation={{$activation}}**。而**$_GET['activation']**就是用来获取链接传过来activation的值的。如果值存在，则进一步查询数据库把activation值和数据库中的activation值进行比较，如果用户账号的**status**的值是0的话则把status的值改为1，并跳转到登陆页面提示用户账号已经激活。如果是已经激活的账号则提示用户账号已经激活。如下图所示：

![](http://pic.yupoo.com/reicky_v/Dqv5fIda/medium.jpg)

OK，一个简单的账号激活功能就完成了。当然还有很多可以完善的地方，比如用户输入的邮箱的验证等数据合法性功能的完善。





    

    