# 实现不伤害 UX 的模态窗口

> 原文：<https://blog.logrocket.com/ux-design/implementing-modal-windows-that-dont-hurt-ux/>

使用任何现代数码产品，都不可能不碰到一款 modal。如果我们试图在台式机或笔记本电脑上保存一个文件，我们很可能会遇到一个要求我们定义文件名和位置的模式。在某些情况下，当移动应用程序请求音频或视频许可时，系统会提示我们通过一个模型来同意或拒绝这个访问请求。

由于模态几乎无处不在，您可能会在某个时候被赋予实现它们的任务。让我们探索一下为什么模态如此普遍，它们是否是实现你目标的最佳选择，这样你就能更好地创建不会损害用户体验的模态。

## 什么是模态？

让我们首先尽可能简单地定义一个模态:模态是位于主应用程序之上的一个窗口。UX 设计者使用模态来显示额外的可操作内容，以帮助用户完成他们想要的任务。

在台式机或笔记本电脑上，点击社交媒体帖子上的反应数量，会打开一个用户列表，这些用户以一种模式对该帖子做出了反应。然后，用户可以使用该信息来执行初始内容范围之外的附加任务。例如，用户可以发现要关注的新用户或对话。

模态更常见的用途是各种桌面应用程序的设置页面。如果我们试图打印一个文件，我们必然会面对一个模态，因为简单地点击 **Print** 并不总是足以让打印发生的指令。

打印时需要定义许多变量:纸张大小、颜色、方向、份数等等。为了加快速度，大多数打印设置模式都有一个默认的设置，通常，我们只需点击打印就可以了。

## 情态动词的类型

[尼尔森诺曼集团](https://www.nngroup.com/articles/popups/)将不同类型的模态汇总成可分类的组，如下所示:

![Modal Groups](img/be2e9c38b702c425bcf655cd001957d6.png)

尽管 Nielsen Norman Group 帮助我们理解了不同类型的模态，但我们还需要探索更多的用例——具体实现以及它们之间的差异。

### 对话模式

对话框是一种系统消息，对话框是一种向用户传达这种消息的模式，例如警告或确认。在某些情况下，这也可以是一个全屏信息，以抓住用户的注意力。此外，对话框可以分为两种类型。它们如下:

#### 模态对话框

模式对话框是一个阻塞对象。我们必须关闭此对话框才能继续任何其他任务。

确认对话框就是一个例子。例如，如果我们试图关闭一个未保存工作的应用程序，它会提示我们在退出前保存或不保存。该模式阻止我们退出应用程序，但它也减少了用户的挫折感，以防未保存的工作是由于用户的错误。

![Confirmation Dialog Box](img/6be651680dd75203e0f901f62988ade3.png)

#### 非模态对话框

无模式对话框是非阻塞对象。我们可以在不关闭对话框的情况下与不属于对话框的其他用户界面元素进行交互。这方面的一个例子是搜索框，我们可以在边上保持打开，并在需要时执行查询。

我们可以在旧版本的 Microsoft Office 产品中找到非模态对话框的例子；最新的版本已经把它们移到了侧边栏，而不是非模态对话框。另一个非模态对话框的例子是当我们在 Gmail 中按下**撰写**按钮时打开的**新消息**提示。

![New Message Prompt](img/fe766eda83834207736fd767606ac196.png)

### 覆盖模态

覆盖是通常包含非关键信息的模态。覆盖可以有几种类型:弹出、灯箱或全屏。

#### 弹出模式

这些通常会自动出现，无需任何用户操作，例如网站上的选择加入表单。他们往往可以跳过，但不断重复出现。它们要么自行消失，要么需要用户执行一些操作来关闭弹出窗口。为了防止不良 UX，在实现弹出窗口时，我们必须记住以下几点:

*   我们不应该在用户进入屏幕后立即显示弹出窗口
*   弹出窗口不应该干扰关键的用户交互
*   弹出窗口必须恰当地选择时间，这样才不会让用户感到沮丧

![Popup](img/2bd6c88012a32183389438db0b14bcd6.png)

#### 灯箱

灯箱使背景的内容不那么突出，因此覆盖内容是主要焦点。我们在尼尔森诺曼集团分类中发现了这一点。您经常会看到 lightbox 效果，即上面这样的弹出窗口将您的注意力集中在屏幕的一个区域。

#### 全屏模式

全屏模式是指内容占据整个屏幕，模式下的内容完全隐藏。当我们希望用户全神贯注，并且不希望他们被下面的内容分散注意力时，这些类型的模态特别有用。

全屏模式在哪里有意义？例如，当我们希望用户在地图上选择他们的位置时，我们可以实现全屏模式。这确保了良好的准确性，因为用户只关注地图。

以下是一个全屏弹出窗口的示例，显示了用户将光标移出网站主体区域的情况(即用户试图退出网站)。

![Popup After Moving Outside Website](img/28af86a1f89e66d8fec529edaa0601a2.png)

## 情态动词的 UX 问题是什么？

从用户的角度来看，当模态扰乱了手头的任务时，会令人沮丧。模态本质上是破坏性的，因为它们位于主应用程序之上。然而，情态动词在以下场景中最令人讨厌:

*   **缺乏组织**:如果模态中的内容组织得不好，用户找不到他们想要的东西，用户会感到失落。例如，每当用户试图在社交媒体平台上报告一个帖子，网站就会打开一个模型来接受一些额外的输入。如果输入的收集不简单和直观，用户在寻求帮助时可能会感到沮丧
*   **关闭困难**:如果没有简单的方法退出某个模式，用户会感到焦虑。例如，一些模态顶部可能没有关闭按钮。相反，它们可能需要一些操作来关闭模式，比如在弹出窗口之外单击。许多应用程序的现代实践是减少双重确认模态。例如，应用程序现在自动保存工作，而不是要求用户在退出前手动保存
*   持续:如果一个模态不断出现，并且没有办法长时间隐藏它，用户会感到沮丧。持久模式的一个例子是试用软件中的一个对话框，提醒用户这是一个试用版，它将过期。好的做法是限制这些模式，并在试用期快结束时向用户展示，这样用户就不会觉得我们在强迫他们购买。在一些应用程序中，通常有一个**贪睡**按钮，用户可以选择关闭这些提醒
*   **不清晰的微副本**:如果模态副本不简洁明了，导航起来就很混乱。最佳实践是遵循其他网站和应用程序中使用的通用语言，以匹配用户的心理模型(即，他们从以前的经验中习惯了什么)。一个很好的网站是 [Microcopy.me](https://www.microcopy.me) 。

## 那么，如何正确使用情态动词呢？

当实现一个模态时，主要的考虑变成:我们希望模态是阻塞的还是非阻塞的？

如果模态呈现的是可选的，用户可以跳过它，那么让它成为非阻塞的是有意义的，比如时事通讯的选择加入表单。如果模态呈现一些关键信息，比如帐户验证，那么背景变暗的阻塞模态是有意义的。

让我们看一些模态类型的例子，它们的缺陷，以及如何和何时使用它们。

### 避免嵌套模态

我们在实现模态时面临的一个关键难题是 web 和桌面应用程序中嵌套模态的概念。一般来说，我们应该不惜一切代价避免在一个模态中使用另一个模态；否则，我们的应用程序会变得混乱和复杂，就像一个镜子房子。

一个模式中的一个模式可以被接受的唯一异常情况是，当我们对关键操作(比如删除或任何需要密码确认的操作)执行双重确认时。

![Nested Modal](img/e53e99fe85d7603da7f629c4b249b8f5.png)

那么，当我们需要显示额外的上下文信息时，我们能做什么呢？有三种选择。

*   我们可以将模态内容移动到它自己的专用页面，而不是模态内容
*   我们可以使用可扩展的侧边栏。现代的应用程序可以在两边都有侧边栏——一个在左边用于主导航，另一个在右边用于上下文组件
*   我们可以有一个巨大的下拉列表，而不是在一个模型中显示一个模型。这是我们的常规下拉框，但是扩展到更大的尺寸，以适应我们的模态内容

![Sidebar and Mega Dropdown](img/53fff9c2b386a62088089a8e5cfca2c3.png)

*   在模态内容不需要太多空间的情况下，我们也可以利用工具提示、通知栏或小吃栏。

![Tooltip Notification Snackbar](img/05db22df1d2a9bd960f6d6162729ea89.png)

### 考虑如何定位情态动词

没有考虑太多的是模态的大小和位置。好的情态动词会有大量的空白，看起来会很愉快，因为它们有很好的比例，以及它们使用视觉层次和空间的方式。

然而，最重要的一点是确保模态处于用户的焦点上。如果是背景变暗的灯箱模式，那么聚焦就很容易实现。

如果我们在背景仍然可见的地方实现模态，那么我们必须注意位置，这样模态就不会造成视觉混乱。在没有背景变暗的情况下，向 UI 中出现的小模态添加动作通常也是一个好主意，以确保它吸引用户的注意力。

### 记住可访问性

让模态设计更具包容性的另一个步骤是让键盘用户和屏幕阅读器能够访问它们。您可以通过使用 tab 键将模式对象切换为选定项目来实现这一点。

屏幕阅读器还应该能够大声读出每个对象的适当标签。在设计组件时，我们已经定义了一个中间状态，称为悬停或选择状态。这个中间状态是一个重要的提示，让我们的用户了解他们在哪里执行他们的动作。

保存用户最后的活动状态也是一个好主意，这样用户可以在重新访问一个模态时从他们停止的地方继续。

### 模态必须是可伸缩的

另一件要考虑的事情是模态的可伸缩性。一个模态不能包含无限量的内容。因此，类似于 UI 屏幕和页面，模态本身可以有分页或步骤，我们可以单击 **Next** 进入下一步。

在这种情况下，我们显示一个进度条是至关重要的，这样用户的期望就能得到正确的管理，他们就能知道他们在这个过程中的位置。用户最后输入的数据应该自动保存以避免失败。

因为我们知道模态是破坏性的，它们会转移用户的注意力，所以我们应该尝试使用更微妙的 UI 用例，比如用户加入和特性声明。工具提示比模态提示更适合用来吸引用户对功能的注意和引导用户。

### 模态的其他最佳实践

最后，我有几个让你的模态步入正轨的小技巧。我们在实现模态时必须小心，否则我们会永久失去用户:

*   要记住的其他一些好的实践是，模态应该容易退出，无论是取消按钮、顶部的关闭按钮、键盘上的退出键，还是在窗口外单击
*   我们实现的模态必须有一个描述性但简洁的标题。包含一个对用户有指导意义的副标题也是一个很好的做法
*   在某些情况下，使用模态并不是一个好主意。例如，当显示错误、加载或成功状态时。在这种情况下使用模态可能是不好的 UX，因为状态改变的频率和它们阻塞父屏幕的事实。相反，我们可以将它们嵌入到现有的屏幕中，这不需要用户进行额外的操作。嵌入这些消息还允许用户在触发变更的主要组件的上下文中理解消息
*   模态不应用于显示装载状态。相反，我们可以把它们放在体内。例如，如果加载由按钮触发，那么按钮本身可以具有加载状态。模态的最佳用途是显示不可逆的动作，进一步引起对动作的注意，并减少不必要的错误
*   理想情况下，模态内部的消息传递应该是直接的、对话式的。例如，删除操作应该询问“您确定要删除吗？”而不是“释放存储空间？”后者是用户有意行为的副产品，把它作为首要问题来问会造成混淆
*   好的模态设计应该遵循和设计其他 UI 元素一样的 UX 原则。例如，我们应该尽量减少一个模型中的选项数量。主要和次要按钮是最常见的，但在某些情况下，我们可以有第三个按钮，如“了解更多”或“获得帮助”，但这些会使用户偏离模态的主要目的。

### 摘要

为了实现不伤害 UX 的模态，我们必须保持良好的模态分析。一个模态不能打断关键的用户交互，并且应该被恰当地计时。模态设计最重要的方面是决定什么时候使用模态是合适的，所以一定要记住这个建议。

## [LogRocket](https://lp.logrocket.com/blg/signup) :无需采访即可获得 UX 洞察的分析

[![](img/1af2ef21ae5da387d71d92a7a09c08e8.png)](https://lp.logrocket.com/blg/signup)

[LogRocket](https://lp.logrocket.com/blg/signup) 让您可以回放用户的产品体验，以可视化竞争，了解影响采用的问题，并结合定性和定量数据，以便您可以创建令人惊叹的数字体验。

查看设计选择、交互和问题如何影响您的用户— [立即尝试 LogRocket】。](hhttps://lp.logrocket.com/blg/signup)