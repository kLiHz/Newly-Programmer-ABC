# 准备篇：什么是计算机？

## 数据

在开始一切前，我们希望向读者介绍“数据”的概念。人类活动总会产生数据，人类也希望记录下这个过程中产生的有意义的数据，并对其进行处理和分析。

从原始社会开始即是如此。比如，人想记录“捕获了多少猎物”，或者“养了多少只动物”，从尝试描绘实物的“壁画”到抽象的“结绳记事”。再后来，人们发明了“文字”这一伟大创举，可以用有限的符号组合，表示出广泛的含义，人们也因此可以以更高效的方式记录下更多生活中的事件。曾经，5 只羊可能需要用五幅图画或者 5 个绳结来表示，而通过文字，我们可以用表示数量“5”的符号和表示物品“羊”的符号的组合来表示这个事物。人们也可以用诸如人称代词“你”“我”和表示行为的词“喜欢”的方式来表示对对方的爱慕。

以上，便是数据的产生、表示以及记录。其中，诸如以 5 个绳结表示 5 只羊的“抽象”的存在，是不可避免的。

## 一些概念的介绍

初中阶段的英语学习告诉我们，电脑的英文是“Computer”。我们知道，“-er”后缀表示经常从事某项动作者，因此可以说，“Computer”就是一种经常进行“Compute”的机器。那，什么是“Compute”呢？

字典告诉我们，“Compute”作及物动词时为“以**数学**的方式来确定某值”，作非及物动词时即为“计算”的意思，意思和“Calculate”接近。可以看出，计算机做的是严谨的数学运算。

是的，尽管现在计算机多以手机、电脑等形式出现，且通常被用于播放视频、进行聊天游戏等用途，但计算机们最早的前身确实是在从事一次又一次单调的计算操作（其实现在也是）。无论是计算两个数相加的结果，还是显示绚丽的图片和视频，它们背后都是简单的运算，只不过是几次和千万次运算的差别。

而归根结底，人们使用计算机就是在和数据打交道。不管是记录信息、读取信息、分享信息，还是从信息中获取更有价值的信息，都是对数据进行的存储、读取和处理。

而计算机需要通过人们编写程序来实现对数据的存储、访问、处理。所谓程序（program），也就是一系列有机组合在一起的指令（command）。

### “一切皆数”

如我们前文所说，计算机的工作就是做大量的计算。因此，如果我们想用**计算机**帮我们**存储**或者**处理**信息——比如录入文本、存储照片、观看视频等——就必须把信息用数字表示出来。这个过程就是所谓的“编码”。

我们日常生活中接触到的数据，几乎都可以使用文字描述出来。那么，通过给我们使用的文字和符号编号，就能用若干数字表示一段文本了，进而表示数据了。

不过，我们常说“百闻不如一见”，视觉和听觉上的体验有时也是十分重要的。现代科学告诉我们，视觉是由光波刺激生物的感光细胞产生的电信号经由神经传输至大脑而产生；而声音的本质则是波在空气中的传播。如果有一种方法可以记录下色光和波形，那么也就可以记录下图像和声音了。

对此，我们依然可以采取“编号”的方式，比如，我们可以用 1 ~ 100 的 100 个数字，表示 100 种颜色，而将若干颜色点（也叫“像素”）排列在一起，就能拼出一幅图像了。而要记录波形，我们可以在波形上选取若干能够代表波形的点，用数值记录下波在此处的强度，以此便得以记录下一个波形，进而记录一段声音了。

> 关于文本编码的问题这里只简单的介绍一下，便于读者理解：最基础的编码有“美国标准信息交换码（ASCII）”，但是它大概只覆盖了基本的英文字母和阿拉伯数字，对于使用英语的用户来说可能够用了，但是对于其他国家的用户来说，要表示自己的文字很显然不够用了，于是各国便开始设计自己的编码方式，比如 GBK、Big5 等等，但是因为这些都是独立开发的，同一个数码在不同的编码下会表示不同的字符，不同编码间会有冲突，于是 Unicode 编码应运而生。

### 比特

我们平时会把计算机叫做“电脑”，因为其运作离不开电。上文我们说到要用数字来表示信息。对于人来说，常用的是十进制数。但是要是在计算机中原样存储十进制数，每一位需要有 10 个状态，这对于电路来说并不好实现，因为它并不是那么稳定：比如我们想规定 0 \~ 9V 之间的每一个整数电压分别表示数字 0 \~ 9，可电压并不会稳定在某一个数值附近。因此我们只能希望一个电压区间来表示一个数，比如 2.5 \~ 3.5V 之间的电压表示 3，5.5 \~ 6.5V 之间的电压表示 6 等等。但考虑到电压还是会有波动，因此这样的方式会很容易产生错误。

于是，为了减少错误发生的可能，计算机中一般采用二进制，计算机中的数也是二进制数。即使用高低电平来表示二进制数位，这使得电压只需达到固定的阈值即可，而不用关心具体的数值；比如我们可以规定大于某个数值的电压为高电平，表示 1；此外表示低电平，表示 0。

除此之外，如果要在计算机中表示 10 进制数，就需要 10 个状态，而我们需要至少 4 个二进制数位才能表示 10 个状态（根据乘法原理，n 个 2 进制数码能表示 \\(2^n\\) 种状态）。这无疑会造成存储的浪费，并且在进制转换时也需要额外的运算，因此不如直接在计算机内部使用 2 进制，只在需要输入输出才进行进制转换。

计算机中，我们把一位二进制数称为一个“比特（bit）”，把 8 个比特称为一个“**字节**（Byte）”。

> 历史上曾经有这样一段时间，对于一个字节有多少位，各种硬件并没有统一的标准。对于早期的编码系统里，人们多使用 6 位的二进制数码来表示一个字符（也就是能表示 64 种字符），而计算机多采用 6 位或 9 位作为一个字节。因此，这些系统常采用 12、18、24、30、36、48 或者 60 来作为**字**（word）的长度（即“字长”，通常为一台机器一次能处理的最长的二进制数的位数），分别对应 2、3、4、5、6、8 或 10 个“6 位长‘字节’”。
>
> 参考链接：[Byte - Wikipedia](https://en.wikipedia.org/wiki/Byte)

> 拓展阅读：[在 1024 特殊日子，怎么能忘记香农及比特 bit - IT 之家](https://www.ithome.com/0/515/575.htm)

### 数的进制

数的进制可以是任意的。对于一个 \\(n\\) 进制数，其第 \\(i\\) 位上的数为 \\(d\\)，则该位表示的数为 \\(d \times n^{i-1}\\)。其中，\\(i\\) 为从数的最低有效位（least significant digit）开始数起，并从 1 开始计数。事实上，不同于现实生活，在计算机中，很多事物都是从 0 开始编号。

由于二进制数书写起来过于长，因此我们也常用 16 进制来表示计算机中的 2 进制数。

## 数据大小的度量单位

在计算机中，对于数据长度的度量，一般以字节为最小单位，使用 1024 作为进制。

| 单位     | 英文     | 含义                                            |
| -------- | -------- | ----------------------------------------------- |
| 字节     | B (Byte) | 计算机中存储信息的计量单位，一般为 8 位二进制数 |
| 千字节   | KB       | 1 KB = 1024 B                                   |
| 兆字节   | MB       | 1 MB = 1024 KB                                  |
| 千兆字节 | GB       | 1 GB = 1024 MB                                  |
| 太字节   | TB       | 1 TB = 1024 GB                                  |

我们在形容数据存储设备、或者形容数据记录的大小时，通常采用上述单位。

如今，在计算机中存储的文本数据，通常以 KB 来计算；在计算机中存储的照片、音频等数据，一般是以 MB 作为单位进行计算的；一般拍摄的视频，在几十至数百 MB 以上；而电影、电视节目，一般以 GB 计量。

> 上述大写的 K、M、G、T，事实上是计算机领域中的度量单位，其中的以“千”为进制是以 1024 为进制，即 2 的 10 次幂（\\(1024 = 2^{10}\\)）。而我们平时常用的“千米（km）”和“千克（kg）”等单位，其中的“千”指的是 1000（10 的 3 次幂），对应为小写 k。
>
> 硬件厂商在生产存储设备时，常使用 1000 作为进制。这也就会造成，一个标称 32 GB 的 U 盘，可能实际只能存放约 29 GB 的数据。
>
> 在区分这两种进制时，有时会使用 K 和 Ki（以及 M 和 Mi 等）来作为区分。KiB（Kilo Binary Byte，kibibyte）为 \\(2^{10}\\) 个字节，KB（Kilo Byte，kilobyte）则为 \\(10^3\\) 个字节。
>
> 总的来说：
>
> - 当 K 单独出现时，可能代表 1000 或 1024
> - 当 K 与 Ki 一起出现时，K 代表 1000，Ki 代表 1024
> - 当 K 与 k 一起出现时，K 代表 1024，k 代表 1000

在形容数据流或者数据传送的能力（带宽）时，我们通常以比特为最小的单位。这里的小写“b”指的是“bit”，与字节的“Byte”作为区分。

| 单位     | 英文 | 含义                   |
| -------- | ---- | ---------------------- |
| 比特     | bit  | 1 bit，即 1 个二进制位 |
| 千比特   | Kb   | 1 Kb = 1024 bit        |
| 兆比特   | Mb   | 1 Mb = 1024 Kb         |
| 千兆比特 | Gb   | 1 Gb = 1024 Mb         |

我们在办理宽带业务时，或者形容视频的码率时，使用的一般是上述的单位。

由于 1 Byte = 8 bit，所以如“MB”和“Mb”之间单位的换算需要乘或除以 8。比如，平时所说“100 M（100 兆）”的宽带，所能提供的数据的传输速度大概是“12.5 MB/s”。

> 在用户端，宽带的带宽一般指的是下行速率，即用户将数据下载到本地的数据。为了确保用户的体验，一般宽带公司会提供略大于 100 Mb 的带宽。实际使用时，根据用户的多少，这个数据也会有所浮动。所谓的“100 Mb”，确切的说应该是“100 Mb/s”。
>
> 关于“宽带”“码率”，我们会在后续详细介绍。

## 生活中的计算机

计算机在我们的生活中无处不在。我们生活中常使用的，除了笔记本电脑、台式电脑，智能手机、智能手表，也是计算机。在我们平时看不见的地方，比如超级计算机，或者遥控器、微波炉内部的芯片，也同样是“计算机”。

像笔记本电脑、台式机这种大小不大、为个人应用而设计的，称为“微型计算机”或“个人计算机（Personal Computer，PC）”，简称“微机”。生活中经常说到的“计算机”，一般也指微机。而如同手机、手表甚至微波炉内部芯片这种，我们一般称之为“嵌入式（embedded）计算机”。嵌入式计算机通常相对简单、体积小，通常被用来控制其它设备——如无人机、机器人、相机等。也有非常庞大的计算机，一般用于特别的科学计算，或者大型的服务需求；这些大型计算机也被称作“超级计算机”。

关于电子计算机，[维基百科](https://zh.wikipedia.org/wiki/电子计算机) 上这样给出定义：

> 计算机（亦称电脑）是指能够通过编程，根据一系列指令执行任意算术或逻辑操作的机器。
> 
> 尽管计算机种类繁多，但**根据图灵机理论，一部具有着基本功能的计算机，应当能够完成任何其它计算机能做的事情**。因此，理论上从智能手机到超级计算机都应该可以完成同样的作业（**不考虑时间和存储因素**）。

### To Be Continued...