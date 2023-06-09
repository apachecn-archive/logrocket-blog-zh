# 使用 Axios 设置请求头

> 原文：<https://blog.logrocket.com/using-axios-set-request-headers/>

几乎所有在用户浏览器中可见的东西都是通过 HTTP 传输的，所以这些网络请求在互联网通信中起着重要的作用。HTTP 请求的一个关键组成部分是报头。HTTP 请求头用于提供关于请求的附加信息。例如，关于所请求的信息、发送者以及发送者希望如何与接收者联系的细节。

Axios 是一个灵活而健壮的解决方案，用于发出 HTTP 请求并拦截来自 Node.js 应用程序和浏览器的 HTTP 响应。但是，Axios 还做得更多。除了 HTTP 客户端的标准 CRUD 功能，Axios 还提供了许多其他有用的内置特性，比如配置默认值、错误处理、请求取消以及自动将 JavaScript 对象序列化为 JSON。

在本文中，我们将探索使用 Axios 为 API 调用设置请求头的不同方式。

## 安装 Axios

对于本文中使用的例子，我们将从 CDN 安装 Axios。它也可以与 npm、Yarn 或 Bower 一起安装。

以下是每种方法的脚本或命令:

```
//via cdn
<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>

// via npm
$ npm install axios

// via yarn
$ yarn add axios

// via bower
$ bower install axios

```

让我们探索使用 Axios 为 API 调用设置请求头的不同方式:

### 传递对象参数

Axios 方法，如`post()`和`get()`，通过提供 headers 对象作为 GET 请求的第二个参数和 POST 请求的第三个参数，使我们能够将头附加到请求上。

让我们看看这对于单个和多个请求是如何工作的:

#### 个人请求

POST 和 GET 请求分别用于创建或检索资源。以下是一些一次性或个人请求的示例。

首先，我们声明包含`headers`对象的`config`对象，它将在发出请求时作为参数提供。我们还声明了一个`api endpoint`和一个`data`对象:

```
const config = {
  headers:{
    header1: value1,
    header2: value2
  }
};
const url = "api endpoint";

const data ={
  name: "Jake Taper",
  email: "[email protected]"
}

```

我们可以使用 GET 请求从 API 端点`url`检索`config`对象:

```
axios.get(url, config)
  .then(res=> console.log(res))
  .catch(err=> console.log(err))

```

在这个例子中，我们传入 API 端点`url`作为第一个参数，传入`config`对象作为第二个参数。

我们可以使用 POST 请求将`data`对象传递给 API 端点`url`:

```
 axios.post(url, data, config)
  .then(res => console.log(res))
  .catch(err => console.log(err))

```

在这个例子中，我们传入 API 端点`url`作为第一个参数，传入一个`data`对象作为第二个参数，传入`config`对象作为第三个参数。

#### 多个请求

在某些情况下，可能需要为多个或后续请求自动设置标头。我们可以通过指定配置默认值来解决这个问题。

此代码为所有请求设置授权头:

```
axios.defaults.headers.common['Authorization'] = `Bearer ${localStorage.getItem('access_token')}`;

```

这段代码为所有`post`请求设置授权头:

```

axios.defaults.headers.post['Authorization'] = `Bearer ${localStorage.getItem('access_token')}`;

```

### 创建特定的 Axios 实例

我们还可以通过创建 Axios 的特定实例来设置 API 调用的请求头。

我们可以使用`require`来创建 Axios 的新实例:

```
const axios = require('axios')

```

但是，此选项不允许我们传入配置。为了正确设置每个请求的头，我们可以使用`axios.create` 创建一个 Axios 实例，然后在该实例上设置一个定制配置:

```
let reqInstance = axios.create({
  headers: {
    Authorization : `Bearer ${localStorage.getItem("access_token")}`
    }
  }
})

```

我们可以在每次使用这个特定实例发出请求时重用这个配置。

当我们使用`reqInstance`发出请求时，将会附加授权头:

```
reqInstance.get(url);

```

### 使用 Axios 拦截器

我们还可以使用 Axios 拦截器来设置 API 调用的请求头。Axios 拦截器是由 Axios 调用的函数。拦截器可以用于在传输请求之前修改请求，或者在传递响应之前修改响应。拦截器本质上相当于来自 [Express](https://expressjs.com/en/guide/using-middleware.html) 或[mongose](https://mongoosejs.com/docs/middleware.html)的中间件。

我以前做过一个项目，需要将包含用户访问令牌的授权头附加到每个请求上。这是一个金融应用程序，系统需要验证每个请求的用户身份。在这个例子中，最好是自动将授权头附加到每个请求，而不是单独设置它们。

身份验证是拦截器最常见的应用之一。客户端应用程序通常通过在授权头中提交秘密访问令牌来向服务器验证用户身份。

我们可以使用 Axios 拦截器为所有请求自动设置`Authorization`头:

```
// Request interceptors for API calls
axios.interceptors.request.use(
  config => {
    config.headers['Authorization'] = `Bearer ${localStorage.getItem('access_token')}`;
        return config;
    },
    error => {
        return Promise.reject(error);
    }
);

```

在这个例子中，我们使用`axios.interceptors.request.use`方法更新每个请求头，并在`Authorization` HTTP 头中设置访问令牌。

我们将来自`config.headers`对象的`Authorization`头作为目标，并将存储在`localStorage`中的`Bearer`令牌设置为它的值。

Axios 拦截器对于监控即将到期的访问令牌也很有用。`refreshToken()`函数可用于在令牌到期前更新令牌:

```
const refreshToken= ()=>{
  // gets new access token
}

```

每当响应返回一个`403`错误时，我们还可以调用`axios.interceptors.response.use()`方法来获取一个新的访问令牌，这意味着现有的令牌已经过期:

```
// Response interceptor for API calls
axios.interceptors.response.use(
    response => {
        return response;
    },
    error => {
        if(error.response.status == 403){
            refreshToken()
        }
    }
);

```

在这个例子中，`axios.interceptors.response.use`方法拦截所有传入的响应，然后检查`response`的状态。如果触发`response`的请求没有通过认证，那么令牌就过期了。在这种情况下，我们调用`refreshToken()`函数来获取新的访问令牌。

## 包扎

在本文中，我们研究了如何通过传递一个对象、创建一个特定的 Axios 实例和使用 Axios 拦截器来使用 Axios 设置 HTTP 请求头。有关 Axios HTTP 客户端的其他功能的信息，请参见其[网站](https://axios-http.com)或 [GitHub](https://github.com/axios/axios) 。

## 您是否添加了新的 JS 库来提高性能或构建新特性？如果他们反其道而行之呢？

毫无疑问，前端变得越来越复杂。当您向应用程序添加新的 JavaScript 库和其他依赖项时，您将需要更多的可见性，以确保您的用户不会遇到未知的问题。

LogRocket 是一个前端应用程序监控解决方案，可以让您回放 JavaScript 错误，就像它们发生在您自己的浏览器中一样，这样您就可以更有效地对错误做出反应。

[![LogRocket Dashboard Free Trial Banner](img/e8a0ab42befa3b3b1ae08c1439527dc6.png)](https://lp.logrocket.com/blg/javascript-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/javascript-signup)

[LogRocket](https://lp.logrocket.com/blg/javascript-signup) 可以与任何应用程序完美配合，不管是什么框架，并且有插件可以记录来自 Redux、Vuex 和@ngrx/store 的额外上下文。您可以汇总并报告问题发生时应用程序的状态，而不是猜测问题发生的原因。LogRocket 还可以监控应用的性能，报告客户端 CPU 负载、客户端内存使用等指标。

自信地构建— [开始免费监控](https://lp.logrocket.com/blg/javascript-signup)。