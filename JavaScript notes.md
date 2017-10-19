# JavaScript notes

### 操作符

与大多数编程语言不同,js里的除法运算`/`,无法整除的两个整数会求出小数结果: `6 / 4 === 1.5`

### 基本数据类型

1. __数字:__ 包括浮点数与整数,例如1, 100, 3.14
2. __字符串:__ 由任意数量字符组成的序列,例如'a', 'one', 'one 2 three'
3. __布尔值:__ true 或 false
4. __undefined:__ 当试图访问一个不存在的变量时,就会得到一个特殊值,`undefined`. 除此之外,使用一个未初始化的变量也会如此, 因为js会自动将变量在初始化之前的值设置为`undefined`.
5. __null:__ 这是另一种只包含一个值的特殊数据类型,所谓`null`值,通常是指没有值,空值,不代表任何东西. `null`与`undefined`最大的不同在于,被赋予`null`的变量通常被认为是已经定义了的,只不过它不代表任何东西.

>任何不属于上述五种基本类型的值都会被认为是一个对象.甚至有时候也会将`null`视为对象,这是一个不代表任何东西的对象(东西).
>`typeof null === 'object'`
>* 基本类型(上面列出的五种类型)
>* 非基本类型(即对象)

##### 查看数据类型 `typeof`

想知道某个变量或值的数据类型,可以使用`typeof`特殊操作符,该操作符会返回一个代表数据类型的字符串,它的值包括`'number'`,`'string'`,`'boolean'`,`'undefined'`,`'object'`和`'function'`.

### 数字

数字以0开头代表八进制,以0x前缀开头代表十六进制.

```
var n = 0377; // 255
var n = 0xff; // 255
```

一个数字可以表示成1e1(或1e+1,1E1,1E+1)这样的指数形式,意思是在数字1后面加1个0,也就是10. 同理,2e+3的意思是数字2后面加3个0,也就是2000,也可以将2e+3理解为数字2的小数点向右移三位,同理,2e-3可以理解为数字2的小数点向左移三位.

```
var n = 2e+3; // 2000
var n = 2e-3; // 0.002
```

##### Infinity

js中能处理的最大值是1.7976931348623157e+308,最小值为5e-324
超出这个范围就是`Infinity`和`-Infinity`,代表正负无限,另外,任何数除0为`Infinity`, 正负无限相加不会得到0,会得到一个`NaN`(Not a Number的缩写),`Infinity`与其他任何数运算的结果都为`Infinity`

```
6 / 0; // Infinity
Infinity - Infinity; // NaN
-Infinity + Infinity; // NaN
Infinity - 20; // Infinity
-Infinity * 3; // -Infinity
typeof Infinity === 'number' // true, Infinity是数字
```

##### NaN

`NaN`虽然名字叫'不是数字',但事实上它依然属于数字,只不过是一种特殊的数字.

```
typeof NaN === 'number' // true, NaN也是数字
var a = 10 * 'f'; // NaN
1 + 2 + NaN; // NaN
```

### 字符串

当数字字符串用于算数运算中,该字符串会被当作数字类型来使用(由于加法操作符的歧义性,这条规则不适用于加法运算)

数字字符串转换为数字的有一种偷懒方法,将该字符串与1相乘即可.(更好的选择是使用`parseInt`函数)

```
var s = '1';
s = 3 * s; // 3
var s = '1';
s++; // 2
var s = '100';
s *= 1; // 100
var s = '' + 66; // '66' 
var s = '\u004a\u0053'; // 'JS' 
```

### 布尔值

使用双重取反符可以将任何值转换为等效的布尔值

```
var s = 'one';
!!s // true
```

空字符串'',null,undefined,数字0,数字NaN,布尔值false
这6个值在逻辑判断时均为false

### 操作符优先级

算数运算,先乘除后加减,跟数学中一样
逻辑运算优先级顺序 ! && ||

>尽量使用括号,而不是依靠操作符优先级来设定代码的执行顺序,这样才能有更好的可读性

### 惰性求值

在一个连续的逻辑操作中,如果在最后一个操作之前就已经明确了结果,那么该操作往往就不必再继续执行了,因为不会对最终结果产生任何影响

此外,JS引擎在一个逻辑表达式中遇到非布尔类型的操作数,那么该操作数的值就会成为该表达式所返回的结果

```
false || 'something'  // 'something'
true && 'something'   // 'something'
```

### 比较操作符

需要注意==, ===, !=, !== 的区别
另外 NaN == NaN 为false,NaN不等于任何东西,包括它自己

### undefined与null

```
1 + undefined;	// NaN
1 + null;		// 1
1 * undefined;	// NaN
1 * null;		// 0
!!undefined;	// false
!!null;			// false
'' + null;		// 'null'
'' + undefined;	// 'undefined'
```

## 数组

```
var arr = []; // [] 推荐
var arr = new Array(); // []
var arr = new Array(3); // [undefined * 3]
var arr = new Array(1,2,3); // [1,2,3]
arr[5] = 'newOne'; // [1,2,3,undefined * 2, 'newOne']
```

### 数组常用操作

```
arr.length;			// 计算长度
arr[arr.length - 1];	// 最后一个元素
arr[arr.length - 3];	// 倒数第三个元素
arr.push(4);	// 在数组末尾追加一个值,返回一个新长度
arr.pop();		// 移除末尾最后一个元素,返回这个元素
arr.shift();	// 移除第一个元素,返回这个元素
arr.unshift(["Paul", 35]);	// 在数组开头追加元素,返回新的长度
```

### splice
删除/替换/插入, 直接修改原数组  
```
var arr = [1, 2, 3, 4, 5];
//参数一,从哪个索引开始
//参数二,切割多少个元素
//参数三…参数N ,要在铰接处插入的元素
arr.splice(1, 2, 3, 4, 5); //此时为1 3 4 5 4 5
arr.splice(1, 3); //此时为1 4 5
arr.splice(2, 0, true); //此时为 1 4 true 5
```

### slice
截取子数组, 不修改原数组, 返回一个截取后的数组
```
var arr = [1, 2, 3, 4, 5];
arr.slice(1, 4); //返回从索引1开始,到索引4(左闭右开区间,不包括结束索引)为2 3 4
```

### indexOf
返回要查找的元素第一次出现所在的索引  
```
var arr = [1, 2, 3, 4, 5, 4, 3, 2, 1];
var index = arr.indexOf(2);
alert(index); // 1
var index = arr.indexOf(2, 4); // 从索引4开始查找元素为2的索引
alert(index); // 7
```

### lastIndexOf
从尾部向头部查找,使用方法同indexOf  

### forEach
对数组中每一个元素执行给定的回调函数, 做一些动作, 无返回  
```
var arr = [1, 2, 3, 4, 5];
arr.forEach(function(item, index, array) {
	alert("index = " + index + ", item = " + item + ", array = " + array);
});
// 弹出5次指定格式的文本对话框
```

### map
```
// 对数组中每一个元素执行给定的回调函数, 返回一个新数组, 不修改原数组
var arr = [1, 2, 3, 4, 5];
/*
	回调函数参数说明
	item 数组元素
	index 数组索引
	array 原始数组
*/
var res = arr.map(function(item, index, array) {
	return item * 2;
});
alert(res); // 2, 4, 6, 8, 10
```

### reduce
```
对数组中每一个元素执行给定的回调函数  
回调函数返回一个累积结果,在下次调用该回调函数时作为参数提供,
方法返回最后一次回调函数的累积结果

一维数组求和
var arr = [1, 2, 3, 4, 5];
var res = arr.reduce(function(prevResult, currItem, index, array) {
	return prevResult + currItem;
});
alert(res); // 15

//reduce第二个参数为prevResult初始值
var arr = [1, 2, 3, 4, 5];
var res = arr.reduce(function(prevResult, currItem, index, array) {
	return prevResult + currItem;
}, 5); //初始值设置为5
alert(res); // 20 
二维数组扁平化
var arr = [
  [1, 2],
  [3, 4],
  [5, 6],
];
var res = arr.reduce(function(prevResult, currItem, index, array) {
	return prevResult.concat(currItem);
});
alert(res); // 1,2,3,4,5,6
```

### reduceRight
按倒序操作, 从右边累积, 方法同reduce

### filter

```
对数组中每一个元素执行给定的回调函数  
过滤符合条件的元素,返回一个新数组, 不修改原数组
var arr = [1, 2, 3, 4, 5];
var res = arr.filter(function(item, index, array) {
	return item > 2;
});
alert(res);  //3, 4, 5
```

### sort

```
数组排序, 直接修改原数组, 默认使用字符串正序排序.
var arr1 = [5, 2, 4, 1, 3];
arr1.sort()
alert(arr1); // 1 2 3 4 5
var arr2 = [10, 5, 8, 1, 2];
arr2.sort();
alert(arr2); // 1 10 2 5 8

如果想按数字排序,要自定义一个方法函数传入sort
var arr = [5, 2, 4, 1, 3];
var index = arr.sort(function(a, b) {
	return a - b;
});
alert(index); // 1 2 3 4 5

var index = arr.sort(function(a, b) {
	return b - a;
});
alert(index); // 5 4 3 2 1
```

### reverse

```
反转数组元素, 直接修改原数组
var arr1 = [1, 2, 3];
arr1.reverse();
alert(arr1); // 3 2 1
```

### concat

```
连接两个数组.不修改数组本身,返回一个连接后的新数组
var arr1 = [1, 2, 3];
var arr2 = [true, 4, 5];
alert(arr1.concat(arr2)); //1 2 3 true 4 5
```

### split

```
用指定分隔符将字符串分割为数组
var string = "Split me into an array";
var array = [];
array = string.split(" ");
```

### join

```
将数组用指定连接符组成一个字符串, 不修改原数组本身, 返回一个新的字符串
var arr1 = [1, 2, 3];
alert(arr1.join('-')); // 1-2-3
```

##### 删除一个元素的值

该方式不会改变数组的长度

```
var arr = [1, 2, 3];
delete a[1]; // 返回true, 原数组变为 [1, undefined, 3]
```

##### 数组的数组(二维数组)

```
var a = [];
a[0] = [1, 2, 3];
a[1] = [4, 5, 6];
a;	// [[1, 2, 3], [4, 5, 6]]
a[1][2];	// 6
```

##### 获取字符串的某个字符

```
var s = 'one';
s[0];	// 'o'
s[1];	// 'n'
s[2];	// 'e'
```

## 对象

```
var obj = Object();
var obj = {};
obj.name = ‘xxx’;
obj.year = 1940;

var obj = {name:’xxx’, year:1940};
var obj2 = {};
obj2.someone = obj;
alert(obj2.someone.name) // xxx

var obj = {
	1: "abc",
	id: 9999,
	"my dog": "duoduo",
	"123abc": "xxx",
};
var num = 1;
obj[num];	// "abc"
obj["my dog"];	// "duoduo"
obj.newProp = "the new";	// 给对象增加一个属性
delete obj.newProp;			// 删除对象的属性

obj.hasOwnProperty(checkProp); // 检查对象的属性是否存在,返回布尔值
```

### 构造函数创建对象

```
var Car = function() { // 注意构造函数变量首字母通常大写
  this.wheels = 4;
  this.engines = 1;
  this.seats = 5;
};
/* 
	使用new关键字调用构造函数创建一个新的对象实例
	当myCar被创建以后,它可以像普通对象一样被使用
*/
var myCar = new Car();
myCar.turboType = "twin";

// 带参数的构造函数
var Car = function(wheels, seats, engines) {
  this.wheels = wheels;
  this.seats = seats;
  this.engines = engines;
};

var myCar = new Car(8, 2, 2);
```

### 类
```
var Bike = function() {
 
  var gear; // 私有属性

  this.getGear = function() {
    return gear;
  };
  
  this.setGear = function(count) {
    gear = count;
  };
};
```

## 条件与循环

判断一个变量是否存在

```
if (somevar) {// todo something...}
```

如果上述代码条件中的变量本身值为false,''或0,就会出问题

推荐使用typeof

```
if (typeof somevar !== 'undefined') {// todo something}
```

##### 替代if...else的三元运算符

```
var ret = (a === 1) ? 'a is one' : 'a is not one';
```

### 循环

* while
* do..while
* for
* for-in

### 正则表达式
```
var str = "The dog chased the cat";
/*
	把要查找的字符串放在//之间
	g表示全局,意味返回所有匹配而不仅仅是第一个
	i表示忽略大小写
	\d 数字选择器 /\d+/g 匹配一个或更多的数字
	\s 空白选择器(" ",\t \f \r \n)
	\S 任何非空白字符
*/
var exp = /the/gi;
str.match(exp)	// 返回匹配的数组,没有匹配则返回null
```

## 函数
函数总是返回一个值,即使没有指定return,它会返回一个undefined  
arguments是实参,parameter是形参

##### 提升
即使函数的声明写在了调用的后面, 解释器会自动把声明提升到当前作用域的顶部,  
注意,函数里的 _greeting_ 变量会发生提升,但是只是提升声明,赋值不会被提升  
所以控制台上输出 _undefined_  
__在脚本的顶部声明函数和变量，这样语法和行为就会相互保持一致。__
```
sum(1, 2);	// 正常执行, 返回3

function sum(a, b) {
	console.log(greeting);	// 这里输出的是 undefined
	var greeting = 'Hello';
	return a + b;
}
```
##### 函数表达式
将一个函数赋值给一个变量, __注意函数表达式不会提升__  
函数表达式：当将函数赋值给变量时，函数可以有名称，也可以是匿名的。 使用变量名称调用在函数表达式中定义的函数。
```
var func1 = function() { return 'hi'; };

// 有名称的函数表达式,只能通过变量调用,即 func2()
var func2 = function myHello() { return 'hello'; };
```

### arguments

通过arguments伪数组,可以得到所有传递给函数的参数

```
function args() { return arguments; }
args();	// []
args(1, 2, true, 'new'); // [1, 2, true, 'new']
```

### 预定义函数
数字格式
```
123.4567.toFixed(2);	// "123.46"
123.4550.toFixed(2);	// "123.45"
123.4551.toFixed(2);	// "123.46"

var a = 1234567;
a.toLocaleString('en-US');	// 1,234,567
```

随机数
```
Math.random(); // 生成0(包括0)到1(不包括1)之间的随机小数
Math.floor(); //向下取整,去掉小数部分
Math.floor(Math.random() * 20); // 0~19
Math.floor(Math.random() * (max - min + 1)) + min;	// 返回[min,max]之间的数 
```


* parseInt(), 将接受的到值(通常是字符串)转换成整数,失败返回NaN

```
parseInt('123');	// 123
parseInt('abc123'); // NaN
parseInt('2abc123');// 2
parseInt('123abc'); // 123
// 推荐使用带第二参数指定进制的方式
parseInt('ff', 10);	// NaN
parseInt('ff', 16);	// 255
parseInt('0377', 10);	// 377
parseInt('0377', 8);	// 255
```

* parseFloat(),只有一个参数,与parseInt相同,把字符串转换为十进制数,可以接受指数形式

```
parseFloat('123e-2');	// 1.23
parseFloat('123e2');	// 12300
```

* isNaN() 判断某个值是否可以参与算术运算,也可检测parseInt和parseFloat调用成功与否.

```
isNaN(123);	// false
isNaN(1.23);// false
isNaN('1.23');	// false
isNaN('a1.23');	// true
isNaN(parseInt('abc123'));	// true
```

* isFinite() 检测输入是否是一个既非Infinity也非NaN的数字

```
isFinite(Infinity);	// false
isFinite(NaN);		// false
isFinite(123);		// true
isFinite(1e308);	// true
isFinite(1e309);	// false
```

* encodeURI(),encodeURIComponent()
* decodeURI(),decodeURIComponent()

在URL或URI中,有一些字符有特殊意义,需要encodeURI()转义,返回一个可用的URL, encodeURIComponent()会把参数当成是URL的一部分去转义,decodeURI(),decodeURIComponent()是它们的反转义函数

```
var url = 'http://www.abc.com/script.php?q=this and that';
encodeURI(url);
// 'http://www.abc.com/script.php?q=this%20and%20that'

var url = 'http://abc.com/?q=this and that';
encodeURIComponent(url);
// 'http%3A%2F%2Fabc.com%2F%3Fq%3Dthis%20and%20that'
```

* eval() 将输入的字符串当作js代码来执行

```
eval('var a = 3;');
a;	// 3
```

### 作用域

js中的作用域跟其他大多数语言不同,它没有块作用域,一个局部作用域是在函数中体现的,全局变量指的是声明在所有函数之外的变量,与之相对的是局部变量,所指的就是在某个函数中定义的变量. 函数内的代码可以像访问自己的局部变量那样访问全局变量,反之则不行.

>如果声明一个变量没有使用var ,该变量就会被默认为全局变量. 最佳实践是尽量将全局变量降到最低, 总是使用var语句来声明变量.

```
var a = 123;
function f() {
  console.log(a);
  var a = 1;
  console.log(a);
}
f();
// 输出undefined和1, 这是因为函数域始终优先于全局域
// 所以局部变量a会覆盖掉所有与它同名的全局变量
// 虽然第一次输出a时还没有被正式定义(undefined)
// 但该变量本身已经存在于局部空间了.
```

### 函数也是数据

这种特殊的数据类型有两个重要的特性:
* 它们所包含的是代码
* 它们是可执行的(或者说是可调用的)

```
function f() {}
var f = function() {};	// 函数的标识记法
typeof f;	// 'function'
```

#### 匿名回调函数

将一个匿名函数作为参数传递给另一个函数去调用

```
function f(a, b, callback) {
  callback(a + b);
}

f(1, 2, function(n) {
  console.log(n);
});  // !!回调函数的参数是由主调函数去赋予实参的
```

#### 自调函数

适合执行某些一次性的或初始化任务

```
(function(name) {
  console.log('Hello, ' + name);
})('Houdi');
```

#### 内部(私有)函数

* 有助于确保全局名字空间的纯净性(命名冲突机会小)
* 私有性,可以选择只将一些必要的函数暴露给"外部世界",保留属于自己的函数,使它们不为应用程序的其他部分所用.

```
function outer() {
	var inner = function() {
		// todo something...
	}
}
```

#### 返回函数的函数

```
function a() {
  console.log('a!');
  return function() {
  	console.log('b!');
  }
}

var newFunc = a();	// 输出'a!' 并返回一个匿名函数
newFunc();	// 输出'b!'
// 或者
a()(); // 输出 'a!'  'b!'
```

#### 能重写自己的函数

由一个函数返回另一个函数来覆盖旧的.这对于某些一次性初始化工作的函数来说特别有用, 第一次调用后重写自己, 避免了每次调用时重复一些不必要的操作.

```
var a = function() {
  function someSetup() {
    // todo something...
  }

  function actualWork() {
    // actual work..
  }

  someSetup();
  return actualWork;
}();
```

这对于某些浏览器相关操作相当有用, 不同浏览器,实现相同任务有可能不同, 最好的选择就是让函数根据当前浏览器来重定义自己, 这就是所谓的"浏览器特性探测"技术.

## 闭包

