---
layout: post
title: laravel中Eloquent初探(2)
categories:
- Life
tags:
- laravel
- php
- 翻译
---

> 前面一篇文章初步了解了在laravel中eloquent的知识和使用方法。今天这篇继续来学习laravel中eloquent中的定义数据库关系这方面的知识，原文地址[A Guide to Using Eloquent ORM in Laravel](http://scotch.io/tutorials/php/a-guide-to-using-eloquent-orm-in-laravel)。

开始之前可以先下载这个[源代码](https://github.com/scotch-io/laravel-eloquent-guide)。

laravel提供的Eloquent ORM能使我们非常容易的跟数据库来交互。这篇文章我们主要学习以下的知识：

- 基本的CRUD的操作
- 设置数据之间一对一的关系
- 设置数据之间一对多的关系
- 设置数据之间多对多的关系

Laravel自带的Eloquent ORM为您的数据库提供了一个优雅的、简单的ActiveRecord实现。每一个数据库的表有一个对应的 "Model" 用来与这张表交互。

比如我们这里有一个**Bear**模型就会对应数据库中的**bears**的一张表。当我们创建模型和数据表的时候，我们就可以在模型里面来操作数据库了。

如，我们要在**Bear**模型里面获得所有的**bears**，我们可以在模型里面使用**Bear::all()**。使用这个方法将会生成对应的sql语句来查询数据库。下面我们再来看一些例子：

- 获取所有的bears使用**Bear:all()**
- 获取一个记录使用**Bear::find(id)**
- 删除一个记录**Bear::delete(id)**

非常简单吧，接下来我们会继续深入的了解它们的使用方法。

### 简单的实例 ###

我们先创建一个关于**bears**的简单应用程序。这个程序主要是包括不同类型的熊比如体重和危险系数等来区分。

这里还有一些**fish**，每一只鱼属于一只熊因为熊不喜欢分享它们的食物。即一对一的关系。

熊喜欢爬树(trees)，每一只熊喜欢爬不同种类的树，即一对多的关系。

这里还有一些**picnics**。和熊是多对多的关系。

![](http://scotch.io/wp-content/uploads/2014/03/eloquent-bears.jpg)

### laravel设置 ###

开始之前，我们需要设置我们的数据库。通过下面几个步骤来设置laravel:

- 下载代码后，运行**composer install --prefer-dist**
- 在**app/config/database.php**文件中设置我们的数据库
- 使用artisan命令来创建migration
- 创建Eloquent models
- 使用Seed来创建一些测试数据

前面两个步骤是开发程序必需的基本的设置。后面是使用migration和seeding来创建我们需要的测试的数据。

### 创建Migrations ###

在laravel中一般使用migrations来管理和创建数据，这里我们需要三张表：**bears**、**fish**和**picnics**。更多的关于migrations的知识可以[laravel官方的文档](http://laravel.com/docs/migrations)看看。

### bear migration ###

首先通过下面的命令来创建migration：

    php artisan migration:make create_bears_table --create=bears

然后进入对应的文件

    // app/database/migrations/####_##_##_######_create_bears_table.php
	...

	Schema::create('bears', function(Blueprint $table)
	{
		$table->increments('id');
	
		$table->string('name');
		$table->string('type');
		$table->integer('danger_level'); // this will be between 1-10
	
		$table->timestamps();
	});
	
	...

默认情况下，它将会自动帮我们创建一个自增的id字段以及两个时间戳分别是**create_at**和**update_at**。当数据有新的变化时，update_at字段会自动更新。

### Fish Migration ###

同样运行下面的命令：

    php artisan migration:make create_fish_table --create=fish

	// app/database/migrations/####_##_##_######_create_fish_table.php
	...
	
	Schema::create('fish', function(Blueprint $table)
	{
		$table->increments('id');
	
		$table->integer('weight'); // we'll use this to demonstrate searching by weight
		$table->integer('bear_id'); // this will contain our foreign key to the bears table
	
		$table->timestamps();
	});

> 我们上面使用了bears和picnics来作为表名，而没有使用fishes来作为fish的表名。而laravel默认会把类名的小写、复数的形式将作为表名，除非它被显式地指定。所以，在这种情况下，Eloquent 将假设 bears 模型在 bears 表中保存记录。当然您可以在模型中定义一个table属性来指定一个自定义的表名。

### Tree Migration ###

	php artisan migration:make create_trees_table --create=trees

    // app/database/migrations/####_##_##_######_create_trees_table.php
	...
	
	Schema::create('trees', function(Blueprint $table)
	{
		$table->increments('id');
	
		$table->string('type');
		$table->integer('age'); // how old is the tree
		$table->integer('bear_id'); // which bear climbs this tree
	
		$table->timestamps();
	});

### Picnic Migration ###

    php artisan migrate:make create_picnics_table --create=picnics

	// app/database/migrations/####_##_##_######_create_picnics_table.php
	...
	
	Schema::create('picnics', function(Blueprint $table)
	{
		$table->increments('id');
	
		$table->string('name');
		$table->integer('taste_level'); // how tasty is this picnic?
	
		$table->timestamps();
	});

现在需要把bears和picnic关联起来。我们可以通过创建一个中间表来实现这个目的。通过它来实现多对多的关系：

    php artisan migrate:make create_bears_picnics_table --create=bears_picnics

	// app/database/migrations/####_##_##_######_create_bears_picnics_table.php
	...
	
	Schema::create('bears_picnics', function(Blueprint $table)
	{
		$table->increments('id');
	
		$table->integer('bear_id'); // the id of the bear
		$table->integer('picnic_id'); // the id of the picnic that this bear is at
	
		$table->timestamps();
	});

通过这张表我们就可以来把bears和picnic关联起来。

### 迁移数据 ###

数据表的迁移文件准备好后，我们就可以使用artisan的命令来创建数据库了:

    php artisan migrate

![](http://pic.yupoo.com/reicky_v/DHn4cvf1/966Bk.jpg)

### Eloquent Models ###

创建好数据库后，我们需要一些添加一些数据进去。添加数据需要依靠**Eloquent**来实现。所以在添加数据前先创建好相关的模型。

下面来创建Eloquent models。数据库之间的关系也在这里来定义。

### Bear Model ###

首先来看看**Bear**模型。

    // app/models/Bear.php
	<?php
	
	class Bear extends Eloquent {
		
		// MASS ASSIGNMENT -------------------------------------------------------
		// define which attributes are mass assignable (for security)
		// we only want these 3 attributes able to be filled
		protected $fillable = array('name', 'type', 'danger_level');
	
		// DEFINE RELATIONSHIPS --------------------------------------------------
		// each bear HAS one fish to eat
		public function fish() {
			return $this->hasOne('Fish'); // this matches the Eloquent model
		}
	
		// each bear climbs many trees
		public function trees() {
			return $this->hasMany('Tree');
		}
	
		// each bear BELONGS to many picnic
		// define our pivot table also
		public function picnics() {
			return $this->belongsToMany('Picnic', 'bears_picnics', 'bear_id', 'picnic_id');
		}
	
	}

**集体赋值**：我们需要通过[集体赋值属性](http://laravel.com/docs/eloquent#mass-assignment)来指定哪些属性可以被集体赋值。这可以在类或接口层设置。

**定义关系**：在laravel中定义数据库之间的关系非常简单。比如上面代码中**return $this->hasOne('Fish')**就定义了Fish与Bear之间的关系。

在laravel中我们可以通过不同的方法来定义数据库之间的关系如：**hasOne**、**hasMany**、**belongsTo**、**BelongsToMany**等方法来定义。可以去[官方的文档](http://laravel.com/docs/eloquent#relationships)看看详细的信息。

在laravel中，**Eloquent model和数据库表将按默认的约定来使用表**，即类名的小写、复数的形式将作为表名，除非它被显式地指定。所以，在这种情况下，Eloquent将假设Bear模型在bears表中保存记录。当然你可以在模型中定义一个table属性来指定一个自定义的表名。

### Fish Model ###

下面是Fish模型的代码。

    // app/models/Fish.php
	<?php
	
	class Fish extends Eloquent {
		
		// MASS ASSIGNMENT -------------------------------------------------------
		// define which attributes are mass assignable (for security)
		// we only want these 3 attributes able to be filled
		protected $fillable = array('weight', 'bear_id');
	
		// LINK THIS MODEL TO OUR DATABASE TABLE ---------------------------------
		// since the plural of fish isnt what we named our database table we have to define it
		protected $table = 'fish';
	
		// DEFINE RELATIONSHIPS --------------------------------------------------
		public function bear() {
			return $this->belongsTo('Bear');
		}
	
	}

正如上面说到的，我们在模型中使用了**protected $table**自定义了表名。

正如我们在**Bear**模型中定义的关系一样，在Fish模型中我们使用了**belongsTo()**来定义它跟bear之间的关系。

### Tree Model ###

    // app/models/Tree.php
	<?php
	
	class Tree extends Eloquent {
		
		// MASS ASSIGNMENT -------------------------------------------------------
		// define which attributes are mass assignable (for security)
		// we only want these 3 attributes able to be filled
		protected $fillable = array('type', 'age', 'bear_id');
	
		// DEFINE RELATIONSHIPS --------------------------------------------------
		public function bear() {
			return $this->belongsTo('Bear');
		}
	
	}

### Picnic Model ###

依葫芦画瓢：

    // app/models/Picnic.php
	<?php
	
	class Picnic extends Eloquent {
		
		// MASS ASSIGNMENT -------------------------------------------------------
		// define which attributes are mass assignable (for security)
		// we only want these 3 attributes able to be filled
		protected $fillable = array('name', 'taste_level');
	
		// DEFINE RELATIONSHIPS --------------------------------------------------
		// define a many to many relationship
		// also call the linking table
		public function bears() {
			return $this->belongsToMany('Bear', 'bears_picnics', 'picnic_id', 'bear_id');
		}
	
	}

就像其它模型一样，我们定义了需要集体赋值的属性以及数据库之间的关系。我们使用了**belongsToMany()**定义了多对多的关系而不是**hasMany()**来定义。**hasMany()**使用来定义一对多关系的。

OK，现在数据库和模型方面的代码就编写完了，接下来我们将使用Eloquent来填充测试数据。

### 填充测试数据 ###

laravel为我们提供了[seeding](http://laravel.com/docs/migrations#database-seeding)这个工具用来填充程序所需要的测试数据。对于我们测试程序非常有用。

我们需要在app/database/seeds/DatabaseSeeder.php这个文件里来编写我们的测试数据。

一般来说我们会把测试数据写在一个单独的文件，不过这里我们把测试数据全部写入一个文件中。

	// app/database/seeds/DatabaseSeeder.php
	<?php
	
	class DatabaseSeeder extends Seeder {
	
		/**
		 * Run the database seeds.
		 *
		 * @return void
		 */
		public function run()
		{
			Eloquent::unguard();
	
			// call our class and run our seeds
			$this->call('BearAppSeeder');
			$this->command->info('Bear app seeds finished.'); // show information in the command line after everything is run
		}
	
	}
	
	// our own seeder class
	// usually this would be its own file
	class BearAppSeeder extends Seeder {
	
	public function run() {

		// clear our database ------------------------------------------
		DB::table('bears')->delete();
		DB::table('fish')->delete();
		DB::table('picnics')->delete();
		DB::table('trees')->delete();
		DB::table('bears_picnics')->delete();

		// seed our bears table -----------------------
		// we'll create three different bears

		// bear 1 is named Lawly. She is extremely dangerous. Especially when hungry.
		$bearLawly = Bear::create(array(
			'name'         => 'Lawly',
			'type'         => 'Grizzly',
			'danger_level' => 8
		));

		// bear 2 is named Cerms. He has a loud growl but is pretty much harmless.
		$bearCerms = Bear::create(array(
			'name'         => 'Cerms',
			'type'         => 'Black',
			'danger_level' => 4
		));

		// bear 3 is named Adobot. He is a polar bear. He drinks vodka.
		$bearAdobot = Bear::create(array(
			'name'         => 'Adobot',
			'type'         => 'Polar',
			'danger_level' => 3
		));

		$this->command->info('The bears are alive!');

		// seed our fish table ------------------------
		// our fish wont have names... because theyre going to be eaten

		// we will use the variables we used to create the bears to get their id

		Fish::create(array(
			'weight'  => 5,
			'bear_id' => $bearLawly->id
		));
		Fish::create(array(
			'weight'  => 12,
			'bear_id' => $bearCerms->id
		));
		Fish::create(array(
			'weight'  => 4,
			'bear_id' => $bearAdobot->id
		));
		
		$this->command->info('They are eating fish!');

		// seed our trees table ---------------------
		Tree::create(array(
			'type'    => 'Redwood',
			'age'     => 500,
			'bear_id' => $bearLawly->id
		));
		Tree::create(array(
			'type'    => 'Oak',
			'age'     => 400,
			'bear_id' => $bearLawly->id
		));

		$this->command->info('Climb bears! Be free!');

		// seed our picnics table ---------------------

		// we will create one picnic and apply all bears to this one picnic
		$picnicYellowstone = Picnic::create(array(
			'name'        => 'Yellowstone',
			'taste_level' => 6
		));
		$picnicGrandCanyon = Picnic::create(array(
			'name'        => 'Grand Canyon',
			'taste_level' => 5
		));
		
		// link our bears to picnics ---------------------
		// for our purposes we'll just add all bears to both picnics for our many to many relationship
		$bearLawly->picnics()->attach($picnicYellowstone->id);
		$bearLawly->picnics()->attach($picnicGrandCanyon->id);

		$bearCerms->picnics()->attach($picnicYellowstone->id);
		$bearCerms->picnics()->attach($picnicGrandCanyon->id);

		$bearAdobot->picnics()->attach($picnicYellowstone->id);
		$bearAdobot->picnics()->attach($picnicGrandCanyon->id);

		$this->command->info('They are terrorizing picnics!');

	}

	}

在我们的文件中我们插入了一些bears、fish、picnics测试数据以及关联了bears和picnic。

当处理多对多关系时，您也可以插入相关模型。我们这里需要用到**attach()**方法来添加相关记录。即用**attach()**方法把picnic中的记录的id跟bear中的数据关联起来即**$bearLawly->id**。

当seeder文件准备好后，在命令行中运行下面的命令：

    php artisan db:seed

![](http://pic.yupoo.com/reicky_v/DHnVfSDv/n1XvB.jpg)

OK，现在可以去数据库看看我们的测试数据已经添加到数据库中了。

下面我们来学习怎么使用eloquent对象来查询数据，会看到使用eloquent来查询数据是多么的容易。

### Using Eloquent ###

使用Eloquent，可以很容易来对数据库进行增删查改。仅仅只要调用模型和对应的方法就行了，即数据库的CRUD。

### 使用Eloquent来进行数据库的增删查改 ###

#### 添加数据 ####

我们可以使用**::create()**方法来添加数据。

    // create a bear
	Bear::create(array(
		'name'         => 'Super Cool',
		'type'         => 'Black',
		'danger_level' => 1
	));

	// alternatively you can create an object, assign values, then save
	$bear               = new Bear;

	$bear->name         = 'Super Cool';
	$bear->type         = 'Black';
	$bear->danger_level = 1;

	// save the bear to the database
	$bear->save();
	
你也可以创建一个新的bear对象，来添加数据。然后使用**save()**方法来保存。

当然我们也可以使用**firstOrCreate()**和**firstOrNew()**方法来添加数据。这两个方法会首先去检查是否存在相关的属性的字段，如果bear没有被检测到，它会创建一个新的实例对象。

    // find the bear or create it into the database
	Bear::firstOrCreate(array('name' => 'Lawly'));

	// find the bear or instantiate a new instance into the object we want
	$bear = Bear::firstOrNew(array('name' => 'Cerms'));

#### 查询数据 ####

使用Eloquent也很方便去查询相关的数据。

如下所示：

    // 获取所有的记录
	$bears = Bear::all();

	// 根据id来获取数据
	$bear = Bear::find(1);

	// 根据特定的字段来获取数据
	$bearLawly = Bear::where('name', '=', 'Lawly')->first();

	// 依据条件来查询数据
	$dangerousBears = Bear::where('danger_level', '>', 5)->get();

当我们使用**where**条件查询语句的时候，要配合使用**get()**或者是**first()**方法来取得数据。first()方法会获取第一条记录。

#### 更新数据 ####

更新数据，仅仅只需要找到我们需要更新的数据，改变相关属性的值然后保存就可以了。相当的简单。

    // let's change the danger level of Lawly to level 10

	// find the bear
	$lawly = Bear::where('name', '=', 'Lawly')->first();

	// change the attribute
	$lawly->danger_level = 10;

	// save to our database
	$lawly->save();

#### 删除数据 ####

删除数据比更新数据还要简单。这里有两个方法:找到你需要删除的数据然后使用delete就可以删除，或者是使用**destroy**方法来删除。

    // find and delete a record
	$bear = Bear::find(1);
	$bear->delete();

	// delete a record 
	Bear::destroy(1);

	// delete multiple records 
	Bear::destroy(1, 2, 3);

	// find and delete all bears with a danger level over 5
	Bear::where('danger_level', '>', 5)->delete();

### 根据数据库之间的关系来查询 ###

我们定义了bear和fish之间是一对一的关系，以及picnics和bears之间是多对多的关系。使用Eloquent我们可以根据数据之间的关系来查询数据。

#### 查询一对一的数据 ####

让我们来看看在模型中怎么来根据数据之间的一对一的关系来查询。

    // find a bear named Adobot
	$adobot = Bear::where('name', '=', 'Adobot')->first();

	// get the fish that Adobot has
	$fish = $adobot->fish;

	// get the weight of the fish Adobot is going to eat
	$fish->weight;

	// alternatively you could go straight to the weight attribute
	$adobot->fish->weight;

#### 查询一对多的数据 ####

通过下面的代码我们可以来查询所有的Lawly喜欢爬的树的数据：

    
	// find the trees lawly climbs
	$lawly = Bear::where('name', '=', 'Lawly')->first();

	foreach ($lawly->trees as $tree)
		echo $tree->type . ' ' . $tree->age;

### 查询多对多的数据 ###

我们来查询下为Cerms的熊喜欢的picnics的数据，也可以查询所有喜欢Yellowstone的熊的数据。

    // get the picnics that Cerms goes to ------------------------
	$cerms = Bear::where('name', '=', 'Cerms')->first();

	// get the picnics and their names and taste levels
	foreach ($cerms->picnics as $picnic) 
		echo $picnic->name . ' ' . $picnic->taste_level;

	// get the bears that go to the Grand Canyon picnic -------------
	$grandCanyon = Picnic::where('name', '=', 'Grand Canyon')->first();

	// show the bears
	foreach ($grandCanyon->bears as $bear)
		echo $bear->name . ' ' . $bear->type . ' ' . $bear->danger_level;

正如你看到的，通过Eloquent模型我们可以很容易的和数据库打交道。

### 实例 ###

下面让我们通过实际的编码通过实实在在的网页来展现我们查询的数据，来实际检测我们的代码。

我们需要编写好一个视图文件和规划好路由。

### 路由 ###

    / app/routes.php

	...
	// create our route, return a view file (app/views/eloquent.blade.php)
	// we will also send the records we want to the view

	Route::get('eloquent', function() {

		return View::make('eloquent')

			// all the bears (will also return the fish, trees, and picnics that belong to them)
			->with('bears', Bear::all()->with('trees', 'picnics'));

	});

### 视图文件 ###

我们使用laravel为我们提供的[blade模板引擎](http://laravel.com/docs/templates#blade-templating)来编写我们的视图文件。创建视图文件app/views/eloquent.blade.php。

    <!-- app/views/eloquent.blade.php -->

	<!doctype html>
	<html lang="en">
	<head>
	<meta charset="UTF-8">
	<title>Eloquent Bears</title>

	<!-- CSS -->
	<!-- BOOTSTRAP -->
	<link rel="stylesheet" href="//netdna.bootstrapcdn.com/bootstrap/3.1.1/css/bootstrap.min.css">
	<style>
		body { padding-top:50px; } /* add some padding to the top of our site */
	</style>
	</head>
	<body class="container">
	<div class="col-sm-8 col-sm-offset-2">

	<!-- BEARS -->
	<!-- loop over the bears and show off some things -->
	@foreach ($bears as $bear)

		<!-- GET OUR BASIC BEAR INFORMATION -->
		<h2>{{ $bear->name }} <small>{{ $bear->type }}: Level {{ $bear->danger_level }}</small></h2>

		<!-- SHOW OFF THE TREES -->
		<h4>Trees</h4>
		@foreach ($bear->trees as $tree) 
			<p>{{ $tree->type }}</p>
		@endforeach

		<!-- SHOW OFF THE PICNICS -->
		<h4>Picnics</h4>
		@foreach ($bear->picnics as $picnic)
			<p>{{ $picnic->name }}: Taste Level {{ $picnic->taste_level }}</p>
		@endforeach 

	@endforeach

	</div>
	</body>
	</html>

现在如果你在浏览器中打开http://example.com/eloquent这个地址，你会看到查询到的数据展现在你面前。

### 总结 ###

通过这篇文章，我们学习到了laravel中的eloquent很多的知识点，主要是以下内容：

- Migrations
- Eloquent models
- Seeding
- Defining relationship (定义数据之间的关系)
- Querying our database(查询数据)
- Querying relationships(查询关系数据)

当然Eloquent的知识远不止这些，你可以通过官方文档了解更多的信息。

更多的关于数据库方面的应用可以去我以前写的[这篇文章](http://scotch.io/tutorials/simple-laravel-crud-with-resource-controllers)了解更多的信息。










 