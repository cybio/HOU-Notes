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
```C
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

Windows程序是基于消息的,系统监听你对程序做出的动作(鼠标,键盘)事件,然后由Windows发送消息给程序,即Windows调用了程序的一个函数,这个函数是你写的,函数的参数描述了由Windows发送给程序的特定消息,这个函数就是回调函数"窗口过程".

当Windows程序开始执行时,会创建一个"消息队列",Windows程序中一般都包含一小段称为"消息魂环"的代码,用于从消息队列中检索消息,并将其分发给相应的窗口过程.

```C
#include <windows.h>

LRESULT CALLBACK WndProc(HWND, UINT, WPARAM, LPARAM);

int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, PSTR szCmdLine, int iCmdShow)
{
	static TCHAR szAppName[] = TEXT("HelloWin");
	HWND hwnd;
	MSG msg;
	WNDCLASS wndclass;

	wndclass.style = CS_HREDRAW | CS_VREDRAW;
	wndclass.lpfnWndProc = WndProc;
	wndclass.cbClsExtra = 0;
	wndclass.cbWndExtra = 0;
	wndclass.hInstance = hInstance;
	wndclass.hIcon = LoadIcon(NULL,	IDI_APPLICATION);
	wndclass.hCursor = LoadCursor(NULL, IDC_ARROW);
	wndclass.hbrBackground = (HBRUSH)GetStockObject(WHITE_BRUSH);
	wndclass.lpszMenuName = NULL;
	wndclass.lpszClassName = szAppName;

	if (!RegisterClass(&wndclass))
	{
		MessageBox(NULL, TEXT("This program requires Windows NT!"), szAppName, MB_ICONERROR);
		return 0;
	}

	hwnd = CreateWindow(szAppName,	// window class name
						TEXT("谈笑风生又一年"),	// window caption
						WS_OVERLAPPEDWINDOW,	// window style
						CW_USEDEFAULT,	// initial x position
						CW_USEDEFAULT,	// initial y position
						400,			// initial x size
						300,			// initial y size
						NULL,			// parent window handle
						NULL,			// window menu handle
						hInstance,		// program instance handle
						NULL);			// creation parameters

	ShowWindow(hwnd, iCmdShow);
	UpdateWindow(hwnd);

	while (GetMessage(&msg, NULL, 0, 0))
	{
		TranslateMessage(&msg);
		DispatchMessage(&msg);
	}

	return msg.wParam;
}

LRESULT CALLBACK WndProc(HWND hwnd, UINT message, WPARAM wParam, LPARAM lParam)
{
	HDC hdc;
	PAINTSTRUCT ps;
	RECT rect;

	switch (message)
	{
	case WM_CREATE:
		PlaySound(TEXT("xxoo.wav"), NULL, SND_FILENAME | SND_ASYNC | SND_LOOP);
		return 0;

	case WM_PAINT:
		hdc = BeginPaint(hwnd, &ps);
		GetClientRect(hwnd, &rect);
		DrawText(hdc, TEXT("这是一个基础窗口"), -1, &rect,
			DT_SINGLELINE | DT_CENTER | DT_VCENTER);
		EndPaint(hwnd, &ps);
		return 0;

	case WM_DESTROY:
		PostQuitMessage(0);
		return 0;	
	}

	return DefWindowProc(hwnd, message, wParam, lParam);
}
```

窗口中标题栏和窗口边框之间的白色区域为"客户区"(client area)
Windows程序员交流用语中称呼窗口过程为"win prock"

##### 大写标识符前缀的意义

前缀|说明|全名
:-:|:-:|:-:
CS|类样式选项|class style
CW|创建窗口选项|create window
DT|文本绘制选项|draw text
IDI|图标ID号|ID Icon
IDC|光标ID号|ID Cursor
MB|消息框选项|message box
SND|声音选项|sound
WM|窗口消息|window message
WS|窗口风格|window style

##### 新的数据类型


类型名|说明
:-:|:-:
UINT|unsigned int
PSTR|char*
WPARAM|unsigned int,unsigned short(win98)
LPARAM|long
LRESULT|long
WINAPI|__stdcall
CALLBACK|__stdcall
MSG|消息结构
WNDCLASS|窗口类结构
PAINTSTRUCT|绘制结构
RECT|矩形结构
HINSTANCE|实例句柄--程序本身
HWND|窗口句柄
HDC|设备环境(上下文)句柄
HICON|图标句柄
HCURSOR|鼠标指针句柄
HBRUSH|图形画刷句柄

##### 匈牙利标记(命名)法

前缀|数据类型
:-:|:-:
c|char,WCHAR,TCHAR
by|byte
n|short
i|int
x,y|int,x,y坐标
cx,cy|int,x,y长度,c表示count
B,f|BOOL(int),flag
w|WORD,unsigned short
l|long
dw|DWORD,unsigned long
fn|function
s|string
sz|string with '\0'
h|handle
p|point

### 窗口类

通常如下定义一个窗口类:

    WNDCLASS wndclass;

然后对该结构的10个字段初始化,并调用RegisterClass函数

* 类样式

    wndclass.style = CS_HREDRAW | CS_VREDRAW;

所有CS前缀标识符如下:

```
#define CS_VREDRAW			0x0001
#define CS_HREDRAW			0x0002
#define CS_KEYCVTWINDOW		0x0004
#define CS_DBLCLKS			0x0008
#define CS_OWNDC			0x0020
#define CS_CLASSDC			0x0040
#define CS_PARENTDC			0x0080
#define CS_NOKEYCVT			0x0100
#define CS_NOCLOSE			0x0200
#define CS_SAVEBITS			0x0800
#define CS_BYTEALIGNCLIENT	0x1000
#define CS_BYTEALIGNWINDOW	0x2000
#define CS_GLOBALCLASS		0x4000
#define CS_IME				0x00010000
```

* 设置窗口过程函数

    wndclass.lpfnWndProc = WndProc;

* 类结构和Windows内部维护的窗口结构中预留的额外空间:

    wndclass.cbClsExtra = 0;
    wndclass.cbWndExtra = 0;

应用程序可以根据需要来使用这些额外的空间.

* 实例句柄

    wndclass.hInstance = hInstance;

* 窗口图标

    wndclass.hIcon = LoadIcon(NULL, IDI_APPLICATION);

* 鼠标指针

    wndclass.hCursor = LoadCursor(NULL, IDC_ARROW);

* 客户区背景色

    wndclass.hbrBackground = GetStockObject(WHITE_BRUSH);

* 窗口的菜单

    wndclass.lpszMenuName = NULL;

* 窗口类名称

    wndclass.lpszClassName = szAppName;

当程序只创建一个窗口时,窗口类名称一般与程序名相同.

#### 注册窗口类

```
// 如果注册失败, RegisterClass会返回0
if (!RegisterClass(&wndclass))
{
    MessageBox(NULL, TEXT("Error info"), szAppName, MB_ICONERROR);
    return 0;
}
```

利用GetLastError函数可以获知函数调用错误的原因,返回错误代码,可以通过WINERROR.H查看.

### 窗口的创建

