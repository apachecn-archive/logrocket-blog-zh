# 智能合同开发:要避免的常见错误

> 原文：<https://blog.logrocket.com/smart-contract-development-common-mistakes-avoid/>

为区块链构建智能合约与为 Web2 构建应用程序非常不同。首先，智能合约在区块链上部署后是不可变的，而 Web2 应用程序是可伸缩的和松散耦合的。其次，智能合约代码具有实际的货币价值，使得开发过程中的安全性更加重要。

不幸的是，由于被利用的错误和漏洞，以及不正确编写的代码，Web3 空间遭受了许多攻击和巨大损失。

智能合约中存在的漏洞或错误将会给任何在 mainnet 上部署后与之交互的人带来麻烦。

在本文中，我将分享提高智能合约开发安全性的关键信息。我将提供详细的说明来帮助您识别和预防常见的智能合约漏洞和攻击媒介。

*向前跳转:*

## 重入攻击

可重入攻击是最早的智能合约漏洞之一；它于 2016 年在[区块链以太坊首次被发现。](https://www.nytimes.com/2016/06/18/business/dealbook/hacker-may-have-removed-more-than-50-million-from-experimental-cybercurrency-project.html)

为了接收以太网，智能合约必须具有回退功能。回退功能由发送以太网的智能合约调用。如果发送智能协定的代码编写不正确，接收智能协定就有机会利用发送方。

下面是一个不安全的发送智能协定的示例:

```
// INSECURE
mapping (address => uint) private userBalances;

function withdrawBalance() public {
    uint amountToWithdraw = userBalances[msg.sender];
    (bool success, ) = msg.sender.call.value(amountToWithdraw)(""); 
        // At this point, the caller's code is executed, and can call withdrawBalance again
    require(success);
    userBalances[msg.sender] = 0;
}

```

上面的代码使用了一个映射数据结构来跟踪契约中的`userBalances`。在调用`withdrawBalance`函数后，代码将`userBalances`的值重置为零。

这个函数容易受到重入攻击，因为它在更新用户在`userBalances`映射中的余额之前向用户发送以太网。这是不安全的，因为它给了接收者一个机会，在用户的余额被更新之前，在其回退函数中再次调用`withdrawBalance`函数。换句话说，接管人可以提取比合同中更多的钱。

以下是撰写智能合同的更好方法:

```
mapping (address => uint) private userBalances;

function withdrawBalance() public {
    uint amountToWithdraw = userBalances[msg.sender];
    userBalances[msg.sender] = 0;
    // The user's balance is already 0, so future invocations won't withdraw anything
    (bool success, ) = msg.sender.call.value(amountToWithdraw)("");
    require(success);
}

```

现在让我们看一个更实际的例子，一个不安全的智能契约可能会遭受可重入攻击。代码是一个不安全的可靠性合同，名为`EtherStore`:

```
// SPDX-License-Identifier: MIT
pragma solidity >=0.4.22 <0.9.0;

contract EtherStore {
        // Mapping Data structure to keep track of balances of users 
    mapping(address => uint256) public balances;

        // As the name suggests, you can use this function to deposit Ether to the contract
    function deposit() public payable {
        balances[msg.sender] += msg.value;
    }

        // For withdrawing the Ether stored by users
    function withdraw() public {

                // Gets the balance of the user calling the withdraw function
        uint256 bal = balances[msg.sender];

                // Checks if the user/caller has Ether in this contract
        require(bal > 0);

                // Transfers the Ether to the user 
                // Vulnerability point
        (bool sent, ) = msg.sender.call{value: bal}("");
        require(sent, "Failed to send Ether");

        balances[msg.sender] = 0;
    }

    // Helper function to check the balance of this contract
    function getBalance() public view returns (uint256) {
        return address(this).balance;
    }
}

```

这份合同允许用户存入乙醚，然后在他们愿意的时候提取他们的余额。契约使用映射来跟踪用户的余额，但是它犯了一个错误，即在更新用户的余额之前发送 Ether。这使得契约容易受到可重入攻击。

一个恶意的参与者可以部署一个地址为`EtherStore`的契约，利用可重入性漏洞，然后提取比他们最初存放的更多的以太网。

### 防止重入

为了减轻恶意参与者的重入攻击的风险，在编写从契约中发出以太的函数时，请遵循 Solidity 的检查-效果-交互模式:

*   检查:确认调用者是否有资格调用该函数
*   效果:在发出乙醚之前，进行函数将触发的状态改变
*   互动:发出以太

注意，易受攻击的`EtherStore`契约做了一些类似的事情，但是顺序不对。它检查调用者是否有资格使用该函数，但是然后跳到交互阶段而不实现效果。

下面是一个遵循检查-效果-交互模式的安全`EtherStore`契约的示例:

```
// SPDX-License-Identifier: MIT
pragma solidity >=0.4.22 <0.9.0;

contract EtherStore {
        // Mapping Data structure to keep track of balances of users 
    mapping(address => uint256) public balances;

        // As the name suggests, you can use this function to deposit Ether to the contract
    function deposit() public payable {
        balances[msg.sender] += msg.value;
    }

        // For withdrawing the Ether stored by users
    function withdraw() public {

                // Gets the balance of the user calling the withdraw function
        uint256 bal = balances[msg.sender];

                // Check
        require(bal > 0);

                // Effect
                balances[msg.sender] = 0;

                // Interaction 
        (bool sent, ) = msg.sender.call{value: bal}("");
        require(sent, "Failed to send Ether");
    }

    // Helper function to check the balance of this contract
    function getBalance() public view returns (uint256) {
        return address(this).balance;
    }
} 

```

## 算术溢出和下溢

算术溢出和下溢是源于可靠性本身的问题。较新版本的 Solidity (0.8.0 和更高版本)可以检测这种类型的错误。

Solidity 中的`uint`数据类型用于声明从 0 开始的[无符号整数](https://www.ibm.com/docs/en/aix/7.2?topic=types-signed-unsigned-integers)。该数据类型被限制在一定的范围内，包括`uint8`、`uint16`、`uint32`、`uint64`、`uint128,`和`uint256`。例如，`uint8`表示从 0 到 2^8–1(即 0 到 255)的所有无符号整数。

如果我们想使用一个高于 255 的数字，我们可以首先考虑采用一个范围更广的`uint8`子类型。然而，这种解决方案不是最理想的，因为它不适用于最大`uint`子类型范围之外的数字。

`uint256`表示从 0 到 2^256-1.的所有无符号整数如果我们想表示一个超出其范围的`uint`，我们没有替代的`uint`类型可以使用。这似乎不是一个大问题。毕竟，2^256-1 是一个非常大的数字，所以想要在它的范围之外工作的可能性非常低。

然而，如果我们更仔细地研究这个问题，我们会发现这个问题更重要。

版本低于 0.8.0 的 Solidity 编译器对于使用超出正在使用的`uint`类型范围的无符号整数不会给出警告。相反，Solidity 编译器做了一些有点幽默的事情。它将超出范围的数字转换为零-没有警告！

让我们看一个例子。

假设我们使用一个`uint8`类型来存储数字 256，它显然大于 2^8-1(255)；

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

```
uint8 num = 256;

```

Solidity 编译器的反应是溢出无符号整数。

换句话说，因为`uint8`只表示从 0 到 255 的整数，所以大于这个范围的无符号整数将被重置为它的值和`uint`类型的上限之间的差(即，本例中的 256-255 或 1)。新的整数(本例中为 1)“溢出”到`uint8`范围的第一个整数；范围是 0 到 255，所以第一个整数是 0。

同样，这仅发生在低于 0.8.0 的坚固度版本中。

现在，让我们来看一个带有算术上溢/下溢漏洞的合同，以便实际了解这种类型的漏洞可能造成的威胁:

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.7.6;

contract TimeLock {
    mapping(address => uint) public balances;
    mapping(address => uint) public lockTime;

    function deposit() external payable {
        balances[msg.sender] += msg.value;
        lockTime[msg.sender] = block.timestamp + 1 weeks;
    }

    function increaseLockTime(uint _secondsToIncrease) public {
        lockTime[msg.sender] += _secondsToIncrease;
    }

    function withdraw() public {
        require(balances[msg.sender] > 0, "Insufficient funds");
        require(block.timestamp > lockTime[msg.sender], "Lock time not expired");

        uint amount = balances[msg.sender];
        balances[msg.sender] = 0;

        (bool sent, ) = msg.sender.call{value: amount}("");
        require(sent, "Failed to send Ether");
    }
}

```

上面的代码是针对一个`TimeLock`智能合约的。该合同旨在将用户的以太网锁定一周的时间；它还具有增加持续时间的功能。对于智能合约来说，这是一个非常好的工具。不幸的是，这个契约容易受到上溢/下溢攻击，因为`withdraw`函数逻辑依赖于`uint`。

恶意参与者可以编写代码，将乙醚存放到合同中，然后通过将锁定时间函数“溢出”(本质上是重置)到零，在锁定期结束之前提取更大的金额。

### 防止算术溢出和下溢

在高于 0.8.0 的 Solidity 版本中，编译器将检测并调整算术溢出/下溢漏洞。

如果您使用的是低于 0.8.0 的 Solidity 版本，您可以使用 SafeMath 库来防止此漏洞。

下面是我们使用`SafeMath`的`TimeLock`契约的安全版本:

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.7.6;

library SafeMath {

  function mul(uint256 a, uint256 b) internal pure returns (uint256) {
    if (a == 0) {
      return 0;
    }
    uint256 c = a * b;
    assert(c / a == b);
    return c;
  }

  function div(uint256 a, uint256 b) internal pure returns (uint256) {
    // assert(b > 0); // Solidity automatically throws when dividing by 0
    uint256 c = a / b;
    // assert(a == b * c + a % b); // There is no case in which this doesn't hold
    return c;
  }

  function sub(uint256 a, uint256 b) internal pure returns (uint256) {
    assert(b <= a);
    return a - b;
  }

  function add(uint256 a, uint256 b) internal pure returns (uint256) {
    uint256 c = a + b;
    assert(c >= a);
    return c;
  }
}

contract TimeLock {
    using SafeMath for uint; // use the library for uint type
    mapping(address => uint256) public balances;
    mapping(address => uint256) public lockTime;

    function deposit() public payable {
        balances[msg.sender] = balances[msg.sender].add(msg.value);
        lockTime[msg.sender] = now.add(1 weeks);
    }

    function increaseLockTime(uint256 _secondsToIncrease) public {
        lockTime[msg.sender] = lockTime[msg.sender].add(_secondsToIncrease);
    }

    function withdraw() public {
        require(balances[msg.sender] > 0);
        require(now > lockTime[msg.sender]);
        uint transferValue = balances[msg.sender];
        balances[msg.sender] = 0;
        msg.sender.transfer(transferValue);
    }
}

```

## 跨功能竞争条件

竞争条件是编程中一个非常普遍的问题，它们以各种形式出现在智能合约开发中。我们之前讨论过的可重入性漏洞实际上是可靠性中的一种竞争条件。

当两个或多个可靠性函数在有机会进行更新之前，试图访问相同的状态变量进行各自的计算时，会出现跨函数竞争情况。

让我们来看看 Solidity 智能合同，其中观察到了此漏洞:

```
mapping(address => uint256) public userBalances;

/* uses userBalance to transfer funds */
function transfer(address to, uint256 amount) public {
    if (userBalances[msg.sender] >= amount) {
        userBalances[to] += amount;
        userBalances[msg.sender] -= amount;
    }
}

/* uses userBalances to withdraw funds */
function withdrawalBalance() public {
    uint256 amountToWithdraw = userBalances[msg.sender];
        // makes external call to the address receiving the ether
    (bool sent, ) = msg.sender.call{value: amountToWithdraw}("");
    require(sent, "Failed to send Ether");
    userBalances[msg.sender] = 0;
}

```

在上面的契约中，我们有两个函数:`transfer`和`withdrawalBalance`。此合同可能会引发跨职能部门的错误。

一个糟糕的参与者可以写一个契约，几乎同时调用撤销和转移功能。如果成功，攻击者将能够把乙醚转移到另一个地址，同时也能收回乙醚。

为了更好地理解这个漏洞，让我们考虑一个链外的例子。假设您的银行账户中有 100 美元，想要转账和取款。一个竞争条件的例子是，在您的帐户余额更新之前，您同时转账$50 和取款$50。在这种情况下，即使你花了 100 美元，你的账户上最终(至少暂时)还有 50 美元。这被称为双重支出，这是区块链试图解决的问题类型的一个例子。

### 防止跨函数竞争情况

正如我前面提到的，跨函数 bug 和可重入攻击之间有一些相似之处。这两个漏洞都是竞争条件的类型，并且都是由外部调用的不当使用引起的。

在他们的解决方案中可以找到另一个相似之处。当在编程中处理一般的竞争条件时，建议锁定正在用于计算的特定变量，直到该过程完成。幸运的是，坚固性具有我们之前讨论过的检查-效果-相互作用模式。

为了说明检查-效果-交互模式是如何工作的，让我们假设一个函数要提取特定数量的乙醚，并且所讨论的契约有一个变量来跟踪用户余额。

首先，我们检查调用者是否有资格调用这个函数。然后，在效果阶段，我们在进行外部调用之前对变量进行更改。最后，在交互阶段，我们发出以太的外部调用。

当正确实现时，这种模式足以阻止智能契约中的跨功能竞争情况。

这是我们之前检查过的跨功能竞争条件契约的安全版本:

```
mapping(address => uint256) public userBalances;

/* uses userBalance to transfer funds */
function transfer(address to, uint256 amount) public {
    if (userBalances[msg.sender] >= amount) {
        userBalances[to] += amount;
        userBalances[msg.sender] -= amount;
        return true;
    }
}

/* uses userBalances to withdraw funds */
function withdrawalBalance() public {
    // Not really a check but close enough
    uint256 amountToWithdraw = userBalances[msg.sender];

    // Effect 
    userBalances[msg.sender] = 0;

    // Interaction
    (bool sent, ) = msg.sender.call{value: amountToWithdraw}("");
    require(sent, "Failed to send Ether");
}

```

## 交易顺序依赖性

由于以太坊是一个公共区块链，因此出现的一些漏洞与竞争条件有关，在竞争条件下，恶意行为者使用用户提供的信息来运行交易，这些交易以他人为代价使自己受益。

以太坊上的每个交易在被挖掘之前都被发送到内存池。采矿者选择他们想要添加到区块的交易，并开采到区块链。矿商有动机选择天然气价格更高的交易，因为这些交易会转化为更大的回报。在这种情况下，如果最后一个交易的天然气价格高于第一个交易的价格，则最后进入 Mempool 的交易有可能首先被开采。

这一漏洞使不良行为者能够从某些交易中获取敏感信息，并有机会在最初的交易被挖掘之前利用这些信息为自己谋利。

为了进一步阐明这一点，让我们看一个例子。

想象一个使用了一定量以太的契约，第一个将特定字符串传递给该契约的函数的人将获得存储在该契约中的以太。用户 A 发现了访问契约中以太所需的字符串，因此他们调用函数并传递该字符串。调用该函数的用户 A 的事务必须首先到达 Mempool，因此他们传递给该函数的输入将对有权查看 Mempool 的每个人可见。

现在，假设一个恶意参与者(用户 B)可以查看用户 A 在他们的事务中使用的字符串。用户 B 使用该字符串运行相同的功能，但天然气价格更高，导致矿工首先选择用户 B 的字符串。用户 B 看起来像第一个发现字符串的人，他们从契约中得到以太。

交易顺序依赖攻击通常被称为抢先运行。领先可能有多种形式。在另一个场景中，可以建立一个契约，使得无论是谁解决了这个函数，都可以获得契约中的所有以太，但是有一个规定，要运行这个函数，用户必须通过一个`Require`来检查他们作为输入传递给函数的字符串是否与特定哈希的前映像匹配。

通常，这是抵御攻击的一个很好的保护措施，因为很难计算散列的前像。然而，一个坏的参与者可以用正确的解决方案监视任何用户的`Mempool`，复制用户的输入，然后用更高的油价运行函数，以便从合同中获得乙醚。

抢先交易的另一个例子是，用户 A 批准用户 B 用`Approve`和`TransferFrom`函数花费一定数量的代币，但后来减少了金额。现在，让我们假设用户 B 正在监视内存池，并看到用户 A 的事务拯救了他们分配的令牌数。用户 B 可以将他们自己的交易(具有更高的汽油价格)发送到 Mempool，花费最初数量的分配令牌。由于用户 B 的事务是在用户 A 减少其令牌之前挖掘的，因此用户 B 获得了原始分配。此外，当用户 A 的交易最终被挖掘时，他们还会收到数量减少的令牌！

### 防止交易订单依赖

前置运行或交易顺序依赖性攻击很难阻止，但并非不可能。

防止抢跑的一个方法是在智能合同中创建天然气价格的上限，这样用户就不能改变天然气价格。然而，这种策略只能阻止普通用户，因为矿工仍然可以绕过上限。在许多情况下，可能没有理由惊慌，因为矿工很难瞄准一个单一的区块。

第二种方法是让用户采用[提交/展示方案](https://blog.logrocket.com/build-random-number-generator-blockchain/#the-commit-reveal-protocol)。用户发送的第一个事务将以特定的方式加密(提交阶段。然后，当事务被挖掘时，用户发送另一个事务，对第一个事务中发送的信息进行解密(显示阶段)。用这种方法，普通用户和矿工都拉不到前跑攻击。

## 时间戳依赖性

使用`block.timestamp`函数获取当前时间，然后使用这些数据在智能契约中执行一些业务逻辑，这可能很有诱惑力。然而，由于以太坊的分散性质，不建议这样做。

`block.timestamp`函数提供的时间不准确，可能被恶意矿工操纵。

### 防止时间戳依赖

关于时间戳依赖性，需要记住以下几点:

*   不要依赖`block.timestamp`或`blockhash`作为随机的来源，除非你知道你在做什么；这两种功能都会在一定程度上受到矿工的影响
*   当前块的时间戳必须严格大于上一个块的时间戳，但唯一的保证是它将位于规范链中两个连续块的时间戳之间
*   如果您必须在智能合约的业务逻辑中使用时间，那么可以考虑使用 oracles，比如 [Chainlink](https://docs.chain.link) ，它聚合离线时间戳数据

## 结论

在本文中，我们回顾了几个常见的智能合约漏洞以及一些需要避免的智能合约开发错误:可重入性、算术溢出和下溢、跨函数竞争条件、事务顺序依赖和时间戳依赖。

本文中回顾的所有智能合同示例都是用 Solidity 编写的。

如果你觉得这篇文章有帮助，请分享给其他智能合约开发者。当然，我们无法预测未来可能出现的新漏洞。但是，与此同时，我们可以通过吸取 Web3 领域最近的损失来减轻威胁。

## 加入像 Bitso 和 Coinsquare 这样的组织，他们使用 LogRocket 主动监控他们的 Web3 应用

影响用户在您的应用中激活和交易的能力的客户端问题会极大地影响您的底线。如果您对监控 UX 问题、自动显示 JavaScript 错误、跟踪缓慢的网络请求和组件加载时间感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/web3-signup)

.

[![LogRocket Dashboard Free Trial Banner](img/dacb06c713aec161ffeaffae5bd048cd.png)](https://lp.logrocket.com/blg/web3-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/web3-signup)

LogRocket 就像是网络和移动应用的 DVR，记录你的网络应用或网站上发生的一切。您可以汇总和报告关键的前端性能指标，重放用户会话和应用程序状态，记录网络请求，并自动显示所有错误，而不是猜测问题发生的原因。

现代化您调试 web 和移动应用的方式— [开始免费监控](https://lp.logrocket.com/blg/web3-signup)。