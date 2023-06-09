# 组件状态:本地状态、Redux 存储和加载器

> 原文：<https://blog.logrocket.com/component-state-local-state-redux-store-and-loaders/>

在 React 中，[组件](https://reactjs.org/docs/react-component.html)被视为一等公民，因此熟悉它们的内部工作机制至关重要。组件的行为主要取决于它的道具或状态。它们之间的区别在于，状态对于组件是私有的，对于外部世界是不可见的。换句话说，状态负责一个组件在幕后的行为，并且可以被认为是它的真实来源。

像**本地状态**、 **Redux store** ，甚至 **this** 的使用，对于一个组件的状态管理有多种方式。但是，在管理状态时，每种方法都有自己的优点和缺点。

## 地方州

React 中的本地状态允许您[为组件实例化一个普通的 JavaScript 对象](https://reactjs.org/docs/faq-state.html)，并保存可能影响其呈现的信息。本地状态在组件中被隔离管理，不受其他组件的影响。

请记住，在 React 的上下文中使用本地状态需要您使用 [ES6 类](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)来创建组件，这些类带有一个构造函数来实例化组件的初始需求。此外，当创建[功能组件](https://www.robinwieruch.de/react-function-component)时，您可以选择使用[使用状态钩子](https://reactjs.org/docs/hooks-state.html)。

在用 ES6 类构建的组件中，每当状态改变时(仅通过 setState 函数可用)，React 触发重新呈现，这对于更新应用程序的状态是必不可少的。这里有一个例子:

```
import React from 'react';

Class FlowerShop extends React.Component {

  constructor(props) {
    super(props);
    this.state = {
      roses: 100
    }
    this.buyRose = this.buyRose.bind(this);
  }

  buyRose() {
    this.setState({
      roses: this.state.roses + 1
    })
  }

  render() {
    return (
      <div>
        <button
          onClick={ this.buyRose }>
          Buy Rose
        </button>
        { this.state.roses }
      </div>
    )
  }

}
```

想象一下，上面的组件就像一个花店，它有自己的内部跟踪系统来查看商店在给定时间有多少玫瑰。如果 FlowerShop 组件是唯一可以访问该状态的实体，那么这可以正常工作。但是想象一下，如果这家店决定开第二家分店。在这种情况下，第二家花店也需要访问可用玫瑰的数量(也称为 this.state)。使用本地状态是不可能的。

现在我们已经意识到本地状态的一个很大的缺点，那就是共享状态。另一方面，如果我们想要跟踪一个组件的隔离状态，该状态不会与外部世界的其他部分共享(如 UI 状态)，那么本地状态将是该用例的完美工具。

## 我们不只是写 Redux，我们也谈论它。现在听着:

或者以后订阅

### Redux 商店

## 所以我们到了第二个用例，它是组件之间的共享状态。这就是 Redux store 发挥作用的地方。简而言之，Redux 有一个[全局存储库](https://redux.js.org/api/store/)，作为应用程序的真实来源。为了将这个扩展到花店的例子，想象一个花店的主要总部。现在，这个总部了解花店连锁店的一切，如果他们中的任何人需要访问可用的玫瑰数量，它将能够为他们提供该信息。这里有一个例子:

出于我们当前讨论的目的，从上面的 Redux 结构中提取的重要内容是`mapStateToProps`和`connect`函数。在这个场景中，当类似于`buyRose`函数的事件被触发时，一个事件被调度，Redux 的全局存储被更新。

```
import React from 'react';
import { connect } from 'react-redux'
import Events from './Events.js'

Class FlowerShop extends React.Component {

  constructor(props) {
    super(props);
    this.buyRose = this.buyRose.bind(this);
  }

  buyRose() {
    this.props.dispatch(Events.buyRose())
  }

  render() {
    return (
      <div>
        <button
          onClick={ this.buyRose }>
          Buy Rose
        </button>
        { this.state.roses }
      </div>
    )
  }

}

const mapStateToProps = (store) => {
  return { roses: store.roses }
}

export default connect(mapStateToProps)(FlowerShop)
```

因此，我们使用`mapToState`函数来访问 Redux 的全局存储，并将其用作 FlowerShop 组件中的道具。这个结构的伟大之处在于，每当我们更新道具时，React 都会触发重新渲染，就像更新状态一样。

最后，`connect`是将所有东西粘合在一起的神奇功能，因此我们的 FlowerShop 组件及其道具将被映射到全局商店及其状态。

Redux 是一个强大的工具，它的逻辑概念使得理解和操作应用程序状态的结构变得更加容易；尤其是对于范围较大的应用程序。但是它会给更简单和更小的应用程序带来许多麻烦，而这些应用程序可能是不必要的。此外，它不是管理应用程序全局状态的唯一解决方案。作为开发者或者软件架构师，你更重要的是理解 Redux 结构背后的推理。在这种情况下，您可能能够在您的应用程序中以更有效的方式使用它，甚至创建您自己的更有效的极简解决方案。我们将在接下来讨论这个问题。

装载机

## 正如[丹·阿布拉莫夫](https://twitter.com/dan_abramov)所介绍的，似乎有两种类型的 React 组件，[表示组件和容器组件](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0)。例如，表示组件应该是哑的或无状态的，而容器组件应该是智能的或有状态的。但是正如文章中暗示的，假设任何组件只属于这些类别中的一个是错误的。有时忽略这种区别是完全可以的(也是必要的),但是采用这种分离复杂有状态逻辑的心理模型可以在大型代码库中获得回报。

众所周知，[在 React 组件之间重用有状态逻辑是困难的](https://reactjs.org/docs/hooks-intro.html#its-hard-to-reuse-stateful-logic-between-components)。对于这个特殊的问题已经有了解决方案，比如[钩子](https://reactjs.org/docs/hooks-intro.html)、[渲染道具](https://reactjs.org/docs/render-props.html)和[高阶组件](https://reactjs.org/docs/higher-order-components.html)，但是它们都有不同程度的复杂性和优缺点。在本文中，我不会对这些解决方案进行相互比较，因为它们会根据您的项目需求而有所不同。相反，我将讨论一个具体的用例，使用高阶组件来解决我以前项目中的一个重复问题。

假设在您的项目中有某种类型的实体(就像我们的花店示例中的可用鲜花列表),几个组件可能需要它。在这个场景中，这些组件的所有父组件都需要执行相同的 API 调用，并用返回的 API 结果更新它们各自的状态。但是我们不想重复自己的工作，并决定最好提取功能，并将它们转移到新的实体中，我们称之为加载器。

为了继续我们的组件状态管理工作，让我们构建一个简单的加载器示例。加载器是一个实体，它负责在表示组件的范围之外进行 API 调用，然后包装该组件(因此是更高阶的组件)并将其内部状态映射到组件属性。在这种情况下，组件不需要知道它的属性是如何派生的。简单又花哨。对！🙂

正如您在上面的代码示例中看到的，从 API 调用获取数据并将其设置为 props 的整个过程隐藏在 loader 中。因此，当像`FlowerShop`这样的组件被`FlowerLoader`包装时，它可以访问`roses`道具，而不需要将它保持在本地状态或 redux 存储状态，并在每次新的 API 调用后更新它。

```
import React from 'react';
import { FlowerLoader } from './loaders';

// Functional Component for simplicity
const FlowerShop = (props) => {

const { roses } = props;

  return (
    <div>
      <button>
        Buy Rose
      </button>
      { roses }
    </div>
  )
};

let Wrapper = FlowerShop;
Wrapper = FlowerLoader(FlowerShop);
```

```
import React from 'react';

// API Call to get the Flowers based on a key
const { GetFlowers } = require('./api');

const NOP = () => null;

const FlowerLoader = (component, placeholder, key = 'roses') => {

placeholder = placeholder || NOP;

// Acts as a higher order function
class Wrapper extends React.Component {
    constructor(props) {
    super(props);
    this.state = { };
  }

  UNSAFE_componentWillMount = () => {
    let roses = this.props[key];
    // We can also add more states here like
    // let lily = this.props[lily];

    if (roses != null) {
      GetFlowers(this.onFlower, roses);
    }
  }

  // The state needs to be updated when receiving newProps
  UNSAFE_componentWillReceiveProps = (newProps) => {
    let roses = newProps[key];

    if (roses != null) {
      GetFlowers(this.onFlower, roses);
    }
  }

  // Callback function to setState if API call was successful
  onFlower = (err, roses) => {
    if (err || !roses) {
      // Do nothing
    } else {
      this.setState({ [key]: roses });
    }
  }

  render() {
    // Mapping state to props
    const localProps = Object.assign({}, this.props, this.state);

    // Extra check to see if the component should be rendered or the placeholder
    const hasRoses = localProps[key] != null;

    // https://reactjs.org/docs/react-api.html#createelement
    return React.createElement(
      hasRoses ? component : placeholder,
      localProps
    );
  }
}

return Wrapper;

};
```

结论

## **当…** 时使用本地状态

您有一个非常简单的应用程序，并且不想设置像 Redux 这样的工具

*   您需要使用和设置短期状态，如文本输入中的键入值
*   该状态不需要与其他组件共享
*   **使用 Redux store 当…**

您的应用程序更加复杂，将状态分成不同的部分似乎是必要的

*   您需要使用和设置长期状态，比如 API 调用的结果
*   状态需要与其他组件共享
*   **在以下情况下使用装载机…**

通过一遍又一遍地设置相同类型的状态和状态更新器，你在重复你自己。使用装载机将结束这种重复

[LogRocket](https://lp.logrocket.com/blg/react-signup-general) :全面了解您的生产 React 应用

## 调试 React 应用程序可能很困难，尤其是当用户遇到难以重现的问题时。如果您对监视和跟踪 Redux 状态、自动显示 JavaScript 错误以及跟踪缓慢的网络请求和组件加载时间感兴趣，

.

[try LogRocket](https://lp.logrocket.com/blg/react-signup-general)

LogRocket 结合了会话回放、产品分析和错误跟踪，使软件团队能够创建理想的 web 和移动产品体验。这对你来说意味着什么？

[![](img/f300c244a1a1cf916df8b4cb02bec6c6.png) ](https://lp.logrocket.com/blg/react-signup-general) [![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/react-signup-general) 

LogRocket 不是猜测错误发生的原因，也不是要求用户提供截图和日志转储，而是让您回放问题，就像它们发生在您自己的浏览器中一样，以快速了解哪里出错了。

不再有嘈杂的警报。智能错误跟踪允许您对问题进行分类，然后从中学习。获得有影响的用户问题的通知，而不是误报。警报越少，有用的信号越多。

LogRocket Redux 中间件包为您的用户会话增加了一层额外的可见性。LogRocket 记录 Redux 存储中的所有操作和状态。

现代化您调试 React 应用的方式— [开始免费监控](https://lp.logrocket.com/blg/react-signup-general)。

Modernize how you debug your React apps — [start monitoring for free](https://lp.logrocket.com/blg/react-signup-general).