# webpack 中的树抖动和代码分裂

> 原文：<https://blog.logrocket.com/tree-shaking-and-code-splitting-in-webpack/>

## 什么是树摇动？

树抖动，也称为死代码消除，是在您的产品构建中删除未使用的代码的实践。向最终用户交付尽可能少的代码非常重要。通过静态分析我们的源代码，我们可以确定哪些没有被使用，并将其从我们的最终包中排除。

## 什么是代码拆分？

另一方面，代码分割是指将生产构建代码分割成多个按需加载的模块。如果您在与用户交互后在代码中使用了第三方库，我们可以在初始包中排除该第三方代码，只在需要时加载它，以实现更快的加载时间。

## webpack 中的树晃动

在 [webpack](https://webpack.js.org/blog/2020-10-10-webpack-5-release/#commonjs-tree-shaking) 中，树抖动对 ECMAScript 模块(ESM)和 CommonJS 都有效，但对[异步模块定义(AMD)](https://blog.logrocket.com/javascript-reference-guide-js-modules/) 或[通用模块定义(UMD)](https://blog.logrocket.com/javascript-reference-guide-js-modules/) 无效。

ESM 允许最佳的树抖动，因为 CommonJS、AMD 和 UMD 都可能是不确定的，因此不可能静态分析以有效消除死代码。

例如，在 [Node.js](https://blog.logrocket.com/tag/node/) 中，您可以有条件地运行带有变量的`require`来加载随机脚本。Webpack 不可能在构建时知道你所有的导入和导出，所以它会尝试[树动摇一些构造](https://webpack.js.org/blog/2020-10-10-webpack-5-release/#commonjs-tree-shaking)，一旦事情变得过于动态就退出。

对于 ESM 也是如此，下面的代码可以强制 webpack 退出树抖动`app.js`,因为导入的使用不是静态的。

```
import * as App from 'app.js'

const variable = // some variable

console.log(App[variable])

```

而且，尽管 UMD 作为一个模块系统是一个吸引人的选择，因为它在任何地方都可以工作，但它是不可动摇的，所以，根据微软的肖恩·拉金所说，最好坚持使用 ESM，让使用你的代码的开发者处理从一个模块系统到另一个模块系统的转换。

## webpack 入门

在使用 webpack 时，您会发现有些代码比其他功能相似的代码更容易受到树的影响。要覆盖 webpack 使用的所有启发式方法来彻底改变你的代码是不可能的，所以我们将把用例限制在几个重要的用例上。

要运行一个基本的 webpack 项目，安装`webpack`和`webpack-cli`。

```
$ yarn init -y
$ yarn add -D webpack webpack-cli

```

在一个`src`目录下创建两个文件，`src/index.js`和`src/person.js`:

```
// src/person.js
export const person = { name: "John", age: 30 };

```

在`person.js`中，导出一个`person`对象用于其他模块。

```
// src/index.js
import { person } from "./person";

console.log(person.name);

```

默认情况下，运行`yarn webpack`将使用`src/index.js`作为入口点，并输出一个`dist/main.js`构建文件。该命令还会警告我们没有设置`mode`，并将在`production`模式下运行 webpack。

如果打开`build/main.js`，你会发现下面这段无格式的代码，和我们写的源代码相差甚远。

```
// dist/main.js
(() => {
  "use strict";
  console.log("John");
})();

```

请注意，webpack 将代码包装在[life](https://developer.mozilla.org/en-US/docs/Glossary/IIFE)中，并将所有模块捆绑到一个文件中，它将继续这样做，直到我们告诉它不这样做。

它还正确地确定了我们没有完整地使用`person`对象，也不需要一个`person`变量来开始。

如果我们重用`person.name`(例如，通过复制我们的`console.log`调用，)webpack 将在它被优化和最小化之后在我们的包中维护它，但是将继续从我们的`person`对象中树抖动未使用的属性:

```
// dist/main.js
(() => {
  "use strict";
  const o = "John";
  console.log(o), console.log(o);
})();

```

使用这个设置，让我们探索一些我们在模块中使用的导入和导出模式。

## 在 webpack 中使用名称空间导入和树抖动

我们将切换到一个`component.js`文件来处理熟悉的主题。在`component.js`中，我们可以编写开源组件库中的代码，并导出一些组件:

```
// src/component.js
export const Root = () => "root";
export const Title = () => "title";
export const Overlay = () => "overlay";

```

在`index.js`中，我们使用`Title`组件:

```
// src/index.js
import { Title } from "./component";

console.log(Title());

```

编译这两个文件，我们得到下面的代码:

```
// dist/main.js
(() => {
  "use strict";
  console.log("title");
})(); 
```

就树的可动摇性而言，使用名称空间导入与使用命名导入的工作方式相同。

我们可以在几个公开的软件包文档中找到这种模式，比如 [Yup 的](https://github.com/jquense/yup#usage)和 [Radix UI 的](https://www.radix-ui.com/docs/primitives/components/dialog#anatomy)。在 webpack 5 中，这一点得到了增强，也涵盖了[嵌套导入](https://webpack.js.org/blog/2020-10-10-webpack-5-release/#nested-tree-shaking)。

```
// src/index.js
import * as Component from "./component";

console.log(Component.Title());

```

捆绑这段代码将会产生与之前完全相同的输出。

名称空间导入允许我们在一个对象下封装几个导入。但是，有些库作者自己处理这个问题，为您创建了那个对象，然后通常将它导出为默认导出，就像 React 一样。

```
// src/component.js
export const Root = () => "root";
export const Title = () => "title";
export const Description = () => "description";

Root.Title = Title;
Root.Description = Description;

```

这种模式很常见，一个组件被分配给其余的组件。例如，你可以通过一个`Object.assign`调用在 [HeadlessUI](https://headlessui.dev/react/dialog#basic-example) 中找到这个模式。

不幸的是，它不再是树摇动的了，因为`Root.`赋值是动态的，可以有条件地调用。Webpack 不能再对此进行静态分析，该包将如下所示:

```
// dist/main.js
(() => {
  "use strict";
  const t = () => "root";
  (t.Title = () => "title"),
    (t.Description = () => "description"),
    console.log("title");
})();

```

尽管我们没有在任何地方使用`description`函数，但它是在生产代码中提供的。

我们可以通过导出一个实际对象来解决这个问题并保持类似的体验:

```
// src/component.js
export const Root = () => "root";
export const Title = () => "title";
export const Description = () => "description";

export const Component = {
  Root,
  Title,
  Description,
};
```

```
// src/index.js
import { Component } from "./component";

console.log(Component.Title());.
```

```
// dist/main.js
(() => {
  "use strict";
  console.log("title");
})();

```

## webpack 中的树摇动类

与函数不同，类不能被绑定器静态分析。如果你有一个像下面这样的类，方法`greet`和`farewell`即使不用也不能树摇。

```
// src/person.js
export class Person {
  constructor(name) {
    this.name = name;
  }

  greet(greeting = "Hello") {
    return `${greeting}! I'm ${this.name}`;
  }

  farewell() {
    return `Goodbye!`;
  }
}
```

```
// src/index.js
import { Person } from "./person";

const John = new Person("John");

console.log(John.farewell());

```

虽然我们只使用了`farewell`方法而不是`greet`方法，但是我们的捆绑代码包含了`farewell`和`greet`方法。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

为了解决这个问题，我们可以将方法提取为独立的函数，将类作为参数。

```
// src/person.js
export class Person {
  constructor(name) {
    this.name = name;
  }
}

export function greet(person, greeting = "Hello") {
  return `${greeting}! I'm ${person.name}`;
}

export function farewell() {
  return `Goodbye!`;
}

```

现在，我们导入`greet`，这导致`farewell`从我们的包中被树摇动。

```
// src/index.js
import { Person, greet } from "./person";

const John = new Person("John");

console.log(greet(John, "Hi")); // "Hi! I'm John"

```

## 摇树副作用

在函数式编程中，我们习惯于纯代码工作。我们导入和导出只接收输入并产生输出的代码。相比之下，具有副作用的代码是在全局上下文中修改某些内容的代码(例如，多填充)。

作为副作用的模块不能树摇，因为它们没有导入和导出。但是，代码并不一定是模块才会有副作用。以下面的代码为例:

```
// src/side-effect.js
export const foo = "foo";

const mayHaveSideEffect = (greeting) => {
  fetch("/api");
  return `${greeting}!!`;
};

export const bar = mayHaveSideEffect("Hello");
```

```
// src/index.js
import { foo } from "./side-effect";

console.log(foo);

```

`bar`变量在初始化时会触发一个副作用。Webpack 意识到了这一点，必须在包中包含副作用代码，尽管我们根本没有使用`bar`:

```
// dist/main.js
(() => {
  "use strict";
  fetch("/api"), console.log("foo");
})();

```

为了指示 webpack 去掉初始化`bar`的副作用，我们可以使用`PURE`魔法注释，就像这样:

```
// src/side-effect.js
export const bar = /*#__PURE__*/ mayHaveSideEffect("Hello");

// dist/main.js
(() => {
  "use strict";
  console.log("foo");
})();

```

## webpack 中的代码拆分

在 webpack 之前，开发者使用脚本标签、IIFE 和 [JSON with padding (JSONP)](https://en.wikipedia.org/wiki/JSONP) 的组合来组织和编写模块化代码。

举个例子:

```
<body>
  <script src="global.js"></script>
  <script src="carousel.js"></script> <!-- carousel.js depends on global.js -->
  <script src="shop.js"></script> <!-- shop.js depends on global.js -->
</body>

```

如果`carousel.js`要用一个已经在`global.js`中声明的名字来声明一个变量，它将会覆盖它并使整个应用程序崩溃。所以，生命被用来封装代码，以免影响其他代码。

```
var foo = 'bar';

(function () {
  var foo = 'baz';
})()
```

生命是一个函数，它立即调用自己，在这个过程中创建新的作用域，而不干扰以前的作用域。

这个工作流程的最后一部分是 JSONP 的使用，它是在 [CORS](https://blog.logrocket.com/the-ultimate-guide-to-enabling-cross-origin-resource-sharing-cors/) 还没有标准化的时候创建的，在浏览器中禁止从服务器请求 JSON 文件。

JSONP 是一个 JavaScript 文件，当被请求时，它立即调用带有特定数据或逻辑的预定义函数。注意，函数不一定是 JSON。

```
<script type="text/javascript">
  var callback = function(json) {
      console.log(json)
    }
</script>
<script type="text/javascript" src="https://example.com/jsonp.js"></script>
<!--
  // jsonp.js contains:
  callback("The quick brown fox jumps over the lazy dog")

  when https://example.com/jsonp.js gets loaded,
  "The quick brown fox..." will be logged to the console immediately.
-->
```

您可以看到，使用这些概念来模块化我们的代码可能很麻烦并且容易出错。但事实上，这些正是驱动 webpack 的概念。webpack 所做的只是通过静态分析来自动化这个过程，同时提供一流的开发人员体验和额外的功能，其中包括树抖动。

很明显，代码分割或延迟加载只是 webpack 创建和附加更多的脚本标签，这些标签在 [webpack 世界中被称为块](https://webpack.js.org/glossary/#c)。

处理延迟加载模块的代码已经在页面上了。并且，JSONP 用于在模块加载后立即执行代码。

```
<script type="text/javascript">
  var handleLazyLoadedComponent = function(component) {/* ... */}
</script>
<script type="text/javascript" src="chunk.js"></script>
<!-- chunk.js calls handleLazyLoadedComponent with the right code to work seamlessly -->
```

## webpack 中的代码拆分

为了利用代码分割，我们可以使用全局`import`函数:

```
// src/lazy.js
export const logger = console.log;
```

```
// src/index.js
const importLogger = () => import("./lazy");

document.addEventListener("click", () => {
  importLogger().then((module) => {
    module.logger("hello world");
  });
});

```

在`index.js`中，我们没有静态导入我们的`logger`函数，而是选择在事件触发时按需导入。`import`返回一个用整个模块解决的承诺。

在我们的捆绑代码中，我们现在看到两个文件而不是一个，有效地分割了我们的代码。

## webpack 中的动态导入

因为 webpack 在构建时使用静态分析捆绑我们的应用程序，所以它不能在运行时提供真正的动态导入。如果您试图将`import`函数与变量(即`import(someVariable)`)一起使用，webpack 会警告您不要这样做。但是，如果你给 webpack 一个在哪里寻找动态模块的提示，它会在编译时在预期使用它们的情况下将它们全部代码拆分。

例如，假设我们有一个包含三个文件的`numbers`目录:`one.js`、`two.js`和`three.js`，它导出数字:

```
// src/numbers/one.js
export const one = 1;

// src/numbers/two.js
export const two = 2;

// src/numbers/three.js
export const three = 3;

```

如果我们想要动态导入这些文件，我们需要在`import`函数调用中硬编码路径:

```
// src/index.js
const getNumber = (number) => import(`./numbers/${number}.js`);

document.addEventListener("click", () => {
  getNumber("one").then((module) => {
    console.log(module.one);
  });
});

```

如果在我们的`numbers`目录中有不是`.js`文件的模块(例如，JSON 或 CSS 文件),通过在导入调用中包含 JavaScript 文件有助于缩小导入范围。

这将创建三个额外的包，尽管我们在代码中只使用了一个包。

## 摇树动态导入

动态导入解析整个模块——用它的默认和命名导出——而不用树摇动未使用的导入。

要动态导入一个节点模块并树摇，我们可以先创建一个只导出我们想要的模块，然后动态导入。

像 [Material-UI](https://www.npmjs.com/package/@material-ui/core) 和 [lodash.es](https://www.npmjs.com/package/lodash-es) 这样的库是以一种你可以基于文件结构访问导出的方式构建的。在这种情况下，我们可以跳过重新导出模块，直接在第一个位置导入它。

## 结论

在本文中，我们介绍了 webpack 中的树抖动，并学习了如何使常见的模式成为树抖动的。我们还讨论了 webpack 在代码分割时是如何工作的，以及如何在运行时动态导入模块。最后，本文介绍了如何将树抖动和代码分割结合起来，以获得可能的最佳包。感谢阅读。

## 使用 [LogRocket](https://lp.logrocket.com/blg/signup) 消除传统错误报告的干扰

[![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/signup)

[LogRocket](https://lp.logrocket.com/blg/signup) 是一个数字体验分析解决方案，它可以保护您免受数百个假阳性错误警报的影响，只针对几个真正重要的项目。LogRocket 会告诉您应用程序中实际影响用户的最具影响力的 bug 和 UX 问题。

然后，使用具有深层技术遥测的会话重放来确切地查看用户看到了什么以及是什么导致了问题，就像你在他们身后看一样。

LogRocket 自动聚合客户端错误、JS 异常、前端性能指标和用户交互。然后 LogRocket 使用机器学习来告诉你哪些问题正在影响大多数用户，并提供你需要修复它的上下文。

关注重要的 bug—[今天就试试 LogRocket】。](https://lp.logrocket.com/blg/signup-issue-free)