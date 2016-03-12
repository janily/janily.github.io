---
layout: post
title: laravel中Eloquent初探
categories:
- Life
tags:
- laravel
- php
- 翻译
---

> 今天这篇文章来学习在laravel中的强大的Eloquent ORM，Laravel 自带的 Eloquent ORM 为您的数据库提供了一个优雅的、简单的 ActiveRecord 实现。每一个数据库的表有一个对应的 "Model" 用来与这张表交互。原文地址是[Laravel Eloquent ORM Tutorial](http://vegibit.com/laravel-eloquent-orm-tutorial/)。

Laravel 自带的 Eloquent ORM 为您的数据库提供了一个优雅的、简单的 ActiveRecord 实现。每一个数据库的表有一个对应的 "Model" 用来与这张表交互。如果你理解php中的对象，那你也会明白怎么去使用Eloquent。当然要理解Eloquent也并不是很简单，但是在laravel模型中使用Eloquent确实非常舒服，因为它提供的Eloquent的语法非常简单。使用Eloquent确实能大大的提高我们的开发体验。OK，开始吧！

### 数据表与Eloquent之间的关系

在laravel中，每一个数据库的表有一个对应的 "Model" 用来与这张表交互。

### 创建一个Eloquent Model

我们需要创建Painters和Paintings两张表，首先我们来创建Painer模型，在开始之前先来创建Painters这张表，我们这里使用了Laravel 4 Generators来进行相关操作，在终端中运行下面的命令：

`php artisan generate:migration create_painters_table`
`php artisan generate:migration create_paintings_table`

我们Migration文件夹中看到相关migration的代码，编辑如下：

	<?php
	//  painters table
 
	use Illuminate\Database\Schema\Blueprint;
	use Illuminate\Database\Migrations\Migration;
 
	class CreatePaintersTable extends Migration {
 
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('painters', function(Blueprint $table)
        {
            $table->increments('id');
            $table->string('username')->unique();
            $table->text('bio');
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
        Schema::drop('painters');
    }
 
	}
 
    <?php
	//  paintings table
 
	use Illuminate\Database\Schema\Blueprint;
	use Illuminate\Database\Migrations\Migration;
 
	class CreatePaintingsTable extends Migration {
 
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('paintings', function(Blueprint $table)
        {
            $table->increments('id');
            $table->string('title');
            $table->text('body');
            $table->integer('painter_id');
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
        Schema::drop('paintings');
    }
 
	}
	
然后运行**php artisan migrate**在数据库中建好相关的表。

现在就可以来创建模型来操作数据库了。下面先来给painters这个表来创建Painter模型：

	class Painter extends Eloquent {}
	
Eloquent Model创建好后，我们可以使用一行代码来看看Eloquent Model 为我们提供了哪些方法。

	Route::get('/', function()
	{
 
	$reflection = new ReflectionClass('Painter');  //  	inspect the methods and constants of any class!
 
	print_r($reflection->getMethods());
    
	});
	
运行上面的代码后，我们会看到会输出跟[官方文档](http://laravel.com/api/class-Illuminate.Database.Eloquent.Model.html)一样的164个方法。当然使用laravel为我们提供的DB类我们可以很方便进行crub操作。那如果使用Eloquent该怎么进行crub操作呢：

	Event::listen('illuminate.query',function($sql)){
		var_dump($sql);
	});
	
上面代码会输出相关数据库查询的操作的方法，对于在进行数据库操作的时候很有帮助。

### Create(插入)

我们可以使用**save()**方法来向数据库插入数据：

	Route::get('/',function(){
		$painter = new Painter;
		$painter->username = 'janily';
		$painter->bio = 'janily painter,scientist';
		$painter->save();
	});
	
> 这个跟使用 'insert into painter(username,bio,update_at,create_at) values(?,?,?,?)'(length=90)差不多。

### Retrieve(select)数据查询

我们可以使用**all()和first()**方法来查询数据：

	Route::get('/',function(){
		$painters = Painter::all()->first();
		
		echo $painter->username.'<br>';
		echo $painter->bio;
	});
	
> 相当于使用 'select * from painters'方法来查询数据。

### update 数据更新

一般数据库也就是增删查改这类型的操作，上面进行的是增查操作，下面我们来进行更新数据库的操作：

	Route::get('/',function(){
		$painters = Painter::all->first();
		$painters->username = 'janily chen';
		$painters->save();
	});
	
> 相当于使用 select * from painters update painters set username=?,update_at = ? where id = ?

### Delete 删除数据

使用eloquent来删除数据非常容易：

	Route::get('/',function(){
		$painter = Painter::all()->first();
		$painter->delete();
	});
	
> 相当于 select * from painter 
> delete from painter where id = ?'

就简单的两句代码就把数据删除掉了。

### Relationships in Eloquent 关系

当然，您的数据库可能是彼此相关的。比如，一篇博客文章可能有许多评论，或者一个订单与下订单的用户相关。Eloquent 使得管理和处理这些关系变得简单。

首先使用下面掉代码来增加写测试数据：

	Route::get('/', function()
	{
    $painter = new Painter;  
    $painter->username = 'Leonardo Da Vinci';
    $painter->bio = 'Renaissance painter, scientist, inventor, and more. Da Vinci is one of most famous painters for his iconic Mona Lisa and Last Supper.';
    $painter->save();    
 
    $painter = new Painter;  
    $painter->username = 'Vincent Van Gogh';
    $painter->bio = 'Dutch post-impressionist painter. Famous paintings include: Sunflowers, The Starry night, Cafe Terrace at Night.';
    $painter->save();    
 
    $painter = new Painter;  
    $painter->username = 'Rembrandt';
    $painter->bio = 'One of greatest painters, admired for his vivid realism. Famous paintings include The Jewish Bride, The Storm of the sea of Galilee';
    $painter->save();    
	});
	
现在我们新添加了几个Painter。现在需要给Painter插入一些数据也就是Painting。这里根据Painter掉id来插入数据，我们这里需要知道掉是四个Painter的ID，分别是1、2、3、4。我们需要使用ID把Painter和painting关联起来。使用它们之间的关系就可以很方便来查询数据。

例如：

	Route::get('/',funtion(){
		$painter = Painter::find(4);
		$painting = new Paingting;
		$painting->title = 'the storm on the sea fo galilee';
		$painting->body = 'the storm on the sea of body';
		$painting->painter_id = $painter->id;
		$painting->save();
		
	});
	
我们需要执行上面的代码来为每一个painter插入至少两条数据。

### hasMany

每一个painter会有多个painting，就像一个作者会写很多的书一样，或者是一只鸡会生很多的鸡蛋。在laravel中，我们可以中模型中来定义数据库之间的关系：

	<?php 
	class Painter extends Eloquent {
		
		public funtion paintings()
		{
			return $this->hasMany('Painting');
		}
	}
	
在上面的代码中，$this关键字指向模型的名字。所以$this->hasMany('Painting');可以理解为**this Painter hasMany Painting**。

### belongsTo

反过来，则所有的painting都必须属于一个painter。在模型中这样来表示：

	<?php 
	class Painting extends Eloquent {
		public funtion painter ()
		{
			return $this->belongsTo('Painter');
		}
	}
	
### 查询数据

上面我们使用了**hasMany**和**belongsTo**方法定义好了数据之间的关系，然后就可以来查询数据了。比如我们需要查询Da Vincin用户的painting并且把结果返回给我们。就可以使用下面的代码来实现：

	Route::get('/',function(){
		$painter = Painter::whereUsername('Leonardo Da Vinci')->first();
		foreach($painter->paintings as $painting){
			echo $painting->title.'<br>';
			echo $painting->body.'<br><br>';
		}
	});
	
> 相当于sql中的 select * from painters where username = ? limit 1
> select * from paintings where paintings.painter_id = ?

运行之后就会输出查询结果。

你可能会注意到上面到**whereUsername**方法。在laravel中我们可以把**where**方法和我们需要中表中查询到字段合并起来。在Painter模型中我们定义了**This Painter hasMany Painting**这层关系。

现在让我们来做点有趣点东东。比如我们可以反过来想，我们可能会想知道Potato Eaters是哪位Painter的作品。在Painting模型中我们定义来**this Painting belongsTo Painter**这层关系，所以我们可以反过来查询：

	Route::get('/',function(){
	
		$painting = Painting::whereTitle('The Potato Eaters')->first();
		echo Painter::find($painting->painter_id)->username;
	});
	
> 相当于 select * from paintings where title = ? limit 1
> select * from painters where id = ? limit 1

运行上面的代码，我们就可以查询到相关到数据。从上面可以看出，当我们定义好了数据之间当关系的时候，我们可以通过模型查询任何字段。当我们在**Painting**模型中创建好一个**Painting**的实例**$painting**，我们可以通过**Painter**模型来查询表中的任何字段。怎么做呢？

	Route::get('/',function(){
		$painting = Painting::whereTitle('The Potato Eaters')->first();
		
		echo $painting->painter->username. '<br>';
		echo $painting->painter->bio;
	});

> 相当于 select * from paintings where title = ? limit 1
> select * from painters where painters.id = ? limit 1

这里我们需要注意的是，我们创建了一个Painting模型的一个实例，我们就可以通过它来访问painter表中的字段。

通过上面可以发现，我们可以查询任何我们想要的数据。下面我们就来实战一下，我们来取出所有的painting以及对应的painter。

	Route::get('/',function(){
		$paintings = Painting::all();
		
		foreach($paintings as $painting) {
			echo $painting->painter->username;
			echo 'painted the';
			echo $painting->title;
		}
	});
	
但是这里相当于执行来下面这些sql语句：

	string ‘select * from paintings‘ (length=25)
	string ‘select * from painters where painters.id = ? limit 1′ (length=58)
	Leonardo Da Vinci painted the Mona Lisa string ‘select * from painters where painters.id = ? 	limit 1′ (length=58)
	Leonardo Da Vinci painted the Last Supper
	string ‘select * from painters where painters.id = ? limit 1′ (length=58)
	Vincent Van Gogh painted the The Starry Night
	string ‘select * from painters where painters.id = ? limit 1′ (length=58)
	Vincent Van Gogh painted the The Potato Eaters
	string ‘select * from painters where painters.id = ? limit 1′ (length=58)
	Rembrandt painted the The Night Watch
	string ‘select * from painters where painters.id = ? limit 1′ (length=58)
	Rembrandt painted the The Storm on the Sea of Galilee

执行这么多的sql语句总归不是一个好的方法，性能上看更是如此。不过我们可以使用预先加载来减少查询的数量，laravel给我们提供来一个**with()**方法来执行这样的操作：

	Route::get('/',function(){
		$paintings = Painting::with('painter')->get();
		
		foreach($paintings as $painting) 
		{
			echo $painting->painter->username;
			echo 'paited the':
			echo $painting->title;
			echo '<br>';
		}
	});
	
使用with()方法后，只相当于执行来两条sql语句大大提高了性能：

	string ‘select * from paintings‘ (length=25)
	string ‘select * from painters where painters.id in (?, ?, ?)’ (length=59)
	
laravel中的Eloquent Model的知识一些基本的原理和应用就介绍到这里了。关于laravel到知识还有很多，后面接着聊。











	

	







	

	













