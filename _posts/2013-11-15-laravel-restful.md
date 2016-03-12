---
layout: post
title: larvavel4开发web app(1)：RESTful风格API学习
categories:
- Life
tags:
- php
- 翻译
---

> 最近工作之余一直在学习PHP开发的知识，想自己开发一个web app应用，设计差不多设计完了，接下来是进入编码阶段。决定采用laravel来作为开发框架，并且采用RESTful风格API的开发方式来开发。laravel当然也支持RESTful风格API的开发，现在把学习laravel过程中的一些历程记录记下来。至于基本的laravel的知识可以去官方文档看看，这里有[中文地址](http://www.golaravel.com/docs/)。当然最好是去英文的官方网站看最新的一手技术文档。首先来学习下laravel4中怎么来构建RESTful风格的API。这篇文章来自[tutsplus](http://net.tutsplus.com/)网站的[Laravel 4: A Start at a RESTful API](http://net.tutsplus.com/tutorials/php/laravel-4-a-start-at-a-restful-api/)。

至于RESTful风格API我前面已经有一篇关于这方面的文章，可以去看看[这里](http://dictmoom.f2e.me/2013/10/restful/)。这里就不再阐述。

### App ###

我们构建一个稍后阅读的应用，这个应用的包括的一些操作就是，创建，阅读，更新和删除。

那么就开始吧！

#### 安装Laravel 4 ####

现在PHP中开发一般都用composer来作为包管理器，同样laravel也可以用composer来安装，至于怎么安装这里就不阐述了，可以去这下面的两个地址看一看，[ install of Laravel 4](http://four.laravel.com/#install-laravel)和[quickstart guide](http://fideloper.com/laravel-4-uber-quick-start-with-auth-guide?utm_source=nettuts&utm_medium=article&utm_content=api&utm_campaign=guest_author)。

装完后，需要生成一个安全加密的key。在命令行里很容易就可以做到，如下：

    $ php artisan key:generate

当然，你也可以在**app/config/app.php**来手动添加：

    /*
	|--------------------------------------------------------------------------
	| Encryption Key
	|--------------------------------------------------------------------------
	|
	| This key is used by the Illuminate encrypter service and should be set
	| to a random, long string, otherwise these encrypted values will not
	| be safe. Make sure to change it before deploying any application!
	|
	*/
	 
	'key' => md5('this is one way to get an encryption key set'),

### Database ###

接下来就是规划和创建数据库。

主要是包含两个表：

- Users,存放用户名和密码
- URLs,存放链接地址和描述

我们会用到Laravel4中的[migrations](http://four.laravel.com/docs/migrations)来创建和管理我们的数据库。

### 设置数据库 ###

打开**app/config/database.php **这个文件，设置好你的数据库。我们这里用到的数据库程序是MySQL。

    'connections' => array(
 
    'mysql' => array(
        'driver'    => 'mysql',
        'host'      => 'localhost',
        'database'  => 'read_it_later',
        'username'  => 'your_username',
        'password'  => 'your_password',
        'charset'   => 'utf8',
        'collation' => 'utf8_unicode_ci',
        'prefix'    => '',
    ),
	),

### Create Migration Files ###

    $ php artisan migrate:make create_users_table --table=users --create
	$ php artisan migrate:make create_urls_table --table=urls --create

运行上面两个命令，会创建migration的文件用于创建数据库。

首先打开**app/database/migrations/SOME_DATE_create_users_table.php**这个文件，在up这个函数里面添加我们要创建数据库的字段。

    public function up()
	{
	    Schema::create('users', function(Blueprint $table)
	    {
	        $table->increments('id');
	        $table->string('username')->unique();
	        $table->string('password');
	        $table->timestamps();
	    });
	}

在上面的方法里面，我们创建了四个字段，分别是username,password,id和时间戳。接下来编辑**app/database/migrations/SOME_DATE_create_urls_table.php**文件，在up方法里面添加下面代码：

    public function up()
	{
	    Schema::create('urls', function(Blueprint $table)
	    {
	        $table->increments('id');
	        $table->integer('user_id');
	        $table->string('url');
	        $table->string('description');
	        $table->timestamps();
	    });
	}

这里需要注意一点的是，我们创建了一个user_id字段，这样可以把url和users两个表关联起来。

### 创建一些示例数据 ###

我们可以用laravel提供的[seeds](http://four.laravel.com/docs/migrations#database-seeding)来创建一些示例用的数据。

在**app/database/seeds **这个目录创建一个**UserTableSeeder.php**的文件，添加如下代码：

    <?php
 
		class UserTableSeeder extends Seeder {
		 
		    public function run()
		    {
		        DB::table('users')->delete();
		 
		        User::create(array(
		            'username' => 'firstuser',
		            'password' => Hash::make('first_password')
		        ));
		 
		        User::create(array(
		            'username' => 'seconduser',
		            'password' => Hash::make('second_password')
		        ));
		    }
		 
		}

接下来，我们要在**app/database/seeds/DatabaseSeeder.php**这个文件里，添加下面代码，这样在运行seeder的时候，能够运行我们创建的seeder文件从而创建示例数据。

    public function run()
	{
	    Eloquent::unguard();
	 
	    // Add or Uncomment this line
	    $this->call('UserTableSeeder');
	}

然后在命令行里面运行下面的命令：

    // Create the two tables
	$ php artisan migrate
	 
	// Create the sample users
	$ php artisan db:seed

### 模型(Models) ###

Laravel 自带的 Eloquent ORM 为您的数据库提供了一个优雅的、简单的 ActiveRecord 实现。每一个数据库的表有一个对应的 "Model" 用来与这张表交互。

创建和编辑文件**app/models/Url.php**

    <?php
 
	class Url extends Eloquent {
	 
	    protected $table = 'urls';
	 
	}

### 身份验证(Authentication) ###

Laravel的[filters](http://four.laravel.com/docs/routing#route-filters)为我们提供了很好用的身份验证系统，我们能够使用它作为一个验证模型和API请求交互。

打开**app/filters.php**文件，我们可以看到这样的代码：

    Route::filter('auth.basic', function()
	{
	    return Auth::basic();
	});

我们可能要调整下验证规则，默认的验证规则是以*email*来验证的，我们这里是以*user*来验证的，我们只要改一下**Author::base**里的参数就可以了，如下：

	Route::filter('auth.basic', function()
	{
	    return Auth::basic("username");
	});

### 路由(Routes) ###

让我们创建一个路由*testauth*来测试下我们的验证系统。

编辑app/routes.php文件:

    Route::get('/authtest', array('before' => 'auth.basic', function()
	{
	    return View::make('hello');
	}));

我们可以用curl工具来测试我们的api。在命令行中敲入以下命令来测试我们的验证系统：

    $ curl -i localhost/l4api/public/index.php/authtest
	HTTP/1.1 401 Unauthorized
	Date: Tue, 21 May 2013 18:47:59 GMT
	WWW-Authenticate: Basic
	Vary: Accept-Encoding
	Content-Type: text/html; charset=UTF-8
	 
	Invalid credentials

我们可以看到在命令行里看到输出的信息是验证无效，下面我们再用用户名和密码来验证下：

    $ curl --user firstuser:first_password localhost/l4api/public/index.php/authtest
	HTTP/1.1 200 OK
	Date: Tue, 21 May 2013 18:50:51 GMT
	Vary: Accept-Encoding
	Content-Type: text/html; charset=UTF-8
	 
	<h1>Hello World!</h1>

可以看到通过了验证。

到此，我们完成了一个基本的搭建工作。

- 安装laravel 4
- 创建一个数据库
- 创建模型
- 创建一个验证系统

### 创建响应请求的方法(Creating Functional Requests) ###

在Laravel有提供[ RESTful controllers](http://four.laravel.com/docs/controllers#restful-controllers)。当然我们也可用[ Resourceful Controllers](http://four.laravel.com/docs/controllers#resource-controllers)，我们可以基于它创建我们的API。

#### 创建Resourceful Controller ####

在命令行中敲入以下命令来生成资源控制器:

    php artisan controller:make UrlController

编辑**app/routes.php**这个文件

    // Route group for API versioning
	Route::group(array('prefix' => 'api/v1', 'before' => 'auth.basic'), function()
	{
	    Route::resource('url', 'UrlController');
	});

上面路由定义以下的一些内容：

- 会响应这个url*http://example.com/api/v1/url*
- 我们还可以扩展我们的url,比如*/api/v1/user*
- 我们这里还做到了对API的版本控制。这样我们就可以很方便的对我们的API进行升级，比如我们可以创建一个**v2**的路由组，来升级我们的API。

编辑app/controllers/UrlController.php这个文件：

    public function index()
	{
	    return 'Hello, API';
	}
现在我们的resourceful的控制器就可以正常的工作了。

### 创建url方法 ###

编辑*app/controllers/UrlController.php*文件

    /**
	 * 这个方法是用存储新添加url的
	 *
	 * @return Response
	 */
		public function store()
		{
		    $url = new Url;
		    $url->url = Request::get('url');
		    $url->description = Request::get('description');
		    $url->user_id = Auth::user()->id;
		 
		    // Validation and Filtering is sorely needed!!
		    // Seriously, I'm a bad person for leaving that out.
		 
		    $url->save();
		 
		    return Response::json(array(
		        'error' => false,
		        'urls' => $urls->toArray()),
		        200
		    );
		}

照例，我们用curl来测试我们存放url的api即*store()*方法。

    $ curl -i --user firstuser:first_password -d 'url=http://google.com&description=A Search Engine' localhost/l4api/public/index.php/api/v1/url
	HTTP/1.1 201 Created
	Date: Tue, 21 May 2013 19:10:52 GMT
	Content-Type: application/json
	 
	{"error":false,"message":"URL created"}

我们来多创建几条数据：

    $ curl --user firstuser:first_password -d 'url=http://fideloper.com&description=A Great Blog' localhost/l4api/public/index.php/api/v1/url
 
	$ curl --user seconduser:second_password -d 'url=http://digitalsurgeons.com&description=A Marketing Agency' localhost/l4api/public/index.php/api/v1/url
	 
	$ curl --user seconduser:second_password -d 'url=http://www.poppstrong.com/&description=I feel for him' localhost/l4api/public/index.php/api/v1/url

接下来创建显示URLs列表的方法

    /**
	 * 显示数据库中所有url.
	 *
	 * @return Response
	 */
	public function index()
	{
	    //Formerly: return 'Hello, API';
	 
	    $urls = Url::where('user_id', Auth::user()->id)->get();
	 
	    return Response::json(array(
	        'error' => false,
	        'urls' => $urls->toArray()),
	        200
	    );
	}
	 
	/**
	 * 显示特定的用户的url列表
	 *
	 * @param  int  $id
	 * @return Response
	 */
	public function show($id)
	{
	    // Make sure current user owns the requested resource
	    $url = Url::where('user_id', Auth::user()->id)
	            ->where('id', $id)
	            ->take(1)
	            ->get();
	 
	    return Response::json(array(
	        'error' => false,
	        'urls' => $url->toArray()),
	        200
	    );
	}

我们可以测试下：

    $ curl --user firstuser:first_password localhost/l4api/public/index.php/api/v1/url
	{
	    "error": false,
	    "urls": [
	       {
	            "created_at": "2013-02-01 02:39:10",
	            "description": "A Search Engine",
	            "id": "2",
	            "updated_at": "2013-02-01 02:39:10",
	            "url": "http://google.com",
	            "user_id": "1"
	        },
	        {
	            "created_at": "2013-02-01 02:44:34",
	            "description": "A Great Blog",
	            "id": "3",
	            "updated_at": "2013-02-01 02:44:34",
	            "url": "http://fideloper.com",
	            "user_id": "1"
	        }
	    ]
	}
	 
	$ curl --user firstuser:first_password localhost/l4api/public/index.php/api/v1/url/1
	{
	    "error": false,
	    "urls": [
	        {
	            "created_at": "2013-02-01 02:39:10",
	            "description": "A Search Engine",
	            "id": "2",
	            "updated_at": "2013-02-01 02:39:10",
	            "url": "http://google.com",
	            "user_id": "1"
	        }
	    ]
	}

接下来是用户删除url。

    /**
	 * 删除用户指定的url.
	 *
	 * @param  int  $id
	 * @return Response
	 */
	public function destroy($id)
	{
	    $url = Url::where('user_id', Auth::user()->id)->find($id);
	 
	    $url->delete();
	 
	    return Response::json(array(
	        'error' => false,
	        'message' => 'url deleted'),
	        200
	        );
	}

还是来测试下：

    $ curl -i -X DELETE --user firstuser:first_password localhost/l4api/public/index.php/api/v1/url/1
	HTTP/1.1 200 OK
	Date: Tue, 21 May 2013 19:24:19 GMT
	Content-Type: application/json
	 
	{"error":false,"message":"url deleted"}

最后一个方法是用户更新url。

    /**
	 * Update the specified resource in storage.
	 *
	 * @param  int  $id
	 * @return Response
	 */
	public function update($id)
	{
	    $url = Url::where('user_id', Auth::user()->id)->find($id);
	 
	    if ( Request::get('url') )
	    {
	        $url->url = Request::get('url');
	    }
	 
	    if ( Request::get('description') )
	    {
	        $url->description = Request::get('description');
	    }
	 
	    $url->save();
	 
	    return Response::json(array(
	        'error' => false,
	        'message' => 'url updated'),
	        200
	    );
	}

照例测试，运行以下命令：

    $ curl -i -X PUT --user seconduser:second_password -d 'url=http://yahoo.com' localhost/l4api/public/index.php/api/v1/url/4
	HTTP/1.1 200 OK
	Date: Tue, 21 May 2013 19:34:21 GMT
	Content-Type: application/json
	 
	{"error":false,"message":"url updated"}
	 
	// View our changes
	$ curl --user seconduser:second_password localhost/l4api/public/index.php/api/v1/url/4
	{
	    "error": false,
	    "urls": [
	        {
	            "created_at": "2013-02-01 02:44:34",
	            "description": "I feel for him",
	            "id": "3",
	            "updated_at": "2013-02-02 18:44:18",
	            "url": "http://yahoo.com",
	            "user_id": "1"
	        }
	    ]
	}

这样一个简单restful的API就完成了，用laravel4非常容易打造一个restful风格的API。

我们来回顾下我们学到的东西：

- 安装Laravel
- 用migrations创建数据库和用seeding创建示例数据
- 使用Eloquent ORM来操作数据库
- 使用laravel提供的Auth来验证
- 设置路由和API
- 使用Resourceful Controllers来编写API

后端的API写好了，下一步我们进一步学习laravel中的用户验证系统。