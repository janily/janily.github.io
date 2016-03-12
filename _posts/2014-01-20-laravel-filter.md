---
layout: post
title: laravel4中的filter的使用方法
categories:
- Life
tags:
- laravel
- php 
---

在web应用开发中经常会有这样的场景，就是对用户访问特定的web页面进行验证。不仅仅是登录注册之类的验证也包括对非注册用户的验证。

当然为了更好地组织我们的程序，我们需要把这种验证类型的功能抽象出来，比如在一个web应用中，我们可能有多个地方需要对用户进行验证，可以供我们随时调用。

laravel4为我们提供了**Filters**这个功能。Filters可以允许我们在路由里面设置相关的验证逻辑，这意味着我们可以指定某些特定的用户才能做特定的事情，比如管理员才能登录管理后台；注册用户才能发表文章。

在本文中，我们就来看看在laravel4中如何使用Filters来进行验证功能的编写。

### 什么是Filter ###

过滤器(Filter)提供了非常方便的方法来限制对应用程序中某些功能访问，例如对于需要验证才能访问的功能就非常有用。

比如，你只允许已经注册的用户来访问一些特定的页面，你可以在路由里面绑定一个Filter来验证用户是否是注册用户从而决定它能访问哪些页面。

当laravel在给用户返回请求信息之前，它会自动调用filter来对用户进行验证。如果filter对用户的请求进行响应的话，它就会自动处理相关的验证逻辑。比如当你的验证逻辑是验证用户是否是注册用户，如果它检测到当前的用户是没有登录的用户，那么它就会重定向到登录页面。

filter就是一个抽象的一个验证逻辑功能，它能在请求发生之前或者是之后来进行验证。

laravel提供了多种验证功能，可以去**app/filters.php**这个文件里看看。

### Before 和 After ###

这是两个最基本的验证模块：

    App::before(function($request)
	{
	  //
	});
	 
	App::after(function($request, $response)
	{
	  //
	});

顾名思义，这两个验证模块就是在请求前或者是请求后来运行的。

### 理解Filter ###

为了更好地理解Filter是如何运行的，我们来看看一个用户验证系统是如何运行的：

    Route::filter('auth', function()
	{
	  if (Auth::guest()) return Redirect::guest('login');
	});

路由Filter接受两个参数。第一个参数是filter的名字，这里是指**auth**。第二个参数是一个闭包函数([(What are PHP Lambdas and Closures?](http://culttt.com/2013/03/25/what-are-php-lambdas-and-closures/))这个函数就是你用来处理验证逻辑功能的。

所以在上面的这个用户验证模块中，laravel会检测当前用户是否是登录用户，如果不是登录用户则返回登录页面让用户登录。如果是登录用户则直接跳过验证模块响应请求。

### 给路由绑定Filter ###

一般我们会把Filter绑定到路由里面来实现我们想要的功能。

绑定一个路由只需要在路由第二个参数来定义我们的验证逻辑就可以了：

    Route::get('user/account', array('before' => 'auth',
	  'uses' => 'UserController@account',
	  'as' => 'user.account'
	));

在上面的例子中，我们绑定了一个用户的验证系统，只有注册的并且已经登录的用户才能进入用户账号的页面。

我们也可以给指定的请求绑定多个验证模块：

    Route::get('user/premium', array('before' => 'auth|premium',
	  'uses' => 'UserController@premium',
	  'as' => 'user.premium'
	));

在这个例子中，同时检测用户是否是注册用户以及是否是高级账号。

需要注意的是在多重验证中，如果第一个验证模块没有通过那后面的模块将不会再进行验证。这样的设置是合理的，因为第一个都没通过验证，那后面的验证就没有必要再进行验证了。

### 基于模式的过滤器(Filter) ###

你也可以指针对URI为一组路由指定过滤器。比如下面这个例子：

    Route::filter('admin', function()
	{
	  // Is this an admin?
	});
	 
	Route::when('admin/*', 'admin');

在这个例子中，admin过滤器将会应用到所有以admin/开头的路由中。星号是通配符，将会匹配任意多个字符的组合。

还可以针对HTTP动作限定模式过滤器：

    Route::when('admin/*', 'admin', array('post'));

这里只会验证发过来的post请求。

### 路由过滤组 ###

有时你可能需要为一组路由应用过滤器。使用路由组就可以避免单独为每个路由指定过滤器了：

    Route::group(array('before' => 'auth'), function()
	{
	  Route::get('user/account', 'UserController@account');
	  Route::get('user/settings', 'UserController@settings');
	  Route::get('post/create', 'PostController@create');
	  Route::post('post/store', 'PostController@store');
	  // ...
	});

在上面的代码中，每一个请求都会自动绑定我们指定的验证模块。

### 自定义验证 ###

在上面的代码中我们都是使用laravel提供的现有的验证模块来实现验证功能。laravel也允许我们自定义验证模块来实现特定的验证功能。

这样做有必要吗？如果你有很多复杂的验证需求，为了不使系统验证系统过于复杂，我们完全可以按照自己的需求来自定义过滤功能模块。

自定义验证功能需要用到laravel中的**IoC Container**(IoC 容器)这个功能，Laravel控制器反转容器**IoC Container**是一个强大的工具来处理类依赖关系。依赖注入是一个不用硬代码处理类依赖关系的方法。依赖关系是在运行时注入的，允许处理依赖时具有更大的灵活性。

比如下面这个例子：

    class ApiFilter {
 
	  public function filter()
	  {
	    if (!$this->valid($access_token)
	      return Response::json(array(
	      'error' => 'Your access token is not valid'
	    ), 403);
	  }
	 
	}

你就可以像下面这样来使用它：

    Route::filter('api.auth', 'ApiFilter');

### 总结 ###

过滤验证(Filter)非常容易使用，只需要我们定义一次，就可以在不同的路由请求里使用。

几乎99%的web应用都会使用过滤验证功能。虽然在本篇文章中我们使用的例子都是基于用户验证系统来讲解验证系统的，但是它的用法完全不止于此，我们可以在任何我们想验证过滤的地方来使用它。可以去官方的文档地址详细了解其使用方法这里提供[中文文档的地址](http://www.golaravel.com/docs/4.0/routing/#route-filters)可以去看一下。

