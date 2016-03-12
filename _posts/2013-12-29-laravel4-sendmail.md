---
layout: post
title: 用laravel4来发送邮件
categories:
- Life
tags:
- laravel
- php
---

发送邮件对于一个web应用来说是非常重要的部分。通常邮件一般用于通知用户关于网站的一些更新的信息或者是活动信息，比如，对于博客来说更新了文章就要用邮件及时通知用户让用户知晓。这篇文章就来说说在laravel4中发送邮件的一些基础知识。

在用laravel4发送邮件之前，要在**app/config/mail.php**这个文件中进行一些设置，下面是这些选项的一个具体说明：

- driver 是设置邮件的发送方式。默认是smtp的方式，当然也支持
- host  是发送邮件的主机地址
- port  smtp发送邮件的端口
- from 设置邮件发送方的地址
- encryption 加密方式
- username 你的smtp的用户名
- password 你的smtp的密码
- sendmail 指定发送邮件的程序
- pretend 这个是用于在开发的时候调试发送邮件用的。当它的值设置为**true**的时候，邮件不会真的发送，而是会记录在本地的日志文件中

那我们用gmail来测试一下用laravel发送邮件。需要你有一个gmail的邮箱账号才能使用。让我们来编辑**mail.php**文件。代码如下：

    return array(
 
	   'driver' => 'smtp',
	 
	   'host' => 'smtp.gmail.com',
	 
	   'port' => 587,
	 
	   'from' => array('address' => 'authapp@awesomeauthapp. com', 'name' => 'Awesome Laravel 4 Auth App'),
	 
	   'encryption' => 'tls',
	 
	   'username' => 'your_gmail_username',
	 
	   'password' => 'your_gmail_password',
	 
	   'sendmail' => '/usr/sbin/sendmail -bs',
	 
	   'pretend' => false,
 
	);

这里设置了用gmail来作为我们的邮件发送服务器，需要注意的是你要把你gmail的账户和密码填到对应的栏目才能正确发送。这里把pretend的值设置为false。

然后就可以使用laravel内置的**Mail::send **方法来发送邮件，我们这里给一个实例代码如下：

    Mail::send('emails.hello',$data,function($message){  //queue
		$message->to('350600239@qq.com')->subject('hello world');
		//$message->attach(public_path().'/images/hero-unit.jpg');附件
	});

	return '发送成功';

传入send方法的第一个参数为生成邮件体所用的视图名。第二个参数$data是要传入视图的数据，第三个参数为闭包，允许你为邮件配置各种选项。具体详细的可以去官方的文章看看，这里是中文的[地址](http://www.golaravel.com/docs/4.1/mail/)。

你还可以设置在发完邮件后的返回一个发送成功的提示，比如我这里就设置了发送成功的提示，如下图所示：

![](http://pic.yupoo.com/reicky_v/Dq3C9xMi/medium.jpg)

当然一个漂亮的邮件模板对于给用户留下一个好的印象也是非常重要的，mailclimp给我们提供了一些非常精美的[邮件模板](http://mailchimp.com/resources/html-email-templates/)。

对于web应用来说，给用户发送邮件几乎是一个标配的功能。现在有很多专业提供发送邮件服务的第三方的邮件服务应用。比如[mailchimp](http://mailchimp.com/)就是一个非常优秀的第三方的邮件服务商。它提供了很多了的功能，用它来管理web应用用户邮箱是一个非常好的选择。

而且有第三方的集成包把mailchimp封装成了一个类，使用起来非常方便。

下篇文章再详细的说说它在laravel4中的使用方法。