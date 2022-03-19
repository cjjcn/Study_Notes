# 序言&第一章
```c
#include<windows.h>

int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, PSTR szCmdLine, int iCmdshow)
{
	MessageBox(NULL, TEXT("Hello, Windows 98!"), TEXT("HelloMsg"), 0);
	return 0;
}
```
Windows程序的进入点是WinMain，总是以上方程序中的形式出现，该入口在`WINBASE.H`中有如下声明：
```c
int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, LPSTR lpCmdLine, int nShowCmd);
```
- WinMain的第一个参数被称为执行实体代号，唯一标识当前运行程序。在多次运行同一程序时，便创建了该程序的多个执行实体。程序检查`hPrevInstance`参数即可确定自身的其他执行实体是否正在运行。在32位Windows中，该概念已被抛弃。传给WinMain的第二个参数总是NULL
- 其中`LOSTR`和`PSTR`均作为指向字串的指标。`LP`字首代表长指标，这是16位元Windows下的产物。
- 第三个参数用于执行程序的命令列
- 第四个参数指出程序最初显示的方式（正常或最大化铺满整个画面）
- WinMain函数声明返回一个int数值，定义在`WINDEF.H`:
```c
#define WINAPI _stdcall
```
