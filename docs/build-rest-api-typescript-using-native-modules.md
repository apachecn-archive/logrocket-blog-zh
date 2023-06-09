# 如何用 TypeScript 构建一个只使用本机模块的 REST API

> 原文：<https://blog.logrocket.com/build-rest-api-typescript-using-native-modules/>

[TypeScript](https://blog.logrocket.com/tag/typescript/) 是 JavaScript 的类型化超集，在运行时编译成普通 JavaScript。JavaScript 的动态特性不允许您捕捉任何意外的结果，除非程序已经被执行，因此 TypeScript 类型系统将使您能够在编译时而不是运行时捕捉这样的错误。这意味着任何有效的 JavaScript 代码都是等价的有效的类型脚本代码。

TypeScript 既是一种语言，也是一套工具。作为一种语言，它包括语法、关键字和[类型注释](https://blog.logrocket.com/types-vs-interfaces-in-typescript/)。它的工具集提供了类型信息，IDE 可以使用这些信息来提供自动完成、类型提示、重构选项、静态类型和其他功能。

TypeScript 和 JavaScript 生态系统提供了各种工具和库，帮助您更快地构建应用程序。例如，在创建服务器时，您可能会使用 [Express](https://blog.logrocket.com/express-middleware-a-complete-guide/) 库来设置您的 API。Express 为您提供了大规模设置和管理 HTTP 服务器的功能。

然而，这些库是使用普通 JavaScript 开发的。在幕后，您的 Express 服务器运行在原始的 TypeScript 或 JavaScript 上。这意味着 Express 提供的函数和方法抽象了普通基础语言的底层逻辑。您的服务器不直接与 TypeScript 或 JavaScript 提供的基本逻辑进行交互；相反，Express 访问普通代码，并将其转换为可伸缩代码，从而减少代码库。

使用这些库可以加快开发速度，同时减少冗余代码；他们提供的优势是无可争议的。但是，您可能希望利用 TypeScript 的“基本框架”,并使用普通代码运行您的应用程序。这将帮助您执行最纯粹的服务器，而无需使用 Express 之类的库。

本指南将演示如何使用 TypeScript 创建一个只使用本机模块的 REST API。该项目旨在帮助您学习如何在 Node.js 中创建一个 HTTP 服务器，而无需使用额外的库。

## 如何用 Node.js 设置 TypeScript

本教程将使用 Node 来运行 TypeScript。Node 是一个 JavaScript 运行时，旨在创建可伸缩的异步事件驱动应用程序。

继续在您的计算机上安装[节点运行时](https://nodejs.org/en/)。然后，创建一个项目文件夹，并使用`npm init -y`初始化您的项目。

让我们配置节点来运行 TypeScript 代码。您需要节点可用的 TypeScript 依赖项。为此，请使用以下命令安装 TypeScript 节点包:

```
npm install -D typescript

```

您现在可以使用`tsc --init`来利用您的 TypeScript 项目。这将使用默认的 TypeScript 编译选项创建一个`tsconfig.json`文件。

> `tsc --init`可能要求您使用命令`npm install -g typescript`在您的计算机上全局安装 TypeScript。

运行 TypeScript 代码时，需要使用 Node 命令执行上述依赖项。为此，使用一个类型脚本执行引擎和名为 [ts-node](https://typestrong.org/ts-node/) 的 REPL 库。Ts-node 允许您运行指向`.ts`文件的一次性命令，编译并在浏览器上运行它们。

继续安装 ts 节点，如下所示:

```
npm install -D ts-node 

```

然后，编辑您的`package.json`脚本标签:

```
"scripts": {
   "start": "ts-node ./src/index.ts"
},

```

这意味着 ts-node 将把`/src/index.ts`文件指向主文件，并执行`index.ts`指向的`.ts`代码和模块。

最后，向您的项目添加一个`@types/node`库。Node 运行在 JavaScript 上，这个项目使用 TypeScript。因此，您需要为运行 TypeScript 的节点添加类型定义。

[@types/node](https://www.npmjs.com/package/@types/node) 包含内置的 TypeScript 定义。要安装它，请运行:

```
npm install @types/node

```

## 如何创建简单的 TypeScript HTTP 服务器

TypeScript 节点安装程序已准备好运行您的代码。让我们看看如何创建一个使用 HTTP 本机模块运行的基本 HTTP 服务器。

首先，创建一个`src`文件夹并添加一个`index.ts`文件。然后，按照以下步骤使用 Node 设置一个基本的 TypeScript HTTP 服务器。

首先，导入 HTTP 本机模块:

```
import HTTP from "HTTP";

```

要创建 HTTP 服务器和客户机，您需要使用 HTTP 命令`from "http"`。这将允许您拥有创建服务器所必需的功能。

接下来，创建一个从中接收数据的本地服务器:

```
const myServer = http.createServer((req, res) => {
   res.write('Hello World!')
   res.end()
});

```

使用`createServer()`函数设置一个服务器实例。这个函数允许您发出 HTTP 请求和响应。`res.write`代码允许您指定服务器应该执行的传入消息。`res.end()`结束设置传入请求，即使没有数据写入请求体或由 HTTP 响应发送。

然后，启动服务器并监听连接:

```
myServer.listen(3000, () => {
   console.log('Server is running on port 3000\. Go to http://localhost:3000/')
});

myServer.close()

```

`listen()`将创建一个本地主机 TCP 服务器监听端口 3000。在这种情况下，`3000`必须是未使用的端口，一旦服务器开始监听连接，它将立即被分配到该端口。`listen()`方法是异步的，它管理服务器在退出当前连接时如何接受新连接。当所有连接都结束时，服务器异步关闭。如果出现错误，服务器将被调用一个`Error`并立即关闭。

一旦你使用`npm start`运行你的应用，服务器就会启动，你可以在`[http://localhost:3000/](http://localhost:3000/)`访问它。响应正文中指定的`Hello World!`消息将在您的浏览器上提供。这个基本的 HTTP API 是非常低级的，运行在最基本的 TypeScript 上。

## 如何创建 CRUD 类型脚本 REST API

上面的例子只使用了 HTTP 模块来设置一个基本的服务器。让我们深入研究并构建一个使用 CRUD(创建、读取、更新和删除)方法的 REST API。

要设置这个 API，首先要将我们的示例待办事项列表存储在一个 JSON 文件中。然后，在`src`文件夹中创建一个`store.json`文件，并添加以下任务列表:

```
[
   {
     "id": 1,
     "title": "Learn React",
     "completed": false
   },
   {
     "id": 2,
     "title": "Learn Redux",
     "completed": false
   },
   {
     "id": 3,
     "title": "Learn React Router",
     "completed": false
   },
   {
     "id": 4,
     "title": "Cooking Lunch",
     "completed": true
   }
]

```

待办事项 API 将引用这些数据来执行基于服务器的方法，如 GET、POST、PUT 和 DELETE。

### 设置任务结构

使用 TypeScript 时，可以使用类、继承、接口和其他面向对象的形式。JavaScript 使用类，但是这些类是 JavaScript 对象的模板。

JavaScript 没有接口，因为它们只在 TypeScript 中可用。接口帮助您定义类型，使您保持在代码的边缘。这确保了参数和变量结构是强类型的。

接口基本上反映了一个对象的结构，该对象可以作为参数传递给类或者由类实现。它们只定义结构和指定类型一次。因此，您可以在代码中的任何地方重用它们，而不必每次都复制相同的类型。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

您可以使用接口来反映任务数据的结构。这将指定您希望 API 与之交互的对象的结构。在这种情况下，当您调用 API 时，您获得的任务信息与镜像任务数据的结构相同。

让我们用接口来定义待办事项 API 应该有什么属性。在`src`目录中创建一个新文件，并将其命名为`ITask.ts`。

在该文件中，将任务结构定义如下:

```
// Task structure
interface Task {
   id: number;
   title: string;
   completed: boolean;
}

export { Task }

```

这将创建一个定义域数据的模型。请确保将其导出，以便其他项目模块可以访问其属性。

### 添加 API 控制器

控制器用于处理发送回 HTTP 响应的 HTTP 请求。API 控制器函数处理端点的这些请求。控制器控制在`ITask.ts`中定义的结构和 API 端点返回给用户的数据。在这种情况下，每个控制器将处理处理每个 CRUD 操作的逻辑。

继续在`src`目录中创建一个`controller.ts`文件。然后，添加以下导入并创建每个 CRUD 控制器，如下所示:

```
// access the data store (store.json)
import fs from "fs";
import path from "path";

// handle requests and reponses
import { ServerResponse, IncomingMessage } from "http";

// access the task structure
import { Task } from "./ITask";

```

### 获取任务

创建一个函数`getTasks()`。该函数获取`store.json`文件中列出的所有任务:

```
const getTasks = (req: IncomingMessage, res: ServerResponse) => {
   return fs.readFile(
     path.join(__dirname, "store.json"),
     "utf8",
     (err, data) => {
       // Read the store.json file
       // Check out any errors
       if (err) {
         // error, send an error message
         res.writeHead(500, { "Content-Type": "application/json" });
         res.end(
           JSON.stringify({
             success: false,
             error: err,
           })
         );
       } else {
         // no error, send the data
         res.writeHead(200, { "Content-Type": "application/json" });
         res.end(
           JSON.stringify({
             success: true,
             message: JSON.parse(data),
           })
         );
       }
     }
   );
};

```

用户使用指向`getTasks()`函数的端点发送请求。该控制器将接收该请求以及该请求想要做什么。然后，`ITask`接口会设置数据并给出响应。控制器`getTasks()`将得到这个响应，并将其数据传递给被执行的端点。在这种情况下，控制器将读取存储在`store.json`文件中的数据，并返回待办事项列表。

### 添加新的任务

首先，创建一个名为`addTask()`的函数。这个`addTask()`控制器将处理添加新任务的逻辑，就像这样:

```
const addTask = (req: IncomingMessage, res: ServerResponse) => {
   // Read the data from the request
   let data = "";

   req.on("data", (chunk) => {
     data += chunk.toString();
   });

   // When the request is done
   req.on("end", () => {
     let task = JSON.parse(data);

     // Read the store.json file
     fs.readFile(path.join(__dirname, "store.json"), "utf8", (err, data) => {
       // Check out any errors
       if (err) {
         // error, send an error message
         res.writeHead(500, { "Content-Type": "application/json" });
         res.end(
           JSON.stringify({
             success: false,
             error: err,
           })
         );
       } else {
         // no error, get the current tasks
         let tasks: [Task] = JSON.parse(data);
         // get the id of the latest task
         let latest_id = tasks.reduce(
           (max = 0, task: Task) => (task.id > max ? task.id : max),
           0
         );
         // increment the id by 1
         task.id = latest_id + 1;
         // add the new task to the tasks array
         tasks.push(task);
         // write the new tasks array to the store.json file
         fs.writeFile(
           path.join(__dirname, "store.json"),
           JSON.stringify(tasks),
           (err) => {
             // Check out any errors
             if (err) {
               // error, send an error message
               res.writeHead(500, { "Content-Type": "application/json" });
               res.end(
                 JSON.stringify({
                   success: false,
                   error: err,
                 })
               );
             } else {
               // no error, send the data
               res.writeHead(200, { "Content-Type": "application/json" });
               res.end(
                 JSON.stringify({
                   success: true,
                   message: task,
                 })
               );
             }
           }
         );
       }
     });
   });
};

```

在上面的代码中，用户使用指向`AddTasks()`函数的端点发送请求。该控制器将首先从添加新任务的请求中读取数据。然后，它读取`store.json`文件并准备接收新的数据条目。`ITask`接口将设置创建新任务所需的属性，并向`AddTasks()`给出响应。

如果发送的请求与`ITask`设置的结构匹配，`AddTasks()`将接受它的消息，并将新的任务细节写入`store.json`文件。

### 更新任务

您可能希望更新现有任务的值。这将要求您发送一个请求，通知保存您想要更新一些值。

为此，创建一个`updateTask()`函数，如下所示:

```
const updateTask = (req: IncomingMessage, res: ServerResponse) => {
   // Read the data from the request
   let data = "";
   req.on("data", (chunk) => {
     data += chunk.toString();
   });
   // When the request is done
   req.on("end", () => {
     // Parse the data
     let task: Task = JSON.parse(data);
     // Read the store.json file
     fs.readFile(path.join(__dirname, "store.json"), "utf8", (err, data) => {
       // Check out any errors
       if (err) {
         // error, send an error message
         res.writeHead(500, { "Content-Type": "application/json" });
         res.end(
           JSON.stringify({
             success: false,
             error: err,
           })
         );
       } else {
         // no error, get the current tasks
         let tasks: [Task] = JSON.parse(data);
         // find the task with the id
         let index = tasks.findIndex((t) => t.id == task.id);
         // replace the task with the new one
         tasks[index] = task;
         // write the new tasks array to the store.json file
         fs.writeFile(
           path.join(__dirname, "store.json"),
           JSON.stringify(tasks),
           (err) => {
             // Check out any errors
             if (err) {
               // error, send an error message
               res.writeHead(500, { "Content-Type": "application/json" });
               res.end(
                 JSON.stringify({
                   success: false,
                   error: err,
                 })
               );
             } else {
               // no error, send the data
               res.writeHead(200, { "Content-Type": "application/json" });
               res.end(
                 JSON.stringify({
                   success: true,
                   message: task,
                 })
               );
             }
           }
         );
       }
     });
   });
};

```

这将根据存储在`store.json`文件中的现有数据检查发送的数据。在这种情况下，服务器将检查 ID 值是否与任何现有任务匹配。`ITask`将检查更新值是否与设置的结构匹配，并向`updateTask()`返回响应。如果是，该值将被更新，并将响应发送到请求端点。

### 删除任务

同样，您可以从存储中删除任务。下面是帮助您发送执行删除请求的请求的代码:

```
const deleteTask = (req: IncomingMessage, res: ServerResponse) => {
   // Read the data from the request
   let data = "";
   req.on("data", (chunk) => {
     data += chunk.toString();
   });
   // When the request is done
   req.on("end", () => {
     // Parse the data
     let task: Task = JSON.parse(data);
     // Read the store.json file
     fs.readFile(path.join(__dirname, "store.json"), "utf8", (err, data) => {
       // Check out any errors
       if (err) {
         // error, send an error message
         res.writeHead(500, { "Content-Type": "application/json" });
         res.end(
           JSON.stringify({
             success: false,
             error: err,
           })
         );
       } else {
         // no error, get the current tasks
         let tasks: [Task] = JSON.parse(data);
         // find the task with the id
         let index = tasks.findIndex((t) => t.id == task.id);
         // remove the task
         tasks.splice(index, 1);
         // write the new tasks array to the store.json file
         fs.writeFile(
           path.join(__dirname, "store.json"),
           JSON.stringify(tasks),
           (err) => {
             // Check out any errors
             if (err) {
               // error, send an error message
               res.writeHead(500, { "Content-Type": "application/json" });
               res.end(
                 JSON.stringify({
                   success: false,
                   error: err,
                 })
               );
             } else {
               // no error, send the data
               res.writeHead(200, { "Content-Type": "application/json" });
               res.end(
                 JSON.stringify({
                   success: true,
                   message: task,
                 })
               );
             }
           }
         );
       }
     });
   });
};

```

最后，导出这些控制器，以便其他应用程序模块可以访问它们:

```
export { getTasks, addTask, updateTask, deleteTask };

```

### 设置服务器和任务路由

一旦设置了控制器，就需要创建和公开各种端点来创建、读取、更新和删除任务。端点是执行请求数据的 URL。

该端点将与 HTTP 方法结合使用，对数据执行特定的操作。这些 HTTP 方法包括 GET、POST、PUT 和 DELETE。每个 HTTP 方法将被映射到与其定义的路由匹配的单个控制器。

导航到您的`./src/index.ts`文件，并如下设置您的方法端点:

```
import HTTP from "HTTP";

// import controller
import { getTasks, addTask, updateTask, deleteTask } from "./controller";

// create the http server
const server = http.createServer((req, res) => {
   // get tasks
   if (req.method == "GET" && req.url == "/api/tasks") {
     return getTasks(req, res);
   }

   // Creating a task
   if (req.method == "POST" && req.url == "/api/tasks") {
     return addTask(req, res);
   }

   // Updating a task
   if (req.method == "PUT" && req.url == "/api/tasks") {
     return updateTask(req, res);
   }

   // Deleting a task
   if (req.method == "DELETE" && req.url == "/api/tasks") {
     return deleteTask(req, res);
   }
});

// set up the server port and listen for connections
server.listen(3000, () => {
   console.log("Server is running on port 3000");
});

```

这定义了四个端点:

这也将在本地主机上公开此端点。服务器将被映射到端口 3000。

一旦启动并运行，它将根据执行路由监听连接。
待办事项 API 已经准备好了，您可以使用`npm start`运行它，并开始测试不同的端点。

## 结论

用普通代码运行你的应用程序会让你对运行应用程序基础的代码有所了解。Vanilla TypeScript 将帮助您创建其他开发人员可以用来加速其开发工作流的包。

任何普通代码的最大缺点是，你需要写很多行代码来执行一个普通的任务。使用允许您编写几行代码的包，同样的任务仍然可以运行。这意味着当运行普通代码时，您必须管理应用程序中的大多数操作。

## [LogRocket](https://lp.logrocket.com/blg/typescript-signup) :全面了解您的网络和移动应用

[![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/typescript-signup)

LogRocket 是一个前端应用程序监控解决方案，可以让您回放问题，就像问题发生在您自己的浏览器中一样。LogRocket 不需要猜测错误发生的原因，也不需要向用户询问截图和日志转储，而是让您重放会话以快速了解哪里出错了。它可以与任何应用程序完美配合，不管是什么框架，并且有插件可以记录来自 Redux、Vuex 和@ngrx/store 的额外上下文。

除了记录 Redux 操作和状态，LogRocket 还记录控制台日志、JavaScript 错误、堆栈跟踪、带有头+正文的网络请求/响应、浏览器元数据和自定义日志。它还使用 DOM 来记录页面上的 HTML 和 CSS，甚至为最复杂的单页面和移动应用程序重新创建像素级完美视频。

[Try it for free](https://lp.logrocket.com/blg/typescript-signup)

.