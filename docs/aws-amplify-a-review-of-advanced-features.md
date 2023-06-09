# AWS Amplify:高级特性回顾

> 原文：<https://blog.logrocket.com/aws-amplify-a-review-of-advanced-features/>

AWS Amplify 于 2017 年 11 月首次亮相，自那以来，一直在推出各种应用。在本文中，我们将回顾什么是 Amplify，它是如何工作的，以及一些你可能不知道的更高级的 Amplify 特性。

## 什么是 AWS Amplify？

AWS Amplify 是一个 Java 库连接的命令行接口，用于为客户端访问工具链。Amplify 的界面提供了快速访问[功能，通过容器将 Java 库链接到其他云服务，如](https://blog.logrocket.com/the-question-of-why-in-single-page-application-frameworks-91383446d0f5/) [AWS AppSync](https://aws.amazon.com/appsync/) 和 [AWS S3](https://aws.amazon.com/s3/) ，与单页应用(SPA)框架一起工作。Amplify 还可以连接到现有的存储库。

使用 Amplify 提供的 [CLI](https://docs.amplify.aws/cli) 和库工具链，您可以低成本快速构建、部署、托管、试验和迭代 web 和移动应用。

### 通过 AWS 集成轻松开发

使用 AWS Amplify 不需要大量的应用程序开发经验，就可以利用 Amplify 控制台进行 [CI/CD](https://blog.logrocket.com/from-front-end-developer-to-a-devops-an-intro-to-ci-cd-7a8a8713fb34/) 。这部分是因为 Amplify 控制台充当了现代应用程序的前端 CI/CD 托管服务。

Amplify 通过连接到您的代码库并在每次代码提交时推出单一工作流，简化了后端和前端应用程序部署。Amplify 还利用亚马逊的 CDN[CloudFront](https://aws.amazon.com/cloudfront/)来简化托管。

凭借其容器化的创建方法和 AWS [CloudFormation](https://aws.amazon.com/cloudformation/) 部署，Amplify 允许您推送更新，同时消除前端和后端的不一致。此外，虽然去无服务器有[的好处和权衡](https://blog.logrocket.com/benefits-trade-offs-serverless/)，Amplify 使你的移动和网络应用程序代码更容易使用云管理服务。

为了创建轻量级和强大的 GraphQL APIs，AWS Amplify 还集成了 AWS AppSync，允许您查询亚马逊的 DynamoDB NoSQL 数据存储或通过亚马逊 API 网关选择 REST API。您还可以使用定制的 AWS Lambda，连接到 AWS S3，并使用 CloudFormation 来支持您的配置、供应和配置漂移部署。

关于 AWS 服务的更多信息，请查看此备忘单。

## 在开发环境中使用 Amplify

Amplify 支持第三方工具集成，如 Github、Gitlab 和 [AWS 代码提交库](https://aws.amazon.com/codecommit/)。使用 Amplify CLI，您可以通过将应用程序与其他服务(如 Amazon Cognito、AWS Appsync 或 AWS Lambda 或 S3)绑定，快速为应用程序添加功能。

Amplify 遵循亚马逊的“按使用付费”模式。您通常会为构建分钟、前端存储和提供的数据以及您的 CloudFormation 模板创建的后端资源付费。无服务器的强大之处在于它能够随着您的应用程序用户群的增长而增长，这也是 Amplify 成为发展中企业的优秀工具的原因之一。

在接下来的章节中，我们将讨论一些你可以用 Amplify 做的有趣的事情。对于所有功能，假设 Amplify CLI 已经安装[并配置](https://docs.amplify.aws/cli/start/install)；出于演示目的，我们将在 React 应用程序中工作。

## 证明

Amplify 允许您使用 [Amazon Cognito](https://aws.amazon.com/cognito/) 创建无缝且易于实施的身份验证，其中包括登录、注册、忘记密码选项和多因素身份验证工作流等操作。您还可以添加与谷歌、脸书和亚马逊等社交账户的登录集成。

### 放大器中认证

首先，添加身份验证库:

```
amplify add auth
```

在 React 应用程序目录中，安装 Amplify 库:

```
npm install aws-amplify @aws-amplify/ui-react
```

在您的 App.js 中，确保您从`aws-amplify/ui-react`库中导入了所需的组件。目前，我们正在进口`withAuthenticator`和`AmplifyAuthenticator`:

```
import React from 'react'; 
import { withAuthenticator, AmplifySignOut } from '@aws-amplify/ui-react'; 
const App = () => ( <div> <AmplifySignOut /> My App </div> ); export default withAuthenticator(App);
```

您还会注意到这里的`AmplifySignOut`组件，它呈现了一个注销按钮。

## 数据存储

如果您正在开发一个将使用设备上持久存储引擎的应用程序，Amplify】可以派上用场。Amplify 允许您使用 GraphQL 在移动/网络应用程序和云之间同步数据。

由 AWS Appsync 支持的 Datastore 允许利用存储的数据和分布式数据，而无需为离线和在线场景编写任何额外的代码。这使得处理分布式数据就像处理本地数据一样简单方便。Datastore 还允许您将数据保存在本地设备上，甚至无需创建 AWS 帐户。

### 放大器中的数据存储:

在 React 中开始使用数据存储的最快方法是:

```
npx create-react-app amplify-datastore --use-npm 
cd amplify-datastore 
npx [email protected]
```

## 分析学

一旦你的应用程序被部署，使用某种形式的分析来衡量人们如何与你的应用程序交互，它的总体表现如何，以及它如何在不同的平台和浏览器上运行，这对你可能是有益的。

Amplify 由 [AWS Pinpoint](https://aws.amazon.com/pinpoint/) 和 [AWS Kinesis](https://aws.amazon.com/kinesis/) 提供支持，让您使用自动跟踪来跟踪用户会话、网页指标，并创建自定义用户属性和应用内指标。当您使用 Amplify 进行分析时，您还将获得实时数据流访问，以获取用户洞察并构建数据驱动的营销策略，从而提高客户采用率和参与度。

### 在 Amplify 中使用分析

**后端设置**

要设置后端，请在项目根文件夹中运行以下命令:

```
amplify add analytics
```

确保您提供了端点资源名称(`yourPinpointResourceName`)。您可以使用 amplify push 部署后端。

部署后，名为 aws-export.js 的配置文件将被复制到您的项目文件夹目录中，AWS Pinpoint 链接将通过 CLI 与您共享，以跟踪您的应用程序事件。

**前端设置**

要在 frontend 上设置，请确保在 App.js 中导入 aws-export.js 文件和 Amplify 文件:

```
import Amplify from 'aws-amplify'; 
import awsconfig from './aws-exports';
Amplify.configure(awsconfig);
```

### 记录事件

要记录事件，您可以简化创建记录方法，如下所示:

```
import Amplify, { Analytics } from 'aws-amplify'; 
Analytics.record({ name: 'albumVisit' });
```

## AI/ML，带放大器

是的，你没看错！您可以向您的 Amplify 应用程序添加 AI 和 ML 功能，包括文本预测、从文本生成语音、文本翻译、图像识别、文本解释等。Amplify 简化了高级用例的编排，如上传图像进行自动交易，并使用 GraphQL 指令链接多个 AI/ML 操作。

Amplify 的 AI/ML 能力由 AWS [SageMaker](https://aws.amazon.com/sagemaker/) 和亚马逊[机器学习服务](https://aws.amazon.com/machine-learning/)提供支持。您不需要旋转自己的服务器来在 React 应用程序中使用这个模型或功能。

### 使用 AI/ML 和 Amplify

**后端设置:**

要设置后端，请在项目根文件夹中运行以下命令:

```
amplify add predictions
```

一旦所需的后端配置就绪，就可以使用`amplify push`进行部署。名为 aws-export.js 的配置文件将被复制到您的项目文件夹目录中。

**前端设置:**

导入在应用程序的根入口点中生成的配置文件；在本演示中，我们将在 React 中使用 app.js:

```
import Amplify from 'aws-amplify';
import Predictions, { AmazonAIPredictionsProvider } from '@aws-amplify/predictions'; 
import awsconfig from './aws-exports';
Amplify.configure(awsconfig); Amplify.addPluggable(new AmazonAIPredictionsProvider());
```

## 储存；储备

Amplify 为管理用户内容提供了一种非常简单的机制，无论您的数据是面向公众的、存储在私有存储桶中的还是受保护的。这使得将您的应用程序存储需求投入生产变得容易。毫不奇怪，Amplify 内置了对亚马逊 S3 的支持。

### 使用带放大器的存储

**后端设置:**

要设置后端，请在项目根文件夹中运行以下命令:

```
amplify add storage
```

接下来，CLI 将让您选择一种内容类型，并引导您完成启用`Auth`的选项(如果之前尚未启用的话)。它还会要求您命名您的 S3 桶。

如前几节所述，您可以使用`amplify push`进行部署。名为 aws-export.js 的配置文件将被复制到您的项目文件夹目录中。

**前端设置:**

确保您安装了 aws-amplify 来使用存储库。可以用`npm install -S aws-simplify`安装。

导入在应用程序的根入口点中生成的配置文件；在本演示中，我们将在 React 中使用 app.js:

```
import Amplify, { Storage } from 'aws-amplify'; 
import awsconfig from './aws-exports'; 
Amplify.configure(awsconfig);
```

如有必要，您也可以在`aws-export.js`中手动配置存储设置。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

## **结论**

Amplify 出现才几年，但发展非常迅速。事实上，亚马逊甚至将其移动中心推向了 Amplify。

使用 Amplify 有一些显著的优势，其中许多都与利用 AWS 无服务器技术的能力有关。Amplify 还使提升 CI/CD 变得简单明了。

最后一点:Amplify 在 CLI 中工作得最好，并允许开发人员从他们选择的编辑器中快速推出 spa。但是，如果您不喜欢从 CLI 工作，Amplify 可能不是您的最佳选择。

AWS 服务的美妙之处在于，它们让开发人员专注于他们的代码，其他一切都由 AWS 负责。有了 amplifier，有大量 AWS 集成可供选择，让您以低成本和高效率构建和部署 web 和移动应用程序。

## 您是否添加了新的 JS 库来提高性能或构建新特性？如果他们反其道而行之呢？

毫无疑问，前端变得越来越复杂。当您向应用程序添加新的 JavaScript 库和其他依赖项时，您将需要更多的可见性，以确保您的用户不会遇到未知的问题。

LogRocket 是一个前端应用程序监控解决方案，可以让您回放 JavaScript 错误，就像它们发生在您自己的浏览器中一样，这样您就可以更有效地对错误做出反应。

[![LogRocket Dashboard Free Trial Banner](img/e8a0ab42befa3b3b1ae08c1439527dc6.png)](https://lp.logrocket.com/blg/javascript-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/javascript-signup)

[LogRocket](https://lp.logrocket.com/blg/javascript-signup) 可以与任何应用程序完美配合，不管是什么框架，并且有插件可以记录来自 Redux、Vuex 和@ngrx/store 的额外上下文。您可以汇总并报告问题发生时应用程序的状态，而不是猜测问题发生的原因。LogRocket 还可以监控应用的性能，报告客户端 CPU 负载、客户端内存使用等指标。

自信地构建— [开始免费监控](https://lp.logrocket.com/blg/javascript-signup)。