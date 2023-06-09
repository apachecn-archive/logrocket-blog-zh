# 为什么我(最终)从 Apollo Client 转向 urql

> 原文：<https://blog.logrocket.com/why-i-finally-switched-to-urql-from-apollo-client/>

在前端应用程序中使用 GraphQL 就像玩一场与使用 REST 不同的游戏。诸如 [urql](https://formidable.com/open-source/urql/) 、 [Apollo Client](https://www.apollographql.com/) 和 [Relay](https://relay.dev/) 之类的客户端库能够提供与诸如 [Axios](https://github.com/axios/axios) 或 [fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch) 之类的其他库不同的功能。

怎么会这样因为 GraphQL 是一个自以为是的 API 规范，其中服务器和客户端都认同模式格式和查询格式。基于此，它们可以提供多种高级特性，比如缓存数据的实用程序、基于操作自动生成 React 钩子以及乐观突变。

有时候图书馆会太固执己见，提供太多的“魔法”。我使用 Apollo 客户端已经有一段时间了，对它的缓存和本地状态机制感到失望。

这种“膨胀”，加上最近看到的开源社区管理不善，最终让我筋疲力尽。我意识到我需要在别处寻找 GraphQL 客户端库。

## urql 是什么？

输入 urql，这是一个很好的选择。它不是街区里的新成员——它从 2019 年就已经存在了——但我刚刚做出了转变，袖手旁观是我的决定。

大多数术语与 Apollo Client 相同，这使得从 Apollo 到 urql 的转换相当简单。urql 具有大多数相同的功能，但也提供了一些改进，包括更好的文档、更好的配置默认值，以及对离线模式、文件上传、身份验证流程和第一方 Next.js 插件等内容的第一方支持。

当你把 Apollo Client 和 urql 放在一起的时候，你会开始奇怪为什么 Apollo Client 会如此受欢迎。

## 再见阿波罗客户👋，你好 urql

当我写这篇文章的时候， [Apollo 客户端 Github 库](https://github.com/apollographql/apollo-client)的发布数量是 795。相比之下， [urql 有 16 个](https://github.com/FormidableLabs/urql)。"但是问题数量和代码质量没有关系！"你可能会对我说。这是真的，但它给你的感觉和代码气味一样——你知道有些事情不对劲。

更深入地看，您可以看到大量的问题，需要几个月才能修复的错误，以及来自外部贡献者的拉请求似乎永远不会被合并。Apollo 似乎没有专注于构建社区想要的大客户包。

这种行为向我表明，Apollo 使用开源只是为了营销，而不是为了让他们的产品更好。该公司希望你熟悉阿波罗客户端，然后购买他们的产品，在我看来，不是真正的开源软件。这是开放核心商业模式的负面影响之一。

我开始在其他地方寻找一个 GraphQL 客户端，它有一个更快乐、更有凝聚力的社区。当一个工具设计得很好并且具有社区想要的特性时，产生的问题就更少，对拉请求的需求也更少。令人生畏的是 urql 背后的机构，他们关心的是以快速和可维护的方式创建应用程序，而不是试图引导用户使用他们的产品。

## 为什么使用 urql？

对于我来说，urql 是与 Apollo Client 合作了这么久之后的一股新鲜空气。有很多小事情加起来会让开发者体验更好，尤其是对新手来说。这里只是几个。

### urql 中的文档是全面的

拥有优秀的文档是任何开源库的一个关键特性。如果没有好的文档，社区中会有更多关于如何使用它和它如何在内部工作的困惑。我将 urql 的彻底的[文档](https://formidable.com/open-source/urql/docs/)归因于它发行数量如此之低的原因。我只花了几个小时就看完了整个*文档。*

这令人印象深刻，因为它显示了图书馆是多么专注，结构是多么深思熟虑。一些亮点包括[关于 urql 如何工作的架构的一页纸](https://formidable.com/open-source/urql/docs/architecture/)和[将自己与其他 GraphQL 客户端(如 Apollo)进行比较的表格](https://formidable.com/open-source/urql/docs/comparison/)。

### urql 中的插件和包有第一方的支持

当我听说 urql 对其他功能提供一流的支持时，我真正注意到了它，例如[离线模式](https://formidable.com/open-source/urql/docs/graphcache/offline/)、[文件上传](https://formidable.com/open-source/urql/docs/advanced/persistence-and-uploads/#file-uploads)、[认证](https://formidable.com/open-source/urql/docs/api/auth-exchange/)和 [Next.js](https://formidable.com/open-source/urql/docs/advanced/server-side-rendering/#nextjs) 。这些都是我一直认为的 GraphQL 客户端的基本特性，很高兴看到 urql 有第一方对它们的支持。

例如，[urql 身份验证交换包](https://formidable.com/open-source/urql/docs/api/auth-exchange/)只让您实现几个方法，就可以在您的客户机中拥有一个完整的身份验证流程，包括令牌刷新逻辑。你可以在 Apollo 客户端实现所有这些功能，但没有官方文档或软件包。这意味着你要花更多的时间研究社区解决方案、黑客和代码。

```
// All the code needed to support offline mode in urql
import { createClient } from 'urql';
import { offlineExchange } from '@urql/exchange-graphcache';
import { makeDefaultStorage } from '@urql/exchange-graphcache/default-storage';

const storage = makeDefaultStorage({
  idbName: 'apiCache',
  maxAge: 7, // The maximum age of the persisted data in days
});

const cache = offlineExchange({
  schema,
  storage,
  updates: {
    /* ... */
  },
  optimistic: {
    /* ... */
  },
});

const client = createClient({
  url: 'http://localhost:3000/graphql',
  exchanges: [cache]
});

```

同样很棒的是，我在使用 Apollo Client 时不必放弃我喜欢的东西，比如开发工具和 React hooks 生成，因为 urql 有一个[开发工具浏览器扩展](https://formidable.com/open-source/urql/docs/advanced/debugging/#devtools)和[graph QL 代码生成器插件](https://www.graphql-code-generator.com/docs/plugins/typescript-urql)。

### urql 中的缓存简单而有效

有一个共同的开发者格言，缓存失效是编程中最难的事情之一。经过几个小时调试阿波罗客户端正常化缓存，我相信这一点。urql 的缓存缺省值对新手来说是合理的，可以扩展到更高级。

我很感激它没有强迫你默认使用一个规范化的缓存，但是[提供了一个文档缓存](https://formidable.com/open-source/urql/docs/basics/document-caching/)。这通过散列查询及其变量来实现——简单而有效！

学习一个复杂的、完全规范化的缓存存储是如何工作的，只是为了开始使用一个客户端库，这似乎很难。只有提供规范化缓存是我觉得 Apollo 客户端出错的地方。

管理规范化的缓存有一个陡峭的学习曲线，对于许多应用程序来说这是不必要的。很棒的是，urql 提供了一个单独的包，你可以在以后选择。我在其他包中也看到了这种趋势，比如 [React Query](https://react-query.tanstack.com/) 。

> 而绝大多数用户实际上并不需要规范化缓存，甚至并不像他们认为的那样从中受益。–[反应查询单据](https://react-query.tanstack.com/graphql#_top)

```
import { ApolloClient, InMemoryCache } from '@apollo/client';

const client = new ApolloClient({
  uri: "http://localhost:4000/graphql",
  // Normalized cache is required
  cache: new InMemoryCache()
});

import { createClient } from "urql";

// Document cache enabled by default
export const client = createClient({
  url: "http://localhost:4000/graphql",
});

```

### urql 中简化了本地状态

urql 忠于服务器数据，不像 Apollo 客户机那样提供管理本地状态的功能。在我看来，这很好，因为在 React 中管理本地状态的完整库变得越来越不需要了。一开始，混合服务器端状态和本地状态似乎很理想(所有状态都在一个地方)，但是当您需要确定哪些数据是新的，哪些是旧的，以及何时更新数据时，可能会出现问题。

[React Context](https://reactjs.org/docs/context.html) 对于正在进行大量道具演练的情况来说是一个很好的解决方案，这有时是人们使用本地州管理库的主要原因。如果你正在寻找一种管理有状态工作流的方法，我也会推荐 [XState](https://github.com/davidkpiano/xstate) ，有时人们会使用[Redux reducer](https://redux.js.org/recipes/reducing-boilerplate#reducers)来管理有状态工作流。

### 交易所可以理解的违约行为

[交换](https://formidable.com/open-source/urql/docs/architecture/#the-exchanges)类似于 Apollo 客户端中的链接，通过拦截请求来提供扩展客户端功能的方法。urql 的不同之处在于，您甚至可以选择基本的服务，允许您对客户端的行为进行更多的控制和理解。

[开始使用](https://formidable.com/open-source/urql/docs/basics/react-preact/#setting-up-the-client)时，客户端没有要求交换，使用默认列表。根据我的经验，开始时只有少量的交换，随着时间的推移或当我需要时增加更多的交换会使调试更容易。urql 表明它在支持许多不同的用例时非常重视可扩展性。

下面是一个例子，在你习惯了 urql 之后，你可能会用到它:

```
import { createClient, dedupExchange, cacheExchange, fetchExchange } from 'urql';

const client = createClient({
  url: 'http://localhost:4000/graphql',
  exchanges: [
    // deduplicates requests if we send the same queries twice
    dedupExchange,
    // from prior example
    cacheExchange,
    // responsible for sending our requests to our GraphQL API
    fetchExchange,
  ],
});

```

### uqrl 提供了一个 Next.js 支持插件

Next.js 是目前最流行的 React 使用方式之一。集成 Apollo 客户端使用 Next.js SSR 过去一直是个巨大的痛苦。随着每一次升级，[你将不得不寻找例子](https://github.com/vercel/next.js/blob/canary/examples/with-apollo/lib/apolloClient.js#L30)，并且可能需要改变它的工作方式。

没有来自 Apollo 的官方插件，你将不得不继续维护这个集成。如前所述，urql 有一个官方的 Next.js 插件，这使得它很容易集成。

```
// Simple React component integrating with Next.js using the plugin
import React from 'react';
import Head from 'next/head';
import { withUrqlClient } from 'next-urql';

import PokemonList from '../components/pokemon_list';
import PokemonTypes from '../components/pokemon_types';

const Root = () => (
  <div>
    <Head>
      <title>Root</title>
      <link rel="icon" href="/static/favicon.ico" />
    </Head>

    <PokemonList />
    <PokemonTypes />
  </div>
);

export default withUrqlClient(() => ({ url: 'https://graphql-pokemon.now.sh' }))(Root);

```

## 结论

urql 比 Apollo Client 更有优势，因为它有统一的社区、优秀的文档、第一方插件和缓存系统。我特别喜欢他们似乎是如何与社区合作，而不是反对它。

我最近一直在尝试许多 GraphQL 客户端，看看还有什么可以与 Apollo 进行比较，看到 urql 有多棒令人耳目一新。我预见自己会在所有 GraphQL 应用程序中使用它。我希望这能促使您亲自尝试 urql，看看您有什么想法。感谢阅读！

## 使用 [LogRocket](https://lp.logrocket.com/blg/signup) 消除传统错误报告的干扰

[![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/signup)

[LogRocket](https://lp.logrocket.com/blg/signup) 是一个数字体验分析解决方案，它可以保护您免受数百个假阳性错误警报的影响，只针对几个真正重要的项目。LogRocket 会告诉您应用程序中实际影响用户的最具影响力的 bug 和 UX 问题。

然后，使用具有深层技术遥测的会话重放来确切地查看用户看到了什么以及是什么导致了问题，就像你在他们身后看一样。

LogRocket 自动聚合客户端错误、JS 异常、前端性能指标和用户交互。然后 LogRocket 使用机器学习来告诉你哪些问题正在影响大多数用户，并提供你需要修复它的上下文。

关注重要的 bug—[今天就试试 LogRocket】。](https://lp.logrocket.com/blg/signup-issue-free)