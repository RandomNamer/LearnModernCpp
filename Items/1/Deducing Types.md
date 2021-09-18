# Chapter I. 类型推断

C++98系统只有针对模版的类型推断，C++11则在此基础上扩展，增加了`auto`和`decltype`关键字，C++14则增加了这两个关键字的应用范围。

*“没有对类型推断系统的坚实理解，用现代C++进行高效编程是不可能的。”*

## Item 1 模版类型推断

考虑下面的代码：

```C++
template<typename T>
void f(ParamType param)
```



