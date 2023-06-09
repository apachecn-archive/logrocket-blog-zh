# 将 robots.txt 文件添加到 Next.js 应用程序中

> 原文：<https://blog.logrocket.com/adding-robots-txt-file-next-js-app/>

Next.js 提供了各种各样的优秀特性。无论是它生成页面的方式(静态地或根据服务器请求)还是用[增量静态再生](https://nextjs.org/docs/basic-features/data-fetching/incremental-static-regeneration)更新页面，这个框架都有很多令人兴奋的选项来吸引开发人员。在 Next.js 的所有特性中，它的 SEO 支持是它优于 Create React App 等其他框架的主要优势之一。

React 对于 JavaScript 开发人员来说是一门很棒的语言，但不幸的是，它对于 SEO 来说相当糟糕。原因是 React 是客户端渲染的。具体来说，当用户请求一个页面时，不是服务器返回 HTML，而是服务器提供 JavaScript，浏览器将使用 JavaScript 来构建页面。

因此，SPA 中的初始页面加载通常比使用服务器端呈现的应用程序要长。除此之外，在很长一段时间里，谷歌机器人没有正确抓取 JavaScript。

Next.js 不仅基于 React，还为开发人员提供了服务器端渲染，从而解决了这个问题。这使得开发人员很容易迁移他们的应用程序。

SEO 的一个重要部分是在你的网站上有一个`robots.txt`。在本文中，您将发现什么是`robots.txt`,以及如何将它添加到您的 Next.js 应用程序中，这不是 Next 的现成功能。

## 什么是`robots.txt`文件？

t 文件是一个网络标准文件，它告诉搜索引擎爬虫，比如谷歌机器人，它们能抓取或不能抓取哪些页面。该文件位于您的主机的根目录下，因此可以通过以下 URL 进行访问:`yourdomain.com/robots.txt`。

如前所述，这个文件允许你告诉机器人哪些页面和文件是可抓取的。您可能希望应用程序的某些部分不可访问，比如您的管理页面。

您可以禁止以下 URL:

```
User-agent: nameOfBot
Disallow: /admin/

```

或者允许他们这样:

```
User-agent: *
Allow: /

```

在你的文件的结尾，你也应该为你的站点地图添加一行目的地，如下所示:

```
Sitemap: http://www.yourdomain.com/sitemap.xml

```

*注意，不熟悉网站地图？不要犹豫，去看看谷歌关于这个主题的文档。*

最后，您的`robots.txt`看起来会像这样:

```
User-agent: nameOfBot
Disallow: /admin/

User-agent: *
Allow: /

Sitemap: http://www.yourdomain.com/sitemap.xml

```

## 如何向 Next.js 应用程序添加一个`robots.txt`文件

向您的应用程序添加一个`robots.txt`文件非常容易。每个 Next.js 项目都附带一个名为`public`的文件夹。该文件夹允许您存储静态资产，然后可以从您的域的根目录访问这些资产。因此，通过在您的公共文件夹中存储一个像`dolphin.jpeg`这样的图像，当您的项目被构建时，将可以从 URL `[http://www.yourdomain.com/dolphin.jpeg](http://www.yourdomain.com/dolphin.jpeg)`访问它。同样的技术可以用于你的`robots.txt`文件。

因此，要将一个`robots.txt`文件添加到您的应用程序中，您所要做的就是将它放到应用程序的`public`文件夹中。

## 备选方案:动态生成

有一种方法可以动态生成您的`robots.txt`文件。为此，您可以利用 Next.js 的两个特性: [API 路由](https://nextjs.org/docs/api-routes/introduction)和[重写](https://nextjs.org/docs/api-reference/next.config.js/rewrites)。

Next.js 允许您定义 API 路由。这意味着当一个请求被发送到一个特定的 API 端点时，您可以为您的`robots.txt`文件返回正确的内容。

为此，在您的`pages/api`文件夹中创建一个`robots.js`文件。这将自动创建一条路线。在这个文件中，添加您的处理程序，它将返回您的`robots.txt`内容:

```
export default function handler(req, res) {
    res.send('Robots.txt content goes there'); // Send your `robots.txt content here
}

```

不幸的是，这仅在 URL `/api/robots`可用，如上所述，搜索引擎爬虫将寻找`/robots.txt` url。

谢天谢地，Next.js 提供了一个叫做重写的特性。这允许您将特定的目标路径重新路由到另一个路径。在这个特殊的例子中，您希望将所有对`/robots.txt`的请求重定向到`/api/robots`。

为此，进入您的`next.config.js`并添加重写内容:

```
/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
  async rewrites() {
      return [
          {
              source: '/robots.txt',
              destination: '/api/robots'
          }
      ];
  }
}

module.exports = nextConfig

```

有了这个，每次你访问`/robots.txt`，它会调用`/api/robots`并显示“Robots.txt 内容到了”的消息。

## 如何验证你的`robots.txt`文件？

一旦您将应用程序部署到生产环境中，您就可以通过谷歌搜索提供的[测试器来验证您的`robots.txt`文件。如果您的文件是有效的，您应该会看到一条消息，显示 **0 个错误**。](https://www.google.com/webmasters/tools/robots-testing-tool?siteUrl=https://mariestarck.com/)

![Robots.txt File With No Errors](img/e14567d984a65332c321bb95ae7ff03d.png)

要在生产环境中部署您的应用程序，我强烈推荐使用 [Vercel](https://vercel.com/dashboard) 。这个平台是由 Next.js 的创始人创建的，构建时就考虑到了这个框架。LogRocket 有一个关于[如何在 Vercel](https://blog.logrocket.com/deploy-react-app-for-free-using-vercel/) 上部署 Next.js 应用程序的教程。

## 结论

SEO 对于需要被发现的网站非常重要。为了有一个好的网页排名，网页需要很容易被搜索引擎抓取。Next.js 通过提供内置的 SEO 支持，使 React 开发人员能够轻松实现这一点。这种支持包括能够轻松地向项目添加一个`robots.txt`文件。

在本文中，您了解了什么是`robots.txt`文件，如何将它添加到 Next.js 应用程序中，以及如何在部署应用程序时验证它。

一旦你建立了你的`robots.txt`文件，你还需要一个网站地图。谢天谢地，LogRocket 也有一篇关于如何在 Next.js 中构建站点地图生成器的文章。

## [LogRocket](https://lp.logrocket.com/blg/nextjs-signup) :全面了解生产 Next.js 应用

调试下一个应用程序可能会很困难，尤其是当用户遇到难以重现的问题时。如果您对监视和跟踪状态、自动显示 JavaScript 错误、跟踪缓慢的网络请求和组件加载时间感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/nextjs-signup)

.

[![](img/f300c244a1a1cf916df8b4cb02bec6c6.png)](https://lp.logrocket.com/blg/nextjs-signup)[![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/nextjs-signup)

LogRocket 就像是网络和移动应用的 DVR，记录下你的 Next.js 应用上发生的一切。您可以汇总并报告问题发生时应用程序的状态，而不是猜测问题发生的原因。LogRocket 还可以监控应用程序的性能，报告客户端 CPU 负载、客户端内存使用等指标。

LogRocket Redux 中间件包为您的用户会话增加了一层额外的可见性。LogRocket 记录 Redux 存储中的所有操作和状态。

让您调试 Next.js 应用的方式现代化— [开始免费监控](https://lp.logrocket.com/blg/nextjs-signup)。