# 建立一个命令行工作环境

开始这部分前，读者需要了解：

- Windows 下软件的安装

一般所谓的命令行工作环境，包含终端 + Shell（提供基础操作，如文件创建、网络通信），C/C++ 编译器（用来构建其他很多软件的必要程序），包管理器以及其他形形色色的工具等。

## 配置包管理器

每个操作系统一般都具有命令行操作的界面，也有相应的一些命令行工具或者实用 Shell 命令。但是像 Windows、macOS 等操作系统并不具有包管理器，通常需要用户手动安装包管理器。

通常，这些包管理器会提供一个 Shell 脚本，用户只需要下载并执行这个脚本即可完成包管理器的安装。这个脚本所执行的配置过程一般包含“下载程序”和“将程序添加到环境变量”两个步骤。当然，也有可能需要用户手动将包管理器可执行文件所在的位置添加进环境变量，具体要根据软件的说明来决定。

> 实际上，由于互联网环境的复杂性，这个过程通常并不流畅，可能需要用户修改脚本中的内容，以便更好的执行程序的下载等任务。

包管理器通常是 CLI 程序。将其可执行文件添加进环境变量中之后，我们就可以在 Shell 中调用这个包管理器了。

Windows 上可用的包管理器有 Chocolatey，其主要针对日常软件的安装；针对软件开发则有 vcpkg，可以安装构建程序所需的依赖。macOS 上常用 Homebrew。

## 配置工作环境

### 1. Windows

Windows 已经带有 Win32 控制台和 PowerShell 环境，其实是具有基础的命令行工作的环境的。

除此之外，我们可以再选择使用一些包管理器管理自己的软件包。

要打造开发环境，我们还需要 C/C++ 编译器。可以选择安装“MSVC 生成工具”，这会带来 MSVC 编译器等工具。当然，也可以选择直接安装 [Visual Studio](https://visualstudio.microsoft.com/)，不过这是一个完整的 IDE，其体量比较庞大。


### 2. 在 Windows 上体验类 Unix 环境

macOS 以及诸多 Linux 的发行版的命令行工作环境是类似的（默认都为 bash，也可选择 zsh，其使用也与 bash类似）。两者之间有着类似的工具，命令几乎可以通用。而 Windows 的 PowerShell 命令则别出一格，很多功能、操作的实现所用的命令并不相同。

如果有必要，我们可以选择再在 Windows 上搭建一个类 Unix 的环境。常用的有安装虚拟机（在虚拟机上运行类 Unix 系统）、使用 MSYS2、使用 WSL（Windows Linux 子系统）等。

#### 概念介绍：工具链

我们先来介绍一下工具链（Toolchains）的概念。顾名思义，其意为一套“连锁”的工具，通常包含构建所需的编译器，以及用于安装软件库的包管理器等等。

比如在 Ubuntu 上，我们通常使用 GNU GCC 作为编译器，使用 APT 作为包管理。在 macOS 上，我们使用 Apple-Clang 编译器，并可以选择 Homebrew 作为包管理器。在 Windows 上，我们可以使用 MSVC 作为编译器，使用 vcpkg 管理包（也可以手动管理各种软件包）；也可以安装 MSYS2，使用 MinGW gcc 作为编译器，使用其携带的包管理器 pacman 管理软件包；也可以使用 WSL 上的环境（Linux 环境）进行开发。

#### 法一：安装虚拟机

安装虚拟机可以体验最接近真实的 Linux 发行版体验。其问题主要在于联动性和效率上。

#### 法二：MSYS2

如果条件不足以安装虚拟机、或者不希望使用虚拟机，我们可以选择安装模拟类 Unix 系统的操作的软件。有很多种方式，这里我们介绍 MSYS2 的使用。

MSYS2 安装后有三个环境，“MSYS2 MSYS”、“MSYS2 MinGW 64-bit”、“MSYS2 MinGW 32-bit”。我们常用的是“MSYS2 MinGW 64-bit”环境。

> 这里需要提一下 MinGW gcc，我们可以简单地将其理解为 Windows 上可用的（原生的） gcc 编译器。

MSYS2 为我们提供了类似 Linux 发行版的操作环境。在 MSYS2 的安装目录下有一个“home”目录，这个目录就是我们 MSYS2 环境中的家目录。MSYS2 环境中，Windows 下的各盘符被挂载在根目录下对应的目录上，比如“C盘”被挂载在“`/c/`”下，“D盘”被挂载在“`/d/`”下。

在“MSYS2 MinGW 64-bit”环境中，C/C++ 编译器为“MinGW gcc 64-bit”，其可以生成 Windows 平台下原生的可执行文件（exe）。其包管理器安装的软件也是由 MinGW gcc 构建的软件。我们把这一整套称为“MSYS2 MinGW 64-bit”工具链。

> 我们也可以不使用 MSYS2，只安装 MinGW gcc 作为编译器，并手动安装其他需要的工具和包。

### 3. Ubuntu

Ubuntu 默认没有安装编译器，我们可以通过包管理器安装所需的软件工具等。

```bash
sudo apt update
sudo apt install build-essential
sudo apt install cmake
```

### 4. macOS

首先，我们要安装一些基础的命令行工具。Xcode 是 Apple 的开发工具，我们可以选择只安装命令行工具。

```bash
xcode-select --install
```

键入以上命令后，macOS 会开始安装 Xcode 命令行工具，其中包括我们需要的编译器（Apple-Clang）。

macOS 默认没有包管理器。安装完成后，我们可以选择安装包管理器——Homebrew。具体安装步骤和使用这里不再赘述。

> 当然，选择安装 Xcode，也会安装 Xcode 命令行工具。但是作为一个完备的 IDE， Xcode 会十分庞大。

### 5. 代码编辑器

常用的代码编辑器可以使用 [Visual Studio Code](code.visualstudio.com)，其跨平台可用，且免费开源；虽然只是编辑器，但有海量插件可以实现各种自动化的操作，某种程度上可以当成 IDE 使用。

可以说，不管在哪个平台上，VSCode 都是非常厉害好用的。不过在使用 C++ 的代码智能感知插件时要记得清理缓存，默认的上限是 5 GB，可以修改。

### 6. 集成开发环境

集成开发环境（IDE）是包含了代码编辑、调试、协作等诸多功能的软件。除了我们刚刚提到的 Xcode 和 Visual Studio 等，也有 JetBrains 系的 IDE，如 CLion、PyCharm 等。不同于前两者，JetBrains 系的 IDE 一般不会附带安装构建需要的工具。这时就需要使用我们之前配置的编译工具链，有时也称为“外部生成工具”。

JetBrains 系的 IDE 也是跨平台的，社区版免费；同时提供学生优惠，即用教育邮箱（或其他方式）认证后可以使用专业版。其产品包括开发 C/C++ 项目用的 CLion ，也有编写 Python 的 PyCharm。

[如何评价 JetBrains 的新 C/C++ IDE CLion？](https://www.zhihu.com/question/25259569)

如今，Visual Studio 提供免费的社区版，而商业版本需要收费。Visual Studio 在使用过程中可能会在项目文件夹下产生很大的缓存，使用时需要注意。

现代 IDE 基本都有**代码补全**、**语法检查**等功能。为什么强调这些功能呢？比如代码补全功能不需要编程者记录确切的函数名，只需要几次击键就可以联想出函数名，可以提高编程效率，同时也能降低错误率。而语法检查则能让编程者及时发现错误，同样能够提高效率。

### 7. 类 Unix 环境的好处

类 Unix 环境下开发，能够方便的找到标头文件（C++），而使用包管理器，也给我们软件的安装带来很多便利，对于常见依赖问题的解决比较方便。

在 Windows 上使用如 MSYS2 这样的环境便是出于这样的考虑。但是模拟的环境毕竟不如原生，而且 Visual Studio 也推出了自己的 vcpkg 包管理器，并且也支持 CMake 管理 C/C++ 项目，寻找依赖也很方便。




扩展阅读：

> [MinGW、MinGW-w64 与 TDM-GCC 应该如何选择？ - 知乎](https://www.zhihu.com/question/39952667)
>
> [如果仅考虑 Windows 平台，不用 msvc 而去用 gcc 的理由有哪些？ - 知乎](https://www.zhihu.com/question/41733001)
>
> [Qt 用 MSVC 和 MinGW 哪个编译器编译程序比较好？ - 知乎](https://www.zhihu.com/question/331375227)
>
> [怎么安装 mingw 离线包？ - 知乎](https://www.zhihu.com/question/313334589)
>
> [如何评价 MSYS2 以及未来发展方向如何？](https://www.zhihu.com/question/37025275)
