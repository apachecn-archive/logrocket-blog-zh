# Safari 什么时候才能最终整合起来？

> 原文：<https://blog.logrocket.com/when-will-safari-finally-get-it-together/>

## 最大的侮辱

将浏览器称为“新的 Internet Explorer”是所有现代浏览器厂商都想避免的事情。对苹果来说，不幸的是，Safari 已经在特定的子主题和尖刻的黑客新闻中获得了不想要的标签。Internet Explorer 6 阻止了 web 开发者使用许多最新、最酷的 web APIs，直到 2016 年 1 月微软正式停止支持它。Safari 在更小的程度上也在做同样的事情。

在 15.4 版本发布之前，Safari——或者更确切地说，是驱动它的 WebKit 引擎——远远落后于他们的竞争对手。15.4 之前的 PWA 支持是有限的，不支持主屏幕图标和通知。阴谋论者推测，苹果故意削弱 WebKit 以保护其应用商店业务。这些指控并不成立，但 Safari 仍然与臭名昭著的 Internet Explorer 有些许相似之处。

以下是苹果布道者珍·西蒙斯(Jen Simmons)恍然大悟的时刻:

可怜的珍似乎对这些批评有点吃惊。这条发自 2022 年 2 月的推文清楚地表明，苹果并没有意识到那些伟大的、无知的开发者大众认为 IE 6 的灵魂在 Safari 内部。

我们中的许多人都足够老了，以至于还记得 ie6–11 的黑暗日子，当时 web 开发人员在每个开发周期的末尾开始了专门针对 IE 修复 bug 的周期。

随着自动更新的 Internet Explorer Edge 的出现，微软也在帮助世界摆脱其旧的、Edge 之前的浏览器，Safari 发现自己正在努力获得任何流行。

根据 Web 平台测试仪表板，Safari 在 15.4 版本中的 interop 2022 仪表板上从 50 分跃升至 72 分。那么，这种粗鄙的指责 Safari 是新 IE 的说法有事实依据吗？

## Safari 为什么会获得“新 IE”的称号？

Safari 的新名称不讨人喜欢的主要原因之一是，苹果明智地将其浏览器版本与 macOS 版本捆绑在一起。这使得发布关键漏洞修复需要几周或几个月的时间。包括 Rich Harris 在内的许多人都公开评论说这是开发人员的主要痛点:

相比之下，Chrome 的发布周期为四周。但是 Safari 可能需要几个月才能发布一个小版本:

| 次要版本 | iOS 版本 | 发布日期 |
| --- | --- | --- |
| 15.1 | 15.1 | 2021 年 10 月 25 日 |
| 15.2 | 15.2 | 2021 年 12 月 13 日 |
| 15.3 | 15.3 | 2022 年 1 月 26 日 |
| 15.4 | 15.4 | 2022 年 3 月 14 日 |

更新被认为是不透明的，没有公开的路线图，也没有新功能或错误修复预计何时到来的信号。

PWA 支持让 Safari 成为 Chrome 和 Firefox 最穷的亲戚。对于任何想要在浏览器中创建类似应用程序的体验的开发者来说，缺乏推送通知是一个巨大的损失。不支持延迟加载图像是一个需要填补的巨大漏洞。

苹果在 2017 年 6 月添加了 [WebRTC](https://webrtc.org/) 支持，这大约是 Chrome 正式添加开箱即用功能的四年半之后。您可能会认为这种延迟阻碍了 WebRTC 的广泛采用。

在 [15.4 发布](https://webkit.org/blog/12445/new-webkit-features-in-safari-15-4/)之前，可以公平地说苹果已经失手了。他们至少需要对境况不佳的野生动物园进行清理和润色。他们成功了吗？

## safari 15.4 版让世界回归正轨

苹果在[15.4](https://webkit.org/blog/12445/new-webkit-features-in-safari-15-4/)版本中增加了 70 项新功能。70 个新特性是一个重要的版本，它与小规模、经常发布的现代连续交付实践背道而驰。相比之下，Chrome 99 有 28 个安全补丁。

其中之一是——最后——延迟加载映像的能力，这是对包大小敏感的开发人员和对延迟敏感的开发人员绝对必须具备的。然而，我们不要忘记，自 2019 年发布的版本 77 以来，他一直是 Chrome 的一个功能，自 2020 年发布的版本 75 以来，他一直是 Firefox 的一个功能。

Safari 的 PWA 支持在此版本中有所改进，最终支持在 web 应用清单中声明图标。[service worker navigation preload](https://w3c.github.io/ServiceWorker/#service-worker-registration-navigationpreload)是一个受欢迎的新增功能，它可以通过允许网络请求与 service worker 启动并行发生来减少启动时间。不幸的是，推送通知仍然只有实验性的支持。

任何称职的 web 开发人员都曾在桌面和移动 Safari 上与许多滚动错误作斗争。15.4 版本引入了[平滑滚动](https://www.w3.org/TR/css-overflow-3/#smooth-scrolling)，让开发者能够从一个位置跳到另一个位置，并平滑地动画滚动操作。

另一个值得注意的增加是[完美的 WebRTC 协商](https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API/Perfect_negotiation)，最终使 Safari 符合 [WebRTC 1.0 规范](https://wpt.fyi/results/webrtc/RTCPeerConnection-perfect-negotiation.https.html?run_id=5729316372480000&run_id=5655500883492864&run_id=5068969076588544&run_id=5723126955507712)。从背景来看，Chrome 在 2015 年发布的 Chrome 47 中开始添加 WebRTC 支持，Firefox 在 2013 年发布的 Firefox 20 中开始添加支持。Safari 很晚才参加聚会，并在 2020 年发布的 Safari 13.5 中开始添加 WebRTC 支持！

## 我们在 15.4 中没有得到的

没有推送通知是这个版本的一个明显的遗漏，这引起了许多 PWA 开发人员的愤怒。Web 应用程序或 PWAs 无法在 Safari 中提供与 Chrome 或 Firefox 相当的体验，除非以非实验性的方式在桌面和移动设备上添加这一功能。遗憾的是，这仍然是实验性的，因为这仍然是一个需要填补的空白，以获得真正的 PWA 支持。

另一个让我与 Chrome 联系在一起的功能是[配置文件](https://www.theverge.com/2021/3/2/22309154/google-chrome-reading-list-new-profile-bookmarks-colors-features)。作为一名自由软件开发人员，我可能在一个工作日有多个账户，能够在 Chrome 档案中将这些账户联系在一起是一种生产力优势，其他人应该在这方面进行创新。

## Safari 成功了吗？

苹果应该为 15.4 版本得到很多信任，我希望随后的次要和主要版本继续推动这种巨大的飞跃。至少现在，苹果确实意识到了开发者对 Safari 的看法，而且大量的资源似乎都在致力于改进 Safari。

在移动设备上，Safari 在全球市场份额上仍然是第二名，苹果至少需要巩固这一地位才能继续留在游戏中，然后争取第一名。

## 我们需要创新，Chrome 需要竞争

在出版的时候，Chrome 太占优势了。它在全球桌面和移动设备上占有超过 60%的市场份额。

来源: [StatCounter 全球统计–浏览器市场份额](https://gs.statcounter.com/browser-market-share)

这种优势的一个后果是，谷歌开发者在重要的对话中获得了太多的发言权，比如 TC39 会议。他们带来了太多符合自己需求的提议，比如 [protobuf](https://developers.google.com/protocol-buffers) 和 [Brotli](https://en.wikipedia.org/wiki/Brotli) ，最终从非谷歌开发者提出的其他想法中获益。首先，他们在[完全关闭了承诺取消，这是一个悲伤的 GitHub 问题](https://github.com/tc39/proposal-cancelable-promises/issues/70)，它将永远留在我的脑海里。

非 Chrome 浏览器被拿来与搜索引擎 [DuckDuckGo](https://duckduckgo.com/) 相比较，后者是谷歌搜索的竞争对手，我希望能成功，但我仍然使用谷歌，因为结果更好。

竞争孕育创新，我们需要可行的替代方案来推动技术进步。不幸的是，在我写作的这段时间，在 Chrome 上开发是最适合我快速完成工作的地方。我不能做出不符合人体工程学的立场，但苹果公司有足够的资金和开发资源来至少创造公平的竞争环境，如果不是让 Safari 成为真正的竞争对手的话。但是看起来，至少在 15.4 之前，他们要么故意选择不这样做，要么没有意识到这种需要。

## 下一步是什么？

Safari 的下一步是一个清晰的路线图和更好的更新故事。我不知道 Chrome 的版本号，因为更新刚刚发生，但我现在对 Safari(和 Internet Explorer)版本都很清楚。版本号应该无关紧要，而不是声名狼藉。

## 使用 [LogRocket](https://lp.logrocket.com/blg/signup) 消除传统错误报告的干扰

[![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/signup)

[LogRocket](https://lp.logrocket.com/blg/signup) 是一个数字体验分析解决方案，它可以保护您免受数百个假阳性错误警报的影响，只针对几个真正重要的项目。LogRocket 会告诉您应用程序中实际影响用户的最具影响力的 bug 和 UX 问题。

然后，使用具有深层技术遥测的会话重放来确切地查看用户看到了什么以及是什么导致了问题，就像你在他们身后看一样。

LogRocket 自动聚合客户端错误、JS 异常、前端性能指标和用户交互。然后 LogRocket 使用机器学习来告诉你哪些问题正在影响大多数用户，并提供你需要修复它的上下文。

关注重要的 bug—[今天就试试 LogRocket】。](https://lp.logrocket.com/blg/signup-issue-free)