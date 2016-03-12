---
layout: post
title: laravel实战系列：使用laravel开发一个博客应用(1)
categories:
- Life
tags:
- laravel
- php
---

这篇文章我们使用laravel来开发一个博客应用程序，有点长可能要分几部分来写。

我们将要写的这个博客程序有以下几个特征并且会应用到前面所学的知识：

- 主要分为主页和日志详细页面
- 可以搜索发表的日志
- 可以针对单篇的日志发表评论
- 管理员可以对日志和评论进行增删查改的操作

ok，开始吧！

### 设置开发环境 ###

通过前面的几篇文章，对于搭建laravel开发环境应该说是轻车熟路了，首先我们需要在数据库中创建一个数据库，我们这里使用的是mysql，新建一个**blog**的数据库。并在**/app/config/database.php**文件中配置好数据库连接的相关信息，如下所示：

	'mysql' => array(
    'driver'    => 'mysql',
    'host'      => 'localhost',
    'database'  => 'blog',
    'username'  => 'root',
    'password'  => '',
    'charset'   => 'utf8',
    'collation' => 'utf8_unicode_ci',
    'prefix'    => '',
	),

### 使用迁移来创建数据库 ###

我们使用laravel提供的数据库迁移工具来创建程序需要的数据库。我们这个博客程序需要**posts**和**comments**两个表来存放用户的日志和评论，在终端中运行下面两个命令来生成数据库迁移文件：

	php artisan migrate:make create_posts_table --create=posts
	php artisan migrate:make create_comments_table --create=comments

首先打开生成的**posts**的迁移文件，编辑代码如下：

	<?php

	use Illuminate\Database\Schema\Blueprint;
	use Illuminate\Database\Migrations\Migration;

	class CreatePostsTable extends Migration {

	/**
	 * Run the migrations.
	 *
	 * @return void
	 */
	public function up()
	{
		Schema::create('posts', function(Blueprint $table)
		{
			$table->increments('id');
			$table->string('title');
            $table->string('read_more');
            $table->text('content');
            $table->unsignedInteger('comment_count');
			$table->timestamps();
			$table->engine = 'MyISAM';
		});
		DB::statement('ALTER TABLE posts ADD FULLTEXT search(title, content)');
	}

	/**
	 * Reverse the migrations.
	 *
	 * @return void
	 */
	public function down()
	{
		Schema::table('posts', function(Blueprint $table) {
            $table->dropIndex('search');
            $table->drop();
        });
	}

	}

上面的代码中使用**$table->engine = 'MyISAM';**指定MySQL中的存储引擎是**MyISAM**。并且在**posts**表中使用了[MySQL fulltext search](http://dev.mysql.com/doc/refman/5.0/en/fulltext-search.html)全文索引。

下面打开**comments**的迁移文件，编辑代码如下：

	<?php

	use Illuminate\Database\Schema\Blueprint;
	use Illuminate\Database\Migrations\Migration;
	
	class CreateCommentsTable extends Migration {

	/**
	 * Run the migrations.
	 *
	 * @return void
	 */
	public function up()
	{
		Schema::create('comments', function(Blueprint $table)
		{
			$table->increments('id');
			$table->unsignedInteger('post_id');
            $table->string('commenter');
            $table->string('email');
            $table->text('comment');
            $table->boolean('approved');
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
		Schema::drop('comments');
	}

	} 

**post_id**这个字段能帮助我们使用Eloquent ORM定义一对多的关系。我们使用**approved**来控制评论的范围，在验证的时候将覆盖**users**表。

### 使用Eloquent ORM来创建模型 ###

我们使用我们创建的表的名字的单数来作为模型的名字，这样它就能自动为我们对应数据库中相对应的表来进行操作，当然也可以指定表。比如模型的名字为**Post**就会执行对应的**posts**表。

在**app/models**目录中新建**Post.php**和**Comment.php**两个文件，编辑代码如下：

	<?php

	class Post extends Eloquent {
 
    public function comments()
    {
        return $this->hasMany('Comment');
    }
 
	}

	class Comment extends Eloquent {
	 
	    public function post()
	    {
	        return $this->belongsTo('Post');
	    }
	}

### 填充测试数据 ###

Laravel 还包含一个简单的方式通过填充类使用测试数据填充您的数据库。所有的填充类都存放在 app/database/seeds 目录下。我们这里新建一个**PostCommentSeeder**类来填充**posts**和**comments**两个表，代码如下：

	<?php
 
	class PostCommentSeeder extends Seeder {
 
    public function run()
    {
        $content = 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. 
                    Praesent vel ligula scelerisque, vehicula dui eu, fermentum velit. 
                    Phasellus ac ornare eros, quis malesuada augue. Nunc ac nibh at mauris dapibus fermentum. 
                    In in aliquet nisi, ut scelerisque arcu. Integer tempor, nunc ac lacinia cursus, 
                    mauris justo volutpat elit, 
                    eget accumsan nulla nisi ut nisi. Etiam non convallis ligula. Nulla urna augue, 
                    dignissim ac semper in, ornare ac mauris. Duis nec felis mauris.';
        for( $i = 1 ; $i <= 20 ; $i++ )
        {
            $post = new Post;
            $post->title = "Post no $i";
            $post->read_more = substr($content, 0, 120);
            $post->content = $content;
            $post->save();
 
            $maxComments = mt_rand(3,15);
            for( $j = 1 ; $j <= $maxComments; $j++)
            {
                $comment = new Comment;
                $comment->commenter = 'xyz';
                $comment->comment = substr($content, 0, 120);
                $comment->email = 'xyz@xmail.com';
                $comment->approved = 1;
                $post->comments()->save($comment);
                $post->increment('comment_count');
            }   
        }
    }
	}

在上面的代码中，我们使用了一个循环来创建了20篇日志，包括**title**、**read_more**、**content**这个三个字段的内容，而且每创建一篇日志评论的数量也是自增的。在终端中运行下面的命令来生成填充测试数据：

	php artisan db:seed

运行命令之后就会在数据库中对应的表中填充我们指定的测试数据。有关使用填充测试的信息可以去[这个地址](http://www.golaravel.com/docs/4.1/migrations/#database-seeding)看看详细的介绍。

### 使用artisan命令来测试 ###

下面我们来使用artisan命令来做一些有趣的事情。使用artisan命令可以非常容易来跟数据库互动。我们将使用**artisan tinker**命令在终端中来跟数据互动一下。首先在终端中运行下面命令进入命令环境：

	php artisan tinker

然后我们使用**find()**方法来根据id查询数据。

	>$post = Post::find(2);
	>$post->setHidden(['content','read_more','updated_at']);
	>echo $post;
	{"id":"2","title":"Post no 2","comment_count":"7","created_at":"2014-01-06 09:43:44"}

也可以使用**take()**和**skip()**方法来循环遍历获取指定的数据。

	>$post = Post::skip(5)->take(2)->get();
	>foreach($post as $value) echo "post id:$value->id ";
	post id:6 post id:7

使用select()和first()。

	>$post = Post::select('id','title')->first();
	>echo $post;
	{"id":"1","title":"Post no 1"}

使用where()和select()。

	>$post = Post::select('id','title')->where('id','=',10)->first();
	>echo $post;
	{"id":"10","title":"Post no 10"}

根据数据之间的关系来获取数据记录。

	>$post = Post::find(4);
	>echo $post->comments[0]->commenter;
	xyz

	>$comment = Comment::find(1);
	>echo $comment->post->title;
	Post no 1

关于Eloquent ORM可以去[这个地址](http://www.golaravel.com/docs/4.1/eloquent/)查看详细的介绍。

接下来是控制器部分的编写了。

### 控制器 ###

我们这里仍然使用**RESTful**风格的控制器来根据HTTP请求的不同调用不同的方法来处理数据。**RESTful**风格的控制器用起来确实非常爽。

> 有几点需要注意：使用**protected $layout**来设置视图的布局文件；使用**nest()**来嵌套视图；使用**with()**方法传递数据给视图文件。

### 创建BlogController控制器 ###

在**app/controllers**目录中创建**BlogController.php**文件，代码如下：

	<?php 

	class BlogController extends BaseController {

	public function __construct() {

		$this->beforeFilter('guest',['only' => ['getLogin']]);
		$this->beforeFilter('auth',['only' => ['getLogout']]);
	}

	public function getIndex()
	{
		$posts = Post::orderBy('id','desc')->paginate(10);
		$posts->getEnvironment()->setViewName('paginate::simple');
		$this->layout->title = '首页 | 我的博客';
		$this->layout->main = View::make('home')->nest('content','index',compact('posts'));
	}

	public function getSearch()
	{
		$searchTerm = Input::get('s');
		$posts = Post::whereRaw('match(title,content) against(? in boolean mode)',[$searchTerm])
			->paginate(10);
		$posts = getEnvironment()->setViewName('paginate::slider');
		$posts->appends(['s'=>$searchTerm]);
		$this->layout->with('title','Search:'.$searchTerm);
		$this->layout->main = View::make('home')
					->nest('content','index',($posts->isEmpty()) ? ['notFound' => true] : compact('posts'));
	}

	public function getLogin() {
		$this->layout->title='login';
        $this->layout->main = View::make('login');
	}

	public function postLogin()
    {
        $credentials = [
            'username'=>Input::get('username'),
            'password'=>Input::get('password')
        ];
        $rules = [
            'username' => 'required',
            'password'=>'required'
        ];
        $validator = Validator::make($credentials,$rules);
        if($validator->passes())
        {
            if(Auth::attempt($credentials))
                return Redirect::to('admin/dash-board');
            return Redirect::back()->withInput()->with('failure','username or password is invalid!');
        }
        else
        {
            return Redirect::back()->withErrors($validator)->withInput();
        }
    }
 
    public function getLogout()
    {
        Auth::logout();
        return Redirect::to('/');
    }
	}

下面来分析下代码。**getIndex()**方法来获取所有数据，并且使用**Paginate()**方法来对数据进行分页，每十条数据一页。

使用**getSearch()**方法来实现搜索功能。我们是通过**Post**模型来查询数据的。而**whereRaw()**方法则可以执行**sql**语句来查询数据，还记得我们在**posts**数据库中设置了全文检索么。而**getSearch()**方法中的**whereRaw()**方法中执行的sql语句中的Match函数就是用来全文检索的。具体就是在select的where字句中用match函数,索引的关键词用agianst标识，in boolean mod是只有含有关键字就行，不用在乎位置，是不是起始位置。我们还可以通过分页器的appends方法为分页链接添加上自定查询字符串。

后面三个方法是关于登录和登出功能，用到了laravel中提供的用户验证的功能，关于laravel中的用户验证这一块可以去这个[地址](http://www.golaravel.com/docs/4.1/security/)看看。

### PostController ###

新建一个**PostController.php**文件，编辑代码如下：

	<?php
 
	class PostController extends BaseController
	{
 
    /* 获取所有的文章 */
    public function listPost()
    {
        $posts = Post::orderBy('id','desc')->paginate(10);
        $this->layout->title = 'Post listings';
        $this->layout->main = View::make('dash')->nest('content','posts.list',compact('posts'));
    }

    // 显示单篇文章
    public function showPost(Post $post)
    {
        $comments = $post->comments()->where('approved', '=', 1)->get();
        $this->layout->title = $post->title;
        $this->layout->main = View::make('home')->nest('content', 'posts.single', compact('post', 'comments'));
    }

    // 创建新的日志
 
    public function newPost()
    {
        $this->layout->title = 'New Post';
        $this->layout->main = View::make('dash')->nest('content', 'posts.new');
    }
    
    // 编辑新的日志
    public function editPost(Post $post)
    {
        $this->layout->title = 'Edit Post';
        $this->layout->main = View::make('dash')->nest('content', 'posts.edit', compact('post'));
    }

    // 删除日志
 
    public function deletePost(Post $post)
    {
        $post->delete();
        return Redirect::route('post.list')->with('success', 'Post is deleted!');
    }
 
    /* 保存日志  */
    public function savePost()
    {
        $post = [
            'title' => Input::get('title'),
            'content' => Input::get('content'),
        ];
        $rules = [
            'title' => 'required',
            'content' => 'required',
        ];
        $valid = Validator::make($post, $rules);
        if ($valid->passes())
        {
            $post = new Post($post);
            $post->comment_count = 0;
            $post->read_more = (strlen($post->content) > 120) ? substr($post->content, 0, 120) : $post->content;
            $post->save();
            return Redirect::to('admin/dash-board')->with('success', 'Post is saved!');
        }
        else
            return Redirect::back()->withErrors($valid)->withInput();
    }
    
    // 更新日志
    public function updatePost(Post $post)
    {
        $data = [
            'title' => Input::get('title'),
            'content' => Input::get('content'),
        ];
        $rules = [
            'title' => 'required',
            'content' => 'required',
        ];
        $valid = Validator::make($data, $rules);
        if ($valid->passes())
        {
            $post->title = $data['title'];
            $post->content = $data['content'];
            $post->read_more = (strlen($post->content) > 120) ? substr($post->content, 0, 120) : $post->content;
            if(count($post->getDirty()) > 0) 
            {
                $post->save();
                return Redirect::back()->with('success', 'Post is updated!');
            }
            else
                return Redirect::back()->with('success','Nothing to update!');
        }
        else
            return Redirect::back()->withErrors($valid)->withInput();
    }
 
	}

> 模型绑定，为在路由中注入模型实例提供了便捷的途径。例如，你可以向路由中注入匹配用户ID的整个模型实例，而不是仅仅注入用户ID。首先，使用 Route::model 方法指定要被注入的模型。看下面这个例子：

	<?php
	//file: app/routes.php
	 
	Route::model('post','Post');
	 
	Route::get('post/{post}',function(Post $post)
	{
	    echo $post->title;
	 
	});

由于我们已经把**post**绑定到**Post**模型，现在，当我们用文章id作为参数访问这个路由，文章与ID的对应关系将被自动注入到路由，然后就可以访问到文章的属性了。

在**PostController**中，**showPost()**方法是显示单篇文章并且还会显示评论。我们是把日志和评论作为参数传递给视图(app/views/posts/single.blade.php)来显示日志和评论。视图部分稍后会来专门讲。

**listPost**方法是显示所有日志列表，这个可以用来给管理员来进行日志的管理即增删查改。

**newPost**方法是用来发表日志的。当然这里会使用到表单来收集用户输入的数据，发送数据后就是用**savePost**方法来保存数据。在**savePost()**方法中，我们先会通过验证才来保存数据否则就返回错误信息给用户。

### CommentsController ###

下面的代码是评论部分代码：

	<?php
 
	class CommentController extends BaseController {
 
    /* 显示评论 */
    public function listComment()
    {
        $comments = Comment::orderBy('id','desc')->paginate(20);
        $this->layout->title = 'Comment Listings';
        $this->layout->main = View::make('dash')->nest('content','comments.list',compact('comments'));
    }
 
    public function newComment(Post $post)
    {
        $comment = [
            'commenter' => Input::get('commenter'),
            'email' => Input::get('email'),
            'comment' => Input::get('comment'),
        ];
        $rules = [
            'commenter' => 'required',
            'email' => 'required | email',
            'comment' => 'required',
        ];
        $valid = Validator::make($comment, $rules);
        if($valid->passes())
        {
            $comment = new Comment($comment);
            $comment->approved = 'no';
            $post->comments()->save($comment);
            /* 调转到页面的评论部分 */
            return Redirect::to(URL::previous().'#reply')
                    ->with('success','Comment has been submitted and waiting for approval!');
        }
        else
        {
            return Redirect::to(URL::previous().'#reply')->withErrors($valid)->withInput();
        }
    }
 
    public function showComment(Comment $comment)
    {
        if(Request::ajax())
            return View::make('comments.show',compact('comment'));
        // 使用ajax方法来处理评论
        //else{}
    }
 
    public function deleteComment(Comment $comment)
    {
        $post = $comment->post;
        $status = $comment->approved;
        $comment->delete();
        ($status === 'yes') ? $post->decrement('comment_count') : '';
        return Redirect::back()->with('success','Comment deleted!');
    }
 
    /* 更新评论的显示权限 */
 
    public function updateComment(Comment $comment)
    {
        $comment->approved = Input::get('status');
        $comment->save();
        $comment->post->comment_count = Comment::where('post_id','=',$comment->post->id)
            ->where('approved','=',1)->count();
        $comment->post->save();
        return Redirect::back()->with('success','Comment '. (($comment->approved === 'yes') ? 'Approved' : 'Disapproved'));
    }
 
	}

在上面的代码中，**listComment**方法是用来显示评论的。当产生一个新的评论的时候，**newComment**方法会把评论保存到数据库中，当保存评论的时候**newComment**方法会默认把评论显示权限设置为'no'。当然管理员可以在后台控制评论显示与否的状态这里会用到**updateComment**方法来更新评论的状态，当然在更新评论显示状态的时候也会同时更新**comment_count**字段的值。**deleteComment**方法是用来删除评论的。

**showComment**方法是用来显示评论内容的，这里我们使用了Ajax方法来显示。具体会在后面讲到。

ok，先到这里，明天继续。

这个博客实战的文章主要是来自[这里](http://www.codeheaps.com/php-programming/creating-blog-using-laravel-4-part-1/)。
    








