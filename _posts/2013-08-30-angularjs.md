---
layout: post
title: AngularJS学习初探(5)-一个小的通讯录实例教程
categories:
- Life
tags:
- javascript
- 翻译
---

Angularjs系列初步入门大大小小写了四篇，现在我们用它来做一个小小的通讯录管理程序，来进一步巩固下AngularJS的基础知识，这个通讯录包括名字、电话号码和地址。开始之前，我们先来创建一个html页面，如下

    <!doctype html>
	<html lang="en" ng-app="contactManager">
	<head>
	  <meta charset="utf-8">
	  <title>Contact Manager</title>
	</head>
	<body ng-controller="AppCtrl">
	  <h1>AngularJS Contact Manager</h1>
	  <!-- App template goes here -->
	  <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.0.1/angular.min.js"></script>
	  <script src="app.js">
	  </script><script src="controllers.js"></script>
	</body>
	</html>

当然，按照惯例还得添加一个app.js文件

    var contactManager = angular.module(&lsquo;contactManager&rsquo;, []);

接着是写控制器部分了.控制器是我们实现应用的主要部分，用来保存我们的数据:

    contactManager.controller('AppCtrl',function AppCtrl ($scope) {
    $scope.contacts = [{
      name: 'Brian Ford',
      phone: '555-555-5555',
      address: [
        '1600 Amphitheatre Parkway',
        'Mountain View, CA 94043'
      ]
    }];
  	});

正如前面例子里讲到的，我们把数据存在一个javascript对象了。angularJS不需要扩展一些javasscript里不存在的一些功能，或者是规定坚持某一种继承的设计模式，所以你可以自由地用你喜欢的模式，去扩展javascript的功能，比如用Underscore.js这个工具库来使用javascriptES5新标准中的功能。最主要的是保持代码的简洁和逻辑的简洁。

下面，让我们制作一个前端的显示的页面，把这段代码放到body中间，如下所示

    <p ng-show="contacts.length == 0">There are no contacts here!</p>
	<ul>
	  <li ng-repeat="(id, contact) in contacts">
	    <a href="#/info/{{id}}">{{contact.name}}</a>
	    [<a href="#/remove/{{id}}">remove</a>]
	  </li>
	</ul>
	<a href="#/add">Add new item</a>

上面代码中ng-repeat将会从contacts数组中遍历出相应的数据输出到对应的标签中。运行后会显示为下面的样子

![](http://pic.yupoo.com/reicky_v/D7OubHJa/ZtCn1.jpg)

注意到上面的代码中，当没有数据的时候，用了一个段落文本来提醒用户没有数据。用了 ng-show这个指令来实现的，这里用了一个条件，当没有数据的时候才显示文本，当有数据的时候就不会显示这个文本了。

现在让我们的通讯录程序增加一些增删查改的特性。那就得增加一些页面来实现这些功能。我们可以用angular中的ng-view和路由来管理这些页面。

开始之前，我们先新建一个名为protials的文件夹，然后对应我们的增删查改的功能来新建一些页面。然就是设置路由。把代码添加到app.js文件中，如下

    var contactManager = angular.module('contactManager', [])
  	.config(function($routeProvider) {
    $routeProvider.when('/index', {
      templateUrl: 'partials/index.html'
    })
    .when('/info/:id', {
      templateUrl: 'partials/info.html',
      controller: 'InfoCtrl'
    })
    .when('/add', {
      templateUrl: 'partials/add.html',
      controller: 'AddCtrl'
    })
    .when('/edit/:id', {
      templateUrl: 'partials/edit.html',
      controller: 'EditCtrl'
    })
    .when('/remove/:id', {
      templateUrl: 'partials/remove.html',
      controller: 'RemoveCtrl'
    })
    .otherwise({
      redirectTo: '/index'
    });
  	});

现在，我们的程序能够响应我们不同的动作，并且对应不同的ID，而其它的动作将会直接重定向到首页，而重定向到首页的这个功能不需要写任何的逻辑方面的代码，而增删查改需要写一些简单的功能代码来处理。

处理增删查改的功能代码，写到 controllers.js这个文件里，首先我们来写一个叫InfoCtrl的控制器，主要是用来控制显示详细信息的，如下：

    contactManager.controller('InfoCtrl',
  	function InfoCtrl($scope, $routeParams) {
    $scope.contact = $scope.contacts[$routeParams.id];
  	});

这里我们通过一个控制器的别名作为一个参数在传递给我们的路由，从而设置对应的的跳转。

在angular中，scope是控制器和视图之间的纽带。在scope中设置$watch表达式。$watch让directive能够得知属性的变化，从而将更新后的值渲染到DOM中。所以 contacts array 中值会显示到InfoCtrl控制器中的info.html模板中:

    <p>Name: {{contact.name}}</p>
	<p>Phone Number: {{contact.phone}}</p>
	<p ng-repeat="line in contact.address">{{line}}</p>

接着来写增加联系人这个功能，基于对应的路由的ID参数， 通过$scope.contact这个变量中对应的名字显示给用户。

     contactManager.controller('AddCtrl',
	  	function AddCtrl($scope, $location) {
	    $scope.contact = {};
	    $scope.add = function () {
	      $scope.contacts.push($scope.contact);
	      $location.url('/');
	    };
	  	}); 

这里有一个add方法，用push方法把数据存到contacts数组中。用到的模板是partials/add.html文件，而且编辑功能用到的模板和增加功能的模板是一样的，都是基于form表单来提交数据。

这里要用ng-include这个表达式，来引入我们的模板。比如在增加新的联系人的时候，要用到form.html这个模板，就可以用ng-include引用，如下

    <h2>Add a new Contact</h2>
	<form ng-submit="add()" ng-include="'partials/form.html'"></form>

form.html模板的具体内容如下

    <div>
	  <label>Name</label>: <input ng-model="contact.name">
	</div>
	<div>
	  <label>Phone Number</label>: <input ng-model="contact.phone">
	</div>
	<div>
	  <label>Address 1</label>: <input ng-model="contact.address.0">
	</div>
	<div>
	  <label>Address 2</label>: <input ng-model="contact.address.1">
	</div>
	<input type="submit" value="Submit">

添加这个模板后，当我们点击确定按钮时，就会触发add方法，从而把相关数据保存到contacts数组中。添加完用户后，add()方法就会重定向到index页面如下图这个样子

![](http://pic.yupoo.com/reicky_v/D7P5q6Wo/medium.jpg)

下面是编辑的控制器，它会把contact变量的作用初始化为一个空的对象。然后通过edit方法把编辑后的值传递给contacts数组保存起来。

接下来，是新建一个编辑用模板 partials/edit.html:

    <h2>Edit {{contact.name}}</h2>
	<form ng-submit="edit()" ng-include="'partials/form.html'"></form>

这里跟添加功能用到了同一个模板form.html,但是提交处理的方法是不一样的，这里是通过 ng-submit="edit()"来处理数据的。

最后我们来实现删除的功能，删除的控制器也会把contact变量的作用初始化为一个空的对象。然后通过remove方法把编辑后的值传递给contacts数组保存起来。

    contactManager.controller('RemoveCtrl',
  	function RemoveCtrl($scope, $routeParams, $location) {
    $scope.contact = $scope.contacts[$routeParams.id];
    $scope.remove = function () {
      $scope.contacts.splice($routeParams.id, 1);
      $location.url('/');
    };
    $scope.back = function () {
      $location.url('/');
    };
 	});

(译者注)在上面的代码中，有两个方法，remove方法是通过javascript中的splice方法来实现删除功能的，Array.splice具体语法是

    arrayObj .splice(start, deleteCount [, value0, value1...valueN])

- arrayObj必选项。一个 Array 对象。
- start必选项。指定从数组中移除元素的开始位置，这个位置是从 0 开始计算的。 
- deleteCount必选项。要移除的元素的个数。 
- item1, item2,. . .,itemN必选项。要在所移除元素的位置上插入的新元素。 

splice 方法可以移除从 start 位置开始的指定个数的元素并插入新元素，从而修改 arrayObj。返回值是一个由所移除的元素组成的新 Array 对象。 

这样就很轻松理解了删除方法中的代码。

当然也要添加我们的删除模板

    <p>Are you sure that you want to remove {{contact.name}}?</p>
	<button ng-click="remove()">Yes</button>
	<button ng-click="back()">No</button>

至此，一根简单的通讯录的应用就完成了。当然这里还有很多可以改进的，你可以用angular中的filters方法来给通讯录实现排序和搜索的功能。

当然这仅仅是用到angular的一些小的特性，就完成了一个功能完整的通讯录应用。angular是框架有一个强大的依赖注入系统，使得我们完全可以按照自己的需求来打造我们的应用。

因为angularjs的控制器也是一个javascript的函数，所以我们可以按需传递参数给它。此外，angular主要是用html标签和一些标记名称来完成依赖注入的功能，所以对于设计来说很容调整程序的功能。这种分离的开发方式对于开发者来说是非常一个好开发的体验。

这篇文章翻译自[Write an app in AngularJS](http://www.netmagazine.com/tutorials/write-app-angularjs)。





    