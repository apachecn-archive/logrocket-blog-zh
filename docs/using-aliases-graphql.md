# 在 GraphQL 中使用别名

> 原文：<https://blog.logrocket.com/using-aliases-graphql/>

如果您已经将 GraphQL 用于您的生产应用程序，那么您可能会遇到这样的场景:您需要您的[定制查询来返回定制字段名称](https://blog.logrocket.com/graphql-queries-in-simple-terms/)。或者，您可能在同一个服务器响应中收到了两个查询结果，导致字段名称冲突。

在这些情况下，我们可以使用 GraphQL 别名来改进我们的查询。在本教程中，我们将介绍什么是别名，它是如何工作的，以及何时应该使用它。我们将研究几个场景，并用一个相关的例子来扩展这个概念。我们开始吧！

## 什么是 GraphQL 别名？

别名允许我们重命名查询结果中返回的数据。别名不会改变原来的模式，相反，它们操纵从数据库中提取的查询结果的结构，根据您的规范显示它。

当您想提高查询效率时，别名就派上了用场。例如，假设您正试图通过应用各种过滤器来获取相同的数据。许多开发人员倾向于编写单独的查询，并在独立的 API 调用中执行它们。在这种情况和其他情况下，别名可以帮助您优化和改进查询的组织。

在几种情况下，您可能希望更改接收查询结果的字段的名称。让我们来看两个不同的例子。

### 在一个查询中提取多个对象

使用别名，您可以在单个 GraphQL 查询中组合对同一对象的多次提取。

假设您正在构建一个在提要中显示帖子列表的应用程序。应用程序中的典型帖子看起来像下面的代码块:

```
type Post {
  id: string
  parent: string
  type: string # POST or COMMENT
  author: string
  title: string
  text: string
  createdAt: string
}
```

为了简单起见，我们将每个字段的数据类型限制为 string。理想情况下，您应该为`type`字段使用 enum，为`createdAt`字段使用定制的日期标量。

注意，我们定义了一个`type`字段，以适应同一表格中不同类型的帖子。在我们的例子中，我们有帖子和评论。

对`getPosts`的查询看起来像下面的代码块:

```
query getPosts {
  posts {
    id
    parent
    type
    author
    title
    text
    createdAt
  }
}
```

现在，假设您的数据库中有一篇文章和一条评论。如果您运行上面的查询，您将得到类似于下面代码块的响应:

```
{
  "data": {
    "posts": [
      {
        "id": "s9d8fhsd-fsdf",
        "parent": null,
        "type": "POST",
        "author": "Korg",
        "title": "Let's start a revolution",
        "text": "Hey, man. I'm Korg. We're gonna get outta here on that big spaceship. Wanna come?",
        "createdAt": "11:06 AM IST, Aug 7 2021"
      },
      {
        "id": "d8g6dffd-jfod",
        "parent": "s9d8fhsd-fsdf",
        "type": "COMMENT",
        "author": "Loki",
        "title": null,
        "text": "Well, it seems that you are in dire need of leadership.",
        "createdAt": "11:09 AM IST, Aug 7 2021"
      }
    ]
  }
}
```

你的数据库也可能包含一长串帖子和评论，就像上面的例子一样。您可以尝试编写一个查询，在单独的帖子和评论列表中获取帖子:

```
query getPosts {
  posts(type: "POST") {
    id
    author
    title
    text
    createdAt
  }
  posts(type: "COMMENT") {
    id
    parent
    author
    text
    createdAt
  }
}
```

但是，您会意识到该查询根本不运行，它会抛出以下错误:

```
{
  "errors": [
    {
      "message": "Fields 'posts' conflict because they have differing arguments. Use different aliases on the fields to fetch both if this was intentional.",
      "locations": [
        {
          "line": 2,
          "column": 3
        },
        {
          "line": 9,
          "column": 3
        }
      ]
    }
  ]
}
```

当执行时，查询返回相同字段名`posts`的两个结果列表。使用别名，我们可以将字段名更改为两个不同的关键字，从而使查询完美运行:

```
query getPosts {
  posts: posts(type: "POST") {
    id
    author
    title
    text
    createdAt
  }
  comments: posts(type: "COMMENT") {
    id
    parent
    author
    text
    createdAt
  }
}
```

现在，您可以在同一个 GraphQL 查询中轻松地进行多个 fetch 调用，从而节省网络使用并降低代码复杂性。

### 为您的查询结果添加含义

不言自明的名称使任何开发人员都能很好地理解您的代码。数据库结果也是如此。您需要确保处理 API 响应的开发人员非常了解数据的含义。让我们学习如何给查询结果添加含义，以帮助提供有用的命名约定。

让我们重新考虑一下我们之前的例子。我们的模式看起来像下面的代码块:

```
type Post {
  id: string
  type: string # POST or COMMENT
  author: string
  title: string
  text: string
  createdAt: string
}
```

该模式包含一个父字段，用于将评论链接到帖子，定义哪个帖子是评论的父。虽然这对于数据库工程师来说很有意义，但对于前端工程师来说可能不会马上清楚。

在这种情况下，您可以构建传统的查询来更好地描述父属性:

```
query getPosts {
  posts {
    id
    parentPost: parent
    type
    author
    title
    text
    createdAt
  }
}
```

您可以随意重命名其他属性。在执行查询后返回的结果中，原始名称将被完全覆盖。典型的响应类似于下面的代码块:

```
{
  "data": {
    "posts": [
      {
        "id": "s9d8fhsd-fsdf",
        "parentPost": null,
        "type": "POST",
        "author": "Korg",
        "title": "Let's start a revolution",
        "text": "Hey, man. I'm Korg. We're gonna get outta here on that big spaceship. Wanna come?",
        "createdAt": "11:06 AM IST, Aug 7 2021"
      },
      {
        "id": "d8g6dffd-jfod",
        "parentPost": "s9d8fhsd-fsdf",
        "type": "COMMENT",
        "author": "Loki",
        "title": null,
        "text": "Well, it seems that you are in dire need of leadership.",
        "createdAt": "11:09 AM IST, Aug 7 2021"
      }
    ]
  }
}
```

## GraphQL 别名最佳实践

虽然在 GraphQL 中实现别名相当简单，但是使用不当会对应用程序造成严重破坏。要优化 GraphQL 别名的结果，请务必记住以下几点。

### 自明命名

因为别名允许您更改结果的字段名称，所以确保您选择的名称对实际数据有意义是很重要的。

尽管选择一个方便、简短且易于理解的名称很有诱惑力，但您也必须考虑到其他开发人员可能会在您之后从事该项目的工作。除非您的命名约定是不言自明的，否则他们可能很难理解您的代码，这可能会在您的项目中导致错误。

例如，下面的代码包含一个命名不当的别名示例:

```
query getPosts {
  p: posts(type: "POST") {
    id
    author
    title
    text
    createdAt
  }
  c: posts(type: "COMMENT") {
    id
    parent
    author
    text
    createdAt
  }
}
```

将两个中间查询结果命名为帖子的`p`和评论的`c`似乎很方便，但是，让我们看看上面查询的结果:

```
{
  "data": {
    "p": [
      {
        "id": "s9d8fhsd-fsdf",
        "author": "Korg",
        "title": "Let's start a revolution",
        "text": "Hey, man. I'm Korg. We're gonna get outta here on that big spaceship. Wanna come?",
        "createdAt": "11:06 AM IST, Aug 7 2021"
      },
    ],
    "c": [
      {
        "id": "d8g6dffd-jfod",
        "parent": "s9d8fhsd-fsdf",
        "author": "Loki",
        "text": "Well, it seems that you are in dire need of leadership.",
        "createdAt": "11:09 AM IST, Aug 7 2021"
      }
    ]
  }
}
```

如您所见，没有关于结果类型的上下文信息，很难理解`p`和`c`的含义。因此，在这种情况下，坚持使用全名`posts`和`comments`会是更好的选择。

### 仅在需要时使用别名

别名只是一个现有字段的假名，所以即使不一定需要它们，也很容易使用它们。但是，重命名会导致代码中出现不必要的映射。例如，让我们以我们的`getPosts`查询为例:

```
query getPosts {
  posts {
    id
    parent
    type
    author
    title
    text
    createdAt
  }
}
```

`posts`定义中的每个字段都是不言自明的。但是，您可能希望为每个字段添加更强的描述符，如下所示:

```
query getPosts {
  posts {
    postId: id
    parentPost: parent
    postType: type
    author
    title
    postContent: text
    createdAt
  }
}
```

请记住，每次重命名时，您都需要在代码中到处传播新名称。在添加别名之前，您应该考虑附加细节的有用性。我建议只有在解决重复出现的问题时才使用别名。

### 进行服务器端更改

如果您发现自己在重复重命名相同的字段，您可能需要考虑删除别名并在服务器上重命名字段。类似地，如果您必须在客户机上频繁使用别名，您可能需要减少数据库模式。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

别名是用来快速重命名事物的一种方便的方法，但是，你不应该过分依赖它们。如果您发现自己经常使用别名，那么您的服务器端命名约定可能有问题。在这种情况下，使用别名只会使代码库更加复杂。

## 结论

别名是重新组织 GraphQL 查询结果的好方法。当您的后端数据模型和前端数据规范不完全匹配，并且您需要手动调整它们时，别名尤其有用。

如果您需要一次在同一个源上获取多个查询结果，别名是必需的。然而，尽量少用别名也很重要。虽然别名大多是无害的，并且只在数据被返回后才改变数据，但是不必要的使用会引起数据模式的混乱，甚至导致错误。

我希望你喜欢这个教程！

## 监控生产中失败和缓慢的 GraphQL 请求

虽然 GraphQL 有一些调试请求和响应的特性，但确保 GraphQL 可靠地为您的生产应用程序提供资源是一件比较困难的事情。如果您对确保对后端或第三方服务的网络请求成功感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/graphql-signup)

.

[![](img/432a3823c85b3fb72a206e6236a29f48.png)![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/graphql-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/graphql-signup)

LogRocket 就像是网络和移动应用的 DVR，记录下你网站上发生的每一件事。您可以汇总并报告有问题的 GraphQL 请求，以快速了解根本原因，而不是猜测问题发生的原因。此外，您可以跟踪 Apollo 客户机状态并检查 GraphQL 查询的键值对。

LogRocket 检测您的应用程序以记录基线性能计时，如页面加载时间、到达第一个字节的时间、慢速网络请求，还记录 Redux、NgRx 和 Vuex 操作/状态。

[Start monitoring for free](https://lp.logrocket.com/blg/graphql-signup)

.