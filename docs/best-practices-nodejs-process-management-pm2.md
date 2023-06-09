# PM2 的 Node.js 流程管理最佳实践

> 原文：<https://blog.logrocket.com/best-practices-nodejs-process-management-pm2/>

编写完美的应用程序只是开发人员工作的一半。无论您是使用 Heroku、Docker 还是 Kubernetes 来管理您的基础设施，您如何设计应用的部署策略并在整个生命周期中对其进行监控都将决定您项目的成败。

在本文中，我们将探索在 Node.js 应用程序中管理生产部署的一些最佳实践。首先，我们将回顾 Node.js 的内部工作方式，然后阐明流程级别的生产部署需求。最后，我们将探索管理生产部署的可能方法。

请记住，在 Node.js 生态系统中，有无数种方法可以执行生产部署。因此，本文将只关注生产环境中的 Node.js 流程管理，而不是容器化或完全托管的应用程序。我们开始吧！

## Node.js 内部

为了更好地理解生产流程管理的需求，让我们考虑一下 Node.js 作为一个异步、事件驱动的单线程平台是如何在内部运行的。

Node.js 可以异步执行耗时的操作，而不会阻塞其他操作。例如，Node.js 可以通过命令回调在一个 [FIFO 队列(事件循环](https://blog.logrocket.com/a-complete-guide-to-the-node-js-event-loop/))中执行来并发处理多个网络请求。

作为一个事件驱动的环境，Node.js 在内部传播状态变化和事件。例如，打开文件时，Node.js 不是在文件打开前一直阻止执行，而是在文件打开时才触发事件。因此， 回调在完成后执行。

Node.js 的单个实例在单个线程中运行。即使在多核系统中，Node.js 进程也不会利用多个 CPU。

既然我们已经回顾了定义 Node.js 的主要特征，那么让我们深入了解流程级别的生产部署的内在需求。

## Node.js 生产要求

一个成功的生产部署由许多移动部分组成，例如，源代码管理、持续集成管道和持续交付管道。部署后监控应用程序的流程和生命周期只是更大流程的一部分。

在我们考虑管理应用生命周期的不同策略之前，让我们回顾一下成功部署策略的要求和最佳实践。

例如，假设我们正在构建一个服务于 HTTP 请求的 web 服务器，这是 Node.js 的一个非常常见的用例。

当在生产环境中运行 web 服务器时，假设它必须持续活动才能响应传入的 HTTP 请求是安全的。web 服务器必须具有弹性，能够从意外错误、崩溃和未处理的异常中正常恢复。在 Node.js 中，一个未处理的异常可能会导致整个进程崩溃。

其次，一旦部署，服务器应该充分利用底层的基础设施。当通过云提供商部署 Node.js 时，通常会有多个 vCPUs 可用。默认情况下，Node.js 进程只利用一个 CPU，错过了许多潜在的性能提升。

最后，随着我们的 web 服务器获得更多的流量，我们需要监控 Node.js 进程来发现错误的行为，比如内存泄漏，并识别潜在的瓶颈。在不受监控的进程上调试生产问题就像使用语音命令编码一样，这不适合胆小的人！

您还应该考虑特定于 Node.js 应用程序的任何其他要求，例如 Chrome 的编程执行。虽然这些超出了本文的范围，但是根据您的具体用例，它们可能是值得考虑的。

## PM2 的流程管理

现在我们已经了解了生产中 Node.js 流程管理的主要需求，让我们利用最流行的 Node.js 流程管理器 [PM2](https://pm2.keymetrics.io/) 来解决它们。

首先，让我们通过将 PM2 安装为 npm 软件包并替换脚本来设置它，如下所示:

```
// Add pm2 to your app (install it globally for terminal access)
npm install pm2 -g

// Start your app with:
pm2 start app.js

// 🎉 Congratulations, your app processes are now managed by PM2!

```

让我们看看 PM2 如何解决我们上面描述的过程管理级别的需求。

### 跳回

每当 Node.js 中抛出未处理的异常时，进程将崩溃并退出。我们不能完全控制这种情况发生的频率，所以保守地计划是至关重要的。我们可以在堆栈中的不同级别恢复进程崩溃，例如，系统进程级别、容器级别或服务器级别。

当处理不依赖于服务器的文件系统或内存来保存请求之间的数据的无状态 Node.js 进程时，我更喜欢在系统进程级别处理崩溃恢复，优先考虑速度。例如，当我们在容器级别重启一个容器内的进程(如 Docker)时，Node.js 进程的启动时间将被添加到容器的重启时间中。

默认情况下，PM2 会立即重启任何崩溃的进程，从而提高弹性。尽管生产中的崩溃是一件严重的事情，但自动重启可以让您提前修复崩溃的根源，从而最大限度地减少对客户的影响。

### 利用基础设施

即使 Node.js 进程是单线程的，我们也有几种方法可以利用该进程运行环境的总 CPU 容量。

首先，我们可以启动 web 服务器流程的多个实例，然后使用外部工具(比如 NGIX)手动平衡请求负载。然而，Node.js 已经提供了一种方法来使用集群模块让多个进程共享同一个端口。

此外，PM2 抽象出集群模块，允许联网的 Node.js 应用程序扩展到所有可用的 CPU。命令行参数会自动创建与可用 CPU 内核一样多的进程，在它们之间对传入的网络流量进行负载平衡。

当与自动重启功能相结合时，该应用程序变得更有弹性。PM2 独立处理每个进程的生命周期，并仅在需要时重新启动:

```
// Start the app with as many processes as CPU cores are available
pm2 start app.js -i max

```

### 监视

虽然您可以手动[接入 JavaScript V8 引擎](https://nodejs.org/api/cli.html#cli_cpu_prof)并收集您想要的数据，但我建议使用预构建的应用性能监控(APM)工具。幸运的是，PM2 已经通过终端和付费网络服务提供了一个。您可以访问关于应用服务的有价值的信息，如 CPU、内存使用、请求延迟和控制台日志。

应用程序在 PM2 上运行后，在终端中添加以下代码:

```
pm2 monit

```

![Node V8 Engine Pm2 Information Dashboard](img/845ec647cf42ecffae183d6e4bcaaa73.png)

## 结论

希望这篇文章阐明了生产中适当的 Node.js 过程管理的重要性，这是我自己的经验中经常忽略的一个方面。

生产过程管理是一个复杂的话题，PM2 不是一个银弹。您项目的最佳解决方案将取决于您公司使用的堆栈及其现有的基础设施。也就是说，有必要检查一下您的生产 Node.js 部署是否受益于该基础设施。

良好的流程管理只是扩展 Node.js 应用程序的第一步。编码快乐！

## 200 只![](img/61167b9d027ca73ed5aaf59a9ec31267.png)显示器出现故障，生产中网络请求缓慢

部署基于节点的 web 应用程序或网站是容易的部分。确保您的节点实例继续为您的应用程序提供资源是事情变得更加困难的地方。如果您对确保对后端或第三方服务的请求成功感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/node-signup)

.

[![LogRocket Network Request Monitoring](img/cae72fd2a54c5f02a6398c4867894844.png)](https://lp.logrocket.com/blg/node-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/node-signup)

LogRocket 就像是网络和移动应用程序的 DVR，记录下用户与你的应用程序交互时发生的一切。您可以汇总并报告有问题的网络请求，以快速了解根本原因，而不是猜测问题发生的原因。

LogRocket 检测您的应用程序以记录基线性能计时，如页面加载时间、到达第一个字节的时间、慢速网络请求，还记录 Redux、NgRx 和 Vuex 操作/状态。

[Start monitoring for free](https://lp.logrocket.com/blg/node-signup)

.