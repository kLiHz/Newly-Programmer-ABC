# C++ auto 关键字

根据 [en.cppreference.com 关于 auto][placeholder-cppref] 的页面，其被称为“类型说明符占位符（placeholder type specifier）”。

简单地说，C++11 之后，编程者可以使用 `auto` 关键字，令编译器推导出变量的类型，而不再需要显式声明。

`auto` 关键字的方便之处在于：

- 代替冗长复杂的变量声明；
- 当变量的类型不确定时，比如依赖模板参数的变量类型

C++14 之后，函数的返回值类型也可用 auto 占位，如下：

```cpp
template<typename T>
auto multiply(T a, T b) { return a * b; }

int main() {
    long long a = 1, b = 2;
    long long c = multiply(a, b);
}
```

## 参考资料

- [Placeholder type specifiers (since C++11) - cppreference.com][placeholder-cppref]
- C++11 特性：auto 关键字 <https://www.cnblogs.com/QG-whz/p/4951177.html>

[placeholder-cppref]: https://en.cppreference.com/w/cpp/language/auto
