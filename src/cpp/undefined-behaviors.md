# 未定义行为

[C++ 中的未定义行为 | Just For Fun (selfboot.cn)](https://selfboot.cn/2016/09/18/c++_undefined_behaviours/)

[关于 C++ 未定义行为的一些事 - Studying Father's luogu blog - 洛谷博客](https://studyingfather.blog.luogu.org/undefined-behavior)

未定义行为（Undefined Behavior），有时简称为“UB”，它指的是，由于违反了语言的一些语法规则，而导致程序变得没有意义。

[Undefined behavior - cppreference.com](https://en.cppreference.com/w/cpp/language/ub)

简单地说，这些行为的结果是没有标准明确规定的，也就是说结果是不可预料的，可能编译器不同、编译器的版本不同，就会产生不同的结果。

直观表现包括但不限于：

- 程序 Debug 下结果正常，Release 下结果不正常（或者反过来/或者都不正常）；
- 代码在自己这里正常运行，但是发给别人就会有错。

因此，程序中最好避免未定义行为，也就是按规矩做事，也应避免使用不符合 C/C++ 规范的行为来实现想要的功能。

当一个表达式中对同一变量做了多次修改，那么结果是未定义的：比如，求表达式 `i+++++i` 的值；或者 `result = x() + y()`，其中 `x()` 或 `y()` 是有**副作用**（side-effect）的函数（也就是说，先后调用 `x` 或 `y` 可能会导致得到的结果不同）。

> 副作用：执行函数会修改其外部的值，比如全局变量。

如果某天遇到了关于类似问题，要明白讨论这个问题是没有意义的。

此外，不要为了省几行代码，而写出有歧义的表达式；尽量将表达式分成若干语句来写，以便阅读和维护。
