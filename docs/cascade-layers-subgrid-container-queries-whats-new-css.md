# 层叠层、子网格和容器查询:CSS 中的新内容

> 原文：<https://blog.logrocket.com/cascade-layers-subgrid-container-queries-whats-new-css/>

随着 CSS 逐渐接近它的 30 岁生日，它继续以前所未有的速度发展和改进。让我们粗略地看一下它即将推出的一些最新特性:层叠层、子网格和容器查询。

## CSS 中的层叠层

首先，让我们快速了解一下“级联”的确切含义

样式可以来自几个不同的来源，比如浏览器的默认值或者我们作为 web 开发人员创作的样式表。其中一些来源优先于其他来源。传统上，我们认为级联如下:

1.  用户代理样式表(浏览器默认值)
2.  用户设置
3.  作者样式表

网站作者样式表中的样式优先于浏览器默认样式。(这不要与[选择器特异性](https://css-tricks.com/specifics-on-css-specificity/)混淆，后者在级联的每一层内被解析。)

作为 CSS 作者，新的级联层特性允许我们添加和控制级联的附加层。因此，作者现在可以创建多个级联层并控制它们的优先顺序，而不是将网站的所有样式都集中在一个级联层中:

1.  用户代理样式表(浏览器默认值)
2.  用户设置
3.  作者层“C”
4.  作者层“B”
5.  作者层“A”
6.  等等。

在我们深入定义级联层的语法之前，让我们看一些用例，以及通过对级联的额外控制可以解决的问题。

### 为什么在 CSS 中使用层叠层？

级联层允许我们避开 CSS 代码中一些最棘手的部分。在某些情况下，我们将避免使用`!important`,因为基本的、可覆盖的样式可以简单地放在较低优先级的级联层中。

例如，您可以考虑将典型的“重置”样式放在它们自己的层叠层中，将第三方 CSS 库中的样式放在另一个层叠层中。您还可以将实用程序类——比如像 Tailwind CSS 这样的“原子 CSS”方法所使用的那些——放入它们自己的级联层中，以优先于通用样式，而不需要`!important`。

另一个用例可能是将现有的设计系统划分为不同的层，以方便主题化和定制。看看现代 JavaScript 框架如何采用级联层来改善他们的开发人员和用户体验将是很有趣的，这些框架通常带有类似 CSS 模块或通过仿真 shadow DOM 进行风格封装的工作区，尽管这些功能可能会更直接地受到`[@scope](https://drafts.csswg.org/css-scoping/)` [提案](https://drafts.csswg.org/css-scoping/)的影响，这是不同但相关的。

### CSS 中的语法

我们用`@layer`语句定义级联层和属于它们的样式。虽然不是必需的，但是您可以预先声明多个层(没有任何样式)来确定它们的优先顺序，如下所示:

```
@layer reset, base, utilities;

```

这将创建三个层，分别叫做`reset`、`base`和`utilities`。在`utilities`中定义的样式优先于`base`样式，后者优先于`reset`中的样式。

要向图层添加样式，我们在块中定义它们，如下所示:

```
@layer utilities {
  p {
    color: blue;
  }
}

@layer base {
  p {
    color: red;
  }
}

```

请注意，因为我们之前声明了层并定义了它们的顺序(初始行:`@layer reset, base, utilities;`)，所以页面上的任何段落都将是蓝色的，即使`base`层的样式是稍后定义的。如果我们省略了那一行，`base`层会优先，因为它确实出现在我们样式表的后面，因此段落会是红色的。

还可以创建匿名图层:

```
@layer {
  p {
    color: green;
  }
}

```

或将外部样式表加载到特定层:

```
@import url('utilities.css') layer(utilities);

```

我们还可以将层嵌套在其他层中:

```
@layer outer {
  @layer innerOne {
    /* ... */
  }
}

/* Dot syntax works, too. */
@layer outer.innerTwo {
  /* ... */
}

```

有几个选项和足够的灵活性！

### 什么时候可以在 CSS 中使用层叠层？

很快！在撰写本文时，cascade layers 已经发布到大多数 evergreen 浏览器上，并正在获得更广泛的支持。请参阅参考资料，如[caniuse.com](https://caniuse.com/css-cascade-layers)获取更新的浏览器支持信息。

### 最后一个`!important`注意

随着[多个级联层](https://blog.logrocket.com/how-css-works-understanding-the-cascade-d181cd89a4d8/)的加入，对`!important`如何工作的更深入理解变得更加重要。关于`!important`如何融入层叠以及它对层叠层的影响的细微差别，要有一个清晰且写得很好的解释(包括视觉效果)，一定要看看[这篇文章](https://css-tricks.com/css-cascade-layers/)。

## 介绍 CSS 的`subgrid`

这个新特性比级联层简单得多——至少在概念上是这样——但肯定仍然令人兴奋，CSS 作者对此充满期待。目前，网格行和列配置只扩展到网格容器的直接子容器。

这些子网格本身也可以是网格容器，但是为了与父网格保持一致，它们需要仔细配置它们的列或行，以精确匹配父网格的列或行。有时这很难或不可能，这取决于网格设置。

![Striped Outer and Inner Grids With a Frowny Face](img/fc0b731e1aed8fe3378aea9d3c9b5710.png)

It’s sometimes difficult to sync nested grids.

添加了`grid-template-rows`和`grid-template-columns`的`subgrid`值后，子网格现在可以继承父网格的行和列设置，并无缝、轻松地正确定位子网格，而无需定义自己的行和列设置或与之保持同步。不错！

![Striped Outer and Subgrid With Smirk Face](img/cd2c51106ee616c89967a09c39b30226.png)

Subgrids are perfect for when nested grids need to stay in sync.

关于`subgrid`更详细的解释，参见 MDN 文档上的[“子网格”。](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout/Subgrid)

### 什么时候可以用`subgrid`？

在撰写本文时，[只有 Firefox 实现了新的`subgrid`值](https://caniuse.com/css-subgrid)，但其他常青树浏览器也不会落后太多。

## 掌握 CSS 中的容器查询

当浏览器支持媒体查询时，网络在响应设计领域向前迈出了一大步。媒体查询解决了无数问题，但它们也暴露了复杂响应布局的一个新问题:上下文感知响应。

例如，您可能有一个组件存在于网页的主要部分中，并且需要根据该主要部分的宽度来更改其布局。但是，主部分的宽度本身取决于浏览器视口的宽度，并由其自己的相应布局设置控制。

对于当今的媒体查询，更改内部组件布局的唯一方法是基于浏览器视窗宽度的更改，这意味着您需要小心地保持其媒体查询与主部分的媒体查询同步，以便您的组件在每个视窗大小下看起来都是正确的。而且，如果你改变了你的主布局，你也需要更新你的组件。

容器查询解决了这个问题，它允许 CSS 作者响应任意元素的尺寸，而不仅仅是视口的尺寸。这将把组件样式从它们的父容器中分离出来，使响应组件更容易维护——并减少视觉布局错误！

### 什么时候可以使用容器查询？

不幸的是，该规范仍在积极开发中，还没有浏览器实现容器查询，因为最终的语法仍在完善中。然而，这是最广泛要求的特性之一，所以我们应该在不久的将来看到容器查询的定期进展和重大更新。

## 结论

CSS 正在迅速发展，为我们 web 开发人员和用户提供更好的原语。为您的下一个项目关注这些顶级特性！

## 你的前端是否占用了用户的 CPU？

随着 web 前端变得越来越复杂，资源贪婪的特性对浏览器的要求越来越高。如果您对监控和跟踪生产环境中所有用户的客户端 CPU 使用、内存使用等感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/css-signup)

.

[![LogRocket Dashboard Free Trial Banner](img/dacb06c713aec161ffeaffae5bd048cd.png)](https://lp.logrocket.com/blg/css-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/css-signup)

LogRocket 就像是网络和移动应用的 DVR，记录你的网络应用或网站上发生的一切。您可以汇总和报告关键的前端性能指标，重放用户会话和应用程序状态，记录网络请求，并自动显示所有错误，而不是猜测问题发生的原因。

现代化您调试 web 和移动应用的方式— [开始免费监控](https://lp.logrocket.com/blg/css-signup)。