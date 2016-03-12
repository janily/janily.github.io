---
layout: post
title: laravel实战系列：在laravel中使用RESTful API和前端MVC框架Ractive.js来构建应用
categories:
- Life
tags:
- laravel
- php
- ractivejs
---

> 在前端MVC框架越来越火热的今天，从之前的Backbone.js到现在的AngularJS、Ember.js，大家都渐渐意识到了前端框架带给开发人员的好处。但是无论AngularJS亦或Ember.js都是重量级的框架，学习曲线都很陡峭。而Ractive.js是一款简单却功能强大的JS库，它实现了模板，数据绑定，DOM实时更新，事件处理等多个有用的功能。今天我们就来学习使用laravel来构建RESTful API配合Ractive.js来构建一个简单的web应用。

在这篇文章中，我们使用laravel来创建RESTful API负责把数据JSON的格式返回给前端，而前端则利用Ractive.js来处理数据以及数据的发送。我们会通过一个发表评论的小程序来学习如何在web开发中使用RESTful风格的API和前端使用基于MVVM模式的js来开发web应用。

首先来看一下我们这个程序的特点：

- 基于laravel创建RESTful风格的API来处理数据的创建、删除和查询
- 使用Ractive.js在前端处理数据

### 首先来使用laravel来创建RESTful风格的API ###

我们需要做下面这些设置：

- 创建数据库
- 填充一些测试数据
- 创建路由
- 创建资源控制器

#### 创建数据库 ####

使用laravel的数据迁移工具创建一个**comments**的表，包括**text**和**author**字段。在终端中运行下面的命令：

	php artisan migrate:make create_comments_table --create=comments

编辑迁移文件如下：

	<?php

	use Illuminate\Database\Schema\Blueprint;
	use Illuminate\Database\Migrations\Migration;
	
	class CreateCommentsTable extends Migration {

	/**
	 * Run the migrations.
	 *
	 * @return void
	 */
	public function up()
	{
		Schema::create('comments', function(Blueprint $table)
		{
			$table->increments('id');

			$table->string('text');
			$table->string('author');
			$table->timestamps();
		});
	}

	/**
	 * Reverse the migrations.
	 *
	 * @return void
	 */
	public function down()
	{
		Schema::drop('comments');
	}

	}

然后在终端中运行**php artisan migrate**命令创建相关的表。

下面来创建Comment模型，这里依然是使用了laravel中的Eloquent Model这强大ORM，在**app/models**目录下创建**comment.php**文件，编辑代码如下：

	<?php

	
	class Comment extends Eloquent {
		protected $fillable = array('author', 'text'); 
	}

下面是来填充测试数据，在**app/database/seeds**目录下创建**CommentTableSeeder.php**文件，代码如下：

	<?php

	class CommentTableSeeder extends Seeder 
	{

	public function run()
	{
		DB::table('comments')->delete();

		Comment::create(array(
			'author' => 'Chris Sevilleja',
			'text' => 'Look I am a test comment.'
		));

		Comment::create(array(
			'author' => 'Nick Cerminara',
			'text' => 'This is going to be super crazy.'
		));

		Comment::create(array(
			'author' => 'Holly Lloyd',
			'text' => 'I am a master of Laravel and Ractive.'
		));
	}

	}

然后打开**app/database/seeds/DatabaseSeeder.php**文件，编辑代码如下：

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

		$this->call('CommentTableSeeder');
	}

	}

接着在终端中运行下面的命令填充测试数据：

	php artisan db:seed

创建完数据库后，我们使用laravel中的[资源控制器](http://www.golaravel.com/docs/4.1/controllers/#resource-controllers)来构建RESTful风格的API。在终端下运行下面的命令：

	php artisan controller:make CommentController --only=index,store,destroy

在我们这个程序中，只需要处理上面这三个资源的操作。因为laravel只需要把把数据以JSON的形式返回给我们就可以了，其它的我们在前端用javascript来处理。

使用laravel来处理返回数据的格式也非常容易，完整的控制器代码如下：

	<?php

	class CommentController extends \BaseController {

	/**
	 * Send back all comments as JSON
	 *
	 * @return Response
	 */
	public function index()
	{
		return Response::json(Comment::get());
	}

	/**
	 * Store a newly created resource in storage.
	 *
	 * @return Response
	 */
	public function store()
	{
		Comment::create(array(
			'author' => Input::get('author'),
			'text' => Input::get('text')
		));

		return Response::json(array('success' => true));
	}

	/**
	 * Remove the specified resource from storage.
	 *
	 * @param  int  $id
	 * @return Response
	 */
	public function destroy($id)
	{
		Comment::destroy($id);

		return Response::json(array('success' => true));
	}


	}

可以看到使用laravel中的Eloquent非常容易处理数据的增删查改。下面来定义路由。

### 路由 ###

要使用laravel的API来处理数据。我们需要定义好路由来根据我们定义好的API来发送数据以及渲染模板。

我们会在路由中为URL添加一个**api**的前缀，这样如果我们要获取所有的数据，就要使用**http://example.com/api/comments**这样的URL来访问。最后路由的代码如下所示：

	<?php

	/*
	|--------------------------------------------------------------------------
	| Application Routes
	|--------------------------------------------------------------------------
	|
	| Here is where you can register all of the routes for an application.
	| It's a breeze. Simply tell Laravel the URIs it should respond to
	| and give it the Closure to execute when that URI is requested.
	|
	*/
	
	Route::get('/', function()
	{
		return View::make('index');
	});
	
	
	// api 路由
	Route::group(array('prefix' => 'api'), function() {
	
		// 分别对应方法来处理数据
		Route::resource('comments', 'CommentController', 
			array('only' => array('index', 'store', 'destroy')));
	});
	
	// 当控制器中没有任何方法匹配请求时，就会调用一个全局响应的方法。这个方法命名为 missingMethod ，它接收请求的参数数组作为方法的唯一参数。
	App::missing(function($exception)
	{
		return View::make('index');
	});

我们可以在终端中运行**php artisan routes**命令来查看所有的方法的路径。如下图所示：

![](http://pic.yupoo.com/reicky_v/DK7mX3QF/gT6Ul.png)

我们可以看到创建评论、显示评论以及删除评论方法对应的方法。使用上面的路径我们就可以使用Ractive.js来处理数据。

Ok，后端的部分就完成了，下面我们继续来完成前端部分的代码。

我们这里的前端部分是使用了Ractive.js官网示例里面评论例子的模板，具体地址[这里](http://examples.ractivejs.org/comments)。

> 这里需要注意一下，由于laravel中的**blade**模板引擎和**Ractive.js**的使用的语法都是两个大括号，如果不做处理的话，那laravel也会解析实际上是使用**Ractive.js**写的代码，那就会出现错误了。对于这种情况，我们可以去在blade或者是ractive.js的配置文件里改变它们的默认语法。不过我们这里可以在符号前面添加一个**@**符号，这样blade就不会解析它，而ractive.js则会正确解析它。

我们在**app/views**目录下新建一个**index.blade.php**视图文件，代码如下图所示：

![](http://pic.yupoo.com/reicky_v/DKyKZNoW/mE5MD.jpg)

然后在**public**目录下的css文件夹里新建一个style.css文件，输入以下代码：

	h3, h4 {
	font-family: 'Voltaire';
	}
	
	h4 {
		margin: 0;
	}
	
	.comment-block {
		position: relative;
		padding: 0 0 0 10em;
		margin: 0 0 1em 0;
	}
	
	.comment-author {
		position: absolute;
		left: 0;
		top: 0;
		width: 9em;
		background-color: #eee;
		padding: 0.5em;
	}
	
	.comment-text {
		position: relative;
		width: 100%;
		height: 100%;
		border-top: 1px solid #eee;
		padding: 0.5em 0.5em 0.5em 1em;
		box-sizing: border-box;
		-moz-box-sizing: border-box;
	}
	
	.comment-text p:last-child {
		margin: 0;
	}
	
	form {
		position: relative;
		padding: 0 0 0 10.5em;
	}
	
	.author-input {
		position: absolute;
		left: 0;
		top: 0;
		font-size: inherit;
		font-family: inherit;
		width: 10em;
		padding: 0.5em;
		margin: 0;
		border: 1px solid #eee;
		box-shadow: inset 1px 1px 3px rgba(0,0,0,0.1);
		box-sizing: border-box;
		-moz-box-sizing: border-box;
	}
	
	textarea {
		font-size: inherit;
		font-family: inherit;
		width: 100%;
		height: 5em;
		padding: 0.5em;
		border: 1px solid #eee;
		box-shadow: inset 1px 1px 3px rgba(0,0,0,0.1);
		box-sizing: border-box;
		-moz-box-sizing: border-box;
	}
	
	input[type="submit"] {
		/*appearance: none;*/
		background-color: #729d34;
		border: none;
		padding: 0.5em;
		font-size: inherit;
		font-family: 'Voltaire';
		color: white;
		opacity: 0.5;
		cursor: pointer;
	}
	
	input[type="submit"]:hover, input[type="submit"]:focus {
		opacity: 1;
		outline: none;
	}

接着在**public**目录下的js文件夹里新建一个app.js文件，输入以下代码：

	var ractive,data,datas;

	$.ajax({
	  type:"get",
	  url:"/api/comments",
	  dataType: "json",
	  async: false,
	  success:function(data){
	    datas = data;
	  }
	});
	ractive = new Ractive({
	  el: 'container',
	  template: '#comment-template',
	  noIntro: true, 
	  data: {
	    comments: datas
	  }
	});
	
	ractive.on( 'post', function ( event ) {
	  var comment;
	
	  // 阻止页面重新加载
	  event.original.preventDefault();
	
	  // 获取输入的数据
	  comment = {
	    author: this.get( 'author' ),
	    text: this.get( 'text' )
	  };
	
	  this.get( 'comments' ).push( comment );
	
	  // 重置
	  document.activeElement.blur();
	  this.set({ author: '', text: '' });
	
	  // 发送数据到服务端，存入数据库
	  this.fire( 'new comment', comment );
	
	  $.post( '/api/comments', comment);
	});

上面的代码很容易理解。首先是定义了几个后面需要用到的变量，然后是使用jQuery中的**$.ajax()**方法来从服务端获取Json数据，这里需要注意一点的是我们这里设置获取数据的方法是同步，即**async: false**。然后利用在模板中绑定的**post**事件，当触发事件的时候，先把数据更新到dom中，然后使用**$.post()**方法把数据发送给服务端来处理并把数据存入到数据库中。运行结果如下图所示：

![](http://pic.yupoo.com/reicky_v/DKzjAj2c/JA000.jpg)

[源代码地址](https://github.com/janily/laravel-rest)

OK，一个简单的评论系统就完成了。当然还有很多地方可以完善，比如我们还可以基于这个评论功能，添加一些其它的功能模块如：编辑评论、用户等。just do it!  










