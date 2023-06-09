# 8 个常见的 React 错误消息以及如何解决它们

> 原文：<https://blog.logrocket.com/8-common-react-error-messages-how-address-them/>

无论您是具有多年经验的 React 开发人员，还是刚开始涉足该领域，您肯定会在某个时候遇到错误消息。您是否编写了导致这些错误的代码并不重要——没有人编写完美的代码，我们很幸运 React 通过确保我们保持在正确的轨道上来帮助我们。

然而，重要的是您解决这些错误消息的方法。偶然发现它们，在谷歌上搜索它们，并根据其他人的经验修改你的代码是一种方法。

另一种方法——也许是更好的方法——是理解错误背后的细节，以及为什么它首先是一个问题。

本文将通过回顾一些最常见的 React 错误消息并解释它们的含义、后果以及如何修复它们来帮助您理解这些细节。

我们将讨论以下错误消息:

这将帮助你更好地理解潜在的错误，并防止你在将来犯类似的错误。

## 警告:列表中的每个孩子都应该有一个唯一的`key`道具

```
import { Card } from "./Card";

const data = [
  { id: 1, text: "Lorem ipsum dolor sit amet, consectetur adipiscing elit." },
  { id: 2, text: "Phasellus semper scelerisque leo at tempus." },
  { id: 3, text: "Duis aliquet sollicitudin neque," }
];

export default function App() {
  return (
    <div className="container">
      {data.map((content) => (
        <div className="card">
          <Card text={content.text} />
        </div>
      ))}
    </div>
  );
}

```

React 开发中最常见的事情之一是获取数组的项，并使用组件根据项的内容来呈现它们。多亏了 JSX，我们可以使用`Array.map`函数轻松地将该逻辑嵌入到组件中，并从回调中返回所需的组件。

然而，在你的浏览器控制台中收到一个 React 警告也是很常见的，它说列表中的每个孩子都应该有一个唯一的`key`道具。在养成给每个孩子一个独特的`key`道具的习惯之前，你可能会多次遇到这种警告，尤其是如果你对 React 缺乏经验的话。但是，在你养成习惯之前，你如何改正它呢？

### 如何解决这个问题

如警告所示，您必须向从`map`回调返回的 JSX 的最外层元素添加一个`key`道具。然而，你要用的密钥有几个要求。关键应该是:

1.  字符串或数字
2.  对于列表中的特定项目是唯一的
3.  代表列表中跨呈现项目

```
export default function App() {
  return (
    <div className="container">
      {data.map((content) => (
        <div key={content.id} className="card">
          <Card text={content.text} />
        </div>
      ))}
    </div>
  );
}

```

虽然如果你不遵守这些要求，你的应用程序不会崩溃，但它会导致一些意想不到的、通常是不想要的行为。React 使用这些键来确定列表中的哪些子元素发生了变化，并使用这些信息来确定先前 DOM 的哪些部分可以重用，以及在重新呈现组件时应该重新计算哪些部分。因此，添加这些键总是明智的。

## 防止在键中使用数组索引

在前面的警告的基础上，我们进入同样常见的关于同一主题的 [ESLint 警告](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-array-index-key.md)。这个警告通常会出现在你已经习惯了将一个`key`道具和列表中的 JSX 放在一起之后。

```
import { Card } from "./Card";

// Notice that we don't include pre-generated identifiers anymore.
const data = [
  { text: "Lorem ipsum dolor sit amet, consectetur adipiscing elit." },
  { text: "Phasellus semper scelerisque leo at tempus." },
  { text: "Duis aliquet sollicitudin neque," }
];

export default function App() {
  return (
    <div className="container">
      {data.map((content, index) => (
        <div key={index} className="card">
          <Card text={content.text} />
        </div>
      ))}
    </div>
  );
}

```

有时，您的数据没有唯一的标识符。一个简单的解决方法是使用列表中当前项目的索引。然而，使用数组中该项的索引作为键的问题是，它不能代表渲染中的特定项。

假设我们有一个包含几个条目的列表，用户通过移除第二个条目与它们进行交互。对于第一项，其底层 DOM 结构没有任何变化；这反映在它的键上，键保持不变，`0`。

对于第三项及以后的内容，它们的内容没有改变，所以它们的底层结构也不应该改变。然而，所有其他项目*的`key`属性将*改变，因为键是基于数组索引的。React 将假设它们已经改变并重新计算它们的结构——这是不必要的。这会对性能产生负面影响，还会导致不一致和不正确的状态。

### 如何解决这个问题

为了解决这个问题，记住键不一定是标识符是很重要的。只要它们是惟一的，并且代表最终的 DOM 结构，那么无论您想使用什么键都可以。

```
export default function App() {
  return (
    <div className="container">
      {data.map((content) => (
        <div key={content.text} className="card">{/* This is the best we can do, but it works */}
          <Card text={content.text} />
        </div>
      ))}
    </div>
  );
}

```

## React Hook `useXXX`被有条件调用。React 挂钩必须在每个组件渲染中以完全相同的顺序调用

在开发过程中，我们可以用不同的方式优化我们的代码。您可以做的一件事是确保某些代码只在需要该代码的代码分支中执行。尤其是在处理耗费大量时间和资源的代码时，这在性能方面会有很大的不同。

```
const Toggle = () => {
  const [isOpen, setIsOpen] = useState(false);

  if (isOpen) {
    return <div>{/* ... */}</div>;
  }
  const openToggle = useCallback(() => setIsOpen(true), []);
  return <button onClick={openToggle}>{/* ... */}</button>;
};

```

不幸的是，将这种优化技术应用到钩子上会给你一个警告，不要有条件地调用 React 钩子，因为[你必须在每个组件渲染](https://reactjs.org/docs/hooks-rules.html)中以相同的顺序调用它们。

这是必要的，因为在内部，React 使用钩子被调用的顺序来跟踪它们的底层状态，并在渲染之间保存它们。如果打乱了这个顺序，React 将在内部不知道哪个状态与钩子匹配。这导致了 React 的主要问题，甚至可能导致 bug。

### 如何解决这个问题

React 钩子必须总是在组件的顶层被调用——并且是无条件的。实际上，这通常归结为保留组件的第一部分用于 React 钩子初始化。

```
const Toggle = () => {
  const [isOpen, setIsOpen] = useState(false);
  const openToggle = useCallback(() => setIsOpen(true), []);

  if (isOpen) {
    return <div>{/* ... */}</div>;
  }
  return <button onClick={openToggle}>{/* ... */}</button>;
};

```

## React Hook 缺少依赖项:“XXX”。要么包含它，要么移除依赖项数组

React 挂钩的一个有趣的方面是依赖关系数组。几乎每个 React 钩子都接受一个数组形式的第二个参数，在数组内部，你可以定义钩子的依赖关系。当任何依赖关系改变时，React 将检测到它并重新触发挂钩。

在他们的文档中，React 建议开发人员总是[在依赖关系数组](https://blog.logrocket.com/guide-to-react-useeffect-hook/#importance-dependency-array)中包含所有变量，如果它们在钩子中使用并在改变时影响组件的渲染。

### 如何解决这个问题

为此，建议使用`[react-hooks ESLint plugin](https://www.npmjs.com/package/eslint-plugin-react-hooks)`中的`[exhaustive-deps rule](https://www.npmjs.com/package/eslint-plugin-react-hooks#advanced-configuration)`。当任何一个 React 钩子没有定义所有的依赖项时，激活它会警告你。

```
const Component = ({ value, onChange }) => {
  useEffect(() => {
    if (value) {
      onChange(value);
    }
  }, [value]); // `onChange` isn't included as a dependency here.

  // ...
}

```

您应该对依赖关系数组问题做到详尽无遗的原因与 JavaScript 中的[闭包和作用域的概念有关。如果 React 钩子的主回调使用了自己作用域之外的变量，那么它只能记住这些变量在执行时的版本。](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures#closure_scope_chain)

但是当这些变量改变时，回调的结束不能自动地选择那些改变的版本。这可能导致使用过期的依赖关系引用来执行 React 钩子代码，并导致与预期不同的行为。

出于这个原因，我们总是建议对依赖数组进行详尽的描述。这样做解决了以这种方式调用 React 挂钩的所有可能问题，因为它将 React 指向要跟踪的变量。当 React 检测到任何变量的变化时，它将重新运行回调，允许它获得依赖项的变化版本并按预期运行。

## 无法对卸载的组件执行反应状态更新

当处理组件中的异步数据或逻辑流时，您可能会在浏览器的控制台中遇到一个运行时错误，告诉您不能对已经卸载的组件执行状态更新。问题是在组件树的某个地方，状态更新被触发到一个已经卸载的组件上。

```
const Component = () => {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetchAsyncData().then((data) => setData(data));
  }, []);

  // ...
};

```

这是由依赖于异步请求的状态更新引起的。异步请求在组件生命周期的某个地方开始(比如在一个`useEffect`钩子中),但是需要一段时间才能完成。

同时，组件已经被卸载了(例如，由于用户交互)，但是原始的异步请求仍然完成——因为它没有连接到 React 生命周期——并且触发了组件的状态更新。此处触发错误是因为组件不再存在。

### 如何解决这个问题

有几种方法可以解决这个问题，所有这些方法都可以归结为两个不同的概念。首先，可以跟踪组件是否已安装，我们可以基于此执行操作。

虽然这种方法可行，但不推荐使用。这种方法的问题在于，它不必要地保留了一个未安装组件的引用，这会导致内存泄漏和性能问题。

```
const Component = () => {
  const [data, setData] = useState(null);
  const isMounted = useRef(true);

  useEffect(() => {
    fetchAsyncData().then(data => {
      if(isMounted.current) {
        setData(data);
      }
    });

    return () => {
      isMounted.current = false;
    };
  }, []);

  // ...
}

```

第二种也是首选的方法是在组件卸载时取消异步请求。一些异步请求库已经有了取消这种请求的机制。如果是这样，就像在`useEffect`钩子的清理回调期间取消请求一样简单。

如果您没有使用这样的库，您可以使用`AbortController`来实现同样的效果。这些取消方法唯一的缺点是它们完全依赖于一个库的实现或者浏览器对 T2 的支持。

```
const Component = () => {
  const [data, setData] = useState(null);

  useEffect(() => {
    const controller = new AbortController();
    fetch(url, { signal: controller.signal }).then((data) => setData(data));
    return () => {
      controller.abort();
    }
  }, []);

  // ...
};

```

## 太多的重新渲染。React 限制渲染的数量以防止无限循环

无限循环是每个开发人员的祸根，React 开发人员也不例外。幸运的是，React 做了很好的工作来检测它们，并在您的整个设备变得没有反应之前警告您。

### 如何解决这个问题

正如警告所暗示的，问题是你的组件触发了太多的重新渲染。当您的组件在很短的时间内对太多的状态更新进行排队时，就会发生这种情况。导致无限循环的最常见原因是:

*   直接在渲染中执行状态更新
*   没有为事件处理程序提供适当的回调

如果您遇到这个特别的警告，请确保检查组件的这两个方面。

```
const Component = () => {
  const [count, setCount] = useState(0);

  setCount(count + 1); // State update in the render

  return (
    <div className="App">
      {/* onClick doesn't receive a proper callback */}
      <button onClick={setCount((prevCount) => prevCount + 1)}>
        Increment that counter
      </button>
    </div>
  );
}

```

## 对象作为反应子对象无效/函数作为反应子对象无效

在 React 中，我们可以在组件中向 DOM 呈现很多东西。选择几乎是无穷无尽的:所有的 HTML 标记、任何 JSX 元素、任何原始的 JavaScript 值、一组先前的值，甚至 JavaScript 表达式，只要它们的值等于任何先前的值。

尽管如此，不幸的是，React 仍然不接受作为 React 孩子可能存在的一切。更具体地说，您不能将对象和函数呈现到 DOM 中，因为这两个数据值不会计算出 React 可以呈现到 DOM 中的任何有意义的内容。因此，任何这样做的尝试都会导致 React 以所提到的错误的形式抱怨它。

### 如何解决这个问题

如果您面临这些错误中的任何一个，建议验证您正在呈现的变量是预期的类型。最常见的情况是，这个问题是由于在 JSX 中渲染一个子元素或变量引起的，假设它是一个原始值，但实际上，它是一个对象或函数。作为一种预防方法，拥有一个合适的类型系统会有很大的帮助。

```
const Component = ({ body }) => (
  <div>
    <h1>{/* */}</h1>
    {/* Have to be sure the `body` prop is a valid React child */}
    <div className="body">{body}</div>
  </div>
);

```

## 相邻的 JSX 元素必须用封闭标记括起来

React 最大的好处之一是能够通过组合许多较小的组件来构建一个完整的应用程序。每个组件都可以以它应该呈现的 JSX 的形式定义它的 UI，这最终有助于应用程序的整个 DOM 结构。

```
const Component = () => (
  <div><NiceComponent /></div>
  <div><GoodComponent /></div>
);

```

由于 React 的复合性质，一个常见的尝试是在一个组件的根中返回两个只在另一个组件中使用的 JSX 元素。然而，这样做会给 React 开发人员带来一个警告，告诉他们必须将相邻的 JSX 元素放在封闭标签中。

从一般 React 开发人员的角度来看，这个组件只能在另一个组件内部使用。因此，在他们的心理模型中，从一个组件中返回两个元素是非常合理的，因为无论外部元素是在这个组件中定义的还是在父组件中定义的，最终的 DOM 结构都是相同的。

然而，React 不能够做出这种假设。潜在地，这个组件可以在根中使用并破坏应用程序，因为它将导致无效的 DOM 结构。

### 如何解决这个问题

React 开发人员应该总是将从一个组件返回的多个 JSX 元素包装在一个封闭标签中。这可以是一个元素，一个组件，或者 React 的片段，如果你确定组件不需要外部元素的话。

```
const Component = () => (
  <React.Fragment>
    <div><NiceComponent /></div>
    <div><GoodComponent /></div>
  </React.Fragment>
);

```

## 最后的想法

不管你有多少经验，在开发过程中遇到错误是不可避免的。然而，您处理这些错误消息的方式也表明了您作为 React 开发人员的能力。要正确地做到这一点，有必要理解这些错误并知道它们为什么会发生。

为了帮助您做到这一点，本文回顾了您在 React 开发过程中会遇到的八个最常见的 React 错误消息。我们讨论了错误消息背后的含义、潜在的错误、如何解决错误，以及如果不修复错误会发生什么。

有了这些知识，您现在应该更彻底地理解这些错误，并感到有能力编写更少的包含这些错误的代码，从而获得更高质量的代码。

## 使用 LogRocket 消除传统反应错误报告的噪音

[LogRocket](https://lp.logrocket.com/blg/react-signup-issue-free)

是一款 React analytics 解决方案，可保护您免受数百个误报错误警报的影响，只针对少数真正重要的项目。LogRocket 告诉您 React 应用程序中实际影响用户的最具影响力的 bug 和 UX 问题。

[![](img/f300c244a1a1cf916df8b4cb02bec6c6.png) ](https://lp.logrocket.com/blg/react-signup-general) [ ![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png) ](https://lp.logrocket.com/blg/react-signup-general) [LogRocket](https://lp.logrocket.com/blg/react-signup-issue-free)

自动聚合客户端错误、反应错误边界、还原状态、缓慢的组件加载时间、JS 异常、前端性能指标和用户交互。然后，LogRocket 使用机器学习来通知您影响大多数用户的最具影响力的问题，并提供您修复它所需的上下文。

关注重要的 React bug—[今天就试试 LogRocket】。](https://lp.logrocket.com/blg/react-signup-issue-free)