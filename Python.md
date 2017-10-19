# Python

### 字符串
字符串不能和数字相加,但是可以相乘
```
print 'a' + 1	// 错误
print 'a' * 3	// 输出 aaa
```
字符串索引
```
name = 'David'
print 'David'[0]	// D
print name[-1]	// d
```
子字符串, 从start到stop-1(即不包括stop位的字符)
```
word = 'assume'
print word[4:6]	// me
print word[4:]	// me
print word[:2]	// as
print word[:]	// assume
```
查找字符串
```
str = 'This is string!!'
print str.find('string')	//	返回第一次出现的位置 8
print str[8:]	// string!!
print str.find('word')	// 查找不到返回 -1
print str.find('')	// 始终返回 0 ,因为空字符串始终存在,只不过不可见
```
带参数的字符串查找
```
str = 'I am David I am David'
print str.find('am', 3)	// 从某个位置(含)之后开始查找,此处返回 13
print str[13:]	// 'am David'
```
反转字符串
```
str = 'abc'
str[::-1]	// 'cba'
```
测量字符串长度
```
len(str)	// 3
```
###### a,b = x,y 赋值
它将右边的所有值，赋值给对应的左侧变量。比如说：在 a, b = 3, 4 之后，a 的值是3，b 的值是4.  

这里真正发生的是，右侧的值都被打包成一个元组，并扩展到左侧的变量。