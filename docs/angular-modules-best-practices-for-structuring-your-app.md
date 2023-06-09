# 角度模块:构建应用程序的最佳实践

> 原文：<https://blog.logrocket.com/angular-modules-best-practices-for-structuring-your-app/>

曾几何时，前端开发很简单，因为大部分繁重的工作都在后端处理。今天，应用程序的大部分逻辑驻留在前端，需要支持可重用性、可扩展性和可维护性的智能架构。

在本教程中，我们将向您展示如何将您的应用程序组织成一系列更小的功能块或模块。我们将介绍构建一个结构良好的 Angular 应用程序的过程，该应用程序使用模块来加强关注点的分离和可重用性。我们还将回顾使用[角度模块](https://angular.io/guide/architecture-modules)的一些最佳实践。

以下是我们将要介绍的内容:

要继续学习，您应该在您的机器上安装 Node.js V10.x，并且事先掌握 Angular 的工作知识。

## 什么是角模？

在 Angular 应用程序中，模块是具有独立程序功能的小段代码。

对于 Angular 模块如何工作的一个简单例子，假设你正在构建一个电子商务网站或应用程序。您可能有一个用于购物车部分的模块，一个用于产品部分的模块，一个用于客户部分的模块，等等。每个模块代表你的电子商务应用程序的一个独立部分。

## 为什么在 Angular 中使用模块？

如果你没有全神贯注于适当的软件架构，你的应用程序的某些部分可能会变得混乱无序，使得重用代码或者孤立地测试任何一段代码变得更加困难。

模块使您能够:

*   独立于其他功能开发特定的功能
*   通过允许每个开发团队处理一个单独的特性，更容易地管理团队
*   明确定义当前模块运行所需的模块列表
*   逐步部署功能
*   构建可伸缩的应用程序
*   轻松编写测试

## 角度中的模块类型

Angular 中可用的模块类型包括根模块、核心模块、共享模块和功能模块。让我们更详细地看一下每种角度模块类型。

### 根模块

[根模块](https://angular.io/guide/bootstrapping)是角度应用中的主要模块。它由[A](https://blog.logrocket.com/6-useful-features-in-angular-cli-eb502bd95874/)ngular CLIas`AppModule`生成，并在应用程序启动时自举。

每隔一个角模直接或间接地依赖于根模。一个角度应用程序中只能存在一个根模块。

### 核心模块

核心模块通常包含在 Angular 应用程序中使用一次的组件，如导航栏、加载器、页脚等。该模块应在`AppModule`中全局加载。

### 共享模块

一个[共享模块](https://angular.io/guide/sharing-ngmodules)由可以在特性模块中重用的指令、管道和组件组成。

例如，假设您编写了一个自定义的网格或过滤器，可在 Angular 应用程序的多个模块中重用。您可以将这些模块标识为共享模块。

### 功能模块

[功能模块](https://angular.io/guide/feature-modules)包含您的角度应用的主要功能。

例如，如果您正在构建一个电子商务应用程序，您可能有特定的功能集，如订单、产品、购物车、客户等。将这些功能分解成功能模块有助于将应用程序划分成专用的区域。这使得多个开发人员或开发团队能够在不破坏应用程序其他部分的情况下处理独立的功能。

## 实际例子:构建电子商务应用程序

为了展示 Angular 模块在实践中是如何工作的，我们将带您了解在 Angular 中构建电子商务应用程序的过程。我们将使用 Angular CLI 工具来搭建我们的新项目。

要检查是否安装了 Angular CLI 工具，请在终端中运行以下命令:

```
ng --version
//or
ng v

```

使用以下命令安装 Angular CLI 工具:

```
npm install -g @angular/[email protected] 

```

现在，我们可以使用以下命令创建一个新的角度项目:

```
ng new angular-modules

```

这通常需要一些时间，取决于您的互联网连接。成功后，它会创建一个名为`angular-modules`的文件夹。导航到新创建的文件夹，使用以下命令启动应用程序:

```
ng serve

```

### 在 Angular 中创建模块

当设置一个新的 Angular 应用程序时，第一步是定义应用程序需要的所有特性，然后将这些特性分成我们前面讨论过的模块类型。然后，可以用组件、指令、管道和服务来填充每个模块，这些组件、指令、管道和服务赋予模块所代表的特性以形状。

要创建模块，请运行以下 Angular CLI 命令:

```
ng generate module products

```

在 Angular 项目的根目录下，运行上面的命令来创建一个产品模块。这将在 app 目录中创建一个产品目录。产品目录包含一个负责定义模块的 TypeScript 文件。

### 向模块注册组件

要创建组件并同时在模块中注册它，请使用以下 Angular CLI 命令:

```
ng generate component products/productItem --module=products

```

`--module`标志表示组件注册到的模块——在我们的例子中是`products`模块。它也可以在生成指令和管道时使用。

创建组件时，Angular CLI 在创建文件夹和文件时会将 camel case 转换为 kebab case。因此，`productItem`组件的文件夹及其对应的文件从`productItem`转换为`product-item`。

用以下内容更新`products/product-item/product-item.component.html`:

```
<h2>Hello, Product-item component is working!</h2>

```

### 访问模块

在上一节中，我们在一个单独的模块中创建了我们的`productItem`组件。现在，让我们在浏览器中呈现这个组件。

默认情况下，`App`组件呈现在浏览器中。对于要在浏览器中呈现的`productItem`组件，它必须由`App`组件访问。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

首先，产品模块必须导出`productItem`组件，以便它可以在任何需要它的角度模块中使用。我们可以通过`ProductsModule`的导出属性来实现这一点。

用以下内容更新`products.module.ts`文件:

```
//src/apps/products/products.module.ts
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { ProductItemComponent } from './product-item/product-item.component';

@NgModule({
  declarations: [ProductItemComponent],
  imports: [
    CommonModule
  ],
  exports: [ProductItemComponent]
})
export class ProductsModule { }

```

这样，任何 Angular 模块现在都可以导入`ProductsModule`来访问其所有导出的模块、组件、指令和管道。

按如下方式更新`app.component.html`文件:

```
<app-product-item></app-product-item>

```

接下来，使用`AppModule`的导入属性在`AppModule`中全局注册`ProductModule`。

按如下方式更新`app.module.ts`文件:

```
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent } from './app.component';
import {ProductsModule} from "./products/products.module"
@NgModule({
  declarations: [
    AppComponent,
  ],
  imports: [
    BrowserModule,
    ProductsModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }

```

现在，您可以在应用程序的任何地方访问`ProductItemComponent`。

如果您尝试使用`ng serve`运行我们的 Angular 应用程序，您将得到以下结果:

```
Hello, Product-item component is working!

```

就是这样！下一步，您可以通过为客户、订单和购物车特性创建模块及其相应的组件来进行更多的练习。

## 结论

在本教程中，我们探索了 Angular 模块，回顾了 Angular 中的模块类型，并展示了使用模块构建 Angular 应用程序的一些好处。

Angular 模块使您能够改进应用程序的组织和结构。这样做可以提高代码的可重用性和可测试性，并在扩展 Angular 应用程序时带来出色的开发体验。

## 像用户一样体验 Angular 应用程序

调试 Angular 应用程序可能很困难，尤其是当用户遇到难以重现的问题时。如果您对监视和跟踪生产中所有用户的角度状态和动作感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/angular-signup)

.

[![LogRocket Dashboard Free Trial Banner](img/2794ac39244976f37c4941d9a910be23.png)](https://lp.logrocket.com/blg/angular-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/angular-signup)

LogRocket 就像是网络和移动应用程序的 DVR，记录你网站上发生的一切，包括网络请求、JavaScript 错误等等。您可以汇总并报告问题发生时应用程序的状态，而不是猜测问题发生的原因。

LogRocket NgRx 插件将角度状态和动作记录到 LogRocket 控制台，为您提供导致错误的环境，以及出现问题时应用程序的状态。

现代化调试 Angular 应用的方式- [开始免费监控](https://lp.logrocket.com/blg/angular-signup)。