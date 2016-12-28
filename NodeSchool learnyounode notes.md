# NodeSchool learnyounode notes

### 安装与启动

    $ npm install -g learnyounode

    $ learnyounode

## 你好，世界 (第 1 个习题，共 13 个)

  编写一个程序，在终端（标准输出 stdout）打印出 "HELLO WORLD"。

## 提示

  要编写一个 Node.js 程序，你只需要创建一个后缀名为 .js 的文件，然后用
  JavaScript 去编写即可。完成后，你可以通过命令行 node 命令来运行它，例如：

     $ node program.js

  和浏览器一样，你可以在你的 Node.js 程序中书写 console：

     console.log("text")

  当你写完后，你需要运行：

     $ learnyounode verify program.js

  来进行后续处理。这样你的程序将会被执行一些测试，并且生成一个测试报告，如果你成功了的话，相应课程将会被标记为完成。

##### 答案

    console.log('HELLO WORLD')

## 婴儿学步 (第 2 个习题，共 13 个)

  编写一个简单的程序，使其能接收一个或者多个命令行参数，并且在终端（标准输出stdout）中打印出这些参数的总和。

## 提示

  你可以通过 process 这个全局对象来获取命令行中的参数。process 对象拥有一个名为 argv 的属性，该属性是一个数组，数组中包含了整条命令的所有部分。

  首先，请先编写一个包含如下带简易代码的程序来熟悉一下：

     console.log(process.argv)

  通过执行命令 node program.js 并在后面多加几个参数，来运行我们的程序，比如：

     $ node program.js 1 2 3

  这样，你就会得到这样一个数组：

     [ 'node', '/path/to/your/program.js', '1', '2', '3' ]

  你需要考虑的问题是，如何去循环遍历这些代表数字的参数，从而得到他们的总和。
  process.argv 数组的第一个元素永远都会是 node，并且第二个参数总是指向你的程序的路径，所以，你应该从第三个元素（index 是 2）开始，依次累加，直到数组末尾。

  同时，你需要注意的是，所有 process.argv 中的数组的元素都是字符串类型的，因此，你需要将它们强制转换成数字。你可以通过加上 + 前缀，或者将其传给 Number() 来解决。例如： +process.argv[2] 或者 Number(process.argv[2])。

  learnyounode 会在你执行 learnyounode verify program.js 的时候提供参数给你的程序，所以你不需要自己去加参数了。如果仅仅只是想测试一下，而不想验证验证你的答案，你可以通过输入 learnyounode run program.js 来测试。当你使用 run 的时候，learnyounode
  会进入它各个练习所准备的测试环境。

##### 答案

    var result = 0

    for (var i = 2; i < process.argv.length; i++) {
      result += Number(process.argv[i])
    }

    console.log(result)

## 第一个 I/O！ (第 3 个习题，共 13 个)

  编写一个程序，执行一个同步的文件系统操作，读取一个文件，并且在终端（标准输出 stdout）打印出这个文件中的内容的行(\n)数。类似于执行 cat file | wc -l 这个命令。

  所要读取的文件的完整路径会在命令行第一个参数提供。

## 提示

  要执行一个对文件系统的操作，你将会用到 fs 这个 Node 核心模块。要加载这类核心模块，或者其他的"全局"模块，可以用下面的方式引入：

     var fs = require('fs')

  现在你可以通过 fs 这个变量来访问整个 fs 模块了。

  在 fs 中，所有的同步（或者阻塞）的操作文件系统的方法名都会以 'Sync' 结尾。要读取一个文件，你将需要使用  fs.readFileSync('/path/to/file') 方法。这个方法会返回一个包含文件完整内容的 Buffer 对象。  

  Buffer 对象是 Node 用来高效处理数据的方式，无论该数据是 ascii 还是二进制文件，或者其他的格式。Buffer 可以很容易地通过调用 toString() 方法转换为字符串。如：var str = buf.toString()。
  
  如果你在想如何更简单地去计算行数，请回想一下，一个 JavaScript 字符串，可以使用 .split() 分割成子字符串数组，而且，'\n'
  可以作为分隔符。注意，供测试的文件末尾的最后一行并没有进行换行，即没有 '\n' 的存在，因此，使用这个方法的话，所得的数组的长度会比行数多一个。

##### 答案

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

## 第一个异步 I/O！ (第 4 个习题，共 13 个)

  编写一个程序，执行一个异步的对文件系统的操作：读取一个文件，并且在终端（标准输出 stdout）打印出这个文件中的内容的行数。类似于执行 cat file | wc -l 这个命令。

  所要读取的文件的完整路径会在命令行第一个参数提供。

# 提示

  这个题目的答案几乎和前一个问题一样，但是你需要用更加符合 Node.js 的风格的方式解决：异步。

  你将要使用 fs.readFile() 方法，而不是 fs.readFileSync()，并且你需要从你所传入的回调函数中去收集数据（这些数据会作为第二参数传递给你的回调函数），而不是使用方法的返回值。想要学习更多关于回调函数的知识，请查阅：[https://github.com/maxogden/art-of-node#callbacks](https://github.com/maxogden/art-of-node#callbacks)

  记住，我们习惯中的 Node.js 回调函数都有像如下所示的特征：

     function callback (err, data) { /* ... */ }

  所以，你可以通过检查第一个参数的真假值来判断是否有错误发生。如果没有错误发生，你会在第二个参数中获取到一个 Buffer 对象。和 readFileSync() 一样，你可以传入 'utf8' 作为它的第二个参数，然后把回调函数作为第三个参数，这样，你得到的将会是一个字符串，而不是 Buffer。

##### 答案

    var fs = require('fs')
    var file = process.argv[2]

    fs.readFile(file, function (err, contents) {
      if (err) {
        return console.log(err)
      }
      // 你也可以使用 fs.readFile(file, 'utf8', callback)
      var lines = contents.toString().split('\n').length - 1
      console.log(lines)
    })

## LS 过滤器 (第 5 个习题，共 13 个)

  编写一个程序来打印出指定目录下的文件列表，并且以特定的文件名扩展名来过滤这个列表。这次会有两个参数提供给你，第一个是所给的文件目录路径（如：path/to/dir），第二个参数则是需要过滤出来的文件的扩展名。

  举个例子：如果第二个参数是 txt，那么你需要过滤出那些扩展名为 .txt的文件。

  注意，第二个参数将不会带有开头的 .。

  你需要在终端中打印出这个被过滤出来的列表，每一行一个文件。另外，你必须使用异步的 I/O 操作。

## 提示

  fs.readdir() 方法接收两个参数：第一个是一个路径，第二个则是回调函数，这个回调函数会有如下特征：

     function callback (err, list) { /* ... */ }

  这里的 list 是一个数组，它所包含的元素是每个文件的文件名（字符串形式）。  

  你可能会发现 node 自带的 path 模块也很有用，特别是它那个 extname 方法。  

##### 答案

    var fs = require('fs')
    var path = require('path')

    var folder = process.argv[2]
    var ext = '.' + process.argv[3]

    fs.readdir(folder, function (err, files) {
      if (err) return console.error(err)
      files.forEach(function (file) {
        if (path.extname(file) === ext) {
          console.log(file)
        }
      })
    })

## 使其模块化 (第 6 个习题，共 13 个)

  这个问题和前面一个一样，但是这次会介绍模块的概念。你将需要创建两个文件来解决这个问题。

  编写一个程序来打印出所给文件目录的所含文件的列表，并且以特定的文件名后缀来过滤这个列表。这次将会提供两个参数，第一个参数是要列举的目录，第二个参数是要过滤的文件扩展名。你需要在终端中打印出过滤出来的文件列表（一个文件一行）。此外，你必须使用异步 I/O。

  你得编写一个模块文件去做大部分的事情。这个模块必须导出（export）一个函数，这个函数将接收三个参数：目录名、文件扩展名、回调函数，并按此顺序传递。文件扩展名必须和传递给你的程序的扩展名字符串一模一样。也就是说，请不要把它转成正则表达式或者加上 "." 前缀或者做其他的处理，而是直接传到你的模块中去，在模块中，你可以做一些处理来使你的过滤器能正常工作。

  这个回调函数必须以 Node 编程中惯用的约定形式（err, data）去调用。这个约定指明了，除非发生了错误，否则所传进去给回调函数的第一
  个参数将会是 null，第二个参数才会是你的数据。在本题中，这个数据将会是你过滤出来的文件列表，并且是以数组的形式。如果你接收到了一个错误，如：来自 fs.readdir() 的错误，则必须将这个错误作为第一个，也是唯一的参数传递给回调函数，并执行回调函数。

  你绝对不能直接在你的模块文件中把结果打印到终端中，你只能在你的原始程序文件中编写打印结果的代码。

  当你的程序接收到一些错误的时候，请简单的捕获它们，并且在终端中打印出相关的信息

  这里有四则规定，你的模块必须遵守：

   » 导出一个函数，这个函数能准确接收上述的参数。
   » 当有错误发生，或者有数据的时候，准确调用回调函数。
   » 不要改变其他的任何东西，比如全局变量或者 stdout。
   » 处理所有可能发生的错误，并把它们传递给回调函数。

  遵循一些约定的好处是，你的模块可以被任何其他也遵守这些约定的人所使用。因此，这里你新建的模块可以被其他 learnyounode 的学习者使用，或者拿去验证，都会工作得很好。

## 提示

  通过创建一个仅包含目录读取和文件过滤相关的函数的文件来建立一个新的模块。要使模块导出（export）单一函数（single function），你可以将你的函数赋值给 module.exports 对象：

     module.exports = function (args) { /* ... */ }

  或者你也可以使用命名函数，然后把函数名赋值给 module.exports。

  要在你原来的程序中使用你新的模块，请用 require() 载入你的模块，这和载入 fs 模块时候用 require('fs') 一样，唯一的区别在于你的本地模块需要加上 './' 这个相对路径前缀。所以，如果你的模块文件名字是 mymodule.js，那么你需要像这样写：

     var mymodule = require('./mymodule.js')

  '.js' 这个文件扩展名通常是可以省略的。

  现在，mymodule 这个变量就指向了你的模块中 module.exports了，因为你刚导出了一个单一的函数，所以现在所声明的变量 mymodule 就是那个模块所导出的函数了，你可以直接调用它了！

  同样，请记住，尽早捕获你的错误，并且在回调中返回：

     function bar (callback) {
       foo(function (err, data) {
         if (err)
           return callback(err) // 尽早返回错误

         // ... 没有错误，处理 `data`

         // 一切顺利，传递 null 作为 callback 的第一个参数

         callback(null, data)
       })
     }

##### 答案

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

## HTTP 客户端 (第 7 个习题，共 13 个)

  编写一个程序来发起一个 HTTP GET 请求，所请求的 URL 为命令行参数的第一个。然后将每一个 "data" 事件所得的数据，以字符串形式在终端（标准输出 stdout）的新的一行打印出来。

## 提示

  完成这个练习，你需要使用 Node.js 核心模块之一：http。
  
  http.get() 方法是用来发起简单的 GET 请求的快捷方式，使用这个方法可以一定程度简化你的程序。http.get() 的第一个参数是你想要 GET 的 URL，第二个参数则是回调函数。

  与其他的回调函数不同，这个回调函数有如下这些特征：

     function callback (response) { /* ... */ }

  response 对象是一个 Node 的 Stream 类型的对象，你可以将 Node Stream 当做一个会触发一些事件的对象，其中我们通常所需要关心的事件有三个： "data"，"error" 以及 "end"。你可以像这样来监听一个事件：

     response.on("data", function (data) { /* ... */ })

  'data' 事件会在每个数据块到达并已经可以对其进行一些处理的时候被触发。数据块的大小将取决于数据源。

  你从 http.get() 所获得的 response 对象/Stream 还有一个 setEncoding() 的方法。如果你调用这个方法，并为其指定参数为 utf8，那么 data 事件中会传递字符串，而不是标准的 Node Buffer 对象，这样，你也不用再手动将 Buffer 对象转换成字符串了。

##### 答案

    var http = require('http')

    http.get(process.argv[2], function (response) {
      response.setEncoding('utf8')
      response.on('data', console.log)
      response.on('error', console.error)
    }).on('error', console.error)

## HTTP 收集器 (第 8 个习题，共 13 个)

  编写一个程序，发起一个 HTTP GET 请求，请求的 URL 为所提供给你的命令行参数的第一个。收集所有服务器所返回的数据（不仅仅包括 "data" 事件）然后在终端（标准输出 stdout）用两行打印出来。

  你所打印的内容，第一行应该是一个整数，用来表示你所收到的字符串内容长度，第二行则是服务器返回给你的完整的字符串结果。

## 提示

  你有两种解题方法：

  1) 你可以把所有 "data" 事件所得的结果收集起来，暂存并追加在一起，而不是在收到后立刻打印出来。通过监听 "end" 事件，可以确定 stream 是否完成传输，如果传输结束了，你就可以将你收集到的结果打印出来了。

  2) 使用一个第三方模块，来简化从 stream 中收集数据的繁琐步骤。这里有两个不同的模块都提供了一些有用的 API 来解决这个问题（似乎还有好多另外的模块可以选哦！）：bl (Buffer list) 或者 concat-stream，来选一个吧！

  <https://npmjs.com/bl> <https://npmjs.com/concat-stream>

  要安装一个 Node 模块，需用到 Node 的包管理工具 npm，输入：

     $ npm install bl

  这样，相应的模块的最新版本便会被下载到当前目录下一个名为 node_modules 的子目录中。任何在这个子目录中的模块都可以简单地使用 require 语法来将模块载入到你的程序中，并且不需要加 ./ 这样的路径前缀，如下所示：

     var bl = require('bl')

  这里，Node 会先查找是否有这个名字的核心模块，如果没有，再查找在 node_modules 目录下是否有这个模块。  

  你可以把一个 stream pipe 到 bl 或 concat-stream 中去，它们会为你收集数据。一旦 stream 传输结束，一个回调函数会被执行，并且，这个回调函数会带上所收集的数据：

     response.pipe(bl(function (err, data) { /* ... */ }))
     // 或
     response.pipe(concatStream(function (data) { /* ... */ }))

  要注意的是你可能需要使用 data.toString() 来把 Buffer 转换为字符串。
  
##### 答案

    var http = require('http')
    var bl = require('bl')

    http.get(process.argv[2], function (response) {
      response.pipe(bl(function (err, data) {
        if (err) {
          return console.error(err)
        }
        data = data.toString()
        console.log(data.length)
        console.log(data)
      }))
    })

## 玩转异步 (第 9 个习题，共 13 个)

  这次的问题和之前的问题（HTTP 收集器）很像，也是需要使用到 http.get() 方法。然而，这一次，将有三个 URL 作为前三个命令行参数提供给你。

  你需要收集每一个 URL 所返回的完整内容，然后将它们在终端（标准输出stdout）打印出来。这次你不需要打印出这些内容的长度，仅仅是内容本身即可（字符串形式）；每个 URL 对应的内容为一行。重点是你必须按照这些 URL 在参数列表中的顺序将相应的内容排列打印出来才算完成。

## 提示

  不要期待这三台服务器能好好的一起玩耍！他们可能不会把完整的响应的结果按照你希望的顺序返回给你，所以你不能天真地只是在收到响应后直接打印出来，因为这样做的话，他们的顺序可能会乱掉。

  你需要去跟踪到底有多少 URL 完整地返回了他们的内容，然后用一个队列存储起来。一旦你拥有了所有的结果，你才可以把它们打印到终端。

  对回调进行计数是处理 Node 中的异步的基础。比起你自己去做，你会发现去依赖一个第三方的模块或者库会更方便，比如 [async](https://npmjs.com/async) 或者 [after](https://npmjs.com/after)。不过，在本次练习中，你应该尝试自己去解决，而不是依赖外部的模块。

##### 答案

    var http = require('http')
    var bl = require('bl')
    var results = []
    var count = 0

    function printResults () {
      for (var i = 0; i < 3; i++) {
        console.log(results[i])
      }
    }

    function httpGet (index) {
      http.get(process.argv[2 + index], function (response) {
        response.pipe(bl(function (err, data) {
          if (err) {
            return console.error(err)
          }

          results[index] = data.toString()
          count++

          if (count === 3) {
            printResults()
          }
        }))
      })
    }

    for (var i = 0; i < 3; i++) {
      httpGet(i)
    }

## 授时服务器 (第 10 个习题，共 13 个)

  编写一个 TCP 时间服务器

  你的服务器应当监听一个端口，以获取一些 TCP 连接，这个端口会经由第一个命令行参数传递给你的程序。针对每一个 TCP 连接，你都必须写入当前的日期和24小时制的时间，如下格式：

     "YYYY-MM-DD hh:mm"

  然后紧接着是一个换行符。

  月份、日、小时和分钟必须用零填充成为固定的两位数：

     "2013-07-06 17:42"

## 提示

  这次练习中，我们将会创建一个 TCP 服务器。这里将不会涉及到任何 HTTP 的事情，因此我们只需使用 net 这个 Node 核心模块就可以了。它包含了所有的基础网络功能。

  net 模块拥有一个名叫 net.createServer() 的方法，它会接收一个回调函数。和 Node 中其他的回调函数不同，createServer() 所用的回调函数将会被调用多次。你的服务器每收到一个 TCP 连接，都会调用一次这个回调函数。这个回调函数有如下特征：

     function callback (socket) { /* ... */ }

  net.createServer() 也会返回一个 TCP 服务器的实例，你必须调用 server.listen(portNumber) 来让你的服务器开始监听一个特定的端口。

  一个典型的 Node TCP 服务器将会如下所示：

     var net = require('net')
     var server = net.createServer(function (socket) {
       // socket 处理逻辑
     })
     server.listen(8000)

  记住，请一定监听由第一个命令行参数指定的端口。

  socket 对象包含了很多关于各个连接的信息（meta-data），但是它也同时是一个 Node 双工流（duplex Stream），所以，它即可以读，也可以写。对这个练习来说，我们只需要对 socket 写数据和关闭它就可以了。

  使用  socket.write(data) 可以写数据到 socket 中，用  socket.end() 可以关闭一个 socket。另外， .end() 方法也可以接收一个数据对象作为参数，因此，你可简单地使用 socket.end(data) 来完成写数据和关闭两个操作。  

  要创建一个日期，你需要使用 new Date() 并且自定义一个格式，这些方法将会很有用：

     date.getFullYear()
     date.getMonth()     // 从 0 开始
     date.getDate()      // 返回当前月的日期
     date.getHours()
     date.getMinutes()

  或者，如果你喜欢尝鲜的话，可以使用  strftime 这个模块。其中 strftime(fmt, date) 这个方法可以接收一个和 unix 命令 date 相似的时间日期格式。你可以在这里查看更多关于 strftime 的信息：[https://github.com/samsonjs/strftime](https://github.com/samsonjs/strftime)

##### 答案

    var net = require('net')

    function zeroFill (i) {
      return (i < 10 ? '0' : '') + i
    }

    function now () {
      var d = new Date()
      return d.getFullYear() + '-' +
        zeroFill(d.getMonth() + 1) + '-' +
        zeroFill(d.getDate()) + ' ' +
        zeroFill(d.getHours()) + ':' +
        zeroFill(d.getMinutes())
    }

    var server = net.createServer(function (socket) {
      socket.end(now() + '\n')
    })

    server.listen(Number(process.argv[2]))

## HTTP 文件服务器 (第 11 个习题，共 13 个)

  编写一个 HTTP 文件 服务器，它用于将每次所请求的文件返回给客户端。

  你的服务器需要监听所提供给你的第一个命令行参数所制定的端口。

  同时，第二个会提供给你的程序的参数则是所需要响应的文本文件的位置。在这一题中，你必须使用 fs.createReadStream() 方法以 stream 的形式作出请求响应。

## 提示

  由于我们需要创建的是一个 HTTP 服务而不是普通的 TCP 服务，因此，你应该使用 http 这个 Node 核心模块。它和 net 模块类似，http 模块拥有一个叫做 http.createServer() 的方法，所不同的是它所创建的服务器是用 HTTP 协议进行通信的。

  http.createServer() 接收一个回调函数作为参数，回调函数会在你的服务器每一次进行连接的时候执行，这个回调函数有以下的特征：

     function callback (request, response) { /* ... */ }

  在这里，这两个参数是代表一个 HTTP 请求以及相应的响应的两个对象。request 用来从请求中获取一些的属性，例如请求头和查询字符（query-string)，而 response 会发送数据给客户端，包括响应头部和响应主体。

  request 和 response 也都是 Node stream！这意味着，如果需要的话，你可以使用流式处理（streaming）所抽象的那些方法来实现发送和接收数据。

  http.createServer() 会返回一个 HTTP 服务器的实例。你需要调用 server.listen(portNumber) 方法去监听一个特定的端口。

  一个典型的 Node HTTP 服务器将会是这个样子：

     var http = require('http')
     var server = http.createServer(function (req, res) {
       // 处理请求的逻辑...
     })
     server.listen(8000)
  
  fs 这个核心模块也含有一些用来处理文件的流式（stream） API。你可以使用 fs.createReadStream() 方法来为命令行参数指定的文件创建一个 stream。这个方法会返回一个 stream 对象，该对象可以使用类似 src.pipe(dst) 的语法把数据从 src流传输(pipe) 到dst流中。通过这种形式，你可以轻松地把一个文件系统的 stream 和一个 HTTP 响应的 stream 连接起来。

##### 答案

    var http = require('http')
    var fs = require('fs')

    var server = http.createServer(function (req, res) {
      res.writeHead(200, { 'content-type': 'text/plain' })

      fs.createReadStream(process.argv[3]).pipe(res)
    })

    server.listen(Number(process.argv[2]))

## HTTP 大写转换器 (第 12 个习题，共 13 个)

  编写一个 HTTP 服务器，它只接受 POST 形式的请求，并且将 POST 请求主体（body）所带的字符转换成大写形式，然后返回给客户端。

  你的服务器需要监听由第一个命令行参数所指定的端口。

## 提示

  这里将不限制你使用 stream 处理 request 和 response 对象，并且这将更为简单。

  在 npm 中，有很多不同的模块可以用来在 stream 传输过程中 "转换" stream 中的数据。对于本次练习来说，through2-map 这个模块有一个比较简单的 API 可以使用。

  through2-map 允许你创建一个 transform stream，它仅需要一个函数就能完成「接收一个数据块，处理完后返回这个数据块」 的功能 ，它的工作模式类似于 Array#map()，但是是针对 stream 的：

     var map = require('through2-map')
     inStream.pipe(map(function (chunk) {
       return chunk.toString().split('').reverse().join('')
     })).pipe(outStream)

  在上面的例子中，从 inStream 传进来的数据会被转换成字符串（如果它不是字符串的话），并且字符会反转处理，然后传入 outStream。所以，我们这里是做了一个字符串反转器！记住！尽管，数据块（chunk）的大小是由上游（up-stream）所决定的，但是你还是可以在这之上对传进来的数据做一点小小的处理的。

  要安装 through2-map，输入:

     $ npm install through2-map

##### 答案

    var http = require('http')
    var map = require('through2-map')

    var server = http.createServer(function (req, res) {
      if (req.method !== 'POST') {
        return res.end('send me a POST\n')
      }

      req.pipe(map(function (chunk) {
        return chunk.toString().toUpperCase()
      })).pipe(res)
    })

    server.listen(Number(process.argv[2]))

## HTTP JSON API 服务器 (第 13 个习题，共 13 个)

  编写一个 HTTP 服务器，每当接收到一个路径为 '/api/parsetime' 的 GET 请求的时候，响应一些 JSON 数据。我们期望请求会包含一个查询参数（query string），key 是 "iso"，值是 ISO 格式的时间。

  如:

  /api/parsetime?iso=2013-08-10T12:10:15.474Z

  所响应的 JSON 应该只包含三个属性：'hour'，'minute' 和 'second'。例如：

     {
       "hour": 14,
       "minute": 23,
       "second": 15
     }

  然后增再加一个接口，路径为 '/api/unixtime'，它可以接收相同的查询参数（query string），但是它的返回会包含一个属性：'unixtime'，相应值是一个 UNIX 时间戳。例如:

     { "unixtime": 1376136615474 }

  你的服务器需要监听第一个命令行参数所指定的端口。

## 提示

  HTTP 服务器的 request 对象含有一个 url 属性，你可以通过它来决定具体需要走哪一条 "路由"。

  你可以使用 Node 的核心模块 'url' 来处理 URL 和 查询参数（query string）。
  url.parse(request.url, true) 方法会处理 request.url，它返回的对象中包含了一些很有帮助的属性，方便方便你处理
  querystring。

  举个例子，你可以在命令行窗口输入以下命令试试：

     $ node -pe "require('url').parse('/test?q=1', true)"
  

  你的响应应该是一个 JSON 字符串的形式。请查看 JSON.stringify()
  来获取更多信息。

  你也应当争做 Web 世界的好公民，正确地为响应设置 Content-Type 属性：

     res.writeHead(200, { 'Content-Type': 'application/json' })

  JavaScript 的 Date 可以将日期以 ISO 的格式展现出来，如：new Date().toISOString()。并且，如果你把一个字符串传给Date的构造函数，它也可以帮你将字符串处理成日期类型。另外，Date#getTime() 放个应该也会很有用。

##### 答案

    var http = require('http')
    var url = require('url')

    function parsetime (time) {
      return {
        hour: time.getHours(),
        minute: time.getMinutes(),
        second: time.getSeconds()
      }
    }

    function unixtime (time) {
      return { unixtime: time.getTime() }
    }

    var server = http.createServer(function (req, res) {
      var parsedUrl = url.parse(req.url, true)
      var time = new Date(parsedUrl.query.iso)
      var result

      if (/^\/api\/parsetime/.test(req.url)) {
        result = parsetime(time)
      } else if (/^\/api\/unixtime/.test(req.url)) {
        result = unixtime(time)
      }

      if (result) {
        res.writeHead(200, { 'Content-Type': 'application/json' })
        res.end(JSON.stringify(result))
      } else {
        res.writeHead(404)
        res.end()
      }
    })
    server.listen(Number(process.argv[2]))

###