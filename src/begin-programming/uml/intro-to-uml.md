# UML：统一建模语言

UML（Unified Modeling Language，统一**建模语言**），是一种通用的、与开发相关的建模语言，用于软件开发领域，意在提供一种使系统设计可视化的标准方式。

> **建模语言**（[Modeling language - Wikipedia](https://en.wikipedia.org/wiki/Modeling_language)）：任何一种**通过**一套相互协调的规则而定义出来的一种**结构**，来**表达信息**、知识或者系统的人造语言。这些规则用来解释结构中组成元素的含义。

关于UML的更多介绍，请阅读维基百科：[Unified Modeling Language - Wikipedia](https://en.wikipedia.org/wiki/Unified_Modeling_Language)

[UML - Class Diagram - Tutorialspoint](https://www.tutorialspoint.com/uml/uml_class_diagram.htm)

简单地说，就是用来描述一个系统的设计的语言，这个语言规定的有一些约定好的“语法”和符号，要用这些规定好的方式来表述信息。

就好比，要讲述我们对于一个系统（或者流程）的设计，我们可以用自己方式的涂鸦来呈现给别人，但是别人有可能看不懂，而使用UML规范画图，就不容易产生误解，这两者就像是方言和普通话的差别。

UML中有很多种图表，简单的来说可以分成两类，一些用来表示表示如结构组成之类的信息（*structural* information）；另一类表示广义上的行为（general types of *behavior*），其中又包括几种表示不同层面下交互（*interaction*）的图标。


| 类型                         | 介绍                                                         |
| :--------------------------- | ------------------------------------------------------------ |
| 结构图 (Structure Diagram)   | 组织结构图表示一个系统的静态层面，它强调的是一个系统在建模时所必须要**展现**出的事物。因此，其被广泛用来说明软件系统的软件架构。 |
| 行为图 (Behavior Diagram)    | 行为图表现系统的动态层面。它强调的是在建模时系统必须要**发生**的事情。因此，一般用来说明软件系统的功能。 |
| 交互图 (Interaction Diagram) | 交互图作为行为图的一个子集，交互图强调的是在建模过程中系统的控制流程，以及各组件之间的信息交互。 |

## 如何画图？

有很多软件可以使用。

### 直观绘图法

有一款免费的开源软件 [Diagram Software and Flowchart Maker (diagrams.net)](https://www.diagrams.net/)，在线就可以使用，也可以下载客户端，而且还有针对 VS Code 等 IDE 的集成等等。

GitHub仓库地址：[jgraph/drawio: Source to app.diagrams.net (github.com)](https://github.com/jgraph/drawio)

### 代码绘图法

相关链接：[Text to UML and other "diagrams as code" tools - Fastest way to create your models (modeling-languages.com)](https://modeling-languages.com/text-uml-tools-complete-list/)

#### yUML

简单的小图可以用 yUML，十分推荐。复杂的图还是建议用上边的工具来画。

有在线网站可以解析渲染 yUML，也有 VS Code 集成。VS Code 插件在商店搜寻“yUML”即可安装。

在线网站：[Create UML diagrams online in seconds, no special tools needed. (**yuml.me**)](https://yuml.me/)

关于 yUML 的**语法**，VS Code 插件的作者有一个整理：[Home · jaime-olivares/yuml-diagram Wiki (github.com)](https://github.com/jaime-olivares/yuml-diagram/wiki)

yUML 不仅可以画类图，其他的活动图、行为图等等也是可以的。

要使用集成在 VS Code 中的插件，还请参阅作者整理的 Wiki。

#### PlantUML

PlantUML 稍微复杂些。

教程链接：[Drawing a UML Diagram in the VS Code &#124; by Joe T. Santhanavanich &#124; Towards Data Science](https://towardsdatascience.com/drawing-a-uml-diagram-in-the-vs-code-53c2e67deffe)

教程中提到的 PlantUML 网站：[Open-source tool that uses simple textual descriptions to draw beautiful UML diagrams. (plantuml.com)](https://plantuml.com/en/)。这个网站也有 [中文版](https://plantuml.com/zh/)，也可以点击页面上的语言按钮切换。

