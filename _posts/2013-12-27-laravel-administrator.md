---
layout: post
title: laravel学习之用Laravel Administrator来开发后台管理系统
categories:
- Life
tags:
- php
- laravel
---

> php学习了一段时间，主要是学习了用laravel这个php框架来进行web开发，这个框架真心不错。期间用laravel开发了一个基于google地图外卖的web应用，基本功能已经开发完了。后面打算把整个开发过程中的写成一个系列来总结在开发过程中的点点滴滴。这篇文章主要是来说说怎么用Laravel Administrator来快速制作一个后台管理系统。Laravel Administrator是laravel的一个三方的包，而且它充分利用了laravel的数据库的Eloquent模型的特点来管理和创建数据，非常方便。这里是Laravel Administrator的官方文档的地址[Laravel Administrator文档地址](http://administrator.frozennode.com/)。本文要求有一定的php和laravel的基础知识。

开始之前，首先要安装好Laravel Administrator这个包，这里是用PHP开发中的composer来进行第三方的库类的管理。laravel这个框架也是基于composer来进行开发工作的，首先在你的composer.json文件中的**require**这个json数组里面添加如下代码：

    "require": {
    "laravel/framework": "4.0.*",
    "frozennode/administrator": "dev-master"
	}

然后在命令行里面运行**composer update**来进行安装，安装后还需要打开**app/config/app.php**这个文件，注册服务提供者到 app/config/app.php的providers数组:

    'providers' => array(
    'Frozennode\Administrator\AdministratorServiceProvider',
	)

我这里主要是基于laravel4来进行开发的，如果你用的laravel的版本是3的话，安装方法还是有点不一样的，可以去官方文档看关于laravel3中的Laravel Administrator的安装方法，这里就不再阐述了。

然后运行下面这条命令：

    php artisan config:publish frozennode/administrator  

它会生成一个文件：

    app/config/packages/frozennode/administrator/administrator.php  

这就是我们后台的管理和配置文件啦,如下图所示：

![](http://pic.yupoo.com/reicky_v/DpR1oWe4/medium.jpg)

接下来我们就来配置一下后台管理系统，下面是我的配置代码为了方便起见，我把相关的注释删除掉了。

    <?php

	return array(

	'uri' => 'admin',   //后台访问地址

	'title' => '后台管理',    //标题设置

	'model_config_path' => app('path') . '/config/administrator',   //设置文件的地址

	'settings_config_path' => app('path') . '/config/administrator/settings',

	'menu' => array('topics','books'),   //这个就是我们来设置自己后台管理的具体栏目页面的一个导航

	'permission'=> function()     //这里是设置用户验证的，我们这里方便起见，就没有设置用户验证
	{
		return true;
	},

	'use_dashboard' => false,     //这里是设置管理后台的主页面的，如果值为false，则是把home_page的值设置为主页面

	'dashboard_view' => '',

	'home_page' => 'topics',     //这里设置后台的主页面

	'login_path' => 'user/login',   //登陆页面路径设置

	'logout_path' => false,

	'login_redirect_key' => 'redirect',

	'global_rows_per_page' => 20,

	'locales' => array('en','zh-CN'),    //设置后台管理界面的语言，这里设置了英文和中文

	);

设置完后，由于我们设置了两个导航栏目，所以我们要新建两个页面，我们在**app/config**这个路径下新建一个文件夹**administrator**然后在里面新建两个文件**books.php**和**topics.php**两个文件，如下图所示：

![](http://pic.yupoo.com/reicky_v/DpRTQRBP/medium.jpg)

然后在数据库中新建三张表，这个可以用laravel提供migrations来创建数据库非常方便。

    php artisan migrate:make add_books_topics

运行后会生成一个文件：

![](http://pic.yupoo.com/reicky_v/DpRXrl80/medium.jpg)

修改代码如下：

    public function up()
	{
		//
		Schema::create('books',function($table){
		    $table->increments('id');
		    $table->string('name',50);
		    $table->string('author',50);
		    $table->timestamps();
		});
		
		Schema::create('topics',function($table){
		    $table->increments('id');
		    $table->string('name',50);
		    $table->timestamps();
		});
		
		Schema::create('books_topics',function($table){
		    $table->increments('id');
		    $table->integer('book_id')->unsigned();
		    $table->integer('topic_id')->unsigned();
		    $table->timestamps();
		});
	}

然后运行下面的命令：

    php artisan migrate

就会自动帮你把相关的表建好。我们新建了三个表并且用**books_topics**这个表把**books**和**topics**这两个表关联起来。在laravel4中Eloquent使得管理和处理这些关系变得简单。所以我们需要在**models**文件夹里新建**Book.php**和**Topic.php**两个模型文件来处理它们之间的关系。

![](http://pic.yupoo.com/reicky_v/DpS5m28Q/medium.jpg)

Book.php文件代码如下:

    class Book extends Eloquent {

	public function topics()
	{
		return $this->belongsToMany('Topic','books_topics');
					
	}
	}

Topic.php文件代码如下：

    class Topic extends Eloquent {

	public function books()
	{
		return $this->belongsToMany('Topic','books_topics');
	}
	}

我们这里使用了laravel4中的belongsToMany函数来定义多对多关系。

基本的准备工作完成后，接下来就是来配置Administrator了。还记得我们上面新建的两个文件**books.php**和**topics.php**么，接下来就是用administrator提供的选配来配置我们后台页面所需要的一些功能。

首先来配置**topics**这个文件，代码如下：

    <?php


	return array(
	    /**
	     * 标题
	     *
	     * @type string
	     */
	    'title' => 'Topics',
	    
	    /**
	     * 内容
	     *
	     * @type string
	     */
	    'single' => 'topic',
	    
	    /**
	     * 这里是指定我们定义的eloquent的模型
	     *
	     * @type string
	     */
	    'model' => 'Topic',
	    
		/**
	     * columns是用来设置显示内容用的
	     *
	     * @type string
	     */
	    'columns' => array(
	        'name' => array(
	            'title' => 'Name',
	        )
	    ),
	    
		//可编辑的字段
	    'edit_fields' => array(
	    'name' => array(
	        'title' => 'Name',
	        'type' => 'text'
	        ),
	    )
	);

下面是Books的代码：

    <?php


	return array(
	    /**
	     * Model title
	     *
	     * @type string
	     */
	    'title' => 'Books',
	    
	    /**
	     * The singular name of your model
	     *
	     * @type string
	     */
	    'single' => 'book',
	    
	    /**
	     * The class name of the Eloquent model that this config represents
	     *
	     * @type string
	     */
	    'model' => 'Book',
	    
	    'columns' => array(
	        'name' => array(
	            'title' => 'Name',
	        )
	    ),
	    
	    'edit_fields' => array(
	    'name' => array(
	        'title' => 'Name',
	        'type' => 'text'
	        ),
	     'topics' => array(
	        'type' => 'relationship',
	        'title' => 'Topics',
	        'name_field' => 'name',
	    ),
	    )
	);

跟上面的差不多，这里有一个relationship，这里是用来关联表的，这个东东不错哦，像在web开发中经常有这样的需求，表与表之间需要相互关联。我们这里是把Book里面的内容跟Topics关联起来。

OK，现在我们一个简单的后台管理系统就开发完毕。由于我是在本地url.dev这个应用上开发的，所以在的后台地址是url.dev/admin，下面就是我浏览的样子：

![](http://pic.yupoo.com/reicky_v/DpSk12mB/medium.jpg)

Administrator就介绍到这里啦，其实Administrator提供了很多的配置选型，可以去它官方的[文档地址看看](http://administrator.frozennode.com/docs/introduction)，使用它可以大大提高我们的开发效率。还能提高我们的开发体验。
