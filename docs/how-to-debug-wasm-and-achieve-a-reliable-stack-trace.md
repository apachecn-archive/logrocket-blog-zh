# 如何调试 Wasm 并实现可靠的堆栈跟踪

> 原文：<https://blog.logrocket.com/how-to-debug-wasm-and-achieve-a-reliable-stack-trace/>

WebAssembly(或 Wasm)允许开发人员在互联网浏览器中以接近本地速度使用从 Rust、C 或 C+等语言收集的代码。

Wasm [也可以成功地用作需要快速沙箱的平台的轻量级 docker 替代品。诸如标准 WASI 之类的平台无关的接口可以实现文件系统访问、诸如标准输入和输出之类的基本功能以及其他类似功能。](https://blog.logrocket.com/webassembly-how-and-why-559b7f96cd71/)

尽管有很多好处，但调试 Wasm 可能很困难，因为真实的 bug 变得越来越复杂，难以重现。源映射可以用来找到可靠的堆栈跟踪，查看它们真正的样子，并在 Wasm 中获得可靠的文件名。

在本文中，我们将讨论 Wasm，为什么它很难调试，以及在调试 Wasm 时可以使用的一些解决方法，包括源代码映射和堆栈展开。

## 什么是 WebAssembly？

Wasm 是现代 web 浏览器的代码，它依赖于具有非常紧凑的二进制格式的语言，并为其他编码语言提供编译目标，以便它们可以在 web 上工作。它与 JavaScript 协同工作，共享许多功能。

这样做的结果是，即使是不知道如何编写 Wasm 代码的开发人员也仍然可以使用它。考虑到 75%的 web 开发人员报告使用 JavaScript 来满足他们的大部分编码需求，WASM 的兼容性尤其具有吸引力。

不是所有的函数都可以在同一个内存空间中访问，因此在程序中使用堆栈。DWARF 对于调试其他程序是有用的，但是在任何当前的执行引擎中，Wasm 的[功能也不完全。一年多前，DWARF 中的变化被实现，以允许对 WebAssembly 的理解。这伴随着对 Wasm 的 LLVM 后端的重大修改。](https://thenewstack.io/the-pain-of-debugging-webassembly/)

最终，掌握不同类型的代码及其调试方法对于开发人员来说非常重要，尤其是那些在云计算和软件即服务(SaaS)行业工作的开发人员。这是因为 SaaS [允许从任何装有浏览器的设备上访问数据](https://bluetree.ai/saas-business-models-explained/),并依赖于浏览器服务器上的应用程序代码。在不同浏览器中兼容且无错误的代码最终会增强客户体验并增加客户保留率。

## 堆栈解退

首先，您希望获得堆栈跟踪。我们如何做到这一点？它从展开堆栈开始。WebAssembly 的展开方案需要被激活，这是[通常用 libunwind 等库](https://blog.logrocket.com/rust-compression-libraries/)来完成的。对于 Wasm 的退栈，主要关注的是返回地址。除此之外的任何事情都是不必要的。

您可以通过捕获寄存器来展开堆栈，从而在程序运行时展开。当 Rust 错误警告或 [C++异常出现](https://www.tutorialspoint.com/cplusplus/cpp_exceptions_handling.htm)时，最好使用这种方法。栈卷绕可以在面临异常时执行析构函数，

展开堆栈的另一种方法是使用内存转储。带有寄存器的整个堆栈内存被转储到内存转储中，然后展开。很明显，WebAssembly 在促进栈展开方面不是最好的，但是如果你的浏览器和大多数浏览器一样使用 JavaScript，这并不是一个大问题。

因为 Wasm 本质上是一个堆栈机器，所以可以在 JavaScript 自己的堆栈跟踪中查看函数调用。通过在 JavaScript 中创建一个异常对象，您可以通过这个方法分析它的堆栈跟踪。

## DWARF 调试标准和 Wasm

[DWARF 调试标准](http://dwarfstd.org)长期用于二进制文件的分步调试。DWARF 经常用于 LLVM 和其他编译器后端，尽管它不是为此而设计的。尽管 DWARF 与 Wasm 在任何当前的执行引擎中都不兼容，但 Chrome 和 Firefox 仍然可以利用调试信息。他们通过将源地图链接到执行 Wasm 来实现这一点。

![DWARF Linking to WASM With Source Map](img/86807bc13f8092dcee0edc2fffecd714.png)

[Source](https://i.stack.imgur.com/XkamG.png)

这对于确保安全性很重要，因为许多人会对在 Chrome 或 Firefox 等浏览器上使用 JavaScript 持谨慎态度。布里斯班的软件开发人员 Will Ellis 来自澳大利亚隐私网站，他说，当运行 Chrome 等浏览器时，“有些人选择完全关闭 JavaScript，只允许它在他们真正信任的网站上运行。”幸运的是，源映射可以定义原始输入文件和结果 JavaScript 指令之间的映射格式，有点像翻译器。这样，可以针对原始输入文件的视图执行浏览器调试。

DWARF 调试标准将 DWARF 数据中的部分嵌入到可执行文件中。WebAssembly 是一种可扩展的对象格式，这使得这种嵌入成为可能。Wasm 由两种主要格式组成:WAST(一种基于文本的格式)和二进制格式。

因此，一些 Wasm 工具不能识别字节偏移量。不幸的是，当使用调试信息时，这两个版本不兼容。

在 Wasm 文件中嵌入调试信息有一些严重的问题。这使得任何人都有可能从他们的元数据或构建机器中反编译代码并查看文件描述。另一个问题是 Wasm 的 DWARF 数据只能存储在一个非常大的文件中，比主 Wasm 文件大得多。

DWARF splitting 是这里的答案，其中 DWARF 数据与主可执行文件分离。一旦被分割，调试数据可以被存储在不可执行和非功能性的文件中，仅用于调试。

一旦它们被分开，你如何把它们连接起来？在主 Wasm 文件中为特殊部分嵌入一个可以下载用于调试的文件的引用，就像使用一个源映射一样。

将调试数据与正确的 Wasm 文件连接起来[非常重要。为此，调试 ID 是必需的。Wasm 工具链可以获取这些文件，并将它们放在符号服务器上，用于调试数据和二进制文件。](https://rustwasm.github.io/book/reference/debugging.html)

在 Wasm 中，对源地图的挑战也很普遍。这是因为在调试模式下很难确定信息范围、访问或映射函数名以及查找变量，并且只能用于基于文本的 Wasm 版本。

请记住，在调试时考虑 WebAssembly 与 JavaScript 和其他 Wasm 模块的交互方式非常重要。Wasm 中的堆栈跟踪带有一个编码位置信息的文件名，但是仍然很难找到函数索引，因为两个不同的模块可以有相同的函数索引。Wasm 模块保存在隔离的容器中，但是仍然可以导出和导入函数。

## 结论

WebAssembly 对于开发者来说是一个很棒的工具，尽管它在调试方面存在挑战。尽管要绕过基于堆栈的设计还需要一些步骤和一点创造力，但仍然可以使用 DWARF 成功调试 Wasm。

## 使用 [LogRocket](https://lp.logrocket.com/blg/signup) 消除传统错误报告的干扰

[![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/signup)

[LogRocket](https://lp.logrocket.com/blg/signup) 是一个数字体验分析解决方案，它可以保护您免受数百个假阳性错误警报的影响，只针对几个真正重要的项目。LogRocket 会告诉您应用程序中实际影响用户的最具影响力的 bug 和 UX 问题。

然后，使用具有深层技术遥测的会话重放来确切地查看用户看到了什么以及是什么导致了问题，就像你在他们身后看一样。

LogRocket 自动聚合客户端错误、JS 异常、前端性能指标和用户交互。然后 LogRocket 使用机器学习来告诉你哪些问题正在影响大多数用户，并提供你需要修复它的上下文。

关注重要的 bug—[今天就试试 LogRocket】。](https://lp.logrocket.com/blg/signup-issue-free)