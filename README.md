# absnake-in-windows

抽象版贪食蛇. 学习用项目, 用win32API制作图形界面程序. 移植历程: linux终端 -> windows终端 -> arm-linux开发板图形界面 -> windows图形界面

## Learn

主要学习资料来自 [[使用 C++ 和 Win32 API 为 Windows 电脑生成桌面应用]](https://learn.microsoft.com/zh-cn/windows/win32/)

### WIN32简介中的TIPS

数据类型重定义:

| 数据类型 | 大小  | 相当于               |
| -------- | ----- | -------------------- |
| `BYTE`   | 8 位  | `unsigned char`      |
| `WORD`   | 16 位 | `unsigned short`     |
| `DWORD`  | 32 位 | `unsigned int`       |
| `INT32`  | 32 位 | `int`                |
| `INT64`  | 64 位 | `long`               |
| `UINT32` | 32 位 | `unsigned int`       |
| `UINT64` | 64 位 | `unsigned long long` |
| `LONG`   | 32 位 | `long`               |
| `ULONG`  | 32 位 | `unsigned long`      |

看来Win32的编程也有很多历史包袱.
例如: `INT`和`LONG`完全相同.
又例如: `BOOL` 的本质是 `unsigned int`, 而不是 `bool` 或者与bool同长度的 `char`. 它们不能相互转换.

Windows用宽字符 `wchar_t` 代替字符, 要写宽字符字符串, 要用 `L` 前缀. 例如: `L"hello"`.

Windows对字符编码的描述有些混乱:

- `ANSI`: 实际上指 `Windows code pages` ，这个模式根据当前 locale 选定具体的编码.
- `Unicode`: 实际上仅仅是 `UTF-16`, 就算英文也占用两个字节.
- `UTF-8`: 实际上是 `UTF-8 with BOM` 编码, 虽然差不多, 但直接按 `UTF-8` 来读又会出问题.

`Unicode`是后来引进的, 它用一套编码方式描述整个世界的语言. 现在的编程最好都用这种.
题外话, 可能是因为引入 `Unicode` 时, 旧系统存在大量 `ANSI` 编码的文件, 没法自动区分某个文件是 `ANSI` 还是 `Unicode`, 所以才给新的文件加上了 `BOM` 标记.

窗口不是对象, 是HandleWindow(HWND).
可以想象Windows有一张表, 用HandleWindow(HWND)来查找窗口, 以获取各种属性, 执行各种操作.

### 激动人心! 窗口程序的入口

Windows 程序以一个名为 `WinMain` 或 `wWinMain` 的函数为入口:

```c++
/// @brief 应用程序的入口点.
/// @param hInstance 是 `实例的句柄` 或 `模块的句柄`. 当可执行文件加载到内存中时, 操作系统使用此值来标识可执行文件.  某些Windows函数需要实例句柄, 例如加载图标或位图.
/// @param hPrevInstance 没有任何意义. 它在16位Windows中使用, 但现在始终为零.
/// @param pCmdLine 以Unicode字符串的形式包含命令行参数.
/// @param nCmdShow 是一个标志, 指示主应用程序窗口是最小化, 最大化还是正常显示.
/// @return 操作系统不使用返回值, 但你可以使用该值将状态代码传递给另一个程序.
int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, PWSTR pCmdLine, int nCmdShow);
```

## TODO-List

- [ ] 先用win32API制作一个简单的窗口程序.
- [ ] 了解Win32API的输入输出如何工作
- [ ] 决定要创建哪些类, 它们怎么工作
