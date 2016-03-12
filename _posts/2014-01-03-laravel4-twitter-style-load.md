---
layout: post
title: 用laravel4和jQuery实现一个Ajax风格的加载更多的效果(上)
categories:
- Life
tags:
- laravel
- php
---

> 今后打算通过一些实战的功能来学习laravel和PHP，今天这篇文章就来学习下用laravel和jQuery来实现一个Ajax风格的加载效果，首先需要一定基础的PHP知识以及熟悉laravel这个框架的一些基本知识。开始之前需要配置一些开发环境，这里就不再阐述了。不熟悉的可以去看看这篇文章有详细的讲解[Laravel 安装指南](https://github.com/maliang/LikeLaravel/blob/master/base/install.md)。

首先，我们还是来新建一个laravel项目，运行下面的命令(以后的学习都会以这个项目为基础)新建一个learnlv的项目:

    composer create-project laravel/laravel learnlv

安装完后，要对laravel进行一些配置，主要是数据库的配置。可以使用phpmyadmin来新建一个名为learnlv的库，laravel给我们提供了非常方便的数据库操作功能，即**migrate**，使用之前要对数据库进行配置如下所示(app/config/database.php)：

    'mysql' => array(
	  'driver'    => 'mysql',
	  'host'      => 'localhost',
	  'database'  => 'learnlv',
	  'username'  => 'root',
	  'password'  => '',
	  'charset'   => 'utf8',
	  'collation' => 'utf8_unicode_ci',
	  'prefix'    => '',
	),

我们可以使用[ Jeffrey Way](https://github.com/JeffreyWay)为laravel开发的[Laravel4 Generator](https://github.com/JeffreyWay/Laravel-4-Generators)这个工具更容易的使用migrations来操纵数据库。

首先打开**composer.json**文件，添加如下依赖:

    "require": {
	  "laravel/framework": "4.0.*",
	  "way/generators": "dev-master"
	}

运行下面的命令：

    $ composer update

然后打开**app/config/app.php**这个文件中的**providers**这个数组添加generators服务：

    'Way\Generators\GeneratorsServiceProvider',

下面就可以使用Migrations命令来进行数据库相关的操作。首先还是要安装一下migrate这个命令：

    php artisan migrate：install

接下来新建一个**message**的表，运行一下命令：

    $ php artisan generate:migration create_message_table --fields="message:string"

它会在**app/database/migrations**这个目录下生成一个Migration文件。

打开它对相关字段进行设置，这里我们把**message**设置为VARCHAR类型（在laravel中用string表示VARCHAR类型）设置如下：

    public function up()
	{
		Schema::create('message', function(Blueprint $table) {
			$table->increments('id');
			$table->string('message');
			$table->timestamps();
		});
	}

然后运行下面的命令，就会在数据库中把表建好：

    php artisan migrate

数据库建好后，在模型文件夹里新建一个**Message.php**的模型文件，代码如下：

    <?php

	class Message extends Eloquent {
	
		protected $table = 'message';
	
	}

这样我们就可以基于laravel为我们提供的Eloquent模型来操作数据库，非常好用。

接下来是相关的视图文件，首先需要把页面公共部分提取出来，我们可以在**views**目录下新建一个**layout**的文件夹用来存放公共和布局文件。

新建**header.blade.php**、**footer.blade.php**和**master.blade.php**三个文件，代码分别如下：

**header.blade.php**

    <!doctype html>
	<html>
    <head>
        <meta charset="utf-8">
        <meta name="description" content="">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <title>Untitled</title>
        {{HTML::style('css/bootstrap.min.css')}}
        <script src="//upcdn.b0.upaiyun.com/libs/jquery/jquery-1.9.0.min.js"></script>
        {{HTML::script('js/app.js')}}
        <style type="text/css">
        	*{ margin:0px; padding:0px }
			ol.timeline
			{ 
			list-style:none
			}
			ol.timeline li
			{ 
			position:relative;
			border-bottom:1px #dedede dashed; 
			padding:8px; 
			}
			.morebox
			{
			font-weight:bold;
			color:#333333;
			text-align:center;
			border:solid 1px #333333;
			padding:8px;
			margin-top:8px;
			margin-bottom:8px;
			-moz-border-radius: 6px;
			-webkit-border-radius: 6px;
			}
			.morebox a{ color:#333333; text-decoration:none}
			.morebox a:hover{ color:#333333; text-decoration:none}
			#container{margin-left:60px; width:580px }
        </style>
    </head>
    <body>

这里为了方便起见就用内联的方式来编写样式，以后的实例会基于bootstrap这个前端框架来做，所以我们需要引入bootstrap相关的样式和js文件。而我们自己编写的js代码会写在app.js这个文件。

**footer.blade.php**

    </body>
	</html>

**master.blade.php**

    @include('layout.header')
	@yield('content','coming soon')  
	@include('layout.footer')

laravel中是用内置的**blade**的模板引擎来编译PHP的，所以我们新建文件的时候要记得在文件名中写上blade的标示。

基本设置完后，为了演示功能还需要添加一些演示用的数据。使用laravel提供的**Seeder**可以很方便的添加测试数据。**seeder**主要是在**app/database/seeds**这个文件里来设置。我们会看到**DatabaseSeeder.php**的文件代码如下：

    <?php
 
	class DatabaseSeeder extends Seeder {
	 
	  /**
	   * Run the database seeds.
	   *
	   * @return void
	   */
	   public function run()
	   {
	     Eloquent::unguard();
	 
	     // $this->call('MessageTableSeeder');
	   }
	 
	}

可以看到在这个文件里有一个**run**的方法，通过这个方法我们就可以指定执行相关的seeder文件命令从而生成数据。我们新建一个**MessageTableSeeder.php**文件，代码如下：

    <?php

	class MessageTableSeeder extends Seeder {

	/**
	 * Run the database seeds.
	 *
	 * @return void
	 */
	public function run()
	{
		DB::table('message')->insert([

				['message' => 'seeder title1 ppppp'],
				['message' => 'seeder title2 ppppp'],
				['message' => 'seeder title3 ppppp'],
				['message' => 'seeder title4 ppppp'],
				['messagew' => '加载更多消息'],
				['message1' => '加载更多消息'],
				['message2' => 'seeder title6 ppppp'],
				['message3' => 'seeder title6 ppppp'],
				['message5' => 'seeder title6 ppppp'],
				['message6' => 'seeder title6 ppppp'],
				['message1' => '加载更多消息'],
				['message1' => '加载更多消息'],
				['message1' => '加载更多消息'],
				['message1' => '加载更多消息'],
				['message1' => '加载更多消息'],
				['message1' => '加载更多消息'],
				['message1' => '加载更多消息'],
				['message1' => '加载更多消息'],
				['message1' => '加载更多消息77'],
				['message1' => '加载更多消息45'],
				['message1' => '加载更多消息66'],
				['message1' => '加载更多消息'],
				['message1' => '加载更多消息'],
				['message1' => '加载更多消息1'],
			]);
		}
	
	}

这里是使用了laravel的Eloquent来插入数据的，可以去官方的文档详细了解相关的命令。然后在命令行里运行下面的命令：

    php artisan db:seed

就会在数据库中生成我插入的测试数据。准备工作就差不多做好了，下一步是正式来编写交互代码了，下一篇接着看。







    