---
layout: post
title: laravel实战系列：使用laravel来开发一个短链接应用
categories:
- Life
tags:
- laravel
- php
---

> 最近在看一本关于laravel实战的一本书**Laravel Application Development Blueprints - Kilidagi, Arda**，这本书非常不错，没有枯燥的一些理论知识，全部是一些实战型的例子的讲解。前面已经学习了很多的关于laravel一些基本知识，这本书不但可以巩固laravel的基础知识，最重要的是运用laravel的知识从实战出发，开发一些小型应用的项目。达到融会贯通的目的。现在把在看书学习的一个过程记录下来...。

这篇文章，我们来学习使用laravel来开发一个短链接应用。

这部分主要用到的知识有以下内容：

- 数据库迁移
- 创建表单
- 创建模型
- 保存数据
- 查询并显示数据

### 迁移 ###

我们这里使用mysql为默认的数据库。在使用数据库迁移之前，我们先在数据库中创建一个urls的数据库，然后在**app/config/database.php**文件中配置数据库相关的信息，如下所示：

	'mysql' => array(
			'driver'    => 'mysql',
			'host'      => 'localhost',
			'database'  => 'urls',
			'username'  => 'root',
			'password'  => '',
			'charset'   => 'utf8',
			'collation' => 'utf8_unicode_ci',
			'prefix'    => '',
	),

然后使用下面的命令来创建数据库迁移：

	php artisan migrate:make create_links_table --create=links

--create这里表示创建一个新的表名。

在这个**app/database/migrations**目录下打开我们的迁移文件**2014_04_28_031151_create_links_table.php**，编辑并写入下面的代码：
	...

	/**
	 * Run the migrations.
	 *
	 * @return void
	 */
	public function up()
	{
		Schema::create('links', function(Blueprint $table)
		{
			$table->increments('id');
			$table->text('url');
			$table->string('hash',400);
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
		Schema::drop('links');
	}
	...

然后运行下面的命令：

	php artisan migrate

就会在数据库中生成links这张表。

### 创建视图 ###

我们这个短链生成器的界面非常简单，就一个form表单，在**app/views**下面创建**一个form.blade.php**文件，代码如下所示：

![](http://pic.yupoo.com/reicky_v/DIrz6JJR/7J7gx.png)

从上面的代码中可以看到，我们使用了laravel为我们提供的HTML方法来引入CSS文件，其实HTML方法还可以做很多的事情，更多的关于HTML方法可以去[这里](http://www.laravel-tricks.com/tricks/generating-html-using-html-methods)看看。

然后是CSS文件，我们在public目录下新建一个CSS目录，再新建一个**style.css**文件，代码如下：

    div#container{padding-top:100px;text-align:center;width:75%;margin:auto;border-radius:4px}
	div#container h2{font-family:Arial,sans-serif;font-size:28px;color:#555}
	div#container h3{font-family:Arial,sans-serif;font-size:28px}
	div#container h3.error{color:#a00}
	div#container h3.success{color:#0a0}
	div#container input{display:block;width:90%;float:left;font-size:24px;border-radius:5px}
	div#error,div#success{border-radius:3px;display:block;width:90%;padding:10px}
	div#error{background:#ff8080;border:1px solid red}
	div#success{background:#80ff80;border:1px solid #0f0}

最后的样子如下图所示：

![](http://pic.yupoo.com/reicky_v/DIrDcSGZ/UXzRC.png)

下面我们来详细的分析下视图文件中的表单部分的代码。

- Form::open()是来创建表单，第一个参数是用来指定处理数据的方法。可以是一个url地址，或者是已定义的路由和控制器来操作数据。
- Form::text()方法使用来创建类型为text的input元素。第一个参数是指定input的name属性；第二个参数是指定input的默认的value属性；第三个参数还接受以数组的形式来指定input元素的其它属性，比如class或者是id等。
- Input::old()它会返回我们上一次提交数据的值给input的值。有助于提升用户体验，比如我们这里如果用户输入的值不是合法的url地址，那它会返回用户这个不合法的值，用户可以快速修改，而不用再重新输入。
- Form::close()关闭form表单。

### 创建模型 ###

laravel为我们提供非常好用的ORM即**Eloquent**，接下来我们在**app/models**目录创建一个**Link.php**文件来定义Link模型：

	<?php

	class Link extends Eloquent {
		protected $table = 'links';
		protected $fillable = array('url','hash');
		public $timestamps = false;
	}

上面的代码，主要是定义了下面这些东东“

- 指定模型中操作的表为links。
- 当创建一个新的模型，您可以传递属性的数组到模型的构造函数。这些属性将通过集体赋值分配给模型。这是很方便的，但把用户的输入盲目地传给模型可能是一个严重的安全问题。如果把用户输入盲目地传递给模型，用户可以自由地修改任何或者全部模型的属性。基于这个原因，默认情况下所有 Eloquent 模型将防止集体赋值。我们这里使用了**fillable**来指定可以集体赋值的字段。
- public $timestamps = false;表示不更新时间戳。

### 路由 ###

现在我们在**routes.php**文件中来定义我们应用方位地址:

	Route::get('/', function()
	{
		return View::make('form');
	});

### 保存数据 ###

由于我们这个应用非常简单，我们可以直接在路由中来处理数据。因为在laravel中，路由可以接收像**get**、**post**、**put**以及**delete**这样的请求。

代码如下：
	
	Route::post('/',function(){

	//定义验证规则
	$rules = array(
		'link' => 'required|url'
	);

	//运行验证
	$validation = Validator::make(Input::all(),$rules);

	//如果验证失败，则返回主页面并提示错误信息
	if($validation->fails()) {
		return Redirect::to('/')
				->withInput()
				->withErrors($validation);
	} else {
		//如果我们输入的数据在数据库中已经存在，则在视图中输出数据
		$link = Link::where('url','=',Input::get('link'))
			->first();
		if($link) {
			return Redirect::to('/')
				->withInput()
				->with('link',$link->hash);
		//如果没有则创建数据
		} else {
			//首先创建一个新的hash值
			do {
				$newHash = Str::random(6);
			} while(Link::where('hash','=',$newHash)->count() > 0);

			//然后把数据存入到数据中对应的字段中
			Link::create(array(
				'url'	=> Input::get('link'),
				'hash'	=> $newHash
			));

			//最后把hash传递给视图
			return Redirect::to('/')
				->withInput()
				->with('link',$newHash); 
		}
	}


	});

我们这里使用了laravel自带的一个验证的类，用于验证数据以及获取错误消息。

我们首先是定义了一个**$rules**数组来指定每一个字段的规则。我们指定规则非常简单，是用户必须输入url，并且输入的url地址必须是合法的url地址，才能通过验证。

我们使用** Validator::make()**方法来运行验证。传递给 make 函数的第一个参数是待验证的数据，第二个参数是对该数据需要应用的验证规则。多个验证规则可以通过 "|" 字符进行分隔，或者作为数组的一个单独的元素。

在我们的这个程序中，由于只有一个link字段需要验证，所以我们使用了**Input::all()**方法来把input的值传递给make方法。

一旦一个 Validator 实例被创建，可以使用 fails （或者 passes）函数执行这个验证。如果验证失败，可以使用$messages = $validator->messages();来获得验证失败的相关提示信息。

	if($validation->fails()) {
		return Redirect::to('/')
				->withInput()
				->withErrors($validation);
	}

而我们程序则指定如果验证失败，则返回到主页面。当验证失败的时候，我们使用 withErrors 函数把 Validator 实例传递给 Redirect。这个函数将刷新 Session 中保存的错误消息，使得它们在下次请求中可用。并且我们还使用了**withInput()**方法，这个方法非常有用，当我们返回错误信息的时候，它会把我们上次提交的数据填充在表单中对应的字段中，方便我们快速修改，提升用户体验。

### 在视图中显示提示信息 ###

当然，最终的提示信息是要通过视图展示给用户的。我们在视图中会看下面的代码：

![](http://pic.yupoo.com/reicky_v/DIs07sFZ/3SlLx.png)

我们使用了**Session::has('errors')**来检测在session中错误信息并刷新显示它。由于我们这里只指定了一个验证信息，所以我们使用了**first('link')**方法来获取第一个字段的错误信息。

接着来看代码，我们保存数据的逻辑主要是下面几步：

- 检测用户输入的数据是否已经存在数据库中
- 如果存在，则返回数据
- 如果数据在数据库中没有记录，则创建一个新的hash值
- 把数据保存在数据库中
- 返回创建好的短链接给用户

    //如果我们输入的数据在数据库中已经存在，则在视图中输出数据
		$link = Link::where('url','=',Input::get('link'))
			->first();

这里使用**where()**来查找数据并保存在$link变量中。

	//如果存在，则把数据输出到视图中
		if($link) {
			return Redirect::to('/')
				->withInput()
				->with('link',$link->hash);

如果存在数据，我们使用**with()**方法传递相关参数给视图。with()方法接受两个参数，第一个参数是视图中使用的名字，第二个参数是想要输出的值。所以在视图中可以看到下面的代码：

![](http://pic.yupoo.com/reicky_v/DIs9TEol/UJiYD.png)

这里使用了laravel中的**Html**方法中的**link（）**来生成链接。**link()**方法需要两个参数：

- 第一个参数是链接的地址。如果我们提供一个字段，我们这里提供的是一个hash值，它会自动帮我们把字段添加到我们主url地址的后面。
- 第二个参数是链接的文字。
- 我们还可以以数组的形式来传递参数来定义链接其它的属性，比如class、id以及target等属性。

			//首先创建一个新的hash值
			do {
				$newHash = Str::random(6);
			} while(Link::where('hash','=',$newHash)->count() > 0);

			//然后把数据存入到数据中对应的字段中
			Link::create(array(
				'url'	=> Input::get('link'),
				'hash'	=> $newHash
			));

			//最后把hash传递给视图
			return Redirect::to('/')
				->withInput()
				->with('link',$newHash); 
		}

如果检测到数据是新数据，则保存到数据库中。首先是使用laravel中的Str类中的**Str::random(6)**方法来随机生成一个6位数的字符串。

我们使用了**create()**方法来插入新的数据。create方法在一行代码中保存一个新的模型。被插入的模型实例将从函数中返回。但是，在您这样做之前，您需要在模型中指定 fillable 或者 guarded 属性，因为所有 Eloquent 模型默认阻止集体赋值。

在我们的程序中，我们的url是放在name属性为link的input中，所以我们可以使用**Input::get()**方法来获取对应的值。

插入数据后，返回到主页面并且把url的hash值传递给视图。

### 取出url短链接地址并跳转 ###

最后一步是从数据库中取出hash字符串生成对应的短链接，点击的时候跳转到hash对应的url。在**routes.php**文件中添加下面的代码：

	Route::get('{hash}',function($hash) {
	//我们会根据hash的值，来查询数据库中对应的链接并保存在$link变量中
	$link = Link::where('hash','=',$hash)
		->first();
	//如果存在，则跳转到对应的链接
	if($link) {
		return Redirect::to($link->url);
	//如果没有，则返回相关的错误信息
	} else {
		return Redirect::to('/')
			->with('message','失效的链接');
	}
	})->where('hash', '[0-9a-zA-Z]{6}');

在上面的代码中，我们把hash作为一个参数传递给路由。并使用**where()**方法来查询数据库中是否有这个hash数据。如果有则跳转到hash对应的url地址网页。如果没有，则跳转到主页并提示错误信息。并且通过where的方法使用正则表达式来匹配hash是否符合我们指定的格式，这样有利于我们程序的安全。

OK，一个简单短链接生成器就完成了。源代码已经放到github上面，可以从[这个地址](https://github.com/janily/laravel-short)来下载。

在这篇文章中我们学会了使用laravel提供的数据迁移命令来创建数据库等工作，以及使用laravel提供的表单类来验证表单数据。使用Eloquent ORM来操作数据库。

下篇文章我们接着会学习到laravel更高级的功能。



