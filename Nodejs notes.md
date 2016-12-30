# Nodejs notes

Node.js是一个JavaScript运行环境(runtime). 它对Google V8引擎进行了封装. V8引擎执行JavaScript的速度非常快,性能非常好.Node.js对一些特殊用例进行了优化,提供了替代的API,使得V8在非浏览器环境下运行的更好.

Node.js是一个基于Chrome JavaScript运行时建立的平台,用于方便的搭建响应速度快,易于扩展的网络应用. Node.js使用事件驱动,非阻塞I/O模型而得以轻量和高效,非常适合在分布式设备上运行数据密集型的实时应用.

## 发展史

- 2009年2月, Ryan Dahl在博客上宣布准备基于V8创建一个轻量级的Web服务器并提供一套库.
- 2009年5月, Ryan Dahl在Github上发布了最初版本的部分Node.js包,随后几个月里,有人开始使用Node.js开发应用.
- 2009年11月和2010年4月,两届JSConf大会都安排了Node.js的讲座.
- 2010年年底, Node.js获得云计算服务商Joyent资助,创始人Ryan Dahl加入Joyent全职负责Node.js的发展.
- 2011年7月, Node.js在微软的支持下发布Windows版本.

## 安装

[Node.js官网](https://nodejs.org/en/)

## 初体验

起一个web服务器

	var http = require('http');
	
	http.createServer(function(req, res) {
	    res.writeHead(200, {'Content-Type': 'text/plain'});
	    res.end('Hello World\n');
	}).listen(8000);
	
	console.log('Server running at http://localhost:8000/');

## 命令行下的全局对象

- 在浏览器里是 __window__
- 在nodejs里是 __process__

## 模块
nodejs里每一个单独js文件都可以理解成一个模块,通过npm包管理工具又可以在项目里引入新的功能模块.

### 模块的分类

- 核心模块 http fs path...
- 文件模块 `var util = require('./util.js')`
- 第三方模块 `var promise = require('bluebird')`

#### 创建,导出,导入与使用模块的简单示例

student模块

	function add(student) {
	    console.log('add student: ' + student);
	}
	
	exports.add = add;

teacher模块

	function add(teacher) {
	    console.log('add teacher: ' + teacher);
	}
	
	exports.add = add;

klass(班级)模块

	var student = require('./student'),
	    teacher = require('./teacher');
	
	function add(teacherName, students) {
	    teacher.add(teacherName);
	
	    students.forEach(function(item) {
	        student.add(item);
	    });
	}
	
	exports.add = add;

学校模块

	var klass = require('./klass');
	
	exports.add = function(klasses) {
	    klasses.forEach(function(item) {
	        var teacherName = item.teacherName;
	        var students = item.students;
	        klass.add(teacherName, students);
	    });
	};

项目启动入口

	var school = require('./school');
	
	var klasses = [
	    {
	        teacherName: 'Houdi',
	        students: ['白富美', '高富帅']
	    },
	    {
	        teacherName: 'Scott',
	        students: ['张三', '李四']
	    }
	];
	
	school.add(klasses);

## API

##### URL

包含 URL 分解和解析工具。通过调用 require('url') 使用。

	url.parse(urlStr[, parseQueryString][, slashesDenoteHost])
	url.format(urlObj)
	url.resolve(from, to)

##### 查询字符串(Query Strings)

该模块提供了一些处理查询字符串的实用程序。它提供以下方法：

	querystring.stringify(obj[, sep][, eq][, options])
	querystring.parse(str[, sep][, eq][, options])
	querystring.escape
	querystring.unescape

