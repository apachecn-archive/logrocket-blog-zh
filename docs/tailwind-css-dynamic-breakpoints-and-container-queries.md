# 顺风 CSS:使用动态断点和容器查询

> 原文：<https://blog.logrocket.com/tailwind-css-dynamic-breakpoints-and-container-queries/>

样式是网页设计的一个重要组成部分，影响着用户对你的网站的观感。正确的样式可以提高网站的视觉吸引力、可用性和专业外观。它还可以支持建立一个网站的总体基调和个性。

除了美观，造型还有实用的优势。例如，如果你使用正确的字体大小和样式来提高可读性和易读性，用户会发现在你的网站上消费内容更简单。

然而，从头开始创建这些样式非常耗时，需要全面的规划。开发人员经常使用预建的库来简化网页样式，使其更符合标准，而不是从头开始每个项目。

这些预先计划好的库被称为 CSS 框架，其中之一就是 [Tailwind CSS](https://tailwindcss.com) 。在本教程中，我们将探索使用动态断点，多配置，和顺风 CSS 的容器查询。我们开始吧！

## 背景:顺风 CSS 简介

Tailwind CSS 是一个[实用优先的 CSS 框架，用于快速创建定制用户界面](https://blog.logrocket.com/comparing-tailwind-css-bootstrap-time-ditch-ui-kits/)。Tailwind CSS 易于使用且高度可定制，它提供了一个低级样式解决方案，允许开发人员轻松地从头开始创建自定义设计，而无需编写过多的 CSS。

作为其主要优势之一，Tailwind CSS 提供了一大组实用程序类，您可以使用它们来设计 HTML 元素的样式。这些类使用简单，很容易以各种方式组合起来，以实现特定元素所需的样式。

尽管相对较新，但 Tailwind CSS 因其小巧、简单、灵活和易于使用而在开发人员中广受欢迎。

## 顺风 CSS 中有哪些动态断点？

在 CSS 中，动态断点指的是允许网页调整其布局的任意视口大小。动态断点对于创建在各种设备和屏幕尺寸上外观和功能良好的响应式设计非常有用。

在 Tailwind CSS v3.2 中，有两种方法可以创建动态断点，`max-*`变体和`min-*`变体。

### 使用`max-*`变体

新的`max-*`变体允许您基于您配置的断点应用[最大宽度媒体查询](https://v2.tailwindcss.com/docs/max-width):

```
 <h1 class="text-black max-lg:text-red-600">
  <!-- Applies`text-red-600` when screenwidth is <= 1023px  -->
 </h1>

```

使用上面的代码，只要屏幕宽度在`0`到`1023px`的范围内，我们的标题文本就会变成红色。

### 使用`min-*`变体

与`max-*`变体不同，`min-*`变体允许您基于任意值应用[最小宽度媒体查询](https://tailwindcss.com/docs/min-width):

```
   <h1 class="text-black min-[712px]:text-red-600">
  <!-- Applies`text-red-600` when screenwidth is >=712px  -->
 </h1&gt;

```

为了确保所有这些动态断点在浏览器中为您提供预期的行为，在您的配置文件中添加一个`screens`对象，如下所示:

```
 // tailwind.config.js
module.exports = {
  theme: {
    screens: {
      sm: "640px",
      md: "768px",
      lg: "1024px",
      xl: "1280px",
      "2xl": "1536px",
    },
  },
};

```

[T](https://tailwindcss.com/blog/tailwindcss-v3-2#max-width-and-dynamic-breakpoints) [ailwind CSS v3.2.4 文档](https://tailwindcss.com/blog/tailwindcss-v3-2#max-width-and-dynamic-breakpoints)推荐使用最小宽度断点。

## 顺风 CSS 中的容器查询是什么？

容器查询是一个提议的 CSS 特性，它允许您基于父容器的大小而不是视口的大小对元素应用样式。尽管容器查询不是官方 CSS 规范的一部分，但是由于各种各样的 polyfills 和库，您可以在项目中使用它们。

Tailwind CSS v3.2.4 发布了一个 [`@tailwindcss/container-queries`](https://tailwindcss.com/blog/tailwindcss-v3-2#container-queries) 插件，它使用新的`@`语法将容器查询支持添加到框架中，以区别于普通的媒体查询。

首先，通过运行以下命令安装插件:

```
#npm
npm install @tailwindcss/container-queries

#yarn 
yarn add @tailwindcss/container-queries

```

然后，将插件添加到您的`tailwind.config.js`文件中:

```
/** @type {import('tailwindcss').Config} */
module.exports = {
  ...
  plugins: [
    require('@tailwindcss/container-queries'),
    // ...
  ],
}

```

接下来，我们使用`@container`类将一个元素标记为容器，并使用容器变量基于该容器的大小应用样式:

```
   <div class="@container">
        <div class="... block @lg:flex">
        </div>
    </div>

```

在上面的代码中，每当容器大于`32rem`时，我们将`div`的显示改为 flex。现成的，Tailwind CSS 包括以下一组容器大小:

| 名字 | 价值 |
| --- | --- |
| `xs` | T2`20rem` |
| `sm` | T2`24rem` |
| T2`md` | T2`28rem` |
| T2`lg` | T2`32rem` |
| T2`xl` | T2`36rem` |
| T2`2xl` | T2`42rem` |
| T2`3xl` | T2`48rem` |
| T2`4xl` | T2`56rem` |
| `5xl` | T2`64rem` |
| `6xl` | T2`72rem` |
| `7xl` | T2`80rem` |

但是，您可以通过在配置文件中添加一个`containers`对象来配置可用的值，如下所示:

```
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      containers: {
        2xs: '16rem',
        // etc...
      },
    },
  },
}

```

除了使用默认提供的容器大小之一，您还可以使用自己选择的任意值:

```
 <div class="@container">
        <div class="... block @[16rem]:flex">
        </div>
    </div&gt;

```

## 在顺风 CSS 中使用多个配置文件

Tailwind CSS 是在考虑定制的基础上从头开始设计的。有了它的配置文件，很容易使这些配置更适合我们的用例。我们可能希望应用程序的不同部分有不同的配置文件；一个典型的例子是为应用程序面向客户的部分使用一个配置文件，为应用程序的管理部分使用另一个配置文件。

在 Tailwind v3.2.4 之前，开发人员有不同的变通方法在 Tailwind CSS 中使用多个配置文件。这些变通办法的一些版本涉及使用`presets`和`extend`选项。然而，这些变通办法并没有解决问题，因为它们将多个配置组合成一个并生成一个 CSS 文件，从而违背了拥有多个样式表的目的。

### 使用`@config`指令

Tailwind CSS v3.2.4 包含了一个新的 [`@config`](https://www.npmjs.com/package/config) 指令，让你指定哪个 Tailwind CSS 配置用于那个文件。

让我们假设我们有一个名为`tailwind.dashboard.config.js`的顺风 CSS 配置文件和一个我们想在其中使用该配置文件的 CSS 文件。我们可以指定在 CSS 文件中使用什么配置文件，如下所示:

```
@config "./tailwind.dashboard.config.js"
@tailwind base;
@tailwind components;
@tailwind utilities;

```

## 结论

动态断点、多配置和容器查询是强大的顺风 CSS 功能，可以显著提高设计系统的灵活性和可维护性。

借助动态断点，您可以轻松创建适应不同屏幕尺寸和设备的响应式设计。多配置使您能够模块化和跨项目重用配置，而容器查询允许您基于容器元素的尺寸而不是视口大小来应用样式。

通过组合这些功能，您可以创建更加健壮和灵活的样式，以处理更广泛的设备和屏幕尺寸。我希望你喜欢这篇文章。有问题一定要留下评论！

## 你的前端是否占用了用户的 CPU？

随着 web 前端变得越来越复杂，资源贪婪的特性对浏览器的要求越来越高。如果您对监控和跟踪生产环境中所有用户的客户端 CPU 使用、内存使用等感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/css-signup)

.

[![LogRocket Dashboard Free Trial Banner](img/dacb06c713aec161ffeaffae5bd048cd.png)](https://lp.logrocket.com/blg/css-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/css-signup)

LogRocket 就像是网络和移动应用的 DVR，记录你的网络应用或网站上发生的一切。您可以汇总和报告关键的前端性能指标，重放用户会话和应用程序状态，记录网络请求，并自动显示所有错误，而不是猜测问题发生的原因。

现代化您调试 web 和移动应用的方式— [开始免费监控](https://lp.logrocket.com/blg/css-signup)。