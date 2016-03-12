---
layout: post
title: laravel实战系列：使用laravel开发一个图片分享类的应用
categories:
- Life
tags:
- laravel
- php
---

继续laravel实战系列，今天我们来使用laravel编写一个简单的图片分享应用。在这个程序中，我们将会使用和学习到下面的知识：

- 使用laravel的迁移来管理和创建数据库
- 创建一个photo的模型
- 设置一些默认的配置
- 安装和使用第三方的开发包
- 创建一个安全的文件上传系统
- 验证表单
- 显示图片
- 删除图片

### 使用迁移来创建图片的数据表 ###

在创建相关的表之前，先要在数据库中创建一个**images**的数据库。创建好之后，我们在终端中运行下面的命令来创建数据库的迁移文件：

	php artisan migrate:make create_photos_table create=photos

那在这个表中我们需要定义些什么字段呢？我们需要定义一个**id**、**image titles**、**image file name**以及时间戳。ok，现在打开迁移文件编写如下代码：

	<?php

	use Illuminate\Database\Schema\Blueprint;
	use Illuminate\Database\Migrations\Migration;
	
	class CreatePhotosTable extends Migration {

	/**
	 * Run the migrations.
	 *
	 * @return void
	 */
	public function up()
	{
		Schema::create('photos', function(Blueprint $table)
		{
			$table->increments('id');
			$table->string('title',400)->default('');
			$table->string('image',400)->default('');
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
		Schema::drop('photos');
	}

	}

然后在终端中运行下面的命令：

	php artisan migrate

运行之后就会在数据库中生成对应的表。

### 创建photo模型 ###

在laravel中操作数据库，它为我们提供了非常强大的**Eloquent ORM**。

在**app/models**目录中，新建一个**images.php**文件编写以下代码：

	<?php 
	
	class Photo extends Eloquent {

		protected $table = 'photos';

		protected $fillable = array('title','image');

		public $timestamps = true;
	}

在上面的代码中，我们指定Eloquent要操作的表为**photos**。并且使用fillable属性指定哪些属性可以被集体赋值。设置好之后，我们就可以使用Eloquent ORM提供的强大的特性来非常方便的操作数据库了。

模型创建好后，按照前面的路子下面就要开始编写控制器的代码了。不过在这之前，因为我们这个是图片分享的程序，就涉及到图片上传所以我们还需要配置下图片上传。比如图片上传最大的体积；还有缩略图的尺寸等相关配置。我们可以新建一个配置文件来设置这些值。

### 配置文件 ###

在laravel中，设置一些功能的默认值非常容易。所有的**config**都以**key=>value**这样的形式来表示。

在**app/config**目录中创建**image.php**的配置文件，编写以下代码：

	<?php 	

	// 图片上传配置文件

	return array (

		// 设置上传的路径

		'upload_folder' => 'uploads',

		// 设置缩略图存放的文件夹

		'thumb_width' => 'uploads/thumbs',

		// 缩略图的宽和高

		'thumb_width' => 320,

		'thumb_width' => 240

	);

当然你可以在文件中编写任何你想配置的设置。你可以使用laravel提供的**Config**类中的**get()**方法来获取配置文件中的值。

	Config::get('filename.key')

在括号中参数之间使用点连接起来的，第一个参数是配置文件的名字，第二个是配置文件中具体某个配置的名字。比如在我们上面的配置文件中，如果要获取上传文件的路径的值，我们可以这样做：

	Config::get('image.upload_folder')

上面的代码会直接返回**public/uploads**这个值给我们。

还有件需要注意的事情：我们在程序中定义了一些文件夹的名字，但是并没有在程序中创建它。在一些服务器环境中，当上传文件的时候。会自动创建不存在文件夹。不过有时候服务端会报错。所以为了安全起见我们还是在**pubic**中创建我们需要的文件夹，如下所示：

	uploads/
	
	uploads/thumbs

现在我们就可以开始来处理图片上传部分的代码了。

### 安装和使用第三方开发包 ###

在处理图片上传之前，我们需要安装一些第三方的图片处理的开发包。laravel 4是使用的**Composer**这个包管理工具来管理开发，而是用composer来安装和更新第三方的开发包将会变得非常简单。我们这里会使用到**Intervention**这个开发包。要安装第三方包按着下面的步骤来就可以了：

- 1、首先是确保你的composer.phar文件是最新的，我们可以通过**composer.phar self-update**这个命令把更新到最新版本。
- 2、打开**composer.json**文件，添加第三方包的名字在**require**里面，如下所示：
	"require": {
		"laravel/framework": "4.1.*",
		"intervention/image" : "dev-master"
	},
- 3、设置好之后，在终端中运行下面的命令：
	php artisan update

运行之后，它会检查**composer.json**文件并更新安装所有的依赖文件，如果有新的依赖文件，它会下载并安装好它。

- 4、安装好之后，我们还需要配置一下才能使用安装好的第三方的开发包。在这里即要配置**Intervention**这个类。打开**app/config/app.php**文件，添加**providers**：**Intervention\Image\ImageServiceProvider**
- 5、我们还可以为它配置别名。这样我们使用它就会更加容易。在aliases数组里面添加下面的代码：**'Image' => 'Intervention\Image\Facades\Image'**,
- 6、这个类提供的方法非常简洁。比如我们要剪裁图片我们可以这样做：**Image::make(Input::file('photo')->getRealPath())->resize(300,200)->save('foo.jpg');**。更多的关于**Intervention**这个开发包的信息可以去它的官方网站看看[http://intervention.olivervogel.net](http://intervention.olivervogel.net)。

现在基本的工作准备好后，下一步是创建好上传文件的视图，当然也是基于表单的。

### 创建一个安全的上传文件的视图 ###

现在我们需要创建一个基于表单的上传文件的视图。根据不同的请求通过控制器来处理相关的视图逻辑：

- 1、打开路由文件**app/routes.php**，删除默认的**Route::get()**路由，添加以下代码：

	Route::get('/',array('as'=>'index_page','uses'=>'ImageController@getIndex'));

	Route::post('/',array('as'=>'index_page_post','before'=>'csrf','uses' => 'ImageController@postIndex'));

**as**是用给路由指定名字的，重定向和生成URL时，使用命名路由会更方便。并且当路由改变的时候，由于使用了命名路由，你在其它的地方使用的url仍然指向正确的路径。**before**是路由过滤器，路由过滤器提供了非常方便的方法来限制对应用程序中某些功能访问，例如对于需要验证才能访问的功能就非常有用。我们这里使用的是**csrf**来过滤御CSRF（跨站请求伪造）式攻击，如果是csrf攻击，我们不会往下执行代码。当然也可以同时使用多个过滤器，如**filter1**|**filter2**。也可以在控制器中使用**CSRF**过滤。

- 2、下面来编写控制器相关的方法。先在**app/controllers**中创建**ImageController.php**文件：

	<?php 

		class ImageController extends BaseController {
	
		public function getIndex () {
			return View::make('tpl.index');
		}
	}

我们的控制器是采用RESTful风格的；所以我们的方法名称是**getIndex()**。我们这个方法只是简单的加载了一个视图文件。

- 3、创建视图文件。在**app/views**目录下创建一个**frontend_master.blade.php**。代码如下：

![](http://pic.yupoo.com/reicky_v/DJnyVQH7/Wl32d.png)

- 4、在**views**目录新建一个**tpl**目录，然后在**tpl**新建一个一个**index.blade.php**文件，代码如下：

![](http://pic.yupoo.com/reicky_v/DJnAiU8N/gassk.jpg)

由于我们是使用**frontend_master.blade.php**作为布局视图，所以在上面的代码中我们使用了**@extends()**方法来引入布局文件。

我们使用了laravel中的**Form**类来生成包含**title**和**upload**两个字段的表单。在表单中，我们传递了两个参数给它，**url**定义了表单要提交数据的路径，而**files**的值为**true**，则告诉表单用来上传文件。

至于安全方面的问题，laravel也为我们考虑到了。Laravel框架提供了一种简单的方法帮助你的应用抵御CSRF（跨站请求伪造）式攻击。Laravel框架会自动在用户 session 中存放一个随机令牌（token），并且将这个令牌作为一个隐藏字段包含在表单信息中。

	<input name="_token" type="hidden" value="BndR9iWzoKiseXuADa7gwzG5zf2ZKK3pPiRNvOGE">

- 5、编写样式文件。在**public**目录中新建一个**css**文件夹，然后新建一个**style.css**文件，代码如下：

	body{width:60%; margin:auto; background:#dedede}
	h2{font-size:40px; text-align:center; font-family: Tahoma,Arial,sans-serif}
	h3{font-size:25px; border-radius:4px; font-family: Tahoma,Arial,sans-serif; text-align:center; width:100%}
	h3.error{border:3px solid #d00; background-color: #f66; color:#d00 }
	h3.success{border:3px solid #0d0; background-color:#0f0; color:#0d0}
	p{font-size:25px; font-weight: bold; color: black; font-family: Tahoma,Arial,sans-serif}
	ul{float:left;width:100%;list-style:none}
	li{float:left;margin-right:10px}
	input{float:left; width:100%; border-radius:13px; font-size:20px; height:30px; border:10px 0 10px 0; margin-bottom:20px}

最后视图文件如下图所示：

![](http://pic.yupoo.com/reicky_v/DJnKjv1r/zTqGE.jpg)

### 表单验证 ###

这一个部分主要是来实现表单验证功能，以确保数据的完整性。通过验证后，上传文件，创建缩略图以及把图片的相关信息保存在数据库中。

1、首先在模型文件里定义表单验证规则。在模型文件夹里的**photo.php**文件中编写以下代码：

	// 验证规则

	public static $upload_rules = array(

		'title' => 'required|min:3',
		'image' => 'required|image'

	)

我们使用了**public**这个关键字，所以在模型文件外面也可以访问到它。

在我们定义的规则中**title**和**image**这两个字段是必需输入的，**title**至少要包含3个字符。而**image**则制定文件类型必需是图片类型的文件。laravel是通过**Fileinfo**扩展来读取文件的类型的，所以需要在php配置文件中配置好。

2、接下来是在控制器中使用**post**方法来处理表单提交过来的文件。在**ImageController.php**文件中编辑代码如下：

	public function postIndex() {
		//首先是验证文件是否符合规则

		$validation = Validator::make(Input::all(),Images::$upload_rules);

		// 如果验证失败，则重定向到首页并提示错误信息

		if($validation->fails()) {
			return Redirect::to('/') 
				->withInput()
				->withErrors($validation);
		}
		else {
			// 如果通过则上传文件

			$Image = Input::file('image');

			// 通过上传文件的名称来获取文件名

			$filename = $image->getClientOriginalName();

			$filename = pathinfo($filename,PATHINFO_FILENAME);

			// 对于文件外面还可以做好一点，在文件名前添加8位数的随机数字，这样也可以确保文件名称的唯一性

			$fullname = Str::slug(Str::random(8).$filename).'.'.
				$image->getClientOriginalExtension();

			//上传完文件后，我们还要创建缩略图并移动到thumbs文件夹

			$upload = $image->move
				(Config::get('image.upload_folder'),$fullname);

			// 下面是调用先前下载好第三方的开发包来处理缩略图方面的工作

			Image::make(Config::get('image.upload_folder').'/'.$fullname)
			->resize(Config::get('image.thumb_with'),null,true)
			->save(Config::get('image.thumb_folder').'/'.$fullname);

			// 如果文件是已经上传了的，则提示信息给用户。反之则提示成功的上传的信息。

			if($upload) {
				//上传完图片把相关信息插入到数据库中
				$insert_id = DB::table('images')->insertGetId(
				    array(
				    	'title' => Input::get('title'),
				    	'image' => $fullname
				    )
				);

				//重定向对应图片链接的页面
				return Redirect::to(URL::to('snatch/'.$insert_id))
					->with('success','Your image is uploaded successfully!');
			} else {
				//提示错误信息
				return Redirect::to('/')
					->withInput()
					->with('error','Sorry, the image could not be uploaded, please try again later');
			}
		}
	}

下面来分析下上面的代码：

- 1、首先是调用在模型里已经定义好的验证规则**Image::$upload_rules**。
- 2、然后是定义了上传文件的文件名。先使用**getClientOriginalName()**方法获取文件的名字，然后使用**getClientOriginalExtension()**方法来获取文件的扩展名。我们使用了**STR**这个类中的**random()**方法来生成8为的随机字符串来赋值给文件名。最后使用laravel中STR类中的**slug()**方法来创建友好的URL。
- 3、所有的变量定义好后，使用**move()**方法来把上传的文件移动到指定的文件夹。这个方法有两个参数，一个是要移动的路径的名称，第二个参数是上传文件的文件名。
- 4、上传后，我们使用了已经下载好的**Intervention**这个第三方的开发包来处理缩略图的相关工作。
- 5、最后是把上传图片的相关的信息插入到数据库即图片的名字和title。我们使用了**insertGetId()**方法来插入数据并获取ID的值。当然我们也可以使用**Eloquent ORM**的**create()**方法来插入数据，使用**$create->id**来获取数据的id。
- 6、完成并获取id后，跳转到缩略图的页面，可以看到缩略图的相关信息。

### 显示图片 ###

现在需要新建一个显示上传图片信息的视图。这个视图主要是要完成下面的工作：

1、首先是定义一个**GET**的路由。打开**routes.php**文件，编写下面的代码：

	Route::get('snatch/{id}', 
	array('as' => 'get_image_information',
	'uses' => 'ImageController@getSnatch'))
	->where('id','[0-9]+');

在上面的代码中使用**where()**方法我们定义了一个id变量，并且使用了正则表达式来对id进行了验证过滤，这样可以确保id是有数字组成的。

2、在控制器中定义**getSnatch()**方法。

	public function getSnatch($id) {

		// 从数据中查询图片的id
		$image = Image::find($id);

		// 如果找到，就加载视图，否则就返回到主页面并提示错误信息

		if($image) {
			return View::make('tpl.permalink')
				->with('image',$image);
		} else {
			return Redirect::
				to('/')->with('error','Image not find');
		}

	}

首先使用**Eloquent ORM**中的**find()**方法来查询图片信息。如果没有，则表示数据库中没有记录。这里使用了**if**的语法来判断数据库中是否有记录，如果有则加载对应的视图文件并把查询到的信息用**$image**变量使用**with()**方法传递给视图。如果没有，则返回到首页并提示错误信息。

3、在**app/views/tpl**目录中创建**permalink.blade.php**文件，代码如下：

![](http://pic.yupoo.com/reicky_v/DJtnLKZt/zwjCh.jpg)

代码比较好理解。这里有用到**HTML**类中一个新的方法**entities()**，其实就是php中的**htmlentities()**方法。因为我们是使用Eloquent来定义的**$image**变量，所以我们可以直接**$image->columName**来获取对应的值。

现在我们可以试着上传一张图片看看，不出意外应该是下面这样的：

![](http://pic.yupoo.com/reicky_v/DJuGDZSv/e308A.jpg)

4、那如果我们要显示所有的图片呢?这个好办，按着下面的步骤来就行了：

### 显示所有的图片 ###

1、定义URL，打开**routes.php**文件，添加下面的代码：

	// 显示所有的图片
	Route::get('all',array('as' => 'all_images','uses' => 'ImageController@getAll'));

2、然后在控制器中编写**getAll()**方法，在控制机器中编写如下代码：

上面的代码中，我们获取了所有的数据并使用laravel中的**paginate()**方法来进行分页。

3、在**app/views/tpl/**目录中新建一个**all_image.blade.php**文件，代码如下：

![](http://pic.yupoo.com/reicky_v/DJuT85o6/koCL7.jpg)

我们首先检查是否查询到图片，如果有则使用**foreach**语句把数据循环出来并装载到**li**标签里面。而**links()**方法则是配合**paginate()**方法来生成分页标签的。至于分页模板你也可以在**app/config/view.php**模板中配置。

如果检查到数据库中没有图片，则提示用户上传图片。

### 从数据库和服务端删除图片 ###

现在我们来为我们的程序添加删除功能，同时删除数据库中的记录和服务端的图片文件。

1、在路由文件中定义一个规则，打开**routes.php**文件，添加下面的代码：

	Route::get('delete/{id}',array('as' => 'delete_image','uses' => 'ImageController@getDelete'))
		->where('id','[0-9]+');

2、在**ImageController.php**文件中编写**getDelete()**方法：

	public function getDelete($id) {

		// 找到要删除图片的信息
		$image = Images::find($id);

		// 如果存在，则执行下面的代码

		if($image) {
			// 首先是删除服务端存放的图片文件

			File::delete(Config::get('image.upload_folder').'/'$image->image);
			File::delete(Config::get('image.thumb_folder').'/'$image->image);

			// 然后再删除数据中的相关的记录

			$image->delete();

			// 重定向到主页并提示用户成功删除

			return Redirect::to('/')
				->with('success','Image deleted successfully');
		} else {
			//如果没有查询到图片，就重定向到主页并提示错误信息

			return Redirect::to('/')
				->with('error','No image with given ID found');
		}

	}
	
代码的分析这里就不写了，在注释里有详细的解释。

Ok，一个简单的图片分享网站的程序就完成了，当然还有很多的地方可以完善。比如UI方面的可以做的更好一些等等。具体代码已经上传到github上面，可以去这个[地址](https://github.com/janily/laravel-share/)下载。




	



