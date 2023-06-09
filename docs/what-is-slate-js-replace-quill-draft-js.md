# 什么是 Slate.js，它会取代 Quill 和 Draft.js 吗？

> 原文：<https://blog.logrocket.com/what-is-slate-js-replace-quill-draft-js/>

[石板](https://github.com/ianstormtaylor/slate)T2。js 是一个高度可定制的平台，用于创建富文本编辑器，也称为 WYSIWYG 编辑器。它使你能够创建强大、直观的编辑器，类似于你可能在 Medium、Dropbox Paper 或 Google Docs 中使用的编辑器。这些正迅速成为许多网络应用的标准特性，而 Slate 这样的工具使它们更容易实现，确保你的程序不会陷入复杂之中。

虽然在撰写本文时 Slate 仍处于测试阶段，但人们自然会问它是否有潜力取代更多的[成熟平台，如 Quill](https://blog.logrocket.com/build-a-wysiwyg-text-editor-using-quill/) 和 [Draft.js](https://blog.logrocket.com/building-rich-text-editors-in-react-using-draft-js-and-react-draft-wysiwyg/) 。简短的回答是，现在下结论还为时过早，但这里有一些事实来支持为什么会这样。

## Slate.js 有什么新功能？

为 React 应用程序构建富文本编辑器并不容易。随着应用程序规模的增长，需要一个更高效、支持更丰富的工具。使用像 Quill 这样的框架，开发人员必须通过大量的修改来解决性能问题。但是 Slate.js 旨在通过以下特性使事情变得更简单:

### 一流的插件

其他基于 React 的富文本编辑器如 Quill 和 Draft 提供插件，为用户提供额外的功能。另一方面，Slate 承认插件是一级实体；基本的编辑功能甚至被设计成一个独特的插件。这意味着您可以完全改变编辑体验，允许您开发像 Medium 或 Dropbox 这样的复杂编辑器，而不必与库的标准假设相冲突。

### 平行于大教堂

DOM 是 Slate 数据模型的基础。该文档是一个分层树，使用选择、范围和公开所有常用的事件处理程序。这意味着表格和嵌套块引用等复杂功能是可能的。Slate 几乎可以执行您在 DOM 中可以执行的任何操作。

### 嵌套文档模型

Slate 的文档模型和 DOM 本身一样，是一个分层的递归树。根据您的用例，您可以合并复杂的组件，如表和嵌套的块引用，正如我们上面提到的，或者您可以通过仅使用一个层次来简化事情。

### 无状态视图和不可变数据

Slate 编辑器是无状态的，通过 React 和 Immutable.js 利用不可变的数据结构，使得推理代码和编写插件变得更加容易。举例来说，为了便于比较，Quill 处理自己的修改，并且不允许用户阻止编辑。Quill 不能阻止这种变化，但它会在值与现有状态不同的任何时候覆盖内容。

### 无模式核心

Slate 的核心逻辑对您将要更改的数据结构不做任何假设，因此当您需要超越最基本的用例时，不会有任何假设嵌入到库中，让您措手不及。在使用 Quill 和 Draft.js 时，这可能会导致严重的性能问题。

### 清晰的核心边界

有了插件优先的设计和无模式的核心,“核心”和“定制”之间的界限更加明显，这意味着核心体验不会陷入边缘情况。

### 直观的变化

石板文本是用“变更”来编辑的，这意味着是高层次的、易于创建和理解的，允许自定义功能尽可能具有表现力。这极大地提高了您对代码进行推理的能力。

### 支持协作的数据模型

Slate 的数据格式旨在允许在其上构建协作编辑，因此如果您决定让您的编辑器协作，开发人员不必重新考虑所有事情。

## Slate.js 在行动

现在，让我们通过构建一个简单的富文本编辑器来看看 Slate 的运行情况。首先，我们需要创建一个新的 React 项目；为此，我们将使用 Create React 应用程序。运行下面的命令:

```
npx create-react-app rich-text-editor
```

您可以在安装必要的软件包时冲一杯咖啡。安装完成后，使用下面的命令安装我们的 Slate 实现所需的三个包:

```
npm i --save slate slate-react slate-history
```

然后，启动应用程序:

```
npm start
```

接下来，打开`App.js`组件并导入我们已安装的包:

```
import React, { useMemo, useState } from 'react'
import { createEditor } from 'slate'
import { Slate, Editable, withReact } from 'slate-react'
```

下一步是创建一个新的`Editor`对象。我们将使用`useEditor`钩子使我们的编辑器在渲染时保持稳定。然后，我们将创建一个状态，用一个段落和一些虚拟文本来处理编辑器中的输入:

```
const editor = useMemo(() => withReact(createEditor()), [])
const [value, setValue] = useState([
    {
      type: 'paragraph',
      children: [{ text: 'I am a Slate rich editor.' }],
    },
  ])
```

现在，让我们跟踪我们的 Slate 编辑器、它的插件、它的值、它的选择，以及通过呈现 Slate 上下文提供程序对编辑器所做的所有更改。然后，在我们的 React 上下文中呈现`<Editable>`组件。

在 React 中，`<Editable>`组件的行为类似于`contentEditable`组件。无论何时呈现，它都会为最近的`editor`上下文呈现一个可编辑的富文本文档。用下面的代码修改呈现方法:

```
return (
    <Slate
      editor={editor}
      value={value}
      onChange={newValue => setValue(newValue)}
    >
      <Editable />
    </Slate>
  )
```

现在你有了你的文本编辑器，在你最喜欢的浏览器中测试一下`localhost:3000`上的应用程序。

## 为什么选择 Slate.js？

创建 Slate 是为了解决开发人员在使用 Quill 和 Draft.js 构建大规模应用程序时可能遇到的挑战。它旨在通过进行调整来转变文档的创建，这对于开发高级行为是必要的。用鹅毛笔或草稿经常被证明过于复杂。

奎尔，毫无疑问，是一个插播编辑；不用改变什么就可以开始了。然而，如果您超越了最基本的用例，您可能会遇到某些性能问题，这已经成为[一个公认的](https://github.com/quilljs/quill/issues/2197) [缺陷](https://github.com/quilljs/quill/issues/2197)。

另一方面，Slate 旨在通过为您提供做任何选择的灵活性来提高实际生产力。Slate 提供了与 Markdown、Google Docs 和 Medium out of the box 的复杂集成，以实现与队友的无缝协作。

它允许您执行复杂的操作，如添加表格，以及将图像和项目符号列表插入到这些表格中。Slate.js 使得序列化为 HTML、Markdown 和其他格式成为可能。像将文档转换成 HTML 或 Markdown 这样的简单任务，使用较少的样板代码会变得容易得多。

综上所述，Slate.js 绝对值得一试。

## Slate 会取代 Quill 和 Draft.js 吗？

说实话，可能不是这样的。Slate.js 仍处于测试阶段，这意味着稳定版本尚未发布。你的应用可能会崩溃，或者某些功能可能无法正常工作。

再说一次，尽管它们并不完美，但 Quill 和 Draft.js 已经投入生产很长时间了。尽管我们可能不希望如此，但对于编程语言来说，没有十全十美这回事。最重要的是，对于一个组织来说，在短时间内将其系统改变为全新的东西并不容易。

最终，Slate 还没有被严格地用于生产级应用程序，也没有被证明可以处理那些暴露 Quill 和 Draft 效率低下的模糊任务。

也许在一年左右的时间里，我们将开始听到公司和开发人员讲述他们使用 Slate 的经历——他们是如何克服 Quill 和 Draft 中已知的缺陷的。也许它背后的社区会修改它，使它成为一个真正可行的解决方案。会取代鹅毛笔和草稿吗？我们现在还不知道。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

## 您是否添加了新的 JS 库来提高性能或构建新特性？如果他们反其道而行之呢？

毫无疑问，前端变得越来越复杂。当您向应用程序添加新的 JavaScript 库和其他依赖项时，您将需要更多的可见性，以确保您的用户不会遇到未知的问题。

LogRocket 是一个前端应用程序监控解决方案，可以让您回放 JavaScript 错误，就像它们发生在您自己的浏览器中一样，这样您就可以更有效地对错误做出反应。

[![LogRocket Dashboard Free Trial Banner](img/e8a0ab42befa3b3b1ae08c1439527dc6.png)](https://lp.logrocket.com/blg/javascript-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/javascript-signup)

[LogRocket](https://lp.logrocket.com/blg/javascript-signup) 可以与任何应用程序完美配合，不管是什么框架，并且有插件可以记录来自 Redux、Vuex 和@ngrx/store 的额外上下文。您可以汇总并报告问题发生时应用程序的状态，而不是猜测问题发生的原因。LogRocket 还可以监控应用的性能，报告客户端 CPU 负载、客户端内存使用等指标。

自信地构建— [开始免费监控](https://lp.logrocket.com/blg/javascript-signup)。