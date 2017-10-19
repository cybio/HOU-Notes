## Algorithms

#### 选择排序
```c
void sortArray(int* a, int len)
{
	for (int i = 0; i < len; i++)
	{
		for (int j = i + 1; j < len; j++)
		{
			if (a[i] > a[j])
			{
				a[i] ^= a[j];
				a[j] ^= a[i];
				a[i] ^= a[j];
			}
		}
	}
}
```

#### 最大公约数(greatest common divisor)
```
// 辗转相除法
while (v != 0)
{
    temp = u % v;
    u = v;
    v = temp;
}
// The greatest common divisor is variable u.
```

#### 数字反转
```
     while ( number != 0 ) {
          right_digit = number % 10;
          printf ("%i", right_digit);
          number = number / 10;
     }
```

#### 闰年判断
```
	// 能被4整除且不能被100整除,或者能被400整除的年份是闰年
    rem_4 = year % 4;
    rem_100 = year % 100;
    rem_400 = year % 400;

    if ( (rem_4 == 0  &&  rem_100 != 0)  ||  rem_400 == 0 )
```

#### 求素数
```
{
  int n = 120, i, j;

  for (i = 2; i <= n; ++i) {

      for (j = 2; j * j <= i; ++j)
        if (i % j == 0) break;

      if (j * j > i)
        printf("%d is prime\n", i);
  }  
}
```

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

function palindrome(str) {  
  str = str.toLowerCase().match(/[a-z0-9]+/gi).join("");

  console.log(str);
  if (str.length <= 1) return true;
  
  for (var i = 0; i < Math.floor(str.length / 2); i++) {
    if (str.charAt(i) != str.charAt(str.length - (i + 1))) return false;
  }
  
  return true; 
}
```

#### 将JS表达式转为等效的布尔值

```
!!"String" === true	// true
!!0 === false	// true
```

#### 随机数

```
Math.floor(Math.random() * (max - min + 1)) + min;	// 返回[min,max]之间的数
```

