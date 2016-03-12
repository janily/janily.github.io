---
layout: post
title: laravel实战系列：使用laravel开发一个博客应用完结篇
categories:
- Life
tags:
- laravel
- php
---

通过上一篇文章完成了博客程序的控制器和模型部分代码的编写，并且还是用laravel提供Database Seeding功能填充了测试数据。今天我们继续来完善博客程序的功能，主要是还剩下路由和视图两部分，先来完成路由部分的代码。

### 路由 ###

这个路由得好好说说。打开**app/routes.php**文件，比如下面的代码：

	<?php
 
	// will be used to handle GET requests.
	Route::get('index',function()
	{
	    echo 'this is index page';
	});
	 
	Route::get('login',function()
	{
	    echo 'GET login requests will be hndled here.';
	});
	 
	// will be used to handle POST requests.
	Route::post('login', function() 
	{
	    echo 'POST login requests will be handled here.';
	});

上面几行简单的代码都是根据相对应的URL和请求类型来执行对应闭包函数的代码。

当然我们也可以直接在路由中直接指定对应的控制器中的方法来代替闭包方法：

	<?php
	Route::get('users', 'UsersController@getIndex');

上面的代码中表示**/users**这样的url会绑定**UserController**控制器中的**getIndex()**方法。当然也可以根据HTTP中的put和delete响应类型来使用**Route::put**和**Route::delete**路由方法。

#### 路由绑定参数 ####

先来看看下面的代码：

	<?php
 
	// 参数会传递给闭包处理的函数
	Route::any('post/{id}',function($id)
	{
	    echo "post with id: $id";
	});
	 
	//模型和路由绑定
	Route::model('post','Post'); //给路由参数{post}绑定模型
	 
	// 针对任何的请求类型都会输出模型中的id
	Route::any('post/{post}',function($post)
	{
	    echo "post with id: $post->id";
	});

#### 命名路由和路由过滤器 ####

> 路由过滤器提供了非常方便的方法来限制对应用程序中某些功能访问，例如对于需要验证才能访问的功能就非常有用。Laravel框架自身已经提供了一些过滤器，包括 auth过滤器、auth.basic过滤器、guest过滤器以及csrf过滤器。这些过滤器都定义在app/filter.php文件中。

比如：

	//定义一个过滤器
	Route::filter('simpleBeforeFilter', function()
	{
	    echo 'this is simple before filter';
	});
	 
	//给路由绑定过滤器
	Route::get('admin',['before'=>'simpleBeforeFilter',function()
	{
	    echo 'simple  Beforefilter is already called';
	}]);

我们也可以使用**as**来给路由指定自定义名字，重定向和生成URL时，使用命名路由会更方便。你可以为路由指定一个名字，如下所示：

	Route::get('user/profile', array('as' => 'profile', function()
	{
	    //
	}));

还可以为 controller action指定路由名称：

	Route::get('user/profile', array('as' => 'profile', 'uses' => 'UserController@showProfile'));

现在，你可以使用路由名称来创建URL和重定向：

	$url = URL::route('profile');

	$redirect = Redirect::route('profile');

详细的关于laravel中的路由知识可以去laravel官网文档地址看看，这里推荐laravel一个中文[文档地址](http://www.golaravel.com/docs/4.1/routing/#named-routes)。

### 创建博客程序的路由 ###

打开**routes.php**文件，编辑代码如下：

	/* 绑定模型 */
	Route::model('post','Post');
	Route::model('comment','Comment');
	 
	/* 普通用户 */
	Route::get('/post/{post}/show',['as' => 'post.show','uses' => 'PostController@showPost']);
	Route::post('/post/{post}/comment',['as' => 'comment.new','uses' =>'CommentController@newComment']);
	 
	/* 管理员用户 */
	Route::group(['prefix' => 'admin','before'=>'auth'],function()
	{
	    /*以管理员身份可以管理评论和日志*/
	    Route::get('dash-board',function()
	    {
	        $layout = View::make('master');
	        $layout->title = 'DashBoard';
	        $layout->main = View::make('dash')->with('content','Hi admin, Welcome to Dashboard!');
	        return $layout;
	 
	    });
	    Route::get('/post/list',['as' => 'post.list','uses' => 'PostController@listPost']);
	    Route::get('/post/new',['as' => 'post.new','uses' => 'PostController@newPost']);
	    Route::get('/post/{post}/edit',['as' => 'post.edit','uses' => 'PostController@editPost']);
	    Route::get('/post/{post}/delete',['as' => 'post.delete','uses' => 'PostController@deletePost']);
	    Route::get('/comment/list',['as' => 'comment.list','uses' => 'CommentController@listComment']);
	    Route::get('/comment/{comment}/show',['as' => 'comment.show','uses' => 'CommentController@showComment']);
	    Route::get('/comment/{comment}/delete',['as' => 'comment.delete','uses' => 'CommentController@deleteComment']);
	 
	    Route::post('/post/save',['as' => 'post.save','uses' => 'PostController@savePost']);
	    Route::post('/post/{post}/update',['as' => 'post.update','uses' => 'PostController@updatePost']);
	    Route::post('/comment/{comment}/update',['as' => 'comment.update','uses' => 'CommentController@updateComment']);
	 
	});
	 
	/* 首页路由 */
	Route::controller('/','BlogController');

上面的代码中我们绑定了两个模型到路由中，分别是**Post**和**Comment**模型。接着是**post.show**和**comment.show**两个路由，一个是显示日志，一个是用户发表评论的。

后面我们使用了一个路由组应用过滤器。使用路由组就可以避免单独为每个路由指定过滤器了。通过**prefix**属性为组路由设置前缀，这样我们在以管理员的身份编辑管理日志的时候，就会自动生成带有前缀的URL，如**URL::route('post.edit',12)**就会实际生成**http://localhost/admin/post/12/edit**这样的url地址。

至于验证部分，我们使用了laravel默认提供的**auth**和**guest**两个过滤器，来验证用户是否是登录的用户，如果不是登录用户则跳转到登录页面。在**auth**过滤器中具体代码是这样的：

	<?php
 
	//file: app/filters.php
	Route::filter('auth', function()
	{
	    if (Auth::guest()) return Redirect::guest('login');
	});

由于我们是使用了路由组来指定的过滤器，所以就避免单独为每个路由指定过滤器了。关于laravel中的**Auth**过滤器的使用可以去看看[这篇文章](http://www.codeheaps.com/php-programming/building-user-authentication-in-laravel-4-part-1/)。

下一部分是视图部分。

### 布局和视图 ###

在MVC类型的web可开发框架中的视图是用来展示经过处理后数据的以及应用程序的逻辑。在一个典型的MVC的web开发框架中，当用户发起一个请求的时候，通过路由指向对应的控制器中的方法来处理接受到的数据，然后通过视图把数据渲染出来。

下面通过一个简单的例子来了解一下laravel中视图的用法：

	<?php
	//file: app/controllers/IndexController.php
	 
	class IndexController extends BaseController {
	 
	    public function index()
	    {
	        return View::make('index',['name'=>'usman']);
	    }
	}
	 
	//file: app/routes.php
	 
	Route::get('index','IndexController@index');
	 
	//file: app/views/index.blade.php
	<html>
	<head>
	    <title>Index page</title>
	</head>
	<body>
	    <p> my name is {{$name}}</p>
	</body>
	</html>

在上面的代码中，**IndexController.php**文件中，定义了一个**index**方法即调用视图文件中的**index.blade.php**文件。而路由则制定使用控制器中的**index**方法，所以我们的视图就能响应对应的请求而展现出来。

#### 布局视图 ####

所谓布局视图就是用来定义一个网页整体的布局及外观，以及包含网页的一些共同的元素。比如，头部，底部以及边栏等等。laravel提供了两种方法来创建布局视图。

- 控制器布局视图
- 使用blade模板引擎的继承来使用布局视图

我们这里是在控制器里使用的布局视图，所以这里只是来创建相关的视图文件。

首先是创建布局视图文件，在**app/views/master.blade.php**代码如下图所示:

![](http://pic.yupoo.com/reicky_v/DJOl80T5/j1v8b.png)

![](http://pic.yupoo.com/reicky_v/DJOl812s/pXWRH.png)

> 由于我们是在控制器来调用布局视图文件的，所以需要在**BaseController**中定义**protected $layout='master'**；这样我们就可以在其它的控制器中调用视图文件。这里我们是以[Foundation 5](http://foundation.zurb.com/)这个前端框架来制作我们的视图的，具体可以去[这里](http://www.codeheaps.com/web-design/zurb-foundation-5-creating-responsive-blog-layout/)看看它的使用方法。

同样我们使用了**HTML**中的**HTML:style()**和**HTML:script()**方法来引入CSS和javascript文件。

> 在laravel中是使用blade模板引擎来渲染视图文件的。可以使用**{{$title}}**这样的语法来输出变量。也可以使用**@if @endif**、**@foreach @endforeach**等语法。

在上面的代码中**{{$main}}**使用来根据不同的方法来插入不同的子视图文件的。

我们来看看在**BlogController**中的**getIndex**方法中的一段代码，就知道怎么在控制器使用布局视图文件了：

	<?php
 
	//设置title
	$this->layout->title = 'Home Page | Laravel 4 Blog'; 
	 
	//使用nest()方法把index视图注入到home视图文件
	$this->layout->main = View::make('home')->nest('content','index',compact('posts'));

下面我们来创建视图文件，首先来看看视图文件的一个目录组成：

![](http://www.codeheaps.com/wp-content/uploads/2014/03/Blog-Tutrial-view-directory-300x275.png)

首先是：

**home.blade.php**

![](http://pic.yupoo.com/reicky_v/DJOGS5pd/14zRUt.png)

**index.blade.php**

![](http://pic.yupoo.com/reicky_v/DJOGSWhS/LRxVi.png)

**sidebar.blade.php**

![](http://pic.yupoo.com/reicky_v/DJOGTlbx/SAzAr.png)

在home视图文件中，主要有两个区域：一个是来显示index内容的，一个是用来显示sidebar视图的，使用了**@include**语法直接导入它。

在index.blade.php模板文件中，我们遍历出已经发表的日志的条目并且加上了**read more**的链接。

如下图所示：

![](http://www.codeheaps.com/wp-content/uploads/2014/01/laravel-blog-tutorial-300x199.png)

> 视图合成器即View::composer，视图合成器可以是回调函数或者类方法，它们在创建视图时被调用。如果你想在应用程序中，每次创建视图时都为其绑定一些数据，使用视图合成器可以将代码组织到一个地方。因此，视图合成器就好像是 “视图模型”或者是“主持人”。

在sidebar模板中，我们并没有处理**$recentPosts**变量。使用视图合成器可以这样来做：

	<?php
	 View::composer('sidebar', function($view)
	{
	    $view->recentPosts = Post::orderBy('id','desc')->take(5)->get();
	});

在管理员面板，有一些操作链接如发表日志，显示评论等。

使用了Foundation 5提供的[Reveal Modal](http://foundation.zurb.com/docs/components/reveal.html)插件来进行Ajax操作。

Ok，具体其它的视图文件这里就不详细说了可以去[下载源代码](https://github.com/usm4n/laravel-tutorial-codeheaps/archive/master.zip)详细查看。

至此一个简单的博客程序就完成了。







	