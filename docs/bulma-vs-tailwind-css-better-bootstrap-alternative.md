# 布尔玛 vs .顺风 CSS:自举替代方案

> 原文：<https://blog.logrocket.com/bulma-vs-tailwind-css-better-bootstrap-alternative/>

Bootstrap 是一个开源的 CSS 框架，它采用移动优先的方法来开发响应性站点。它专注于简化整个开发过程，以减少花费在编写普通 CSS 上的时间。开发人员可以利用 Boostrap 的许多类来访问按钮、排版、表单、图像和图标的模板。简而言之，Bootstrap 简化并消除了 web 开发中的许多繁琐工作。

尽管 Bootstrap 仍然是 web 开发领域无可匹敌的王者，但近年来已经出现了几种替代产品。一些较新的框架受到优先考虑灵活性和速度的开发人员的青睐。

在本文中，我们将仔细研究两个流行的 Boostrap 替代方案:布尔玛和顺风 CSS。我们将考虑这两个框架如何组合，以及它们如何试图解决 Bootstrap 的一些缺点。我们还将讨论开发人员认为每个框架最吸引人的地方，以及什么类型的项目可能最适合布尔玛或顺风。

## 布尔玛

布尔玛是一个免费的开源 CSS 框架，主要由 Sass 和 Flexbox 系统构建。它提供了易于开发的模块化色谱柱，并配有令人惊叹的预定义调色板，以提供大量的设计选项。这些颜色可以和布尔玛丰富的类一起用于快速 UI 开发。

布尔玛不断更新新功能，使其适应最新的发展趋势。参考[布尔玛的文档](https://bulma.io/documentation/)了解更多关于这个框架的信息。

### 布尔玛 vs. Bootstrap:优点

与 Bootstrap 相比，布尔玛为定制提供了更多的灵活性。此外，它的模块化结构提供了对单个项目的更多控制。与 Bootstrap 不同，布尔玛允许用户只导入所需功能所需的模块，忽略任何不必要的模块。

### 布尔玛 vs. Bootstrap:缺点

与 Bootstrap 不同，布尔玛只支持 CSS。它不包括内置的 JavaScript 或 jQuery。在布尔玛中添加一个需要使用 JavaScript 或 jQuery 的基本特性，比如 toggle，需要编写一个定制脚本。

## 顺风 CSS

Tailwind 是一个越来越受欢迎的免费、实用优先的用户界面开发框架。从 2017 年作为辅助项目的初始版本开始，它迅速受到欢迎，成为[最广泛使用的实用程序优先的 CSS 框架，用于快速开发](https://adamwathan.me/tailwindcss-from-side-project-byproduct-to-multi-mullion-dollar-business/)。

Tailwind 可以用来创建高度定制的、优雅的用户界面，而无需编写定制的 CSS。这个框架被很好地记录下来，每年都有新的补充。

### 顺风 CSS 与引导:优势

与 Bootstrap 的文件大小(接近 300kb)相比，Tailwind 的文件大小相对较小(大约 27kb)。此外，使用 [purgeCSS](https://purgecss.com) 可以进一步减小顺风项目的整体文件大小。该工具通过检查文件并移除任何未使用的 CSS 类来减小文件大小。较小的文件大小是有利的，因为它通常意味着更快的页面加载速度。

### 顺风 CSS 与自举:缺点

尽管 Tailwind 本身并不包含预定义的组件，但是, [T](https://tailwindui.com) [ailwind UI](https://tailwindui.com) 包含了主要使用 Tailwind 构建的现成组件。这些组件既可以用于简单的 HTML 项目，也可以用于更复杂的 React 或 Vue 应用程序。

## Bulma 诉 tailwind css 案

除了布尔玛和 Tailwind 都是用于简单 web 界面开发的开源 CSS 框架，这两者之间没有太多相似之处。它们的目标都是简化 UI 开发，但是在解决这个问题上，它们采取了完全不同的方法。

Tailwind 允许开发者对界面的外观做出重要的决定。与此同时，布尔玛为开发人员做出大部分决定，提供内置模块来实现快速开发。

Tailwind CSS 是一个实用优先的框架，它不提供预构建的组件或 UI 工具包。相反，为开发人员提供了从头构建所有组件和布局的必要工具。

另一方面，布尔玛像其他传统的 CSS 框架一样带有预制组件，包括 Bootstrap 和 [MUI](https://mui.com) (以前的 Material UI)。另一种思考方式是，布尔玛提供了一个现成的模板，而 Tailwind 提供了开发者构建自己的模板所需的所有工具。

## 开发商喜欢布尔玛的什么

具有 Bootstrap 背景的开发人员通常会发现布尔玛相对容易掌握，因为两者都遵循相似的方法。Bootstrap 和布尔玛的功能类似于快速界面开发 UI 工具包。

对于开发用户界面经验有限的开发人员来说，布尔玛也是一个很好的选择。有了布尔玛，只需几个类就可以建立并运行一个 UI。这是顺风不容易实现的。即使在 Tailwind 中构建最基本的接口，也需要对框架的不同类进行详细、仔细的研究。

布尔玛还提供了高度的灵活性，允许开发者轻松定制。这与 Bootstrap 之类的类似框架形成了对比，Bootstrap 使得定制相当复杂。这种复杂性就是为什么大多数 Bootstrap 开发人员使用框架的默认模板，并且大多数 Bootstrap 站点具有非常相似的外观。

## 开发者喜欢顺风 CSS 的什么

大多数开发人员都会同意，不受任何限制地构建 UI 的能力怎么强调都不为过。与 Bootstrap 或布尔玛不同，Tailwind 不会将设计决策强加给开发者。这意味着所有的 UI 组件都可以从头开始构建。

另一个好处是，尽管 Tailwind 的标记文件布局很复杂，但开发人员仍然发现调试和跟踪样式错误相对容易。扫描 Tailwind 标记文件并进行任何必要的更改以反映开发人员的意图是一个简单的过程。

对于已经定义了界面、布局或设计美学的项目，Tailwind 是理想的选择。开发人员可以在最少约束的情况下，对每个组件的外观做出重要决定。

## 结论

布尔玛对顺风的争论类似于臭名昭著的 [React 对 Vue.js](https://blog.logrocket.com/angular-vs-react-vs-vue-js-comparing-performance/) dust up。没有明显的赢家。如果您正在寻找一个实用优先的框架，允许从头开始构建定制组件，那么您可能希望考虑 Tailwind。如果你需要快速构建一个 UI，并且正在寻找预定义的组件和接口，那么你可能希望考虑将布尔玛作为你的首选框架。

## 你的前端是否占用了用户的 CPU？

随着 web 前端变得越来越复杂，资源贪婪的特性对浏览器的要求越来越高。如果您对监控和跟踪生产环境中所有用户的客户端 CPU 使用、内存使用等感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/css-signup)

.

[![LogRocket Dashboard Free Trial Banner](img/dacb06c713aec161ffeaffae5bd048cd.png)](https://lp.logrocket.com/blg/css-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/css-signup)

LogRocket 就像是网络和移动应用的 DVR，记录你的网络应用或网站上发生的一切。您可以汇总和报告关键的前端性能指标，重放用户会话和应用程序状态，记录网络请求，并自动显示所有错误，而不是猜测问题发生的原因。

现代化您调试 web 和移动应用的方式— [开始免费监控](https://lp.logrocket.com/blg/css-signup)。