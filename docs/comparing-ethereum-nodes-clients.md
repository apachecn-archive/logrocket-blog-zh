# 比较以太坊节点和客户端

> 原文：<https://blog.logrocket.com/comparing-ethereum-nodes-clients/>

多个独立的网络符合以太坊协议。这些网络本质上都是自己的以太坊环境，底层基础设施由互连的计算机组成。这些连接的计算机被称为节点，并且在每台计算机上运行的软件应用被称为客户端。运行在节点上的以太坊客户端有助于维护协议的单一规范状态，从而维护其固有的安全性。在一个节点上运行以太坊客户端使我们能够安全可靠地使用协议，同时也为以太坊生态系统做出贡献。

在本文中，我们将研究三种以太坊节点之间的差异。我们还将了解各种开源以太坊客户端的不同特性、支持、编程语言和许可证。

## 以太坊节点是什么？

以太坊节点是在以太坊网络上存储、验证和交换数据的计算机或服务器。以太坊客户端可以运行三种类型的节点:

完整节点、轻型节点和归档节点的不同之处在于它们同步的区块链数据量以及它们存储的以太坊最新状态信息的部分。这三种节点类型中的每一种都采用不同的同步方法来检索和解释数据。让我们来看看。

### 以太坊完整节点

顾名思义，以太坊完整节点存储区块链数据的完整副本。它们也有助于以太网内的数据分发和块验证。完整节点持续同步，以便与以太坊区块链保持同步。以太坊网络上运行的智能合约可以与全节点交互。不幸的是，这种类型的节点运行起来非常昂贵并且需要大量资源。

### 以太坊光节点

以太坊光节点的存储数据量很少。它们只存储头链数据，如时间戳和前一个块的散列。在需要额外信息的地方，光节点将查询区块链。通过这种方式，可以存储更少的信息，并在需要时根据请求检索附加信息。轻型节点可以对照块头中的状态根来验证存储数据的有效性。这种类型的节点对于嵌入式工具或智能手机等低容量设备来说可能是有利的，因为它们不承担密集的数据存储和写入活动。

### 以太坊存档节点

以太坊存档节点存储所有数据并建立历史区块链国家的存档。顾名思义，这些节点相当于区块链数据的一种存档。归档节点将保存以前的数据，即使在客户端完成同步之后。另一方面，全节点和轻节点将“修剪”历史区块链数据。这意味着他们将能够重建，但不能保留历史数据。存档节点可以保存千兆字节的历史数据，这使它们对普通用户来说不那么有吸引力，但对服务提供商来说却很有价值，例如块探索者、钱包供应商和链分析。

## 以太坊客户端是什么？

客户端是下载到计算机上的任何种类的软件，允许用户与服务器提供的另一种类型的软件或服务进行交互。以太坊客户端是以太坊的实现，它验证每个块中的所有事务，确保网络安全和数据正确。

在以太坊社区中，有多个开源执行客户端(以前称为 Eth1 客户端或以太坊客户端)，每个客户端都是由不同的团队使用不同的编程语言创建的。

每个以太坊客户端都有一套独特的功能和优势。由于它们的多样性，实现可以适应不同的用户群。选择客户端时，要考虑它的特性、支持、编程语言和许可。

让我们仔细看看以下开源以太坊客户端:

### 去以太坊

Go Ethereum(或 [Geth](https://geth.ethereum.org/) )是官方对以太坊协议的 Golang 实现。Geth 是目前最流行的以太坊客户端。它拥有最大的用户群，并为消费者和开发者提供了大量用 Go 编写的工具。Geth 是开放源码的，并在 GNU LGPL v3 下获得许可。

*   **Use** : Geth 可以用来创建一个完整节点、轻量节点或者归档节点
*   **安装** : Geth 可以使用一个针对本地机器的首选包管理器来安装，可以作为 Docker 容器运行，也可以从头开始构建。关于如何安装 Geth 的详细信息，请参见[官方文档](https://geth.ethereum.org/docs/install-and-build/installing-geth)

### 虚空思维

[Nethermind](https://nethermind.io/) 是一个用 C#编写的以太坊实现。NET 技术堆栈编程语言，运行在所有主要平台上，包括 ARM。Nethermind 简化了与当前基础架构的集成，同时保持了稳定性、可靠性、数据完整性和安全性。Nethermind 提供了关于如何设置以太坊节点应用程序的详细文档。

*   **用途**:nether mind 客户端可以用来创建私有以太坊网络或者去中心化的应用。它无缝地提供了以下功能:
    *   优化的虚拟机
    *   网络和特性，如 Prometheus/Grafana 仪表板、seq 企业日志支持、分析插件和 JSON RPC 跟踪
    *   状态访问
*   **安装**:查看[官方文档](https://docs.nethermind.io/nethermind/first-steps-with-nethermind/getting-started)了解如何安装启动虚空思维

### 起源

[Erigon](https://github.com/ledgerwatch/erigon) 是一款 Go Ethereum fork，专注于速度和节省磁盘空间。Erigon 是一个完全重新架构的以太坊实现，用 Go 编写；然而，未来的计划要求将它移植到其他语言。Erigon 的开发是为了提供更快、更模块化、更优化的以太坊实现。该客户端可以在不到三天的时间内完成完整的归档节点同步，存储空间不到 2TB。

*   **使用** : Erigon 提供了几个特性，使其成为设置节点应用程序的好选择:
    *   通过使用键值数据库实现高效的状态存储
    *   在将临时文件中的数据插入中央数据库以执行特定任务之前，通过预处理临时文件中的数据来实现快速初始同步。这减少了写入放大，并使数据库插入速度提高了几个数量级
    *   独立的 JSON-RPC 守护进程。这个守护进程可以连接到本地和远程数据库。对于只读调用，这个 RPC 守护进程不需要与主 Erigon 二进制文件运行在同一个系统上，它甚至可以从数据库快照运行
*   **安装**:见 [GitHub](https://github.com/ledgerwatch/erigon) 关于如何安装和运行 Erigon 的信息

### 贝苏

[Hyperledger Besu](https://besu.hyperledger.org/en/stable/) 是用 Java 编写的开源以太坊客户端，在 Apache 2.0 许可下发布。它支持以太坊公共网络，以及私有和测试网络，如 Rinkeby、Ropsten 和 gorli。Besu 实施基于工作证明(Ethash)和权威证明(IBFT 2.0、Clique 和 QBFT)的共识程序。

*   **使用**:对于 Truffle、Remix 和 web3j 等工具， [Besu 客户端](https://besu.hyperledger.org/en/stable/)对智能合约和 dApp 开发、部署和运营用例有很好的支持。客户端支持一些 JSON-RPC API 方法，如 eth、net、web3、debug 和 miner。对于密钥管理支持，您可以使用 EthSigner 和 Besu 来签署事务和访问您的密钥库
*   **安装** : Besu 可以使用自制软件或 Java JDK 安装，可以作为 Docker 容器运行，也可以从头开始构建。参见[官方文档](https://besu.hyperledger.org/en/stable/HowTo/Get-Started/Installation-Options/Options/)了解如何安装和运行 Besu

### 开放以太坊

OpenEthereum 是一个基于 CLI 的高级以太坊客户端，速度快，功能丰富。这是写在铁锈和 GPLv3 许可。OpenEthereum 旨在为需要快速同步和增加正常运行时间的快速、可靠服务提供必要的基础设施。这个客户端提供了一个干净的模块化代码库。遗憾的是，OpenEthereum 已被弃用，不再受支持。请谨慎使用它，如果可能，请选择不同的客户端实现。

*   **使用** : OpenEthereum 易于定制开发，支持与现有服务或产品的简单集成。尽管速度很快，但 OpenEthereum 只需要很少的内存和存储空间
*   **安装**:关于设置 OpenEthereum 的信息，请参见[官方文档](https://openethereum.github.io/Setup)

## 结论

在本文中，我们探讨了三种不同类型的以太坊节点应用程序:完整、轻量和归档。节点可能需要大量时间来同步区块链数据，并且它们需要持续维护，具体取决于您要查找的数据类型。例如，如果您需要来自区块链的较旧数据，您将需要使用归档节点，即使它速度较慢。

我们还调查了五个以太坊客户。每个客户端都提供不同的功能，因此请选择最适合您需求的一个。希望您发现这些信息对下次需要旋转节点应用程序有所帮助。

## 加入像 Bitso 和 Coinsquare 这样的组织，他们使用 LogRocket 主动监控他们的 Web3 应用

影响用户在您的应用中激活和交易的能力的客户端问题会极大地影响您的底线。如果您对监控 UX 问题、自动显示 JavaScript 错误、跟踪缓慢的网络请求和组件加载时间感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/web3-signup)

.

[![LogRocket Dashboard Free Trial Banner](img/dacb06c713aec161ffeaffae5bd048cd.png)](https://lp.logrocket.com/blg/web3-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/web3-signup)

LogRocket 就像是网络和移动应用的 DVR，记录你的网络应用或网站上发生的一切。您可以汇总和报告关键的前端性能指标，重放用户会话和应用程序状态，记录网络请求，并自动显示所有错误，而不是猜测问题发生的原因。

现代化您调试 web 和移动应用的方式— [开始免费监控](https://lp.logrocket.com/blg/web3-signup)。