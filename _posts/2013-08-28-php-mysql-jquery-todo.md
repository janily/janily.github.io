---
layout: post
title: php学习-用PHP、MYSQL和jQuery做一个任务管理系统
categories:
- Life
tags:
- php
- jQuery
- 翻译
---

在这篇教程中，我们用PHP、MYSQL和jQuery做一个任务管理系统,用到了AJAX开发思路。在这个过程中，也将会用到PHP中的面向对象的开发思想。在用户界面上，用Jquery UI来处理诸如AJAX的一些交互。

为了更好地理解我们的例子，先来看下最终的运行效果[这里](http://demo.tutorialzine.com/2010/03/ajax-todo-list-jquery-php-mysql-css/demo.php)。

## PHP ##

这个教程更多的是面向有一定开发经验的人的，如果你还不是很熟悉PHP中的一些概念建议你去相关的文档查看下，这里提供一些链接。[面向对象](http://php.net/manual/zh/language.oop5.basic.php), [静态方法](http://php.net/manual/zh/language.oop5.static.php), [异常处理类](http://php.net/manual/zh/language.exceptions.php), [魔术方法](http://php.net/manual/zh/language.oop5.magic.php#language.oop5.magic.tostring)。

任务管理系统的主要功能是，编辑、创建、删除、排序等功能，依据这些功能，要创建对应的方法，如下所示

    /* 定义一个ToDo类 */

	class ToDo{

    /* 定义一个data变量，用来存储任务 */

    private $data;

    /* 构造函数允行开发者在一个类中定义一个方法作为构造函数。具有构造函数的类会在每次创建新对象时先调用此方法，所以非常适合在使用对象之前做一些初始化工作。 */
    public function __construct($par){
        if(is_array($par))
            $this->data = $par;
    }

    /*
        PHP中的"魔术方法"（Magic methods）。在命名自己的类方法时不能使用这些方法名，除非是想使用其魔术功能。
    */

    public function __toString(){

        // The string we return is outputted by the echo statement

        return '
            <li id="todo-'.$this->data['id'].'" class="todo">

                <div class="text">'.$this->data['text'].'</div>

                <div class="actions">
                    <a href="" class="edit">Edit</a>
                    <a href="" class="delete">Delete</a>
                </div>

            </li>';
    }

上面代码中的构造方法中将数组作为参数传递，并将存储在$data变量中。该数组将用mysql_fetch_assoc() 方法从数据库里面把ID和任务名取出来。

这里用到了一个魔术方法 __toString()，__toString() 方法用于一个类被当成字符串时应怎样回应。例如 echo $obj; 应该显示些什么。此方法必须返回一个字符串，否则将发出一条 E_RECOVERABLE_ERROR 级别的致命错误。例如

    <?php
	// Declare a simple class
	class TestClass
	{
	    public $foo;
	
	    public function __construct($foo) 
	    {
	        $this->foo = $foo;
	    }
	
	    public function __toString() {
	        return $this->foo;
	    }
	}
	
	$class = new TestClass('Hello');
	echo $class;
	?>

以上例程会输出：

	Hello

那在todo的类中的__toString()，将会把data中数据取出来，对应的显示在我们写好的字符串中，这里是一个li标签，并且好包含了两个用于操作的链接。如图所示：

![](http://pic.yupoo.com/reicky_v/D7tMM0HT/AtJTk.png)

接着继续来看代码

    /*
        编辑方法是用来编辑任务的，并保存到数据库中
    */

    public static function edit($id, $text){

        $text = self::esc($text);
        if(!$text) throw new Exception("Wrong update text!");

        mysql_query("	UPDATE tz_todo
                        SET text='".$text."'
                        WHERE id=".$id
                    );

        if(mysql_affected_rows($GLOBALS['link'])!=1)
            throw new Exception("Couldn't update item!");
    }

    /*
        删除方法是用来删除数据的
    */

    public static function delete($id){

        mysql_query("DELETE FROM tz_todo WHERE id=".$id);

        if(mysql_affected_rows($GLOBALS['link'])!=1)
            throw new Exception("Couldn't delete item!");
    }

    /*
        排序方法是用来对任务列表排序的，并把排序的id保存到数据库
    */

    public static function rearrange($key_value){

        $updateVals = array();
        foreach($key_value as $k=>$v)
        {
            $strVals[] = 'WHEN '.(int)$v.' THEN '.((int)$k+1).PHP_EOL;
        }

        if(!$strVals) throw new Exception("No data!");

        // We are using the CASE SQL operator to update the ToDo positions en masse:

        mysql_query("	UPDATE tz_todo SET position = CASE id
                        ".join($strVals)."
                        ELSE position
                        END");

        if(mysql_error($GLOBALS['link']))
            throw new Exception("Error updating positions!");
    }

这里定义了几个任务的操作方法，这些方法不需要实例化对象来访问，你可以随时用ToDo::edit($par1,$par2)方法来调用你想用的方法。

### 相关方法说明 ###

注意一下，我们这里用到了exception这个异常处理方法，异常（Exception）用于在指定的错误发生时改变脚本的正常流程。当异常被触发时，通常会发生

- 当前代码状态被保存
- 代码执行被切换到预定义的异常处理器函数
- 根据情况，处理器也许会从保存的代码状态重新开始执行代码，终止脚本执行，或从代码中另外的位置继续执行脚本

异常的基本使用 

    <?php
	//create function with an exception
	function checkNum($number)
	 {
	 if($number>1)
	  {
	  throw new Exception("Value must be 1 or below");
	  }
	 return true;
	 }
	
	//trigger exception
	checkNum(2);
	?>

会输出

    Fatal error: Uncaught exception 'Exception' 
	with message 'Value must be 1 or below' in C:\webfolder\test.php:6 
	Stack trace: #0 C:\webfolder\test.php(12): 
	checkNum(28) #1 {main} thrown in C:\webfolder\test.php on line 6

mysql_affected_rows方法取得前一次 MySQL 操作所影响的记录行数，语法如下

    mysql_affected_rows(link_identifier)

link_identifier：必需。MySQL 的连接标识符。如果没有指定，默认使用最后被 mysql_connect() 打开的连接。如果没有找到该连接，函数会尝试调用 mysql_connect() 建立连接并使用它。如果发生意外，没有找到连接或无法建立连接，系统发出 E_WARNING 级别的警告信息。取得最近一次与 link_identifier 关联的 INSERT，UPDATE 或 DELETE 查询所影响的记录行数。

执行成功，则返回受影响的行的数目，如果最近一次查询失败的话，函数返回 -1。

当使用 UPDATE 查询，MySQL 不会将原值与新值一样的列更新。这样使得 mysql_affected_rows() 函数返回值不一定就是查询条件所符合的记录数，只有真正被修改的记录数才会被返回。

这里还用到了mysql中的when和case语法，详情可以去mysql官方文档[看看](http://dev.mysql.com/doc/refman/5.0/en/case-statement.html)。

## 第三部分todo代码 ##

    /*
        创建方法，是新增任务的方法，把相关数据保存到数据库，然后用ajax的方法显示到前端
    */

    public static function createNew($text){

        $text = self::esc($text);
        if(!$text) throw new Exception("Wrong input data!");

        $posResult = mysql_query("SELECT MAX(position)+1 FROM tz_todo");

        if(mysql_num_rows($posResult))
            list($position) = mysql_fetch_array($posResult);

        if(!$position) $position = 1;

        mysql_query("INSERT INTO tz_todo SET text='".$text."', position = ".$position);

        if(mysql_affected_rows($GLOBALS['link'])!=1)
            throw new Exception("Error inserting TODO!");

        // Creating a new ToDo and outputting it directly:

        echo (new ToDo(array(
            'id'	=> mysql_insert_id($GLOBALS['link']),
            'text'	=> $text
        )));

        exit;
    }

    /*
        格式化字符串
    */

    public static function esc($str){

        if(ini_get('magic_quotes_gpc'))
            $str = stripslashes($str); //stripslashes() 函数删除由 addslashes() 函数添加的反斜杠。

        return mysql_real_escape_string(strip_tags($str));//mysql_real_escape_string() 函数转义 SQL 语句中使用的字符串中的特殊字符。
    }

上面代码中用到了self::这个关键字，self是指向类本身，也就是PHP self关键字是不指向任何已经实例化的对象，一般self使用来指向类中的静态变量。
并且中间使用"::"来连接，就是我们所谓的域运算符。

我们还定义了一个esc()方法来格式化用户输入的字符串。

这里我们用到了mysql_insert_id这个方法，mysql_insert_id() 函数返回上一步 INSERT 操作产生的 ID。如果上一查询没有产生 AUTO_INCREMENT 的 ID，则 mysql_insert_id() 返回 0。

好了类写完了，我们来看看怎么来使用这个类。新建一个demo.php的文件

    //从数据库中取出数据
	$query = mysql_query("SELECT * FROM `tz_todo` ORDER BY `position` ASC");
	
	$todos = array();
	
	// 实例化一个ToDo类出来，把数据写入到$todo中
	
	while($row = mysql_fetch_assoc($query)){
	    $todos[] = new ToDo($row);
	}

	foreach($todos as $item){
    echo $item;
	}

当然，我们得在demo.php中载入todo.class.php文件，这样我们才能使用ToDo类中的方法。接着我们用循环把数据取出来，还记得我们上面用到了__toString() 方法吗？它会把我们的数据按照一定的格式显示出来。

对于前段跟后端我们采用的是AJAX方法来通信，我们把所有AJAX方法写在一个文件里，就叫ajax.php。如下所示

    $id = (int)$_GET['id'];

	try{

    switch($_GET['action'])
    {
        case 'delete':
            ToDo::delete($id);
            break;

        case 'rearrange':
            ToDo::rearrange($_GET['positions']);
            break;

        case 'edit':
            ToDo::edit($id,$_GET['text']);
            break;

        case 'new':
            ToDo::createNew($_GET['text']);
            break;
    }

	}
	catch(Exception $e){
	//	echo $e->getMessage();
	    die("0");
	}
	
	echo "1";

我们这里用到了Try, throw 和 catch语法来处理我们的错误。处理程序应包括

- Try - 使用异常的函数应该位于 "try" 代码块内。如果没有触发异常，则代码将照常继续执行。但是如果异常被触发，会抛出一个异常。
- Throw - 这里规定如何触发异常。每一个 "throw" 必须对应至少一个 "catch"
- Catch - "catch" 代码块会捕获异常，并创建一个包含异常信息的对象

通过从exception对象调用 $e->getMessage()，输出来自该异常的错误消息。

## MySQL ##

我们创建一个 tz_todo的数据表，来保存我们的添加的数据。包含下面这些字段

![](http://pic.yupoo.com/reicky_v/D7ujESCE/6Zw1D.png)

唯一的自增索引值、任务位置字段、任务内容、时间四个字段。在下载源文件中有已经写好了的sql文件，当然如果是在你自己的机子上运行的话，记得要修改下connect.php这个文件中的相关数据库的信息。

## HTML ##

主要的内容部分，我们已经用PHP生成了，但是还是要一个基本html页面。首先我们得引入 jQuery, jQuery UI相关的js和css文件。当然得遵循一些开发中的好的实践。比如在头部引入css文件，在底部引入js文件如下所示

    <link rel="stylesheet" href="jquery-ui.css" type="text/css" />
	<link rel="stylesheet" type="text/css" href="styles.css" />
	
	<script type="text/javascript" src="jquery.min.js"></script>
	<script type="text/javascript" src="jquery-ui.min.js"></script>
	<script type="text/javascript" src="script.js"></script>

接着是主要内容部分的结构

    <div id="main">

    <ul class="todoList">

    <?php

        // Looping and outputting the $todos array. The __toString() method
        // is used internally to convert the objects to strings:

        foreach($todos as $item){
            echo $item;
        }

        ?>

    </ul>

    <a id="addButton" class="green-button" href="">Add a ToDo</a>

	</div>
	
	<!-- 这个div是用于弹出框的 -->
	<div id="dialog-confirm" title="Delete TODO Item?">Are you sure you want to delete this TODO item?</div>

每一个任务将会循环输出在li标签中，我们将用到jquery ui中sortable方法来进行排序。

## CSS ## 

    ul.todoList{
    margin:0 auto;
    width:500px;
    position:relative;
	}
	
	ul.todoList li{
	    background-color:#F9F9F9;
	    border:1px solid #EEEEEE;
	    list-style:none;
	    margin:6px;
	    padding:6px 9px;
	    position:relative;
	    cursor:n-resize;
	
	    /* CSS3 text shadow and rounded corners: */
	
	    text-shadow:1px 1px 0 white;
	
	    -moz-border-radius:6px;
	    -webkit-border-radius:6px;
	    border-radius:6px;
	}
	
	ul.todoList li:hover{
	    border-color:#9be0f9;
	
	    /* CSS3 glow effect: */
	    -moz-box-shadow:0 0 5px #A6E5FD;
	    -webkit-box-shadow:0 0 5px #A6E5FD;
	    box-shadow:0 0 5px #A6E5FD;
	}
	/* The edit textbox */

	.todo input{
	    border:1px solid #CCCCCC;
	    color:#666666;
	    font-family:Arial,Helvetica,sans-serif;
	    font-size:0.725em;
	    padding:3px 4px;
	    width:300px;
	}
	
	/* The Save and Cancel edit links: */
	
	.editTodo{
	    display:inline;
	    font-size:0.6em;
	    padding-left:9px;
	}
	
	.editTodo a{
	    font-weight:bold;
	}
	
	a.discardChanges{
	    color:#C00 !important;
	}
	
	a.saveChanges{
	    color:#4DB209 !important;
	}
    
样式这里就不详细说了，这里用到了一些CSS3样式，在一些老的浏览器就不支持了，主要是IE了。不过现在好多了，采取渐进增强不失为一种好的策略。

为了不让用户误删任务，我们还做了一个对话框，如下图所示

![](http://pic.yupoo.com/reicky_v/D7veOXWF/V5vqJ.png)

## jQuery ##

现在终于轮到javascript出场了。这里我们用到了jQuery UI中的sortable, 和 dialog方法。

    $(document).ready(function(){
    /* 运用了jquery ui的 sortable方法排序*/

    $(".todoList").sortable({
        axis		: 'y',				// Only vertical movements allowed
        containment	: 'window',			// Constrained by the window
        update		: function(){		// The function is called after the todos are rearranged

            // The toArray method returns an array with the ids of the todos
            var arr = $(".todoList").sortable('toArray');

            // Striping the todo- prefix of the ids:

            arr = $.map(arr,function(val,key){
                return val.replace('todo-','');
            });

            // Saving with AJAX
            $.get('ajax.php',{action:'rearrange',positions:arr});
        }
    });

    // 定义一个变量
    // 保存当前编辑的任务

    var currentTODO;

    // Configuring the delete confirmation dialog
    $("#dialog-confirm").dialog({
        resizable: false,
        height:130,
        modal: true,
        autoOpen:false,
        buttons: {
            'Delete item': function() {

                $.get("ajax.php",{"action":"delete","id":currentTODO.data('id')},function(msg){
                    currentTODO.fadeOut('fast');
                })

                $(this).dialog('close');
            },
            Cancel: function() {
                $(this).dialog('close');
            }
        }
    });

这里主要是用到了jquery中的ajax方法来提交信息到后台。

接下来看看，有关任务操作事件的绑定和后台通信的代码，也是运用ajax方法。

     // 双击任务的时候给它绑定单击事件
    $('.todo').live('dblclick',function(){
        $(this).find('a.edit').click();
    });

    // 当单击的时候任务里的链接的时候，保存当前的任务到currentTODO变量中
    // 

    $('.todo a').live('click',function(e){

        currentTODO = $(this).closest('.todo');
        currentTODO.data('id',currentTODO.attr('id').replace('todo-',''));

        e.preventDefault();
    });

    // 监听删除事件触发dialog事件，弹出弹出框:

    $('.todo a.delete').live('click',function(){
        $("#dialog-confirm").dialog('open');
    });

    // 监听编辑事件

    $('.todo a.edit').live('click',function(){

        var container = currentTODO.find('.text');

        if(!currentTODO.data('origText'))
        {
            // 保存当前编辑好的值到currentTODO中
            // 

            currentTODO.data('origText',container.text());
        }
        else
        {
            // 如果已经点击编辑按钮，阻止用户第二次点击，防止重复提交信息
            return false;
        }

        $('<input type="text">').val(container.text()).appendTo(container.empty());

        // 添加保存和取消按钮
        container.append(
            '<div class="editTodo">'+
                '<a class="saveChanges" href="">Save</a> or <a class="discardChanges" href="">Cancel</a>'+
            '</div>'
        );

    });

这里用到jquery中的live来绑定事件，主要是利用了事件冒泡的原理，这样即使新添加的元素，也会绑定点击事件。当然在最新版的jquery中，提倡用on代替live来绑定事件。

继续来看看下，其它的操作任务的事件绑定的代码

      // 取消和编辑事件绑定

    $('.todo a.discardChanges').live('click',function(){
        currentTODO.find('.text')
                    .text(currentTODO.data('origText'))
                    .end()
                    .removeData('origText');
    });

    // 保存事件绑定:

    $('.todo a.saveChanges').live('click',function(){
        var text = currentTODO.find("input[type=text]").val();

        $.get("ajax.php",{'action':'edit','id':currentTODO.data('id'),'text':text});

        currentTODO.removeData('origText')
                    .find(".text")
                    .text(text);
    });

    // 新增任务事件绑定

    var timestamp;
    $('#addButton').click(function(e){

        // Only one todo per 5 seconds is allowed:
        if(Date.now() - timestamp<5000) return false;

        $.get("ajax.php",{'action':'new','text':'New Todo Item. Doubleclick to Edit.'},function(msg){

            // 添加新任务的结构到页面中去
            $(msg).hide().appendTo('.todoList').fadeIn();
        });

        // 更新时间
        timestamp = Date.now();

        e.preventDefault();
    });

	});

## 总结 ##

今天通过一个任务管理程序，学习了ajax和后台程序通讯的编程方法，熟悉了PHP和mysql的一些方法，这仅仅是开始。

这篇文章主要参考了[AJAX-ed Todo List With PHP, MySQL & jQuery](http://tutorialzine.com/2010/03/ajax-todo-list-jquery-php-mysql-css/)这篇文章。不过文章中我添加大部分关于一些PHP中方法的说明。和原文还是有蛮多差别的。

    

    

