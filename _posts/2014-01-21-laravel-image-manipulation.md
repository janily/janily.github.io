---
layout: post
title: 在laravel4中的用Imagine来处理图片
categories:
- Life
tags:
- laravel
- php 
- 翻译
---

> 现在laravel开发社区真是越来越好啦，生机勃勃在社区中很多开发者贡献了非常不错第三方的开发包，有很多web开发中常见的应用功能都有现成的开发包存在，不用我们再重复发明轮子大大提升了开发效率。今天这篇文章我们就来学习下在laravel4中使用**Imagine**这个开发包来处理和管理图片。原文地址[Image manipulation in Laravel 4 with Imagine...](http://creolab.hr/2013/07/image-manipulation-in-laravel-4-with-imagine/)。

开始之前安装设置laravel，并在Composer.json文件里面添加[Imagine](https://github.com/avalanche123/Imagine)的依赖并安装它，首先来安装[laravel4](http://laravel.com/)。

    composer create-project laravel/laravel l4image --pre-dist

然后编辑Composer.json文件运行：

    composer update

安装imagine这个第三方的文件。

下面我们创建一个用来处理图片的类。我一般会在app目录下新建一个“services”文件夹然后把类文件放在里面，我们创建Image文件：

**app/services/Image.php**

    <?php namespace App\Services;
 
	class Image {
	 
	}

既然使用了laravel来开发，我们可以使用laravel的方式来创建一个[Facade](http://www.golaravel.com/docs/4.1/facades/#practical-usage)，我们创建如下文件夹和文件：

**app/facades/ImageFacade.php**

    <?php namespace App\Facades;
 
	use Illuminate\Support\Facades\Facade;
	 
	class ImageFacade extends Facade {
	 
	    protected static function getFacadeAccessor()
	    {
	        return new \App\Services\Image;
	    }
	 
	}

现在我们可以给我们定义的类指定一个别名这样使用起来更加方便。

为了能够在使用这个类的时候能够自动加载。我们需要把**app/facades**和**app/services**这两个目录添加到**composer.json**文件中的**autoload classmap**数组中去。

ok，准备工作完毕，也许这个准备工作需要花点时间，但是只要设置好了，后面使用它的时候就会很方便了事半功倍何乐而不为呢？

设置完后接下来就是来编写相关的图片处理方法了。一般图片处理中剪裁图片的需求最常见，那就先来创建一个剪裁图片的方法**resize()**。这个方法需要几个参数：图片的原始路径，需要剪裁的宽和高以及图片压缩的质量等等。首先来初始化一下我们的这个类：

**app/services/image.php**

    <?php namespace App\Services;
 
	use Config, File, Log;
	 
	class Image {
 
    /**
     * Instance of the Imagine package
     * @var Imagine\Gd\Imagine
     */
    protected $imagine;
 
    /**
     * Type of library used by the service
     * @var string
     */
    protected $library;
 
    /**
     * Initialize the image service
     * @return void
     */
    public function __construct()
    {
        if ( ! $this->imagine)
        {
            $this->library = Config::get('image.library', 'gd');
 
            // Now create the instance
            if     ($this->library == 'imagick') $this->imagine = new \Imagine\Imagick\Imagine();
            elseif ($this->library == 'gmagick') $this->imagine = new \Imagine\Gmagick\Imagine();
            elseif ($this->library == 'gd')      $this->imagine = new \Imagine\Gd\Imagine();
            else                                 $this->imagine = new \Imagine\Gd\Imagine();
        }
    }
 
	}

在上面的代码里，我们设置了一个构造函数主要是用来设置第三方图片处理类库的，如我们这里使用的**GD/ImageMagick**等等。

下面在app目录下的config文件夹里新建一个**image.php**配置文件，主要是用来配置图片的路径、使用的第三方的图片库等选项：

**app/config/image.php**

    return array(
    'library'     => 'imagick',
    'upload_dir'  => 'uploads',
    'upload_path' => public_path() . '/uploads/',
    'quality'     => 85,
	);

配置完后是编写我们的**resize()**方法，代码如下：

    /**
	 * Resize an image
	 * @param  string  $url
	 * @param  integer $width
	 * @param  integer $height
	 * @param  boolean $crop
	 * @return string
	 */
	public function resize($url, $width = 100, $height = null, $crop = false, $quality = 90)
	{
    if ($url)
    {
        // URL info
        $info = pathinfo($url);
 
        // The size
        if ( ! $height) $height = $width;
 
        // Quality
        $quality = Config::get('image.quality', $quality);
 
        // Directories and file names
        $fileName       = $info['basename'];
        $sourceDirPath  = public_path() . '/' . $info['dirname'];
        $sourceFilePath = $sourceDirPath . '/' . $fileName;
        $targetDirName  = $width . 'x' . $height . ($crop ? '_crop' : '');
        $targetDirPath  = $sourceDirPath . '/' . $targetDirName . '/';
        $targetFilePath = $targetDirPath . $fileName;
        $targetUrl      = asset($info['dirname'] . '/' . $targetDirName . '/' . $fileName);
 
        // Create directory if missing
        try
        {
            // Create dir if missing
            if ( ! File::isDirectory($targetDirPath) and $targetDirPath) @File::makeDirectory($targetDirPath);
 
            // Set the size
            $size = new \Imagine\Image\Box($width, $height);
 
            // Now the mode
            $mode = $crop ? \Imagine\Image\ImageInterface::THUMBNAIL_OUTBOUND : \Imagine\Image\ImageInterface::THUMBNAIL_INSET;
 
            if ( ! File::exists($targetFilePath) or (File::lastModified($targetFilePath) < File::lastModified($sourceFilePath)))
            {
                $this->imagine->open($sourceFilePath)
                              ->thumbnail($size, $mode)
                              ->save($targetFilePath, array('quality' => $quality));
            }
        }
        catch (\Exception $e)
        {
            Log::error('[IMAGE SERVICE] Failed to resize image "' . $url . '" [' . $e->getMessage() . ']');
        }
 
        return $targetUrl;
    }
	}

我们来看看上面的代码具体含义。首先用**pathinfo()**这个方法来检测图片的URL。然后再以这个地址的图片按指定宽高来检测图片。比如我们需要剪裁**public/uploads/article/1/chuck.jpg**这张图片，我们就可以使用定义的resize方法来剪裁这张图片：

    Image::resize('uploads/article/1/chuck.jpg', 200, 200);

图片就会自动剪裁保存在**public/uploads/article/1/200×200/chuck.jpg**这个目录。具体的细节可以去[这里](http://imagine.readthedocs.org/)详细了解。

如果想把图片剪裁为一个正方形那只需要指定一个高的值就可以了:

    Image::resize('uploads/article/1/chuck.jpg', 200);

在web应用中还有一个很常见的需求就是创建缩略图，那下面就来创建一个缩略图的方法：

    /**
	* Helper for creating thumbs
	* @param string $url
	* @param integer $width
	* @param integer $height
	* @return string
	*/
	public function thumb($url, $width, $height = null)
	{
	    return $this->resize($url, $width, $height, true);
	}

比如我们需要创建一个80*80的缩略图，我们就可以这样来创建：

    
	Image::thumb('uploads/article/1/chuck.jpg', 80);

**public/uploads/article/1/80x80_crop/chuck.jpg**这张缩略图就会自动创建。创建缩略图的前提是这张图片不存在或者是新剪裁一张图片。当然你也可设置只要调用它就会创建图片，如果已经存在就覆盖它。

### 图片上传 ###

图片上传几乎是每一个web应用必备的功能，那我们就来创建一个图片上传的方法。但是考虑到当用户访问网站的时候，如果你的网站有很多图片需要剪裁的话那会花费很多的时间来加载图片，尤其是对用户体验会有损害。所以我们可以在上传图片就对图片进行指定的尺寸剪裁，我们可以考虑使用队列来处理以节约时间，不过我们这里就不展开了，我们这里只是编写一个简洁的上传图片功能。

我这里就不展开阐述图片上传的细节了，关于这个网络上有很多这方面的教程。但是你需要在你的控制器编写以下的代码：

    Image::upload(Input::file('image'), 'articles/' . $article->id, true);

下面的代码就是上传方法的具体代码：

    /**
	 * Upload an image to the public storage
	 * @param  File $file
	 * @return string
	 */
	public function upload($file, $dir = null, $createDimensions = false)
	{
    if ($file)
    {
        // Generate random dir
        if ( ! $dir) $dir = str_random(8);
 
        // Get file info and try to move
        $destination = Config::get('image.upload_path') . $dir;
        $filename    = $file-&gt;getClientOriginalName();
        $path        = Config::get('image.upload_dir') . '/' . $dir . '/' . $filename;
        $uploaded    = $file-&gt;move($destination, $filename);
 
        if ($uploaded)
        {
            if ($createDimensions) $this->createDimensions($path);
 
            return $path;
        }
    }
	}

当我们成功上传图片后，就会调用**createDimensions()**方法来剪裁图片。我们需要在配置文件中设置我们需要剪裁的尺寸。

**app/config/image.php**

    return array(
    'library'     => 'imagick',
    'upload_path' => public_path() . '/uploads/',
    'quality'     => 85,
 
    'dimensions' => array(
        'thumb'  => array(100, 100, true,  80),
        'medium' => array(600, 400, false, 90),
    ),
	);

接下来就是创建**createDimensions()**方法。把需要剪裁的尺寸放在设置文件里的**dimensions**这个数组里，然后循环数组里按照数组里的尺寸来剪裁图片。

    /**
	 * Creates image dimensions based on a configuration
	 * @param  string $url
	 * @param  array  $dimensions
	 * @return void
	 */
	public function createDimensions($url, $dimensions = array())
	{
	    // Get default dimensions
	    $defaultDimensions = Config::get('image.dimensions');
	 
	    if (is_array($defaultDimensions)) $dimensions = array_merge($defaultDimensions, $dimensions);
	 
	    foreach ($dimensions as $dimension)
	    {
	        // Get dimmensions and quality
	        $width   = (int) $dimension[0];
	        $height  = isset($dimension[1]) ?  (int) $dimension[1] : $width;
	        $crop    = isset($dimension[2]) ? (bool) $dimension[2] : false;
	        $quality = isset($dimension[3]) ?  (int) $dimension[3] : Config::get('image.quality');
	 
	        // Run resizer
	        $img = $this->resize($url, $width, $height, $crop, $quality);
	    }
	}

当然你可以指定任意的尺寸来覆盖配置文件里的尺寸来剪裁图片。




    