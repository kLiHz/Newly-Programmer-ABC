# 静态网页生成

静态网页（Static web pages）的内容，往往不会根据访问时间、地点以及访问者的不同而发生变化。根据维基百科的定义，静态网页将以在存储器中存储的形式，原样提供给用户。

> 与之相对的，动态网页的内容往往由 Web 应用生成。
>
> 通过 JavaScript 和“文档对象模型（Document Object Model）”，网页也能实现动态的内容呈现。这些计算是在客户端浏览器进行的，不涉及到服务器端代码的执行，因此这些网页也能够由静态网页托管服务商托管。

通常，静态网页就是存储在计算机（的文件系统）中的 HTML 文档，通过使用支持 HTTP 协议的 Web 服务器，就能为同一网络下的用户所访问。

> 以更宽泛的概念来说，只要内容是固定的，静态网页也可以包括由 Web 服务器根据一定的模板和存储在数据库中的信息生成的网页。

静态网页很适合用于发布不需要经常更新的内容。当网页的内容和数量变多时，往往需要使用一些自动化工具来进行管理。

对于一般用户来说，有很多工具可以用来生成静态网页。这些工具通常能够根据（用户指定的）一定的主题或模板，将用户使用 Markdown 语法（或者一些 Markdown 的扩展语法）编写的内容，生成为静态网页。

参考资料：

- [Static web page - Wikipedia](https://en.wikipedia.org/wiki/Static_web_page)
- [Dynamic web page - Wikipedia](https://en.wikipedia.org/wiki/Dynamic_web_page)

## mdBook

下面，将以 [mdBook][mdbook] 为例介绍静态网页生成工具的使用。

正如其名“mdBook”，其从 Markdown 文件生成静态页面，并像书籍一样以章节方式组织内容。对于流水式的内容，如日志、随想等，也可以使用时间标签作为章节组织内容。

一般来说，要使用 mdBook，只需要下载其可执行文件。在 [mdBook 的 GitHub Release 页][mdbook-release] 提供有针对 x86\_64 架构的 Windows、macOS 以及 Linux 系统的的预构建版本。读者也可以访问 [mdBook 的 GitHub 主页][mdbook] 获得更多信息（使用方式、教程）。

对于一般 Windows 用户，可以下载后缀为 `x86_64-pc-windows-msvc.zip` 的压缩包。

下载完成后，可以将压缩包中的可执行程序放在某目录下的 `mdbook` 目录（如不存在，须事先创建），比如 `C:/Users/%UserName%/softwares/mdbook/`、`D:/Program Files/mdbook/` 或者 `D:/softwares/mdbook/`。

之后，选择或新建一个工作目录，就可以开始编写 mdBook 了。比如，选择 `C:/Users/%UserName%/Documents/MyBook/`。

## mdBook 使用介绍

关于 mdBook 的使用，可以参考 [mdBook 提供的教程兼说明文档](https://rust-lang.github.io/mdBook/)。下面也会提供一些简单的使用介绍。

由于 mdBook 需要使用命令行界面使用，因此可以选择集成终端的编辑器，如 [Visual Studio Code][vscode]。也可以选择其他自己喜欢的文本编辑器。

[mdbook]: https://github.com/rust-lang/mdBook
[mdbook-release]: https://github.com/rust-lang/mdBook/releases
[vscode]: https://code.visualstudio.com

### 初始化

首先，可以打开终端，切换到工作目录，执行如下命令来让 mdBook 进行一些初始化操作：

```
"D:/softwares/mdbook/mdbook" init
```

> 如果使用 Visual Studio Code，在打开某一工作目录后，内置终端默认的当前路径就是工作目录。可在顶部“Terminal（终端）”选项新建一个终端会话，或按下快捷键 <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>`</kbd>（Windows）。

如果已经将 mdBook 可执行文件所在的路径添加到环境变量 `PATH`，即可直接使用 `mdbook` 命令：

```
mdbook init
```

之后，mdBook 会询问是否创建 `.gitignore` 文件，以及书籍的标题。创建的 `.gitignore` 文件中包含默认的输出目录 `book`，这样，我们的构建产物（静态网页）就不会被 Git 追踪。书籍的标题则会体现在生成的 `book.toml` 文件中。

> 可以在命令行选项中提供这些信息，这样 `mdbook` 便不会再询问用户。
> 
> ```
> mdbook init --ignore --title="My Book"
> ```

### book.toml

TOML 是一种配置文件的格式。下面将简单介绍一下 `book.toml` 的内容：

初始化后得到的 `book.toml` 类似如下：

```toml
[book]
authors = ["henry"]
language = "en"
multilingual = false
src = "src"
title = "My Book"
```

`[book]` 表明下面的属性（配置）是针对于这本书的。其中，`language` 属性标记了本书的语言，可以使用 `zh-Hans` 表示简体中文；`src` 属性指定了书籍源文件目录位于何处。

如果书中需要使用数学公式，可以在 `book.toml` 中添加以下内容，为生成的 HTML 内容添加 MathJax 支持。

修改后的 `book.toml` 内容如下：

```toml
[book]
authors = ["henry"]
title = "mdBook 模板"
language = "zh-Hans"    # "zh-Hant": Traditional Chinese
multilingual = false
src = "src"

[output.html]
mathjax-support = true  # MathJax 支持
```

> 不同于一些 Markdown 扩展语法，mdBook 分别采用 `\( \)` 和 `\[ \]` 来标注行内和行间公式。不过，由于 MathJax 支持工作在最后的 HTML 成品阶段，且 mdBook 默认的预处理器不会对标注公式的语法进行识别，因此用户如需在 Markdown 中使用公式，则标记公式的符号以及公式中出现的符号均需被转义。例如，用户需要在文档中书写 `\\(\\sin{x\_1}\\)`，才能在最终 HTML 输出中得到 `\(\sin{x_1}\)`，进而被 MathJax 正确识别。

### SUMMARY.md

`src` 目录下的 `SUMMARY.md` 文件标记了整个书籍的框架。也就是说，书中出现的所有页面（章节），都应该在 `SUMMARY.md` 中标记出来。

`SUMMARY.md` 文件对格式要求严格，但是也易于掌握。

简单来说，其使用 Markdwon 中声明超链接的语法，以标记章节的名称，以及对应文件的路径（相对路径）。若链接留空，则为声明草稿章节。

```markdown
[Some Chapter](relative/path/to/file.md)

[Draft Chapter]()
```

使用类似如下的语法，标记章节的层级，以及编号章节与无编号的章节（前言、后言等）。

```markdown
[Preface](relative/path/to/file1.md)

- [First Chapter]()
- [Second Chapter](relative/path/to/file2.md)
    - [Sub Chapter 1](relative/path/to/file3.md)

[Acknowledge](relative/path/to/file4.md)
```

可以在章节中添加分割线和分区标题。

```markdown
# Part Title

- [First Chapter]()
- [Second Chapter](relative/path/to/file2.md)
    - [Sub Chapter 1](relative/path/to/file3.md)

# Title Of Another Part

- [Another Chapter]()

---

- [Also Another Chapter]()

```

### 内容组织

一般情况下，章节及其下的子章节属于同一个类别，因此，在组织这些章节的源文件时，可以将它们放在一个目录下。

> 章节首页（或者索引页）的可以使用 `README.md` 作为文件名，该文件将会被渲染为对应目录下的 `index.html` 文件，这样，在部署好页面后，在浏览器中访问 `https://<domain>/some-chapter/` 即可查看对应的 README 页面。

例如，读者可以参考类似如下的 `SUMMARY.md` 的结构，组织 mdBook 中的页面层级。

```markdown
- [Mammals](mammals/README.md)
    - [Monkeys](mammals/monkeys.md)
    - [Tigers](mammals/tigers.md)
```

## 参考链接

- [SUMMARY.md - mdBook Documentation](https://rust-lang.github.io/mdBook/format/summary.html)


