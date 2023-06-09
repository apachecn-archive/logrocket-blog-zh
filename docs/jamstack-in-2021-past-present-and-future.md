# 2021 年的 Jamstack:过去、现在和未来

> 原文：<https://blog.logrocket.com/jamstack-in-2021-past-present-and-future/>

科技世界中有许多术语看起来比实际情况更复杂。这当然可以说是 Jamstack。最近，越来越多的人开始使用它，但许多人对它到底是什么仍有复杂的看法。

## 什么是 Jamstack？

开发栈只是用来构建或运行应用程序的底层工具、框架和库。想到的是均值栈(MongoDB、Express.js、Angular 和 Node.js)和 MERN 栈(MongoDB、Express.js、React 和 Node.js)。

Jamstack 是一种软件架构，旨在使 web 更快、更安全、更可伸缩。它利用了以下基础组件:

*   **J**–JavaScript
*   **A**–原料药
*   **M** – markup

Jamstack 建立在这样一个概念上，即网站可以静态服务，但也可以通过底层组件(主要是 JavaScript 和 API)提供动态内容。

Jamstack 项目的例子从一个简单的 HTML 网页(可能没有部署到 web 主机的 JavaScript)到一个由 Vue.js 构建的完整的博客(由像 [Ghost](https://ghost.org) 这样的无头 CMS 提供支持)。这两者是截然不同的实现，但它们仍然基于相同的原理。

### 这些不同的例子是如何联系在一起的？

在使用 Jamstack 构建 web 应用程序时，我们遵循的方法是将身份验证或支付处理等任务外包给 API，重点主要放在应用程序的前端构建上。

因此，Jamstack 是前端开发人员的梦想。如果你现在想建一个博客，你不必担心后端——管理数据库来存储文章、图片等。使用 Jamstack 可用的 API，例如 headless CMS，您可以专注于您擅长的前端。

但是，需要注意的是，您不必将项目中的所有三个组件都视为 Jamstack。不需要用 JavaScript 你不需要使用 API。

我们构建的许多静态项目都属于这一类。作为静态应用程序构建和部署的 Vue、Angular 或 React 应用程序是 Jamstack。那个简单的 HTML 文件没有 JavaScript，实际上不做任何动态的事情？是的，这也是一个 Jamstack 应用程序。

### JAMstack 还是 Jamstack？

你可能已经注意到上面提到的其他栈(MEAN 和 MERN)都是大写的，事实上，Jamstack 也曾经被风格化为 JAMstack。但是因为 JAMstack 现在包含的不仅仅是它最初的缩写 JAM 所代表的，它已经成为一个术语，仅仅代表静态服务的 web 应用程序的整个概念。

所以如果你关心这个单词的正确写法，答案是 Jamstack。

## Jamstack 的过去、现在和未来

过去，我们通过让应用程序向 web 服务器发送请求来构建应用程序，然后这些服务器为每个站点访问构建 HTML。这是一种响应速度较慢的 web 体验，因为每个页面请求都要做大量的工作。

我们继续使用 AJAX (Asynchronous JavaScript 和 XML，在 XML 大部分被 JSON 取代后，它也被重新命名为 AJAX)之类的工具来构建异步 web 体验，以构建交互式 web 应用程序，减少我们必须向服务器发出的页面加载和请求的数量。

单页应用程序出现了，给网站带来了更快的转变，有了原生应用程序的感觉。许多工具和框架被开发出来，使开发者更容易利用这一点。然而，一个主要的缺点是，水疗中心很难进行搜索引擎优化(SEO ),因为单个页面没有自己的 URL。

SPAs 的这个主要缺点导致了人们使用像 Jekyll 这样的静态站点生成器(SSG)来构建更多静态站点的趋势，Jekyll 也看到了越来越多的采用，因为它支持 GitHub Pages，GitHub Pages 是一个 GitHub 平台，允许用户从他们的存储库中托管网站。

SSG 和 spa 之间的主要区别在于，在构建 SSG 时已经完成了繁重的工作，并且生成了单独的页面。相比之下，spa 必须根据请求创建页面和视图。这也意味着 SSGs 更好的 SEO。由于繁重的工作只在后台进行一次，因此即使流量增加，SSG 也能保持高性能。

静态站点生成器也属于 Jamstack。你可以选择在 [Netlify](https://netlify.com) 或者像 [Cloudflare](https://cloudflare.com) 这样的 CDN 上托管它们。cdn 有助于降低托管 web 应用的成本，同时提高速度、可扩展性和安全性。它们还使 Jamstack 更快，因为它们倾向于从地理上更靠近网站访问者的服务器提供内容，这意味着更快的加载时间和更好的整体体验。

今天，Jamstack 网站得到了许多流行框架的支持和支持，您可以使用这些框架来构建下一个 web 应用程序。其中一些包括 React (Gatsby，Next.js)，Vue (Gridsome，Nuxt.js)，Hugo，还有[一大堆](https://jamstack.org/generators/)。有了这些工具，您很可能已经熟悉了构建 Jamstack 站点所需的东西。

### 今天是什么造就了一个网站 Jamstack？

根据标记和服务方式，可以将网站视为 Jamstack。如果您的应用程序在请求时没有在服务器上编译和呈现 HTML 文件，那么您的网站就是 Jamstack。再说一次，你不必使用 JavaScript，甚至不必调用 API 来使你的网站被认为是 Jamstack。

### Jamstack 还有意义吗？

我还在写这个，所以可以肯定。我认为 Jamstack 最大的好处是，它使得任何类型的开发人员，尤其是前端开发人员，都可以轻松地构建解决方案，而不必太担心后端活动。它非常适合快速构建[最小可行产品(MVP)](https://blog.logrocket.com/product-management/what-is-minimum-viable-product-mvp-how-to-define/)、附带项目、投资组合等等。

即使在构建自己的 API 时，也可以构建一个使用该 API 的 Vue 项目，从而消除 API 和前端之间的依赖性——这对于前端开发人员或团队来说更容易处理和维护。

## Jamstack 的下一步是什么？

Jamstack 还有许多令人兴奋的可能性，它是一个正在积极发展的社区，您也可以成为其中的一员。如果您想了解更多信息，成为社区的一部分，并交流想法，您可以查看 [Jamstack 社区页面](https://jamstack.org/community/)以查找您附近的活动和社区。

如果你想了解更多关于 Jamstack 的惊人之处，你可以在它的官方网页上阅读更多关于它的好处——安全性、规模、性能、可维护性、可移植性和开发者体验。互联网上还有很多其他资源展示了如何使用多种框架和工具构建 Jamstack 网站。一定要去看看。

## 额外资源