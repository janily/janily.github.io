---
layout: post
title: 用laravel4来实现重设密码的功能
categories:
- Life
tags:
- laravel
- php
---

> 前面有写过用laravel实现了一个用户注册系统。注册认真是一个具有社交属性网站的两个基本的功能。一般一个web应用的注册验证系统还具备第三个功能就是能够让用户重设密码，比如当用户忘记了密码的时候，可以提醒用户来重设密码。本篇文章就来用laravel来实现重设密码的功能。代码是在以前的用户验证系统的代码基础上来编写的。开始之前可以去先看看关于用laravel用户验证的[这篇文章](http://janily.gitcafe.com/life/2013/11/16/laravel-auth/)。

在laravel4有提供这样的功能，实现起来非常容易。

### remindable接口 ###

在开始之前我们应该确保我们的模型已经启用了laravel的remindable接口，在我们的**User**模型代码应该是下面这样的：

    class User extends Magniloquent implements UserInterface, RemindableInterface {}

相信大部分的开发者都明白重设密码这个功能的逻辑，这里我们还是要说说在laravel中是如何来实现的。

- 首先提醒用户重设密码的邮件是发送到用户注册的邮箱地址
- 重设密码会生成一个密令存储在数据库中并且会生成一个创建时的时间戳
- 发送给用户的重设密码的链接会以邮件的方式发送。并且会把我们生成的密令作为一个参数发送给链接
- 当用户点击链接的时候，就会跳转到用户重设密码的界面
- 如果密令是正确的，就可以允许用户重设密码

### 准备工作 ###

看了上面的一个实现过程，我们来看看我们需要做一些什么的准备工作：

#### 数据库创建 ####

我们需要创建一个存储密令的数据表。把它存储到用户表不是一个好主意，显然laravel为我们考虑到了这一点，所以它会自动帮我们生成一张存储密令的数据表。

#### 路由 ####

需要创建两个路由分别用来获取数据和返回结果。

#### 控制器 ####

我们需要创建一个控制器用来控制修改密码的相关方法和显示

#### 视图 ####

最后当然需要创建一个重设密码的视图，引导用户设置密码。

### 创建密令表 ###

在上面有说到laravel有为我们提供了自动创建重设密码表的命令。非常方便。

在命令行里运行下面的命令：

    php artisan auth:reminders

会生成一个migration文件：

    <?php
 
		use Illuminate\Database\Migrations\Migration;
		 
		class CreatePasswordRemindersTable extends Migration {
		 
		  /**
		   * Run the migrations.
		   *
		   * @return void
		   */
		  public function up()
		  {
		    Schema::create('password_reminders', function($t)
		    {
		      $t->string('email');
		      $t->string('token');
		      $t->timestamp('created_at');
		    });
		  }
		 
		  /**
		   * Reverse the migrations.
		   *
		   * @return void
		   */
		  public function down()
		  {
		    Schema::drop('password_reminders');
		  }
		 
		}

然后运行：

    php artisan migrate

### 创建重设密码表单 ###

我们需要创建一个表单让用户输入邮件地址，从而给用户发送重设密码的链接。

首先在**routes.php**文件中创建一个新的路由：

    Route::get('password/reset', array(
	  'uses' => 'PasswordController@remind',
	  'as' => 'password.remind'
	));

这里我们创建了一个简单的GET方法的路由并且和控制器里的方法关联起来。

下一步是创建一个名为**PasswordController.php**，代码如下：

    class PasswordController extends BaseController {
 
	  public function remind()
	  {
	    return View::make('password.remind');
	  }
	}

可以看到在上面的控制器的方法里面，我们指定了一个视图模板来显示信息。

需要在视图目录下新建一个**password**的文件夹以及在里面新建一个名为**remind.blade.php**的视图文件，代码如下：

![](http://pic.yupoo.com/reicky_v/DrRIvlXQ/medium.jpg)

这里用到了laravel为我们提供的表单的一些方法，可以去[这个地址](http://www.golaravel.com/docs/4.1/html/)了解它的详细的用法。上面的视图会生成一个让用户输入它们的邮箱地址的文本框和一个确定按钮。当用户提交的时候，它会直接把数据POST到**password.request**这个路由。

### 发送邮件 ###

下一步是设置另外一个路由，用来响应用户发送过来的表单数据，回到路由文件**routes.php**，添加以下的代码：

    Route::post('password/reset', array(
	  'uses' => 'PasswordController@request',
	  'as' => 'password.request'
	));

然后在**PasswordController.php**文件，添加下面这个方法：

    public function request()
	{
	  $credentials = array('email' => Input::get('email'));
	 
	  return Password::remind($credentials);
	}

这个方法非常简单，就是用来获取用户添加的邮箱地址并发送邮件。当然可能会提醒你还没有设置邮件服务器地址的报错信息。你可能需要在**app/config/mail.php**设置相关的信息来发送邮件，可以去看我以前写的关于用laravel发送邮件的[这篇文章](http://janily.gitcafe.com/life/2013/12/29/laravel4-sendmail/)。

要发送邮件你得设置相关的SMTP服务器。现在有很多第三方提供发送邮件的服务，比如[SendGrid](http://sendgrid.com/)这个也很不错。

如果你想知道默认的邮件模板是咋样的，你可以去这个目录看看**app/views/auth/reminder.blade.php**。你可以任意改变这个模板，只需要url地址正确就可以了。

### 创建重置表单 ###

下面是创建重置表单这部分的功能。

首先还是从路由开始：

    Route::get('password/reset/{token}', array(
	  'uses' => 'PasswordController@reset',
	  'as' => 'password.reset'
	));

这个路由需要在url后面传送一个安全的密令参数。这个参数laravel会为我们自动生成。

下面在**PasswordController**控制器里编写reset的方法：

    public function reset($token)
	{
	  return View::make('password.reset')->with('token', $token);
	}

你可能会注意到我们在reset方法里传递了一个$token的参数，我们是通过视图来传递的。

最后在**app/views/password**目录里创建一个**reset.blade.php**视图文件添加下面的代码：

![](http://pic.yupoo.com/reicky_v/DrRIwKD0/medium.jpg)	

这也是一个简单的表单。主要让用户设置新的密码的。

在上面的表单里面你可以看到，我们这里设置了一个隐藏的文本域是用来接收路由传递过来的**$token**参数用的。这个参数可以用来跟数据库中的$token做比较的。

### 更新设置密码 ###

最后一部分就是用来重新设置用户设置的密码。

首先是路由：

    Route::post('password/reset/{token}', array(
	  'uses' => 'PasswordController@update',
	  'as' => 'password.update'
	));

然后在**Password**控制器里编写update方法：

    public function update()
	{
	  $credentials = array('email' => Input::get('email'));
	 
	  return Password::reset($credentials, function($user, $password)
	  {
	    $user->password = Hash::make($password);
	 
	    $user->save();
	 
	    return Redirect::to('login')->with('flash', 'Your password has been reset');
	  });
	}

laravel也为我们提供重置密码的方法。如果设置成功，laravel会为你自动重新设置并保存密码，并且返回一个设置成功的消息。

laravel提供的重置密码的方法会自动验证相关的请求，它会验证确保用户发送过来的$token和数据库中的$token相匹配。如果检测到用户输入有错误，它会返回一个错误的提示信息。

我们只需要做一点点的工作就能把密码重置的功能搞定，laravel4会帮我们处理相关的的逻辑和细节方面的问题，并且可以在任意的项目中使用它，而不用重新发明轮子。


