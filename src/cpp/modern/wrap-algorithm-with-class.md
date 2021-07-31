# 用类包装算法

开始本内容之前，建议读者：

- 了解 C++ 类和对象的概念；
- 了解声明式编程、函数式编程、闭包；

## 解决问题的一类算法

有时，我们会见到编程者将算法放在一个名为 `Solution` 的 `class` 里面。

单词“Solution”是“解决方案”的意思。那么 `class Soulution` 就是声明出一个针对某问题解决方案的类，类中用来存放关于解决这一问题所需要的各种函数/方法、变量。

为什么要这样做呢，简单说来就是：

- 抽象层面上更符合逻辑——针对某一问题的解决方案，方案里可以有若干成员变量和成员函数
- 生成若干实例互不干扰——因为每个类的实例内部的（非静态）成员变量是独立的
- 更“干净”，即不会污染到程序其他地方的代码——类中的成员不会与类外其他标识符冲突

那么这个类（的对象），可以理解成**算法 + 算法运行需要的环境**。如果算法不需要额外的变量，也可以把类中相关函数设置成**静态**的。

具体来说，比如我们有一个问题，需要用函数 `funcA()` 解决，而 `funcA()` 又需要调用函数 `funcX()`， 而完成这个问题可能还需要一个 `int` 型的变量`val`。

按照 C 语言的思路，程序可能会这样编写：

```cpp
int val;

void funcX() { /* ... */ }

void funcA() { /* ... */ }

int main() { /* ... */ }
```

这样的话变量 `val`，以及函数 `funcA()` 和 `funcX()` 都是全局的，如果和其他很多很多的代码放在一起，就有名称冲突的可能。不过在 C++ 中，我们可以把它们放在一个类里面（根据情况，如果是静态方法，也可以放在一个命名空间里）。

```cpp
class Solution {
private:
  int val;
  void funcX() { /* ... */ }

public:
  void funcA() { /* ... */ }
};
```

因为需要供外部调用的只有 `funcA` 这个接口，所以只把它设置成 `public` 的。

然后在需要调用的地方调用就好了，比如 `main` 函数中：

```cpp
int main() {
  Solution s;
  s.funcA();
}
```

当然，如果是解决某一问题的解决方案，也不一定用`Solution`作为类名，比如`SolveAngle`、`MatrixAdd`之类的也可以。

> 另请参见：[C++ 中的 class Solution 是什么意思？ - 知乎](https://www.zhihu.com/question/443211709/answer/1718958336)


## 另一个例子

假如要写一个求某一元二次多项式（quadratic polynomial）的值的算法，该怎么实现呢？

要确定一个一元二次多项式，需要 3 个参数。再加上一个用来表示变量值的参数，一共需要 4 个参数。

比如我们想求 \\(x^2 + 2x + 1\\) 在 \\(x=1\\)、\\(x=4\\) 和 \\(x=5\\) 处的值，我们可以编写一个函数，然后这样编写程序：

```cpp
#include <vector>
#include <iostream>

auto 
value_of_quad_poly(double a, double b, double c, double x) {
    return a * x * x + b * x + c;
}

int main() {
    std::vector<double> values = {1, 4, 5};

    std::vector<double> results;
    for (auto x : values) {
        results.push_back(value_of_quad_poly(1, 2, 1, x));
    }

    for (auto val : results) { std::cout << val << " "; }
    std::cout << std::endl;

    return 0;
}
```

使用函数确实方便；当然，我们也可以把函数改为 lambda 表达式：

```cpp
#include <vector>
#include <iostream>

int main() {
    std::vector<double> values = {1, 4, 5};

    auto value_of_quad_poly = [](double a, double b, double c, double x) {
        return a * x * x + b * x + c;
    };

    std::vector<double> results;
    for (auto x : values) { results.push_back(value_of_quad_poly(1, 2, 1, x)); }

    for (auto val : results) { std::cout << val << " "; }
    std::cout << std::endl;

    return 0;
}
```

但我们很快发现，用来确定二次多项式的三个参数是重复的，显得有些多余。

现在，我们**重新思考**如何为这个问题设计算法：针对每个特定的多项式，仅有每次传入的变量值是不同的；于是，我们可以设计一个类，这个类实例化出的每一个对象即是针对某一个特定多项式的解决方案：

```cpp
#include <vector>
#include <iostream>

class SolveValueOfQuadraticPoly {
private:
    double a, b, c;
public:
    SolveValueOfQuadraticPoly(double a_, double b_, double c_) : a(a_), b(b_), c(c_) {}
    auto value(double x) { return a * x * x + b * x + c; }
};

int main() {
    std::vector<double> values = {1, 4, 5};

    SolveValueOfQuadraticPoly solver(1,2,1);

    std::vector<double> results;
    for (auto x : values) { results.push_back(solver.value(x)); }

    for (auto val : results) { std::cout << val << " "; }
    std::cout << std::endl;

    return 0;
}
```

我们甚至可以重载 `operator()`，这样看起来就更漂亮了：

```cpp
#include <vector>
#include <iostream>

class SolveValueOfQuadraticPoly {
private:
    double a, b, c;
    auto value(double x) { return a * x * x + b * x + c; }
public:
    SolveValueOfQuadraticPoly(double a_, double b_, double c_) : a(a_), b(b_), c(c_) {}
    auto operator()(double x) { return this->value(x); }
};

int main() {
    
    SolveValueOfQuadraticPoly solver(1,2,1);

    std::vector<double> values = {1, 4, 5};

    std::vector<double> results;
    for (auto x : values) { results.push_back(solver(x)); }

    for (auto val : results) { std::cout << val << " "; }
    std::cout << std::endl;

    return 0;
}
```

每一个“solver”，其实就是一个所谓的“闭包”。
