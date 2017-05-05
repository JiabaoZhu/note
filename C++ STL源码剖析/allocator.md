# `allocator`  
**`allocator` 是 C++ 的一个模板类，它提供类型化的内存分配以及对象的分配和撤销。**  
最简单的实现就是通过对类进行 `new`、`delete` 或者 `malloc`、`free` 进行简单的封装。  
C++ STL 中也把它管理的对象的构造和析构分成两个不同的阶段，一是在 `allocator` 中对对象的空间配置和空间释放；二是在 `stl_construct` 中调用对象的构造函数对已经配置好的空间初始化和析构函数在空间释放之前进行对象的删除操作。  
一句话：**`allocator` 负责空间的配置和释放，`stl_construct` 负责对象的构造与析构。**(在 `bits/stl_construct.h` 中，但是要注意，因为在 STL 容器中进行的数据插入都是将一个存在的对象放入容器，所以进行的都是拷贝操作，也就是说 `construct` 中，调用的都是对象的拷贝构造函数)。  

根据 Allocator requirements 我们需要提供一些 typedefs：  
```C++  
template <class T>
class allocator
{  
public:
	typedef T           value_type;
	typedef T*          pointer;
	typedef const T*    const_pointer;
	typedef T&          reference;
	typedef const T&    const_reference;
	typedef size_t      size_type;
	typedef ptrdiff_t   difference_type;
};
```  

根据 The default allocator 的描述，我们要提供如下一些接口：
1. `allocator::allocator()`  
    默认构造函数。  
2. `allocator::allocator(const allocator&)`  
    默认拷贝构造函数。  
3. `template <class U> allocator::allocator(const allocator<U>&)`  
    泛化的拷贝构造函数。  
4. `allocator::~allocator()`  
    默认析构函数。  
5. `pointer allocate(size_type size, const void* hint = 0)`
    分配空间(大小为 `sizeof(T) * size` )，第二个参数用来辅助创建
6. ``



