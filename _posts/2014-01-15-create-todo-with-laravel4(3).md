---
layout: post
title: laravel实战学习-用laravel4创建一个ToDo应用(3)
categories:
- Life
tags:
- laravel
- php
---

上一篇我们完成了程序的控制器、视图以及路由逻辑这三部分功能的编写。这篇文章我们来学习用laravel中提供的表单方法来编写创建、编辑和删除的功能。开始之前你可以去laravel的网站查看一些表单组件的用法。

### 创建菜单 ###

我们分别需要为项目和任务编写创建、编辑、删除这些方法。

首先分别在**/app/views/projects/index.blade.php**和**/app/views/projects/show.blade.php**这两个视图文件添加创建项目和创建任务的链接，代码如下：

    <!-- /app/views/projects/index.blade.php -->
	@section('main')
	<h2>Projects</h2>
	@if ( !$projects->count() )
		You have no projects
	@else
		<ul>
			@foreach( $projects as $project )
				<li>
					<a href="{{ route('projects.show', $project->slug) }}">{{ $project->name }}</a>
					(
						{{ Form::open(array('class' => 'inline', 'method' => 'DELETE', 'route' => array('projects.destroy', $project->slug))) }}
							{{ link_to_route('projects.edit', 'Edit', array($project->slug), array('class' => 'btn btn-info')) }},
 
							{{ Form::submit('Delete', array('class' => 'btn btn-danger')) }}
						{{ Form::close() }}
					)
				</li>
			@endforeach
		</ul>
	@endif
 
	<p>{{ link_to_route('projects.create', 'Create Project') }}</p>
	@stop
 
	<!-- /app/views/projects/show.blade.php -->
	@section('main')
	<h2>{{ $project->name }}</h2>
	@if ( !$project->tasks->count() )
		Your project has no tasks.
	@else
		<ul>
			@foreach( $project->tasks as $task )
				<li>
					<a href="{{ route('projects.tasks.show', [$project->slug, $task->slug]) }}">{{ $task->name }}</a>
					(
						{{ Form::open(array('class' => 'inline', 'method' => 'DELETE', 'route' => array('projects.tasks.destroy', $project->slug, $task->slug))) }}
							{{ link_to_route('projects.tasks.edit', 'Edit', array($project->slug, $task->slug), array('class' => 'btn btn-info')) }},
 
							{{ Form::submit('Delete', array('class' => 'btn btn-danger')) }}
						{{ Form::close() }}
					)
				</li>
			@endforeach
		</ul>
	@endif
 
	<p>
		{{ link_to_route('projects.index', 'Back to Projects') }} |
		{{ link_to_route('projects.tasks.create', 'Create Task', $project->slug) }}
	</p>
	@stop

上面的代码很容易理解，关键的一点就是laravel中资源控制器为我们提供了对应**增删查改**这些HTTP请求的方法，可以去这个[地址](http://www.golaravel.com/docs/4.1/controllers/#resource-controllers)看看详细的说明。

### 创建添加和编辑页面 ###

创建了**增删查改**这些方法后，接下来就是创建对应的页面，主要是创建项目任务以及编辑项目任务的页面。

首先来创建创建项目和编辑项目的页面，代码如下：

    <!-- /app/views/projects/create.blade.php -->
	@section('main')
	<h2>Create Project</h2>
 
	{{ Form::model(new Project, ['route' => ['projects.store']]) }}
		@include('projects/partials/_form', ['submit_text' => 'Create Project'])
	{{ Form::close() }}
	@stop
 
	<!-- /app/views/projects/edit.blade.php -->
	@section('main')
	<h2>Edit Project</h2>
 
	{{ Form::model($project, ['method' => 'PATCH', 'route' => ['projects.update', $project->slug]]) }}
		@include('projects/partials/_form', ['submit_text' => 'Edit Project'])
	{{ Form::close() }}
	@stop

然后是任务的创建和编辑页面，代码如下：

    <!-- /app/views/tasks/create.blade.php -->
	@section('main')
		<h2>Create Task for Project "{{ $project->name }}"</h2>
	 
		{{ Form::model(new Task, ['route' => ['projects.tasks.store', $project->slug]]) }}
			@include('tasks/partials/_form', ['submit_text' => 'Create Task'])
		{{ Form::close() }}
	@stop
	 
	<!-- /app/views/tasks/edit.blade.php -->
	@section('main')
		<h2>Edit Task "{{ $task->name }}"</h2>
	 
		{{ Form::model($task, ['method' => 'PATCH', 'route' => ['projects.tasks.update', $project->slug, $task->slug]]) }}
			@include('tasks/partials/_form', ['submit_text' => 'Edit Task'])
		{{ Form::close() }}
	@stop

接下来我们熟悉下laravel的几个关于表单的几个概念和使用方法：

#### 引入公共的表单 ####

你可能会注意到在上面的代码中我们使用了**@include**方法。使用这个方法引入一个表单的视图文件，现在我们就来创建这两个表单视图文件，这是因为在创建项目遍及项目和创建任务遍及任务都用到相同的表单。首先在**/app/views/projects/**这个目录创建**partials**文件夹，然后在里面创建**_form.blade.php**文件，任务的表单视图文件的创建方法跟项目的表单创建方法一样。

#### 表单与模型绑定 ####

我们可以使用 Form::model 方法将模型中的内容填充到表单中。可以去[这个地址](http://www.golaravel.com/docs/4.1/html/#form-model-binding)详细了解关于表单和路由绑定的知识。

创建和编辑项目分别对应两个表单，并且使用了资源控制器提供的路由方法**projects.store**和**projects.update**来处理用户提交的数据。关于资源路由控制器的详细介绍可以去这个[地址](http://www.golaravel.com/docs/4.1/controllers/#resource-controllers)了解。

#### CSRF防御机制 ####

Laravel框架提供了一种简单的方法帮助你的应用抵御CSRF（跨站请求伪造）式攻击。Laravel框架会自动在用户 session 中存放一个随机令牌（token），并且将这个令牌作为一个隐藏字段包含在表单信息中。关于[CSRF防御机制](http://www.golaravel.com/docs/4.1/html/#csrf-protection)你可以去官方的文档了解详细的信息。

### 编写表单(Form) ###

前面我们分别为项目和任务创建了空的表单的视图文件，代码如下图所示：

首先是**/app/views/projects/partials/_form.blade.php**

![](http://pic.yupoo.com/reicky_v/DsVzvXlT/medium.jpg)

然后是**/app/views/tasks/partials/_form.blade.php**

![](http://pic.yupoo.com/reicky_v/DsVzxnrE/medium.jpg)

项目和任务的创建和编辑表单编写好后，接下来就来完善控制器的相关方法代码如下：

    // ProjectsController
	public function store()
	{
		$input = Input::all();
		Project::create( $input );
	 
		return Redirect::route('projects.index')->with('message', 'Project created');
	}
	 
	public function update(Project $project)
	{
		$input = array_except(Input::all(), '_method');
		$project->update($input);
	 
		return Redirect::route('projects.show', $project->slug)->with('message', 'Project updated.');
	}
	 
	public function destroy(Project $project)
	{
		$project->delete();
	 
		return Redirect::route('projects.index')->with('message', 'Project deleted.');
	}
	 
	// TasksController
	public function store(Project $project)
	{
		$input = Input::all();
		$input['project_id'] = $project->id;
		Task::create( $input );
	 
		return Redirect::route('projects.show', $project->slug)->with('Task created.');
	}
	 
	public function update(Project $project, Task $task)
	{
		$input = array_except(Input::all(), '_method');
		$task->update($input);
	 
		return Redirect::route('projects.tasks.show', [$project->slug, $task->slug])->with('message', 'Task updated.');
	}
	 
	public function destroy(Project $project, Task $task)
	{
		$task->delete();
	 
		return Redirect::route('projects.show', $project->slug)->with('message', 'Task deleted.');
	}

#### 信息提示 ####

在上面的代码中可以看到频繁的用到了**with()**这个方法。**with()**方法是用来传递数据给视图， with()方法将数据写到了Session中，通过Session::get 方法即可获取该数据。

在上面的代码中，我们用with()方法来根据用户的操作提示相对应的信息，比如当用户创建任务成功的时候，就提示用户成功创建任务。

### 总结 ###

这篇文章我们进一步学习了用laravel提供的表单的相关方法来操作和保存用户提交的数据以及表单跟模型绑定的知识，也知道了laravel为我们提供了CSRF防御机制来确保安全。

现在基本上整个程序能正常运行了，如果我们要修改我们的测试数据可以运行**migrate:refresh**这个命令来回滚并重新运行所有数据的迁移。