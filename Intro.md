# 简介
*如果你只是需要"现代C++"的一些基础特性，这样的资源有很多。这本书主要面向如何使用现代C++的特性去构建正确、高效、可维护、可移植的程序。*

## 术语和俗称

<img src="/Users/zzy/Library/Application Support/typora-user-images/image-20210913092610853.png" alt="image-20210913092610853" style="zoom:50%;" />

C++ 98 主要缺乏对并发的支持，C++ 11支持了Lambda，C++ 14支持了函数返回类型推导。

### 左值 右值

C++ 11最普适的特性就是`move`语法，它的基础就是区分左值和右值。理论上右值是函数返回的临时对象，左值一般指通过指针和左值引用（&）实际引用的对象。*在以往的C++中，右值赋值时会产生不必要的深拷贝，而现在，通过move函数可以将复制右值的引用，然后将其置null，实现浅拷贝。*一个有用的启发是，要确定一个表达式是否为左值，只需要看能不能拿到它的地址即可。

一个典型的move Constructor：

```c++
class Widget{
	public: 
  	Widget(Widget&& rhs);
}
```

`rhs`自身是一个左值，但它的类型是右值**引用**。

在C++中，语法上不能区分从copy Constructor中创建的对象和move Constructor中创建的对象。比如：

```C++
void someFunc(Widget w);        // someFunc's parameter w is passed by value

Widget wid;                     // wid is some Widget

someFunc(wid);                  // in this call to someFunc, w is a copy of wid that's created via copy construction

someFunc(std::move(wid));       // in this call to SomeFunc, w is a copy of wid that's  created via move construction

```

传入函数的参数是argument，无论是左值还是右值。函数内引用的参数是parameter，它一定是左值。由此有perfect-forwarding问题，也就是一个argument被传递给一个函数，又传递给另外一个函数，保留其左值/右值特性。

### 函数对象

指支持`operator()`的对象。更宽泛的定义包括任何可以以`func(argument)`调用的东西，包括函数指针和函数。如果再包括成员函数指针，我们就可以得到一组callable对象。

通过lambda创建的函数对象被称作闭包。很少需要区别闭包和lambda的区别，所以两者经统称为lambda。类似不区分的概念如函数模版/模版函数、类模版、模版类。

### 声明和定义

定义（Definition）通常也可以被称为声明（Declaration）。

函数签名就是不包括其名称和参数名称的函数特征，比如`bool(const Widget&)`。

### 指针

- raw pointers：内建的指针，比如new返回的指针。
- smart pointers：重写了`operator->`和`operator*`的对象。