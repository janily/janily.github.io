---
layout: post
title: laravel实战学习-用laravel4创建一个ToDo应用(4)
categories:
- Life
tags:
- laravel
- php
---

前面三篇文章我们基本完成了主要功能的编写，在这个过程中学习了laravel的一些知识，在本文中我们来学习关于用laravel来验证数据和URL管理方面的知识。

### 服务端验证数据 ###

一般只要是涉及数据提交的都需要进行验证特别是表单提交的数据更是如此。这篇文章我们就来为我们的程序来添加验证。

需要用到[Adrent](https://github.com/laravelbook/ardent)这个第三方包，在composer.json文件中添加下面的代码然后运行**composer update**来安装这个包。也可以运行下面这个命令来安装第三方依赖包：

    composer require "laravelbook/ardent":"dev-master"

既然有验证，那就需要有验证信息提示给用户。打开**/app/views/layouts/main.blade.php**文件中添加以下代码：

    <div id="content">
	@if (Session::has('message'))
		<div class="flash alert">
			<p>{{ Session::get('message') }}</p>
		</div>
	@endif
	@if ($errors->any())
		<ul>
			{{ implode('', $errors->all('<li class="error">:message</li>')) }}
		</ul>
	@endif
 
	@yield('main')
	</div>

然后可以去ardent的github地址看它的[使用方法](https://github.com/laravelbook/ardent#validation)，从文档中可以知道我们的模型需要继承
**\LaravelBook\Ardent\Ardent**并且需要定义相关的验证规则：

    // /app/models/Project.php
	use LaravelBook\Ardent\Ardent;
	class Project extends Ardent {
	public static $rules = array(
		'name'			=> 'required|min:4',
		'slug'			=> 'required',
	);
 
	...
 
	// /app/models/Task.php
	use LaravelBook\Ardent\Ardent;
	class Project extends Ardent {
	public static $rules = array(
		'name'			=> 'required|min:4',
		'slug'			=> 'required|unique',
		'description'	=> 'required',
	);
 
具体详细验证规则可以去laravel的[这个地址](http://laravel.com/docs/validation#available-validation-rules)看看。上面代码定义的规则就是直接从laravel文档地址贴过来的。Ardent不需要我们指定特定的数据表的id来绑定验证规则但是需要调整控制器里的相关方法。

我们来调整下控制器里**store()**和**update()**方法代码如下：

    // /app/controllers/ProjectsController.php
	public function store()
	{
		$input = Input::all();
		$project = new Project($input);
	 
		if ( $project->save() )
			return Redirect::route('projects.index')->with('message', 'Project created.');
		else
			return Redirect::route('projects.create')->withInput()->withErrors( $project->errors() );
	}
 
	public function update(Project $project)
	{
		$input = Input::all();
		$project->fill($input);
	 
		if ( $project->updateUniques() )
			return Redirect::route('projects.show', $project->slug)->with('message', 'Project updated.');
		else
			return Redirect::route('projects.edit', array_get($project->getOriginal(), 'slug'))->withInput()->withErrors( $project->errors() );
	}
 
	// /app/controllers/TasksController.php
	public function store(Project $project)
	{
		$input = Input::all();
		$input['project_id'] = $project->id;
		$task = new Task($input);
	 
		if ( $task->save() )
			return Redirect::route('projects.show', $project->slug)->with('message', 'Task created.');
		else
			return Redirect::route('projects.tasks.create', $project->slug)->withInput()->withErrors( $task->errors() );
	}
	 
	public function update(Project $project, Task $task)
	{
		$input = Input::all();
		$task->fill($input);
	 
		if ( $task->updateUniques() )
			return Redirect::route('projects.tasks.show', [$project->slug, $task->slug])->with('message', 'Task updated.');
		else
			return Redirect::route('projects.tasks.edit', [$project->slug, array_get($task->getOriginal(), 'slug')])->withInput()->withErrors( $task->errors() );
	}

上面的代码可能有一点点复杂，因为我们添加了一些验证方面的东东。

首先来说说**store()**这个方法:

- 首先是获取从表单发送过来的数据
- 把获取过来的数据保存在一个新的模型实例里面
- 保存数据并显示到对应的页面

这里使用了**withInput()**这个方法来接收从表单发送过来的数据并且验证数据的合法性把验证结果的信息通过**withErrors**来显示在页面上。

然后再来说说**update()**这个方法里的代码：

- 跟store()方法一样首先获取表单发送过来的数据
- 把数据保存到模型实例中
- 在保存跟新数据的时候我们使用了ardent提供的**updateUniques()**这个方法。关于这个方法可以去ardent的[文档](https://github.com/laravelbook/ardent#uniquerules)看详细的说明，因为在我们定义的规则中**slug**这个字段我们定义的规则是**unique**，所以如果按laravel默认的验证规则的话，你必须通过它对应的不同ID来更新它，否则laravel认为你这是重复提交数据而不会去更新它。而ardent中的**updateUnique**方法很好的解决了这个问题。
- 通过验证后，重定向到显示页面显示我们更新的数据
- 如果没有通过验证，不能直接返回到**$project->slug**这个数据，因为我们的数据模型已经包含了一些更新数据的细节。我们需要使用下面的方法返回当前表单中的数据：

    array_get($task->getOriginal(), 'slug')

可以好好理解下这里的代码，试着实际运行下程序来验证我们刚刚写的代码。

### 更好地管理slug ###

先来说说**slug**这个是用来干啥的，slug是为了更好的管理url。

比如在一个博客中，我们一般这样来使用id来定义每一篇文章的地址：

    http://example.com/post/1
	http://example.com/post/2

但是这样不利于SEO，如果用文章的title来作为参数传给URL，但是这样也有一个问题就是URL会变得特别长简直无法直视：

    http://example.com/post/My+Dinner+With+Andr%C3%A9+%26+Fran%C3%A7ois

解决方法就是定义一个slug个字段来代替title。laravel提供了**Str::slug()**方法来定义生成slug：

    http://example.com/post/my-dinner-with-andre-francois

这样的url地址优雅简洁还有利于搜索引擎的收录。

我们可以使用[Eloquent-Sluggable](http://registry.autopergamene.eu/package/cviebrock-eloquent-sluggable)这个第三方的包来解决这个问题。

在**composer.json**中添加这个包的依赖：

    "cviebrock/eloquent-sluggable": "1.0.*"

运行**composer update**来安装。

然后打开**/app/config/app.php**把eloquent-sluggable服务注册到数组中：

    'providers' => array(
	...
	'CviebrockEloquentSluggableSluggableServiceProvider',
	),
 
	'aliases' => array(
	...
	'Sluggable'       => 'CviebrockEloquentSluggableFacadesSluggable',
	),

然后运行下面的命令生成配置文件：

    php artisan config:publish cviebrock/eloquent-sluggable

#### 配置 ####

然后我们可以打开**/app/config/packages/cviebrock/eloquent-sluggable/config.php **这个设置文件设置**build_from**这个选项name的值。

打开**/app/models/Project.php**和**/app/models/Task.php**文件来设置**sluggable**：

    public static $sluggable = array();

我们不需要手动设置Sluggable，这一切只需要交给**Eloquent-Suggable**来处理就行了。打开**/app/views/projects/partials/_form.blade.php** 和 **/app/views/tasks/partials/_form.blade.php **文件，删除设置**slug**而存在的li标签。

最后我们需要在控制器中设置**Eloquent-Sluggable**。一般它会自动来处理相关的工作但是为了捕鱼Ardent冲突，我们需要手动来调用Sluggable代码如下：

    // /app/controllers/ProjectsController.php
	public function store()
	{
		$input = Input::all();
		$project = new Project($input);
		Sluggable::make($project);
	 
		if ( $project->save() )
			return Redirect::route('projects.index')->with('message', 'Project created.');
		else
			return Redirect::route('projects.create')->withInput()->withErrors( $project->errors() );
	}
 
	// /app/controllers/TasksController.php
	public function store(Project $project)
	{
		$input = Input::all();
		$input['project_id'] = $project->id;
		$task = new Task($input);
		Sluggable::make($task);
	 
		if ( $task->save() )
			return Redirect::route('projects.show', $project->slug)->with('message', 'Task created.');
		else
			return Redirect::route('projects.tasks.create', $project->slug)->withInput()->withErrors( $task->errors() );
	}

设置完后我们可以来创建一些新的项目和任务来测试程序是否正确。

### 总结 ###

通过这四篇文章，我们学习laravel中第三方包的使用，生成测试数据和用migrations来生成数据表、视图模板、通过资源控制来处理数据的增删查改以及通过第三方包来进行数据验证，还有URL的管理方面的知识。还了解了laravel中像路由模型绑定和CSRF防御等一些高级的概念。尽管我们做的这个todo程序看起来好像很简单，正所谓合抱之木，生于垒土；千里之行，始于足下。一个大型的应用程序也是一个一个小的程序组成，而laravel能够使我们的开发变得更加简单和快乐。Happy coding！




    