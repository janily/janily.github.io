---
layout: post
title: Django web开发尝鲜
categories:
- Life
tags:
- python
- Django
- web开发

---

> 周末花了点时间，熟悉了Django这个web开发框架的一些基本知识，开发一个web，最基本的无非就是数据的**增删查改**。顺便就纪录下来。


开始之前当然是要把相关的环境安装好，这里就不再详细说了，推荐去这个[地址](https://www.gitbook.com/book/andrew-liu/django-blog)看下。这个教程非常不错，用来入门Django;同时官方文档也是要经常去的，写的非常好。[中文地址](http://python.usyiyi.cn/django/intro/tutorial01.html)，[英文地址](https://docs.djangoproject.com/en/1.9/)推荐使用**[PyCharm](https://www.jetbrains.com/pycharm/download/)**，不管是调试代码、还是智能提示等常见的IDE功能它都有，专门为python开发而打造的，非常好用。

这里只是着重的想提醒下，如果你不想再配置环境方面花时间，推荐使用Linux系统或者是Mac系统来进行开发，配置各种开发环境省心省力，可以把精力集中重要的事情上，而不是在环境上折腾。虽然现在windows在对开发者对友好度也改善了很多，但还是不够，你懂的。

这篇文章主要是来讲讲Django中数据的增删查改的，也是web开发中最基本的功能。

今天要实现的一个小的应用就是，添加数据并显示。可以对数据进行修改和删除，如下示意图所示：

![](http://pic.yupoo.com/reicky_v/F9aQCTo9/NyEQp.png)

[源代码地址](https://github.com/janily/dataFunny)


### 项目创建

正式开始，我创建一个名为**databaseFun**的Django项目。

在Django中，使用命令可以很方便的创建项目。创建指令如下：


```
$ django-admin.py startproject databaseFun

```

运行命令后，Django会为我们创建一个基本的项目文件目录和一些基础文件。

### 建立Django app

>在Django中，不通的功能是以app形势存在的。这是跟其它web开发框架一个很大的区别，将不通功能放在单独的app中，非常方便代码或者是功能的复用。

运行下面的命令来新建一个**database**的app:


```
python manage.py startapp database

```

建立好app后，还需要到配置文件中(databaseFun/databaseFun/setting.py)注册我们的app：


```
INSTALLED_APPS = (
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'database',
)

```

### 数据库

接下来是配置数据库来，Django支持各种数据库。Django默认是实用**SQLite**这个轻量级的数据库，无需任何的设置以及安装，默认集成在系统里。开发一些轻量级的应用使用非常方便。

在**/databaseFun/databaseFun/setting.py**文件中可以来配置数据库设置。

像Django这类的web开发框架基本上都会带有**Model**这个功能，有了它我们就不需要写**sql**语句来跟数据库打交道了。

在databaseFun/database/models.py下编写如下代码:

```
from django.db import models
from django import forms

# Create your models here.


class Artist(models.Model):
    name = models.CharField("artist",max_length=50)
    year_formed = models.PositiveIntegerField()


class ArtistForm(forms.ModelForm):
    class Meta:
        model = Artist;
        fields = ['name','year_formed'];
```

CharField 用于存储字符串, max_length设置最大长度。

PositiveIntegerField 用于存储数字，但必须是正值，不能是负数。

这里还用到了Django提供的表单类的方法来生成表单。在Django中，表单的方法足够应付日常的开发需求。

我们这里指定表单类生成来两个文本输入框。在后面的模版中，我们就不用再写html，直接引用表单方法，就可以生成对应的结构。

相关表单详细的介绍信息可以去这个[地址](http://python.usyiyi.cn/django/topics/forms/index.html)仔细看看。

### 同步数据库

编写好数据模型文件后，运行下面的命令来创建相关的表：

```
$ python manage.py makemigrations

```

然后运行下面的命令来同步数据库文件：

```
$ python manage.py migrate

```

直接可以进入Django中的交互式shell中进行数据的增删查改：

```
$ python manage.py shell
Python 2.7.10 (default, Aug 22 2015, 20:33:39) 
[GCC 4.2.1 Compatible Apple LLVM 7.0.0 (clang-700.0.59.1)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
(InteractiveConsole)
>>> 

```

下面，我们实际来进行下数据的操作：

首先是数据的增加操作：

```
>>> from database.models import Artist
>>> newArtist = Artist(name="janily",year_formed=2015)
>>> newArtist.save()
>>> newArtist.year_formed = 1987
>>> newArtist.save()

```

然后是数据的查询：

```
>>> allArtists = Artist.objects.all()
>>> allArtists[0].id
1
>>> allArtists[0].name
u'janily'
>>> for artist in allArtists:
...     print(artist.name)
... 
janily

```

更详细的可以去官方的文档查看。

数据已经准备就绪，接下来就是路由的配置和视图(Views)函数的编写了。

### 路由和Views

首先来说明下网页程序的逻辑

**request进来->从服务器获取数据->处理数据->把网页呈现出来**。

路由设置相当于客户端向服务器发出request请求的入口, 并用来指明要调用的程序逻辑；

views用来处理程序逻辑, 然后呈现到template(一般为GET方法, POST方法略有不同)；

template一般为html+CSS的形式, 主要是呈现给用户的表现形式。

所以我们首先来设置我们的路由，即不同的请求调用不同的逻辑。

我们这个简单的应用主要有这么几个逻辑：

1、增加数据

2、显示数据

3、显示数据详情

4、编辑数据

5、删除数据

在**databaseFun/databaseFun/urls.py**文件中编写以下代码：

![](http://pic.yupoo.com/reicky_v/F9pNwBat/KaV2F.png)

路由函数有四个参数, 两个是必须的:regex和view, 两个可选的:kwargs和name。

Django中的路由使用正则表达式来匹配，直到匹配到为止。当匹配到的时候，就会调用database/views.py中的对应的视图函数。

**^(?P<id>\d+)/$** 这个正则表达式的意思是将传入的一位或者多位数字作为参数传递到views中作为参数, 其中?P<id>定义名称用于标识匹配的内容。我们这里是传送id的值给对应的视图函数来处理。

下面我们来针对路由中的处理逻辑来编写对应的处理方法，在**databaseFun/database/views.py**编写以下代码：


```
from django.shortcuts import render,render_to_response
from django.http import HttpRequest,HttpResponse,HttpResponseRedirect
from django.template import RequestContext
from django.http import Http404
from database.models import *

# Create your views here.

# 添加数据方法
def create(request):
    if request.method == "GET":
        form = ArtistForm();
        return render(request ,'create.html',{'form':form});
    elif request.method == "POST":
        form = ArtistForm(request.POST);
        form.save();
        return HttpResponseRedirect('/artists');

# 编辑数据方法
def update(request, id=''):
    form = ArtistForm(request.POST)

    if form.is_valid():
        artist = Artist.objects.get(id = id)
        form = ArtistForm(request.POST,instance= artist)
        form.save()
        return HttpResponseRedirect('/artists')
    else:
        artist = Artist.objects.get(id = id)
        form = ArtistForm(instance=artist)
        return render_to_response('update.html',{ 'form':form }, context_instance=RequestContext(request))


#删除数据方法
def delete(request, id=''):
    try:
        artist = Artist.objects.get(id=id)
    except Exception:
        raise Http404
    if artist:
        artist.delete()
        return HttpResponseRedirect('/artists')

#展示数据方法
def artists(request):
    artists = Artist.objects.all();
    return render_to_response('artist.html', {'artists': artists})

#数据详情方法
def detail(request, id=''):
    try:
        artist = Artist.objects.get(id = id)
    except Artist.DoesNotExist:
        raise Http404
    return render_to_response('details.html', {'artist': artist})

```

### 模版

数据以及业务处理逻辑都准备好了，接下来就是前台展示了。

首先在**database**文件夹下新建一个**templates**文件夹，用来放置html模板文件。

我们这里需要新建四个模板文件：

1、显示数据用的

2、显示数据详情用的

3、新增数据用的

4、编辑数据用的

新建database/artist.html文件，用来显示数据用的模板文件：

![](http://pic.yupoo.com/reicky_v/F9pNxAza/n4CiV.png)

这里使用了for循环语句来取出所有的数据并显示在页面上，还可以对数据进行编辑删除操作。

其它三个模板就不分别列出来了，可以去[这里](https://github.com/janily/dataFunny)查看详细的代码。

下面就来实际运行下吧！

在项目目录下运行下面的命令，启动web:


```
python manage.py runserver

```

打开[http://127.0.0.1:8000/artists/](http://127.0.0.1:8000/artists/)首先是进入首页，再没有数据的时候是空白页，如下图所示：

![](http://pic.yupoo.com/reicky_v/F9b8C6Hv/lzthE.png)

先来添加几条数据看看，打开[http://127.0.0.1:8000/artists/create/](http://127.0.0.1:8000/artists/create/)，如下图所示：

![](http://pic.yupoo.com/reicky_v/F9b8DpIU/gYIOR.png)

然后就回在首页显示刚刚添加的数据：

![](http://pic.yupoo.com/reicky_v/F9b8DIfT/NXqAA.png)

当然，你还可以对数据进行编辑修改，这里就不一一演示了。















