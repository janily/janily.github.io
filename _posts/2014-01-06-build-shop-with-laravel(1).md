---
layout: post
title: 用laravel4来编写一个商店应用(1)
categories:
- Life
tags:
- laravel
- php
- 翻译
---

> 今天在浏览[laravel.io](laravel.io)这个网站的时候，看到一篇[maxoffsky](http://maxoffsky.com/)写的一篇用laravel4来编写一个商店应用的教程文章，现在出到了第三篇看了看觉的非常不错。打算翻译出来。原文地址[Laravel Shop tutorial #1 – Building a review system](http://maxoffsky.com/code-blog/laravel-shop-tutorial-1-building-a-review-system/)。

![](http://pic.yupoo.com/reicky_v/DrqZ47Px/medium.jpg)

本篇文章主要是来实现网站的评分和商品浏览的功能。商品浏览和评分是电商类网站必备的功能，以及对于社交等需要用户参与产生内容的一些web应用来说也是必不可少的功能。

首先来看一下演示demo[http://laravel-shop.gopagoda.com/](http://laravel-shop.gopagoda.com/)，源代码地址[https://github.com/msurguy/laravel-shop-reviews](https://github.com/msurguy/laravel-shop-reviews)。

一个典型的评分系统一般都是以用户打分的形式来实现的，比如常见的1-5颗星的打分形式就是其中一种，如[Etsy.com](https://www.etsy.com/listing/168808125/instant-download-thanksgiving-cupcake?ref=shop_home_active):

![](http://pic.yupoo.com/reicky_v/Drr2i3Bk/medium.jpg)

**想象一下我们平常见过的一些电子商务类的web应用，是不是都包含用户的评论、评分这两个特征。**

在本篇教程中，我们就来实现这样的功能。

下面我们来规划一下要实现这些功能，我们要做哪些工作：

- 首先我们还是来看一下我们的演示demo来了解具体的功能点(上面的演示demo就是啦)。
- 定义显示产品以及显示产品详情页面的路由(url)。
- 为评论和产品定义好数据库的结构。
- 创建显示产品的主页和显示产品详情的页面的模板。
- 创建定义评分和评论的函数。
- 定义路径的逻辑。
- 在必要的情况下重构我们的代码。

现在对于编写一个星星的打分效果非常容易，我也发现非常多的用jQuery实现这样效果的插件，而且用户体验都非常不错。我这里使用的是这个插件[https://github.com/dobtco/starrr](https://github.com/dobtco/starrr)，我对它进行了一些修改，主要是为了和[Bootstrap3](http://bootsnipp.com/snippets/featured/expanding-review-and-rating-box)配合使用，源代码在这里[源代码](https://github.com/msurguy/laravel-shop-reviews)。

#### **实现评论系统的要点** ####

首先，我们需要考虑程序的效率，比如我们怎么来存储用户对商品的评论。

一般的做法是，分别为商品、评论以及打分在数据库中创建三个表。然后把它们相互关联起来，比如产品和评论的关系是多对多的关系(一个产品有多个评论)以及评论和评分是一对一的关系(一个评论只能有一个评分)。

这样的做法有一个缺点，那就是这里的评论的数据会产生很大的开销(由于它需要和产品和评分关联起来，会产生过多的sql查询)。那我们为什么不把评论和评分这两张表合并起来呢？这样也许在数据的结构上会更好一点。

当用户发表评论的时候，我们连同评论和评分一起保存到数据库之中。当然我们还需要把用户对产品的评论综合起来计算出它的平均值来显示为产品的总评分，并且显示在产品详情页面里。

一般来说一个在线电子商城通常会在首页显示一些产品的列表。我们这里也这样来在主页上显示一些产品。而且产品评分和评论功能这两个功能的可用性对于我们应用的性能有非常大的影响，所以我们在显示评论和评分数据的时候应尽可能减少数据库的查询。

考虑下这样一个场景，如果我们经常性的动态的去计算产品的评论和评分，这样可能会在性能上产生一些问题。为了避免这个问题，我们可以事先把产品的评论和评分计算好保存在指定的表中，这样当我们需要取得产品的评分和评论数据的时候，我们不需要执行过多的sql语句来获取数据。

分析完后，接下来是实际编码了。我已经把源代码都上传到了[github上面](https://github.com/msurguy/laravel-shop-reviews)你也可以在这个[地址](http://laravel-shop.gopagoda.com/)来体验一下我们的demo效果。

首先来定义路由，代码如下：

    <?php
 
	// 首页显示产品列表
	Route::get('/', function()
	{
	  return 'Shop homepage';
	});
	 
	// 显示产品详情
	Route::get('products/{id}', function($id)
	{
	  return 'Product: '.$id;
	});
	 
	// 用户评论
	Route::post('products/{id}', array('before'=>'csrf', function($id)
	{   
	  return 'Review submitted for product '.$id;
	}));

正所谓好的开始是成功的一半。通过本篇文章我们会不断完善我们的路由。现在我们先来规划和指定我们应用的数据结构和数据模型。

### **数据结构和数据模型** ###

我这里制作了一个简单的数据结构的线框图主要是关于产品和评论以及评分的一个关系图，如下图所示：

![](http://pic.yupoo.com/reicky_v/DrrsNWMm/medium.jpg)

我们就可以根据上面的这个图来定义我们的数据表。

主要是包含以下三个表：

- 用户表(主要是包含用户的一些比如名字、密码和登录信息)
- 产品表(存储产品的一些信息--名字、描述以及产品图片)
- 评论和评分系统表(主要是存储用户给产品的评分和评论)

下面这张图是数据表之间关系的一个视觉结构图:

![](http://pic.yupoo.com/reicky_v/DrrxKquh/medium.jpg)

可以点击[这个链接](http://www.laravelsd.com/share/CW5d1k)去看原图，可以看到更清楚。

下面是创建相关表的SQL语句:

    CREATE TABLE `products` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `published` tinyint(1) NOT NULL DEFAULT '0',
	  `rating_cache` float(2,1) unsigned NOT NULL DEFAULT '3.0',
	  `rating_count` int(11) unsigned NOT NULL DEFAULT '0',
	  `name` varchar(255) NOT NULL,
	  `pricing` float(9,2) unsigned NOT NULL DEFAULT '0.00',
	  `short_description` varchar(255) NOT NULL,
	  `long_description` text NOT NULL,
	  `icon` varchar(255) NOT NULL,
	  `created_at` datetime NOT NULL,
	  `updated_at` datetime NOT NULL,
	  PRIMARY KEY (`id`)
	) ENGINE=InnoDB  DEFAULT CHARSET=latin1 AUTO_INCREMENT=1 ;
	 
	INSERT INTO `products` (`id`, `published`, `rating_cache`, `rating_count`, `name`, `pricing`, `short_description`, `long_description`, `icon`, `created_at`, `updated_at`) VALUES
	(1, 1, 3.0, 0, 'First product', 20.99, 'This is a short description asdf as This is a short description asdf as', 'Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum', '', '2013-11-06 05:11:00', '2013-11-12 05:51:07'),
	(2, 1, 3.0, 0, 'Second product', 55.00, 'This is a short description', 'Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum', '', '2013-11-06 05:11:00', '2013-11-11 16:17:23'),
	(3, 1, 3.0, 0, 'Third product', 65.00, 'This is a short description', 'Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum', '', '2013-11-06 05:11:00', '2013-11-06 06:08:00'),
	(4, 1, 3.0, 0, 'Fourth product', 85.00, 'This is a short description', 'Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum', '', '2013-11-06 05:11:00', '2013-11-06 06:08:00'),
	(5, 1, 3.0, 0, 'Fifth product', 95.00, 'This is a short description', 'Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum', '', '2013-11-06 05:11:00', '2013-11-06 06:08:00');
	 
	CREATE TABLE `reviews` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `product_id` int(11) NOT NULL,
	  `user_id` int(11) NOT NULL,
	  `rating` int(11) NOT NULL,
	  `comment` text NOT NULL,
	  `approved` tinyint(1) unsigned NOT NULL DEFAULT '1',
	  `spam` tinyint(1) unsigned NOT NULL DEFAULT '0',
	  `created_at` datetime NOT NULL,
	  `updated_at` datetime NOT NULL,
	  PRIMARY KEY (`id`)
	) ENGINE=InnoDB DEFAULT CHARSET=latin1 AUTO_INCREMENT=1 ;
	 
	CREATE TABLE `users` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `user_type` int(10) unsigned NOT NULL DEFAULT '0',
	  `email` varchar(128) NOT NULL,
	  `password` varchar(128) NOT NULL,
	  `created_at` datetime NOT NULL,
	  `updated_at` datetime NOT NULL,
	  PRIMARY KEY (`id`)
	) ENGINE=MyISAM DEFAULT CHARSET=latin1 AUTO_INCREMENT=1 ;

创建好数据库后，我们就可以来定义表与表之间的关系了，也就是数据模型。

首先来创建产品和评论和评分之间数据之间的关系，即它们之间的关系是一对多的关系，我们在**app/models**目录下新建一个**Product.php**的文件代码如下：

    <?php
 
	class Product extends Eloquent
	{
	  public function reviews()
	  {
	    return $this->hasMany('Review');
	  }
	}

接下来是建立评论系统与产品之间的数据模型关系，以及使用laravel为我们提供的[Scope](http://www.golaravel.com/docs/4.1/eloquent/#query-scopes)定义一些查询范围。代码如下所示：

    class Review extends Eloquent
	{
	 
	  public function user()
	  {
	    return $this->belongsTo('User');
	  }
	 
	  public function product()
	  {
	    return $this->belongsTo('Product');
	  }
	 
	  public function scopeApproved($query)
	  {
	    return $query->where('approved', true);
	  }
	 
	  public function scopeSpam($query)
	  {
	    return $query->where('spam', true);
	  }
	 
	  public function scopeNotSpam($query)
	  {
	    return $query->where('spam', false);
	  }
	}

上面定义了评论与用户以及产品之间的关系，当然后面还会添加一些函数来计算和统计评分和评论，不过现在已经足够我们进行下一步开发了，我们下一步是创建产品显示以及评论和评分显示的视图模板。

### 创建产品和评论显示的视图模板 ###

这里假设你具备一些用laravel开发的知识(如果没有的话可以去[官方文档看看](http://www.golaravel.com/docs/4.1/introduction/)或者是买我出的一本关于laravel开发的书[comprehensive Laravel book](http://affiliate.manning.com/idevaffiliate.php?id=1294_374))这里我就不会再教大家怎么去建立视图模板这方面的东东了，不过对于评分和评论我已经用bootstrap建好了一个HTML模板文件，你可以去[看看](http://bootsnipp.com/msurguy/snippets/PjPa)，如下所示：

<iframe id="result-iframe" allowtransparency="true" frameborder="0" scrolling="yes" sandbox="allow-scripts" src="http://bootsnipp.com/fullscreen/PjPa" style="width: 100%; height: 400px;"></iframe>

然后我们就可以在视图模板中输出相关产品的评分和评论数据了，如下所示：

    @foreach($reviews as $review)
	  <hr>
	  <div class="row">
	    <div class="col-md-12">
	    @for ($i=1; $i <= 5 ; $i++)
	      <span class="glyphicon glyphicon-star{{ ($i <= $review->rating) ? '' : '-empty'}}"></span>
	    @endfor
	 
	    {{ $review->user ? $review->user->name : 'Anonymous'}} <span class="pull-right">{{$review->timeago}}</span> 
	 
	    <p>{{{$review->comment}}}</p>
	    </div>
	  </div>
	@endforeach

其中**$review->timeago**是一个输出当前时间的函数，这个方法会添加到模型文件夹里面的**Review.php**文件中:

    public function getTimeagoAttribute()
	{
	  $date = CarbonCarbon::createFromTimeStamp(strtotime($this->created_at))->diffForHumans();
	  return $date;
	}  

CarbonCarbon是laravel内置的一个处理时间的类可以到这个地址看[详细文档](http://lukearmstrong.co.uk/docs/laravel4/Carbon/Carbon.html#method_createFromTimestamp)。

不会注意到我们这里用了**$review->comment**这样的表达式。这是laravel约定的显示输出方式。我这里使用了三元运算来输出评论。

你可以去看看视图的详细代码[https://github.com/msurguy/laravel-shop-reviews](https://github.com/msurguy/laravel-shop-reviews)。

到此为止还剩下几个功能需要编写，一个是存储用的评论以及用路由把我们编写好的功能串联起来。

### 编写存储用户评论和计算评分 ###

首先，让我们在**app/models/Review.php**模型文件里创建一个**storeReviewForProduct**的方法，这个方法主要是做以下几件事情:

- 用户提交评论和评分的时候，获取产品的ID
- 在产品模型里面添加相关评论和评分的方法
- 计算产品的评分

把下面的代码添加到**Review.php**文件里面：

     
	public function getTimeagoAttribute()
	{
	  ...
	}
	 
	//这个方法是根据产品的ID来显示评分和评论，这里调用了recalculateRating这个方法来计算产品的评分
	public function storeReviewForProduct($productID, $comment, $rating)
	{
	  $product = Product::find($productID);
	 
	  // 这行代码是用来验证用户是否登录的
	  //$this->user_id = Auth::user()->id;
	 
	  $this->comment = $comment;
	  $this->rating = $rating;
	  $product->reviews()->save($this);
	 
	  // 计算商品的评分
	  $product->recalculateRating($rating);
	}

当然我们这里没有保存发表评论和评分的用户名，因为我们还没创建登录系统。

下面我们需要在在**app/models/Product.php**这个模型文件里添加计算评论的方法，代码如下：

    <?php
 
	class Product extends Eloquent
	{
	  public function reviews()
	  { ... }
	 
	  // 评分的平均值是所有评分的平均值
	  // 把得到的平均值缓存起来，这样再下次显示的时候就不需要再计算了
	  // 统计评论的条数
	 
	  public function recalculateRating($rating)
	  {
	    $reviews = $this->reviews()->notSpam()->approved();
	    $avgRating = $reviews->avg('rating');
	    $this->rating_cache = round($avgRating,1);
	    $this->rating_count = $reviews->count();
	    $this->save();
	  }
	}

OK，差不多产品的评分系统的功能实现了，下面是用路由把这些逻辑功能联系起来整合在一起为一个完整的web应用。

### 路由编写 ###

在前面我们已经定义好了三个路由，分别是显示所有产品，产品详情以及评分系统。现在我们来完善下路由方面的逻辑判断的功能。

首先是首页显示所有商品这个路由：

    // 显示产品
	Route::get('/', function()
	{
	  $products = Product::all();
	  return View::make('index', array('products'=>$products));
	});

当点击具体单个商品的时候要跳转到商品的详情页面，这就需要把商品的id作为一个参数传送给路由，再根据这个id来显示商品的详细信息：

    // 根究id来显示对应的商品
	Route::get('products/{id}', function($id)
	{
	  $product = Product::find($id);
	  // 获取所有经过审核的评论并分页显示
	  $reviews = $product->reviews()->with('user')->approved()->notSpam()->orderBy('created_at','desc')->paginate(100);
	 
	  return View::make('products.single', array('product'=>$product,'reviews'=>$reviews));
	});

最后，当用户提交评论的时候，我们需要做一个判断验证(这里我们把验证的规则放在Review模型里面，这样能保持路由的简洁)，如果验证通过，就把用户提交的评论和评分保存在数据库中并提示用户评论已提交，代码如下：

    // 处理评论和评分的路由
	Route::post('products/{id}', array('before'=>'csrf', function($id)
	{
	  $input = array(
	    'comment' => Input::get('comment'),
	    'rating'  => Input::get('rating')
	  );
	  // 新建一个Review的模型
	  $review = new Review;
	 
	  // 动用Review模型里面的验证规则来验证用户提交的数据
	  $validator = Validator::make( $input, $review->getCreateRules());
	 
	  //如果通过则保存在数据库中，否则返回错误信息
	  if ($validator->passes()) {
	    $review->storeReviewForProduct($id, $input['comment'], $input['rating']);
	    return Redirect::to('products/'.$id.'#reviews-anchor')->with('review_posted',true);
	  }
	 
	  return Redirect::to('products/'.$id.'#reviews-anchor')->withErrors($validator)->withInput();

	}));

完整的代码可以去可以去看[源代码](https://github.com/msurguy/laravel-shop-reviews/blob/master/app/routes.php)。视图模板的详细代码因为有点多，所以在本文中就省略了，可以去源代码看看详细的代码。

### 代码重构 ###

其实上面代码还有很多可以优化的地方，如下所示：

- 把逻辑处理方面的代码从路由移到相关的控制器里面(比如分别为首页显示和商品详情页面创建shop和product控制器)
- 依靠视图之间的关系和表现来理清模型的代码
- 阻止同一个用户对一个商品进行多次评论(这里可以编写一个用户注册系统来实现)

通过本文我们一个简单的商品系统就完成了，当然这只是第一步，后面我们会不断地完善它，未完待续~~!




