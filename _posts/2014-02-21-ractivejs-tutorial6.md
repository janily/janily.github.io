---
layout: post
title: 	ractive.js学习笔记(6)——条件判断
categories:
- Life
tags:
- javascript
- ractive.js
---

在程序开发中，程序的逻辑判断是一个常见的开发技巧。我们通过一个实例来学习在ractive中条件判断的使用方法。

假设你需要根据不同的用户权限来显示不同的视图，比如登录和未登录的用户显示不同的视图。

在下面这个实例，我们用ractivejs来实现一个简单的根据用户输入的信息来显示不同的视图文件。

新建一个condition.html文件，输入以下代码：

    <!doctype html>
	<html lang='en-GB'>
	<head>
	    <meta charset='utf-8'>
	    <title>ractivejs start</title>
	</head>
	
	<body>
    <h1>ractivejs start</h1>

    <div id='container'></div>

    <script id='myTemplate' type='text/ractive'>
        <h2>City profile</h2>

        {{#notSignedIn}}
          <!-- message for non-signed-in users -->
          <p>Hi there! Please <a href="#" class='button' on-click='signIn'>sign in</a></p>
        {{/notSignedIn}}

        {{#signedIn}}
          <!-- message for signed-in users -->
          <p>Welcome back, {{username}}!</p>
        {{/signedIn}}
    </script>

  	<script src='ractive.js'></script>
	<script>
        var ractive = new Ractive({
          el: 'container',
          template: '#myTemplate',
          data: {
            signedIn: false,
            notSignedIn: true
          }
        });

        ractive.on( 'signIn', function () {
          var name = prompt( 'Enter your username to sign in', 'ractive_fan' );

          ractive.set({
            username: name,
            signedIn: true,
            notSignedIn: false
          });
        });
    </script>
	</body>
	</html>

在浏览器中运行这个文件，会弹出一个对话框让你输入信息然后就会更新视图，显示欢迎用户的信息。

在上面的模板文件中我们是通过**signed**和**notSigned**来进行逻辑判断的，判断用户是否登录。

不过mustache也提供另外一种方式来做这样的判断：

    {{^signedIn}}<!--这里放置用户没有登录情况下输出的视图-->{{/signedIn}}

我们就可以使用它来代替上面代码中的：

    {{#notSigned}}

也可以达到同样的目的。

    {{^signedIn}}
	  <!-- message for non-signed-in users -->
	  <p>Hi there! Please <a class='button' on-tap='signIn'>sign in</a></p>
	{{/signedIn}}
	
	{{#signedIn}}
	  <!-- message for signed-in users -->
	  <p>Welcome back, {{username}}!</p>
	{{/signedIn}}

### True或者false判断 ###

实际上我们还可以更高效地来做一些逻辑判断方面的问题，用**true**或者是**false**来做这个。

上面的代码我们可以写的更精简。我们来定义一个**user**这个对象，并在**user**对象中设置name这个属性。只有在登录的情况下才会显示用户的名字。

我们可以把模板改为下面这样的代码：

    {{^user}}
	  <!-- message for non-signed-in users -->
	  <p>Hi there! Please <a class='button' on-tap='signIn'>sign in</a></p>
	{{/user}}
	
	{{#user}}
	  <!-- message for signed-in users -->
	  <p>Welcome back, {{name}}!</p>
	{{/user}}

然后把事件绑定的代码改为：

    ractive.on( 'signIn', function () {
	  var name = prompt( 'Enter your username to sign in', 'ractive_fan' );
	  ractive.set( 'user.name', name );
	});

同样可以达到同样的目的。

> 在上面的代码中我们设置了一个name属性。如果在对象中不存在这个属性，如果我们使用了**ractive.set('user[0]',name)**或者是**ractive.set('user.0,name)**，那Ractive.js会自动创建这个属性。

下篇继续学习ractive在表格中的应用。

