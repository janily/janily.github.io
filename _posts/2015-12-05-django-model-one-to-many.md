---
layout: post
title: Django中数据库操作一对多关系
categories:
- Life
tags:
- python
- Django
- web开发

---

在web开发中，经常要来定义数据之间的关系。比如一个作者对应多本书，一本书只有一个作者；或者是一个汽车出产商能出产很多汽车，但是一个辆汽车智能是属于一个汽车出产商。

在Django中，为我们提供了三种方法来定义数据之间的关系：

一对一；一对多；多对多这三种关系。足够应付绝大部分的开发需求。

下面我们还是以上一篇文章中的例子来实战下Django中一对多数据的操作。

首先更新模型中的代码，新建一个album的模型：


```
class Album(models.Model):
    name = models.CharField("album",max_length=50)
    artist = models.ForeignKey(Artist)

```

在上面的代码中可以看到在album模型中，我们定义了一个artist的外键。这样album就和artist之间产生了联系，即一个artist可以有多个album；而album只能是属于某一个对象。

在项目根目录运行下面的命令，进入shell命令界面来新建几条数据。

首先是artist：

先创建几条数据：


```
#这是插入artist
from database.models import Artist,Album
artist1 = Artist.objects.create(name="max",year_formed = 2080)
artist2 = Artist.objects.create(name = 'max_chen',year_formed = 2016)

＃这是album

album2 = Album.objects.create(name = 'me',artist = artist2)

```

在上面的我们代码中，我们将album指向来artist。

结合实例非常容易明白这里的关系；即一个artist可以有多个album；而Blum只能是属于单独的artist。

首先在路由文件中添加下面这行到没：

```
url(r'^albums/$', views.album, name='album'),

```

然后是，视图文件中的album方法，在views文件中新建一个album：

如下代码所示：


```
def album(request):
    try:
        print "XXXX"
        album_lists = Album.objects.all();
    except Artist.DoesNotExist:
        raise Http404
    return render_to_response('album.html', {'album_lists': album_lists})

```

这没什么好说的，主要是查询只一步，首先是正向查询，即查看artist下面有多少个album，只需要几行代码就能查询出来。

更新模板文件，artists.html文件，如下图所示：

![](http://pic.yupoo.com/reicky_v/F9up7yX6/mIEan.png)

可以看到我们使用循环把artist所拥有的album都显示出来，关键的是这句：


```
artist.album_set.all

```

访问[http://127.0.0.1:8000/artists/](http://127.0.0.1:8000/artists/)地址就可以看到对应artist显示了他所拥有的相册。

![](http://pic.yupoo.com/reicky_v/F9usoDPO/Qt3ul.png)

第二个，是反向查询，即通过album来查询某一个album是属于谁的，在平常开发中，这样的需求也非常多。在Django也非常简单，这里需要新建一个album.html的模版文件，如下所示：

![](http://pic.yupoo.com/reicky_v/F9ut7PAO/s2fX8.png)

运行下，访问[http://127.0.0.1:8000/albums/](http://127.0.0.1:8000/albums/)，就能看到结果：

![](http://pic.yupoo.com/reicky_v/F9uuOEMl/gCxRg.png)

非常简单，下篇文章再来看看更高级一点的应用。



