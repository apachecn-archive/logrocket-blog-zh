# Node.js 中的 CommonJS 与 ES 模块

> 原文：<https://blog.logrocket.com/commonjs-vs-es-modules-node-js/>

在现代软件开发中，模块将软件代码组织成自包含的块，这些块一起构成更大、更复杂的应用程序。

在浏览器 JavaScript 生态中，JavaScript 模块的使用依赖于`import`和`export`语句；这些语句分别加载和导出 EMCAScript 模块(或 es 模块)。

ES 模块格式是封装 JavaScript 代码以供重用的官方标准格式，大多数现代 web 浏览器本身就支持这些模块。

但是，Node.js 默认支持 CommonJS 模块格式。CommonJS 模块使用`require()`加载，变量和函数使用`module.exports`从 CommonJS 模块导出。

随着 JavaScript 模块系统的标准化，ES 模块格式在 [Node.js v8.5.0](https://github.com/nodejs/node/blob/master/doc/changelogs/CHANGELOG_V8.md#8.5.0) 中被引入。作为一个实验模块，`--experimental-modules`标志是在 Node.js 环境中成功运行 ES 模块所必需的。

不过从 13.2.0 版本开始，Node.js 对 ES 模块有了稳定的支持。

本文不会过多讨论这两种模块格式的用法，而是将 CommonJS 与 es 模块进行比较，以及为什么要使用其中一种而不是另一种。

## 比较 CommonJS 模块和 es 模块语法

默认情况下，Node.js 将 JavaScript 代码视为 CommonJS 模块。因此，CommonJS 模块的特点是模块导入用`require()`语句，模块导出用`module.exports`语句。

例如，这是一个导出两个函数的 CommonJS 模块:

```
module.exports.add = function(a, b) {
        return a + b;
} 

module.exports.subtract = function(a, b) {
        return a - b;
} 

```

我们还可以使用`require()`将公共函数导入到另一个 Node.js 脚本中，就像我们在这里做的一样:

```
const {add, subtract} = require('./util')

console.log(add(5, 5)) // 10
console.log(subtract(10, 5)) // 5

```

如果你正在寻找关于 CommonJS 模块的更深入的教程[，看看这个](https://blog.logrocket.com/es-modules-in-node-today/#commonjsmodulesystem)。

另一方面，库作者也可以通过将文件扩展名从`.js`更改为`.mjs.`来启用 Node.js 包中的 ES 模块

例如，这里有一个简单的 ES 模块(带有一个`.mjs`扩展),导出两个函数供公共使用:

```
// util.mjs

export function add(a, b) {
        return a + b;
}

export function subtract(a, b) {
        return a - b;
}

```

然后我们可以使用`import`语句导入这两个函数:

```
// app.mjs

import {add, subtract} from './util.mjs'

console.log(add(5, 5)) // 10
console.log(subtract(10, 5)) // 5

```

在项目中启用 ES 模块的另一种方法是在最近的`package.json`文件(与您正在制作的包在同一个文件夹中)中添加一个`"type: module"`字段:

```
{
  "name": "my-library",
  "version": "1.0.0",
  "type": "module",
  // ...
}

```

有了这样的包含，Node.js 就将该包中的所有文件都视为 es 模块，并且您不必将文件更改为`.mjs`扩展名。你可以[在这里](https://blog.logrocket.com/how-to-use-ecmascript-modules-with-node-js/)了解更多关于 ES 模块的信息。

或者，您可以安装并设置一个类似 Babel 的 [transpiler，将您的 ES 模块语法编译成 CommonJS 语法](https://www.youtube.com/watch?v=7vGk8vFDGfA)。像 React 和 Vue 这样的项目支持 ES 模块，因为它们使用[巴别塔来编译代码](https://blog.logrocket.com/babel-vs-typescript/)。

## 在 Node.js 中使用 ES 模块和 CommonJS 模块的利弊

### ES 模块是 JavaScript 的标准，而 CommonJS 是 Node.js 中的默认模块

创建 ES 模块格式是为了标准化 JavaScript 模块系统。它已经成为封装 JavaScript 代码以供重用的标准格式。

另一方面，CommonJS 模块系统内置于 Node.js 中。在 Node.js 中引入 ES 模块之前，CommonJS 是 Node.js 模块的标准。因此，有大量的 Node.js 库和模块是用 CommonJS 编写的。

对于浏览器支持，所有主流浏览器都支持 ES 模块语法，您可以在 React 和 Vue.js 等框架中使用`import` / `export`，这些框架使用 Babel 等编译器将`import` / `export`语法向下编译到`require()`，这是较旧的 Node.js 版本原生支持的。

除了作为 JavaScript 模块的标准之外，es 模块的语法也比`require()`更具可读性。由于语法相同，主要在客户端编写 JavaScript 的 Web 开发人员使用 Node.js 模块不会有任何问题。

### 对 ES 模块的 Node.js 支持

#### 较旧的 Node.js 版本不支持 ES 模块

虽然 ES 模块已经成为 JavaScript 中的标准模块格式，但开发人员应该考虑到 Node.js 的旧版本缺乏支持(特别是 Node.js v9 及更低版本)。

换句话说，使用 ES 模块会使应用程序与只支持 CommonJS 模块(即`require()`语法)的 Node.js 早期版本不兼容。

但是有了新的条件导出，我们可以构建双模式库。这些是由较新的 es 模块组成的库，但是它们也向后兼容较旧的 Node.js 版本支持的 CommonJS 模块格式。

换句话说，我们可以构建一个同时支持`import`和`require()`的库，允许我们解决不兼容的问题。

考虑下面的 Node.js 项目:

```
my-node-library
├── lib/
│   ├── browser-lib.js (iife format)
│   ├── module-a.js  (commonjs format)
│   ├── module-a.mjs  (es6 module format)
│   └── private/
│       ├── module-b.js
│       └── module-b.mjs
├── package.json
└── …

```

在`package.json`内部，我们可以使用`exports`字段以两种不同的模块格式导出公共模块(`module-a`)，同时限制对私有模块(`module-b`)的访问:

```
// package.json
{
  "name": "my-library",         
  "exports": {
    ".": {
        "browser": {
          "default": "./lib/browser-module.js"
        }
    },
    "module-a": {
        "import": "./lib/module-a.mjs" 
        "require": "./lib/module-a.js"
    }
  }
}

```

通过提供以下关于我们的`my-library`包的信息，我们现在可以在任何支持它的地方使用它:

```
// For CommonJS 
const moduleA = require('my-library/module-a')

// For ES6 Module
import moduleA from 'my-library/module-a'

// This will not work
const moduleA = require('my-library/lib/module-a')
import moduleA from 'my-awesome-lib/lib/public-module-a'
const moduleB = require('my-library/private/module-b')
import moduleB from 'my-library/private/module-b'

```

因为有了`exports`中的路径，我们可以在不指定绝对路径的情况下导入(和`require()`我们的公共模块。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

通过包含`.js`和`.mjs`的路径，我们可以解决不兼容的问题；我们可以为不同的环境映射包模块，比如浏览器和 Node.js，同时限制对私有模块的访问。

#### 较新的 Node.js 版本完全支持 ES 模块

在大多数较低的 Node.js 版本中，ES 模块被标记为实验性的。这意味着该模块缺少一些特性，并且在`--experimental-modules`标志之后。较新版本的 Node.js 对 ES 模块有稳定的支持。

然而，重要的是要记住，Node.js 要将一个模块视为 ES 模块，必须发生以下情况之一:要么模块的文件扩展名必须从`.js`(对于 CommonJS)转换为`.mjs`(对于 ES 模块)，要么我们必须在最近的`package.json`文件中设置一个`{"type":` `"module"}`字段。

在这种情况下，该包中的所有代码都将被视为 ES 模块，并且应该使用`import` / `export`语句来代替`require()`。

### CommonJS 提供了模块导入的灵活性

在 ES 模块中，只能在文件的开头调用 import 语句。在其他地方调用它会自动将表达式转移到文件开头，甚至会引发错误。

另一方面，使用`require()`作为函数，在运行时被解析。因此，`require()`可以在代码的任何地方被调用。您可以使用它从`if`语句、条件循环和函数中有条件地或动态地加载模块。

例如，您可以像这样在条件语句中调用`require()`:

```
if(user.length > 0){
   const userDetails = require(‘./userDetails.js’);
  // Do something ..
}

```

这里，只有当用户在场时，我们才加载一个名为`userDetails`的模块。

### CommonJS 同步加载模块，es 模块是异步的

使用`require()`的一个限制是它同步加载模块。这意味着模块是一个接一个加载和处理的。

您可能已经猜到，这可能会给包含数百个模块的大规模应用程序带来一些性能问题。在这种情况下，基于其异步行为，`import`可能会优于`require()`。

然而，对于使用几个模块的小规模应用程序来说，`require()`的同步特性可能不是什么大问题。

## 结论:CommonJS 还是 ES 模块？

对于仍然使用旧版本 Node.js 的开发人员来说，使用新的 ES 模块是不切实际的。

由于粗略的支持，将现有项目转换为 ES 模块会导致应用程序与只支持 CommonJS 模块的 Node.js 早期版本不兼容(即`require()`语法)。

因此，迁移您的项目以使用 ES 模块可能不是特别有益。

作为一个初学者，学习 ES 模块是有益和方便的，因为它们正在成为用 JavaScript 为客户端(浏览器)和服务器端(Node.js)定义模块的标准格式。

对于新的 Node.js 项目，ES 模块提供了 CommonJS 的替代方案。ES 模块格式确实为编写同构 JavaScript 提供了一条更简单的途径，它既可以在浏览器上运行，也可以在服务器上运行。

总之，EMCAScript 模块是 JavaScript 的未来。

## 200 只![](img/61167b9d027ca73ed5aaf59a9ec31267.png)显示器出现故障，生产中网络请求缓慢

部署基于节点的 web 应用程序或网站是容易的部分。确保您的节点实例继续为您的应用程序提供资源是事情变得更加困难的地方。如果您对确保对后端或第三方服务的请求成功感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/node-signup)

.

[![LogRocket Network Request Monitoring](img/cae72fd2a54c5f02a6398c4867894844.png)](https://lp.logrocket.com/blg/node-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/node-signup)

LogRocket 就像是网络和移动应用程序的 DVR，记录下用户与你的应用程序交互时发生的一切。您可以汇总并报告有问题的网络请求，以快速了解根本原因，而不是猜测问题发生的原因。

LogRocket 检测您的应用程序以记录基线性能计时，如页面加载时间、到达第一个字节的时间、慢速网络请求，还记录 Redux、NgRx 和 Vuex 操作/状态。

[Start monitoring for free](https://lp.logrocket.com/blg/node-signup)

.