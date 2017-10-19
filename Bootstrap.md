# Bootstrap

[Bootstrap](http://getbootstrap.com/)  
[Bootstrap 中文网](http://www.bootcss.com/)  

### 基本页面
```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- The above 3 meta tags *must* come first in the head; any other head content must come *after* these tags -->
    <title>Bootstrap 101 Template</title>

    <!-- Bootstrap -->
    <link href="css/bootstrap.min.css" rel="stylesheet">

    <!-- HTML5 shim and Respond.js for IE8 support of HTML5 elements and media queries -->
    <!-- WARNING: Respond.js doesn't work if you view the page via file:// -->
    <!--[if lt IE 9]>
      <script src="https://oss.maxcdn.com/html5shiv/3.7.3/html5shiv.min.js"></script>
      <script src="https://oss.maxcdn.com/respond/1.4.2/respond.min.js"></script>
    <![endif]-->
  </head>
  <body>
    <h1>Hello, world!</h1>

    <!-- jQuery (necessary for Bootstrap's JavaScript plugins) -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js"></script>
    <!-- Include all compiled plugins (below), or include individual files as needed -->
    <script src="js/bootstrap.min.js"></script>
  </body>
</html>

```

### 基于网格的设计
能将网页按2, 3, 4等分的设计,采用12列网格
* 所有行必须在一个容器里或流体容器里
* 行内创建水平列组
* 内容在一列里,一个列必须在一个行中
* col-md-*, md表示medium, *代表一个数字,指定列宽
* col-xs-*, xs是extra small的缩写(应用于较小的屏幕,比如手机)

![12cols](http://i.imgur.com/FaYuui8.png)

container-fluid 流式内容
row
col-md-*
col-xs-*
class="well"	为列创造出一种视觉上的深度感

img-responsive 响应式图片

text-center 文本居中
text-left	文本左对齐
text-right	文本右对齐
text-uppercase	文本大写
text-primary	深蓝色文本

btn 按钮样式
class="btn btn-default"	默认样式
btn-block 使按钮独占一行
class="btn btn-primary"
btn-primary 深蓝色按钮,应用主要按钮
btn-info	浅蓝色按钮,常用操作
btn-danger	红色按钮,删除,提示危险操作等
class="form-control"	// 表单控件类,修饰表单文本input

Font Awesome图标库
```
<link rel="stylesheet" href="//cdn.bootcss.com/font-awesome/4.2.0/css/font-awesome.min.css"/>
```
i 元素起初一般是让其它元素有斜体(italic)的功能，不过现在一般用来指代图标。你可以将 Font Awesome 中的 class 属性添加到 i 元素中，把它变成一个图标，比如：
```
<i class="fa fa-info-circle"></i>	info图标
<i class="fa fa-thumbs-up"></i>		大拇指(赞,like)
<i class="fa fa-trash"></i>			垃圾桶(删除)
 <i class="fa fa-paper-plane"></i>	纸飞机(提交)
```

### Bootstrap 的模态交互
* 元素上添加 data-toggle="modal" data-target="你的元素ID"
* 网页引入 bootstrap.js 和 jquery.js
* 在网页的\</body>前添加 __modal__ 代码
```
<!-- Modal -->
<div class="modal fade" id="project1" tabindex="-1" role="dialog" aria-labelledby="myModalLabel" aria-hidden="true">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <h4 class="modal-title" id="myModalLabel">Favorite App Page</h4>
            </div>
            <div class="modal-body">
                <img class="img-responsive" src="images/555.jpeg">
                This was my first project in this class. I learned a lot about HTML and CSS.
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
            </div>
        </div>
    </div>
</div>
```