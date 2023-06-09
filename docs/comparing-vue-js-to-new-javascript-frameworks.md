# 将 Vue.js 与新的 JavaScript 框架进行比较

> 原文：<https://blog.logrocket.com/comparing-vue-js-to-new-javascript-frameworks/>

随着前端设计对于应用程序的成功变得越来越重要，使用最佳前端框架的需求也变得越来越迫切。

找到解决项目中特定问题的最佳框架可以提供更好的前端设计和用户体验，帮助品牌和开发商吸引和留住更多用户。

对于从事 JavaScript 工作的开发人员来说，Vue 已经成为一个流行的、成熟的框架。然而，不同的项目需要不同的解决方案，找到 Vue 的替代品可以推动项目前进，提高速度、性能和社区。

在这篇文章中，我们将 Vue 与 Svelte、Riot、Hyperapp 和 Alpine 进行比较，这些新的、不太为人所知的 JavaScript 框架已经培养了一批追随者，并提供了有用的功能。

## Vue.js 快速概述

Vue 是一个开源的 JavaScript 框架，使用模型-视图-视图模型(MVVM)设计模式，代表了 Vue 应用程序中的三个层次。

如果你熟悉[流行的模型-视图-控制器(MVC)模式](https://blog.logrocket.com/dont-underestimate-the-model-in-mvc/)，Vue 通过使用视图模型层执行控制器任务。

在 Vue 应用程序中，模型层提供对数据的访问。视图模型层包含了将数据从模型移动到视图的逻辑，反之亦然。

Vue 模型可能看起来像这样:

```
var model_data = {
  js_frameworks: [‘Vue’, ‘Svelte’, ‘Riot’, ‘Hyperapp’, ‘Alpine’]
};

```

视图模型层使用双向数据绑定连接视图层和模型层。在 Vue 中，视图模型对象可以按如下方式实例化:

```
var vm = new Vue({ 
  el: ‘#app’,
  data: model_data
});

```

注意，`el`参数使用元素的 ID 将视图模型层连接到视图中的任何元素。在这种情况下，我们将视图模型层绑定到 ID 属性值为`app`的元素。然后，数据参数将视图模型层连接到模型。

视图层由 DOM 及其所有元素组成，向用户显示模型层包含的数据。上面模型和视图模型层的相应视图如下所示:

```
<div id=”app”>
  <ul>
    <li v-for=”framework in js_frameworks”>{{framework}}</li>
  </ul>
</div>

```

上面最外面的 div 的 ID 对应于视图模型层中指定的 ID，提供对视图中模型数据的访问。我们使用 Vue 的语法`v-for`创建一个 for 循环来遍历数据，并将其显示为一个列表。

现在我们已经熟悉了 Vue 及其工作原理，让我们将它与一些新的 JavaScript 框架进行比较。

## vue . js vs .快速

比较框架时要考虑的一个常见特性是速度。在 Vue vs. Svelte 的例子中，观察每个框架如何通过操作 DOM 来构建和运行应用程序提供了这种洞察力。

因为 Vue 通过虚拟 DOM 呈现应用程序的用户界面，所以增强的副本使操作变得更容易。虽然这种方法很快，但在运行时编译会大大减慢加载过程。

然而，Svelte 在构建时解决了这个性能问题。这个 JavaScript 框架以其速度和性能著称。它附带了一个编译器，当在应用程序上运行构建时，它可以将苗条的框架代码转换为普通的 JavaScript。

当完成一个应用程序的构建时，所有苗条的痕迹都消失了，只留下香草 JavaScript。由于浏览器理解 JavaScript，所以不需要下载库，消除了原本花费在下载上的时间。

与 Vue 不同，Svelte 直接对 DOM 进行修改。此外，仅包含普通 JavaScript 代码的包通常比库附带的包要轻。

所有这些方面共同提高整体性能。

虽然 Vue 和 Svelte 都有简单、易于理解的语法，但 Svelte 实现不同功能所需的代码略少。

与使用 MVVM 设计模式的 Vue 相比，Svelte 也完全抛弃了设计模式。相反，Svelte 在同一个页面上创建了包含所有 HTML、CSS 和 JavaScript 的封装组件:

```
<script>
  let name = "Samson";
</script>

<main>

  <input bind:value="{name}" />
  <p>My name is {name}.</p>

</main>

<style>
  p {
    color: red;
  }
</style>

```

在上面的 JavaScript 代码中，我们创建了一个保存字符串的变量。在 HTML 中，输入框和段落通过使用`bind`属性的双向数据绑定来连接。

代码给了我们一个文本框，里面有`name`保存的文本。它还会将文本插入文本框下方的段落中。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

如果我们改变文本框中的值，`name`保存的值和插入段落的值也会改变。在我们的风格中，我们将段落文本的颜色设为红色。

虽然有些人更喜欢 Svelte 将代码、标记和样式保存在一个地方的简单方法，但它通常会被视为过时，而且根据项目的不同，Vue 的现代 MVVM 设计模式可能更受欢迎。

在社区、用户群和支持方面，Vue 确实占了上风。由于 Svelte 的生态系统仍在增长，其用户没有 Vue 开发者享有的资源、开源工具、插件和社区支持。

总的来说，这两个框架都被认为很容易学习，有很好的文档，并且只需要 JavaScript 的基础知识就可以采用。

然而，与 Vue 相比，Svelte 的功能无缝地协同工作以提高性能，具有更短的加载时间，更大的存储空间和整体的轻便性。

## vue . js 诉 Riot.js

Riot.js 引以为豪的是一个轻便简单的 UI 库,它可以帮助开发人员在为他们的应用程序创建优雅的 UI 时立即投入使用。

与 React 非常相似，用户可以在 Riot 中创建自定义标签。这是该库的卖点之一，因为开发人员可以:

*   用 HTML 和 JavaScript 创建标题、导航栏、按钮和卡片等组件
*   为了可读性，将组件包装在可以唯一命名的元素中
*   无限期地重复使用组件

使用 Riot 的另一个优势是它的大小。它标榜自己是一个极简框架，提供创建前端项目所需的最低限度。因为与 Vue 相比，这个框架中的特性更少，所以需要学习的东西更少，而且它在浏览器中加载很快。

与 Vue 使用的 MVVM 模式不同，Riot 使用的是模型-视图-展示者(MVP)模式。模型和视图的工作方式类似于 Vue 的模型和视图，但是，Riot 使用一个表示层来代替视图模型层，将数据从模型传输到视图，反之亦然。

Vue 和 Riot 的一个主要区别是，Vue 使用虚拟 DOM 来呈现应用程序的 UI，而 [Riot 使用表达式绑定](https://v3.riotjs.now.sh/compare/#virtual-dom-vs-expressions-binding)，类似于 AngularJS。这意味着每次对代码进行更改时，它都会进入 DOM 树并更新节点。

表达式绑定对于中小型应用程序是有益的，但是对于大型应用程序可能会导致性能问题。

然而，Vue 相对于 Riot 的一大优势在于它的社区。Riot 还没有被广泛采用，而 Vue 已经被更多的主流公司和开发者采用。

## vue . js vs . hyperapp

Hyperapp 是一个用于创建应用前端的超轻量级框架。它的总大小约为 1KB，比 Vue 启动更快，需要的内存更少，当应用程序在低端设备上运行时，这一优势就会凸显出来。

这些框架之间的一个相似之处是它们都使用虚拟 DOM。

如果你正在构建一个复杂的应用程序，Vue 强大的内置特性和社区将为你提供最好的服务。然而，如果你正在寻找一个简单的框架，你应该试试 Hyperapp。

与 React 类似，Hyperapp 支持 JSX，并允许开发人员创建可重用的组件来与其他框架一起使用。注意，在 Hyperapp 中使用 JSX 时，必须用编译器将 JSX 代码转换成函数调用，因为浏览器不能解释 JSX。

与 Vue 相比，Hyperapp 的简单性使其易于采用。它鼓励不变性，并且比 Vue 提倡的可变性更不容易出错。

像我们到目前为止看到的其他框架一样，Hyperapp 不是很受欢迎。然而，它的小社区积极致力于改进该框架。在这篇文章发表的时候，Hyperapp 还没有网站，它的文档也没有 Vue 的详细。要了解更多关于 Hyperapp 如何工作的信息，请查看其创建者开发的简单教程。

## Vue.js 诉 Alpine.js

使用 Alpine 构建项目很容易上手[。不需要安装，您只需将它的库包含在项目中就可以开始使用它:](https://github.com/alpinejs/alpine)

```
<script src="https://cdn.jsdelivr.net/gh/alpinejs/[email protected]/dist/alpine.min.js" defer></script>

```

不需要复杂的构建工具、捆绑器和包管理器。

虽然 Vue 也为开发者提供了 CDN，但是用户不能使用单文件组件。对于大型 Vue 应用程序，建议通过 npm 安装它。

Alpine 的一大优势是它是轻量级的，使得用户不太可能遇到任何速度和性能问题。它很大程度上受到了 [Tailwind CSS](https://blog.logrocket.com/using-tailwind-css-in-production/) 的启发，因为用户可以使用类直接在 HTML 标记上使用 JavaScript。

Alpine 也比 jQuery 更新，所以它处理 DOM 的方法更现代。与使用虚拟 DOM 的 Vue 不同，Alpine 在构建应用程序时直接修改真实 DOM。

就语法而言， [Alpine 与 Vue 非常相似——这是它的创造者 Caleb Porzio 有意为之。](https://github.com/alpinejs/alpine/tree/v2.8.2#alpinejs)语法附带了 14 条指令，将 JavaScript 加入 HTML:

```
x-data
x-init
x-show
x-bind
x-on
x-if
x-for
x-model
x-text
x-html
x-ref
x-transition
x-spread
x-cloak

```

查看本[指南，了解如何使用这些阿尔卑斯山指令](https://blog.logrocket.com/getting-started-with-alpine-js/)。

对于 Vue 过于繁重的项目来说，Alpine 是一个完美的选择，比如只需要少量功能的简单应用程序。

## 结论

我们仔细观察了一些快速发展的新 JavaScript 框架，有一天可能会对 Vue 这样的成熟框架构成强有力的竞争。

值得注意的是，这篇文章并不是为了展示任何比 Vue 更好的框架，而是为了让读者了解一些不太为人知的框架，这些框架可能满足不同的需求，比如轻便和简单。

查看这些新框架，并尝试在后续项目中使用它们，以直接了解它们所呈现的优势。

## 像用户一样体验您的 Vue 应用

调试 Vue.js 应用程序可能会很困难，尤其是当用户会话期间有几十个(如果不是几百个)突变时。如果您对监视和跟踪生产中所有用户的 Vue 突变感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/vue-signup)

.

[![LogRocket Dashboard Free Trial Banner](img/0d269845910c723dd7df26adab9289cb.png)](https://lp.logrocket.com/blg/vue-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/vue-signup)

LogRocket 就像是网络和移动应用程序的 DVR，记录你的 Vue 应用程序中发生的一切，包括网络请求、JavaScript 错误、性能问题等等。您可以汇总并报告问题发生时应用程序的状态，而不是猜测问题发生的原因。

LogRocket Vuex 插件将 Vuex 突变记录到 LogRocket 控制台，为您提供导致错误的环境，以及出现问题时应用程序的状态。

现代化您调试 Vue 应用的方式- [开始免费监控](https://lp.logrocket.com/blg/vue-signup)。

## 您是否添加了新的 JS 库来提高性能或构建新特性？如果他们反其道而行之呢？

毫无疑问，前端变得越来越复杂。当您向应用程序添加新的 JavaScript 库和其他依赖项时，您将需要更多的可见性，以确保您的用户不会遇到未知的问题。

LogRocket 是一个前端应用程序监控解决方案，可以让您回放 JavaScript 错误，就像它们发生在您自己的浏览器中一样，这样您就可以更有效地对错误做出反应。

[![LogRocket Dashboard Free Trial Banner](img/e8a0ab42befa3b3b1ae08c1439527dc6.png)](https://lp.logrocket.com/blg/javascript-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/javascript-signup)

[LogRocket](https://lp.logrocket.com/blg/javascript-signup) 可以与任何应用程序完美配合，不管是什么框架，并且有插件可以记录来自 Redux、Vuex 和@ngrx/store 的额外上下文。您可以汇总并报告问题发生时应用程序的状态，而不是猜测问题发生的原因。LogRocket 还可以监控应用的性能，报告客户端 CPU 负载、客户端内存使用等指标。

自信地构建— [开始免费监控](https://lp.logrocket.com/blg/javascript-signup)。