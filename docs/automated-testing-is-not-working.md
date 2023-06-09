# 自动化测试不起作用

> 原文：<https://blog.logrocket.com/automated-testing-is-not-working/>

在我开始之前，我想指出我不是指某个特定的项目或某个特定的个人。我相信这些问题是行业性的。几乎所有我合作过的自动化测试人员都竭尽全力让这台有故障的机器工作。我讨厌的是游戏，而不是玩家。

如果我没有弄错的话，我似乎已经在另一个现实中醒来，在这个现实中，大量的金钱、时间和资源被分配给了端到端测试的编写和持续维护。我们有一种被称为自动化测试人员的新型开发人员，他们存在的主要原因不仅仅是为了发现 bug，而且是为了编写回归测试，以消除重新运行初始手动测试的需要。

自动化回归测试在理论上听起来很棒，当发现每个 sprint 中的每个故事都有一个用 [Selenium webdriver](https://www.seleniumhq.org/projects/webdriver/) 编写的端到端测试时，任何开始新工作的人都会留下深刻的印象。

我听说过许多通常用 selenium webdriver 编写的端到端测试由于其脆弱的本质而被删除的故事。测试自动化似乎只会导致 CI 构建的破坏，不确定性测试使得变更和进展几乎不可能。我们的测试自动化工程师太忙或者不愿意执行手动测试，而是用这些表现不佳的时间和资源占用的非确定性测试来点燃地狱的火焰。

失败后重新运行的测试是标准的，甚至由一些测试运行者提供。一些最具挑战性的代码是由经验最少的开发人员编写和维护的。测试代码没有受到同样的关注。我们从未停下来问自己这种疯狂的努力是否值得。我们不跟踪度量标准，我们只添加更多的测试。

这就像一个奇怪的土拨鼠日，只是它是一个破碎的建筑，而不是开始相同系列事件的新的一天。我现在将列出我在一个项目中看到的重复问题，这个项目背负着进行大规模端到端测试套件的负担。

## 错误的期望自动化测试会发现新的缺陷

在撰写本文时，几乎所有的测试都在一组固定的输入上断言它们的期望。下面是一个简单的登录特征文件:

```
Feature: Login Action

Scenario: Successful Login with Valid Credentials

  Given User is on Home Page
  When User Navigate to LogIn Page
  And User enters UserName and Password
  Then Message displayed Login Successfully
```

特征文件在所谓的步骤定义中执行以下 Java 代码:

```
@When("^User enters UserName and Password$")
  public void user_enters_UserName_and_Password() throws Throwable {
  driver.findElement(By.id("log")).sendKeys("testuser_1");
  driver.findElement(By.id("pwd")).sendKeys("[email protected]");
  driver.findElement(By.id("login")).click();
 }
```

只有当这个有限的输入集合触发了 bug 时，这个测试才会发现 bug。新用户输入除了`testuser_1`和`[[email protected]](/cdn-cgi/l/email-protection)`之外的其他字符不会被这个端到端测试捕获。我们可以通过使用黄瓜表来增加输入的数量:

```
Given I open Facebook URL
 And fill up the new account form with the following data
 | First Name | Last Name | Phone No | Password | DOB Day | DOB Month | DOB Year | Gender |
 | Test FN | Test LN | 0123123123 | Pass1234 | 01 | Jan | 1990 | Male |
```

这些测试最有可能发现错误的时间是它们第一次运行的时候。当上述测试仍然存在时，我们将不得不维护这些测试。如果他们使用 selenium webdriver，那么我们可能会在持续集成管道上遇到延迟问题。

这些测试可以从测试金字塔下推到单元测试或集成测试上。

## 不要通过用户界面来驱动所有的测试

我并不是说我们应该废除端到端的测试，但是如果我们想要避免维护这些脆弱的测试，那么我们应该只测试快乐的路径。我想要一个冒烟测试，让我知道最重要的功能正在工作。在开发人员单元测试或集成测试中，异常路径应该在更细粒度的级别上处理。

登录示例中最常见的错误原因是用户输入。我们不应该旋转 selenium 来测试用户输入。我们可以编写廉价的单元测试来检查用户输入，不需要端到端测试的维护开销。我们仍然需要对快乐路径进行端到端的测试，只是为了检查它是否都挂在一起，但是我们不需要对异常路径进行端到端的测试。

测试可以而且应该由单元测试和集成测试来承担。

大家是不是都忘了[考试金字塔](https://martinfowler.com/articles/practical-test-pyramid.html)？

## Selenium webdriver 不适合此用途

我之前在我的博客 [Cypress.io:硒杀手](https://blog.logrocket.com/cypress-io-the-selenium-killer/)中写过这个。不编写非确定性的 selenium 测试几乎是不可能的，因为您必须等待 DOM 和宇宙的四个角落完全一致才能运行您的测试。

如果你在测试一个没有动态内容的静态网页，那么 selenium 是非常优秀的。然而，如果你的网站有一个或多个这样的条件，那么你将不得不面对古怪的或者不确定的测试:

*   从数据库读取和写入
*   JavaScript/ajax 用于动态更新页面，
*   (JavaScript/CSS)从远程服务器加载，
*   CSS 或 JavaScript 用于动画
*   JavaScript 或 React/Angular/Vue 等框架呈现 HTML

面对上述任何一种情况的自动化测试人员都会在他们的测试中加入一系列的等待，轮询等待，检查 ajax 调用是否已经完成，检查 javascript 是否已经加载，检查动画是否已经完成等等。

测试变成了一个绝对的混乱和一个完整的维护噩梦。不知不觉中，您就有了这样的测试代码:

```
click(selector) {
    const el = this.$(selector)
    // make sure element is displayed first
    waitFor(el.waitForDisplayed(2000))
    // this bit waits for element to stop moving (i.e. x/y position is same).
    // Note: I'd probably check width/height in WebdriverIO but not necessary in my use case
    waitFor(
      this.client.executeAsync(function(selector, done) {
        const el = document.querySelector(selector)

        if (!el)
          throw new Error(
            `Couldn't find element even though we .waitForDisplayed it`
          )
        let prevRect
        function checkFinishedAnimating() {
          const nextRect = el.getBoundingClientRect()
          // if it's not the first run (i.e. no prevRect yet) and the position is the same, anim
          // is finished so call done()
          if (
            prevRect != null &&
            prevRect.x === nextRect.x &&
            prevRect.y === nextRect.y
          ) {
            done()
          } else {
            // Otherwise, set the prevRect and wait 100ms to do another check.
            // We can play with what amount of wait works best. Probably less than 100ms.
            prevRect = nextRect
            setTimeout(checkFinishedAnimating, 100)
          }
        }
        checkFinishedAnimating()
      }, selector)
    )
    // then click
    waitFor(el.click())
    return this;
  }
```

看着这段代码，我热泪盈眶。这怎么可能不是一个巨大的薄片，而这需要时间和努力来让这个怪物活着呢？

[Cypress.io](https://www.cypress.io/) 通过将自身嵌入浏览器并在浏览器和代码同步执行的同一事件循环中执行来解决这个问题。采用异步，而不必求助于轮询、睡眠和等待助手，这是非常强大的。

## 测试的有效性没有被跟踪，我们也不会删除糟糕的测试

测试自动化工程师对他们的测试有很强的占有欲，根据我的经验，我们不做任何工作来确定测试是否值得。

我们需要监控测试碎片的工具，如果碎片太高，它会自动隔离测试。隔离将测试从关键路径中移除，并为开发人员归档一个 bug 以减少碎片。

## 从地球表面根除所有非确定性试验

如果重新运行构建是修复测试的解决方案，那么该测试需要被删除。一旦开发人员养成了按下“重新构建”按钮的思维模式，那么对测试套件的所有信任都将不复存在。

## 失败后重新进行测试是彻底失败的标志

测试运行程序[西葫芦](https://github.com/prashant-ramcharan/courgette-jvm/blob/master/README.md)可以可耻地配置为在失败时重新运行:

```
@RunWith(Courgette.class)=
 @CourgetteOptions(
  threads = 1,
  runLevel = CourgetteRunLevel.FEATURE,
  rerunFailedScenarios = true,
  showTestOutput = true,
  ))

 public class TestRunner {
 }
```

`rerunFailedScenarios = true`所说的是我们的测试是不确定的，但我们不在乎，我们只是要重新运行它们，因为希望下次它们会工作。我认为这是承认有罪。当前的测试自动化思想认为这是可接受的行为。

如果您的测试是不确定的，也就是说，当使用相同的输入运行时，它有不同的行为，那么删除它。非确定性测试会耗尽您项目的信心。如果你的开发人员不假思索地按下魔法按钮，那么你已经到了这一步。删除这些测试并重新开始。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

## 端到端测试的维护代价很高

测试维护已经成为许多测试自动化计划的死亡。当更新测试比手工重新运行测试花费更多的精力时，测试自动化将会被放弃。您的测试自动化计划不应该成为高维护成本的牺牲品。

测试不仅仅是简单的执行和报告。环境设置、测试设计、策略、测试数据经常被遗忘。随着运行每个扩展测试套件所需的资源数量，您可以从您选择的云提供商那里看到每月的发票暴涨。

## 自动化测试代码应该被视为生产代码

自动化测试人员通常是开发新手，突然被赋予在 selenium webdriver 中编写复杂的端到端测试的任务，因此，他们需要做以下工作:

*   不要复制粘贴代码。复制和粘贴代码有自己的生命，决不能发生。我经常看到这种情况
*   不要通过用户界面设置测试代码。这种情况我已经见过很多次了，您最终会得到臃肿的测试，这些测试会多次重复运行相同的测试设置代码，以达到为新场景编写更多测试代码的目的。测试需要是独立的和可重复的。每个新特性的播种或初始化应该通过脚本或者在测试之外进行
*   不要使用`Thread.sleep`和其他黑客。每当自动化测试人员使用带有任意数字的`Thread.sleep`，徒劳地希望在`x`毫秒后世界会像他们预期的那样，一只小狗就死在了天堂。失败是使用`Thread.sleep`的唯一结果

自动化测试代码需要接受与真实代码相同的审查。这些难以编写的测试场景不应该是到达终点的复制粘贴的海洋。

## 测试人员不再想测试

我对这一点有一些同情，但是手动测试不像编写代码那样引人注目，所以手动测试被认为是过时和无聊的。自动化测试应该在手动测试之后编写，以捕捉回归。我曾经合作过的许多自动化测试人员不再喜欢手工测试了，并且它正在半途而废。手动测试比用一组固定的输入编写一个测试会发现更多的错误。

现在，在一个全新的标签或故事上编写小黄瓜语法，然后直接编写特征文件和步骤定义是很平常的事情。如果发生了这种情况，那么就绕过手工测试，在实际回归发生之前编写回归测试。我们正在为一个可能永远不会发生的错误编写测试。

## 结论

在我看来，我们在一些不起作用的东西上花费了大量的金钱和资源。我从自动化测试中看到的唯一好的结果是一个非常长的构建，并且我们已经使改变变得异常困难。

我们对自动化测试不感兴趣。原则上听起来很棒。尽管如此，还是有如此多的陷阱，以至于我们会很快陷入一个死胡同，在这个死胡同中，变化是令人痛苦的，并且很难保持测试在没有好的理由的情况下继续存在。

我将留给你们这些我认为需要回答的问题:

*   为什么没有人质疑回报是否值得努力？
*   为什么我们允许古怪的测试成为常规，而不是例外？
*   为什么用相同的输入重新运行一个测试并得到不同的结果是情有可原的，以至于我们有像 courgette 这样的运行者自动地这样做？
*   为什么硒在不适合用途的时候是常态？
*   为什么开发人员在匆忙完成任务时仍然求助于大量的等待、轮询等待，甚至是最糟糕的代码？这是鳞片的根源。

## 使用 [LogRocket](https://lp.logrocket.com/blg/signup) 消除传统错误报告的干扰

[![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/signup)

[LogRocket](https://lp.logrocket.com/blg/signup) 是一个数字体验分析解决方案，它可以保护您免受数百个假阳性错误警报的影响，只针对几个真正重要的项目。LogRocket 会告诉您应用程序中实际影响用户的最具影响力的 bug 和 UX 问题。

然后，使用具有深层技术遥测的会话重放来确切地查看用户看到了什么以及是什么导致了问题，就像你在他们身后看一样。

LogRocket 自动聚合客户端错误、JS 异常、前端性能指标和用户交互。然后 LogRocket 使用机器学习来告诉你哪些问题正在影响大多数用户，并提供你需要修复它的上下文。

关注重要的 bug—[今天就试试 LogRocket】。](https://lp.logrocket.com/blg/signup-issue-free)