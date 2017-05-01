# `typedef`  
**`typedef` 常用来定义一个标识符及关键字的别名，它是语言编译过程的一部分，但它并不实际分配内存空间。也不创建新的类型，`typedef` 在编译时被解释，因此让编译器来应付超越预处理器能力的文本替换而已。**  
在编程中使用 `typedef` 目的一般有两个：一个是给变量一个易记且意义明确的新名字，另一个是简化一些比较复杂的类型声明。  
## `typedef` 和数组指针  
例如我们要定义有 81 个字符元素的数组：  
```  
char line[81];  
char text[81];  
```  
定义一个 `typedef`，每当要用到相同类型和大小的数组时，可以这样：
```  
typedef char Line[81];  
Line line, text;  
```  
同样，可以象下面这样隐藏指针语法：
```  
typedef char* pstr;  
pstr str = "abc";  
int mystrcmp(pstr, pstr);  
```  
## `typedef` 和函数  
函数指针一般用于回调，例如信号处理，`libcurl` 等会应用到回调。回调是比较常用的技术，而回调就要涉及函数指针。  
当我们的程序中有以下函数:  
```  
void printHello(int i);  
```  
然后我们要定义一个函数指针，指向 `printHello`，并且调用这个方法，代码如下:  
```  
void (*pFunc)(int);  
pFunc = &printHello;  
(*pFunc)(110);  
```  
其中 `void (*pFunc)(int)` 是声明一个函数指针，指向返回值是 `void`，调用参数是 `(int)` 的函数，变量名是 `pFunc`，`pFunc` 就是函数指针了，以上是函数指针的简单用法。  
大家可以看到，声明一个函数指针是比较复杂的，尤其是当你要在多处地方声明同一个类型的函数指针变量，代码更加复杂，所以有下面简化的做法:  
```  
typedef void (*PrintHelloHandle)(int);  
```  
使用代码如下:  
```  
PrintHelloHandle pFunc;  
pFunc = &printHello;  
(*pFunc)(110);  
```  
将 `pFunc` 替换 `typedef void (*PrintHelloHandle)(int)`，展开之后就是：
```  
void (*pFunc)(int);  
```  
其实就是声明一个 `pFunc` 函数指针而已，根本没有 `PrintHelloHandle` 这种类型。
以后其他地方的程序需要声明类似的函数指针，只需要下面代码:  
```  
PrintHelloHandle pFuncOther;  
```  
这样，我们的代码就变得更加简洁易懂。  
***  
## 总结：  
1. **`typedef` 定义一种类型的别名，而不只是简单的宏替换，可以用作同时声明指针型的多个对象。而 `#define` 只是简单的字符串替换。**  
举例：`#define char* PCHAR`，`PCHAR pa, pb;`，等同于 `char *pa, pb;`，`pa` 为 `char` 型指针变量，`pb` 为 `char` 型变量；  
而 `typedef char* PCHAR; PCHAR pa, pb;`，等同于 `char *pa, *pb;`，`pa` 与 `pb` 则为两个 `char` 指针变量。  
2. **帮助旧的代码创建 `struct` 对象**。在旧的 C 代码中创建 `struct` 对象时，必须带上 `struct` 关键字，形式为 `struct 结构名 对象名;`。此功能在 C++ 中无太大作用。  
举例：`struct tP { int x; int y; };`，定义对象格式为 `struct tP tt1;`。而在 C++ 中直接使用结构名，不用带上 `struct` 关键字，如 `tP tt1;` 即可。  
当结构体如下定义：`typedef struct tP { int x; int y; } P;` 时，则使用 `P pp1` 定义对象。  
3. **用 `typedef` 定义与平台无关的类型。**  
举例：定义一个 `REAL` 的浮点类型，使其在目标平台上表示最高精度类型，则 `typedef long double REAL`，在不支持 `long double` 平台，更改为 `typedef double REAL` 即可。跨平台时，只要改 `typedef` 本身，不用对其他源码做修改。因为 `typedef` 是定义一种类型的新别名，不是简单的字符串替换，故比宏更稳健。此优点在我们写代码的过程中可以减少不少代码量。
4. **为复杂的声明定义一个新的简单的别名。**  
方法：在原来的声明里逐步用别名替换一部分复杂声明，如此循环，把带变量的部分留到最后替换，得到的即为原声明的最简化版。  
举例：`void (*b[10])(void(*)());`，变量名为 `b`。先替换右边括号里的 `pFunParam` 为别名：`typedef void (*pFunParam)();`。再替换左边的变量 `b` 为别名：`typedef void (*pFunx)(pFunParam);`，原声明的最简化版为：`pFun pFunx[10];`。  
