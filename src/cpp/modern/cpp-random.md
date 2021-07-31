# 用 C++ 的 random 库生成更好的随机数

## C 语言中的 `rand()` 和 `srand()`

学习过 C 语言的读者可能熟悉用 `rand()` 生成随机数的方法，`rand()` 会返回一个伪随机数，范围在 `[0, RAND_MAX)` 之间；根据实现的不同，`RAND_MAX` 的值也会不同，但至少应为 32767。

使用方法如下：

```cpp
#include <stdio.h>
#include <stdlib.h>

int main()
{
    for (int i = 0; i < 5; i++) printf("%d ", rand());  
    return 0;
}
```

如果像上面的程序那样，不事先调用 `srand()`，那么每次程序执行时都是假设调用了 `srand(1)`，结果就是每次得到的随机数序列都是一样的。

也就是说，如果以不同的参数值调用 `srand()`，`rand()` 生成随机数的起点就会不一样。

```cpp
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main()
{
    srand(time(0));
    for (int i = 0; i < 5; i++) printf("%d ", rand());
    return 0;
}
```

`time()` 函数每次会返回一个 `time_t` 类型的值，这样，根据调用的时间不同，生成的随机数也就不同了。

假如想要一个 3 ~ 8 的随机值，你会怎么做呢？也许你会用 `3 + rand() % (8 - 3 + 1)`。但是其实这样是不对的。

为了方便说明，我们假设随机数发生的范围是 1 到 32，且 1 到 32 每个数字出现的概率是相等的，那么调用 32 次 `rand() % 5` 得到的数据和次数可能如下表：

| `rand() % 5` 的值 | 次数 |
| :---------------: | :--: |
|         0         |  6   |
|         1         |  7   |
|         2         |  7   |
|         3         |  6   |
|         4         |  6   |

可见，数据 0 到 4 出现的概率并不是相等的。

那该怎么写呢？[std::rand - cppreference.com](https://en.cppreference.com/w/cpp/numeric/random/rand) 这里给出了一个示例，下面直接复制粘贴：

```cpp
#include <cstdlib>
#include <iostream>
#include <ctime>
 
int main() 
{
    std::srand(std::time(nullptr)); // use current time as seed for random generator
    int random_variable = std::rand();
    std::cout << "Random value on [0 " << RAND_MAX << "]: " 
              << random_variable << '\n';
 
    // roll 6-sided dice 20 times
    for (int n=0; n != 20; ++n) {
        int x = 7;
        while(x > 6) 
            x = 1 + std::rand()/((RAND_MAX + 1u)/6);
            // Note: 1+rand()%6 is biased
        std::cout << x << ' ';
    }
}
```

一共能产生的随机数数目为 `RAND_MAX + 1`，所以通过 `rand()/(RAND_MAX + 1)` 可以均匀的得到 0 至 1 之间的浮点数，将这个数再乘以一个数 k 就可以得到 0 到 k 之间的数了。

生成 \\(\[a,b\]\\) 之间随机数的公式：

\\[
a + \\cfrac{\\tt rand()}{N} \\times (b-a+1) = a + \\cfrac{\\tt rand()}{\\cfrac{N}{(b-a+1)}}
\\]

## 使用 C++ 的 random 库

C++ 中为我们提供了更好用，功能更全面的随机数生成。


在这篇微软的文档中是 [不推荐使用 `rand()`](https://docs.microsoft.com/zh-cn/cpp/c-runtime-library/reference/rand?view=msvc-160) 的：

关于 \<random\> 的介绍：[\<random\> | Microsoft Docs](https://docs.microsoft.com/en-us/cpp/standard-library/random?view=msvc-160) 机器翻译的[中文版](https://docs.microsoft.com/zh-cn/cpp/standard-library/random?view=msvc-160)

其他参考资料：

- [C++ 11 的随机数 - 拾荒志 (murphypei.github.io)](https://murphypei.github.io/blog/2019/10/cpp-random)
- [C++ 随机数 - Sail (sail.name)](https://www.sail.name/2018/08/07/random-number-of-C++/)
- [C++ 生成随机数 (ccyg.studio)](http://blog.ccyg.studio/article/91e20a65-45d1-49e8-b28a-a33b6ac0f96b/)


那么我们用 C++ 该怎么生成随机数呢？

以下内容主要来自[Pseudo-random number generation - cppreference.com][Pseudo-random-num-gen]：

C++ 有一个随机数的库，定义在头文件 `<random>` 中，提供可以生成随机数和伪随机数的类，这些类可以分为两类：

- 均匀随机数生成器（uniform random bit generators, URBGs），包括用来生成伪随机数的随机数生成引擎，以及，真正的随机数生成器，如果有可用的设备的话；
- 随机数分布（random number distributions），将URBGs输出的数据转换到多种多样的统计学分布上（如均匀分布、正态分布等）。

一个 URBG 是一个函数对象；有好几种随机数引擎，比如线性同余法（linear congruential algorithm）、梅森旋转算法（Mersenne twister algorithm）、时滞斐波那契算法（lagged Fibonacci algorithm），这些引擎使用种子（seed）作为熵源生成伪随机数；也有好几种随机数引擎配接器，它们使用另一个随机数引擎作为自己的熵源，通常用来改变原来引擎的频谱特性（spectral characteristics）。

有一些预定义的随机数生成器，比如 `mt19937`，是一个32位的梅森旋转算法（32-bit Mersenne Twister by Matsumoto and Nishimura, 1998）。

也可以用 `std::random_device` 生成不确定的随机数，也就是真随机数；当没有随机数的发生条件（比如随机数发生器）时，也可以用伪随机数生成引擎实现它。

至于随机数的分布，就又有很多种了，就不一一列举了。

那具体怎么在代码中使用呢？下面是从 [std::uniform_int_distribution - cppreference.com](https://en.cppreference.com/w/cpp/numeric/random/uniform_int_distribution) 复制粘贴的一个生成均匀随机数（模拟掷骰子）的例子：

```cpp
#include <random>
#include <iostream>
 
int main()
{
    std::random_device rd;  //如果可用的话，从一个随机数发生器上获得一个真正的随机数
    std::mt19937 gen(rd()); //gen是一个使用rd()作种子初始化的标准梅森旋转算法的随机数发生器
    std::uniform_int_distribution<> distrib(1, 6);
 
    for (int n=0; n<10; ++n)
        //使用`distrib`将`gen`生成的unsigned int转换到[1, 6]之间的int中
        std::cout << distrib(gen) << ' ';
    std::cout << '\n';
}
```

`std::random_device` 会向操作系统请求随机数。

下面是一个展示正态分布的例子，从 [Pseudo-random number generation - cppreference.com][Pseudo-random-num-gen] 复制而来。

```cpp
#include <iostream>
#include <iomanip>
#include <string>
#include <map>
#include <random>
#include <cmath>
 
int main()
{
    // 如果可用，从一个真实的随机数发生设备获得种子
    std::random_device r;
 
    // 在1和6之间随机的选择一个均值mean，用来生成正态分布
    std::default_random_engine e1(r());
    std::uniform_int_distribution<int> uniform_dist(1, 6);
    int mean = uniform_dist(e1);
    std::cout << "Randomly-chosen mean: " << mean << '\n';
 
    // 围绕刚刚选择到的“均值”mean生成一个正态分布
    std::seed_seq seed2{r(), r(), r(), r(), r(), r(), r(), r()}; 
    std::mt19937 e2(seed2);
    std::normal_distribution<> normal_dist(mean, 2);
 
    std::map<int, int> hist;
    for (int n = 0; n < 10000; ++n) {
        ++hist[std::round(normal_dist(e2))];
    }
    std::cout << "Normal distribution around " << mean << ":\n";
    for (auto p : hist) {
        std::cout << std::fixed << std::setprecision(1) << std::setw(2)
                  << p.first << ' ' << std::string(p.second/200, '*') << '\n';
    }
}
```

[Pseudo-random-num-gen]:https://en.cppreference.com/w/cpp/numeric/random


读者可以尝试自行编译运行一下上边的代码，看会输出什么样的结果。

可以继续阅读下面的链接，它们包含更多信息，也有更多实用场景下的例子：

- 怎样在 C++ 中生成随机数？ [How to generate a random number in C++? - Stack Overflow](https://stackoverflow.com/questions/13445688/how-to-generate-a-random-number-in-c)
- 使用 C++11 的 random 库生成随机数：👉 [c++ - Generate random numbers using C++11 random library - Stack Overflow](https://stackoverflow.com/questions/19665818/generate-random-numbers-using-c11-random-library)

