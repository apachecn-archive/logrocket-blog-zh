# 用于 TypeScript 模式构建的 pothos vs . TypeGraphQL-log rocket 博客

> 原文：<https://blog.logrocket.com/pothos-vs-typegraphql-for-typescript-schema-building/>

在本帖中，我们将通过以下几节来比较两个模式构建器(Pothos 和 TypeGraphQL)如何帮助开发人员为他们的服务构建 GraphQL 模式，以及如何使用 TypeScript 和这些工具实际构建模式:

在文章的最后，我们将比较这两种工具提供的特性和一些用例。

### GraphQL 模式构建入门

GraphQL 模式由对象类型和指定它们可以接受的数据类型的字段组成。这些类型被称为标量类型，它们内置于 SDL 核心中。一旦我们定义了一个 GraphQL 模式，只有在对象中定义的指定字段及其各自的类型才能被查询或改变。

在 GraphQL 模式中，我们必须在创建模式时定义查询和变异类型，变异类型除外，它并不总是强制的。这两种类型定义都基于预定义的模式定义了我们对 GraphQL 服务或 API 进行的每个查询的入口点。你可以在博客的其他地方阅读更多关于 GraphQL 中模式构建的内容。

## 什么是坑爹？

Pothos 是一个插件，它提供了一种用 GraphQL 和 TypeScript 创建和构建模式的简单方法。

因为它是基于 TS 的，所以它提供了 GraphQL 模式构建所必需的类型安全。它还建立在 TypeScript 强大的类型系统和类型推断之上，不需要代码生成或到处使用手动类型。

Pothos 模式构建成一个普通模式，使用来自`graphql`包的类型。这意味着它应该与 Node.js 的大多数流行的 GraphQL 服务器实现兼容。在本指南中，我们将使用`@graphql-yoga/node`来运行我们的示例，但是您可以使用您想要的任何服务器。

## 有坑洞的图式建构

在 Pothos 中，通常从数据的形状(定义为类型、接口、类、Prisma 模型等)开始。)然后定义一个使用该数据的 GraphQL 类型，但不一定要符合它的形状。在较大的应用程序中，这种方法感觉起来更自然，因为在这些应用程序中，您有真实的数据，而不是纯粹为 GraphQL API 创建的。

除了其第一手的类型安全的优势——独立于装饰者——pottos 以提供大量以插件形式公开的功能而自豪，这包括一个大型的插件生态系统。Pothos 的主要优势之一是 GraphQL API 和数据在模式中的内部表示方式的分离，这一点我们将在后面看到。

让我们从 [Pothos 文档](https://pothos-graphql.dev/#hello-world)中的一个例子开始:从“Hello，World！”构建一个简单的模式 app。

```
import { createServer } from '@graphql-yoga/node';
import SchemaBuilder from '@pothos/core';

const builder = new SchemaBuilder({});

builder.queryType({
  fields: (t) =&gt; ({
    hello: t.string({
      args: {
        name: t.arg.string(),
      },
      resolve: (parent, { name }) =&gt; `hello, ${name || 'World'}`,
    }),
  }),
});

const server = createServer({
  schema: builder.toSchema({}),
});

server.start();

```

在这个例子中，我们用`graphl-yoga/node`和 Pothos 模式构建器创建了一个简单的样板服务器。我们从 Pothos core 导入了模式构建器，并实例化了一个新的模式构建器，它构建了一个 GraphQL 语言能够理解的简单模式。

之后，我们以一种类型安全的方式用我们的字段类型和参数设置查询构建器。在对传递给查询的字段参数和查询本身进行了所有必要的验证之后，当查询执行时，解析器负责返回响应。

最后，我们将构建的模式传递给`createServer`函数并调用`server.start`方法。

有了 Pothos，我们可以用对象类型的形式定义数据的结构，这让我们知道底层数据类型的细节。之后，我们可以继续定义类型，在这里我们传递我们定义的结构，作为验证实际类型的一种方式。

因此，基本上，我们需要一种传递关于底层数据结构的类型信息的方法，以便我们的字段知道对象类型上有哪些可用的属性。

在[类型推断](https://blog.logrocket.com/understanding-infer-typescript/)的帮助下，我们可以确认何时在对象类型上传递了错误的字段，并确保`objectType`符合我们的类型定义，因为对象可以告诉我们期望什么类型。基于模式中定义的[字段](https://pothos-graphql.dev/docs/guide/fields#fields)，我们可以确定可用数据的性质及其类型。这意味着我们想要添加到模式中的任何数据都必须明确定义。

## 如何定义坑洞中的物体

在 Pothos 中有三种定义对象的方法:使用类、模式类型和引用。

定义 Pothos 类和定义常规类是一样的——我们构造数据并初始化类。请参见下面的示例:

```
export class Person {
  name: string;
  age: number;
  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }
}

```

在定义了类之后，我们可以在上面的类中映射字段的类型。这是通过使用 Pothos [字段构建器](https://pothos-graphql.dev/docs/api/field-builder)来验证上面模式类中的对象类型来完成的。看看下面我们如何做到这一点。

<

pre class = " language-graph QL hljs > const builder = new schema builder({ })；

builder.objectType(Person，{
name: 'Person Schema '，
description: "A person schema "，
fields:(t)=&gt；({})，
})；

`objectParam`参数`Person`表示我们的初始类，它作为一个蓝图，用来验证我们可以根据这个蓝图为每个单独的属性传递的类型。我们这样做是为了当我们在字段中使用这些属性时，我们可以确保它们代表正确的类型。

在上面的 field 对象的帮助下，我们可以继续定义我们的模式中的数据类型。下面让我们看看我们是如何做到的:

```
builder.objectType(Person, {
  name: 'Person Schema',
  description: 'A person schema',
  fields: (t) =&gt; ({
    nameNew: t.string({
    resolve: (parent) =&gt; {
        const name = console.log(parent.name)
      },
    }),
    ageNew: t.int({
      resolve: (parent) =&gt; {
         const age = console.log(parent.age)
      },
    }),
  }),
});

```

正如我们所看到的，我们无法直接访问模式中定义的属性。这是特意设计的，以确保我们只能从底层模式访问属性。

请注意，父 arg 将是 schema 类中指定的当前类型的支持模型的值。

然而，为了直接访问模式或模型中定义的字段属性，我们可以使用`expose`，就像在文档中定义的[一样。](https://pothos-graphql.dev/docs/api/field-builder#expose)

```
exposeString(name, options)

```

注意:`name` arg 可以是支持模型中与所公开的类型相匹配的任何字段。

接下来，我们实际编写一个查询，在解析器的帮助下解析实际值，解析器是一个解析该字段值的函数。让我们用下面的坑洞创建这样一个查询。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

```
builder.queryType({
  fields: (t) =&gt; ({
    Person: t.field({
      type: Person,
      resolve: () =&gt; new Person('Alexander', 25),
    }),
  }),
});

```

下一步是创建一个简单的服务器，并将我们构建的模式传递给服务器，就像我们前面看到的那样。Pothos 可以很好地与任何流行的 GraphQL 服务器实现兼容。

最后，我们对服务器运行常规的 GraphQL 查询。

```
query {
  Person {
    name
    age
  }
}

```

## 坑洞特征

### 定义模式的几种方法

正如我们在上面所概述的，在创建或定义带有 Pothos 的对象类型时(这是一种提供关于模式中底层数据如何构造的类型信息的方式)，我们可以利用上面的类、模式类型甚至是引用。关于如何基于我们的用例使用它的更多信息可以在[文档](https://pothos-graphql.dev/docs/guide/objects#using-schematypes)中找到。

### 打印和生成模式文件

有了 Pothos，我们可以使用 [`graphql-code-generator`](https://www.graphql-code-generator.com/) 生成我们的模式文件。您还可以打印您的模式，这在您想要一个 SDL 版本的模式时非常有用。在这个场景中，我们可以使用`printSchema`或`lexicographicSortSchema`，两者都可以从 GraphQL 包中导入。

### 生成类型

Pothos 没有内置的机制来生成供客户端使用的类型，但是可以配置 [graphql-code-generator](https://www.graphql-code-generator.com/) 来直接使用 TypeScript 文件中的模式。

### 壶背模型

Pothos 强制在外部 GraphQL API 的形状和数据的内部表示之间进行清晰的分离。为了帮助实现这种分离，Pothos 提供了一个支持模型，让您能够更好地控制如何定义模式和解析器使用的类型。

文档中详细解释了 Pothos 的[支持模型](https://pothos-graphql.dev/docs/guide/schema-builder#backing-models)。

### 基于插件的

Pothos 是基于[插件的](https://pothos-graphql.dev/docs/plugins)，它提供了像`simple-objects`、`scope-auth`和`mocks`这样的样例插件，让你的工作更容易。例如，`simple-objects`插件可以更快地构建一个图形，因为您不必为图形中的每个对象提供显式的类型或模型。

### 未电离的

Pothos 对代码是如何构造的没有偏见，并提供了做许多事情的多种方法。事实上，Pothos 甚至为如何组织你的文件提供了指导。

### Pothos SchemaBuilder API

要创建一个带有 Pothos 的模式，我们所要做的就是[从 Pothos core 导入`schemaBuilder`类](https://pothos-graphql.dev/docs/api/schema-builder)，如下所示。

```
import SchemaBuilder from '@pothos/core';
const builder = new SchemaBuilder&lt;{
  Context: {};
}&gt;({
  // plugins 
});

```

schema builder 帮助为我们的图形创建类型，并将创建的类型嵌入到 GraphQL 模式中。关于 [Pothos 模式构建器 API 设计](https://pothos-graphql.dev/docs/api/schema-builder)的详细信息可以在文档中找到。

### 对 ORM 的支持

虽然 Pothos 主要是一个模式构建器，但它也支持大多数 ORM，并与大多数 ORM 很好地集成，特别是通过 Pothos 的 Prisma 插件 Prisma。有了这个插件，我们可以很容易地定义基于 Prisma 的对象类型，同样，也可以定义基于 Prisma 模型的 GraphQL 类型。文档中的[给出了一个例子和设置。](https://pothos-graphql.dev/docs/plugins/prisma#example)

当然，这种集成的一个显著特点是支持强类型 API、自动查询优化(包括关系的`n + 1`查询问题)、支持基于同一数据库模式的许多不同的 GraphQL 模型，等等。文档[涵盖了一切](https://pothos-graphql.dev/docs/plugins/prisma#features)。

注:Prisma 也可以直接和 Pothos 集成。这个插件只是让我们更容易、更高效地使用这两种技术。关于如何执行此集成的[指南](https://pothos-graphql.dev/docs/plugins/prisma#using-prisma-without-a-plugin)包含更多信息。

## 什么是 TypeGraphQL？

TypeGraphQL 提供了一种不同的构建模式的方法。使用 TypeGraphQL，我们只使用类和装饰魔术来定义模式。它主要依赖于 [`graphql-js`](https://github.com/graphql/graphql-js) 、 [`class-validator()`](https://github.com/typestack/class-validator) 、`reflect-metadata`垫片，使得打字稿中的反射工作。`class-validator`是基于装饰器的类属性验证。

在之前的一篇文章中，我们广泛地讨论了用 TypeGraphQL 构建 GraphQL APIs，包括如何创建对象类型:

```
@ObjectType()
class Recipe {
  @Field()
  title: string;

  @Field(type =&gt; [Rate])
  ratings: Rate[];

  @Field({ nullable: true })
  averageRating?: number;
}

```

正如我们在上面看到的，我们通过定义类开始用 TypeGraphQL 定义模式，这些类充当我们模式的蓝图。下面我们来看一个例子。

首先，我们从创建类似于 SDL 中的类型开始。

```
type Person {
  name: String!
  age: Number
  dateofBirth: Date
}

```

接下来，我们可以继续创建类，它必须包含我们的`Person`类型的所有属性和定义的类型。

```
class Recipe {
  name: string;
  age?: number;
  dateofBirth: ate
}

```

我们利用 decorators 来设计类及其属性，就像这样:

```
@ObjectType()
class Person {
  @Field()
  name: string;

  @Field()
  age?: number;

  @Field()
  dateOfBirth: Date;
}

```

然后，我们创建我们所谓的`input`类型，我们需要它来执行我们的查询和突变。

```
@InputType()
class NewPersonInput {
  @Field()
  @MaxLength(40)
  name: string;

  @Field({ nullable: true })
  age?: number;

  @Field()
  dateOfBirth: Date;
}

```

包括`maxLength`在内的字段验证方法都来自`class-validator`库。在创建常规查询和变异之后，最后一步是构建模式，我们将把它传递给我们的 GraphQL 服务器。

```
const schema = await buildSchema({
  resolvers: [PersonResolver],
});

```

我们的`Person`类型的突变类型示例如下所示:

```
type Mutation {
  addNewPerson (newPersonData: NewPersonInput!): Person!
  deletePerson(id: ID!): Boolean!
}

```

## TypeGraphQL 功能

[TypeGraphQL 特性](https://typegraphql.com/docs/introduction.html)包括验证、授权等，帮助开发人员快速编写 GraphQL APIs，减少为所有参数和输入和/或对象类型创建 TypeScript 接口的需要。TypeGraphQL 还通过使用类和一点装饰器帮助来定义模式，从而帮助确保每个人都从一个真实的来源工作。这确实有助于减少代码冗余。

### 支持依赖注入

TypeGraphQL 通过允许用户提供将被框架使用的 IoC 容器来支持[依赖注入](https://typegraphql.com/docs/dependency-injection.html)。

### 严格验证

使用类验证库严格验证字段属性。TypeGraphQL 比 Pothos 更灵活，在我们可能需要以更灵活的方式声明某些字段的类型(如类型参数)的情况下，它支持泛型类型。

### 支持自定义装饰器

TypeGraphQL 支持自定义装饰器，包括方法和参数，这提供了一种减少样板代码和跨多个解析器重用公共代码的好方法。

### 对 ORM 的支持

TypeGraphQL 还对多个不同的第三方 ORM 提供了巨大的支持，包括 TypeORM 和 Prisma。通过 Prisma，TypeGraphQL 提供了与 [typegraphql-prisma 包](https://www.npmjs.com/package/typegraphql-prisma)的集成，我们可以在 npm 上找到它。

TypeGraphQL 已经提供了基于 Prisma 模式生成类型类和解析器的功能，这意味着我们不必编写太多代码来执行常规查询和变异。[文档](https://typegraphql.com/docs/prisma.html#overview)有设置这两种技术的例子，也有一个专门的[网站](https://prisma.typegraphql.com/)，其中包含更多的例子和教程，包括安装说明、配置等等。

## 结论

在这篇文章中，我们研究了两个非常棒的基于类型脚本的库的模式构建方法。尽管可以使用 Pothos 来构建基于 TypeScript 的 GraphQL APIs，但它主要作为模式构建器而大放异彩。

另一方面，TypeGraphQL 更加灵活，允许我们构建支持不同 ORM 的简单 GraphQL APIs。

我们已经介绍了在基于 Node.js/TypeScript 和 GraphQL 的 API 中构建模式的一些重要特性、用例以及方法。这篇文章的目的是向您展示这两个不同且独特的库是如何处理这些过程的，这样您就可以做出明智的决定，在未来的项目中使用下一个最好的工具。

## 监控生产中失败和缓慢的 GraphQL 请求

虽然 GraphQL 有一些调试请求和响应的特性，但确保 GraphQL 可靠地为您的生产应用程序提供资源是一件比较困难的事情。如果您对确保对后端或第三方服务的网络请求成功感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/graphql-signup)

.

[![](img/432a3823c85b3fb72a206e6236a29f48.png)![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/graphql-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/graphql-signup)

LogRocket 就像是网络和移动应用的 DVR，记录下你网站上发生的每一件事。您可以汇总并报告有问题的 GraphQL 请求，以快速了解根本原因，而不是猜测问题发生的原因。此外，您可以跟踪 Apollo 客户机状态并检查 GraphQL 查询的键值对。

LogRocket 检测您的应用程序以记录基线性能计时，如页面加载时间、到达第一个字节的时间、慢速网络请求，还记录 Redux、NgRx 和 Vuex 操作/状态。

[Start monitoring for free](https://lp.logrocket.com/blg/graphql-signup)

.