---
layout: post
title: laravel实战学习-用laravel4创建一个ToDo应用(1)
categories:
- Life
tags:
- laravel
- php
---

> 接下来大概会花2到3篇文章来用laravel4来创建一个todo应用，来进一步学习laravel，主要是参考[flynsarmy](http://www.flynsarmy.com/)网站的[Creating a Basic ToDo Application With Laravel 4](http://www.flynsarmy.com/2013/12/creating-a-basic-todo-application-with-laravel-4-part-1/)这系列的文章，这个系列介绍的非常详细，几乎是手把手教你怎么用laravel，由于我前面也写了一些laravel基础方面的教程，这里就不再详细阐述了，如果是初次接触laravel可以去上面的地址看看。阅读本文前需要你熟悉laravel。

开始之前，就要把laravel环境安装好，至于环境安装可以去看我以前写的[这篇文章](http://janily.gitcafe.com/life/2013/11/15/laravel-restful/)里面有介绍。

安装好后，就正式来开始编码吧，首先在命令行里cd你的项目目录然后运行下面的命令：

    composer create-project laravel/laravel l4todo --pre-dist

安装好后，接下来是设置数据库。打开*/app/config/database.php*这个文件，设置好用户名和密码以及你要连接的数据库的名字，如下所示：

    'connections' => array(
 
	'mysql' => array(
		'driver'    => 'mysql',
		'host'      => 'localhost',
		'database'  => 'l4todo',
		'username'  => 'my_username',
		'password'  => 'my_password',
		'charset'   => 'utf8',
		'collation' => 'utf8_unicode_ci',
		'prefix'    => '',
	),
 
	),

接下来安装**Laravel 4 Generators**这个包，这个包扩展了[artisan](http://laravel.com/docs/artisan)命令的功能，可以大大提高我们的开发效率，具体安装方法可以去它的[github地址查看](https://github.com/JeffreyWay/Laravel-4-Generators)或者是这个地址看[视频](http://tutsplus.s3.amazonaws.com/tutspremium/courses_%24folder%24/WhatsNewInLaravel4/9-Generators.mp4)介绍。

任务管理类的应用一般是由项目和任务列表组成。具体到laravel中我们需要创建**project（项目）**和**task(任务)**这两个模型以及相关的视图、数据表、测试数据以及路由。这些这里就不再阐述了这些也就是现在流行的**MVC**开发模式，你可以去看看这个[视频](http://tutsplus.s3.amazonaws.com/tutspremium/courses_%24folder%24/WhatsNewInLaravel4/9-Generators.mp4)了解laravel的开发模式。

下面我们先来使用Generators建立好项目和任务这两个数据表如下所示：

    php artisan generate:resource project --fields="name:string, slug:string"
    php artisan generate:resource task --fields="project_id:integer, name:string, slug:string, completed:boolean, description:text"

我们只需要运行这两个命令，generate这个包就会为我们自动生成相关的模型、控制器以及视图文件。

现在打开** /app/database/migrations **这个目录，你会看到两个migrations文件。我们将会以projects这个表中的project_id来作为task表的外键约束以保证数据的完整性和一致性。

修改tasks这个scheme代码如下所示：

   public function up()
	{
		Schema::create('tasks', function(Blueprint $table) {
			$table->increments('id');
			$table->integer('project_id')->unsigned();
			$table->foreign('project_id')->references('id')->on('projects')->onDelete('cascade');
			$table->string('name');
			$table->string('slug');
			$table->boolean('completed');
			$table->text('description');
			$table->timestamps();
		});
	}

在上面的代码中你会看到**onDelete(‘cascade’) **这样的语句。这个语句的意思是当我们删除一个projects(项目)的时候，跟这个项目相关联的任务也会自动删除，外键约束的作用就凸显出来了。

配置好后，运行下面的命令来创建这两个表：

    php artisan migrate:refresh

然后回到数据库会发现两个表已经建好了。

### 用seeds来创建测试数据 ###

关于seeds可以去看看[这篇文章](https://tutsplus.com/lesson/database-seeding/)。

我们将添加一些项目和任务的测试数据。分别打开**/app/database/seeds/ **这个目录下的ProjectsTableSeeder.php和TasksTableSeeder.php这两个文件。我们就可以根据相应表来添加数据，这里先来添加3个项目和5个任务。不要忘了把**created_at** 和 **updated_at**这两个字段设置为自动更新当前的时间。

代码如下：

    class ProjectsTableSeeder extends Seeder {
 
	public function run()
	{
		// Uncomment the below to wipe the table clean before populating
		// DB::table('projects')->truncate();
 
		$projects = array(
			['name' => 'Project 1', 'slug' => 'project-1', 'created_at' => new DateTime, 'updated_at' => new DateTime],
			['name' => 'Project 2', 'slug' => 'project-2', 'created_at' => new DateTime, 'updated_at' => new DateTime],
			['name' => 'Project 3', 'slug' => 'project-3', 'created_at' => new DateTime, 'updated_at' => new DateTime],
		);
 
		// Uncomment the below to run the seeder
		DB::table('projects')->insert($projects);
	}
 
	}

	class TasksTableSeeder extends Seeder {
 
	public function run()
	{
		// Uncomment the below to wipe the table clean before populating
		// DB::table('tasks')->truncate();
 
		$tasks = array(
			['name' => 'Task 1', 'slug' => 'task-1', 'project_id' => 1, 'description' => 'My first task', 'created_at' => new DateTime, 'updated_at' => new DateTime],
			['name' => 'Task 2', 'slug' => 'task-2', 'project_id' => 1, 'description' => 'My second task', 'created_at' => new DateTime, 'updated_at' => new DateTime],
			['name' => 'Task 3', 'slug' => 'task-3', 'project_id' => 1, 'description' => 'My third task', 'created_at' => new DateTime, 'updated_at' => new DateTime],
			['name' => 'Task 4', 'slug' => 'task-4', 'project_id' => 2, 'description' => 'My fourth task', 'created_at' => new DateTime, 'updated_at' => new DateTime],
			['name' => 'Task 5', 'slug' => 'task-5', 'project_id' => 2, 'description' => 'My fifth task', 'created_at' => new DateTime, 'updated_at' => new DateTime],
		);
 
		// Uncomment the below to run the seeder
		DB::table('tasks')->insert($tasks);
	}
 
	}

然后运行想下面的命令

    php artisan db:seed或者是php artisan migrate:refresh --seed

就会在对应的表中生成我们定义好的数据。

### 使用Artisan的Tinker来测试程序 ###

现在数据库已经建好了，我们再来介绍artisan命令中的一个好东东**Tinker**，它能提供一个交互式的命令来测试我们的程序，可以去看看[这篇文章](http://blog.enge.me/post/tinkering-with-tinker-like-an-artisan)了解详情。比如，我们可以用它来统计我们数据库中项目的数量：

    $ php artisan tinker
	>echo Project::count();    
	3

这个命令非常有用，后面我们还会用到它。

### Routes(路由) ###

[深入了解](https://tutsplus.com/lesson/nested-resources/)

让我们在命令行里运行下面这个命令来看看我们的路由结构和对应的功能：

    $ php artisan routes
	+--------+-------------------------------+------------------+----------------------------+----------------+---------------+
	| Domain | URI                           | Name             | Action                     | Before Filters | After Filters |
	+--------+-------------------------------+------------------+----------------------------+----------------+---------------+
	|        | GET /                         |                  | Closure                    |                |               |
	|        | GET /projects                 | projects.index   | ProjectsController@index   |                |               |
	|        | GET /projects/create          | projects.create  | ProjectsController@create  |                |               |
	|        | POST /projects                | projects.store   | ProjectsController@store   |                |               |
	|        | GET /projects/{projects}      | projects.show    | ProjectsController@show    |                |               |
	|        | GET /projects/{projects}/edit | projects.edit    | ProjectsController@edit    |                |               |
	|        | PUT /projects/{projects}      | projects.update  | ProjectsController@update  |                |               |
	|        | PATCH /projects/{projects}    |                  | ProjectsController@update  |                |               |
	|        | DELETE /projects/{projects}   | projects.destroy | ProjectsController@destroy |                |               |
	|        | GET /tasks                    | tasks.index      | TasksController@index      |                |               |
	|        | GET /tasks/create             | tasks.create     | TasksController@create     |                |               |
	|        | POST /tasks                   | tasks.store      | TasksController@store      |                |               |
	|        | GET /tasks/{tasks}            | tasks.show       | TasksController@show       |                |               |
	|        | GET /tasks/{tasks}/edit       | tasks.edit       | TasksController@edit       |                |               |
	|        | PUT /tasks/{tasks}            | tasks.update     | TasksController@update     |                |               |
	|        | PATCH /tasks/{tasks}          |                  | TasksController@update     |                |               |
	|        | DELETE /tasks/{tasks}         | tasks.destroy    | TasksController@destroy    |                |               |
	+--------+-------------------------------+------------------+----------------------------+----------------+---------------+

从上面我们可以看到我们的**projects**和**tasks**是并行的，在我们一般的todo类型应用中**tasks**应该是属于**projects**的，所以它的url应该是这样的比如：** /projects/1/tasks/3**这叫做路由嵌套。在laravel中设置这样的路由非常简单，如下所示：

    // Route::resource('tasks', 'TasksController');
	Route::resource('projects.tasks', 'TasksController');

现在再使用**php artisan routes**命令来查看一下路由：

    $ php artisan routes
	+--------+---------------------------------------------+------------------------+----------------------------+----------------+---------------+
	| Domain | URI                                         | Name                   | Action                     | Before Filters | After Filters |
	+--------+---------------------------------------------+------------------------+----------------------------+----------------+---------------+
	|        | GET /                                       |                        | Closure                    |                |               |
	|        | GET /projects                               | projects.index         | ProjectsController@index   |                |               |
	|        | GET /projects/create                        | projects.create        | ProjectsController@create  |                |               |
	|        | POST /projects                              | projects.store         | ProjectsController@store   |                |               |
	|        | GET /projects/{projects}                    | projects.show          | ProjectsController@show    |                |               |
	|        | GET /projects/{projects}/edit               | projects.edit          | ProjectsController@edit    |                |               |
	|        | PUT /projects/{projects}                    | projects.update        | ProjectsController@update  |                |               |
	|        | PATCH /projects/{projects}                  |                        | ProjectsController@update  |                |               |
	|        | DELETE /projects/{projects}                 | projects.destroy       | ProjectsController@destroy |                |               |
	|        | GET /projects/{projects}/tasks              | projects.tasks.index   | TasksController@index      |                |               |
	|        | GET /projects/{projects}/tasks/create       | projects.tasks.create  | TasksController@create     |                |               |
	|        | POST /projects/{projects}/tasks             | projects.tasks.store   | TasksController@store      |                |               |
	|        | GET /projects/{projects}/tasks/{tasks}      | projects.tasks.show    | TasksController@show       |                |               |
	|        | GET /projects/{projects}/tasks/{tasks}/edit | projects.tasks.edit    | TasksController@edit       |                |               |
	|        | PUT /projects/{projects}/tasks/{tasks}      | projects.tasks.update  | TasksController@update     |                |               |
	|        | PATCH /projects/{projects}/tasks/{tasks}    |                        | TasksController@update     |                |               |
	|        | DELETE /projects/{projects}/tasks/{tasks}   | projects.tasks.destroy | TasksController@destroy    |                |               |
	+--------+---------------------------------------------+------------------------+----------------------------+----------------+---------------+

设置完成了，我是越来越喜欢laravel了。

### 优化URLs ###

现在我们的url是这样的**/projects/1/tasks/2**，如果我们把当前的任务的名字代替数字这样的体验会更好一些，如**/projects/my-first-project/tasks/buy-milk**。

打开路由文件**/app/routes.php**和添加以下代码：

    Route::bind('tasks', function($value, $route) {
	return Task::whereSlug($value)->first();
	});
	Route::bind('projects', function($value, $route) {
		return Project::whereSlug($value)->first();
	});

上面的代码会改写我们的url。

我们来总结一下本文我们学到了一些什么：

- 创建了两个资源控制器
- 用migrations来创建数据库
- 创建测试数据
- 设置URL

先到这里，下篇文章我们来编写前端方面的代码。