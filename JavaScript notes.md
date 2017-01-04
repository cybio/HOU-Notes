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

## 函数

```
function sum(a, b) {
	return a + b;
}
```

### arguments

通过arguments伪数组,可以得到所有传递给函数的参数

```
function args() { return arguments; }
args();	// []
args(1, 2, true, 'new'); // [1, 2, true, 'new']
```

### 预定义函数

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

