# P2P 协议在现代浏览器中的接受状况

> 原文：<https://blog.logrocket.com/p2p-protocol-acceptance-state-modern-browsers/>

Web 3.0 是网络空间技术的总称，它提供分散的应用程序，不依赖于中央服务器，提高了用户的自主性和隐私性。虽然许多开发人员专注于使用称为区块链的分布式分类帐来管理数据的 Web 3.0 应用程序，但其他应用程序使用 torrents 或浏览器存储等方法来提供去中心化。

在本文中，我们将回顾这一领域的一些不同技术，讨论它们的现状和未来计划。我们开始吧！

## 流行的网络协议

### 洋葱路由器

洋葱路由是 2002 年发明的洋葱路由器的关键技术。TOR
通过层层服务器和加密技术来掩饰互联网流量的来源。

有了 TOR 浏览器，你可以创建`.Onion`域名，为你的网站提供匿名来源，这被许多人称为深度网络。deep web 包含只能通过 TOR 浏览器上的 onion 协议访问的站点。

TOR 和 Brave browse 都支持洋葱协议。TOR 的开发者还为 iOS 开发了一款名为[洋葱浏览器](https://onionbrowser.com)的浏览器。

### 双子星座

[Gemini 协议](https://gemini.circumlunar.space)创建于 2019 年，是一种互联网协议，旨在介于 Gopher 协议和 HTTP 之间。 [Gemini 提供了改进](https://gemini.circumlunar.space/docs/faq.gmi),比如基于域的虚拟主机、防止断开链接的重定向，以及使用 MIME 类型识别二进制内容。

Gemini 使用一种叫做 gemtext 的轻量级标记语言[来发布网站，这种语言删除了 HTML 的许多高级媒体功能，保持页面较小，以便快速加载。gemtext 看起来很像 markdown，因此带有项目符号列表的标题将使用以下结构:](https://gemini.circumlunar.space/docs/gemtext.gmi)

```
# This is my Heading
* item
* item
* item

```

一般来说，页面是基于文本的，颜色会发生一些变化，这意味着内容加载速度很快，并且能够响应视口。要添加外部媒体，只需使用链接即可。如果内容移动到另一个位置，您可以轻松地创建重定向。

您可以在主页上访问 Gemini 客户列表。在撰写本文时，还没有主流浏览器支持 Gemini，但是有一些传统的 HTTP 站点可以作为代理从任何现代浏览器访问 Gemini 内容，比如 [Mozz](https://portal.mozz.us/gemini/gemini.circumlunar.space/) 。

### IPFS 吗

[星际文件系统](https://ipfs.io)或 IPFS，是 2015 年创建的互联网协议，旨在分散网络上的内容位置。内容不是托管在静态服务器上，而是通过对等点分发，对等点以类似种子的方式播种内容。

只要一个 [IPFS 节点的内容在特定的 IPFS 内容 ID](https://blog.logrocket.com/using-node-js-http-proxy-ipfs-content/) 下，该内容将保持可访问。内容是不可变的，所以任何更新都会产生一个新的内容 ID。在撰写本文时，勇敢者和 [Opera 浏览器](https://www.opera.com)都有对 IPFS 的原生支持，你可以下载扩展来为 Chrome 和 Firefox 添加 IPFS 支持。此外，像 [OrbitDB](https://github.com/orbitdb/orbit-db) 和 [AvionDB](https://github.com/dappkit/aviondb) 这样的数据库都是建立在 IPFS 之上的。

## 基于区块链的协议

### 大量

[Stacks](https://www.stacks.co/what-is-stacks) 是一个基于区块链的协议，通过 blockstack 网络上的比特币交易来结算交易。构建在 blockstack 网络上的应用不能被取下。此外，数据由用户控制，可以在应用程序之间移植。应用程序开发者也可以发行自己的代币，保证比特币网络的安全性。

Stacks 应用的一个流行例子是博客应用 [Sigle](https://www.sigle.io/) 。首先，你可以从 Stacks 网络获得一个密钥，然后你就可以用这个密钥登录 Sigle 或其他应用程序。数据将被绑定到密钥而不是应用程序。

你可以用标准的 HTTP 网站来构建这些应用的前端，所以你不需要任何独特的浏览器。应用程序本身处理数据的创建和获取。网络上的数据被部署到区块链，然后与标准 web 界面配对。

在撰写本文时，另一个区块链网络[algrand](https://www.algorand.com/ecosystem/use-cases/blockstack)正在与 Stacks 合作开发[Clarity 语言](https://clarity-lang.org)，用于在 Stacks 上构建应用。

### BigchainDB

BigchainDB 是一个数据库，任何 web、桌面或移动应用程序都可以使用它在 Bigchain 网络上分发数据。BigchainDB 上的数据被组织成可以创建或转换的资产，而不是记录或集合。

资产对于旨在创建所有权记录的应用程序特别有用。想象你创造了一个在用户之间买卖商品的资产。当用户注册他们想要出售的商品时，他们首先会在自己的大链网络上注册该资产。当用户出售资产时，应用程序将创建一个`TRANSFER`事务，将资产转移到链上的另一个用户或地址。

创建数据的应用程序并不拥有数据，而是将数据分布在区块链中，从而保持用户资产的完整性。这些资产在您的应用程序中使用的数据库中工作，因此您不需要任何特殊的浏览器。应用程序的用户界面可以是标准的 web、桌面或移动应用程序。

## 其他分散技术

### 活动 Pub

ActivityPub 是一个用于创建应用服务器的规范，这些应用服务器可以相互通信并创建联合应用。实现 ActivityPub 规范的应用程序可以与实现该规范的其他应用程序进行通信。这方面的一个例子是微博网络，[乳齿象](https://blog.joinmastodon.org)。

当你注册成为一个乳齿象用户时，你注册了许多独立运行的乳齿象服务器中的一个。尽管每个服务器都独立运行并遵循自己的规则，但只要您所在的服务器允许您与该服务器联合，您就可以关注其他服务器上的用户。服务器可以阻止与某些服务器的联合或通信。

ActivityPub 的服务器规范保护您的隐私。例如，如果你想拥有你的信息，你可以启动你自己的个人服务器并与更大的网络互动。

在其他网络中，您必须要么遵守规则，要么完全失去对网络的访问，但是在 ActivityPub 上，在这种意义上不存在全有或全无的把关。因为每台服务器都是独立运行的，所以您一定会找到一台允许或限制您所需规格的服务器。

### GUNDB

GUNDB 是一个真正赋予用户数据所有权的数据库。GUN 数据库通过 localStorage API 在用户的计算机上存储信息。例如，想象一个社交网络或聊天应用程序，当你注销时，它会使你的数据无法访问，即使你没有登录，它也会优先考虑你的隐私。

localStorage API 真正为用户提供了对隐私和可伸缩性的更大控制。每个用户持有自己的数据，数据通过 WebRTC 传递。例如，想想 Zoom 和其他视频通话应用程序如何在用户之间直接传递数据，从而为查看数据提供巨大的速度。

## 结论

有如此多的协议可用于分散网络和保护隐私和自主权，很难跟踪所有的协议。在本教程中，我们讨论了一些最流行的 Web 3.0 协议，特别关注使用基于区块链的方法的协议。

如果你正在寻找一个好的资源来跟踪所有这些技术，一定要看看下面的 [GitHub 要点](https://github.com/gdamdam/awesome-decentralized-web)。还有，如果你想连接，一定要[跟着我上乳齿象](https://indieweb.social/@alexmerced)。感谢阅读和快乐编码！

## 加入像 Bitso 和 Coinsquare 这样的组织，他们使用 LogRocket 主动监控他们的 Web3 应用

影响用户在您的应用中激活和交易的能力的客户端问题会极大地影响您的底线。如果您对监控 UX 问题、自动显示 JavaScript 错误、跟踪缓慢的网络请求和组件加载时间感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/web3-signup)

.

[![LogRocket Dashboard Free Trial Banner](img/dacb06c713aec161ffeaffae5bd048cd.png)](https://lp.logrocket.com/blg/web3-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/web3-signup)

LogRocket 就像是网络和移动应用的 DVR，记录你的网络应用或网站上发生的一切。您可以汇总和报告关键的前端性能指标，重放用户会话和应用程序状态，记录网络请求，并自动显示所有错误，而不是猜测问题发生的原因。

现代化您调试 web 和移动应用的方式— [开始免费监控](https://lp.logrocket.com/blg/web3-signup)。