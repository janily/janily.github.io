---
layout: post
title: 用php和Jquery制作简单的反馈系统
categories:
- Life
tags:
- php
- 翻译
---

![](http://pic.yupoo.com/reicky_v/D8Qd7ZGM/medium.jpg)

当我们新上线一个web产品的时候，及时的从用户那里得到反馈是非常重要的。但是在现实的情况是，很多网站地没有提供一个很好的反馈机制，或者用户体验非常差的反馈机制。今天，我们就来提供一个非常简单的反馈表单，来解决这个问题。主要用到了PHP和Jquery以及PHPMailer这个类来发送用户的反馈意见到服务端。

### HTML ###

首先来写一个简单HTML结构，在头部引入样式文件，底部引入javascript文件。最后引入javascript文件，对于提高页面的性能有很大的作用。

#### feedback.html ####

    <!DOCTYPE html>
	<html>
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
	<title>Quick Feedback Form w/ PHP and jQuery | Tutorialzine Demo</title>
	
	<link rel="stylesheet" type="text/css" href="styles.css" />
	
	</head>
	
	<body>
	
	<div id="feedback">

    <!-- Five color spans, floated to the left of each other -->

    <span class="color color-1"></span>
    <span class="color color-2"></span>
    <span class="color color-3"></span>
    <span class="color color-4"></span>
    <span class="color color-5"></span>

    <div class="section">

        <!-- The arrow span is floated to the right -->
        <h6><span class="arrow up"></span>Feedback</h6>

        <p class="message">Please include your contact information if you'd like to receive a reply.</p>

        <textarea></textarea>

        <a class="submit" href="">Submit</a>
    </div>
	</div> 
	
	<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.4.2/jquery.min.js"></script>
	<script src="script.js"></script>
	</body>
	</html>

在body标签中间的结构，是我们用来收集用户发送消息的数据用的。

你可能注意到了，有五个颜色类名的span。把宽度设置为20%，并且用浮动来布局。这样就刚好跟id为#feedback的宽度一致。

类名为section的区域，包括一个头部，一个文本域和按钮。如下图

![](http://pic.yupoo.com/reicky_v/D8QiBLTU/medium.jpg)

### CSS ###

接下来是样式部分，开始前首先来说说样式的结构部分。从下面的代码可以看到，每一个样式规则都是以#feedback。这样的写法，就相当于css中的命名空间，这样就可以减少样式之间的冲突。

#### styles.css – Part 1 ####

    #feedback{
    background-color:#9db09f;
    width:310px;
    height:330px;
    position:fixed;
    bottom:0;
    right:120px;
    margin-bottom:-270px;
    z-index:10000;
	}
	
	#feedback .section{
	    background:url('img/bg.png') repeat-x top left;
	    border:1px solid #808f81;
	    border-bottom:none;
	    padding:10px 25px 25px;
	}
	
	#feedback .color{
	    float:left;
	    height:4px;
	    width:20%;
	    overflow:hidden;
	}
	
	#feedback .color-1{ background-color:#d3b112;}
	#feedback .color-2{ background-color:#12b6d3;}
	#feedback .color-3{ background-color:#8fd317;}
	#feedback .color-4{ background-color:#ca57df;}
	#feedback .color-5{ background-color:#8ecbe7;}
	
	#feedback h6{
	    background:url("img/feedback.png") no-repeat;
	    height:38px;
	    margin:5px 0 12px;
	    text-indent:-99999px;
	    cursor:pointer;
	}
	
	#feedback textarea{
	    background-color:#fff;
	    border:none;
	    color:#666666;
	    font:13px 'Lucida Sans',Arial,sans-serif;
	    height:100px;
	    padding:10px;
	    width:236px;
	
	    -moz-box-shadow:4px 4px 0 #8a9b8c;
	    -webkit-box-shadow:4px 4px 0 #8a9b8c;
	    box-shadow:4px 4px 0 #8a9b8c;
	}

首先写的是#feedback这个的样式。用固定定位fixed固定在页面的底部。然后是类名为section区域的样式，和五个span的颜色。最下面的是文本域的样式。

#### styles.css – Part 2 ####

    #feedback a.submit{
    background:url("img/submit.png") no-repeat;
    border:none;
    display:block;
    height:34px;
    margin:20px auto 0;
    text-decoration:none;
    text-indent:-99999px;
    width:91px;
	}
	
	#feedback a.submit:hover{
	    background-position:left bottom;
	}
	
	#feedback a.submit.working{
	    background-position:top right !important;
	    cursor:default;
	}
	
	#feedback .message{
	    font-family:Corbel,Arial,sans-serif;
	    color:#5a665b;
	    text-shadow:1px 1px 0 #b3c2b5;
	    margin-bottom:20px;
	}
	
	#feedback .arrow{
	    background:url('img/arrows.png') no-repeat;
	    float:right;
	    width:23px;
	    height:18px;
	    position:relative;
	    top:10px;
	}
	
	#feedback .arrow.down{ background-position:left top;}
	#feedback h6:hover .down{ background-position:left bottom;}
	#feedback .arrow.up{ background-position:right top;}
	#feedback h6:hover .up{ background-position:right bottom;}
	
	#feedback .response{
	    font-size:21px;
	    margin-top:70px;
	    text-align:center;
	    text-shadow:2px 2px 0 #889889;
	    color:#FCFCFC;
	}

在第二部分的样式中，定义了按钮部分的样式。可以看到这个按钮有三种状态，用到了雪碧图submit.png和图片定位技术来制作这三种状态的样式。这三种状态分别是，正常状态，鼠标滑过状态，和正在提交数据的一个状态。当按钮的状态是正在提交数据的时候，鼠标滑过这个状态是禁止的。如下图

![](http://pic.yupoo.com/reicky_v/D8Qo3XTw/medium.jpg)

### jQuery ###

这个反馈表单有两种状态，默认是隐藏提交反馈表单部分的，当用户点击头部的时候，才显示填写反馈表单部分。这用jQuery非常容易办到，通过绑定监听点击事件加一个动画效果就可以办到。如下面的代码

    $(document).ready(function(){

    // 这个变量是用来处理用户提交数据php文件的路径
    var submitURL = 'submit.php';

    // 选择id为feedback的区域，并缓存起来
    var feedback = $('#feedback');

    $('#feedback h6').click(function(){

        // 把动画的参数用一个变量缓存起来

        var anim    = {
            mb : 0,            // Margin Bottom
            pt : 25            // Padding Top
        };

        var el = $(this).find('.arrow');

        if(el.hasClass('down')){
            anim = {
                mb : -270,
                pt : 10
            };
        }

        // 第一个动画主要是处理箭头的状态的
        // 第二个动画函数是处理表单隐藏与展现的

        feedback.stop().animate({marginBottom: anim.mb});

        feedback.find('.section').stop().animate({paddingTop:anim.pt},function(){
            el.toggleClass('down up');
        });
    });

为了保持代码的简洁，我把状态控制写到一个anim对象中，这样就可以把相关的参数更方便的给jquery方法中animate方法来控制，表单隐藏以及显示的状态主要是用箭头的状态来控制的，即down和up这两个类来控制。

接下来是用ajax和php来提交用户的反馈的数据。

     $('#feedback a.submit').live('click',function(){
        var button = $(this);
        var textarea = feedback.find('textarea');

        // 我们这里判断当按钮有working这个类的时候以及文本域里字节小于5的时候，就阻止事件继续执行

        if(button.hasClass('working') || textarea.val().length < 5){
            return false;
        }

        // 当按钮点击的时候，添加working类
        button.addClass('working');

		//这里用到jQuery里ajax放来来提交数据

        $.ajax({
            url        : submitURL,
            type    : 'post',
            data    : { message : textarea.val()},
            complete    : function(xhr){

                var text = xhr.responseText;

                // 当服务出现错误的时候，将返回一个提示文字:
                if(xhr.status == 404){
                    text = 'Your path to submit.php is incorrect.';
                }

                // 当提交数据成功的时候，把数据发送给指定的PHP文件处理

                button.fadeOut();

                textarea.fadeOut(function(){
                    var span = $('<span>',{
                        className    : 'response',
                        html        : text
                    })
                    .hide()
                    .appendTo(feedback.find('.section'))
                    .show();
                }).val('');
            }
        });

        return false;
    });
	});

我们用到了jQuery中的AJAX方法来-ajax()来提交数据，然后发送给submit.php处理。ajax方法提供$.get()和$post()两种请求方式来处理数据。

我们用到了complete回调函数（推荐使用的注意事项: jqXHR.success(), jqXHR.error(), 和 jqXHR.complete()回调从 jQuery 1.8开始 被弃用。）。jqXHR提供了status这一属性给我们使用，我们这里判断当浏览器相应状态为404错误时，就返回一个提示消息。

接着继续PHP部分了。

### PHP ###

用php通过ajax来处理数据，能够很好地提高用户体验。得打用户提交的数据后，用邮件发送给指定的账号。

#### submit.php ####

    // 设置接收数据的邮箱账号
	$emailAddress = 'me@example.com';
	
	// 使用session
	
	session_name('quickFeedback');
	session_start();
	
	// 当用户提交数据的时间小于10秒或者是在最近的一个小时内发送了10个消息，那么就提示用户等待一段时间再来提交
	
	if(	$_SESSION['lastSubmit'] && ( time() - $_SESSION['lastSubmit'] < 10 || $_SESSION['submitsLastHour'][date('d-m-Y-H')] > 10 )){
	    die('Please wait for a few minutes before sending again.');
	}
	
	$_SESSION['lastSubmit'] = time();
	$_SESSION['submitsLastHour'][date('d-m-Y-H')]++;
	
	require "phpmailer/class.phpmailer.php";
	
	if(ini_get('magic_quotes_gpc')){
	    // If magic quotes are enabled, strip them
	    $_POST['message'] = stripslashes($_POST['message']);
	}
	
	if(mb_strlen($_POST['message'],'utf-8') < 5){
	    die('Your feedback body is too short.');
	}
	
	$msg = nl2br(strip_tags($_POST['message']));
	
	// Using the PHPMailer class
	
	$mail = new PHPMailer();
	$mail->IsMail();
	
	// Adding the receiving email address
	$mail->AddAddress($emailAddress);
	
	$mail->Subject = 'New Quick Feedback Form Submission';
	$mail->MsgHTML($msg);
	
	$mail->AddReplyTo('noreply@'.$_SERVER['HTTP_HOST'], 'Quick Feedback Form');
	$mail->SetFrom('noreply@'.$_SERVER['HTTP_HOST'], 'Quick Feedback Form');
	
	$mail->Send();
	
	echo 'Thank you!';

我们用PHP中的会话管理机制来跟踪用户最近一个小时内发送的消息，以及最后一次提交数据的时间戳。如果提交的时间跟最近一次提交的数据的时间小于10秒，或者是用户在最近一个小时内提交的数据大于10，就提示用户提交过于频繁。

我们用PHPMailer这个类来处理邮件的发送。当然得下载这个类，这个类依赖PHP5，所以要确定一下你用的PHP版本。

PHPMailer这个类有些配置要设置一下，才能正常使用。这里的IsMail()则是设置使用PHP内置的邮件服务。AddAddress() 是用来设置邮件收件地址的。我们这里还设置了邮件的标题以及邮件的内容。

下面是引用我爱水煮鱼博客里的对这个类的使用的介绍，可以看看。

- require_once('class.phpmailer.php');
- require_once("class.smtp.php"); 
- $mail  = new PHPMailer(); 
- $mail->CharSet    ="UTF-8";//设定邮件编码，默认ISO-8859-1，如果发中文此项必须设置为 UTF-8
- $mail->IsSMTP();   // 设定使用SMTP服务
- $mail->SMTPAuth   = true;      // 启用 SMTP 验证功能
- $mail->SMTPSecure = "ssl";                  // SMTP 安全协议
- $mail->Host       = "smtp.gmail.com";       // SMTP 服务器
- $mail->Port       = 465;                    // SMTP服务器的端口号
- $mail->Username   = "your_name@gmail.com";  // SMTP服务器用户名
- $mail->Password   = "your_password";        // SMTP服务器密码
- $mail->SetFrom('发件人地址', '发件人名称');    // 设置发件人地址和名称
- $mail->AddReplyTo("邮件回复人地址","邮件回复人名称");  // 设置邮件回复人地址和名称
- $mail->Subject    = '';                     // 设置邮件标题
- $mail->AltBody    = "为了查看该邮件，请切换到支持 HTML 的邮件客户端"; // 可选项，向下兼容考虑
- $mail->AddAddress('收件人地址', "收件人名称");//$mail->AddAttachment("images/phpmailer.gif"); // 附件 
- if(!$mail->Send()) {echo "发送失败：" . $mail->ErrorInfo;} else { echo "恭喜，邮件发送成功！";}  

**一个简单的反馈系统就完成了！**

你可以把它集成到你项目中使用。非常容易就能让用户分享他们对你提供服务的看法。这段PHP脚本非常容易就能部署到你的项目中。

原文地址[这里](http://tutorialzine.com/2010/09/quick-feedback-form-php-jquery/)。
  



