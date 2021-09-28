# 分支与循环

我们编程时，经常会需要根据不同的情况做出不同的选择，这就是“分支”。我们知道，CPU 是逐一执行指令的，若干指令是连续排列的，如果要实现根据不同情况执行不同的指令，或者重复执行一些命令，则需要使用命令告诉 CPU，使其**跳转**到之前或者之后的命令。这不得不说是很麻烦的。而高级语言为此设计了一些方便的语法，使得编程人员可以以更明了的方式描述这种流程。

## `if`、`else`

顾名思义，`if` 意为“如果”，即满足一定条件才能执行相应的代码。`else` 可以和 `if` 配套使用，当不满足条件时，执行 `else` 块中的代码；`else` 块不是必要的，如果不需要可以忽略。

```cpp
// party-entrance.cpp

#include <iostream>

int main() {
    int age = 0;
    std::cout << "What's your age?\n";
    std::cin >> age;
    if (age < 16) {
        std::cout << "Sorry, you're too young.\n";
    } 
    else {
        std::cout << "Welcome to the party!\n";
    }
}
```

## `for` 循环

常用的循环（loop）有`for` 循环和 `while` 循环。虽然使用 `for` 循环和 `while` 循环可以实现同样的效果，但 `for` 循环通常用于**确定数量**或**确定区间**的循环，比如执行确定次数目的语句，或者遍历（逐个访问）若干项。因此，`for` 循环需要**明确指定循环的起止**，如果不指定，则需要在循环内部判断退出条件，否则会陷入“死循环”（程序一直在循环中执行，不能退出）。

`for` 语句后面跟着一对括号 `()`，括号内部需要有两个分号，分割出共 3 条语句。第 1 条语句用来给**循环变量**赋初值；每次执行循环前都会执行第 2 条语句，用来判断循环是否继续进行——如果语句的真值为真，则继续，否则则终止循环；每次循环执行后会执行第 3 条语句，一般用来更新循环变量的值。

当然，我们可以在循环体外对循环变量赋初值，也可以在循环体内部更新循环变量的值。如果没有必要，可以将括号中的相应语句留空，但是不能省略分割语句用的分号。如果留空第 2 条语句，则相当于该语句的真值**永真**，这意味着，如果循环内部没有其他的退出方式，则程序一旦进入循环体，便无法退出，形成“**死循环**”。

我们可以使用 `break;` 语句强行退出**当前层次**的循环；使用 `continue;` 语句强行结束当前次循环（进入下一次循环前，依旧要进行条件的判断）。

下面的例子使用 `for` 循环完成常见的两类操作：

```cpp
// for-loop.cpp

#include <iostream>

int main() {
    for (int i = 0; i < 10; i = i + 1) {
        std::cout << "I love C++! \n";
    }

    const int ARRAY_LENGTH = 5;
    int a[ARRAY_LENGTH] = {1, 2, 3, 4, 5};

    for (int i = 0; i < ARRAY_LENGTH; i = i + 1) {
        std::cout << a[i] << " ";
    }
}
```

这里也体现了使用**数组**的好处——我们可以通过循环来访问数组中的每个元素。

## 区间 `for` 循环

C++ 11 带来了区间 `for` 循环（ranged for-loop）。这使我们可以更方便的**遍历**数据。

```cpp
// ranged-for-loop.cpp

#include <iostream>

int main() {
    int a[5] = {1, 2, 3, 4, 5};
    
    for (int val : a) {
        std::cout << val << " ";
    }
}
```

语句 `for (int val : a)` 可以解释成：对于 `a` 中的每一个（`int` 类型的）元素，我们将其称为 `val`。每次循环，我们都可以通过 `val` 这个名字访问容器中的一个元素。

C++ STL（Standard Template Library，标准模板库）中提供有各种“容器”，模拟了常用的一些数据结构，可以用来存放数据。

之前我们说到，像 `int a[5];` 这样的语句声明的是一块**固定大小**的空间，而 `std::string` 这种类型采用动态内存分配，我们在使用时大可不必担心空间的问题。同样，STL 中有一种名为 `vector` 的容器，我们可以将其理解为“动态数组”，其内部有一定的机制，可以实现随着用户的使用而自动扩充空间，不必担心超出空间的问题。

```cpp
// for-loop-demo-with-vector.cpp

#include <vector>
#include <iostream>

int main() {
    std::vector<int> a; 
    // 'a' is a 'vector' of 'int's
    a.push_back(1); // a: 1
    a.push_back(2); // a: 1, 2
    a.push_back(3); // a: 1, 2, 3

    for (auto val : a) {
        std::cout << val << " ";
    }
    std::cout << "\n";

    a.pop_back(); // a: 1, 2
    a.pop_back(); // a: 1
    a.pop_back(); // a: <empty>

    std::cout << a.size();
}
```

注：`for (auto val : a)` 中的 `auto` 为“自动类型推断关键字”，也就是将推断类型的工作交给了编译器。合理的使用 `auto` 可以简化我们的代码编写，提高代码的复用性——同样的代码无需改动，也能用于遍历存放 `float` 的 `vector` 甚至其他形式的容器。

## `while` 循环

`while` 循环通常用于不定范围的循环——只要满足循环的条件就一直执行循环体内部的语句。

```cpp
// while-loop-demo-with-stack.cpp

#include <vector>
#include <stack>
#include <iostream>

int main() {
    std::vector<int> a = {1, 2, 3, 4, 5};

    std::stack<int> s;

    for (auto val : a) { s.push(val); }
    // s: [] -> [1] -> [1, 2] -> ... -> [1, 2, 3, 4, 5]

    while (s.empty() == false) {
        auto val = s.top(); // 栈顶元素
        std::cout << val << " ";
        s.pop(); // pop: 弹出（栈顶元素）
    }
    std::cout << "\n";
    // s: [1, 2, 3, 4, 5] -> [1, 2, 3, 4] -> [1, 2, 3] -> ... -> []
}
```

`!` 运算符表示对真值求反。语句 `s.empty() == false` 和 `!s.empty()` 的真值是相同的，二者含义相同，都表示“`s` 不为空（empty）”的含义。

## `do while` 循环

有些时候，我们希望无论如何都执行（或者说，至少执行一次） `while` 循环中的语句，可以使用 `do while` 循环。

假如我们编写一个程序，不断获取用户输入，直到用户输入不满足条件后程序才会退出。

使用普通的 `while` 循环编写，我们可以采用不满足条件即跳出的方式：

```cpp
#include <iostream>

int main() {
    std::cout << "Input a integer less than 10 to quit.\n";
    int val;
    
    while (true) {
        std::cin >> val;
        std::cout << "You inputed " << val << "." << std::endl; // endl: end of line
        if (val > 10) {
            std::cout << val << " is less than 10. Bye! \n"; 
            break; // break 用于直接终止当前层的循环
        }
    }
}
```

由于我们**至少需要获得一次**用户的输入，于是我们可以采用 `do while` 循环：

```cpp
#include <iostream>

int main() {
    std::cout << "Input a integer less than 10 to quit.\n";
    int val;
    do {
        std::cin >> val;
        std::cout << "You inputed " << val << "." << std::endl;
    } while (val > 10);
    std::cout << val << " is less than 10. Bye! \n"; 
}
```

可以看到，采用了 `do while` 循环的代码更加简洁。

## 一些其他的小细节

在 `{} ` 包裹的代码块后，一般不需要加分号；但是，定义类和结构体的语句末则需要添加分号。另外，`do while` 语句后因为不是 `}` 结尾，所以也需要加分号。

因为 C++ 对语句之间分号的数量没有要求，如果不考虑美观要求，其实可以在任何不确定的地方加上分号 `;`。
