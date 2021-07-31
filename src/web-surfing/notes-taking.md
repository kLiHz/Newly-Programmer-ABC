# 做笔记

关于记录自己的学习历程、思路或者思考，有很多种形式，比如笔记、文档、日记、博客……

写博客/笔记的必要性：可以帮助自己复习，和别人交流也能发现自己的问题，或者获得不同于自己的思路。

参考链接：[你應該要嘗試的 WDL（Writing-Drive Learn，寫作驅動學習）by 神Q超人 - Medium](https://medium.com/starbugs/你應該要嘗試的-wdl-writing-drive-learn-寫作驅動學習-3f157c0ab30c)

下面推荐一些可能会用到的工具或平台。

## MS Office

Microsoft 的 Word、PowerPoint 是 Microsoft Office 办公套件中比较常见的办公软件了。

Word 可以用来编写富文本和一些较复杂的排版。对于简单的文档，使用 Markdown 更轻便，也更容易保持简单。

PowerPoint 除了可以制作演示文稿，还可以用来**画图**，且操作较为简单；用户可以将绘制的图片导出为矢量图格式（SVG），空间占用小，而且更清晰。此外，PowerPoint 也能用来做一些简单的平面设计或者动画。总的来说功能还是很强大。

## 笔记软件

从某种程度上来说，Markdown 流行于一部分用户之间。而对于大多数用户来说，日常接触到的多为富文本编辑器。比如 Microsoft OneNote、[Evernote](https://evernote.com)，以及各种各样的在线平台，如 [语雀](https://www.yuque.com/)。这些平台往往还带有云备份功能。但是用户对于内容的掌握不如使用 Markdown 等标记语言，比如，用户添加到文档的图片可能会直接进入程序的数据库，或者上传到云端，而不是以可见可感的文件形式存储在用户本地的计算机。

## 标记语言

传统的富文本编辑器一般是基于 HTML 对用户的内容进行处理和保存。不过，HTML 代码并不适合直接阅读，而 Markdown 设计之初即考虑到易于阅读这一点。此外，由于 Markdown 语法简单，用处非常广泛。

另一方面，由于一般富文本编辑器支持的格式较多，比如字号、颜色，若不用心处理可能会变得难以阅读。而 Markdown 只提供最基础的标记，虽然简单，但容易获得整洁的格式。

此外，除了使用绘图软件，也可以用“标记语言”来画图，类图、流程图等。不过，使用代码画图在对于图案布局的掌控性和代码的易读性之间难以取得平衡。

总的来说，使用标记语言便于修改和存储，也适于进行版本控制（后续会进行介绍）。

编辑器推荐：[几款主流好用的 Markdown 编辑器介绍 黄志千的博客 - CSDN](https://blog.csdn.net/davidhzq/article/details/100815332)

之前介绍到，Markdown 轻便、简单，使用非常广泛，但是它也有问题，最重要的一点就是没有一个统一的标准，经常会遇到在一个地方支持，而在另一个地方就不支持（不同编辑器可能会为 Markdown 增添不同的扩展语法）。

因此，如果要涉及到和别人协作和共享，就不得不约定一个一致的标准，或者使用统一的工具。如果想要将内容分享给别人或者更换平台的情况，也要考虑文档的可导出性和可移植性（portability）。一个方法是限制自己仅使用基础的语法。

另一方面，使用 Markdown 等标记语言往往需要用户自行对内容进行备份。一般，用户需要使用“同步盘”或者使用代码托管平台对自己的内容进行管理以防丢失。

为什么不应该用 Markdown 写文档：[Why You Shouldn't Use "Markdown" for Documentation — Eric Holscher - ericholscher.com](https://www.ericholscher.com/blog/2016/mar/15/dont-use-markdown-for-technical-docs/)

### reStructuredText

reStructuredText，简称 reST。Python 和 Linux 内核的文档均由 reST 撰写；reST 语法支持很全，也很容易拓展，还有原生的 LaTeX 数学公式支持。

除了官方标准（即 Python 的 Docutils 的实现）外，还有一个名为 sphinx 的广泛认可和使用的与官方兼容的拓展实现；Linux 内核文档就是用的 sphinx。可以说，sphinx 可能才是**事实标准**。不过 sphinx 只是原版的一个超集，不必担心有冲突的语法。

初次使用 reStructuredText 可能比较困难，其实根据需要一步步掌握也是可以的。

> 事实标准（*de facto*）[De facto - 维基百科](https://zh.wikipedia.org/zh/De_facto)
>
> 业界标准也称为实质标准或非官方标准，是一种已经获得大众接受，或是已有市场主导地位的习惯或是风俗。
>
> 事实标准是指非由标准化组织制定的，而是由处于技术领先地位的企业、企业集团制定（有的还需行业联盟组织认可，如 DVD 标准需经 DVD 论坛认可），由市场实际接纳的技术标准。

### AsciiDoc

（待补充）

> [AsciiDoc Language Documentation :: Asciidoctor Docs](https://docs.asciidoctor.org/asciidoc/latest/)


### LaTeX

- [LaTeX 入门 - OI Wiki](https://oi-wiki.org/tools/latex/)

- [如何从零开始，入门 LaTeX？ - 知乎](https://www.zhihu.com/question/62943097/answer/203670095)

- [一份其实很短的 LaTeX 入门文档 - 始终 - liam.page](https://liam.page/2014/09/08/latex-introduction/)

- [回「LaTeX 的罪与罚」 - stone-zeng.github.io](https://stone-zeng.github.io/2019-07-23-latex-crime-and-punishment)
