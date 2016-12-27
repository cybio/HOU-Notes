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

#### 啊