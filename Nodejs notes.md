## Nodejs notes

[Nodejs官网](https://nodejs.org/en/)

执行js程序
```JavaScript
// program.js 源文件
console.log('Hello, world');
```
```
$ node program.js // 执行
```

#### process全局对象
你可以通过 process 这个全局对象来获取命令行中的参数。process 对象拥有一个名为 argv 的属性，该属性是一个数组，数组中包含了整条命令的所有部分。

首先，请先编写一个包含如下带简易代码的程序来熟悉一下：

    console.log(process.argv);

通过执行命令 `node program.js`
并在后面多加几个参数，来运行我们的程序，比如：

    $ node program.js 1 2 3

这样，你就会得到这样一个数组：

    [ 'node', '/path/to/your/program.js', '1', '2', '3' ]

如何去循环遍历这些代表数字的参数，从而得到他们的总和。
process.argv 数组的第一个元素永远都会是node，并且第二个参数总是指向你的程序的路径，所以，你应该从第三个元素（index 是 2）开始，依次累加，直到数组末尾。

同时，你需要注意的是，所有 process.argv 中的数组的元素都是字符串类型的，因此，你需要将它们强制转换成数字。你可以通过加上 + 前缀，或者将其传给 Number() 来解决。例如： `+process.argv[2]` 或者 `Number(process.argv[2])`

#### 同步I/O

要执行一个对文件系统的操作，你将会用到 fs 这个 Node 核心模块。要加载这类核心模块，或者其他的"全局"模块，可以用下面的方式引入：

    var fs = require('fs');

现在你可以通过 fs 这个变量来访问整个 fs 模块了。

在 fs 中，所有的同步（或者阻塞）的操作文件系统的方法名都会以 'Sync' 结尾。
要读取一个文件，你将需要使用  `fs.readFileSync('/path/to/file')` 方法。 这个方法会返回一个包含文件完整内容的 Buffer 对象。

Buffer 对象是 Node 用来高效处理数据的方式，无论该数据是 ascii 还是二进制文件，或者其他的格式。Buffer 可以很容易地通过调用 toString() 方法转换为字符串。 如：`var str = buf.toString()`

```JavaScript
var fs = require('fs');

var contents = fs.readFileSync(process.argv[2]);
var lines = contents.toString().split('\n').length - 1;
console.log(lines);

// 只要把 'utf8' 作为 readFileSync 的第二个参数传入
// 就可以不用 .toString() 来得到一个字符串
//
// fs.readFileSync(process.argv[2], 'utf8').split('\n').length - 1
```

#### 异步I/O

用更加符合 Node.js 的风格的方式：异步。

使用 fs.readFile() 方法，而不是 fs.readFileSync()，并且你需要从你所传入的回调函数中去收集数据（这些数据会作为第二参数传递给你的回调函数），而不是使用方法的返回值。

记住，我们习惯中的 Node.js 回调函数都有像如下所示的特征：

    function callback (err, data) { /* ... */ }

所以，你可以通过检查第一个参数的真假值来判断是否有错误发生。如果没有错误发生，你会在第二个参数中获取到一个 Buffer 对象。
和 readFileSync() 一样，你可以传入 'utf8' 作为它的第二个参数，然后把回调函数作为第三个参数，这样，你得到的将会是一个
字符串，而不是 Buffer。

    var fs = require('fs');
    var file = process.argv[2];

    fs.readFile(file, function (err, contents) {
      if (err) {
        return console.log(err);
      }
      // 你也可以使用 fs.readFile(file, 'utf8', callback)
      var lines = contents.toString().split('\n').length - 1;
      console.log(lines);
    });

#### LS过滤器

编写一个程序来打印出指定目录下的文件列表，并且以特定的文件名扩展名来过滤这个列表。

fs.readdir() 方法接收两个参数：第一个是一个路径，第二个则是回调函数，这个回调函数会有如下特征：

    function callback (err, list) { /* ... */ }

这里的 list 是一个数组，它所包含的元素是每个文件的文件名（字符串形式）。 

node 自带的 path 模块也很有用，特别是它那个 extname 方法。

    var fs = require('fs');
    var path = require('path');

    var folder = process.argv[2];
    var ext = '.' + process.argv[3];

    fs.readdir(folder, function (err, files) {
      if (err) return console.error(err);
      files.forEach(function (file) {
        if (path.extname(file) === ext) {
          console.log(file);
        }
      });
    });

#### 模块化

你绝对不能直接在你的模块文件中把结果打印到终端中，你只能在你的原始程序文件中编写打印结果的代码。

当你的程序接收到一些错误的时候，请简单的捕获它们，并且在终端中打印出相关的信息

这里有四则规定，你的模块必须遵守：

 » 导出一个函数，这个函数能准确接收上述的参数。
 » 当有错误发生，或者有数据的时候，准确调用回调函数。
 » 不要改变其他的任何东西，比如全局变量或者 stdout。
 » 处理所有可能发生的错误，并把它们传递给回调函数。

遵循一些约定的好处是，你的模块可以被任何其他也遵守这些约定的人所使用。

通过创建一个仅包含目录读取和文件过滤相关的函数的文件来建立一个新的模块。要使模块导出（export）单一函数（single function），你可以将你的函数赋值给 `module.exports` 对象：

    module.exports = function (args) { /* ... */ }

或者你也可以使用命名函数，然后把函数名赋值给 `module.exports`。

要在你原来的程序中使用你新的模块，请用 `require()` 载入你的模块，这和载入 fs 模块时候用 require('fs') 一样，唯一的区别在于你的本地模块需要加上 './' 这个相对路径前缀。所以，如果你的模块文件名字是 mymodule.js，那么你需要像这样写：

    var mymodule = require('./mymodule.js')

'.js' 这个文件扩展名通常是可以省略的。

现在，mymodule 这个变量就指向了你的模块中 module.exports 了，因为你刚导出了一个单一的函数，所以现在所声明的变量 mymodule 就是那个模块所导出的函数了，你可以直接调用它了！

同样，请记住，尽早捕获你的错误，并且在回调中返回：

```JavaScript
     function bar (callback) {
       foo(function (err, data) {
         if (err)
           return callback(err) // 尽早返回错误

         // ... 没有错误，处理 `data`

         // 一切顺利，传递 null 作为 callback 的第一个参数

         callback(null, data)
       })
     }
```

代码如下:

```JavaScript
	// program.js
    var filterFn = require('./solution_filter.js')
    var dir = process.argv[2]
    var filterStr = process.argv[3]

    filterFn(dir, filterStr, function (err, list) {
      if (err) {
        return console.error('There was an error:', err)
      }

      list.forEach(function (file) {
        console.log(file)
      })
    })
```

```JavaScript	
	// solution_filter.js
    var fs = require('fs')
    var path = require('path')

    module.exports = function (dir, filterStr, callback) {
      fs.readdir(dir, function (err, list) {
        if (err) {
          return callback(err)
        }

        list = list.filter(function (file) {
          return path.extname(file) === '.' + filterStr
        })

        callback(null, list)
      })
    }
```

#### HTTP 客户端

 Node.js 核心模块之一：http。 

 http.get() 方法是用来发起简单的 GET 请求的快捷方式，使用这个方法可以一定程度简化你的程序。http.get() 的第一个参数是你想要 GET 的 URL，第二个参数则是回调函数。

 与其他的回调函数不同，这个回调函数有如下这些特征：

    function callback (response) { /* ... */ }

 response 对象是一个 Node 的 Stream 类型的对象，你可以将 Node Stream 当做一个会触发一些事件的对象，其中我们通常所需要关心的事件有三个：
 "data"，"error" 以及 "end"。你可以像这样来监听一个事件：

    response.on("data", function (data) { /* ... */ })

 'data' 事件会在每个数据块到达并已经可以对其进行一些处理的时候被触发。数据块的大小将取决于数据源。

 你从 http.get() 所获得的 response 对象/Stream 还有一个 setEncoding() 的方法。如果你调用这个方法，并为其指定参数为 utf8，那么 data 事件中会传递字符串，而不是标准的 Node Buffer 对象，这样，你也不用再手动将 Buffer 对象转换成字符串了。


    var http = require('http')

    http.get(process.argv[2], function (response) {
      response.setEncoding('utf8')
      response.on('data', console.log)
      response.on('error', console.error)
    }).on('error', console.error)

#### 1

