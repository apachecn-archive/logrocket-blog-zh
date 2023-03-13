# 利用菲特定律有效引导用户使用你的应用

> 原文：<https://blog.logrocket.com/ux-design/using-fitts-law-guide-users-app/>

## 菲特定律是什么？

1954 年，心理学家保罗·费茨发现，指针与目标之间的距离越大，指针到达目标的时间就越长；换句话说，更近的目标更快更容易接近。然而，目标的大小对所需时间有直接影响，因此较大的目标更快更容易到达。

他的定律指出，当目标小而远时，错误率增加，当目标大而近时，错误率减少。

下图描述了目标和指针在速度、时间和距离方面的关系。

![Fitt's Law Diagram](img/83e316df8a702fc407be030bc57ac677.png)

借助以下示例，让我们更好地理解菲特定律，然后深入讨论这些主题，以便更好地实施菲特定律:

### 示例 1

![Fitt's Law Big and Little Target](img/1c9e1e4a19786554a1e20abde1ccd12a.png)

为了帮助你理解菲特定律，看看上面的图片，告诉我从你所在的地方用一块小石头砸哪个最容易。

如果你不是神枪手，我猜你的答案是前面那个更大的。

### 示例 2

![Fitt's Law Two Target Buttons](img/b99dcc68521531750a39525d11cf9009.png)

让我们看另一个例子。看上面的图片，用你的鼠标(或者你的拇指，如果你正在用你的智能手机阅读这篇文章)指向两个圆圈的中心。

如果我是正确的，你花了几毫秒指向较大的圆，并且最难指向较小的圆。

## 主要像素和神奇像素

当用户停留在页面上时，鼠标指针第一次出现的点称为主像素。为了让设计者充分利用菲特定律，有必要知道主要像素的位置。

诸如 MacOS 和 Windows 之类的操作系统有效地使用了主像素，因为在引导时光标总是出现在主像素区域。

相比之下，在这些操作系统上运行的 web 应用程序、网站和其他软件程序则不是这种情况，因为指针的位置会被改变。好的一面是，当用户做某件事时，你可以计算出可能的像素，即使你在网站上找不到像素。

让我们来看一个最能展示素像素的小例子:

![The Prime Pixel on a Dropdown Menu](img/1eedbb43d272fafff3bfcd4e42d625e6.png)

当用户点击图标旁边的**帮助**按钮时，会出现下拉菜单。这通知用户所执行的动作的结果可能性。

与主像素相似，屏幕的边缘包括四个额外的基本像素，称为幻像素。这些是离主要像素最远的像素，设计师在这里放置任何 UI 元素时都必须深思熟虑。

下面的快照展示了 Windows 11 操作系统中的神奇像素。

**![Magic Pixel Locations in Windows 11](img/6a0286853428a7c614d08230268a4ecd.png)**

由于它们与主要像素的距离，这四个像素被认为是设计中任何重要或交互元素最无用的位置。

一般来说，从设计者的角度来看，主像素是屏幕中间的像素，直到它被作用并移动。这种模式可以在谷歌的搜索框中观察到。当你访问谷歌搜索信息时，搜索框位于中心，让你可以方便地输入和搜索所需的信息。

**![The Prime Pixel on the Google Search Bar](img/343aa55d525457bcee7bd398d4bcc610.png)**

## UI 设计中的菲特定律三法则

菲特定律经常被应用在用户界面设计中，正如在大多数具有强大可用性的数字产品中所看到的那样，在这些产品中，与用户任务相关的所有交互组件都是大的、紧密的、可快速访问的。

在视觉界面设计或任何涉及到用鼠标指针或拇指点击和选择的界面设计领域中，这个法则尤其重要。

一般来说，Fitt 定律可以应用于 UI 设计的三个简单部分:

1.  尺寸:UI 组件需要清晰、可点击、清晰
2.  努力:使用优质和神奇的像素，让你的用户界面元素易于定位和交互
3.  距离:考虑元素之间的距离，结合距离使用大小和努力来产生视觉上吸引人的功能体验

## 在桌面界面中应用菲特定律

### 注意你的重要按钮的大小和距离

当开发一个用户界面时，考虑元素的大小和到最近最常用的交互的距离是很重要的。按钮的大小和到最近的交互元素的距离是按钮最重要的设计准则。

考虑高风险的交互元素也很重要，比如您不希望用户意外激活的删除操作。这些通常应该放在远离最常用的交互元素的地方。

下图中，**删除**按钮与**下载**和**编辑**都在一起。由于**删除**具有固有的破坏性，所以它必须总是放在远离最常用按钮的地方。

![Delete Button in the Wrong Position](img/e201cf9fc06a8ea833706ac7e447e282.png)

### 利用易接近的优势

鼠标指针在边缘(上、下、左、右和角)结束。用户需要更少的精确度，因为他们可以简单地将鼠标扔向角落，指针将受到屏幕边界的限制。这也是你在角落里放 Windows 和 Apple 菜单的原因之一。

![Buttons on the Screen Corners](img/3698714853ab39e740d505d65548a44b.png)

同样，上边距和下边距也更容易接近。顶部和底部不容易触及，但它们仍然比屏幕中间的一个点提供更快的访问。这也是苹果的应用菜单位于屏幕上方的原因。

![An App Menu at the Top of the Screen](img/4e290a440c7d06ad65ed902e83f3af15.png)

### 提供有用的弹出菜单

弹出菜单是缩短指针和菜单项之间的时间和距离的好方法。右键下拉菜单是最常见的弹出菜单类型，可以节省用户搜索设置的时间。

![Helpful Pop-Up Menu](img/a237de7e9151cdf77f50e526880164ea.png)

## 在移动界面中应用菲特定律

在移动用户界面中，光标就是你的手指。与指针相反，手指比鼠标指针粗且不准确。所以移动界面触摸目标要大一点是必不可少的。

例如，考虑占据移动设备上大部分水平屏幕宽度的 CTA 按钮。

![Large Button for a Mobile Device](img/80819572ef40f1cb2f52c6301bddf364.png)

由于其大小和位置，当人们仅使用拇指与移动设备交互时，屏幕底部的按钮很容易被触及。

相比之下，当交互最多的元素位于我们的自然拇指范围之外时，用户必须用双手才能够到它们，这就变得很有挑战性。

### 将按钮放在单手容易够到的位置

牢记这一点，设计师必须始终将所需目标定位在此范围内。

![Phone Locations for Natural Hand Reach](img/1b6f08b0550832545321dcf429a818e4.png)

在应用菲特定律时，理解屏幕的主要目标、它要完成的目标以及我们如何让用户轻松实现这个目标是很重要的。

## 菲特定律的好例子和坏例子

菲特定律不能被认为是所有问题的解决方案；在有些情况下，应用它是有意义的，而在其他情况下，替代原则更为合适。

有些情况下，菲特定律可以被用作一种黑暗模式，这种情况发生在搜索引擎页面上突然弹出的广告上。

在下面的例子中，广告在我打开页面后立即出现在主要像素位置。关闭第一个广告后，第二个广告出现在顶部边缘，带有大文本和 CTA。这只是网站在广告中利用菲特定律的众多方式之一。

![Pop-Up Menu Dark Pattern](img/c05260aca0cf3065693ece47ae805b64.png)

虽然有很多网站在搜索结果旁边显示相关广告的例子，但也有竞争对手利用这一点误导用户。

为了更好地说明菲特定律的例子，我准备了更多的例子。这些应用程序是我个人的最爱，我每天都在使用它们；为了本文的教育目的，我故意让它们变坏。

### 示例 1:查找购买按钮

![Purchase Button Placements](img/c748a47a9d17d45b109c367621fa327d.png)

在上面的快照中，用户的主要动作是完成他们的购物车，并通过支付完成购买。

在这个糟糕的例子中，**购买**选项位于屏幕的右上角，使得用户很难只用一只手来点击。

然而，在这个很好的例子中，**购买**按钮在屏幕的底部，所有相关信息从左到右，这使得点击并完成手头的任务变得很简单。

### 示例 2:较大的表单选项

![Larger Form Options](img/14c9914af3ea25bd4a3c0fe9da819704.png)

在上面的截图中，用户正处于一个健身应用程序的入职过程中，必须选择使用该应用程序的主要目标。

在这个糟糕的例子中，目标只是一个带有单选按钮的清单，这些按钮靠得太近，使得用户很难点击。这个列表也需要更多的认知努力来消化屏幕上的所有信息。此外，下一个的动作按钮**远离用户的拇指范围。**

然而，在这个很好的例子中，所有的目标都设置得很好，支持图形之间的距离也很合适。“行动号召”按钮也可以方便地用拇指操作。

### 示例 3:简化的标签栏

![Simplified Tab Bars](img/774e1e55ceb69447a163df706fa8398e.png)

在示例中，您可以看到应用程序的主页。主页是任何数码产品最重要的部分，因为它决定了用户的下一步行动。做对了，你可能会有一个忠诚的、付费的客户；做错了，几秒钟就卸载了。

在这个糟糕的例子中，标签栏被太多的功能紧密地放在一起，很容易出错。

相比之下，好的例子只使用必要的功能，并将它们放置在彼此安全的距离之外。根据[苹果的可访问性指南](https://developer.apple.com/design/human-interface-guidelines/foundations/accessibility/#:~:text=Give%20all%20touchscreen%20controls%20and%20interactive%20elements%20a%20hit%20target%20that%20measures%20at%20least%2044x44%20pt.%20People%20with%20limited%20mobility%20need%20larger%20hit%20targets%20to%20help%20them%20interact%20with%20your%20app.%20It%20can%20be%20frustrating%20to%20interact%20with%20too%2Dsmall%20controls%20in%20any%20platform%2C%20even%20when%20people%20use%20a%20pointer.)，所有触摸屏控件和交互元素都有一个至少 44 磅的点击目标。

### 例子 4:把丢弃按钮扔得远远的

![Moving the Delete Button Location](img/2451ed5c88a7826ba8ff7b4cb7a57334.png)

这个应用程序可能是这个星球上使用最多的电子邮件服务。在屏幕截图中，您可以看到电子邮件编辑器窗口的两个独立示例，这里的主要目标是编写电子邮件并将其发送给收件人。

并且，在左边的例子中，你可以看到一个**丢弃**按钮紧挨着一个**发送**按钮。这在几个层面上都是不正确的，原因如下:

1.  **发送**和**丢弃**没有相同的意义，把它们放在一起是违反直觉的
2.  **丢弃**按钮的颜色吸引了太多的注意力，而它甚至不是这里的主要功能
3.  **发送**和**丢弃**按钮颜色相同，用户可能不小心点击了**丢弃**而不是**发送**

然而，在右边的例子中，垃圾箱按钮离发送按钮很远，减少了人为错误的可能性。它位于屏幕的边缘，这是另一个可访问点。

## 最后的想法

虽然菲特定律是在 20 世纪 50 年代末，远在互联网出现之前制定的，但它仍然是数字产品设计领域提高参与度和盈利能力的重要原则。

虽然菲特定律是良好可用性的良好起点，但它不是唯一甚至是最重要的设计原则。数据告诉我们的不仅仅是原则可以理论化的东西，了解设计方案是否以用户为中心的最佳选择是对你的用户进行可用性测试。

理论是一个很好的起点，但提出问题，从现实世界中获得灵感，迭代解决方案，更深入地理解事物为什么以及如何工作，然后最终将它们应用到现实世界将是最有效的。

## [LogRocket](https://lp.logrocket.com/blg/signup) :无需采访即可获得 UX 洞察的分析

[![](img/1af2ef21ae5da387d71d92a7a09c08e8.png)](https://lp.logrocket.com/blg/signup)

[LogRocket](https://lp.logrocket.com/blg/signup) 让您可以回放用户的产品体验，以可视化竞争，了解影响采用的问题，并结合定性和定量数据，以便您可以创建令人惊叹的数字体验。

查看设计选择、交互和问题如何影响您的用户— [立即尝试 LogRocket】。](hhttps://lp.logrocket.com/blg/signup)