---
layout: post
title: 使用laravel中的资源控制器来实现数据的增删查改
categories:
- Life
tags:
- laravel
- php
- 翻译
---

> 几乎每一个web应用程序都是数据的增删查改。在laravel中为我们提供了资源控制器使我们更容易的实现数据的增删查改。今天这篇文章就来来学习怎么使用资源控制器。原文地址[Simple Laravel CRUD with Resource Controllers](http://scotch.io/tutorials/simple-laravel-crud-with-resource-controllers)。

[源代码地址](https://github.com/scotch-io/simple-laravel-crud)。

在这篇文章中，我们来编写一个后台来实现数据的增删查改。这个程序的名字叫**nerds**，这中间也会用到我们上篇文章讲到的Eloquent ORM这个知识。

主要包括以下步骤：

- 数据库和模型设置
- 编写资源控制器和路由
- 编写一些视图模板
- 在资源控制器中实现相关数据增删查改的方法

开始吧！

### Nerd数据库设置 ###

在终端中运行下面的命令来创建相关的数据库。

	php artisan migrate:make create_nerds_table --table=nerds --create

然后在**app/database/migrations**目录下打开migration文件。然后编辑文件添加一些字段：

	// app/database/migrations/####_##_##_######_create_nerds_table.php

	<?php
	
	use Illuminate\Database\Schema\Blueprint;
	use Illuminate\Database\Migrations\Migration;
	
	class CreateNerdsTable extends Migration {

	/**
	 * Run the migrations.
	 *
	 * @return void
	 */
	public function up()
	{
		Schema::create('nerds', function(Blueprint $table)
		{
			$table->increments('id');

			$table->string('name', 255);
			$table->string('email', 255);
			$table->integer('nerd_level');

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
		Schema::drop('nerds');
	}

	} 

然后在终端中运行下面的命令在数据库中来生成数据，运行之前确保你**app/config/database.php**设置好相关数据连接信息。

    php artisan migrate

运行之后数据库就建好了。

### 使用Eloquent Model ###

创建好数据库后，我们可以通过Eloquent model来跟数据库交互。详细的可以去阅读上篇文章。

在**app/models**目录下，创建Nerd.php模型文件。

    <?php

	class Nerd extends Eloquent
	{

	}

就这么简单。默认它会连接到**nerds**这张表，稍后会讲到这部分。

### 创建控制器 ###

我们可以使用artisan命令来生成资源控制器文件。

在终端中运行下面的命令：

	php artisan controller:make NerdController

运行之后会生成控制器文件以及相关的方法。

    <?php

	class NerdController extends \BaseController {

	/**
	 * Display a listing of the resource.
	 *
	 * @return Response
	 */
	public function index()
	{
		//
	}

	/**
	 * Show the form for creating a new resource.
	 *
	 * @return Response
	 */
	public function create()
	{
		//
	}

	/**
	 * Store a newly created resource in storage.
	 *
	 * @return Response
	 */
	public function store()
	{
		//
	}

	/**
	 * Display the specified resource.
	 *
	 * @param  int  $id
	 * @return Response
	 */
	public function show($id)
	{
		//
	}

	/**
	 * Show the form for editing the specified resource.
	 *
	 * @param  int  $id
	 * @return Response
	 */
	public function edit($id)
	{
		//
	}

	/**
	 * Update the specified resource in storage.
	 *
	 * @param  int  $id
	 * @return Response
	 */
	public function update($id)
	{
		//
	}

	/**
	 * Remove the specified resource from storage.
	 *
	 * @param  int  $id
	 * @return Response
	 */
	public function destroy($id)
	{
		//
	}

	}

### 设置路由器 ###

创建好控制器后，接下来是创建相关的路由方法。打开路由文件添加下面的代码：

	<?php 
		Route::resource('nerds','NerdController');

它会自动根据你不同的操作执行不同的方法，如下图所示。

![](http://pic.yupoo.com/reicky_v/DHwDOKYP/hRasd.jpg)

> 你也可以在终端中运行php artisan routes命令来看它提供的方法。

### 视图 ###

在路由中主要用来获取数据的GET方法有四个，所以我们需要四个视图文件来显示数据。在**app/views**目录中，我们来创建视图文件，结构如下所示。

![](http://pic.yupoo.com/reicky_v/DHwGIPI6/q8uYb.jpg)

至此，我们程序所需要的控制器，数据库，控制器以及视图文件都准备好了。现在要做的是把它们用代码联系起来，整合成一个完整的应用程序。

### 显示所有的数据 ###

默认情况下，index视图文件显示所有的数据，对应的是控制器中的**index()**方法。

在这个方法中，我们要获取所有的数据并且显示在视图中。

    // app/controllers/NerdController.php

	<?php
	
	...

	/**
	 * Display a listing of the resource.
	 *
	 * @return Response
	 */
	public function index()
	{
		// get all the nerds
		$nerds = Nerd::all();

		// load the view and pass the nerds
		return View::make('nerds.index')
			->with('nerds', $nerds);
	}

	...

下面来编辑视图文件**app/views/nerds/index.blade.php**

我们的视图文件是以[Twitter Bootstrap](http://getbootstrap.com/)这个前端库来构建的，方便好用。

	<!-- app/views/nerds/index.blade.php -->

	<!DOCTYPE html>
	<html>
	<head>
		<title>Look! I'm CRUDding</title>
		<link rel="stylesheet" href="//netdna.bootstrapcdn.com/bootstrap/3.0.0/css/bootstrap.min.css">
	</head>
	<body>
	<div class="container">
	
	<nav class="navbar navbar-inverse">
		<div class="navbar-header">
			<a class="navbar-brand" href="{{ URL::to('nerds') }}">Nerd Alert</a>
		</div>
		<ul class="nav navbar-nav">
			<li><a href="{{ URL::to('nerds') }}">View All Nerds</a></li>
			<li><a href="{{ URL::to('nerds/create') }}">Create a Nerd</a>
		</ul>
	</nav>
	
	<h1>All the Nerds</h1>
	
	<!-- will be used to show any messages -->
	@if (Session::has('message'))
		<div class="alert alert-info">{{ Session::get('message') }}</div>
	@endif
	
	<table class="table table-striped table-bordered">
	<thead>
		<tr>
			<td>ID</td>
			<td>Name</td>
			<td>Email</td>
			<td>Nerd Level</td>
			<td>Actions</td>
		</tr>
	</thead>
	<tbody>
	@foreach($nerds as $key => $value)
		<tr>
			<td>{{ $value->id }}</td>
			<td>{{ $value->name }}</td>
			<td>{{ $value->email }}</td>
			<td>{{ $value->nerd_level }}</td>

			<!-- we will also add show, edit, and delete buttons -->
			<td>

				<!-- delete the nerd (uses the destroy method DESTROY /nerds/{id} -->
				<!-- we will add this later since its a little more complicated than the other two buttons -->

				<!-- show the nerd (uses the show method found at GET /nerds/{id} -->
				<a class="btn btn-small btn-success" href="{{ URL::to('nerds/' . $value->id) }}">Show this Nerd</a>

				<!-- edit this nerd (uses the edit method found at GET /nerds/{id}/edit -->
				<a class="btn btn-small btn-info" href="{{ URL::to('nerds/' . $value->id . '/edit') }}">Edit this Nerd</a>

			</td>
		</tr>
	@endforeach
	</tbody>
	</table>
	
	</div>
	</body>
	</html>
	
它会显示所有的nerds的数据，目前还是空的，如下图所示。因为我们还没添加数据。

![](http://scotch.io/wp-content/uploads/2013/10/index-blade.png)

要添加数据，就需要使用create()方法来保存数据。

	// app/controllers/NerdController.php

	<?php
	
	...
	
		/**
		 * Show the form for creating a new resource.
		 *
		 * @return Response
		 */
		public function create()
		{
			// load the create form (app/views/nerds/create.blade.php)
			return View::make('nerds.create');
		}
	
	...

编辑视图文件** app/views/nerds/create.blade.php**

	<!-- app/views/nerds/create.blade.php -->

	<!DOCTYPE html>
	<html>
	<head>
		<title>Look! I'm CRUDding</title>
		<link rel="stylesheet" href="//netdna.bootstrapcdn.com/bootstrap/3.0.0/css/bootstrap.min.css">
	</head>
	<body>
	<div class="container">
	
	<nav class="navbar navbar-inverse">
		<div class="navbar-header">
			<a class="navbar-brand" href="{{ URL::to('nerds') }}">Nerd Alert</a>
		</div>
		<ul class="nav navbar-nav">
			<li><a href="{{ URL::to('nerds') }}">View All Nerds</a></li>
			<li><a href="{{ URL::to('nerds/create') }}">Create a Nerd</a>
		</ul>
	</nav>
	
	<h1>Create a Nerd</h1>
	
	<!-- if there are creation errors, they will show here -->
	{{ HTML::ul($errors->all()) }}
	
	{{ Form::open(array('url' => 'nerds')) }}
	
		<div class="form-group">
			{{ Form::label('name', 'Name') }}
			{{ Form::text('name', Input::old('name'), array('class' => 'form-control')) }}
		</div>
	
		<div class="form-group">
			{{ Form::label('email', 'Email') }}
			{{ Form::email('email', Input::old('email'), array('class' => 'form-control')) }}
		</div>
	
		<div class="form-group">
			{{ Form::label('nerd_level', 'Nerd Level') }}
			{{ Form::select('nerd_level', array('0' => 'Select a Level', '1' => 'Sees Sunlight', '2' => 'Foosball Fanatic', '3' => 'Basement Dweller'), Input::old('nerd_level'), array('class' => 'form-control')) }}
		</div>
	
		{{ Form::submit('Create the Nerd!', array('class' => 'btn btn-primary')) }}
	
	{{ Form::close() }}
	
	</div>
	</body>
	</html>

上面的代码中，我们首先会检测在保存数据的时候是否有错误信息显示，有则显示。

> 当我们使用{{Form::open()}}的时候，Laravel框架会自动在用户 session 中存放一个随机令牌（token），并且将这个令牌作为一个隐藏字段包含在表单信息中，抵御CSRF（跨站请求伪造）式攻击。

现在我们有了用于收集数据的表单，但是当我们提交数据的时候我们还需要做一些事情。在表单中我们提交的数据是以post的方式提交到example.com/nerds这个地址来处理的。资源控制器会自动使用**store()**方法来处理它。

### 保存数据 ###

我们不需另外指定URL地址来保存数据。当form使用POST的方式提交数据的时候，资源控制器会使用**store()**方法来保存数据。

当然在提交数据的时候，我们需要验证数据的合法性，还要根据数据是否合法返回对应的消息。如果数据符合我们的要求，就会保存它。

    // app/controllers/NerdController.php

	<?php
	
	...

	/**
	 * Store a newly created resource in storage.
	 *
	 * @return Response
	 */
	public function store()
	{
		// validate
		// read more on validation at http://laravel.com/docs/validation
		$rules = array(
			'name'       => 'required',
			'email'      => 'required|email',
			'nerd_level' => 'required|numeric'
		);
		$validator = Validator::make(Input::all(), $rules);

		// process the login
		if ($validator->fails()) {
			return Redirect::to('nerds/create')
				->withErrors($validator)
				->withInput(Input::except('password'));
		} else {
			// store
			$nerd = new Nerd;
			$nerd->name       = Input::get('name');
			$nerd->email      = Input::get('email');
			$nerd->nerd_level = Input::get('nerd_level');
			$nerd->save();

			// redirect
			Session::flash('message', 'Successfully created nerd!');
			return Redirect::to('nerds');
		}
	}

	...

如果有错误信息，会重定向到创建页面并显示错误信息。这样用户就知道具体错误从而修改它。

现在我们点击页面上菜单里的创建栏目，来创建一条数据。然后会自动保存并重定向到首页，如下图所示：

![](http://scotch.io/wp-content/uploads/2013/10/created.png)

### 显示数据show() ###

显示单个数据是调用show()方法，如下所示：

	// app/controllers/NerdController.php

	<?php
	
	...
	
		/**
		 * Display the specified resource.
		 *
		 * @param  int  $id
		 * @return Response
		 */
		public function show($id)
		{
			// get the nerd
			$nerd = Nerd::find($id);
	
			// show the view and pass the nerd to it
			return View::make('nerds.show')
				->with('nerd', $nerd);
		}
	
	...

编辑视图文件**app/views/nerds/show.blade.php**

	<!DOCTYPE html>
	<html>
	<head>
		<title>Look! I'm CRUDding</title>
		<link rel="stylesheet" href="//netdna.bootstrapcdn.com/bootstrap/3.0.0/css/bootstrap.min.css">
	</head>
	<body>
	<div class="container">
	
	<nav class="navbar navbar-inverse">
		<div class="navbar-header">
			<a class="navbar-brand" href="{{ URL::to('nerds') }}">Nerd Alert</a>
		</div>
		<ul class="nav navbar-nav">
			<li><a href="{{ URL::to('nerds') }}">View All Nerds</a></li>
			<li><a href="{{ URL::to('nerds/create') }}">Create a Nerd</a>
		</ul>
	</nav>
	
	<h1>Showing {{ $nerd->name }}</h1>
	
		<div class="jumbotron text-center">
			<h2>{{ $nerd->name }}</h2>
			<p>
				<strong>Email:</strong> {{ $nerd->email }}<br>
				<strong>Level:</strong> {{ $nerd->nerd_level }}
			</p>
		</div>
	
	</div>
	</body>
	</html>

当点击显示按钮的时候，就会跳转到具体单个的显示页面，如下图所示:

![](http://scotch.io/wp-content/uploads/2013/10/show.png)

### 编辑数据edit() ###

编辑数据，我们需要把我们想要编辑的数据从数据库中取出来并显示在表单中。为了更容易的来编辑数据我们使用[表单与模型绑定](http://www.golaravel.com/docs/4.1/html/#form-model-binding)将模型中的内容填充到表单中去。这可以帮助你快速建立与模型绑定的表单，而且当服务器端验证出表单错误时可以轻松回填数据！

	<?php

	...
	
		/**
		 * Show the form for editing the specified resource.
		 *
		 * @param  int  $id
		 * @return Response
		 */
		public function edit($id)
		{
			// get the nerd
			$nerd = Nerd::find($id);
	
			// show the edit form and pass the nerd
			return View::make('nerds.edit')
				->with('nerd', $nerd);
		}
	
	...

视图文件**app/views/nerds/edit.blade.php**

	<!DOCTYPE html>
	<html>
	<head>
		<title>Look! I'm CRUDding</title>
		<link rel="stylesheet" href="//netdna.bootstrapcdn.com/bootstrap/3.0.0/css/bootstrap.min.css">
	</head>
	<body>
	<div class="container">
	
	<nav class="navbar navbar-inverse">
		<div class="navbar-header">
			<a class="navbar-brand" href="{{ URL::to('nerds') }}">Nerd Alert</a>
		</div>
		<ul class="nav navbar-nav">
			<li><a href="{{ URL::to('nerds') }}">View All Nerds</a></li>
			<li><a href="{{ URL::to('nerds/create') }}">Create a Nerd</a>
		</ul>
	</nav>
	
	<h1>Edit {{ $nerd->name }}</h1>
	
	<!-- if there are creation errors, they will show here -->
	{{ HTML::ul($errors->all()) }}
	
	{{ Form::model($nerd, array('route' => array('nerds.update', $nerd->id), 'method' => 'PUT')) }}
	
		<div class="form-group">
			{{ Form::label('name', 'Name') }}
			{{ Form::text('name', null, array('class' => 'form-control')) }}
		</div>
	
		<div class="form-group">
			{{ Form::label('email', 'Email') }}
			{{ Form::email('email', null, array('class' => 'form-control')) }}
		</div>
	
		<div class="form-group">
			{{ Form::label('nerd_level', 'Nerd Level') }}
			{{ Form::select('nerd_level', array('0' => 'Select a Level', '1' => 'Sees Sunlight', '2' => 'Foosball Fanatic', '3' => 'Basement Dweller'), null, array('class' => 'form-control')) }}
		</div>
	
		{{ Form::submit('Edit the Nerd!', array('class' => 'btn btn-primary')) }}
	
	{{ Form::close() }}
	
	</div>
	</body>
	</html>

我们通过**PUT**的方法来提交数据，laravel会自动来处理它。

### 更新数据update() ###

编辑完数据后，我们需要更新对应的数据这里要用到**update()**方法，其实它跟**store()**方法差不多。也是验证数据的合法性然后更新数据。

	// app/controllers/NerdController.php

	<?php
	
	...

	/**
	 * Update the specified resource in storage.
	 *
	 * @param  int  $id
	 * @return Response
	 */
	public function update($id)
	{
		// validate
		// read more on validation at http://laravel.com/docs/validation
		$rules = array(
			'name'       => 'required',
			'email'      => 'required|email',
			'nerd_level' => 'required|numeric'
		);
		$validator = Validator::make(Input::all(), $rules);

		// process the login
		if ($validator->fails()) {
			return Redirect::to('nerds/' . $id . '/edit')
				->withErrors($validator)
				->withInput(Input::except('password'));
		} else {
			// store
			$nerd = Nerd::find($id);
			$nerd->name       = Input::get('name');
			$nerd->email      = Input::get('email');
			$nerd->nerd_level = Input::get('nerd_level');
			$nerd->save();

			// redirect
			Session::flash('message', 'Successfully updated nerd!');
			return Redirect::to('nerds');
		}
	}

	...

### 删除数据destroy() ###

最后一个功能是用户删除数据，先在**app/views/nerds/index.blade.php**视图文件中创建一个删除按钮，如下所示：

	<!-- app/views/nerds/index.blade.php -->

	...

	@foreach($nerds as $key => $value)
		<tr>
			<td>{{ $value->id }}</td>
			<td>{{ $value->name }}</td>
			<td>{{ $value->email }}</td>
			<td>{{ $value->nerd_level }}</td>

			<!-- we will also add show, edit, and delete buttons -->
			<td>

				<!-- delete the nerd (uses the destroy method DESTROY /nerds/{id} -->
				<!-- we will add this later since its a little more complicated than the other two buttons -->
				{{ Form::open(array('url' => 'nerds/' . $value->id, 'class' => 'pull-right')) }}
					{{ Form::hidden('_method', 'DELETE') }}
					{{ Form::submit('Delete this Nerd', array('class' => 'btn btn-warning')) }}
				{{ Form::close() }}

				<!-- show the nerd (uses the show method found at GET /nerds/{id} -->
				<a class="btn btn-small btn-success" href="{{ URL::to('nerds/' . $value->id) }}">Show this Nerd</a>

				<!-- edit this nerd (uses the edit method found at GET /nerds/{id}/edit -->
				<a class="btn btn-small btn-info" href="{{ URL::to('nerds/' . $value->id . '/edit') }}">Edit this Nerd</a>

			</td>
		</tr>
	@endforeach

	... 

现在点击删除按钮，laravel会自动通过nerds.destroy路由在控制器中来处理。

	// app/controllers/NerdController.php
	
	<?php
	
	...
	
		/**
		 * Remove the specified resource from storage.
		 *
		 * @param  int  $id
		 * @return Response
		 */
		public function destroy($id)
		{
			// delete
			$nerd = Nerd::find($id);
			$nerd->delete();
	
			// redirect
			Session::flash('message', 'Successfully deleted the nerd!');
			return Redirect::to('nerds');
		}
	
	...

### 总结 ###

OK，一个应用程序的增删查改就完成了。在laravel中使用资源控制器非常容易处理这类型的任务。仅仅只需要一个控制器，一个路由就可以完成数据的增删查改。详细的代码可以下载下来仔细看看，[源代码下载](https://github.com/scotch-io/simple-laravel-crud/archive/master.zip)。






