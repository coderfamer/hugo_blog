---
title: c++笔记：我所理解的const
date: 2020-11-28 12:07:32
draft:  false
categories:
  - cpp
tags:
  - const
---

# c++笔记：我所理解的const

---

编写程序的时候，我们希望定义个变量，它的值不能被改变，为了满足这一需求，可以用const关键词进行限定。const关键字可用于c++各种环境：

1. 变量
2. 引用和指针
3. 函数参数和返回值
4. 类成员变量
5. 类成员函数

## const修饰变量
  当const修饰于变量时，其值不能改变。即在声明变量之后，不允许进行赋值操作
  ```c++
  void const_variables()
{
    const int a = 20;
    // a++;     // error:对只读变量进行赋值操作
}
  ```

## const 修饰引用

与 const 修饰变量相似，引用本身和引用对象是一体的，所以只存在一种情况。const 引用本身不可变，const 引用对象根据对象属性决定是否可变。

```c++
void const_reference()
{
    int a = 20;
    const int & r = a;
    r++;                // error : 引用本身不可变

    const int b = 20;
    const int & rb = b;
    b++;                // error: 引用对象不可变
    rb++;               // error: 引用本身不可变
}
```

## const 修饰指针

const 对于指针修饰操作，可分为三种方式：修饰指针指向、修饰指针本身或同时修饰指针指向和本身
```c++
char hello[] = "hello"; 
```
### 修饰指针指向

```c++
const char *p = hello;  // 指针可变，指针指向不可变
```

### 修饰指针本身

```c++
char * const p = hello; // 指针不可变，指针指向可变
```

### 同时修饰指针本身和指向

```c++	
const char * const p = hello;  // 指针不可变，指针指向不可变
```

《Effective c++》 中介绍了记忆方法:

**如果关键字`const`出现在星号左边，表示被指物是常量；如果出现在星号右边，表示指针自身是常量；如果出现在星号两边，表示被指物和指针两者都是常量。**

```c++
void const_point()
{
    char a[] = "hello";
    char b[] = "world";

    const char * p1 = a;
    p1[2] = 'm';        // error: 指针本身不可变
    p1 = b;             // 指针指向可变

    char * const p2 = a;
    p2[2] = 'm';        // 指针本身可变
    p2 = b;             // error: 指针指向不可变

    const char * const p3 = a;
    p3[2] = 'm';        // error: 指针本身不可变
    p3 = b;             // error: 指针指向不可变

}
```

## const函数参数和返回类型

对于 const 修饰函数参数，与 const 修饰指针或者变量类似。如果函数内部对函数参数不进行修改操作，则可以对参数用const 修饰。变量、指针或者引用都可以进行操作。

当 const 修饰类对象引用时，对于性能还有一定提升。常规类对象作为参数，要经历 构造、拷贝、析构等流程，如果用const引用，不仅直接保护该对象本身，同时也省去了构造等一些流程。

```c++
// @para: _a  _a值不可变
// @para: _p2  _p1指针本身不可变，_p1指向对象内容可变
// @para: _p2  _p2指针本身和内容都不可变
// @para: _data _data不可变
const char _str[] = "const";
void const_func_args(const int _a, const char * _p1, const char * const _p2, const Data & _data) {
    _a++;			// error

    _p1 = _str;
    _p1[2] = 'm';   //error

    _p2 = _str;     // error
    _p2[2] = 'm';   // error

}
```

对于 const 返回值，记住其中一点即可。const函数返回值不可当左值使用，一定程度上可以防止用户误写代码导致的错误。《Effective c++》中就有一个样例，重载 * 运算符，误使用导致不必要的麻烦。

```c++
class Rational {...};
const Rational operator *(const Rational &lhs, const Rational &rhs);
Rational a, b, c;
(a * b) = c;		// 误写代码导致不必要的麻烦.
```

## const成员变量

c++ const成员变量和普通const变量用法相似。不过也存在明显的区别：

- const 成员变量只能通过声明和初始化列表初始化
- 相同类不同对象的const成员变量值可能不一样。

### const 成员变量初始化

由于const变量的特性，const变量在声明之后就不可改变。对于c++类来说，成员变量生命之后可在构造函数内部进行初始化，这样违背了const变量的特性。不过构造函数还支持初始化列表初始化，对于 const成员变量可在成员初始化列表中进行初始化

```c++
class ConstMember
{
public:
    ConstMember(int _mem) : mem_(_mem) { // 可以在初始化列表中初始化
        mem_ = 20;      // error: 不能在构造函数中对const成员变量进行初始化
    }
private:
    const int mem_ = 30;         // 根据c++类特性，可在类内声明时初始化 
};
```

### 相同类不同对象的const成员变量值可能不一样

当const成员变量在初始化列表中初始化时，可能调用的是有参构造函数，这样在声明类对象的时候，const成员变量值会根据类对象的参数改变

```c++
ConstMember member1(30);	// const成员变量值为30
ConstMember member2(40);	// const成员变量值为40
```

## const成员函数

const成员函数目的也是保护数据，写法是在函数声明尾部加上const

```c++
class ConstFunc
{
public:
    void GetVal() const;
private:
    int val_;
};

void ConstFunc::GetVal() const
{
    return this->val_;
}
```

*注意：const成员函数声明和定义尾部都需要加上const*

上述demo中，显示写出了this指针。这个是c++类成员函数的特性，除static成员函数的所有成员函数都会隐式绑定this指针。默认情况下，this的类型是指向类型非常量版本的常量指针，即 ConstFunc * const。尽管this指针为隐式的，this指针也遵循初始化规则，意味着我们不能把this绑定到一个常量对象上。这种情况也导致了我们不能在常量对象上调用普通成员函数。

- 不能改变成员变量的值
- 不能调用普通成员函数

```c++
class ConstFunc
{
public:
    int GetVal() const {
        SetVal(20);		// error： 不能在常量对象上调用普通成员函数
        data_++;		// error: 不能在常量对象上修改成员变量
        return val_; 
    }
    void SetVal(int _val) { val_ = _val; }
private:
    int val_;
    int data_;
};
```

### const成员函数重载

```c++
class ConstOverload
{
public:
    void GetValue();		
    void GetValue() const;	// 可视为函数重载
};
```



## 其他

###  const 常量表达式 替代预处理宏 #define
当文件中需要定义常量的时候，常规使用 #define 宏定义，可用 const 对 #define 进行替换，原因如下:
  - #define 属于预处理器的语法，在代码层面只进行简单的文本替换。不进入 symbol table 中，对于 debug 和排错有一定的难度。
  - const 是 c++ 编译器语法，会提供类型检查和调试信息。
```c++
// #define MAX_LEN  200
// 使用 const 替代 #define
const int MAX_LEN = 200;
```



### 小知识

对于 const 修饰的变量，有些同学会将 const 关键字写在类型之前，有些会写在类型之后、星号之前。这两种写法的意义是一样的。

```c++
const int * a;
int const * a;
```



## 参考文档
《effective c++》:

- 条款03：尽量以 const、enum、inline 替换#define(Prefer const,enums,and inlines to #defines)

- 条款03：尽可能使用const(Use const whenever possbile)

《c++ primer》第五版:

- 第二章 2.4  const限定符
- 第六章 6.2  const形参和实参
- 第七章 7.1  设计Sales_data类



