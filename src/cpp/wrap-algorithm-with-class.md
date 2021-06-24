# 用类包装算法

开始本内容之前，建议读者：

- 了解 C++ 类和对象的概念；
- 了解声明式编程、函数式编程、闭包；

## 正文

假如要写一个求某一元二次多项式的值的算法，该怎么实现呢？

要确定一个一元二次多项式，需要 3 个参数。再加上一个用来表示变量值的参数，一共需要 4 个参数。

比如我们想求 $x^2 + 2x + 1$ 在 $x=1$ 、 $x=4$ 和 $x=5$ 处的值，我们可以编写一个函数，然后这样编写程序：

```cpp
#include <vector>
#include <iostream>

auto 
value_of_quadratic_polynomial(double a, double b, double c, double x) {
    return a * x * x + b * x + c;
}

int main() {
    std::vector<double> values = {1, 4, 5};

    std::vector<double> results;
    for (auto x : values) {
        results.push_back(value_of_quadratic_polynomial(1, 2, 1, x));
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

    auto value_of_quadratic_polynomial = [](double a, double b, double c, double x) {
        return a * x * x + b * x + c;
    };

    std::vector<double> results;
    for (auto x : values) { results.push_back(value_of_quadratic_polynomial(1, 2, 1, x)); }

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
