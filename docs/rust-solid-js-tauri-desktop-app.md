# Rust、SolidJS 和 Tauri:创建一个跨平台的桌面应用程序

> 原文：<https://blog.logrocket.com/rust-solid-js-tauri-desktop-app/>

2022 年 12 月 15 日，GitHub [称之为 ATOM IDE](https://github.blog/2022-06-08-sunsetting-atom/) 的一天，作为第一个真正的电子应用程序，它在软件史上占有特殊的地位。(虽然，项目在一个社区里发现了[新生命](https://github.com/pulsar-edit/pulsar)[–](https://github.com/pulsar-edit/pulsar)[运行分叉叫脉冲星](https://github.com/pulsar-edit/pulsar))。

Electron 允许开发人员使用他们已经熟悉的标准 web 技术来为 Windows、Mac 和 Linux 构建跨平台的桌面应用程序。这对于 Atom、VSCode 和 Postman 等应用程序来说都很棒，但并不总是理想的。例如，对电子应用程序的一个常见批评是，相对于 C、C++、Rust 或 Go 等低级语言，它们通常会占用大量内存。

在这篇文章中，我们将探索一个为大多数主流桌面平台构建二进制文件的新框架。在本文的教程部分，我们将研究如何在 Tauri 中创建基本命令，如何添加窗口菜单，以及如何构建应用程序。我们开始吧！

为了跟进我们的简单示例，您可以在这里[访问完整代码](https://github.com/AlexMercedCoder/my-first-tauri-application)。

*向前跳转:*

## Tauri 是什么？

Tauri 是一个新的框架，提供了人们[最喜欢的关于 electronic](https://blog.logrocket.com/advanced-electron-js-architecture/)的东西，但是修复了许多安全和性能问题。Tauri 提供了使用 HTML、CSS 和 JavaScript 等 web 技术设计用户界面的能力，但允许您使用低级语言编写应用程序和后端逻辑。

在撰写本文时，Tauri 主要支持 Rust 作为后端语言，但它的 API 可以跨多种语言实现。随着时间的推移，我们可以期待 C、C++、Go、Ruby、Python，甚至 JavaScript 后端绑定，因为用户决定实现它们。

## Tauri 建筑概述

像 Electron 一样，Tauri 使用多进程方法。核心流程本质上是应用程序的后端，所有的数据模型和应用程序逻辑都是用 Rust 编写的。[WebView 进程](https://tauri.app/v1/guides/architecture/process-model/#the-webview-process)是构建在您选择的 JavaScript 框架中的应用程序 UI。您甚至可以使用非 JavaScript 选项，如使用 Dominator 或 ClojureScript 的 Rust。

这两个进程通过事件或命令(RPC 调用)进行通信。任一进程都可以发出事件，当另一个进程执行特定操作时，另一个进程可以侦听这些事件来触发一个进程中的逻辑。

WebView 进程可以发出命令来触发核心进程中的预定义功能。这些是基本的 [RPC 调用](https://main.grokoverflow.com/posts/2022/02-understanding-rpc-node-walkthrough)。

## 带 Tauri 去试驾

为了安装好所有的东西，包括[必备的系统库](https://tauri.app/v1/guides/getting-started/prerequisites)，我建议按照[官方文档中的指南](https://tauri.studio/v1/guides/getting-started/prerequisites)来做。你需要安装 Node.js 和 Rust。完成所有设置后，您可以通过运行以下命令来创建应用程序:

```
npx create-tauri-app

```

你会得到几个选项，选择什么样的包管理器，框架/语言用于 WebView。对于本教程，使用普通 JS 模板选择 **SolidJS** **。**

`cd`到新文件夹，运行`npm install`。然后，运行`npm run tauri dev`，这将打开核心和 WebView 进程，因此您可以在`localhost:3000`预览它。该页面实际上只是显示一个每秒计数的数字。

请注意终端输出，因为可能需要安装一些库和环境变量才能成功构建应用程序。我在一个 POP 操作系统上工作，在它拥有所有的依赖项之前，我必须安装几个库。

## 文件夹结构

我们的应用程序，我称之为`/Your-Tauri-App`，将使用以下文件夹结构:

```
/Your-Tauri-App
|
|-- /node_modules //All the NPM Libraries
|
|-- /src // Your frontend UI Code for the WebView Process
|
|-- /src-tauri // Your backend code for the Core process in Rust
|
|-- .gitignore // files to be ignored by git
|-- index.html // just the HTML file the WebView mounts to
|-- package-lock.json // lockfile for npm
|-- package.json // config file for npm
|-- pnpm-lock.yaml // lock file for pnpm
|-- readme.md // template readme
|-- jsconfig.json // javascript Configurations
|-- vite.config.js // Vite configurations

```

## 创建基本命令

您可以考虑在标准 web 应用程序的后端编写路线之类的命令。然而，我们将使用`invoke`从我们的前端调用这些命令中的一个，而不是使用`fetch`来请求路由。

让我们在`/src-tauri/src/main.rs`中创建一个非常基本的命令:

```
#![cfg_attr(
  all(not(debug_assertions), target_os = "windows"),
  windows_subsystem = "windows"
)]

// Our Tauri Command
#[tauri::command]
fn return_string(word: String) -> String{
    return word
}
fn main() {
  tauri::Builder::default()
     // Register Command with Tauri App
    .invoke_handler(tauri::generate_handler![return_string])
    .run(tauri::generate_context!())
    .expect("error while running tauri application");
}

```

`#[tauri::command]`宏将我们的函数变成一个命令，我们用 Tauri builder 中的`invoke_handler`方法在应用程序中注册我们的命令。

然后，使用`invoke`方法，我们从 Solid 调用这个命令。让我们更新我们的`App.js`文件:

```
import { invoke } from '@tauri-apps/api/tauri'
import {createSignal} from "solid-js"
function App() {
  const [word, setWord] = createSignal("")
  const handleClick = async (event) => {
    // invoke backend action and save result in variable
    const result = await invoke("return_string", {
          word: "You triggered the tauri action!"
        })
    // alert the return value
    setWord(result)
  }
  return (
    <div>
      <h1>Word signal is currently: {word}</h1>
      <button onClick={handleClick}>Trigger the Tauri Action</button>
    </div>
  );
}
export default App;

```

在上面的代码中，我们导入了调用方法`import { invoke } from '@tauri-apps/api/tauri';`。我们调用`invoke`并向它传递两个参数，一个字符串包含要调用的命令的名称，一个对象包含任何参数。

现在，当我们点击新的`**Click Me**` *按钮时，我们发送的文本将被发送回来，确认我们成功地从后端调用了该方法。就是这样！您可以使用 Rust 以及任何 Rust 库来创建命令，以满足您的应用程序的数据管理需求。*

 *您可能希望在应用程序窗口中添加菜单选项。这样做很简单。我们将从 Rust 代码中导入 Tauris 菜单定义工具:

```
//main.rs
use tauri::{CustomMenuItem, Menu, MenuItem, Submenu};

```

然后我们可以定义菜单项，如下所示:

```
// Define two sub items for the submenu
let hello = CustomMenuItem::new("hello".to_string(), "Hello");
let goodbye = CustomMenuItem::new("goodbye".to_string(), "Goodbye");
// define the submenu
let submenu = Submenu::new("Menu", Menu::new().add_item(hello).add_item(goodbye));
// create main menu object
let menu = Menu::new()
  // add native copy functionality
  .add_native_item(MenuItem::Copy)
  // add our submenu
  .add_submenu(submenu);

```

然后，我们可以将菜单传递给应用程序构建器:

```
fn main() {
  tauri::Builder::default()
    .menu(menu)
    .invoke_handler(tauri::generate_handler![return_string])
    .run(tauri::generate_context!())
    .expect("error while running tauri application");
}

```

整个文件应该如下所示:

```
#![cfg_attr(
  all(not(debug_assertions), target_os = "windows"),
  windows_subsystem = "windows"
)]
use tauri::{CustomMenuItem, Menu, MenuItem, Submenu};

// Our Tauri Command
#[tauri::command]
fn return_string(word: String) -> String{
    return word
}
fn main() {
  // Define two sub items for the submenu
  let hello = CustomMenuItem::new("hello".to_string(), "Hello");
  let goodbye = CustomMenuItem::new("goodbye".to_string(), "Goodbye");
  // define the submenu
  let submenu = Submenu::new("Menu", Menu::new().add_item(hello).add_item(goodbye));
  // create main menu object
  let menu = Menu::new()
  // add native copy functionality
  .add_native_item(MenuItem::Copy)
  // add our submenu
  .add_submenu(submenu);

  tauri::Builder::default()
    .menu(menu)
     // Register Command with Tauri App
    .invoke_handler(tauri::generate_handler![return_string])
    .run(tauri::generate_context!())
    .expect("error while running tauri application");
}

```

现在，您应该会在窗口中看到菜单。接下来，我们可以向构建器中添加代码，以将操作或方法与菜单项单击相关联:

```
#![cfg_attr(
  all(not(debug_assertions), target_os = "windows"),
  windows_subsystem = "windows"
)]
use tauri::{CustomMenuItem, Menu, MenuItem, Submenu};

// Our Tauri Command
#[tauri::command]
fn return_string(word: String) -> String{
    return word
}
fn main() {
  // Define two sub items for the submenu
  let hello = CustomMenuItem::new("hello".to_string(), "Hello");
  let goodbye = CustomMenuItem::new("goodbye".to_string(), "Goodbye");
  // define the submenu
  let submenu = Submenu::new("Menu", Menu::new().add_item(hello).add_item(goodbye));
  // create main menu object
  let menu = Menu::new()
  // add native copy functionality
  .add_native_item(MenuItem::Copy)
  // add our submenu
  .add_submenu(submenu);

  tauri::Builder::default()
    .menu(menu)
    //register menu events
    .on_menu_event(|event| {
      match event.menu_item_id() {
        "hello" => {
          event.window().maximize();
        }
        "goodbye" => {
          event.window().minimize();
        }
        _ => {}
      }
    })
    // Register Command with Tauri App
    .invoke_handler(tauri::generate_handler![return_string])
    .run(tauri::generate_context!())
    .expect("error while running tauri application");
}

```

我们根据点击的菜单项将`.on_menu_event`方法附加到模式匹配。在上面的代码中，我们让`"hello"`菜单项最大化窗口，让`"goodbye"`菜单项最小化窗口。

你可以在这里的 [R](https://docs.rs/tauri/1.2.2/tauri/window/struct.Window.html#method.maximize) [ust docs](https://docs.rs/tauri/1.2.2/tauri/window/struct.Window.html#method.maximize) 中看到许多我们可以在窗口上调用的[其他方法，比如关闭窗口或退出程序的方法。](https://docs.rs/tauri/1.2.2/tauri/window/struct.Window.html#method.maximize)

## 构建应用程序

在撰写本文时，Tauri 不支持交叉编译，所以它可以编译到你当前使用的任何操作系统上，在我的例子中是 Linux。

要启用编译，您所要做的就是运行`npm run tauri build`。只要确保将`tauri.config.json`中的 identifier 属性更新为您想要的属性即可。

对我来说，构建完成后，`src-tauri/target/bundle`中有`AppImage`和`Deb`文件。

## 调试应用程序

请记住，我们实际上同时运行两个应用程序:我们的 Solid 应用程序充当 UI，我们的 Rust 应用程序处理应用程序的窗口和后端功能。因此，调试这两种代码库可能会有所不同。

### 在 Linux 上调试编译

编译代码时，您可能会遇到与缺少系统库相关的错误，如下所示:

```
  Package cairo was not found in the pkg-config search path.
  Perhaps you should add the directory containing `cairo.pc'
  to the PKG_CONFIG_PATH environment variable
  No package 'cairo' found
  Package cairo was not found in the pkg-config search path.
  Perhaps you should add the directory containing `cairo.pc'
  to the PKG_CONFIG_PATH environment variable
  No package 'cairo' found

```

假设您安装了 Tauris 文档中列出的所有先决条件，您需要找到这些文件的路径，并定义`PKG_CONFIG_PATH`。对于 Linux，这些文件可能在`/usr`的某个地方，所以运行以下命令来搜索它们:

```
sudo find /usr -name cairo.pc

```

这应该会给你文件的确切位置。然后，您可以构建如下所示的命令来修复`PKG_CONFIG_PATH`:

```
export PKG_CONFIG_PATH=/usr/lib/x86_64-linux-gnu/pkgconfig:/usr/share/pkgconfig:$PACKAGE_CONFIG_PATH

```

将它添加到您的`.bashrc`中，这样您就不必在每个新的终端会话中运行它。

### 调试 Rust 代码

调试 Rust 代码时，可以使用 Rust print 命令向终端打印消息；当命令或菜单事件不能按预期工作时，这对于调试命令或菜单事件中的消息非常有用。

例如，我们可能想要确认 Rust 应用程序正在从我们创建的命令的 Solid add 接收参数。在这种情况下，我们可以在函数中包含一条调试消息，如下所示:

```
// Our Tauri Command
#[tauri::command]
fn return_string(word: String) -> String{
    // debugging message
    println!("The frontend sent us this argument {}", word);
    return word
}

```

当我们从应用程序的前端触发这个命令时，我们可以在终端中看到消息，并在 Rust 中确认收到。

### 调试 WebView

我们可以使用 JavaScript `console.log`进行调试，以记录前端 WebView 代码库中出现的代码的消息。通常，这些代码会出现在浏览器开发工具中，但是我们现在有了自己的原生窗口。

通过添加一个`debug`标志，我们可以使用普通的浏览器开发工具运行该应用程序:

```
npm run tauri dev --debug

```

现在，我们可以使用普通的键盘快捷键来打开我们正在运行的应用程序中的浏览器开发工具，以检查`console.log`和您在 web 浏览器环境中预期的所有其他方面。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

## 结论

Tauri 使用 web 技术和您最喜欢的前端框架开放桌面应用程序，以获得更好的性能和更小的包。Tauri 肯定会吸引许多电子开发者转向需要增强性能的应用。

你可以在 GitHub 上找到这个教程的代码。尽管我们只是触及了 Tauri 的皮毛，但我还是建议您仔细阅读以下很酷的特性:

编码快乐！

## [log rocket](https://lp.logrocket.com/blg/rust-signup):Rust 应用的 web 前端的全面可见性

调试 Rust 应用程序可能很困难，尤其是当用户遇到难以重现的问题时。如果您对监控和跟踪 Rust 应用程序的性能、自动显示错误、跟踪缓慢的网络请求和加载时间感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/rust-signup)

.

[![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/rust-signup)

LogRocket 就像是网络和移动应用程序的 DVR，记录你的 Rust 应用程序上发生的一切。您可以汇总并报告问题发生时应用程序的状态，而不是猜测问题发生的原因。LogRocket 还可以监控应用的性能，报告客户端 CPU 负载、客户端内存使用等指标。

现代化调试 Rust 应用的方式— [开始免费监控](https://lp.logrocket.com/blg/rust-signup)。*