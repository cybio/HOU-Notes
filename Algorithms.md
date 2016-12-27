## Algorithms

#### 判断两个值属于同一区间
    (a < 0 && b < 0) || (a >= 0 && b >= 0)
    // 简写
    (a < 0 == b < 0)

#### 数组降序

```JavaScript
arr.sort(function(v1, v2) { return v2 - v1; });
```

#### 判断N个布尔值是否相等

```JavaScript
return arr.indexOf(!arr[0]) < 0;
```

#### 回文递归,判断字符串是否为回文

```JavaScript
function isPalindrome(text) {
	if (text.length <= 1) return true;
	if (text.charAt(0) != text.charAt(text.length - 1)) return false;
	return isPalindrome(text.substr(1, text.length - 2));
}
```

#### 将JS表达式转为等效的布尔值

```
!!"String" === true	// true
!!0 === false	// true
```

