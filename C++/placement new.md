# placement new  
**`placement new` 就是在用户指定的内存位置上构建新的对象，这个构建过程不需要额外分配内存，只需要调用对象的构造函数即可。**  
举例来说：  
```  
class Foo { };  
Foo* pFoo = new Foo();  
```  
`pFoo` 指向的对象的地址你是不能决定的，因为 `new` 已经为你做了这些工作。  
1. 第一步分配内存。  
2. 第二步调用类的构造函数。  

而 `placement new` 把原本 `new` 做的两步工作分开来。  
1. 第一步你自己分配内存。  
2. 第二步你调用类的构造函数在自己分配的内存上构建新的对象。  

## `placement new` 的标准用法  
```  
class Foo  
{  
    float m_value;  
public:  
    void print() const { std::cout << "address: " << this << std::endl; }  
    void setValue(float value) { std::cout << "set m_value: " << value << std::endl; m_value = value; }  
    void getValue() { std::cout << "get m_value: " << m_value << std::endl; }  
};  
```  

1. 分配内存：  
```  
char* buff = new char[sizeof(Foo)];  
memset(buff, 0, sizeof(Foo));  
```  
2. 构建对象：  
```  
Foo* pFoo = new (buff)Foo;  
```  
3. 使用对象：  
```  
pFoo->print();  
pFoo->setValue(1.0f);  
pFoo->getValue();  
```  
4. 析构对象，显式的调用类的析构函数：  
```  
pFoo->~Foo();  
```  
5. 销毁内存：  
```  
delete[] buff;  
```  
上面5个步骤是标准的 `placement new` 的使用方法。  
对于 `buff` 这块内存可以反复使用，只要重复2、3、4步骤即可。  
  

## `placement new` 的好处：  
1. 在已分配好的内存上进行对象的构建，构建速度快。  
2. 已分配好的内存可以反复利用，有效的避免内存碎片问题。  
