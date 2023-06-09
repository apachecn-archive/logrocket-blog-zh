# 以太坊与流量区块链为 NFT 发展

> 原文：<https://blog.logrocket.com/ethereum-flow-blockchain-nft-development/>

区块链技术是支持加密货币和许多分散应用的协议。区块链是交易的数字记录，在连锁的计算机网络上分发和复制。区块链中的单个交易被称为区块，所有区块以链状结构连接，因此得名区块链。

区块链领域的最新创新是通过不可替代的代币来展示和交易艺术品和收藏品。区块链交易由哈希记录，哈希是一种不可变的加密生成的 ID。每当网络上的某个特定区块发生交易时，新交易的记录就会传播到整个区块链，任何人都很难篡改系统。

在本教程中，我们将了解区块链，讨论 NFT，最后，比较开发 NFT 的[以太坊](https://ethereum.org/en/)和[流程](https://www.onflow.org)。我们开始吧！

## 区块链属性

要阅读本文，您应该熟悉区块链网络的以下属性，这些属性结合在一起，使其成为一项安全可靠的技术:

*   匿名:区块链上的所有参与者都是匿名的或用假名识别
*   安全性:区块链上的所有记录都经过加密，增加了一层安全性
*   分散和透明:所有网络参与者都有每笔交易的记录
*   不变性:区块链网络上的交易是不可逆的，不能被改变
*   时间戳:每笔交易都记录了交易发生的时间
*   分布式:每个参与者同意网络上每个记录的有效性
*   可编程:所有区块链都是可编程的。以太坊使用 Solidity，Flow 使用 Cadence，比特币使用比特币脚本构建智能合约

## 了解 NFTs

NFT 是一种独特的、不可替代的、不可互换的令牌，像以太坊或比特币一样充当加密资产，存储在区块链网络上。

NFT 和加密货币之间的主要区别在于，虽然加密货币可以交易或兑换等量的货币，但 NFT 不能。因为加密货币彼此完全相同，所以它们可以用于金融交易，而 NFT 则不能，因为它们是唯一的。

实际上，NFTs 可以用来以数字方式表示实物资产，比如房地产和艺术品。NFTs 可用于身份管理，创造一个新的市场和一种不同形式的投资。你也可能从游戏、数字艺术或互联网迷因中认出非功能性思维。

本质上，您应该了解以下几点:

1.  NFT 是在区块链上具有唯一身份的加密资产
2.  非功能性测试是不可替代的，即不可互换
3.  NFT 是真实世界对象的数字表示，如艺术品或房地产
4.  NFT 也用于表示产权
5.  真实世界资产的虚拟化使它们可以交易，并减少了欺诈

## 区块链以太坊

以太坊是一个开源的、去中心化的区块链，具有智能合约特性。它是以以太网(ETH)为加密货币的第二大区块链网络。在撰写本文时，许多 NFT 都在以太坊网络上。

一旦开发人员意识到区块链可以用于加密货币交易之外的目的，以太坊就被创造出来以满足广泛的使用案例。

以太坊使用 [Solidity](https://docs.soliditylang.org/en/v0.8.11/) ，一种面向对象的编程语言，来构建智能合约和去中心化应用。Solidity 是一种专门为[以太坊虚拟机](https://ethereum.org/en/developers/docs/evm/)开发的编程语言。

与比特币不同，以太坊可以用于资产的数字化，并作为点对点的货币交换。以太坊在不断发展，看起来像是许多区块链项目的理想解决方案。

以太坊经历了一次挫折，当时流行的、基于以太坊的[C](https://www.cryptokitties.co)[rypto](https://www.cryptokitties.co)[K](https://www.cryptokitties.co)[itties](https://www.cryptokitties.co)游戏减慢了网络速度。因此，另一种被称为“流动区块链”的方法开始流行起来。

## 流量区块链

尽管 Flow 区块链于 2020 年推出，但许多人认为它是以太坊的继任者。

由流行的 CryptoKitties 游戏的创作者在 Dapper Labs 创建的 Flow 是在 CryptoKitties 堵塞了只有大约 10，000 名用户的以太坊网络时引入的。Flow 的目标是创建一个区块链平台，可以容纳数十亿用户使用其分散式应用程序。

Flow 是一个快速的、去中心化的区块链，它增强了去中心化应用程序和 NFT 的开发。像其他区块链一样,“心流”也有自己的货币，称为“心流”。流量是用于支付交易成本和网络赌注的储备资产。

像大多数区块链一样，Flow 引入了自己的面向资源的编程语言， [Cadence](https://docs.onflow.org/cadence/) ，具有一些独特的特性，使其适合智能合约开发。

一些特性包括函数和事务的内置前置和后置条件、强大的静态类型系统以及基于功能的安全性。目前，心流是 NBA 最佳射手 DApp 的首选区块链。

既然我们已经理解了心流和 NFT 的一些基本原理，那么让我们回顾一下在 NFT 开发中每种方法的优缺点。

## 交易费用

当决定使用哪个区块链进行 NFT 开发时，应该考虑相关的交易费用。

以太坊用户支付燃气费作为成功执行智能合约的交易费。费用取决于智能合约的复杂程度和网络负载。在以太坊区块链，使用以太坊本地加密货币(ETH)支付燃气费。有时，平均费用会上升到 20 美元以上。

Flow 引入了两种费用，一种是账户创建费，起价约为 0.001 FLOW，或约 0.008 美元，而交易费起价约为 0.000001 FLOW。只有在创建新账户时，才会收取账户创建费。它涵盖了存储 100kB 数据的成本。每笔交易收取一次交易费，在执行时发生。

> 注:本文写作时， [1 流量= 8.043 美元](https://bitcoinsprice.org/price/flow/usd/1)。

## 可量测性

构建任何类型的应用程序时，可伸缩性都是需要考虑的重要因素。流区块链的多节点架构使其优于以太坊网络，以太坊网络使用分片，这是一种在多个数据库中存储单个数据集的方法。以下是流多节点体系结构中存在的各种节点:

1.  执行节点:处理网络上每个事务的所有计算
2.  收集节点:改善网络连接，并使数据可用于分散的应用程序
3.  共识节点:决定网络上交易的顺序
4.  验证节点:验证执行节点所做的工作

在流区块链网络上，执行和收集节点都被实现来增加吞吐量时间和网络可扩展性，而验证和共识节点负责网络责任和安全性。

以太坊网络上的[日均吞吐时间约为每秒 13 至 15 笔交易，不足以大规模使用。Flow 的目的是改进这一指标，构建具有高安全性的可扩展 DApps，同时真正保持去中心化。](https://legacy.ethgasstation.info/blog/ethereum-transaction-how-long/)

## 智能合同

以太坊区块链是创建智能合约最受欢迎的区块链，以太坊智能合约以其不变性而闻名。执行后，在以太坊智能合约上执行的交易不可更改，增加了安全性和信任度。

您可能想知道如果交易有缺陷会发生什么。Flow 允许用户在其 mainnet 上发布测试状态的智能合约，因此智能合约的原始作者可以逐步更新代码。用户可以决定是随时使用代码，还是等待代码完全更新。

当作者确定代码是安全的，并且不需要再次控制代码时，智能合约就变得不可改变。在 Flow 区块链网络上更新智能合同的能力使其更加灵活，并为最终用户进行了更好的优化。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

## 共识机制

以太坊使用工作验证(PoW)机制，其中矿工竞争看谁将创建更多的块。能够比其他人更快地解决密码问题，在块之间建立密码连接，并与网络共享新块的矿工，最终获得以太网(ETH)。

使用粉末的一个紧迫问题是其高能量需求。一个看起来更好的替代方案是利益证明(PoS)机制，在这种机制中，协议验证者参与交易验证。验证器是随机选择的，创建新的块，与网络共享，并获得 ETH。PoS 可能会减少电费和煤气费中的能源使用。

虽然 Flow 已经在 PoS consensus 协议上运行，但是以太坊还没有实现它。

## 账户模型

以太坊区块链帐户基于 256 位或 32 字节的四个十六进制数字私钥。当对私钥执行数学运算时，会生成公钥。公钥通过一系列数学运算获得有效地址。这是一个单向过程，因此不能从有效地址创建私钥。

在流区块链中，帐户创建是自动进行的，并且可以支持多个公钥。首先，公钥和私钥必须通过[椭圆曲线数字签名算法](https://csrc.nist.gov/glossary/term/elliptic_curve_digital_signature_algorithm) (ECDSA)，这是[数字签名算法](https://csrc.nist.gov/glossary/term/digital_signature_algorithm)的一种变体，它使用椭圆曲线加密技术，或者通过 [Secp256k1 曲线](https://en.bitcoin.it/wiki/Secp256k1)，这是比特币公钥加密技术中使用的椭圆曲线的参数。

然后，交易被发送到区块链。通过此过程，新帐户存储被初始化，并且生成的密钥被分配给帐户。在流区块链上，每个帐户都有映射到它的一对多公钥`1 to n`。对于每一个公钥，都有一个对应的由帐户持有人拥有的私钥。

另一个要考虑的差异在于智能合约的部署。在以太坊区块链上，智能合约被部署到单个账户，而在流量区块链上，账户可以同时部署多个智能合约。

此外，在以太坊区块链上，以太坊帐户能够通过以太坊事件日志跟踪它与之交互的所有令牌和智能合约。以太坊不为智能合约中的账户资产提供单一存储。另一方面，由于流区块链的面向资源的编程范式，用户可以有效地跟踪他们的资源已经交互的智能合同。

## 工作哲学

以太坊背后的基本原则很大程度上依赖于敏捷性、普遍性、简单性、模块化和非歧视性。

由于以太坊的简单性，一个普通的软件工程师应该能够在以太坊上成功开发一个 DApp。

以太坊开发人员认为以太坊区块链是一个通用的基础平台，可用于开发各种用途的应用程序，如 NFT 等实物资产的令牌化、金融工具的开发以及货币的开发。

以太坊开发者正在寻找提高以太坊区块链安全性和可扩展性的方法。智能合约开发需要一个复杂的过程，需要考虑大量的场景，因为每一个不必要的操作都有成本。智能合约开发人员的一个主要任务是在应用程序架构和可靠性特性之间找到平衡。

心流的创造者带来了他们的哲学，这是他们与其他区块链，尤其是以太坊一起工作时所积累的经验的结果。有了流区块链，开发人员不再关注复杂的细节，他们现在更加关注编写业务逻辑。

## 结论

在本文中，我们学习了区块链技术的一些基础知识，NFTs 的基础知识，以及 Flow 和以太坊之间的主要区别。

如果你不打算在区块链上拍卖，那么以太坊将是一个更好的选择。但是，如果您关心灵活性和实现逻辑，并且不怕冒险和尝试新事物，那么流区块链是开发 NFT 的最佳选择。

我希望你喜欢这篇文章。如果你有任何问题，请留下评论。编码快乐！

## 加入像 Bitso 和 Coinsquare 这样的组织，他们使用 LogRocket 主动监控他们的 Web3 应用

影响用户在您的应用中激活和交易的能力的客户端问题会极大地影响您的底线。如果您对监控 UX 问题、自动显示 JavaScript 错误、跟踪缓慢的网络请求和组件加载时间感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/web3-signup)

.

[![LogRocket Dashboard Free Trial Banner](img/dacb06c713aec161ffeaffae5bd048cd.png)](https://lp.logrocket.com/blg/web3-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/web3-signup)

LogRocket 就像是网络和移动应用的 DVR，记录你的网络应用或网站上发生的一切。您可以汇总和报告关键的前端性能指标，重放用户会话和应用程序状态，记录网络请求，并自动显示所有错误，而不是猜测问题发生的原因。

现代化您调试 web 和移动应用的方式— [开始免费监控](https://lp.logrocket.com/blg/web3-signup)。