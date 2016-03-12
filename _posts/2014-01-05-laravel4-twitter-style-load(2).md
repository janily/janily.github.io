---
layout: post
title: 用laravel4和jQuery实现一个Ajax风格的加载更多的效果(下)
categories:
- Life
tags:
- laravel
- php
---

上一篇文章把整个程序的环境和测试数据都准备好了，下面就开始正式的编码了。

### 路由 ###

首先是把路由规划好，我们这里很简单，如下代码所示：

    Route::get('message','HomeController@showMessage');
	Route::post('more/{id}','HomeController@showMore');

就两个，一个是用来显示信息的由于是要从服务器拉去数据，所以我们的路由定义为**get**的方法。并且是执行**HomeController**这个控制器里的**showMessage**方法来显示数据。而第二个路由是用来执行Ajax请求加载数据的，需要根据客户端发送来的请求来发送相对应的数据，所以使用了post方法，并且url的路径是要传送一个ID给服务器来识别的，根据服务器发送来的ID执行**HomeController**控制器里的**showMore**方法来加载数据。

### 控制器 ###

路由规划好后，我们就需要来编写控制器以及相对应的方法了，首先在**app/controllers**目录下新建一个**HomeController**控制器,代码如下：

    <?php

	class HomeController extends BaseController {

		public function showMessage() {
		$message = DB::table('message')->select('message','id')->take(9)->orderBy('id','desc')->get();
		return View::make('home.message')->with('message',$message);
		
		}
	
		public function showMore($id) {
	
			if($id <= 1){
				return "";
			}else{
				$result= DB::table('message')->select('message','id')->take(9)->having('id', '<', $id)->orderBy('id','desc')->get();
				return View::make('home.ajaxmsg')->with('message',$result);
			}
		}
	}

第一个方法**showMessage**是用来从服务端拉去数据显示到前端的页面上，其实就是很简单的一个数据查询。利用laravel为我们提供的Eloquent数据模型我们再也不用写一串长长的sql查询语句了，**showMessage**方法主要是从数据库取出九条数据，而且是按照ID降序的方式来取出数据。然后就把取出的数据渲染到指定的视图里去。所以我们得在**app/views**目录里新建一个home的文件夹然后新建一个**message.blade.php**的视图文件，代码如下：

    @extends('layout.master')

	@section('content')
	<div id='container'>
	<ol class="timeline" id="updates">
	@foreach ($message as $message)
	  <li><span>{{$message->id}}</span>This is message {{ $message->message }}</li>
	@endforeach
	</ol>
	<div id="more{{$message->id}}" class="morebox">
	<a href="#" class="more" id="{{$message->id}}">more</a>
	</div>
	</div>
	@stop

首先是我们的视图文件需要引入我们前面建立的布局文件**layout.master**这个文件，然后插入我们的内容。我们这里使用laravel为我们提供的**foreach**语句来循环遍历我们从数据库中取出数据。除了这个之外，我们还有一个加载数据的按钮，并且把最后一条数据的id的值赋给了按钮作为id的值，后面加载数据的时候就需要根据这个id来加载数据。

由于我们是使用Ajax的方式来加载数据，所以我们还要编写javascript代码，如下所示：

    $(function() {
		$('#container').on("click",".more",function() 
		{
		var ID = $(this).attr("id");

		$("#more"+ID).html('<img src="/image/moreajax.gif" />');
	
		$.ajax({
			type: "POST",
			url: "/more/"+ID,
			data: "lastmsg:"+ ID, 
			cache: false,
			success: function(html){
				if(html == ""){
					$(".morebox").html('The End');// 没有结果
				}else{
					$("ol#updates").append(html);
					$("#more"+ID).remove(); // 删除按钮
				}
			}
	
		});
	
		return false;
                 

	});

我们这里是使用了jQuery的ajax方法，我们首先使用jQuery提供的**on**方法来给加载按钮绑定点击事件。首先获取按钮的id，插入事先准备好的加载动态的gif图片。然后使用jQuery的**Ajax**方法来提交我们获取的id，并且通过服务端返回的结果，这里我们需要做一个判断，如果服务端没有返回数据，则提示用户数据全部加载完毕；如果有数据则继续加载数据。通过路由发送给**HomeController**控制器中的**showMore($id)**方法，根据接受到的ID来从数据库中查询并取出相关数据，代码如下：

    public function showMore($id) {
		if($id <= 1){
			return "";
		}else{
			$result= DB::table('message')->select('message','id')->take(9)->having('id', '<', $id)->orderBy('id','desc')->get();
			return View::make('home.ajaxmsg')->with('message',$result);
		}
	}

这个方法根据客户端传过来的id进行一个判断，如果id小于或等于1，则返回一个空的数据；否则就返回查询到的数据，并调用**ajaxmsg.blade.php**这个视图来渲染数据，ajaxmsg.blade.php文件代码如下：

    @foreach ($message as $message)
	  <li><span>{{$message->id}}</span>This is message {{ $message->message }}</li>
	@endforeach
	<div id="more{{$message->id}}" class="morebox">
	<a href="#" class="more" id="{{$message->id}}">more</a>
	</div>

这里代码跟**message.blade.php**这个视图的代码差不多，只是在原先的视图文件中追加数据。

Ok，这个简单的Ajax风格式的加载效果就完成了，最终效果如下图所示：

![](http://pic.yupoo.com/reicky_v/DrehvDYA/medium.jpg)




