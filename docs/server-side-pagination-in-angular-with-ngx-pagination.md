# Angular 中的服务器端分页

> 原文：<https://blog.logrocket.com/server-side-pagination-in-angular-with-ngx-pagination/>

## 介绍

在本文中，我们将探讨服务器端分页，以及如何用 Angular 的 ngx-pagination 实现它。

在我们开始之前，让我们先看看我们将要涉及的内容，以及在继续深入之前您应该知道的内容。

### 先决条件

1.  获取 API 的基础知识
2.  角度的基本知识
3.  Visual Studio 代码
4.  Node.js 已安装

### 我们将学到什么

本文将涵盖以下内容:

1.  什么是服务器端分页？
2.  什么是 ngx 分页？
3.  安装 ngx 分页包

## 什么是服务器端分页？

服务器端分页是控制从客户端服务器获取的数据请求子集的一种方式，如果我们不想一次显示所有数据，这种方式很有用。服务器端分页需要客户端传入一个参数来获取相关信息。

为了在 Angular 中实现服务器端分页，我们必须使用一个名为 ngx-pagination 的 Angular 分页包管理器。

## 什么是 ngx 分页？

[**Ngx-pagination**](https://www.npmjs.com/package/ngx-pagination#demo) 是一个角度包管理器(或工具)，可用于服务器端分页。它允许 Angular 开发人员按页面查看数据，这对于以更易管理的片段接收数据非常有用。

Ngx-pagination 接受单个参数作为对象来确认分页实例接口。

在本教程中，我们将使用一个模拟的航空乘客数据库来试验服务器端分页。

让我们从运行以下命令开始:

```
npm install ngx-pagination --save

```

安装成功后，打开项目`app.module.ts`并导入`ngx-pagination`模块:

```
import {NgxPaginationModule} from 'ngx-pagination';

@NgModule({
declarations: [
AppComponent
],
imports: [
NgxPaginationModule,
],
export class AppModule { }

```

接下来，将`ngx-pagination`模块传递到导入部分:

```
import { HttpClient } from "@angular/common/http"
constructor( private http: HttpClient) {}

```

在`app.component.ts`中，我们首先导入了 Angular `httpsClients`模块，它允许我们与外部 API 交互。之后，我们进一步将它作为私有 http 进行传递，我们将在调用 API 时使用它。

现在让我们向服务器发出一个请求。

我们将订阅的 API 端点是免费的[即时 web 工具假 API](https://www.instantwebtools.net/fake-rest-api) ，它允许我们访问我们的假航空公司的乘客预订细节等等。我们正在使用即时 Webtool 伪 API，因为它支持分页，并且有超过一千个原始数据集。

在开始之前，让我们创建处理 API 请求的`getAllPassengerData()`函数:

```
ngOnInit() {
this.getAllPassengerData();
}

getAllPassengerData() {
this.http.get("https://api.instantwebtools.net/v1/passenger").subscribe((data: any) => {
console.log(data);
})
};

```

在上面的函数中，我们在`ngOnInit`生命周期钩子中传递了`getAllPassengerData()`函数。`ngOnInit`所做的是在每次加载组件时，或者每次组件在视点上时，加载分配给它的任何代码。

现在，在函数内部，我们订阅了即时 Webtool Fake API 并使用了乘客端点，然后记录响应。然而，还有很多其他的端点，比如“航空公司”。

目前，我们无法访问从 API 获得的数据或响应。因此，我们必须将响应传递给一个变量，以便在`app.component.html`中使用该变量。

在构造函数之后编写下面的代码:

```
passenger: any;

getAllData() {
this.http.get("https://api.instantwebtools.net/v1/passenger")
.subscribe((data: any) => {
  console.log(data);
  this.passenger = data.data;
  })
};

```

我们声明了一个名为`passenger`的变量，然后将我们从 API 得到的响应赋给乘客变量。

现在我们可以用下面的代码利用从`app.component.html`中的请求得到的响应:

```
<div>
  <p
    *ngFor="
      let item of passenger 
    "
  >
    {{ item.airline.name }}
  </p>
</div>

```

如果我们检查浏览器控制台，我们将会看到大量的对象。为了从更大的数组中访问我们想要的数据，我们必须使用`ngFor`遍历数据。如果你看一下上面的`p`标签，你会看到它的作用。

当我们使用下面的命令提供我们的 web 应用程序时，我们将在我们的网页上看到一长串乘客姓名:

```
ng serve

```

这个列表真的很长，向下滚动整个页面不会很有趣。因此，我们将实现服务器端分页，将数据分成页面。这就是角度 ngx 分页的用武之地。

在`app.component.ts`中编写下面的代码:

```
page: number = 1;

```

上面的代码是我们声明并赋值为“1”的变量，它将显示我们所在的当前页面:

```
getAllData() {
  this.http.get("https://api.instantwebtools.net/v1/passenger").subscribe((data: any) => {
      console.log(data.data);
      this.passenger =  data.data;
      this.page =  0
    })
  };

```

因此，我们只需在订阅 API 后传入页面计数。

页码所做的是获取给定的数字计数，并将其传递给 ngx-pagination click 事件。

在`app.component.html`中编写下面的代码:

```
<pagination-controls
  class="pagi"
  (pageChange)="page = $event"
></pagination-controls>

```

如果我们查看 ngx-pagination，我们会注意到一些绑定到它的属性。根据 ngx-pagination 文档，ngx-pagination 有一些属性，我们必须传递给它才能使用。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

这些属性包括`ItemsPerPage`，我们在其中输入每页显示的项目数。该数字不会被分配给 ngx-pagination 组件，并且该属性只接受一个数字(不是字符串或布尔值)。

另一个必需的属性是`CurrrentPage`，它告诉 ngx-pagination 组件它当前在哪个页面上。这个属性也只接受一个数字。

还有很多 ngx 分页属性，但是在这个例子中我们只使用这两个。

让我们恰当地使用这两个属性，这样我们就可以实现 ngx 分页，并以更合理的格式接收乘客姓名列表。

打开`app.component.ts`并编写以下代码:

```
  page = 1;
  passenger: any; 
  itemsPerPage = 6;
  totalItems : any; 

  getAllData() {
    this.http.get(`https://api.instantwebtools.net/v1/passenger?page=${1}&size=${this.itemsPerPage}`).subscribe((data: any) => {
      this.passenger =  data.data;
      this.totalItems = data.totalPassengers;
    })
  }

```

首先，我们开始声明更多的变量，比如`itemsPerPage:`，它保存了每个页面请求显示的项目数。我们还声明了`totalItems:`，它保存了我们的即时 Webtool 假 API 服务器上的乘客总数。

现在，我们将这些参数传递给 API 来接收和循环数据。

在`app.component.html`上，我们可以像下面的代码一样利用分页属性:

```
<div>
  <p
    *ngFor="
      let item of passenger
        | paginate
          : {
              itemsPerPage: itemsPerPage,
              currentPage: page,
              totalItems: totalItems
            }
    "
  >
    {{ item.name }}
    <span>{{ item.airline.country }}</span> <br />
    <img src="{{ item.airline.logo }}" alt="" /> <br />
    <span>{{ item.airline.slogan }}</span> <br />
  </p>
</div>
<pagination-controls
  class="pagi"
  (pageChange)="gty((page = $event))"
></pagination-controls>

```

如果我们仔细观察`p`标签，我们会看到分页属性和我们创建的变量。

然后在分页组件中，我们传入我们创建的第二个函数，然后将`page`属性传递给该函数，就像下面的代码一样:

```
  gty(page: any){
    this.http.get(`https://api.instantwebtools.net/v1/passenger?page=${page}&size=${this.itemsPerPage}`).subscribe((data: any) => {
      this.passenger =  data.data;
      this.totalItems = data.totalPassengers;
    })
  }

```

如果我们现在加载我们的服务器，我们应该得到如下所示的内容:

![Screenshot of webpage with six boxes, each containing passenger and airline name](img/31ae338bdeaddda543194fbf2b9d4053.png)

我们完了！

# 结论

我希望在这篇文章中，您能够理解服务器端分页的重要性，以及我们如何在 Angular 项目中使用 ngx-pagination 来实现它。

## 像用户一样体验 Angular 应用程序

调试 Angular 应用程序可能很困难，尤其是当用户遇到难以重现的问题时。如果您对监视和跟踪生产中所有用户的角度状态和动作感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/angular-signup)

.

[![LogRocket Dashboard Free Trial Banner](img/2794ac39244976f37c4941d9a910be23.png)](https://lp.logrocket.com/blg/angular-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/angular-signup)

LogRocket 就像是网络和移动应用程序的 DVR，记录你网站上发生的一切，包括网络请求、JavaScript 错误等等。您可以汇总并报告问题发生时应用程序的状态，而不是猜测问题发生的原因。

LogRocket NgRx 插件将角度状态和动作记录到 LogRocket 控制台，为您提供导致错误的环境，以及出现问题时应用程序的状态。

现代化调试 Angular 应用的方式- [开始免费监控](https://lp.logrocket.com/blg/angular-signup)。