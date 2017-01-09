# Windows programming notes

## 第一个Windows程序

Windows工作原理的中心思想就是"动态链接". Windows自身带有一大套函数(Windows API), 程序通过调用这些函数来实现它的用户界面和在屏幕上显示的文本与图形. 这些函数都是在动态链接库里实现的,这些文件名称都带有后缀.DLL,或有时带有后缀.EXE  这些文件通常放在\Windows\System32子目录下.

3大核心动态库
* Kernel32.dll, 内存管理,文件输入输出,任务管理等
* User32.dll, 用户界面,窗口管理等
* GDI32.dll, 图形设备接口,负责在屏幕或打印机上显示文本与图形

```
#include <windows.h>

int WINAPI WinMain(HINSTANCE hInstance, 
					HINSTANCE hPrevInstance, 
					PSTR szCmdLine, 
					int iCmdShow) {
	MessageBox(NULL, TEXT("Hello, Windows 10"), TEXT("This is title"), 0);
	return 0;
}
```

>VS下项目属性, C/C++运行库设置/MD是多线程DLL模式,程序运行时动态链接DLL库,选择/MT为多线程模式, 编译链接阶段会把需要的函数链接在你的程序里. 另外code::blocks里屏蔽命令行黑窗口的方法是在项目配置里选择build targets标签把Type选为GUI application即可. 如果是命令行下编译Windows程序,屏蔽程序黑窗口需要加一个参数__-mwindows__

WINDEF.H中用一下语句来定义WINAPI标识符:

    #define WINAPI __stdcall

这条语句规定了一种函数调用约定,表明如何生成在堆栈中放置函数调用参数的机器代码.

WinMain的第一个参数是实例句柄,是一个数值,标识了程序本身.第二个参数是16位时代用来表示此程序运行的其他实例,所有实例都共享代码和只读存储资源(菜单或对话框模版之类的),在32位Windows中,已不再使用,通常总是NULL.

WinMain的第三个参数是用来运行程序的命令行,有些程序启动时用它来把文件装入内存. 第四个参数用来指明程序最初如何显示:正常显示,最大化到全屏或最小化显示在任务栏上.

#### MessageBox 函数

显示一个消息对话框
* 第一个参数是窗口句柄
* 第二个参数是信息框里要显示的文本
* 第三个参数是窗口标题
* 第四个参数是以MB_为前缀的常量组合,可以用OR(|)运算符组合起来

```
#define MB_OK          	0x00000000L
#define MB_OKCANCEL		0x00000001L
#define MB_ICONHAND		0x00000010L
```

返回值有IDOK, ISYES, ISNO, IDCANCEL, IDABORT等..

## Unicode

ANSI C支持多字节字符集,被当作单字节值的字符串,这种宽字符并不一定是Unicode, Unicode只是宽字符编码的一种实现.

先看一下正常字符

```
char c = 'A'; // 0x41
char* p = "Hello!"; // 指针32位占4字节,字符串使用7个字节('\0'结尾)
char a[] = "Hello!";  // 全局字符数组
static char a[] = "Hello!"; // 函数的局部变量必须定义为静态的
```

##### 更宽的字符

C语言中的宽字符是wchar_t类型的,这个类型被定义在多个头文件中,包括WCHAR.H

    typedef unsigned short wchar_t; // 16位宽

单个宽字符的变量:

    wchar_t c = 'A'; // 两字节0x0041,内存中存储顺序为0x41, 0x00

定义一个已初始化的指向宽字符串的指针:

    wchar_t* p = L"Hello!"; // 字符串加L紧接左引号,占14字节

宽字符数组:

    static wchar_t a[] = L"Hello!"; // 占14字节,a[1]=='e'或0x0065

##### 宽字符库函数

如果使用传统的strlen计算字符串长度,strlen会计算第一个字节为字符,然后会认为第二字节的0x00为字符串的结尾零字节,宽字符版本的strlen函数被称为`wcslen`(宽字符字符串长度),定义在STRING.H(也是strlen被定义的地方)和WCHAR.H中.

    wchar_t a[] = L"Hello!";
    printf("%d\n", wcslen(a));	// 6
    printf("%u\n", sizeof(a));  // 14

记住,使用宽字符的时候,字符串的长度并没有改变,改变的只是字节长度.

##### 维护一个源代码文件

在vs中有一个TCHAR.H头文件,它不是ANSI C标准的一部分,所以定义的每一个函数和宏都有一个下划线前缀.

```
// 如果一个命名为_UNICODE的标识符被定义了
#define _tcslen wcslen
typedef wchar_t TCHAR;
#define __T(x) L##x        // 使字母L和宏参数拼接在一起

//如果_UNICODE没有被定义
#define _tcslen strlen
typedef char TCHAR;
#define __T(x) x

//其他两个宏定义成和__T一样
#define _T(x) __T(x)
#define _TEXT(x) __T(x)
```

无论何时在程序中使用其他头文件时,都应在所有其他头文件之前先包含WINDOWS.H
不管怎样,TEXT宏可如下定义:

    #define TEXT(quote) __TEXT(quote)

这些定义让你在同一程序中能够混合使用ASCII和Unicode的字符串,或者是编写一个版本的源代码让它可被编译成ASCII或Unicode的程序.

##### Windows与printf

Windows程序不存在标准输入输出概念, 你可以在Windows程序中使用fprintf函数,但不能使用printf函数. 

好消息是你仍然可以使用sprintf系列的其他函数来显示文本. 可以将格式化后的字符串输出到函数的第一个参数所提供的字符串缓冲区.

sprintf函数定义如下:

    int sprintf(char* szBuffer, const char* szFormat, ...);

该函数返回该字符串长度

```
char szBuffer[100];
sprintf(szBuffer, "The sum of %i and %i is %i", 5, 3, 5 + 3);
```

##### 例子程序
VS2015编译
```
#include <windows.h>
#include <tchar.h>
#include <stdio.h>

int CDECL MessageBoxPrintf(TCHAR* szCaption, TCHAR* szFormat, ...)
{
	TCHAR szBuffer[1024];
	va_list pArgList;

	va_start(pArgList, szFormat);

	_vsnwprintf_s(szBuffer, sizeof(szBuffer) / sizeof(TCHAR), 1024,
		szFormat, pArgList);
	// GCC版本=======================
	_vsntprintf(szBuffer, sizeof(szBuffer) / sizeof(TCHAR),
		szFormat, pArgList);
	//==============================

	va_end(pArgList);

	return MessageBox(NULL, szBuffer, szCaption, 0);
}

int WINAPI WinMain(HINSTANCE hInstance,
	HINSTANCE hPrevInstance,
	PSTR szCmdLine,
	int iCmdShow) {

	int cxScreen, cyScreen;

	cxScreen = GetSystemMetrics(SM_CXSCREEN);
	cyScreen = GetSystemMetrics(SM_CYSCREEN);

	MessageBoxPrintf(TEXT("屏幕尺寸大小"),
		TEXT("The screen is %i pixels wide by %i pixels high."),
		cxScreen, cyScreen);

	return 0;
}

```

## 窗口与消息
