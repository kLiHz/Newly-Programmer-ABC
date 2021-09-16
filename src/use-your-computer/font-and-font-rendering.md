# 字体与字体渲染

之前说到，要在计算机中存储字符，就需要用一种编码方式，将字符和一定的数码对应出来。这篇内容将介绍一些具体的编码，以及字符如何显示出来，为用户所感知。

## ASCII

ASCII 是“美国信息交换标准代码”对应英文的缩写，该编码是电子通讯的字符编码标准。很多现代的编码方式都是基于 ASCII 开发，在其之上支持了许多额外的字符。

ASCII 是在电报码的基础之上开发的，至于它的历史这里不多展开。简单来说，ASCII 涵盖了英文字母表中的所有字母、阿拉伯数字、一些标点符号，以及一些控制字符。控制字符不能被打印出来，仅表“控制”含义，如今常用的有“回车（Carriage Return）”“换行（Line Feed）”“水平制表符（Tab）”。

> 曾经，在英文打字机上，打字机印字的位置是固定的，用户在输入完一行后，需要将纸筒推回到开始的位置（后来印子头可移动的打字机则是移动印子头到初始位置），同时还要将滚筒上卷一行，以便开始新的一行的输入。“Carriage Return”就是指的这个“归位”的操作，不包含换行。
>
> 此外，由于有时需要输入规整排列的文本，也就是以表格形式对齐排布（tabulated）的文本，用户一开始不得不手动按空格键来补齐空格，如果输入错误了还需要按退格（Backspace）键来修正，十分浪费时间。因此，打字机设计出了“Tab”键，按键即可使打字位置移动到下一个表格位点处，如此，定位过程变得方便了许多。

可以使用转义字符的方式来表示这些无法打印出（non-printable）的控制字符。通常，我们使用以反斜线（backslash）开头的一串字符，来表示接下来的字符应当以不同于其字面意义的方式被解读。比如，我们常使用 `\n` 来表示“Line Feed”，使用 `\r` 表示“Carriage Return”，使用 `\t` 表示水平制表符。

为了标记一行的结束（End of Line），Windows 系统常使用连续的两个字符 `\r\n`，而 Unix 系统常常只使用一个字符 `\n`。相应的，这两种换行方式有时也被称作 CRLF 和 LF。

不过，现代计算机，用户一般只需按下键盘上的“Enter”键，即可在文本编辑器中完成“换行”的操作。

按下“Tab”键，即键入一个制表符，会使得文本编辑器或者字处理软件中的输入光标移动到下一个邻近的位点上。而在图形界面中，按下“Tab”有时也可以聚焦在界面中邻近的下一个元素上，比如文本框或者按钮，而不必移动鼠标。

> 在文本命令行界面，一般存在一种叫做“Tab 补全（tab completion）”的功能，用户只需输入命令或路径的前几个字符，随后按下 Tab 键，程序接收到相应的信号后，会根据已经接收到的按键输入进行推测，如果无歧义（即能够唯一确定这个命令），便会将命令直接补全。有些程序也会在有歧义时，将所有的可能列举出来供用户选择。

在计算机软件开发领域，为了使数据、代码等内容便于阅读，会使用“缩进”（即行首的空格）来体现内容的层次。同一层级的代码会使用相同的缩进。在编辑器中，用户可以按下 Tab 键，即可键入一个“垂直制表符（Tab）”，也就是“缩进”。该缩进可能会根据不同的配置而被渲染 2、4 或 8 个空格。有的编程者或者编程语言会倾向于使用一定数量的空格而不是“制表符”来形成缩进，因此，经过一定的设置，一些编辑器可以在用户按下 Tab 键时，插入等量的空格，而不是制表符。

> 如果要保证缩进层级在显示时的统一，则不应混合使用 Tab 和空格。
>
> 一般情况下，用户可以使光标移过空白，通过观察光标跳跃的长度，来判断对应处的字符是空格还是制表符。

参考资料：

- [ASCII - Wikipedia](https://en.wikipedia.org/wiki/ASCII)
- [Carriage return - Wikipedia](https://en.wikipedia.org/wiki/Carriage_return)
- [Tab key - Wikipedia](https://en.wikipedia.org/wiki/Tab_key)
- [What is the difference between a "line feed" and a "carriage return"? - Stack Overflow](https://stackoverflow.com/questions/12747722/what-is-the-difference-between-a-line-feed-and-a-carriage-return)

## Unicode

[Unicode](https://en.wikipedia.org/wiki/Unicode) 是一个信息技术领域的标准，其意在使得世界上绝大多数文字书写系统中文本的编码、表示以及处理达到统一。

> 所谓书写系统（[Writing System](https://en.wikipedia.org/wiki/Writing_system)），是一种基于图形符号以及一定的柜子，以视觉可见的形式来表达言语交流的方式。用更简单的语言来讲，就是我们平时所说的文字。

Unicode 可以由不同字符编码方式来实现。Unicode 标准中规定有若干“Unicode 转换格式（Unicode Transformation Format）”：如 UTF-8、UTF-16 以及 UTF-32 等。常用的编码方式有 UTF-8，UTF-16 以及已经废弃的 UCS-2。

> 此外，[GB 18030](https://en.wikipedia.org/wiki/GB_18030)，全称《信息技术 中文编码字符集》，虽然不是 Unicode 官方的标准，但是其完全实现了 Unicode，并且是中华人民共和国的国家标准。GB 是“国家标准”的拼音缩写。
>
> 在 GB 18030 之前，也有较旧的 [GB/T 2312-1980](https://en.wikipedia.org/wiki/GB_2312) 标准以及对其的扩展 [GBK](https://en.wikipedia.org/wiki/GBK_(character_encoding))。
>
> 1993 年，Unicode 1.1 标准发布，包含了中国大陆、台湾、日本以及韩国使用的共计 20,902 个字符。随后，中国发布了 GB13000.1-93 标准，即和 Unicode 1.1 等价的国家标准。
>
> 作为 GB2312-80 的扩充，GBK 字符集于 1993 年定义，同时利用 GB2312 中未使用的位点支持 GB13000.1-93 中的字符。因此，GBK 对 GB2312 具有向前兼容性。Windows 在“代码页 936”中实现了对 GBK 编码的支持。虽然 GBK 并不是官方标准，但 Windows 95 的广泛使用使得 GBK 成为了“事实标准”。
>
> 虽然 GBK 包含了 Unicode 1.1 以及 GB13000.1-93 中的字符，但是这些标准使用的是不同的代码表。GBK 的存在，仅仅是为了从 GB2312-80 过渡到 GB13000.1-93。
>
> 1995 年，全国信息技术标准化技术委员会确立了“汉字内码扩展规范（GBK）”的 1.0 版本，即 GBK 1.0，其在“代码页 936”上做了些微的改动。
>
> 2000 年，GB 18030-2000 正式发布，在取代 GBK 的同时，也保留了对 GBK 1.0 的兼容性。有时，GBK 也指 GB 18030 中使用 1 字节或 2 字节编码的字符。
>
> 最新的是 2005 年发布的 GB10830-2005 标准。自 2006 年 5 月 1 日起，在中华人民共和国境内销售的软件产品，都需要符合 GB10830 规范。
>
> - [GBK (character encoding) - Wikipedia](https://en.wikipedia.org/wiki/GBK_(character_encoding))
> - [GB 18030 - Wikipedia](https://en.wikipedia.org/wiki/GB_18030)

## 字体渲染

（未完待续）
