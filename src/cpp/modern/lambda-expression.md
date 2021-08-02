# C++ Lambda 表达式

> 参考链接：
>
> - [Lambda expressions (since C++11) - cppreference.com](https://en.cppreference.com/w/cpp/language/lambda)

## 引子

想象一下数学中的“函数”。比如我们有一个 \\(f(x) = 2x + 1\\)，给定一个 \\(x\\) 值，经过函数我们会得到 \\(2x+1\\)。

和我们在编程语言中通常说的函数来比较一下，发现我们平时所说的“函数”，由于庞大繁杂，更像是一个“子过程”。

## 匿名函数

没有和**标识符**绑定的函数定义叫做**匿名函数**（anonymous function），也可以叫做 Lambda 表达式（lambda expression）。匿名函数一般会用作高阶函数的参数，或者在需要返回一个函数的场景下使用。

如果有个函数（若干操作）只需要用一次或者几次，或者说要实现的操作比较轻巧，那么采用匿名函数会比使用具名函数更加简便。

> 参考链接：
>
> - [Anonymous function - Wikipedia](https://en.wikipedia.org/wiki/Anonymous_function)
> - [Lambda 表达式有何用处？如何使用？ - 知乎](https://www.zhihu.com/question/20125256) 

匿名函数抽象出一种运算，比如，想给数组中的每个元素加 1，可以这样写：

```cpp
#include <vector>

int main() {
    std::vector<int> a = {1, 2, 3, 4, 5};
    for (auto &i : a) i = i + 1;
}
```

但是写成下面这样会更清楚地看出，语句是对数组中的每个元素做了一个“加1”的操作；

```cpp
#include <vector>
#include <algorithm>

int main() {
    std::vector<int> a = {1, 2, 3, 4, 5};
    std::for_each(a.begin(), a.end(), [](auto & x){ x = x + 1; }); 
}
```

> 参考链接：
>
> - [std::for_each - cppreference.com](https://en.cppreference.com/w/cpp/algorithm/for_each)
> - [std::ranges::for_each, std::ranges::for_each_result - cppreference.com](https://en.cppreference.com/w/cpp/algorithm/ranges/for_each)

在 Python 中也有类似的语法：

```python
l = [1, 2, 3]
add_one = lambda x : x + 1
l = list(map(add_one, l)) 
print(l) # [2, 3, 4]
```

可见，我们定义了一个“加 1”的操作，然后将这个操作映射（map）到了列表的每一个元素上。

## 函数对象

**函数对象**，有时也被称作**仿函数**（functor），顾名思义，就是一个使用起来像函数，但其实并不是函数的东西。它是通过重载类的`()`运算符来实现的这种效果。

```cpp
#include <iostream>

struct add_n {
    int num;
    add_n(int _n) : num(_n) {}
    int operator()(int val) const { return num + val; }
};

int main() {
    add_n add_16(16);
    std::cout << add_16(16); // 输出32
}
```

上面的代码片中重载了 `add_n` 类的 `operator()` 方法，也就是 `()` 运算符，然后声明了一个 `add_n` 类的实例 `add_16`，接着调用了实例的 `operator()` 方法，看起来就像是使用了一个名为 `add_16` 的函数。



> 参考链接：
>
> - [C++ lambda表达式与函数对象 - 简书 (jianshu.com)](https://www.jianshu.com/p/d686ad9de817) （👈这个文章写的挺不错，下面的内容参照了这篇文章）



## 闭包

闭包可以理解成一保存着函数及其运行环境/状态的包。像上文的 `add_n`（的实例）就可以看成是一个闭包。



## 使用Lambda表达式

先来看 C++ 中 Lambda 表达式的几种形式，其中*captures*是表示需要捕获的变量，也就是匿名函数中需要用到的变量，*params*是需要接收的参数，*ret*是返回值类型，*body*则是函数体

|原型| 说明|
|:---|:---|
| [ *captures* ] ( *params* ) -> *ret* { *body* } | Lambda表达式的原型  |
| [ *captures* ] ( *params* ) { *body* } | 返回值可以自动推导，所以可以不用指明返回值 |
| [ *captures* ] { *body* }|如果函数不要参数，那么参数也可以省略掉|


先从比较简单的几个例子说起：

```cpp
#include <iostream>
int main() {
    auto hello = []{ std::cout << "Hello"; }; // 定义
    hello(); // 调用，输出"Hello"
    
    auto add = [](int a, int b) -> int { return a + b; }; // 指明返回类型
    auto multiply = [](int a, int b) { return a * b; };
    std::cout << "2 + 3 = " << add(2, 3);
    std::cout << "2 * 3 = " << multiply(2,3);
}
```

或者，我们还可以写出这样的程序：

```cpp
// C++ 20

#include <iostream>
#include <string>
#include <format>
#include <functional>

class MakeSentence {
public:
    using StrOp = std::function<std::string(std::string const&)>;
    static auto I_Ate(int num, std::string const& what, StrOp plural_form) {
        std::string tmplt = "I ate {} {}.";
        if (num == 0 || num == 1) {
            return std::format(tmplt, num, what);
        }
        else if (num > 1) {
            return std::format(tmplt, num, plural_form(what));
        }
        else return std::string("I ate nothing.");
    }
};

int main() {
    auto append_s = [](std::string const& str) { return str + "s"; };
    auto append_es = [](std::string const& str) { return str + "es"; };

    std::cout << MakeSentence::I_Ate(1, "apple", append_s) << "\n";
    std::cout << MakeSentence::I_Ate(2, "apple", append_s) << "\n";

    std::cout << MakeSentence::I_Ate(1, "potato", append_es) << "\n";
    std::cout << MakeSentence::I_Ate(2, "potato", append_es) << "\n";
}
```

比如使用标准库的排序的时候也可以用到匿名函数：

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>

int main() {
    using item_id_pair = std::pair<std::string, int>;
    std::vector<item_id_pair> items = {
        {"Melon", 5}, {"Apple", 1}, {"Cherry", 3}
    };
    auto sortByID = [](const item_id_pair & a, const item_id_pair & b){
        return a.second < b.second;
    };
    std::sort(items.begin(), items.end(), sortByID);
    for (auto i : items) {
        std::cout << "ID: " << i.second << "\t" << i.first << '\n';
    }
}
```

那么`[]`有什么用呢（不是为了好看），联系之前提到的的“闭包”，其实`[]`是用来**捕获**（capture）作用域中的变量的。捕获的变量就会被封装进这个“包”里面。捕获可以是值传递的方式，也可以是引用。

在`[]`中写变量名时，该变量就会被以值传递的方式捕捉进闭包类中，这种情况下无法修改捕捉到的值。如果想要改动值传递捕获的值，需要使用`mutable`关键字。

```cpp
#include <iostream>
int main() {
    int x = 10;
    // auto foo = [x](int a) { x += a; return x; }; // error!
    auto foo = [x](int a) mutable { 
        x += a; 
        return x; 
    };
    std::cout << foo(2) << '\n';  // 输出 12
    std::cout << foo(3) << '\n';  // 输出 15
    std::cout << x;               // 输出 10
}
```

在变量名前加 `&` 则为引用捕获：

```cpp
#include <iostream>
int main() {
    int x = 10;
    auto foo = [&x](int a) { x += a; return x; };
    std::cout << foo(2) << '\n';  // 输出 12
    std::cout << foo(3) << '\n';  // 输出 15
    std::cout << x;               // 输出 15
}
```

在`[]`中写`=`表示默认以值捕获所有变量，写`&`表示默认以引用捕获所有变量，如果有例外情况写在后面就好，以逗号`,`分割，如：

- `[=, &x]`：默认以值捕获所有变量，但是`x`是例外，通过引用捕获；
- `[&, x]`：默认以引用捕获所有变量，但是`x`是例外，通过值捕获；

引用捕获不会延长变量生存期，因此有可能出现**悬挂引用**（Dangling references），比如这样：

```cpp
auto make_function(int x) {
    return [&](int a) { return x + a; };
}

int main() {
    auto foo = make_function(5);
    foo(3);
}
```

在调用函数 `foo` 的时候，因为临时变量 `x` 已经被销毁，所以会返回奇奇怪怪的结果。

---

讲到这里，本篇文章就不深入讨论了，未尽事宜请阅读前文给出的链接，或自行查阅资料。

> 参考链接：
>
> - [C++ lambda表达式与函数对象 - 简书 (jianshu.com)](https://www.jianshu.com/p/d686ad9de817)
> - [第 3 章 语言运行期的强化 现代 C++ 教程: 高速上手 C++ 11/14/17/20 - Modern C++ Tutorial: C++ 11/14/17/20 On the Fly (changkun.de)](https://changkun.de/modern-cpp/zh-cn/03-runtime/index.html)
> - [Lambda Expressions in C++ &#124; Microsoft Docs](https://docs.microsoft.com/en-us/cpp/cpp/lambda-expressions-in-cpp)
> - [基础篇：Lambda 表达式和函数对象 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/143884880)



