# 编译运行第一个 C/C++ 程序

## Windows 

### Visual Studio 

针对 Windows 用户，我们先介绍安装 Visual Studio 的方法。Visual Studio 是一个完备的 IDE，也就是，提供编辑代码和编译、构建代码等一系列功能。

笔者个人一开始并不喜欢 Visual Studio，觉得它又大又臃肿，配置启动项也比较麻烦，完全不能上手。

但是比较一圈后，发现 Visual Studio 还是最适合新手的，因为安装过程十分简单，而且使用 CMake 作为项目构建管理工具会轻松明了很多：

- 打开浏览器，访问 https://visualstudio.microsoft.com/，选择 Visual Studio -> Community 社区版（社区版是免费的），这会开始下载安装安装器的安装器；我们不需要考虑太多，等**下载完成后直接运行即可**。
    【2021/7/9】由于最近国际形势紧张，链接访问多有不畅，笔者将需要下载的文件的一份拷贝放在了微云上，链接：https://share.weiyun.com/wGvadaSF。
    ![下载 Visual Studio 安装器](./assets/download-vs-installer.jpg)
- 运行下载好的程序，需要一段时间配置安装器，配置好后，一般会直接进入选择“工作负载”的界面。这里我们只需**勾选“C++ 桌面开发”套件**。注意到，界面右下角提示安装需要 8 GB 的磁盘空间，如果有必要，可以更改 Visual Studio 的安装路径。之后确认即可，接下来只需等待下载安装完成。
    ![Visual Studio Installer 选择工作负载](./assets/choose-vs-workload.jpg)

**至此环境就配置好了**。接下来，我们要找一个存放我们写代码的地方。在计算机中新建一个文件夹，里面新建一个名为 CMakeLists.txt 的文本文档。

打开 Visual Studio，选择打开现有文件夹，选择刚刚新建的这个文件夹，确定即可。

然后就可以开始写代码了。

我们可以在 Visual Studio 的“解决方案资源管理器”中，新建一个 C++ 源文件。

> 也可以在刚刚建立的文件夹中直接新建文件（方法：先在资源管理器中选择“显示文件后缀名”，然后新建一个文本文档，默认的名称为“新建文本文档**.txt**”，我们将名称改为“*某文件名***.cpp**”）。

在 Visual Studio 中的“解决方案资源管理器”打开 CMakeLists.txt，修改其中内容——比如源代码文件叫 hello.cpp，我们要添加这样的语句：

```cmake
add_executable(hello hello.cpp)
```

编辑好后保存即可。语句中的“`hello`”是“目标名”；“`add_executable`”表示我们要生成一个可执行文件，而目标名就是最后生成的可执行文件名。目标名可以自定义。“`hello.cpp`”表示要生成这个目标所需要的源文件。

不出意外，执行完上述操作后，我们就能在界面上方的绿色箭头框的下拉菜单里找到刚刚的目标“hello”。这是选择启动项。

> 启动项，即我们要运行的项目。它一般是我们在 CMakeLists.txt 中定义的可执行文件目标。

要启动（launch）一个启动项，我们需要先生成（或者说构建）这个项对应的目标，也就是常说的编译链接。不过不用担心，你**只需要点击绿色箭头**，这一切都会自动完成。

如果，我们想**加入新的 C++ 源文件**，就只需重复上边的操作。比如，我们编写了一个新的程序，存在名为“bye.cpp”的文件里。这时，我们就需要修改刚刚的 CMakeLists.txt 文件，**确保它是下边的样子**。（新建一个文件时，Visual Studio 会弹出是否将其添加到现有目标的对话框，我们选择取消）

```cmake
add_executable(hello hello.cpp)
add_executable(bye-bye bye.cpp)
```

这样，我们的整个项目，就有了如上**两个生成可执行程序的目标**。

需要注意，保存 CMake 配置文件后，我们需要在下拉框里选择对应的启动项，才能运行对应的程序。

## macOS

我们需要安装编译器。Apple 的 XCode 提供有实用的命令行工具。在程序列表中找到“终端”，执行以下命令：

```bash
xcode-select --install
```

安装完成后即可使用命令行编译器了。

在某目录下以纯文本形式保存 C++ 程序的源代码，并切换到工作路径。注意，将命令中的“path/to/your/cpp/file”替换为实际的路径。

```bash
cd path/to/your/cpp/file
```

假设源代码文件名为“src.cpp”，则我们可以使用如下命令编译、链接生成可执行程序“test1”。

```bash
c++ src.cpp -o test1
```

之后，在命令行运行生成的可执行程序：

```bash
./test1
```

## Ubuntu

在终端中，使用包管理器安装需要的命令行工具：

```bash
sudo apt install build-essential
```

其余步骤与 macOS 类似。
