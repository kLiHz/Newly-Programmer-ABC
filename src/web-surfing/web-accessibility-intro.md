# Web 无障碍

通过视觉获取信息，是我们习以为常的一件事。但是，“视觉”这一获取信息的渠道并不在所有情况下可用。除此之外，“听觉”“言语”等亦是如此。

简单地说，无障碍设计，就是需要考虑到不同场景下的“可访问性（accessibility）”。

因此，对于界面中的元素（控件以及内容），我们需要提供不同场景下的替代物（alternatives）。

良好的无障碍设计可以惠及任何人。比如，嘈杂的环境下，设备所播放的视频中的声音变得不易分辨；低速互联网访问下，图像往往不具备可用性……等等。

这篇内容主要介绍 Web 可访问性。

> *The power of the Web is in its universality.*
>
> *Access by everyone regardless of disability is an essential aspect.*
>
> — Tim Berners-Lee, W3C Director and inventor of the World Wide Web

Web 无障碍简介：[Introduction to Web Accessibility - Web Accessibility Initiative (WAI) - W3C](https://www.w3.org/WAI/fundamentals/accessibility-intro/) （[简体中文页面](https://www.w3.org/WAI/fundamentals/accessibility-intro/zh-hans)）

无障碍原则：[Accessibility Principles - Web Accessibility Initiative (WAI) - W3C][accessibility-principles]（[简体中文页面][accessibility-principles-zh-hans]）

要实现 Web 无障碍，需要几个部分相互配合：

- 网络上的内容（Web content）：比如文本、图像、表格、多媒体等
- 用户用于访问互联网的工具（User Agents）：比如桌面/移动设备上的浏览器、插件、辅助访问工具等
- 编写工具（Authoring tools）：用于制作产出网络内容的工具，如代码编辑器、文档转换器等。

比如，需要用于替代非文本内容的文本，如：

- 对图像、图标、按钮、图形界面的简短的**等价描述**；
- 对图表、示意图、插图上所展示数据的**说明**；
- 对音频、视频等非文本内容的**简介**；
- 对表单、输入框以及其他用户界面组件的**标签**。

针对多媒体，应该提供说明文字及其他替代物。比如，对于音频内容（如电台采访的录音），应当提供文本抄录稿；对于视频，其中重要的影像细节须有文字描述或口述影像。

具体来说，一场电影，除了演员台词，其中关键的声响、通过画面表述的剧情，也应有相应的等价表述（文本、手语等）。

至于文本内容，为了其能够以不同的方式正确的呈现，下面这些是必要的：

- 对内容的结构进行正确地标记，如标题、列表、表格等；
- 连串的信息或者指令步骤，不受具体的呈现方式的影响；
- 浏览器和辅助技术在访问这些内容，支持自定义的呈现方式。

满足了以上要求，内容便可以被正确的读出或重构，无论使用者的喜好为何：字体大小、颜色、内容排版样式等等。同时，正确的结构也能方便的生成大纲，方便用户快速获取信息，以及跳转到特定部分。

上面这些也就意味着，不应该只使用颜色或者特定的样式来传递信息。举个例子，某用户可能认为“将文本设置为红色、大字号”即可达到强调效果，但经过文本重排后，这些文本会变得和普通文本一样。

如果一定需要使用视觉来传递信息，除了设置相应的替换文本外，还应注意使用合适的对比度，以便内容可以被清晰阅读。如果需要使用背景音频，也需要保证音频的音量调整、暂停或停止的选项。

另请参阅：

- [Accessibility Principles - WAI - W3C][accessibility-principles]（[简体中文页面][accessibility-principles-zh-hans]）
- [WAI Web Accessibility Tutorials - w3.org](https://www.w3.org/WAI/tutorials/)
- [让您的网站成为无障碍网站 - Google 协作平台帮助](https://support.google.com/sites/answer/7529116?hl=zh-Hans)

[accessibility-principles]: https://www.w3.org/WAI/fundamentals/accessibility-principles/
[accessibility-principles-zh-hans]: https://www.w3.org/WAI/fundamentals/accessibility-principles/zh-hans

