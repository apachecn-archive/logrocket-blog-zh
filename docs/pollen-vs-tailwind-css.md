# 花粉 vs .顺风 CSS:寻找更好的构建体验

> 原文：<https://blog.logrocket.com/pollen-vs-tailwind-css/>

前端开发不断发展，以满足用户的期望和体验，并简化和加快开发过程，缩短发布周期。

在不断寻求解决方案的过程中，新的框架、范例和架构被创造出来，其中一些扰乱了行业，或者彻底改变了开发人员的工作方式。

在过去的几年中，一组具有完全不同概念的新框架吸引了前端开发人员的注意。这些框架现在在前端世界非常流行，你可能已经很熟悉它们了: [Tailwind CSS](https://tailwindcss.com/) 和[花粉](https://www.pollen.style/)。

在本文中，我们将讨论它们的优点、缺点、实现范例等等，这样您就可以做出在下一个项目中使用哪个框架的明智决定。

## 内容

## 什么是顺风 CSS？

顺风 CSS 颠覆了我们设计网站和网络应用的方式。虽然一开始有争议，但它已经赢得了许多前端开发人员的喜爱，因为它的简单性和一致性，还因为它提供了一种更快的编写 CSS 的方法。

但是 Tailwind 试图解决 CSS 的什么问题呢？浏览器兼容性。虽然一般的 CSS 在过去几年里有所改进，但是如果我们不小心的话，网站在不同的浏览器中会看起来不一样。

此外，我们还有用户产生的问题、幻数、失控的类名，以及使用 `!important`作为覆盖糟糕设计的手段。

Tailwind CSS 通过提取 CSS、[标准化单位](https://blog.logrocket.com/tailwind-css-tips-for-creating-reusable-react-components/)、颜色和属性，并保证跨浏览器兼容性，简化了这一过程。

它还提供了代表每种可能的样式配置的类的集合，开发人员可以使用这些类作为他们设计的构建块。

最精彩的部分？有用！使用 Tailwind CSS 非常棒，尽管它也有一些缺点，我们将在下面讨论。

## 顺风 CSS 的常见问题

### 为单个元素设计样式的类的数量多得离谱

Tailwind CSS 并不完美，该框架中最明显的缺陷之一是实现时看起来有多难看。就连它的创造者也要求开发者“抑制住干呕的冲动，给它一个机会”。

但是到底是什么问题呢？让我们看一个用 Tailwind CSS 构建的简单按钮的例子:

```
<button type="button" class="inline-flex items-center px-2.5 py-1.5 border border-transparent text-xs font-medium rounded shadow-sm text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
  Button text
</button>

```

不可否认，像按钮这样的简单对象读起来会变得过于冗长和难看，但是如果你给 Tailwind 一个机会，它的简单性会战胜它的语法。

### 学习顺风 CSS

每一个前端开发人员都熟悉 CSS，通过经验，开发人员开始学习无数的属性和值来实现站点的期望外观。

当切换到 Tailwind 时，尽管大多数类名都很直观，但开发人员必须学习它们，从添加填充和边距等简单的事情到使用选择器和嵌套属性。

例如，如果我们想要使用 CSS 居中文本，我们将应用以下内容:

```
text-align: center;

```

下面是我们在 Tailwind CSS 中使用的代码:

```
text-center

```

这个名字很有意义，但是我们必须重新学习。当我们转到选择器时，事情也会变得有点复杂。

让我们看看下面的 CSS 例子:

```
<style>
.my-button {
  background-color: #4CAF50; /* Green */
  border: none;
  color: white;
}

.my-button:hover {
  background-color: white; 
  color: black; 
  border: 2px solid #4CAF50;
}
</style>

<button class="my-button" />

```

以及使用顺风的类似实现:

```
<button class="bg-green-600 hover:bg-white border-0 hover:border-2 text-white hover:text-black hover:border-green-600" />

```

选择器的工作方式与 CSS 完全不同——它们在前面加上一个类名，因此很难理解两种状态之间的区别。

与 CSS 类一样，状态被分组在一起；使用 Tailwind CSS，每个类都可以独立于其他类进行调整，并且可以按照几乎任何顺序工作。

### 顺风公司对重复或复杂场景的解决方案

如果我们在屏幕上有多个按钮，所有按钮都有相同的边距、填充和属性，但是颜色不同，会怎么样？我们将不得不在我们的 HTML 代码中复制几个类，并且，如果我们需要改变填充，将`p-2`编码为`p-4`。这变成了一项乏味的任务。

顺风公司对这个问题的解决方案是什么？使用传统的 CSS 方法，定义您的类，并设置您的属性。它们甚至提供了一种使用 Tailwind 的类作为自定义类定义的方法，如下所示:

```
<style>
.my-button {
  @apply bg-green-600 border-0 text-white;
}

.my-button:hover {
  @apply bg-white border-2 text-black border-green-600;
}
</style>

<button class="my-button" />

```

这才是真正的汤。不是顺风，肯定不是 CSS，看起来也不干净。

## 花粉是什么？

进入[花粉](https://www.pollen.style/)，它是为了保护顺风 CSS 的自然之美，同时解决我们已经讨论过的一些问题。通过使用 CSS 变量，它采取了完全不同的方法。

Tailwind CSS 背后的想法很棒。问题在于它的实施。打破它与 CSS 的关系剥夺了它对 CSS 的熟悉和力量。花粉则相反，它依赖于 CSS，并提供了一组预定义的 CSS 变量用于标准化。

让我们看看我们的简单按钮示例，现在使用花粉:

```
<style>
.my-button {
  background-color: var(--color-green-500)
  border: none;
  color: white;
}

.my-button:hover {
  background-color: white; 
  color: black; 
  border: 2px solid var(--color-green-500);
}
</style>

<button class="my-button" />
```

语法看起来很熟悉，我们仍然保留了使用 Tailwind CSS 的一些好处。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

那么，花粉是简单的进化，比顺风好吗？不一定！花粉也有它自己的利弊。

## 使用花粉的缺点

### 类名可能会失控

即使在同一家公司，每个项目对 CSS 类的命名通常也是不同的，这将导致开发人员很难发现类是否在使用中，是否需要为正在构建的新组件添加新的类，在某些情况下，甚至会破坏设计，因为我们修改或删除了一个在比我们最初想象的更多的地方使用的类。

这个基本问题在 Tailwind CSS 中通过标准化类名得到了解决。然而，在花粉中，用户生成的类是标准的。因此，花粉和普通的 CSS 一样容易受到这个问题的影响。

这并不是非常错误，良好的工程实践有助于解决这个问题。然而，根据我的经验，每个项目最终都会陷入 CSS 混乱。

### 与顺风 CSS 相比，花粉是准系统

即使花粉可以通过附加组件进行扩展，但它并不像顺风那样完整。我最喜欢的一个例子是 CSS 网格。网格可能很难理解和实现，而 Tailwind CSS 很好地用`grid-col-1`、`grid-col-3, grid-span-2,`等类抽象了一些复杂性。

有了花粉，你基本上只能靠自己了。

### 花粉是新东西，需要经过实战检验

花粉是一个相对较新的框架，而 Tailwind CSS 已经在数以千计的不同项目中经受了考验

虽然它在概念上看起来很棒，但它在现场的表现将决定它是否是一个长期存在的框架。

这不是一个公平的比较，但是让我们看看这两个项目在写作时的统计数据。

| 顺风 CSS | 花粉 | 明星 |
| 大约 50k | 500 | 叉 |
| 2.3k | 9 | 承诺 |
| ~4k | 196 | 分担人 2035 |
| Tailwind CSS 显然处于主导地位(目前)，但花粉正在快速增长，如果开发人员了解这个项目并喜欢它，它有可能很快赶上 Tailwind CSS。 | 结论 | Tailwind CSS 和花粉的目标都是优先考虑一致性、标准化和可组合性，尽管有两个完全不同的实现。 |

“哪个框架是最好的？”这个问题没有正确的答案，因为每一个都有优点和缺点，但是我想谈谈我个人对这个问题的看法。

## 我个人的偏好是用 Tailwind CSS。我相信好处大于问题，而且，在使用 Tailwind 完成多个项目后，我真的很享受这种体验。

为什么我更喜欢顺风 CSS 而不是花粉？因为花粉和我已经在用 CSS 做的没什么不同。

当我从事一个新的基于 CSS 的项目时，我会首先创建一组 CSS 变量来反映设计系统的基本模式，因此，在某种程度上，我已经在以类似于花粉的方式工作，尽管可能选项更少。这同样适用于你们中的许多人。

花粉并没有带来任何根本性的不同。它只是为我们已经在 CSS 中做的事情提供了更多。相反，Tailwind CSS 引入了一种不同的方法。它抽象了复杂性，并且忽略了丑陋的类语法，非常适合使用。

正因为如此，我的心还是属于顺风 CSS 的。

你最喜欢的框架是哪个？请在评论中告诉我。我很想听听你的想法。

感谢阅读！

你的前端是否占用了用户的 CPU？

随着 web 前端变得越来越复杂，资源贪婪的特性对浏览器的要求越来越高。如果您对监控和跟踪生产环境中所有用户的客户端 CPU 使用情况、内存使用情况等感兴趣，

.

## LogRocket 就像是网络和移动应用的 DVR，记录你的网络应用或网站上发生的一切。您可以汇总和报告关键的前端性能指标，重放用户会话和应用程序状态，记录网络请求，并自动显示所有错误，而不是猜测问题发生的原因。

现代化您调试 web 和移动应用的方式— [开始免费监控](https://lp.logrocket.com/blg/css-signup)。

[try LogRocket](https://lp.logrocket.com/blg/css-signup)

.

[![LogRocket Dashboard Free Trial Banner](img/dacb06c713aec161ffeaffae5bd048cd.png)](https://lp.logrocket.com/blg/css-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/css-signup)

[LogRocket](https://lp.logrocket.com/blg/css-signup) is like a DVR for web and mobile apps, recording everything that happens in your web app or site. Instead of guessing why problems happen, you can aggregate and report on key frontend performance metrics, replay user sessions along with application state, log network requests, and automatically surface all errors.

Modernize how you debug web and mobile apps — [Start monitoring for free](https://lp.logrocket.com/blg/css-signup).