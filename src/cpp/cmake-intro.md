# CMake入门

为什么要使用CMake呢？随着工程变得庞大，构建它会有很多文件的参与，手写 Makefile 等构建文件变得愈发困难且容易出错，使用 CMake 可以简化这个过程；也许，有一个智能的IDE能够帮助你构建文件，但当你把源代码发给别人时，可能对方并没有相应的工具，只能面对源文件措手无策，或者重新编写构建文件，而如果有 CMake，则能由一个文件根据不同的平台生成相应的建构档，事情就会变得轻松而愉快。


> 参考链接：
>
> - [CMake 如何入门？ - 知乎](https://www.zhihu.com/question/58949190)
> - [BrightXiaoHan/CMakeTutorial: CMake中文实战教程 - github.com](https://github.com/BrightXiaoHan/CMakeTutorial) 
> - [如何评价 CMake？ - 知乎](https://www.zhihu.com/question/276415476)
> - CLion的CMake快速入门教程：[Quick CMake Tutorial](https://www.jetbrains.com/help/clion/quick-cmake-tutorial.html)
> - Visual Studio关于CMake项目的讲解：[CMake projects in Visual Studio](https://docs.microsoft.com/en-us/cpp/build/cmake-projects-in-visual-studio)


## 基本的CMake项目

先从最简单的单文件做起，假设现在目录下有一个`main.cpp`文件：

```cpp
#include <iostream>
int main() {
    std::cout << "Hello!";
}
```

我们可以在**和源代码相同的目录下**新建一个`CMakeLists.txt`，然后写入这样的内容：

```cmake
cmake_minimum_required(VERSION 3.13)
project(cmake_test)

set(CMAKE_CXX_STANDARD 14)

add_executable(cmake_testapp main.cpp)
```

这个的例子来自 CLion 的教程。下面是每一行的解释：

| 命令                                     | 解释                                                         |
| ---------------------------------------- | ------------------------------------------------------------ |
| `cmake_minimum_required(VERSION 3.13)`   | 指定CMake的最低版本要求，如果不加会有警告                    |
| `project(cmake_test)`                    | `project`命令定义项目的名称                                  |
| `set(CMAKE_CXX_STANDARD 14)`             | `set`命令用来给变量赋值，这里`CMAKE_CXX_STANDARD`变量用来指定所需最低的C++版本，这里要求C++版本最低为C++14 |
| `add_executable(cmake_testapp main.cpp)` | `add_executable`会添加一个生成可执行文件的“**目标**（target）”，目标的名字为`cmake_testapp`，其将从`main.cpp`被构建（build） |

请读者留意“目标（target）”的概念，这在现代 CMake 中很是重要。

保存`CMakeLists.txt`文件，然后在命令行中输入`cmake .`，便会生成相应的构建文件。

不过通常我们不会在源代码的文件夹里直接构建，因为构建过程会生成很多文件，和源代码会在一起，显得比较杂乱。通常会新建一个目录，可以在当前目录，也可以在任何喜欢的地方，只要在这个目录下将`CMakeLists.txt`的位置告诉 CMake 就可以，然后在这个文件夹里面执行构建的操作，称为“**Out of source building**”。

比如我们在当前目录下新建一个 build 目录，然后在其中进行构建操作。这样，因为 `CMakeLists.txt` 的位置在**上一级目录**，可以用`..`表示。

Unix 下会默认生成 Makefile，生成后执行 make 命令，就会根据 Makefile 中的内容进行一系列操作。

```console
$ mkdir build && cd build
$ cmake ..
$ make
```

Windows 下默认生成 Visual Studio 的项目文件 `*.vcxproj`。

也可以使用`-G`命令指定生成的构建文件。如果之前生成的是其他构建文件，需要先删除构建目录下的 `CMakeCache.txt`。

```console
$ cmake -G "MinGW Makefiles" ..
```

一个项目中可以有多个目标，我们可以指定**多个**生成可执行文件的目标。

比如我们有 `test1.cpp` 和 `test2.cpp` 两个cpp，它们分别会生成不同的程序。可以新建两个名为 `test1` 和 `test2` 的目标（名字随意），修改后的`CMakeLists.txt`如下所示：

```cmake
cmake_minimum_required(VERSION 3.13)
project(cmake_test)

set(CMAKE_CXX_STANDARD 14)

add_executable(test1 test1.cpp)
add_executable(test2 test2.cpp)
```

修改过 `CMakeLists.txt` 之后，需要重新执行 `cmake` 操作，重新生成 CMake 缓存以及构建脚本。如果使用的是IDE，那么可能会自动进行这个过程，或者提示用户进行这个操作。

注：使用`make`时，可以用`make <target>`指定要生成的目标，比如`make test1`。如不指定，则默认全部执行。如果使用了 IDE，可能会有下拉框以供选择。

## 多文件（一）

首先新建一个目录，这个目录就是我们的**项目目录**，比如“cmake_test”，我们的操作都在这个目录下执行，而 `CMakeLists.txt` 一般放在项目根目录下。

```
cmake_test
|   CMakeLists.txt
|   main.cpp
|   solution.cpp
|   solution.h
|
\---build
```



现在，我们的可执行文件的目标需要多个源代码文件生成，比如下面这样，其中 `solution.cpp`、`solution.h`、`main.cpp` 都在同一级目录（项目根目录）下：

```cpp
// solution.h

#ifndef SOLUTION
#define SOLUTION

class Solution {
public:
    static int add(int a, int b);
    static int subtract(int a, int b);
    static int multiply(int a, int b);
};

#endif
```

```cpp
// solution.cpp

#include "solution.h"

int Solution::add(int a, int b) {
    return a + b;
}
int Solution::subtract(int a, int b) {
    return a - b;
}
int Solution::multiply(int a, int b) {
    return a * b;
}
```

```cpp
// main.cpp

#include <iostream>
#include "solution.h"

int main() {
    std::cout << Solution::add(3, 5) << ' ';
    std::cout << Solution::multiply(3, 3);
}
```



注意`solution.cpp`和`main.cpp`中引用我们自定义的头文件的方式`#include "solution.h"`：编译器读取到`solution.cpp`里面的`"solution.h"`时，会根据相对路径寻找相应的文件进行包含，`solution.h`等效于`./solution.h`，也就是和该源代码文件相同目录下的`solution.h`文件，因此编译器能正确的包含（include）这份头文件到源代码文件里。

然后我们的`CMakeLists.txt`这样写：

```cmake
cmake_minimum_required(VERSION 3.13)
project(cmake_test)

set(CMAKE_CXX_STANDARD 14)

add_executable(solution_test main.cpp solution.cpp)
```

这样`solution_test`就会由`solution.cpp`、`main.cpp`构建出来。

是不是很简单？CMake只需要一条语句就能完成，而如果手动使用命令行编译则需要输入四五条命令，而且会随着文件的增多变得愈发棘手。

## 生成库文件

> 参考阅读：
>
> - [add_library — CMake 3.19.3 Documentation](https://cmake.org/cmake/help/latest/command/add_library.html)
>
> - [C++编译器与链接器工作原理_- CSDN博客](https://blog.csdn.net/henry_23/article/details/112691151)

当一些**相对固定**的功能需要经常被其他程序调用，或者有些功能想发给别人但不希望给出源码，我们可以将这些功能生成函数库，这样在使用时只需和可执行文件进行链接，可以节省编译时间。

也很简单，我们使用`add_library`命令即可。还是使用刚刚的代码。我们在上文的`CMakeLists.txt`中添加这样的语句。

```cmake
add_library(test_library STATIC solution.cpp)
```

语句中，`test_library`是我们生成的库的名称，`STATIC`表明生成的为静态链接库，`solution.cpp`则表示使用这个文件生成函数库。

静态库在编译时链接到可执行文件里，而动态库（shared library）则在程序运行时被加载。动态库有时也被称为共享库（shared library）。

> 注：生成的链接库的文件名会在指定的库名称的前边加上`lib`，搜索时也会根据指定的名称再在前边加上`lib`得到的名称进行搜索。比如，上述例子生成的库文件名就为`libtest_library`。

编辑好`CMakeLists.txt`后，不要忘了重新运行一次CMake。

执行生成函数库的这个目标（target）后，一个静态库就会生成在构建目录下。

关于生成的函数库**如何使用**，我们**稍后**再讲解。

## 多文件（二）：有层级的目录

当头文件和源代码文件被放在了不同的目录、不同层级的目录时，事情会变得稍微复杂一点。

比如以我们刚刚的三个文件为例，**现在**把它们放在**不同的目录**里，层级如下图所示：

```
cmake_test
|   CMakeLists.txt
|   main.cpp
|
+---include
|       solution.h
\---src
        solution.cpp
```

我们依然可以使用相对路径来包含头文件：

**main.cpp**

```cpp
#include "include/solution.h"
...
```

**solution.cpp**

```cpp
#include "../include/solution.h"
...
```

这样，编译器在编译`cpp`文件时依然能正确包含到相应的头文件，但是这样的写法不仅繁琐，也会给维护带来不少麻烦：比如随着代码的增多，代码根据新的逻辑被重新划分到了新的若干目录中，那么可能所有源代码中的`include`命令**都要重新修改**，才能让它们重新找到头文件。

所以，对于我们自定义的头文件，我们可以在包含它时**只写头文件的名字**，然后为编译器指定相应的搜索路径，这样，编译器在遇到一个需要包含的头文件名时，就会在其知晓的搜索路径中查找这个文件，即系统头文件路径等、以及用户指定的包含路径（include path）。而当我们的文件目录层级发生变化时，只需要**重新指定头文件的搜索路径**，而不用去修改每一个源代码文件。

### 添加包含路径（include path）

在CMake中，我们可以用`include_directories`命令指定包含路径。

```cmake
include_directories(include)
```

这行命令表示，我们把项目根目录下的`include`目录添加到头文件的搜索路径中。这项设置是全局的，也就是任何文件都能访问到`include`目录下的文件。

现在我们的`CMakeLists.txt`应该长这样：

```cmake
cmake_minimum_required(VERSION 3.13)
project(cmake_test)

set(CMAKE_CXX_STANDARD 14)

include_directories(include)

add_library(test_library STATIC src/solution.cpp)
add_executable(solution_test main.cpp src/solution.cpp)
```

注意现在`solution.cpp`的路径发生了变化，应该写作`src/solution.cpp`。



## 链接函数库

> 参考链接：
>
> - [find_library — CMake 3.19.3 Documentation](https://cmake.org/cmake/help/latest/command/find_library.html)
> - [target_link_libraries — CMake 3.19.2 Documentation](https://cmake.org/cmake/help/latest/command/target_link_libraries.html)

之前我们执行了`test_library`目标后，生成了一个静态库文件`libtest_library.a`，现在来使用它。

默认该文件生成在构建目录下。我们在项目根目录下新建一个`lib`目录，然后把静态库文件复制进去。

我们使用CMake的`find_library`命令来寻找函数库文件：

```cmake
find_library (TEST_LIB test_library lib)
```

这条命令表示：在`lib`目录中查找名为`test_library`的库，并新建一个`TEST_LIB`变量，把结果存放其中。

之后，还要使用`target_link_libraries`命令把库和我们的可执行文件链接起来，这样才能正确执行它。

现在，我们的`CMakeLists.txt`应该这样写了：

```cmake
cmake_minimum_required(VERSION 3.13)
project(cmake_test)

set(CMAKE_CXX_STANDARD 14)

include_directories(include)

add_library(test_library STATIC src/solution.cpp)
add_executable(solution_test main.cpp)

find_library (TEST_LIB test_library lib)
target_link_libraries(solution_test ${TEST_LIB})
```

注意，这里我们`add_executable`命令中不再使用`src/solution.cpp`了，而是在最后将可执行文件和由其生成的库文件链接起来。

其实，`target_link_libraries`命令还可以有其他的方式，比如**项目中已有的**“库文件目标”的名称，即使用`add_library()`创建的目标。这样就不需要`find_library`命令了：

```cmake
target_link_libraries(solution_test test_library)
```

也可以指定一个**具体的路径下的**库文件进行链接：

```cmake
target_link_libraries(solution_test ${PROJECT_SOURCE_DIR}/lib/libtest_library.a)
```



## 选择构建类型（build type）

构建类型有Debug和Release，其中Debug会包含调试信息，Release一般会进行额外的优化来提高运行效率，也会增加编译时间。

可以在运行CMake时指定参数：

```console
$ cmake -DCMAKE_BUILD_TYPE=Debug ..
```

命令行中的`-D`表示参数设置，将`CMAKE_BUILD_TYPE`的值设置为`Debug`。

如果使用IDE，也会有相应的设置。



## 使用其他包（package）

编程中，我们会遇到需要使用其他包 / 函数库的时候，比如OpenCV、Boost等。

为了清晰，我们新建一个目录，名为`opencv_test`。

下面我们调用OpenCV，写一个简单的读取并显示图像的程序：

**show-img.cpp**

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>

int main(int argc, char* argv[])
{
	std::string filename;
	if (argc == 2) { filename = argv[1]; } 
    else {
		std::cout << "Input Filename: ";
		std::cin >> filename;
	}
	cv::Mat img = cv::imread(filename);
	if (!img.empty()) {
		std::cout << "Image loaded successfully.\n";
		cv::namedWindow(filename, cv::WINDOW_NORMAL);
		cv::imshow(filename, img);
		cv::waitKey(0);
	} else {
		std::cout << "Unable to open '" << filename << "'.\n";
	}
	return 0;
}
```



> 参考链接：
>
> - [find_package — CMake 3.19.3 Documentation](https://cmake.org/cmake/help/latest/command/find_package.html)



然后，我们可以使用`find_package`命令：

```cmake
find_package(OpenCV REQUIRED)
```



`REQUIRED`顾名思义，表示这个包是必须的，没有它不行；如果没有找到，则会中断进程，抛出错误信息。

可以在后边跟一个`MODULE`，表示告诉`find_package`使用“Module”模式寻找指定的包。该模式下，CMake会寻找一个名叫`Find<PackageName>.cmake`的文件，在该例下就是`FindOpenCV.cmake`。首先，CMake会在`CMAKE_MODULE_PATH`寻找这个文件，默认情况下这个变量是空的；然后在安装CMake时提供的“寻找单元（Find Modules）”中寻找。

在不指定`REQUIRED`的情况下，CMake会先使用“Module”模式寻找，如果寻找无果，则会进入另一种“Config”模式。这种模式下，CMake则会寻找`<PackageName>Config.cmake`文件，在该例下也就是`OpenCVConfig.cmake`文件。

用户也可以将`CMAKE_FIND_PACKAGE_PREFER_CONFIG`的值设置成`TRUE`，这样`find_package`优先使用“Config”模式。当用户想要使用了一个常见库的自己编译的版本时，这项功能可能会比较实用。

也可以直接指定`CONFIG`模式，或者使用其同义词`NO_MODULE`，这样只会进行“Config”模式的搜索。

查找成功后，CMake会把`<PackageName>_DIR` 变量的值设置为其找到`.cmake`文件的路径，而文件一般会提供一些有用的信息，以变量的形式供用户使用。

比如OpenCV会设置`OpenCV_INCLUDE_DIRS`、`OpenCV_LIBS`等变量，`OpenCV_INCLUDE_DIRS`是OpenCV的包含目录路径，`OpenCV_LIBS`则包含链接所需的OpenCV的库文件。

> 注：虽然成功找到 OpenCV 的安装之后，会写入`OpenCV_DIRS`变量，但并不需要使用`include_directories(${OpenCV_INCLUDE_DIRS})`命令。

所以，我们的`CMakeLists.txt`可以这样编写：

```cmake
cmake_minimum_required(VERSION 3.13)
project(opencv_test)

find_package(OpenCV REQUIRED)

add_executable(show_img show_img.cpp)

target_link_libraries(show_img ${OpenCV_LIBS})
```

如果正确安装了OpenCV，理论上就能找到。

也可以给`<PackageName>_DIR` 变量指定值，则CMake会先在指定路径下寻找`.cmake`文件。



## install

https://blog.csdn.net/ktigerhero3/article/details/68941252/



## configure_file



## 其他

参见 CMake in VS Code



