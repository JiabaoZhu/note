# `typename` 与 `class`  
***  
**`class` 关键字用于定义类，在 C++ 引入模板后，`class` 关键字被用来定义模板。**  
```  
template <class T>  
...  
```  
此时 `class` 关键字表明 `T` 是一个类型，后来为了避免 `class` 在这两个地方的使用可能给人带来混淆，所以引入了 `typename` 关键字，它的作用同 `class` 一样表明后面的符号为一个类型，这样在定义模板的时候就可以使用下面的方式了：  
```  
template <typename T>  
...  
```  
**在模板定义语法中 `class` 与 `typename` 的作用完全一样。**  

***  
**`typename` 关键字还有另外一个作用：使用嵌套依赖类型(nested depended name)**  
如下所示：  
```  
class MyArray  
{  
public:  
    typedef int LengthType;  
    LengthType getLength();  
	...  
};  
template <class T>  
void MyMethod(T array)   
{  
	typedef typename T::LengthType LengthType;  
	LengthType length = array.getLength();  
}  
```  
如果前面没有 `typename`，编译器没有任何办法知道 `T::LengthType` 是一个类型还是一个成员名称(静态数据成员或者静态函数)，所以编译不能够通过。  
这个时候 `typename` 关键字的作用就是告诉 C++ 编译器，`typename` 后面的字符串为一个类型名称，而不是成员函数或者成员变量，这个时候就可以编译通过了。  
