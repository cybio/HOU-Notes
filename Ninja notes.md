## 基于对象的函数式脚本语言JavaScript (Ninja note)

#### 函数
函数都有一个name属性,函数真正的名字是字面量名称,与赋值的变量无关  
另外,一个命名函数声明在顶层,window对象上的同名属性会引用到该函数.
```JavaScript
var fn = function myName() {};
console.log(fn.name);
```
每个函数都有`apply()`和`call()`方法, 作为第一型对象(first-class object),它是由Function()构造器创建的  
每个函数都有隐式参数 this和arguments
```JavaScript
function fn() {
    var result;
	for (var i = 0; i < arguments.length; i++) {
		result += arguments[i];
	}
	this.result = result;
}

var obj1 = {};
var obj2 = {};

fn.apply(obj1, [1, 2, 3, 4]);
fn.call(obj2, 1, 2, 3, 4);
```
#### 作用域
与大多数其他语言作用域不同,变量作用域开始于声明处,结束于函数尾部,会跨域边界块(如大括号),内部函数在当前函数的任何地方都可用(提升),即便是提前引用.
#### 参数
函数的参数列表和实参长度可以不同
* 未赋值的参数被设置为undefined
* 多出的参数不会绑定到参数名称
* 每个函数调用会传入两个隐式参数  
  * arguments,实际传入的参数集合
  * this,作为函数上下文的对象引用

#### 调用
* 作为普通函数调用时,上下文是全局对象window
* 作为方法调用时,上下文是拥有该方法的对象
* 作为构造器调用时,上下文是一个新分配的对象
* 通过`apply()`和`call()`调用时,上下文可以设置成任意值

#### 内联函数
```JavaScript
var fn = function myName() {}; // 给匿名函数命名myName在外部是不可用的,只能通过fn访问匿名函数
```
#### callee属性
callee属性在新版js上会被去除,而且ES5在strict模式下也禁止使用该属性. callee是arguments的一个属性,引用的是当前所执行的函数,该属性可以作为一个可靠的方法引用函数自身.
```JavaScript
function fn(n) {
	if (n <= 1) return 1;
	return arguments.callee(n - 1) + 1;
}
```
### 将函数视为对象
>函数存储

```JavaScript
var store = {
	nextId: 1,
	cache: {},
	add: function(fn) {
		if (!fn.id) {     
			fn.id = store.nextId++;
			return !!(store.cache[fn.id] = fn);           
		}                                            
	}
};

function ninja(){}

assert(store.add(ninja), "Function was safely added.");
assert(!store.add(ninja), "But it was only added once.");
```
>自记忆函数,缓存开销昂贵的计算结果

```JavaScript
function isPrime(value) {
	if (!isPrime.answers) isPrime.answers = {};
	if (isPrime.answers[value] != null) {  
		return isPrime.answers[value];    
	}

	var prime = value != 1; // 1 can never be prime
	for (var i = 2; i < value; i++) {
		if (value % i == 0) {
			prime = false;
			break;
		}
	}

	return isPrime.answers[value] = prime;               
}

assert(isPrime(5), "5 is prime!" );            
assert(isPrime.answers[5], "The answer was cached!" );  
```
>缓存记忆DOM元素

```JavaScript
function getElements(name) {
	if (!getElements.cache) getElements.cache = {};
	return getElements.cache[name] = 
		getElements.cache[name] ||
		document.getElementsByTagName(name);
}       
```
>伪造数组方法

```JavaScript
var elems = {

	length: 0,                                          
	
	add: function(elem) {                              
		Array.prototype.push.call(this, elem);
	},
	
	gather: function(id) {                                  
		this.add(document.getElementById(id));
	}
};

elems.gather("first"); 
                                  
assert(elems.length == 1 && elems[0].nodeType, 
"Verify that we have an element in our stash");

elems.gather("second");
                    
assert(elems.length == 2 && elems[1].nodeType,  
"Verify the other insertion");         
```
### 可变长度的参数列表
>使用apply()支持可变参数

```JavaScript
function smallest(array) {
	return Math.min.apply(Math, array);
}

function largest(array) {
	return Math.max.apply(Math, array);
}

assert(smallest([0, 1, 2, 3]) == 0, 
	"Located the smallest value.");
assert(largest([0, 1, 2, 3]) == 3, 
	"Located the largest value.");
```

>函数重载
>遍历可变长度的参数列表
>Tip: 要检测对应形参是否传入,可以使用
>param === undefined 如果没有对应参数,返回true

```JavaScript
// 一个合并对象的例子,把多个对象属性合并到一个根对象上
function merge(root) {
	for (var i = 0; i < arguments.length; i++) {
		for (var key in arguments[i]) {
			root[key] = arguments[i][key];
		}
	}
	return root;
}

var merged = merge(
	{name: "Batou"},
	{city: "Niihama"});
```

>对arguments非数组集合应用数组操作的方法

```JavaScript
// Example, 求第一个参数乘以其余参数里最大的一个
function multiMax(multi) {
	return multi * Math.max.apply(Math, 
		Array.prototype.slice.call(arguments, 1));
}

assert(multiMax(3, 1, 2, 3) == 9, 
	"3*3=9 (First arg, by largest.)");
```

>函数的length属性
利用参数个数进行函数重载
对于一个函数,在参数方面,可以确定两件事
* 通过length属性,可以知道声明了多少命名参数
* 通过arguments.length,可以知道在调用时传入了多少参数


```JavaScript
// Example 给一个对象增加方法重载(闭包)
function addMethod(object, name, fn) {
	var old = object[name];
	object[name] = function() {
		if (fn.length == arguments.length)
			return fn.apply(this, arguments);
		else if (typeof old == 'function')
			return old.apply(this, arguments);
	};
}

var ninjas = {
	values: ["Dean Edwards", "Sam Stephenson", "Alex Russell"]
};
	
addMethod(ninjas, "find", function() {  
	return this.values;
});
	
addMethod(ninjas, "find", function(name) { 
	var ret = [];
	for (var i = 0; i < this.values.length; i++)
		if (this.values[i].indexOf(name) == 0)
			ret.push(this.values[i]);
	return ret;
});
	
addMethod(ninjas, "find", function(first, last) { 
	var ret = [];
	for (var i = 0; i < this.values.length; i++)
		if (this.values[i] == (first + " " + last))
			ret.push(this.values[i]);
	return ret;
});
	
assert(ninjas.find().length == 3,  
	"Found all ninjas");
assert(ninjas.find("Sam").length == 1,
	"Found ninja by first name");
assert(ninjas.find("Dean", "Edwards").length == 1,
	"Found ninja by first and last name");
assert(ninjas.find("Alex", "Russell", "Jr") == null,
	"Found nothing");
```
### 函数判断
通常`typeof`语句就可以完全满足这一要求
```JavaScript
assert(typeof ninja == "function", 
		"Functions have a type of function");
```
注意事项:
* Firefox,在HTML的`<object>`元素上使用`typeof`的话,会返回`function`,而不是我们期望的`object`
* Internet Explorer,如果对其他窗口(比如iframe)的不存在对象进行类型判断,该类型会返回`unknown`. IE还会将DOM元素的方法报告成`object`,如`typeof domNode.getAttribute == "object"`和`typeof inputElem.focus == "object"`.
* Safari, Safari认为DOM的NodeList是一个`function`,所以`typeof document.body.childNodes == "function"`.

一个不错的解决方式:
```JavaScript
function isFunction(fn) {
	return Object.prototype.toString.call(fn) == "[object Function]";
}
```
我们不直接调用fn.toString()原因有两个
* 不同对象可能有自己的toString()方法实现
* JS中大多数类型都已经有一个预定义的toString()方法覆盖了Object.prototype提供的toString()方法

通过直接访问Object.prototype的方法,可以确保我们得到的不是覆盖版本的toString(),而且最终得到的准确信息正是我们所需要的.

---