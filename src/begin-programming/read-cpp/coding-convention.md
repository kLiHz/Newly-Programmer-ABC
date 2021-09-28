# C/C++ 代码规范

## 符号

以上，我们所声明的变量名、函数名，都可以统称为**符号**（symbol）。

## 命名规则

最基础的规则就是，变量名或者函数名等要有意义，并且能够让读者快速明白变量或者函数的含义。

## 代码规范

代码规范时在满足语法的基础上，使用一定的命名规则、代码格式甚至编程范式。良好的代码规范可以使得代码变得易于阅读和排查错误。

不同编程者可能采用不同的代码规范，但是也有一些规范是通行的。可以通过阅读现有的代码，来熟悉这些规范。

一个项目的代码规范往往需要事先制定，以保持一致性。

## 代码格式

### 缩进

缩进是非常常见的事情，它被用来显示出代码的层级。一般可以用`Tab`或者空格来形成缩进。

要输入一个`Tab`，只要按下键盘上的 Tab 键就好了。

使用`Tab`做缩进，只需要一个`Tab`就够了；而使用空格做缩进，一般会使用 2 或 4 个空格为一个缩进。

根据不同的编辑器设置或个人习惯，一个`Tab`可能会被显示成 2 / 4 / 8 个空格等等。因此，**切忌将Tab将空格(Space)和缩进(Tab)混用**，这样有可能导致代码显示出来的层次变得混乱，难以阅读。

```cpp
int foo(int num) {
    // 一级缩进
    if (num % 2 == 0) {
        // 二级缩进
        return num + 1;
    }
    else return num;
}
```

C/C++ 允许用户在语句之间加任意数量的分号 `;`，也可以在合理的地方使用使用任意数量的空白。有些语言就对代码格式非常敏感，比如 Python，同一层级的语句之间一定要有相同的缩进。

### 大括号

一般有两种方式，一种是左大括号“`{`”放在行末，另一种是`{`单独占一行。一般第一种更为常见。甚至，会有把`else`关键字放在`if`语句大括号的同行。

```cpp
int foo(int num) {
    if (num % 2 == 0) {
        return num + 1;
    } 
    else return num;
}
```

大括号换行的写法：

```cpp
int foo(int num)
{
    if (num % 2 == 0)
    {
        num *= 2;
        num += 1;
    } 
    return num;
}
```

格式的目的是**便于阅读**，并不是绝对的。比如如果几条语句比较简单、逻辑关联强，也可以放在一行。

```cpp 
<template class T>
std::vector<T> stack_reverse(std::stack<T> _stack) {
    std::vector<T> result;
    while (!_stack.empty()) { result.pusk_back(_stack.top()); _stack.pop(); }
    return result;
}
```

### 空格

比如`if`、`else`、`for`、`while`等**关键字后留空格**，可以突出关键字。而相对应的，**函数名后的括号要紧跟函数名**。

```cpp
void print_vector_int(const std::vector<int> & a) {
    for (auto i : a) std::cout << i << ' ';
    std::cout << std::endl;
}
```

左右括号`(`、`)`一般紧紧包裹其中的内容，而`,`、`;`则紧紧跟着其左侧的字符，其右侧要留一个空格。

```cpp
std::vector<int> a = {1, 2, 3, 4, 5};
```

双目运算符（如`+`、`-`、`=`、`==`、`+=`、`<`、`%`等）的左右都要留空格。

```cpp
int num1 = 10 / 2;
int num2 = 20 % 3;
int val = num1 + num2;
```

单目运算符（如`!`、`++`、`--`、`*`、`&`等）紧跟它的操作数，前后不留空格。

```cpp
std::vector<int> a = {
    1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12
};
auto i = a.begin();
while (i != a.end()) {
    std::cout << *i << ' ';
    ++i;
}
```

方括号`[]`、成员运算符`.`、`->`也紧跟其操作数，即前后不留空格。

```cpp
std::vector<int> a = {1, 2, 3, 4, 5};
a[0] = a[1] + a[2];
a.erase(a.begin());

int val = 10;
auto b = new std::vector<int>();
b->push_back(val);
b->erase(b->begin());
```

## 符号命名

养成良好且规范的**变量 / 函数命名**方式和习惯。

> 参考阅读：
>
> - [(转载) 骆驼命名法,帕斯卡命名法与下划线命名法 - CSDN](https://blog.csdn.net/u013321328/article/details/21025473)
>
> - [几种变量命名规则 - CSDN](https://blog.csdn.net/weixin_42205987/article/details/84063439)

符号的命名主要有下划线法、驼峰法等，比如`find_first_of()`、`namedWindow()`等。

**宏名**和**枚举名**（其实枚举就可以视作一种宏定义）一般采用全大写 + 下划线，比如`BGR2GRAY`、`MAX_LENGTH`、`BLUE`等。

一般来说，函数、局部变量名、全局变量名、宏名等，可以采用**不同的命名法**以区分彼此。

变量名也应具有一定的**意义**，比如`tmp`、`temp`一般表示临时变量，`i`、`j`、`k`等一般用作迭代，`cnt`一般用来计数。

```cpp
struct MyStruct {
    int val_a;
    int val_b;
    char class_type;
    enum Type {TYPE_A, TYPE_B};
    
    bool isValid() { return val_a > 10 && val_b > 20; }
}
```

## 注释

写代码的时候记得添加一些注释。良好的注释可以方便自己和其他编程者阅读和修改代码。

通常，对于一个函数应该写明其具有的功能、函数各个参数的意义；对于变量要有对其作用的介绍。这样性质的注释一般写在函数或变量的**声明**处附近。而对于函数的定义部分也最好有相应的解释，可以告诉读者某行或某段代码实现了什么事情、或者为什么这样写。也应注意在关键部分给出相应的提示。

> 很多编辑器、IDE 可以识别到这些注释，并在鼠标悬停在它们的调用上时显示出函数的原型和注释，很是方便。

函数、变量名本身也应该体现一定的意义。如果得当，那么代码则具有**自述性**（self-explaining），而不必要额外再写注释了。

不建议用中文拼音为符号命名，更不要用中文缩写，因为中文的缘故，同音词很多，相同拼音首字母的词语更多，别人很难联想到具体是什么词语。既然是用26个英文字母编程，那么也建议使用英语给变量起名。

> 起变量名在一定程度上也需要一定的英语考究，比如一些词虽然意思相近，但其中的一个会比其他的更合适。

下面是“选猴王”（也就是“约瑟夫问题”）的参考代码：

```cpp
// return the postion number of the monkey king
int get_monkey_king(int n, int m) {
    std::queue<int> monkeys_queue;
    for (int i = 1; i <= n; ++i) monkeys_queue.push(i);
    cnt = 0;
    while (monkeys_queue.size() > 1) {
        ++cnt;
        auto monkey = monkeys_queue.front();
        monkeys_queue.pop();
        if (cnt == m) cnt = 0; 
        else monkeys_queue.push(monkey);
    }
    std::cout << monkeys_queue.front() << std::endl;
}

int main() {
    std::cin >> n >> m;
    std::cout << get_monkey_king(n, m);
}
```

## 编码问题

再次重申，在编码、写文件时，**请使用 Unicode 编码**，它更为通用，使用范围更广泛。虽然一些 Windows 平台上的 IDE，如 Dev-C++ 等只支持 GBK 编码，但是请**不要将就**它们，计算机不是一门守旧的科学。

关于编码问题我们在“开始编程”章节的“代码规范”进行了一些讲述，这里补充一个关于 C/C++ 的知识点。

> 当在**简体中文**版的 Windows 上使用 `gcc` 编译链接 UTF-8 格式且含有中文的源代码生成可执行文件时，你可能会惊讶的发现窗口中输出了一堆乱码。这是由于 `gcc` 自动推断出文件的编码（UTF-8）并以 UTF-8 格式输出可执行文件（即其中的字符串以 UTF-8 的形式硬编码进了可执行文件），而这样编码的程序在简中版的 Windows 上执行时就会发生问题（即以错误的方式读取了）。
>
> 一个**妥协**举措是给 `gcc` 加上一个参数 `-fexec-charset=GBK`，这样输出的可执行文件就能正常执行了。
>
> 关于具体的信息，可以查询有关“控制台代码页”的信息。


> **另请参阅**：
>
> - [小规则让你写出漂亮又高效的程序\_肥宝的实验室 - CSDN 博客](https://blog.csdn.net/u012175089/article/details/51078360)
>
> - [怎样写出无法维护的代码\_肥宝的实验室 - CSDN 博客](https://fable.blog.csdn.net/article/details/51035815)；知乎：[Link](https://zhuanlan.zhihu.com/p/228498043)

