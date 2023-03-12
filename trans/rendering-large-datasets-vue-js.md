# 在 Vue.js - LogRocket 博客中渲染大型数据集

> 原文：<https://blog.logrocket.com/rendering-large-datasets-vue-js/>

假设您正在尝试将一个大型数据集实现到一个表中。你问有多大？假设有 100，000 行需要以单个表格格式查看。

如果您使用 Vue.js 中的原生实现，页面将永远无法呈现所有数据。仅仅因为你是一个前端开发人员，并不意味着你可以不用担心性能问题！

即使您正在呈现一个只有 1000 行的表，用户处理这么长的表也不是一件有趣的事情。你会注意到滚动不像通常那样平滑，尤其是在使用鼠标滚轮的时候。

在本文中，我们将讨论在 Vue 中减少渲染时间和提高大型数据集整体性能的几种方法，以及一些内存处理技巧，这些技巧将帮助您的数据密集型站点运行更流畅，并使用更少的 RAM。

## 页码

这是渲染大型数据集最常见的解决方案之一。分页意味着将表分解成单独的页面，因此一次只能呈现一个页面。

![Example of pagination](img/b1977d70a70848a9f509a080fa05d29d.png)

您可以使用`items` prop，它接受项目的 provider 函数从远程数据库获取数据。然后，在 API 请求中使用分页和过滤，在每个请求中只获取大约 100 项所需的数据。

这似乎很简单。但是，如果您需要在一个页面上加载整个表，该怎么办呢？也许你需要一个端点来把所有东西拉回来，并对数据进行一些计算。

在这种情况下，我们可以使用另一种方法来装载我们的表。

## 加载和显示特定区域的数据

有几种方法可以在不分页的情况下加载特定区域的数据:使用 Clusterize.js 和 Vue-virtual-scroller 以及其他相关组件。

### Clusterize.js

js 是一个很容易解决这个问题的 JavaScript 库。它使我们能够加载和显示表格的特定区域。那么它是如何工作的呢？

表格被放在一个可滚动的容器中，这个容器一次显示几行，允许你在整个表格中移动。在 DOM 结构中将只创建表的可见部分。

一旦用户在表格容器中滚动，就会加载新的表格数据。所以数据加载发生在后台，用户不会注意到任何差异。

![Clusterize.js in action](img/9dd499c412c10dcdea58ec9b1dfc840f.png)

在代码中使用 Clusterize.js 非常简单。

加了这个插件之后性能的差别是显著的。然而，如果你需要确定，在他们的网站的顶部有一个令人信服的例子[，它允许你很容易地比较一个常规表和一个 Clusterize.js 优化的表。一定要去他们的游乐场看看，寻找更多的证据。](https://clusterize.js.org/)

![Clusterize.js playground](img/e328baa96996ab94d8d7b40f35a7fbf8.png)

### 虚拟滚动条和虚拟滚动列表

这些流行的组件允许在 Vue 应用中快速滚动大量数据，但也有一个警告；Vue-virtual-scroller 和 [Vue-virtual-scroll-list](https://www.npmjs.com/package/vue-virtual-scroll-list) 不处理动态高度，除非你硬编码它们。如果你想测试一下，这里有一个供 Vue 虚拟滚动条[使用的操场](https://akryum.github.io/vue-virtual-scroller/#/)。

另一个选项是 [Vue-collection-cluster](https://github.com/flowstudio/vue-collection-cluster) 组件，它允许你动态计算高度，但是它落后于大约 50，000 个项目。

然而，即使有这些缺点，这些库中的每一个都允许您构建一个适当的虚拟卷轴。最后，如果您有一个大约 10-100 MB JSON 数据的数据库，那么您的性能就万事俱备了。

如果你的网站是性能优化的，我们可以进入下一部分。

## 内存处理

当处理大型数据集时，您需要担心的最大问题是处理内存使用。如果您允许用户编辑一个数据量很大的表，您将会遇到内存限制，并且您的 web 浏览器将会完全停止运行 JavaScript。

加载这么多数据会给网络浏览器(以及它们可以在内存中保留的节点数量)带来负担，并导致设备的 RAM 使用量飙升。

在智能手机和平板电脑等内存较小的设备上，这个问题会被放大，甚至可能使这些设备瘫痪。这是贪多嚼不烂。

现在，内存处理可以在许多方面得到改进。下面我把它分成五个步骤。

### 1.限制不必要的数据传递

我们可以通过获取没有相关模型的简单对象来简化事情并减少后端的压力。然后，主要结果将只有相关对象的 ID 键。

此外，通过使用 Axios(或类似的库)通过单独的 AJAX 请求获取相关数据(例如，“客户”、“项目”、“位置”)，我们[可以使用 VueX 将它们](https://blog.logrocket.com/how-to-consume-apis-with-vuex-and-axios/#:~:text=Introducing%20Axios%20and%20Vuex)存储在它们自己的列表属性中。这将避免获取完整的模型树。

首先，为每个对象创建 getters，这样我们可以使用相关的模型来获取标签(或者在需要时获取完整的对象),并且我们的后端不需要多次获取相关的数据:

```
projectsById: state => {
   return _.keyBy(state.projects, "id")
},
```

然后，我们可以获取不同的列表，每个列表都有自己的控制器端点，并将结果缓存到 VueX 存储中。请记住，您可以使用`Axios.all([...]).`发送多个请求

### 2.优化数据处理

有必要优化我们处理数据的方式。您可以使用组件对象作为自定义对象和对象列表的数据存储。

优化的列表组件设置如下所示:

```
module.exports = {
   items: [],
   mixins: [sharedUtils],
   data: function() {
       return {
           columns: {
               all: []
   etc...
```

### 3.使其不发生反应

最好将一个项目数组作为非反应性的来处理，但是如果我们希望表对实时过滤器是反应性的，我们如何以非反应性的方式来处理它呢？

每当用户点击一个过滤器按钮或者输入一个字符串过滤器(比如一个名字)，我们需要触发对条目数组的过滤。这个`processFilters`方法遍历非响应项数组并返回`filteredItems`，它们存储在 DataContext 中，因此在转换时它自动变为反应性的:

```
<tr v-for="item in filteredItems"
```

这样，`filteredItems`内的所有项目都保持反应性，但当它们被过滤掉时也会失去反应性，节省了大量内存。

然而，这里的问题是我们不能在模板中直接使用 DataContext 中的项目。

所以你不能用这个:

```
<div v-if="items.length > 0 && everythingElseIsReady">
```

相反，您必须将 items 数组的长度存储到一个单独的数据属性中。

### 4.有一个隐藏的容器

对于非反应性主数据数组，直接对该主数组中的项目进行的修改不会触发对 UI 或子组件的任何更改。

为了解决这个问题，我们需要一个单独的容器来保存来自后端的所有结果，这个容器有一个较小的(经过过滤的)表示数组。在这种情况下，我们使用良好的 REST 架构来处理非反应式数据存储。

### 5.区分实例化对象和引用对象

有时，当针对不同的主记录多次表示同一个子对象时，您甚至没有意识到，您可能正在创建不引用其他对象的对象。

![Graphic example of a reference object](img/8e798c800e22c94d7e932a303d3750f0.png)

例如，假设您有一个包含一个`university-object`的`student-object`。现在，多个学生上同一所大学。但是当你从后端取 JSON 数据的时候，你确定那些重复的`university-object`是同一所大学吗？或者它们是同一个对象的多个表示？

当然，您可以将`university`作为属性传递给`student-object`。同时，如果你不确定你是引用一个共享的`university-object`还是使用几十个相同子对象的实例，你可以简单地在你的`student-list`组件中引用。

一个学生将包含一个`university-id`，所以用一个单独的 REST 方法(例如`getUniversities()`)获取一个大学列表，并在 UI 层进行配对。这样，您只有一个大学列表，您可以从该列表中解析大学并将其注入到一个人，从而只引用一个。

基本上，您需要管理您的主记录(例如，`persons`或`products`)与相关记录(子对象或关系对象)。

请记住，如果子对象是反应性的，则不能使用该方法。如果它需要是可编辑的，那么你需要确保你没有使用引用的对象！

## 结论

在本文中，我们简要讨论了分页和使用 Clusterize.js 优化网站性能。然后，我们通过五个简单的步骤进入内存处理:限制不必要的数据传递，优化数据处理，使其非反应性，拥有一个隐藏的容器，以及区分对象实例和引用实例。

总的来说，Vue 在处理大型数据集方面相当有效。但是像所有事情一样，查看它是否适合您的需求的最佳方式是创建您需要的组件类型、过滤器和排序，然后用大量(种子或测试)数据加载它们，以检查它们的性能是否足以满足您的需求。

## 像用户一样体验您的 Vue 应用

调试 Vue.js 应用程序可能会很困难，尤其是当用户会话期间有几十个(如果不是几百个)突变时。如果您对监视和跟踪生产中所有用户的 Vue 突变感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/vue-signup)

.

[![LogRocket Dashboard Free Trial Banner](img/0d269845910c723dd7df26adab9289cb.png)](https://lp.logrocket.com/blg/vue-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/vue-signup)

LogRocket 就像是网络和移动应用程序的 DVR，记录你的 Vue 应用程序中发生的一切，包括网络请求、JavaScript 错误、性能问题等等。您可以汇总并报告问题发生时应用程序的状态，而不是猜测问题发生的原因。

LogRocket Vuex 插件将 Vuex 突变记录到 LogRocket 控制台，为您提供导致错误的环境，以及出现问题时应用程序的状态。

现代化您调试 Vue 应用的方式- [开始免费监控](https://lp.logrocket.com/blg/vue-signup)。