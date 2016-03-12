---
layout: post
title: laravel实战学习-用laravel4创建一个ToDo应用(2)
categories:
- Life
tags:
- laravel
- php
---

上一篇文章中，我们完成创建好了数据库以及测试数据和路由。本篇文章我们来继续完善程序主要是控制器、视图以及路由逻辑这三部分功能的编写。

### 控制器和视图 ###

现在如果你打开**projects**这个视图文件夹，会看到一些已经建好的空的视图模板文件。接下来我们就要来网视图模板文件里添加内容。

开始之前，我们先来把公共的布局模板文件建好，在**views**文件夹中新建一个**layouts**文件夹然后在里面新建一个**main.blade.php**的模板文件，代码如下：

    <!DOCTYPE html>
	<!--[if lt IE 7]>      <html class="no-js lt-ie9 lt-ie8 lt-ie7"> <![endif]-->
	<!--[if IE 7]>         <html class="no-js lt-ie9 lt-ie8"> <![endif]-->
	<!--[if IE 8]>         <html class="no-js lt-ie9"> <![endif]-->
	<!--[if gt IE 8]><!--> <html class="no-js"> <!--<![endif]-->
	<head>
	<meta charset="utf-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
	<title>My To-Do App</title>
	<meta name="viewport" content="width=device-width">
	<style>
		#wrapper {width:960px;max-width:100%;margin:auto}
		.inline {display:inline}
		.error {color:red}
	</style>
	</head>
	<body>
		<div id='wrapper'>
			<header>
				<h1>My To-Do App</h1>
			</header>
			<div id="content">
				@if (Session::has('message'))
					<div class="flash alert">
						<p>{{ Session::get('message') }}</p>
					</div>
				@endif
	 
				@yield('main')
			</div>
		</div>
	</body>
	</html>

这个文件是每一个视图文件会用到的，所以我们可以在**BadeController**这个控制器里编写下面的代码这样其它的视图文件就可以很方便的引用它了，代码如下：

    class BaseController extends Controller {
	protected $layout = 'layouts.main';

在其它控制器里面我们可以这样来引用这个模板：

    // return View::make('projects.index');
	$this->layout->content = View::make('projects.index');

可以去官方的文档了解更加详细的使用方法[ Controller Layouts ](http://laravel.com/docs/templates#controller-layouts)。

### 绑定路由模型 ###

laravel4默认的资源控制器根据不同类型的请求为我们提供了show(), edit(), update() 和 destroy()这些方法。这些方法虽然不错，但是它需要我们额外编写一些模型，进行检测等额外的工作。幸好laravel还提供了[route model binding](http://laravel.com/docs/routing#route-model-binding)即路由与模型绑定的功能，模型绑定，为在路由中注入模型实例提供了便捷的途径。例如，你可以向路由中注入匹配用户ID的整个模型实例，而不是仅仅注入用户ID。

打开路由文件，添加前面的两行代码：

    // 绑定模型和路由
	Route::model('tasks', 'Task');
	Route::model('projects', 'Project');
	 
	// 定义路由
	Route::bind('tasks', function($value, $route) {
		return Task::whereSlug($value)-&gt;first();
	});
	Route::bind('projects', function($value, $route) {
		return Project::whereSlug($value)-&gt;first();
	});
	 
	Route::resource('projects', 'ProjectsController');
	Route::resource('projects.tasks', 'TasksController');

然后在**TasksController**和**ProjectsController**这两个控制器把用到id的方法里的**$id**替换为**Task $task**和**Project $project**，如下所示：

    // public function edit($id)
	public function edit(Project $project)

	public function edit(Task $task)

那么就可以在视图中来使用我们传进去的模型了，如：

    public function edit(Project $project)
	{
	    $this->layout->content = View::make('projects.show', compact('project'));
	}

由于我们使用了路由嵌套，所有的task都会包含一个project，我们需要把**TaskController**这个控制器的代码完善一下。所以在task的控制器里的方法都需要把**$project**作为参数传递给相关方法。代码如下：

    class TasksController extends BaseController {
 
	/**
	 * Display a listing of the resource.
	 *
	 * @param  Project  $project
	 * @return Response
	 */
	public function index(Project $project)
	{
		$this->layout->content = View::make('tasks.index', compact('project'));
	}
 
	/**
	 * Show the form for creating a new resource.
	 *
	 * @param  Project  $project
	 * @return Response
	 */
	public function create(Project $project)
	{
		$this->layout->content = View::make('tasks.create', compact('project'));
	}
 
	/**
	 * Store a newly created resource in storage.
	 *
	 * @param  Project  $project
	 * @return Response
	 */
	public function store(Project $project)
	{
		//
	}
 
	/**
	 * Display the specified resource.
	 *
	 * @param  Project  $project
	 * @param  Task     $task
	 * @return Response
	 */
	public function show(Project $project, Task $task)
	{
		$this->layout->content = View::make('tasks.show', compact('project', 'task'));
	}
 
	/**
	 * Show the form for editing the specified resource.
	 *
	 * @param  Project  $project
	 * @param  Task     $task
	 * @return Response
	 */
	public function edit(Project $project, Task $task)
	{
		$this->layout->content = View::make('tasks.edit', compact('project', 'task'));
	}
 
	/**
	 * Update the specified resource in storage.
	 *
	 * @param  Project  $project
	 * @param  Task     $task
	 * @return Response
	 */
	public function update(Project $project, Task $task)
	{
		//
	}
 
	/**
	 * Remove the specified resource from storage.
	 *
	 * @param  Project  $project
	 * @param  Task     $task
	 * @return Response
	 */
	public function destroy(Project $project, Task $task)
	{
		//
	}
 
	}

### 显示数据 ###

首先来编写显示项目列表的页面。打开**/app/views/projects/index.blade.php**，编写一下代码：

    @section('main')
	<h2>Projects</h2>
	@if ( !$projects->count() )
		You have no projects
	@else
		<ul>
			@foreach( $projects as $project )
				<li><a href="{{ route('projects.show', $project->slug) }}">{{ $project->name }}</a></li>
			@endforeach
		</ul>
	@endif
	@stop

在laravel中是使用默认提供的blade的模板文件来显示数据的并且是使用了[route()](http://laravel.com/docs/helpers#urls)方法来定义url的。

这个时候我们来访问[http://localhost:8000/projects/index.php](http://localhost:8000/projects/index.php)地址的时候会提示我们变量的错误。所以我们还需要把**$projects**这个变量传递给视图。打开**/app/controllers/ProjectsController.php**文件，更新**index()**方法：

    public function index()
	{
		$projects = Project::all();
	 
		$this->layout->content = View::make('projects.index', compact('projects'));
	}

### 定义数据模型之间的关系 ###

在项目详细的页面我们需要显示这个项目下所有的任务列表。需要在**$Project**模型中定义[一个一对多的数据关系](http://laravel.com/docs/eloquent#one-to-many)即一个项目包含多个任务。

打开**/app/models/Project.php**文件编写以下代码：

    public function tasks()
	{
		return $this->hasMany('Task');
	}

相反要在**Task**模型里面定义多对一的关系：

    public function project()
	{
		return $this->belongsTo('Project');
	}

我们使用artisan中的tinker命令来检测下：

    $ php artisan tinker
	>echo Project::whereSlug('project-1')->first()->tasks->count();
	3
	>echo Project::whereSlug('project-2')->first()->tasks->count();
	2
	>echo Task::first()->project->name;
	Project 1

OK！现在来更新下视图文件，打开**/app/views/projects/show.blade.php**：

    @section('main')
	<h2>{{ $project->name }}</h2>
	@if ( !$project->tasks->count() )
		Your project has no tasks.
	@else
		<ul>
			@foreach( $project->tasks as $task )
				<li><a href="{{ route('projects.tasks.show', [$project->slug, $task->slug]) }}">{{ $task->name }}</a></li>
			@endforeach
		</ul>
	@endif
	@stop

现在点击项目名称的时候会跳转到这个项目的任务列表的页面。

最后来编写任务显示的模板页面(**/app/views/tasks/show.blade.php**)。这个就非常简单了：

    @section('main')
	<h2>{{ $project->name }} - {{ $task->name }}</h2>
	{{ $task->description }}
	@stop

注意事项：当在定义模型数据关系的时候，非常容易造成大量的**SQL**查询，这样可能在性能上对程序有影响。可以看看下面这个视频来了解怎么来预防和避免这样的问题。

[视频地址](http://www.youtube.com/embed/swhWRMkpVsg)

### 总结 ###

来看看我们今天学习了啥：

- 路由模型绑定
- 模型数据关系
- 控制器
- 用blade模板引擎编写视图文件

现在我们完成了数据的显示这方面的工作。在下一篇文章我们将完成数据的编辑、创建和删除的功能。


    