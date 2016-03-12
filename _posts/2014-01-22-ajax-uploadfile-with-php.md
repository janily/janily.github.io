---
layout: post
title: 在laravel4中用Ajax的方式来上传文件
categories:
- Life
tags:
- laravel
- php 
---

今天这篇文章来学习用laravel4和[Dropzone.js](http://www.dropzonejs.com/)这个jQuery插件来编写实现一个文件上传的功能。

Dropzone.js这个插件非常不错，支持最新的HTML5的拖拽上传并且还有提供上传进度等丰富的API。并且对于一些比较老的的浏览器也提供基本上传的功能。

浏览器支持如下所示：

- Chrome 7+
- Firefox 4+
- IE 10+
- Opera 12+ (Version 12 for MacOS is disabled because their API is buggy)
- Safari 6+

详细的使用方法可以去官方的[文档地址](https://github.com/enyo/dropzone)看看。

开始之前我们需要把dropzone插件的相关文件先下下来。

具体怎么在laravel4中怎么来操作这里就不展开阐述了，前面的文章都讲到过。文件下载好后，需要在视图模板里面引入**dropzone**相关的js和css文件代码如下：

    {{ HTML::style('css/dropzone.css')}}
	{{ HTML::script('js/dropzone.js')}}

我这里新建了一个用来上传的视图文件由于这里只是用来练习就直接用内联的方式来写相关的样式，代码如下：

    <!doctype html>
	<html lang="en">
	<head>
	<meta charset="UTF-8">
	<title>Laravel PHP Framework upload</title>
	<style>
		#dropzone {
		  width: 500px;
		  margin: 30px auto;
		  -webkit-box-shadow: 0 0 50px rgba(0,0,0,0.13);
		  box-shadow: 0 0 50px rgba(0,0,0,0.13);
		  padding: 4px;
		  -webkit-border-radius: 3px;
		  border-radius: 3px;
		}
		#dropzone .dropzone {
		  -webkit-box-shadow: none;
		  box-shadow: none;
		}
		.dropzone .dz-default.dz-message {
		  opacity: 1;
		  -ms-filter: none;
		  filter: none;
		  -webkit-transition: opacity 0.3s ease-in-out;
		  -moz-transition: opacity 0.3s ease-in-out;
		  -o-transition: opacity 0.3s ease-in-out;
		  -ms-transition: opacity 0.3s ease-in-out;
		  transition: opacity 0.3s ease-in-out;
		  background-repeat: no-repeat;
		  background-position: 0 0;
		  position: absolute;
		  width: 428px;
		  height: 123px;
		  margin-left: -214px;
		  margin-top: -61.5px;
		  top: 50%;
		  left: 50%;
		}
		.dropzone .dz-default.dz-message span {
			display: block !important;
			text-align: center;
			font-size: 28px;
		}
	</style>
	&#123;&#123;HTML::style('css/dropzone.css')&#125;&#125;
	&#123;&#123;HTML::script('js/dropzone.js')&#125;&#125;
	</head>
	<body>
		<div id="dropzone">
			<form action="{{ url('/upload')}}" class="dropzone dz-clickable" id="demo-upload">
				<div class="dz-default dz-message"><span>Drop files here to upload</span></div>
			</form>
		</div>
	</body>
	</html>

在上面的视图文件中需要注意的是用dropzone处理完文件上传之后需要交给后端的php文件来处理，所以我们需要指定处理上传文件的php方法可以使用路由或者是控制器来指定文件上传的方法。

我这里是指定在**HomeController**控制器里的**postUpload**方法方法来处理的，在这个方法里我们需要完成下面几件事情：

- 验证上传的文件类型是否符合我们指定的文件类型，比如我们这里只允许上传图片文件。以及文件的大小是否符合我们指定的文件大小。
- 如果验证通过，则上传文件并且重命名上传的文件。
- 如果验证没有通过则发挥一个400的错误提示。
- 如果验证通过，则返回一个成功的提示(200)。
- 如果是服务端出现错误就返回一个服务端的错误信息。

按照上面的逻辑我们来编写**postUpload**这个方法，代码如下：

    public function post_upload(){
 
	    $input = Input::all();
	    $rules = array(
	        'file' => 'image|max:3000',
	    );
	 
    	$validation = Validator::make($input, $rules);
 
    	if ($validation->fails())
	    {
	        return Response::make($validation->errors->first(), 400);
	    }
 
	    $file = Input::file('file');
	 
		$destinationPath = 'uploads/'.str_random(8);
		$filename = $file->getClientOriginalName();
		//$extension =$file->getClientOriginalExtension(); 
		$upload_success = Input::file('file')->move($destinationPath, $filename);
		 
		if( $upload_success ) {
		   return Response::json('success', 200);
		} else {
		   return Response::json('error', 400);
		}
	}

现在我们就可以来上传文件了，如下图所示：

![](http://pic.yupoo.com/reicky_v/DtPKOl8z/medium.jpg)

    
