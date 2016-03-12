---
layout: post
title: 用laravel4和mailchimp来管理用户和推送邮件
categories:
- Life
tags:
- laravel
- php
- mailchimp
---

上篇文章提到了[mailchimp](http://mailchimp.com/)这个第三方的邮件服务，MailChimp是专注于为商业广告客户提供服务，帮助客户发送电子邮件简报，管理邮件订阅列表的这样一个邮件服务商。而且MailChimp还有一个特色就是详尽的统计功能，针对订阅用户进行的统计，可以让客户更好的进行营销管理等。

有付费和免费两个服务，一般而言对于刚开始的创业者来说免费服务也已经够用，一个月可以免费发送1万多封邮件，当然如果你是想用来进行营销的话那就是用付费服务吧。至于mailchimp的注册和使用方法这里就不再详细阐述了，这里推荐一篇关于mailchimp的注册和使用方法的文章可以去看看[Mailchimp注册指南](http://conwaykang.com/archives/49)。

今天主要来说说在Mailchimp结合laravel4的使用方法。有开发者把mailchimp的相关API封装好了，并且在laravel中可以采用composer方式直接安装使用非常方便。我们使用的是这个第三方包[Hugo Firth](http://github.com/hugofirth)

### 1.安装设置mailchimp ###

打开**composer.json**文件，在*require*里面添加mailchimp的依赖，如下所示：

    "require": {
		"laravel/framework": "4.0.*",
		"hugofirth/mailchimp": "2.0.*",
	},

然后在命令行里运行**composer update**命令来安装它。

    composer update

接着编辑**app/config/app.php**这个文件：

在**providers**数组里面把下面这行代码添加到底部：

    'Hugofirth\MailChimp\MailChimpServiceProvider'

还要在**aliases**数组里面添加下面的代码：

    'MailChimpWrapper' => 'Hugofirth\Mailchimp\Facades\MailchimpWrapper'

最后在命令行里面运行下面的命令来生成一个配置文件：

    php artisan config:publish hugofirth/mailchimp

它会生成一个配置文件，如下图所示：

![](http://pic.yupoo.com/reicky_v/DqkGE1eQ/medium.jpg)

需要我们把自己的 MailChimp API的key填到对应的位置。

具体key的位置是**Account Settings -> Exarts ->API Keys**

如下图所示：

![](http://pic.yupoo.com/reicky_v/DqkM4A2l/medium.jpg)

如果没有key的话，可以去创建一个。

上面设置完后，我们就可以在我们的应用里面使用MailChimp的API了，使用之前我们需要知道我们的**List ID**,可以在我们自己的MailChimp的账户里查找到这个ID。

在如下位置：

**Lists -> 你创建的list -> Settings -> List names & defaults**。

![](http://pic.yupoo.com/reicky_v/DqkPCqSb/medium.jpg)

我在使用的时候会报这样一个错误：

    API call to lists/subscribe failed: SSL certificate problem, verify that the CA cert is OK. Details: error:14090086:SSL

后来还是用谷歌搜到了解决方法，可以去这个[地址](http://redwebturtle.blogspot.com/2013/09/mailchimp-api-v20-ssl-error-solution.html)看看。

下面就可以使用MailChimp的API了，我们看几个示例：

添加订阅用户：

    MailchimpWrapper::lists()->subscribe($list_id, array('email' => $email_address));

如果你还想在添加订阅的时候把用户的名字信息也要添加到系统里面，可以使用下面的代码：

    MailchimpWrapper::lists()->subscribe($list_id, array('email' => $email_address), array('FirstName' => $first_name, 'LastName' => $last_name));

取消订阅则使用如下代码：

    MailchimpWrapper::lists()->unsubscribe($list_id, array('email' => $email_address));

也可以使用下面的代码获取特定成员的信息：

    MailchimpWrapper::lists()->memberInfo($list_id, array('email' => $email_address));

当然MailChimp的使用方法远不止这些，可以去官方的文档看看更多的使用方法。[MailChimp API](http://apidocs.mailchimp.com/api/2.0/)。

你可以去我们下载好的文件里面看看它提供了哪些方法：

    /vendor/mailchimp/mailchimp/src/Mailchimp

MailChimp确实非常不错，现在国内好多的公司都使用MailChimp来管理用户。基本的知识就介绍到这里。






 

