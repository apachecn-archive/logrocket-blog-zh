# GraphQL vs. gRPC vs. REST:选择正确的 API 

> 原文：<https://blog.logrocket.com/graphql-vs-grpc-vs-rest-choosing-right-api/>

前端和后端之间的数据交换标准一直有点争议。

由于有不同的 API 技术可用于在客户机和服务器之间共享数据，并且每种技术都有自己独特的功能，因此很难决定哪种技术最适合您。

目前创建 API 最流行的三种技术是 GraphQL、gRPC 和 REST。在这篇文章中，我们将看看每一个是如何工作的，包括它们的优缺点。

## GraphQL

GraphlQL 是一种数据查询语言，它允许客户请求他们需要的任何特定数据。与 REST 的 HTTP 方法相反，GraphQL 使用查询、变异和订阅来获取和操作数据。

查询从服务器请求数据，而突变向服务器发送数据并修改由服务器控制的数据。当数据改变时，订阅通常通过 [Websockets](https://blog.logrocket.com/websockets-tutorial-how-to-go-real-time-with-node-and-react-8e4693fbf843/) 获得实时更新。

总之，GraphQL 支持 JavaScript、Java、Python、Ruby、PHP 等语言。

### 用模式表示数据

使用 GraphQL，[数据通过模式](https://blog.logrocket.com/3-annotations-to-use-in-your-graphql-schema/)表示。模式定义一个对象、该对象包含的字段以及这些字段可以保存的数据类型。下面是一个模式示例:

```
type User {
  name: String!
  age: Number!
}

```

这里，`User`是对象，在里面，我们指定对象拥有的字段或者属性，包括数据类型。

这意味着在对`User`对象进行的任何查询中，唯一能出现的字段是`name`和`age`。对于这两个字段，唯一可以写入或读取的数据分别是字符串和数字。

在 GraphQL 中，当字段类型后面有感叹号时，就像上面那样，这意味着字段不可为空。当对象更新时，必须向它们写入数据，并且这些字段在被查询时总是提供值。

### 用模式定义查询、变异和订阅

如前所述，主要有三种可以在数据对象上执行的操作:查询、突变和订阅。这些也可以使用 GraphQL 的模式语言来定义。

使用`query`模式，您可以通过指定感兴趣的对象和对象下的字段从 GraphQL 服务器请求数据:

```
query {

  users {
    name
    age
  }

}

```

上面，我们请求数据库中的用户列表，特别是他们的姓名和年龄。例如，`user`对象可以有其他字段，如`gender`或`job title`，但是只有我们请求的字段会被发送给我们。

这是 GraphQL 优于 REST 的一个优势。在 REST 中，当查询一个端点时，该端点后面的所有数据都发送给您，让您对数组进行排序并选择您需要的特定字段。

GraphQL 更加灵活，因为它允许您定制结果的结构。这为您节省了用于过滤数据的内存和 CPU 消耗。

而且，因为 GraphQL 查询通常请求较少的数据，所以它们通常比 REST 请求处理得更快。

[使用](https://blog.logrocket.com/robust-graphql-mutations-the-relay-way/) `[mutation](https://blog.logrocket.com/robust-graphql-mutations-the-relay-way/)` [模式](https://blog.logrocket.com/robust-graphql-mutations-the-relay-way/)，您可以对现有数据进行更改或者向您的数据库提交新数据。例如，如果您想创建一个新用户或者更新一个现有用户的详细信息，您可以使用`mutation`:

```
mutation {

  newUser($name: name!, $age: age!) {

    name

    age

  }

}

```

使用`subscription`模式，您可以订阅一个对象，并让您的服务器在该对象发生变化时实时更新您的客户机。

例如，如果您希望在删除或修改现有用户或创建新用户时实时更新客户端应用程序，您可以订阅`User`对象和您感兴趣的字段:

```
subscription User {

  newUser {
    name
    age
  }

}

```

### GraphQL pros

使用 GraphQL，您可以定义每个实例中所需数据的确切范围。通过防止过量的数据从服务器发送到客户端，您的应用程序可以执行得更好。

由于您可以在一个查询中从不同的资源中选择多个字段，GraphQL 消除了多次往返服务器为您的客户机获取数据的需要。

有了这么多第三方代码生成器，很容易为一个平台编写 GraphQL 代码，并使用这些生成器为第二个平台自动创建它。您可以在 [GraphQL 代码生成器](https://www.graphql-code-generator.com)探索各种可能性。

由于您可以定义希望服务器在前端响应的确切数据，因此您可以在服务器端向数据模型添加新字段，而无需创建新的版本化 API。这也导致更少的前端代码中断，仅仅是因为您向模型添加了一个新的字段并且忘记了更新前端代码。

### 图表一致性

GraphQL 有一些严重的[缓存问题](https://travislsmith.com/caching-problems-with-graphql-at-the-edge/#:~:text=are%20these%20problems%3F%3F-,Caching%20Problems%20with%20GraphQL,-There%20are%20three)(特别是 HTTP 缓存)，这些问题阻碍了它的广泛采用。

由于 GraphQL 查询的编写方式，一次请求太多嵌套字段会导致循环查询并使服务器崩溃。在 REST 中实现限速更加无缝。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

GraphQL 也不支持开箱即用的文件上传，需要一种变通方法来实现这一功能。

与 REST 相比，GraphQL 对不同 web/应用程序框架的支持仍然不稳定。JS 生态系统得到了最多的支持，但情况正在发生变化，对其他语言和框架的支持也在日益改善。

最后，与 REST 相比，GraphQL 的学习曲线更加陡峭。

## 休息

列表中最流行和使用最多的 [API 格式是 REST](https://blog.logrocket.com/10-best-practices-for-rest-api-design/) ，代表具象状态转移。

REST APIs 使用 HTTP 方法如`GET`、`POST`、`PUT`和`DELETE`来访问和操作统一资源标识符(URIs)中的数据:

*   方法从服务器检索数据
*   `POST`方法将数据从客户端传输到服务器
*   `PUT`方法修改服务器中的数据
*   `DELETE`方法擦除服务器中的数据。

例如，在使用时，`GET`请求可以在看起来像`/api/users`的 URI 上执行；一个`REST`操作通常将数据从服务器返回到客户端。

然后，REST 响应数据可以以 JSON、CSV、XML 或 RSS 等格式出现，REST 响应数据往往以属性/值对的形式出现:

```
{
  "users": {
    {
      "id": "1",
      "name": "Dan",
      "age": "20"
    },
    {
      "id": "2",
      "name": "Kim",
      "age": "19"
    }
  }
}

```

### 休息专家

REST 中的缓存很简单，因为它利用了 HTTP 的优势，消除了客户端和服务器之间不断交互的需要，从而提高了性能和可伸缩性。

REST 在科技界的成熟是其最大的优势之一。它无处不在，与 GraphQL 和 gRPC 相反，它们可以被认为是创建 API 的利基解决方案。

由于 REST 的流行，许多开发人员已经熟悉了它，并发现它很容易使用。因此，如果您创建一个供许多第三方开发者使用的商业 API，那么 REST 可能是您的最佳选择。

与 GraphQL 相比，监控和限制基于 REST 的 API 更容易，因为每个资源通常位于唯一的端点之后。因此，个人资源也很容易被锁定在付费墙后面。

### 休息缺点

大有效载荷；在 GraphQL 中，您可以选择资源下的特定字段，REST 要求您请求资源下的所有数据，然后遍历资源以获得您需要的数据，这通常会导致很大的负载。您可以通过创建一个新的端点或者添加基于查询参数的服务器端过滤来解决这个问题，但是对于每个用例都这样做是不可行的。

而使用 GraphQL，我们可以批处理不同的资源请求，并在`/graphql`将它们发送到同一个端点，并在一次往返中执行一次，而使用 REST，不同的请求必须发送到不同的端点，从而导致多次往返。

GraphQL 使得修改数据库模式变得非常容易。由于查询只涉及请求特定的字段，因此可以从资源中删除新字段，而不会导致重大更改，也不会要求您对应用程序进行版本控制。

gRPC 还具有自己的向后兼容特性，使得在不破坏客户代码的情况下更新数据结构变得容易。休息没有这种灵活性。

## gRPC

gRPC 是一个高性能的开源远程过程调用(RPC)框架，由 Google 创建。

[在 gRPC 中，协议缓冲区](https://blog.logrocket.com/how-to-build-a-grpc-server-in-dart/#howprotocolbufferswork)向服务器发出 API 请求。这些 API 请求和响应是协议缓冲区消息，每个都必须使用协议缓冲区语言定义自己的类型。

服务被定义为将 API 请求与其对应的响应配对，协议缓冲编译器为您的 API 服务生成服务器和客户端代码构件:

```
// Syntax version
syntax = "proto3";

service UserService {
    rpc GetUser (UserRequest) returns (UserResponse);
}

// Message type
message UserResponse {
    string name = 1;
    string age = 2;
}

message UserRequest {
    string name = 1;
    string age = 2;
}

```

在上面的代码块中，消息 UserRequest 是请求，消息 UserResponse 是响应，GetUser 是 API 端点。

gRPC 支持多种语言，您的客户端应用程序可以用其中任何一种语言编写，因为任何这些语言的客户端代码都可以在`protoc`工具的帮助下生成。到目前为止，gRPC 支持以下现成的语言:C#、C++、Dart、Go、Java、Kotlin、Node、Objective-C、PHP、Python 和 Ruby。

### gRPC pros

gRPC 自带的协议缓冲区比 JSON 支持更多的数据类型，并且由于其优化的二进制格式，速度明显更快。

gRPC 支持开箱即用的全双工流，这使得它适用于视频和语音通话等功能。另一方面，REST 一次处理一个查询。

gRPC 执行负载平衡，使客户机请求均匀地分布在服务器上，以避免特定服务器过载。这提高了应用程序的整体性能。

负载平衡特性，以及 gRPC 默认使用 HTTP2 的事实，使得 gRPC 中的延迟比 REST APIs 中的低得多。

gRPC 被设计成向后兼容的，所以向 gRPC 服务添加新的字段/方法通常不会破坏现有的客户端。这可以确保您在修改服务时不必更新所有现有的客户端。

最后，gRPC 以二进制格式(协议缓冲区)序列化数据，比 REST 的 JSON 快很多，让 gRPC 有了更大的性能优势。

### gRPC 缺点

gRPC 的官方数据格式 Protocol buffers 目前只支持少数几种语言。它不支持像 R 和 Swift 这样的语言。REST 使用的 JSON 几乎被所有语言支持。

gRPC 没有现成的浏览器支持，尽管有一些解决方法。您必须使用 gRPC-Web，它是 gRPC 的 Web 扩展，基于 HTTP 1.1。它没有提供所有的 gRPC 特性，但是如果你想在浏览器中使用 gRPC，它是一个很好的桥梁。

## gRPC 与 REST 和 GraphQL 的比较

| 特征(Feature) | GraphQL(图形 SQL) | 休息一下 | gRPC 号指南 |
| 取数据 | 高效获取数据。只会从服务器获取所需的数据。 | 除非在服务器端定义了新的端点/查询过滤器，否则服务器可能会返回额外的数据。 | 除非在服务器端定义了新的端点/过滤器，否则服务器可能会返回额外的数据。 |
| HTTP 1.1 与 HTTP 2 | 遵循请求-响应模型。它可以使用任何一个 HTTP 版本，但通常是用 HTTP 1.1 构建的。 | 遵循请求-响应模型。它可以使用任何一个 HTTP 版本，但通常仍然是用 HTTP 1.1 构建的。 | 遵循客户端响应模型，基于 HTTP 2。一些服务器有变通办法使它与 HTTP 1.1 一起工作，但这不是默认的。 |
| 浏览器支持 | 无处不在。 | 无处不在。 | 支持有限。需要使用 gRPC-Web，这是 gRPC 对 Web 的扩展，基于 HTTP 1.1。 |
| 有效载荷数据结构 | 使用基于 JSON 的有效负载发送/接收数据。 | 主要使用基于 JSON 和 XML 的有效负载来发送/接收数据。 | 默认情况下，使用协议缓冲区来序列化负载数据。 |
| 代码生成 | 需要使用 GraphQL 代码生成器等第三方工具来生成客户端代码。文档和互动平台可以通过使用 GraphiQL 生成。 | 需要使用 Swagger 等第三方工具生成客户端代码。 | gRPC 为各种目标语言的代码生成提供了本地支持。 |
| 请求缓存 | 默认情况下很难缓存请求，因为每个请求都发送到同一个端点。 | 易于在客户端和服务器端缓存请求。大多数客户机/服务器本身支持它。 | 默认情况下不支持请求/响应缓存。 |

## 结论

归根结底，对于您的特定项目来说，最好的 API 技术将取决于您想要实现的目标。如果您需要一个通用的 API 供许多客户端使用，那么 REST 可能是您最好的选择。

如果您需要一个灵活的 API，让不同的客户机使用它来发出许多独特的请求，那么最好让客户机创建它们自己独特的查询，并且只快速获得它们需要的特定数据，您可以使用 GraphQL 来实现这一点。

如果想要在内部服务之间创建快速、无缝的通信，那么 gRPC 是您的最佳选择。

正如我们所见，这三种技术各有利弊，您可以通过结合技术来获得最佳结果。

## 监控生产中失败和缓慢的 GraphQL 请求

虽然 GraphQL 有一些调试请求和响应的特性，但确保 GraphQL 可靠地为您的生产应用程序提供资源是一件比较困难的事情。如果您对确保对后端或第三方服务的网络请求成功感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/graphql-signup)

.

[![](img/432a3823c85b3fb72a206e6236a29f48.png)![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/graphql-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/graphql-signup)

LogRocket 就像是网络和移动应用的 DVR，记录下你网站上发生的每一件事。您可以汇总并报告有问题的 GraphQL 请求，以快速了解根本原因，而不是猜测问题发生的原因。此外，您可以跟踪 Apollo 客户机状态并检查 GraphQL 查询的键值对。

LogRocket 检测您的应用程序以记录基线性能计时，如页面加载时间、到达第一个字节的时间、慢速网络请求，还记录 Redux、NgRx 和 Vuex 操作/状态。

[Start monitoring for free](https://lp.logrocket.com/blg/graphql-signup)

.