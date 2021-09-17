# 常用命令

开始这篇内容之前，读者需要：

- 了解终端、Shell、CLI 等；
- 理解文件和目录的概念；

## 命令行选项的惯例

观察下面的命令：

```console
$ gcc test1.c -o test1
$ apt list --upgradable
```

这里的“`list`”“`--upgradable`”都是传递给“`apt`”程序的参数；或者说，“`--upgradable`”是用来修饰“`list`”命令的**命令行选项**。

形如“`-o`”这样的命令行选项被称为“短选项（short option）”，更长的则为“长选项”，而形如“`--option`”这样以两个连字符为前缀的完整单词则是 GNU 规范中的长选项格式。

如今，很多软件遵从 GNU 规范的风格设计命令行参数，但这并不能说明其是一个硬性的要求。


```console
$ pacman -Syu
$ jar cvf app1.jar app1
```

比如，也有些软件会采用如同“`-Syu`”这样的格式，“`S`”“`y`”“`u`”三个字母各有含义。而像“`cvf`”这种没有任何前缀字符的命令行选项格式，由于历史原因，至今也仍在使用。

此外，“sub-command”的情况也比较常见：

```console
$ git commit -m "message"
```

总的来说，不同命令行选项的格式都是历史上探寻最佳设计过程中的产物。请读者阅读 [Command-Line Options (catb.org)](http://catb.org/~esr/writings/taoup/html/ch10s05.html) 以获得更详细的介绍：

Unix 传统鼓励使用命令行参数来控制程序的运行，因为这样可以方便的通过脚本来控制应用程序

根据最初的 Unix 传统，命令行选项是以短横线为前缀的**关键字母**，如“`-a`”“`-b`”。如果该命令行选项有参数，则在其后注明，比如“`-a 10`”。对于没有参数的命令行选项，可以将其合并在一起，比如“`-ab`”或者“`-ba`”。

由于单字母的选项用尽了，GNU 风格开始使用以两个连字符为前缀的**关键词**。命令行选项和其参数之间可以以空格或等号“`=`”间隔，如“`--lines=10`”。这种方法很流行，因为阅读起来更为清晰，并且能和之前的单字母无歧义地组合在一起。

---

参考链接：

- [bash - Short/long options with option argument - is this some sort of convention? - Stack Overflow](https://stackoverflow.com/questions/10818443/short-long-options-with-option-argument-is-this-some-sort-of-convention)
- [What is the general syntax of a Unix shell command? - Stack Overflow](https://stackoverflow.com/questions/2160083/what-is-the-general-syntax-of-a-unix-shell-command)



## 实用技巧

1\. 自动补全

在敲出 文件／目录／命令 的前几个字母之后，按下 `Tab` 键，如果输入的没有歧义，系统会自动补全。

如果还存在其他 文件／目录／命令，再按一下 `Tab` 键，系统会提示可能存在的命令

2\. 曾经使用过的命令

按键盘上的 上／下 方向键，可以在曾经使用过的命令之间切换。

## Linux 基本命令

这里只介绍简单的几个命令，更详细的需要读者自行查找资料。

- 查看目录内容 `ls`
- 切换目录 `cd`
- 创建和删除操作 `touch`、`rm`、`mkdir`
- 拷贝和移动文件 `cp`、`mv`

### ls：列出目录的内容

`ls` 是英文单词 list 的简写，其功能为列出目录的内容，是用户最常用的命令之一，类似于 DOS 下的 `dir` 命令

| 参数 | 含义                                         |
| ---- | -------------------------------------------- |
| -a   | 显示指定目录下所有子目录与文件，包括隐藏文件 |
| -l   | 以列表方式显示文件的详细信息                 |
| -h   | 配合 -l 以人性化的方式显示文件大小           |

Linux / Unix 操作系统下，以“`.`”开头的文件为隐藏文件，需要用 `-a` 参数才能显示

```console
$ ls -a      # 显示目录下的所有文件/目录
$ ls -a -l   # 以列表方式显示目录下的所有文件/目录的详细信息
```

### cd：切换目录

`cd` 是英文单词 change directory 的简写，其功能为更改当前的工作目录，也是用户最常用的命令之一。

注意：Linux 所有的 **目录** 和 **文件名** 都是大小写敏感的

```console
$ cd ..      # 切换到上级目录
```

| 命令    | 含义                                                 |
| ------- | ---------------------------------------------------- |
| `cd ~`  | 切换到当前用户的主目录（家目录，亦即`/home/用户名`） |
| `cd .`  | 保持在当前目录不变                                   |
| `cd ..` | 切换到上级目录                                       |
| `cd -`  | 可以在最近两次工作目录之间来回切换                   |

### pwd：查看当前目录

直接在终端中执行 `pwd`，即可获得当前目录的路径。

```console
$ pwd
```

### 使用输出重定向创建文件

我们可以用**输出重定向**的方式新建一个文件。

```console
$ echo "" > hello.txt 
```

上述的命令将会输出空内容到“hello.txt”文件，如果文件不存在，则将会创建一个新文件；如果文件已存在，则将覆盖掉文件原有的内容。

### mkdir：创建一个新的目录

新建目录的名称不能与当前目录中**已有的目录或文件**同名

```console
$ mkdir workspace
$ cd workspace
$ cd ..
```

### rm：删除文件或目录

使用 rm 命令要小心，因为文件删除后不能恢复

| 选项 | 含义                                                  |
| ---- | ----------------------------------------------------- |
| `-f` | 强制删除，忽略不存在的文件，无需提示                  |
| `-r` | 递归地删除目录下的内容，**删除目录** 时必须加此参数 |
| `-v` | 可以显示出该命令具体执行了哪些操作                    |

### 拷贝和移动文件

| 命令                 | 对应英文 | 作用                                 |
| -------------------- | -------- | ------------------------------------ |
| `cp 源文件 目标文件` | copy     | 复制文件或者目录                     |
| `mv 源文件 目标文件` | move     | 移动文件或者目录／文件或者目录重命名 |

参考链接：[文件和目录常用命令 - davidabdy - 博客园 (cnblogs.com)](https://www.cnblogs.com/zkpythonstudy/p/9960512.html)

### 任务控制

很多 Shell 支持“任务控制（job control）”。下面介绍 bash 中的任务控制。

在命令后附加 `&` 字符可以使任务在后台（Background）运行，如：

```console
$ sleep 1000 &
[1] 379
```

数字 `379` 是该任务的进程 ID。

运行在后台的程序如果有输出，也会直接输出到控制台。

使用 `jobs` 命令可以列出后台运行的程序，输出结果编号后的 `+` 表示这是当前的任务，`-` 表示这是之前的任务。

使用 `fg` 命令，可以将后台运行的程序提到前台（Foreground）。`fg` 命令后的数字、`+` 或者 `-`，可以引用 `jobs` 命令输出结果中对应的任务。

```console
$ sleep 100 &
[1] 416

$ sleep 100 &
[2] 417

$ sleep 100 &
[3] 418

$ jobs
[1]   Running                 sleep 100 &
[2]-  Running                 sleep 100 &
[3]+  Running                 sleep 100 &

$ fg %2
sleep 100 &
```

当程序运行在前台时，可以按下 `Control` + `C` 发送信号，终止程序的运行。

按下 `Control` + `Z`，可以使程序暂停（Stopped），并切换至后台。这时通过 `jobs` 命令可以看到该进程处于“Stopped”状态。

使用 `bg` 命令，可以将某编号的任务在后台运行。即，可以使用 `Control` + `Z` 组合键和 `bg` 命令，将一正在前台运行的程序移至后台运行。

```console
$ sleep 100
^Z
[1]+  Stopped                 sleep 100

$ jobs
[1]+  Stopped                 sleep 100

$ bg % 1
[1]+ sleep 100 &

$ jobs
[1]+  Running                 sleep 100 &
```

需要注意，无论将一处于“Stopped”状态的任务移至前台还是后台，该任务都会继续开始执行。

---

参考资料：

- [Understanding the job control commands in Linux – bg, fg and CTRL+Z - The Geek Diary](https://www.thegeekdiary.com/understanding-the-job-control-commands-in-linux-bg-fg-and-ctrlz/)
- [Job Control (Bash Reference Manual) - gnu.org](https://www.gnu.org/software/bash/manual/html_node/Job-Control.html)

## PowerShell

上述提到的命令，在 PowerShell 中也是基本适用的。

## 相关内容

- [《Advanced Bash-Scripting Guide (高级Bash脚本编程指南)》中文版](https://doc.yonyoucloud.com/doc/Advanced-Bash-Scripting-Guide-in-Chinese/index.html)：链接中有原文地址，也有项目的Github地址

- https://www.landiannews.com/archives/tag/VMWare

