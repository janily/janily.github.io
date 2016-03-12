---
layout: post
title: laravel实战系列：使用laravel和ajax来开发一个RESTful风格的Todo应用
categories:
- Life
tags:
- laravel
- php
---

> 今天这篇文章来学习使用laravel和jQuery来开发一个Todo类型的小应用，当然最主要的是学习在laravel中使用ajax技术来进行开发。

[源代码地址](https://github.com/janily/laravel-todo/)

通过这篇文章我们会学到以下知识：

- RESTful控制器以及RESTful路由和响应类型判断

主要是包含以下几个步骤：

- 使用laravel中数据迁移来创建数据库
- 创建todo模型
- 创建视图模板
- 使用Ajax的方式来进行数据交互
- 怎么来识别Ajax响应

当然开始之前运行下面的命令来创建我们的开发环境：

	php artisan create-project laravel/laravel todo

### 使用迁移来创建数据库 ###

在laravel中迁移是一个非常好用的东东，特别是进行数据库创建等工作。

运行下面的命令：

	php artisan migrate:make create_todos_table --create=todos

运行之后，它会在**app/database/migration**目录下生成一个todos表的迁移文件，用来管理这个表的相关属性，编辑代码如下所示：

	class CreateTodosTable extends Migration {

	/**
	 * Run the migrations.
	 *
	 * @return void
	 */
	public function up()
	{
		Schema::create('todos', function(Blueprint $table)
		{
			$table->increments('id');
			$table->string('title',255);
			$table->enum('status',array('0','1'))->default('0');
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
		Schema::drop('todos');
	}

	}

我们定义5个字段：

- 一个是自增的id
- 一个title字段和用来表示任务完成和未完成的status字段，默认值是0
- timestamps()方法会为我们生成两个时间戳，一个创建的时间戳；另外一个是更新的时间戳

然后我们运行下面的命令：

	php artisan migrate

运行之后，它会帮我们在数据库中创建我们定义的表。

### 创建todo模型 ###

在**app/models**目录下创建**Todo.php**文件，编写如下代码：

	<?php
	class Todo extends Eloquent 
	{
		protected $table = 'todos';
	}

我们的模型是继承了**Eloquent**类，即laravel提供的一个**ORM**。

**protected $table = 'todos;**这里定义了**Eloquent**对象要操作的表名。如果不指定的话，那Eloquent则会默认指向模型名字所代表的的名字，在实际开发中最好是来指定相关的表名。

下面我们来创建视图文件。在**app/views**目录下创建**index.blade.php**文件，并写入代码，具体代码可以去下载源代码查看，这里就不列出来了。

在视图文件中我们主要是运用了**foreach**这个循环语句来查询数据库中的记录。使用了**if...else**语句来判断任务是否完成的状态来分别指定样式。

由于我们是使用ajax的方式来进行数据交互操作的，所以我们还需要编写一个用来给ajax调用的模板，创建一个**ajaxData.blade.php**文件，编写下面的代码：

![](http://pic.yupoo.com/reicky_v/DIJaqojm/jTxFQ.jpg)

从源代码中也可以看出，我们把网站需要用到的css、图片、js等静态文件分门别类的放在**public**文件夹中，从上一篇可以知道我们可以很方便的使用laravel提供的**Html**方法来引用这些静态资源文件。

当然要使用ajax技术，我们需要编写javascript代码。js代码主要是处理任务的增删查改即跟**add_task**和**edit_task**这两个表单里的数据打交道。在**/public/assets/js**目录中创建一个**todo.js**文件，具体需要编写的代码可以在源代码里面的**todo.js**查看。

我们这里主要是来分析下代码所代表的的含义。

### 使用Ajax来插入数据到数据库 ###

在我们这个todo程序中，我们主要是运用了**Ajax POST**的方法来把数据插入到数据库中。使用jQuery这个javascript库，我们很容易使用Ajax来进行开发。

在我们的html代码中我们有两个表单来收集用户输入的数据，所以我们可以使用post的方式通过ajax技术提交给服务端从而把数据插入到数据库中。jQuery为我们提供了**$.post()**方法来提交数据。

首先来看看增加数据的js代码：

	$('#add_task').submit(function(event) {
	
	/* 阻止默认事件*/
  	event.preventDefault();
	
	var title = $('#task_title').val();
	if(title){

	  	//提交数据

	  	$.post("/add", {title: title}).done(function(data) {
		  
		  $('#add_task').hide("slow");
		  $("#task_list").append(data);
		  

		});

	}
	else{

		alert("请添加标题");
	}
	});

上面的代码是用来提交数据到服务端的。当用户输入了title，则提交；如果没输入，则不会提交到服务端并提示用户输入title信息。如果数据发送成功，则会隐藏**#add_task**这个表单，并把新添加的数据追加到任务列表中。

接下来是处理编辑数据即修改**title**并通过ajax更新到数据库中。

	$('#edit_task').submit(function(event) {
	
	/*阻止默认事件 */
  	event.preventDefault();

  
	var task_id = $('#edit_task_id').val();
	var title = $('#edit_task_title').val();

	var current_title = $("#span_"+task_id).text();

	var new_title = current_title.replace(current_title, title);

	if(title){

	  	//提交数据

	  	$.post("/update/"+task_id, {title: title}).done(function(data) {
		  
		  $('#edit_task').hide("slow");
		  $("#span_"+task_id).text(new_title);
		  

		});

	}
	else{

		alert("请添加标题");
	}
	});

laravel提供了RESTful这种类型的控制器，这意味着我们可以根据不同网络请求类型如**POST**、**GET**、**PUT**和**DELETE**来使用不同的方法来处理数据。

在定义路由前，先来编写好控制器的代码。在**app/controllers**目录下新建一个**TodoController.php**文件，编写如下代码：

	<?php 
	class TodoController extends BaseController
	{
		public $restful = true;
		public function postAdd() {
			$todo = new Todo();
			$todo->title = Input::get('title');
			$todo->save();
			$last_todo = $todo->id;
			$todos = Todo::whereId($last_todo)->get();
			return View::make('ajaxData')
				->with('todos',$todos);
		}
		public function postUpdate($id) {
			$task = Todo::find($id);
			$task->title = Input::get('title');
			$task->save();
			return 'ok';
		}
	}

在控制器中使用诸如**postFuntion**、**getFuntion**语法来定义RESTful风格的方法。

我们有两个表单需要发送数据给服务端，所以我们需要定义两个POST方法来发送数据和一个GET方法老获取数据并通过模板渲染出来展示给用户。

先来分析下**postUpdate()**方法：

	public function postUpdate($id) {
		$task = Todo::find($id);
		$task->title = Input::get('title');
		$task->save();
		return 'ok';
	}

- 这个方法需要id作为参数才能更新数据。我们通过该**/update/record_id**这样的路径把数据提交给服务端来更新数据。
- **$task = Todo::find($id);**它会根据我们传递的id来查找对应的数据。
- **$task->title = Input::get('title');**则表示获取数据并更新到数据库中。
- **$task->save();**把更新的数据保存到数据库中。

下面来解释**postUpdate()**这个方法:

	public function postAdd() {
		$todo = new Todo();
		$todo->title = Input::get('title');
		$todo->save();
		$last_todo = $todo->id;
		$todos = Todo::whereId($last_todo)->get();
		return View::make('ajaxData')
			->with('todos',$todos);
	}

- **$last_todo = $todo->id;**表示获取出入数据的id。等同于**mysql_insert_id()**方法。
- **$todos = Todo::whereId($last_todo)->get();**则表示获取指定id的信息。
- **return View::make('ajaxData')->with('todos',$todos);**这个对于理解laravel视图模板引擎的工作原理非常重要。**View::make('ajaxData')**表示我们引用的视图模板即我们创建的ajax.blade.php文件。->with('todos',$todos);把我们取到的数据传递给模板给渲染出来。

### 从数据库中查询数据并渲染出来 ###

当然在主页我们还需要把存在的数据都取出来并用视图模板渲染出来展示给用户:

	public function getIndex() {
		$todos = Todo:all();

		return View::make('index')
			-with('todos',$todos);
	}

- **$todos = Todo:all();**表示获取所有的数据并把值赋给$todos变量。
- **View::make('index')**表示应用index这个视图模板。
- **-with('todos',$todos);**表示把$todos这个变量传递给视图模板并使用**foreach**循环语句输出数据。

最后是来定义路由。打开**app**目录下的**routes.php**文件，对于定义RESTful控制器的路由，laravel中可以这样来定义：

	Route::controller('/','TodoController');

它会自动根据不同的请求类型来调用不同的方法来处理数据。当然你也可以像下面这样来定义你的路由：

	Route::method('path/{variable}','TheController@functionName');

### 怎样来识别是否是Ajax请求 ###

现在我们的程序只要是**POST**和**GET**请求都会进行处理。但是我们仅仅只需要通过Ajax请求来执行**add**和**update**这两个方法。laravel中的**Request**类提供很多的方法来处理HTTP请求。其中一个方式是**ajax()**，我们可以使用它在控制器或者是路由中检测是否是**ajax()**请求。

那我们就在路由中来定义一个过滤器来检测用户发送的请求是否是**ajax**。在laravel中过滤器是在**app**目录中的**filters.php**文件中定义的。我们可以在这个文件来定义我们自己的过滤器，在这里我们不打算使用这个方法，不过在后面的文章中会涉及到它。我们可以在路由中来定义我们的过滤器，在路由文件中添加下面的代码：

	Route::filter('ajax_check',function(){
	if(Request::ajax(){
		return true;
	});
	});

在路由中定义过滤器非常简单。再比如：

	Route::get('/add',array('before' => 'ajax_check',fucntion()
	{
		return 'the Request js ajax!';
	}));

我们使用**before**这个变量来指向我们定义的过滤器。这里表示要通过我们定义的过滤器才能继续执行其它的方法。

当然我们也可以在控制器中来检测请求类型。在todo程序中我们就使用这个方法，非常方便。我们需要来改变控制器中的**update**和**add**方法:

	public function postAdd() {
		if(Request::ajax()) {
			$todo = new Todo();
			$todo->title = Input::get('title');
			$todo->save();
			$last_todo = $todo->id;
			$todos = Todo::whereId($last_todo)->get();
			return View::make('ajaxData)
				-with('todos',$todos);
		}
	}

	public function postUpdate($id) {} {

		if(Request::ajax()) {

			$task = Todo::find($id);
			$task->title = Input::get('title');
			$task->save();
			return 'ok';
		}

	}

在我们编写的程序中，主要是数据的增删查改以及标示任务是否完成这几个状态。所以我们还需要编写**getDone()**和**getDelete()**方法，我们知道RESTful控制器接受**GET**类型的请求。我们在控制器中添加以下代码：

	public function getDelete($id) {
		if(Request::ajax()) {
			$todo = Todo::whereId($id)->first();
			$todo->delete();
			return 'ok';
		}
	}

	public function getDone($id) {
		if(Request::ajax()) {
			$task = Todo::find($id);
			$task->status = 1;
			$task->save();
			return 'ok';
		}
	}

相应的我们也需要更新我们的js文件，如下所示：

	function task_done(id){

		$.get("/done/"+id, function(data) {

			if(data=="OK"){

				$("#"+id).addClass("done");
			}
  
	});
	}
	function delete_task(id){

		$.get("/delete/"+id, function(data) {

			if(data=="OK"){
				var target = $("#"+id);

				target.hide('slow', function(){ target.remove(); });

			}
  
	});
	}


	function show_form(form_id){
		
		$("form").hide();

		$('#'+form_id).show("slow");

	}
	function edit_task(id,title){

		$("#edit_task_id").val(id);

		$("#edit_task_title").val(title);

		show_form('edit_task');


	}
	$('#add_task').submit(function(event) {
	
	/* 阻止默认事件 */
  	event.preventDefault();
	
	var title = $('#task_title').val();
	if(title){

	  	//发送数据

	  	$.post("/add", {title: title}).done(function(data) {
		  
		  $('#add_task').hide("slow");
		  $("#task_list").append(data);
		  

		});

	}
	else{

		alert("Please give a title to task");
	}
	});

	$('#edit_task').submit(function(event) {
	
	/* 阻止默认事件 */
  	event.preventDefault();

  
	var task_id = $('#edit_task_id').val();
	var title = $('#edit_task_title').val();

	var current_title = $("#span_"+task_id).text();

	var new_title = current_title.replace(current_title, title);

	if(title){

	  	//发送数据

	  	$.post("/update/"+task_id, {title: title}).done(function(data) {
		  
		  $('#edit_task').hide("slow");
		  $("#span_"+task_id).text(new_title);
		  

		});

	}
	else{

		alert("Please give a title to task");
	}
	});

OK，一个简单的todo应用程序就完成了。用浏览器打开就可以添加或者是删除数据了，如下图所示：

![](http://pic.yupoo.com/reicky_v/DIKjiOCR/yQo1q.jpg)


















	



