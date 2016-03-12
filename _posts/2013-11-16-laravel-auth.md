---
layout: post
title: larvavel4开发web app(2)：用户验证系统学习
categories:
- Life
tags:
- php
- 翻译
---

> 上一篇文章我们学习了用laravel4来打造restful的开发方法，这一片文章我们来学习一下在laravel中怎么来实现用户的验证，我们这一片文章里的代码是基于上一篇文章的代码来做的。这篇文章主要参考了[tutsplus](http://net.tutsplus.com/)网站的[authentication-with-laravel-4/](http://net.tutsplus.com/tutorials/php/authentication-with-laravel-4/)。

由于我们的UI是基于bootstrap这个前端系统来做的，所以我们首先需要安装这个前端库，这个用composer可以很容易的安装，我们只需要在**composer.json**里面添加bootstrap的依赖：

    {
	   "name": "laravel/laravel",
	   "description": "The Laravel Framework.",
	   "keywords": ["framework", "laravel"],
	   "require": {
	      "laravel/framework": "4.0.*",
	      "twitter/bootstrap": "3.0.*@dev"
	 },

然后运行：

    composer update

我们就可以在项目目录里的**vendor**文件夹里看到bootstrap这个库的文件了，如图：

![](http://pic.yupoo.com/reicky_v/DjEg57nj/medium.jpg)

不过Twitter Bootstrap默认是基于less的，我们下下来的文件并没有编译成css，我们可以自行编译less文件，不过我们需要用npm安装它需要的依赖文件。这个需要nodejs环境，我们cd到bootstrap所在的目录。然后运行下面的命令：

    cd ~/Sites/laravel-auth/vendor/twitter/bootstrap
	npm install

安装好所需要的依赖文件后，我们可以用grunt命令来编译less文件，我们下载好的bootstrap已经帮我们写好常见的一些grunt任务，编译less当然不在话下，我们运行下面命令：

    grunt recess

这样就把less文件编译为css文件了。如图：

![](http://pic.yupoo.com/reicky_v/DjElbBjS/medium.jpg)

这里就有一个问题了，默认编译的css文件没有在我们项目的目录里，css文件在laravel4中应该是放在public文件夹里面。当然这用命令也可以很容易把css文件移到public目录里面，我们在命令行里运行以下的命令：

    cd ~/Sites/laravel-auth
	php artisan asset:publish --path="vendor/twitter/bootstrap/bootstrap/css" bootstrap/css

这样我们的css文件就会移到public文件夹里，如图：

![](http://pic.yupoo.com/reicky_v/DjEnvsY8/medium.jpg)

接下来是数据库的创建和设置，这里为了节省时间，我们就用上篇文章已经创建好的用户的数据库来作为基础，这里就不再写了。

一些基本设置完成后，我们可以用下面的命令，来开启服务器，如下图：

![](http://pic.yupoo.com/reicky_v/DjEuU1HS/medium.jpg)

接下来是前端UI制作了，由于我们用的bootstrap这个前端库来做的，非常容易就可以搭建几个页面。

laravel为我们提供非常好用的**blade**模板文件，关于**blade**模板的用法，可以去laravel官方的文档看看，我们首先把公用的布局独立出来为一个公共模板文件，在**app/views/**这个目录下创建一个**main.blade.php**的文件，然后写入下面的代码：

    
	<!DOCTYPE html>
	<html lang="en">
	  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="">
    <meta name="author" content="">
    <link rel="shortcut icon" href="../../docs-assets/ico/favicon.png">

    <title>readlater</title>

    <!-- Bootstrap core CSS -->
    {{HTML::style('packages/css/bootstrap.min.css')}}
    <!-- Custom styles for this template -->
    {{HTML::style('packages/css/main.css')}}
    <!-- Just for debugging purposes. Don't actually copy this line! -->
    <!--[if lt IE 9]><script src="../../docs-assets/js/ie8-responsive-file-warning.js"></script><![endif]-->

    <!-- HTML5 shim and Respond.js IE8 support of HTML5 elements and media queries -->
    <!--[if lt IE 9]>
      <script src="https://oss.maxcdn.com/libs/html5shiv/3.7.0/html5shiv.js"></script>
      <script src="https://oss.maxcdn.com/libs/respond.js/1.3.0/respond.min.js"></script>
    <![endif]-->
	</head>
	
	<body>

    <div class="container">
       {{ $content }}
    </div> <!-- /container -->

    <!-- Bootstrap core JavaScript
    ================================================== -->
    <!-- Placed at the end of the document so the pages load faster -->
	  </body>
	</html>

我们这里新建了一个main.css的文件，用来编写我们自己的样式，接着我们来继续完善我们的布局文件，我们这里需要一个导航用来登录和注册用，我们可以在*body*标签中插入一下代码：

    <div class="navbar navbar-fixed-top">
      <div class="navbar-inner">
         <div class="container">
            <ul class="nav">  
               <li>{{ HTML::link('users/register', 'Register') }}</li>   
               <li>{{ HTML::link('users/login', 'Login') }}</li>   
            </ul>  
         </div>
      </div>
    </div> 

当然对于一个web应用来说，在涉及用户操作的时候我们需要给用户提示一些信息，比如注册成功的信息提示等提示信息。至于信息的提示是通过控制器来控制的，当然需要在网页中显示出来，由于信息提示这类信息大部分是公共的，所以我们把它放在公共的布局文件里。所以我们需要调整下代码，如下所示：

    <div class="navbar navbar-fixed-top">
      <div class="navbar-inner">
         <div class="container">
            <ul class="nav">  
               <li>{{ HTML::link('users/register', 'Register') }}</li>   
               <li>{{ HTML::link('users/login', 'Login') }}</li>   
            </ul>  
         </div>
      </div>
   	</div> 
             
 
    <div class="container">
      @if(Session::has('message'))
         <p class="alert">{{ Session::get('message') }}</p>
      @endif
    </div>

我们这里用了一个**if**语句来检测是否有信息要显示，具体是用到了**Session::has()**这个方法来检测是否有信息显示。

### 创建注册页面 ###

完成基本的布局页面后，接下来是具体创建注册和登陆页面啦，我们先来创建注册页面。

我们首先在**app/controllers**目录创建一个**UsersController**文件，写入下面的代码：

    <?php
 
	class UsersController extends BaseController {
	 
	}
	?>

先在**UserController**控制器里面，我们需要定义一个方法，用来跳转到注册页面，就取名为**getRegister**:

    public function getRegister() {
	   $this->layout->content = View::make('users.register');
	}

我们在公共布局模板文件里定义了content这个变量，所以我们这里注册部分的html代码赋值给content变量，从而输出一个完整的页面。

### 创建路由 ###

为了让页面能够正确跳转到指定的页面，我们需要在路由文件里定义页面跳转的路径。路由文件在这个目录**app/routes.php**，我们需要删除默认的路由定义，创建我们自己的路由：

    Route::controller('users', 'UsersController');

现在只要我们在控制器创建一个新的方法，它就对应路由里面url的对应格式**/users/actionName**。比如，我们有一个**getRegister**方法，那么通过**/users/register**地址就可以访问到我们的页面。

### 创建注册页面 ###

在视图目录（app/views）创建一个users的文件夹。这里就放UserController这个控制器里的视图文件。在**users**文件夹里，创建一个**register.blade.php **的文件，输入下面的代码：

![](http://pic.yupoo.com/reicky_v/DlCkEg4l/medium.jpg)

我们这里用到了laravel4中提供表单的方法来构建我们的表单。我们通过表单中的**open**方法传入一个数组的参数，定义了处理数据的方法的路径**users/create **，通过这个方法我们就可以接受用户数据创建用户。还给表单定义了一个类**form-signup**。

接下来我们通过一个循环来输出表单的验证消息**$errors**。

接下来就是具体的表单元素了，统一的类名为**input-block-level**和占位文字。具体的表单元素可以去官方的文档，我这里给一个中文文档的地址可以去看看，[表单元素](http://www.golaravel.com/docs/html.html)。

记得在表单元素创建完后，我们需要加上**close()**这一句的语法。

接下来就可以开启服务器，通过这个地址**http://localhost:8000/users/register **来访问我们的注册页面，如下图所示：

![](http://pic.yupoo.com/reicky_v/DjFEN0nn/medium.jpg)

### 接受表单数据，执行注册方法 ###

现在如果你点击注册按钮的话，会出现错误提示**NotFoundHttpException**。这是因为我们还没有定义相关的注册方法，接下来就来实现注册方法。

先在**UserController**里创建一个**postCreate**的方法：

    public function postCreate() {
       
	}

通过这个方法，我们来处理表单提交过来的数据，当然还是要验证一下数据的合法性，才保存到数据库。

#### 表单验证 ####

laravel4也为我们提供了表单验证方面的东东。首先我们需要创建表单的验证规则。我一般在模型里面定义数据的验证规则。laravel默认已经为我们在模型文件夹里（models）建立了一个**User.php**的模型文件。相关的表单验证你可以去这个地址看看，[验证](http://www.golaravel.com/docs/validation.html)。

打开**app/models**目录里的*user.php*文件，添加下述代码：

    public static $rules = array(
	   'username'=>'required|alpha|min:2',
	   'password'=>'required|alpha_num|between:6,12|confirmed',
	   'password_confirmation'=>'required|alpha_num|between:6,12'
   );

至于规则所代表的意思，可以去上面给的验证的那个地址看看详情。

验证规则定义好后，我们就可以在**UserController**控制器里来验证我们的数据。在控制器里的**postCreate**方法里面添加如下代码来验证用户输入的数据：

    public function postCreate() {
	   $validator = Validator::make(Input::all(), User::$rules);
	 
	   if ($validator->passes()) {
	      // 如果验证通过，则存入数据库。
	   } else {
	      // 不通过，提示错误信息。
	   }
	}
	}

我们通过调用模型里面的规则来验证用户的数据。**Validator**方法接受两个参数，一个是验证规则，一个是接受到的数据。我们可以通过**Input::all()**来接收用户提交的数据。验证规则就是在模型里面定义好的规则。

我们可以通过**pass()**方法来判断用户提交的数据是否通过验证。它会返回**true**和**false**布尔值。

如果通过验证，那么我们添加下面的代码来保存数据：

    if ($validator->passes()) {
	   $user = new User;
	   $user->username = Input::get('lastname');
	   $user->password = Hash::make(Input::get('password'));
	   $user->save();
	 
	   return Redirect::to('users/login')->with('message', 'Thanks for registering!');
	} else {
	   // 错误提示信息  
	}

如果通过验证，我们创建了一个user模型的实例；通过$user变量，用**Input::get()**方法接收用户发送来的数据，从而把相关的数据保存到数据库中。然后通过**save()**方法来保存数据。

数据保存成功后，我们用**Redirect::to()**方法把页面重定向到登陆页面，还可以通过**with()**方法来传递一个消息提醒。提示用户注册成功，可以进行登录啦。

如果没有通过验证，我们需要重定向到注册页面，并且提醒用户相关的错误信息。如下所示，在**else**语句里面：

    if ($validator->passes()) {
	   $user = new User;
	   $user->username = Input::get('email');
	   $user->password = Hash::make(Input::get('password'));
	   $user->save();
	 
	   return Redirect::to('users/login')->with('message', '!');
	} else {
	   return Redirect::to('users/register')->with('message', 'The following errors occurred')->withErrors($validator)->withInput();
	}

这里我们通过**withErrors($validator)**方法来检测相关的错误信息，然后用**withInput**方法来输出错误信息。

#### CSRF防御机制 ####

Laravel提供了一种简单的方法帮助你的应用抵御CSRF（跨站请求伪造）式攻击。我们可以在我们的**UsersController**控制器里面添加一个构造函数，在构造函数里添加CSRF机制：

    public function __construct() {
	   $this->beforeFilter('csrf', array('on'=>'post'));
	}

Laravel框架会自动在用户 session 中存放一个随机令牌（token），并且将这个令牌作为一个隐藏字段包含在表单信息中。我们这里定义在客户端发起的**post**请求的时候，就启用CSRF防御机制。

#### 创建登录页面 ####

注册成功后，我们需要一个登录页面让用户登陆。

还是在**UserController**控制器里，创建一个**getLogin**方法来处理登录的逻辑：

    public function getLogin() {
    	return View::make('users.login');
	}

这样就会跳转到我们的登录页面，我们需要在**app/views/users**目录里面创建一个登陆页面的模板**login.blade.php**添加下面的代码：

![](http://pic.yupoo.com/reicky_v/DlCkExD3/medium.jpg)

这段代码跟注册模板的页面差不多。

现在我们差不多实现了注册的功能，并且能够正常跳转，下面是我注册一个账号后跳转到登录页面的情形。

![](http://pic.yupoo.com/reicky_v/DjGfAlfl/medium.jpg)

#### 登录处理 ####

注册成功后，还不能登录。我们需要创建一个登录方法来处理登陆。在**UsersController**创建一个**postSignin**方法：

    public function postSignin() {
          
	}

现在我们就来处理登录的验证。添加以下代码：

    if (Auth::attempt(array('username'=>Input::get('email'), 'password'=>Input::get('password')))) {
	   return Redirect::to('users/dashboard')->with('message', '欢迎您回来!');
	} else {
	   return Redirect::to('users/login')
	      ->with('message', '密码或者用户名错误')
	      ->withInput();
	}

在这里我们用到了laravel提供的**Author::attempt**方法。把用户名和密码作为参数传给它。这个方法返回**true**或者**false**值。所以我们使用**attempt**方法来验证用户输入的用户名和密码是否正确，如果正确则登陆成功重定向到**dashboard**页面。如果没有通过登录验证，则返回提示信息，提示用户重新输入登陆。

#### 创建Dashboard页面 ####

当然我们的web应用对于没有注册的用户或者是没有登录的用户，我们也需要展示一个页面。如果用户没有注册，我们需要提醒用户注册或者是登陆。

首先创建一个**getDashboard**的方法:

    public function getDashboard() {
    	
		return View::make('users.dashboard');
	}

然后在我们的构造函数添加一个过滤验证：

	public function __construct() {
	   $this->beforeFilter('csrf', array('on'=>'post'));
	   $this->beforeFilter('auth', array('only'=>array('getDashboard')));
	}

这个规则是检测用户是否登录，如果没有登录则重定向到登陆页面。注意我们这里用到了**only**这个参数，代表只提供对**getDashboard**这一行为提供验证。

默认的**auth**过滤系统，会重定向到*/login*这个路径下的页面，而我们的应用没有遵循这样的目录。所以我们需要修改下它的规则，在** app/filters.php **这个文件里，做如下修改：

    /*
	|--------------------------------------------------------------------------
	| Authentication Filters
	|--------------------------------------------------------------------------
	|
	| The following filters are used to verify that the user of the current
	| session is logged into this application. The "basic" filter easily
	| integrates HTTP Basic authentication for quick, simple checking.
	|
	*/
	 
	Route::filter('auth', function()
	{
	   if (Auth::guest()) return Redirect::guest('users/login');
	});

接下来是创建Dashboard的视图。

在**app/views/users **目录下创建一个*dashboard.blade.php*文件，添加如下的代码：

    <h1>Dashboard</h1>
 
	<p>Welcome to your Dashboard. You rock!</p>

这里只是简单定义了一些文字。

现在我们就可以正常登陆了，下面是我登陆后的页面。

![](http://pic.yupoo.com/reicky_v/DjGsqqnk/medium.jpg)

最后我们需要调整下我们的导航上面的文字。

你有没有注意到，我们现在无论是登陆还是没登录，导航栏上的文字永远是显示登录和注册的文字，这样从用户体验上来说并不是很友好。我们需要在用户已经登录的时候，显示退出登陆，不要再显示登录和注册。打开**master.blade.php**文件，把导航做如下调整：

    <div class="container">
      <div class="header">
          <ul class="nav nav-pills pull-right">
            <li>{{ HTML::link('users/register', '注册') }}</li>   
            <li>{{ HTML::link('users/login', '登录') }}</li>   
          </ul>
          <h3 class="text-muted">Read Later</h3>
      </div>
    </div>

我们这里用了一个*if..else..*的判断语句，并用*Auth::check()*方法来检测用户是否登录，从而根据不同的条件来显示导航上的文字。如下图所示：

![](http://pic.yupoo.com/reicky_v/DjGvYPJv/medium.jpg)

#### 退出登录 ####

有登录必然有退出登录，接下来就来创建退出登录的方法，仍然是在**UsersController**创建一个*getLogout*方法：

    public function getLogout() {
	   Auth::logout();
	   return Redirect::to('users/login')->with('message', '您已退出登录!');
	}

这个方法比较简单，laravel4中的验证系统提供了退出登陆的方法*Auth::logout*，退出登录我们重定向到登录页面，并且提示用户已经退出登陆。

通过本文我们学习了在laravel4中使用验证系统构建一个简单的登陆注册系统，当然还有很多可以完善的地方。

下篇文章就结合所学来做一个稍后阅读的应用。



    


