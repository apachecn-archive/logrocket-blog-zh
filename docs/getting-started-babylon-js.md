# Babylon.js 入门

> 原文：<https://blog.logrocket.com/getting-started-babylon-js/>

在 web 浏览器中构建 3D 图形从未如此简单。加入我的旅程，我们将向您展示如何使用 Babylon.js 创建一个基本场景。

## 目标

本文旨在指导您如何:

1.  创建一个 Vue 组件
2.  创建一个巴比伦类
3.  在画布上渲染一个场景
4.  创建三维网格

## 先决条件

这篇文章是一个初学者友好的教程，尽可能少的麻烦。然而，我建议你对 JavaScript 有很深的理解。

## 装置

### 安装 Vue

首先，我们的工作区需要 Vue3。为此，我们在终端中键入以下命令:

```
npm install -g @vue/cli
```

(注意:您需要安装 Node.js，因为随着我们的继续，您将需要节点包管理器)

在终端中，我们使用下面的命令创建一个新项目，文件夹名为 **bb101** :

```
vue create bb101
```

创建项目文件夹后，系统会提示我们进行一些选择。请执行以下操作:

1.  首先，我们想要手动选择我们想要的特性，所以我们从选择它开始
2.  然后，我们将 TypeScript 添加到我们想要的已经存在的特性列表中
3.  接下来，我们选择 3x (Vue 3)作为项目中使用的 Vue.js 版本
4.  我们通过键入“n”选择 **No** 作为以下两个选项的响应
5.  我们将使用“仅带有错误预防的 ESLint”和“保存时的 Lint”作为我们 linter 的附加 lint 功能
6.  为了放置 ESLint 的配置，我们选择选项“在专用配置文件中”,并选择**否**,为将来的项目保存预置

现在，我们等待一段时间来安装这些流程。接下来，我们使用命令`cd bb101`将终端中的目录更改为我们正在处理的项目的目录，并使用`npm run serve`运行我们的 Vue 应用程序。一旦编译完成，我们将有一个本地主机服务器在浏览器中打开。

![Localhost Server Compiled Example](img/052696ed7306c32c6b2de983bcbdb143.png)

### 安装 Babylon.js

我们需要将 Babylon 包安装到我们的项目中。在这个项目中，我们将利用几个 Babylon 包，但是现在，让我们从 Babylon 的核心包开始。为此，我们在终端中使用以下命令:

```
npm install @babylonjs/core

```

上面的命令会将 babylon.js 安装到我们项目的节点模块文件夹中。安装完成后，我们可以进行下一步。

## 入门指南

### 创建 Vue 组件(baby lone . Vue)

我们首先修改组件文件夹中的默认 helloworld.vue 文件。我们希望重用这个组件，但是它的名字是 BabylonOne，而不是 HelloWorld。

在我们新命名的 baby lone . vue 文件中，我们将清除 HTML 部分中的默认内容(开始和结束 div 中的所有内容),并对 HTML 部分和脚本标记部分进行一些更改，替换为以下内容:

```
<template>
<div>
 <h3>BabylonOne</h3>
 <canvas></canvas>
</div>
</template>
<script lang="ts">
import { defineComponent } from 'vue';
export default defineComponent({
name: 'BabylonOne',
mounted(){ //lifecycle hook
const canvas = document.querySelector("canvas")
 }
});
</script>

```

更改初始文件名将触发一个警告，因为 **HelloWorld** 是 App.vue 文件中使用的名称。在 App.vue 文件中，我们将清除模板标签中的内容，并将之前的组件名改为 BabylonOne(在每一行 **HelloWorld** 所在的地方都这样做)。在我们使用`npm run serve`在终端和浏览器中运行应用程序后，结果应该如下所示:

![Vue Component Result](img/ef465e1fcef90ced9acd0700f15318e0.png)

### 创建巴比伦级

在本节中，我们想要为 Babylon 创建一个 TypeScript 类。为此，我们将在 src 文件夹中创建一个名为**babylone**的子文件夹。

在这个文件夹中，我们将创建一个名为 **BabylonScene** 的新 TypeScript 文件。在这个文件中，我们将从我们的 Babylon 核心包中导入**场景**和**引擎**，并且我们将创建一个名为 **BabylonScene** 的类。

在这个类中，我们将创建一个场景和引擎变量以及一个构造函数，在创建这个类的实例时，我们会自动调用这个构造函数。我们需要构造函数来获取在我们的 Vue 组件中创建的画布元素。

在我们的场景变量中，我们将类型指定为**场景**，将类型的引擎变量指定为**引擎**。接下来，我们将引擎变量添加到我们的构造函数中，并将抗锯齿设置为 **True** 。

我们将在构造函数之外创建一个单独的方法，并将其赋给场景变量**的场景变量**。最后，我们希望在引擎运行的同时渲染场景。因此，我们将`use runRenderLoop`这样做。下面是上述内容的一个实现:

```
import { Scene, Engine } from "@babylonjs/core"
export class BabylonScene {
  scene: Scene;
  engine: Engine;
  constructor(private canvas: HTMLCanvasElement) {
    this.engine = new Engine(this.canvas, true);
    this.scene = this.CreateScene();
    this.engine.runRenderLoop(() => {
      this.scene.render();
    });
  }
  CreateScene(): Scene {
    const scene = new Scene(this.engine);
    return scene;
  }
}

```

### 在 Vue 中渲染场景

为此，我们返回到**babylone**文件，并从 BabylonScene.ts 文件导入 **BabylonScene** 类。在 mounted 中，我们用参数“canvas”调用 BabylonScene 类。以下是脚本标签目前的样子:

```
<script lang="ts">
import { defineComponent } from 'vue';
import { BabylonScene } from '@/BabylonOne/BabylonScene';
export default defineComponent({
 name: 'BabylonOne',
 mounted(){ //lifecycle hook
  const canvas = document.querySelector("canvas")!;
  new BabylonScene(canvas)
 }
});
</script>

```

(注意:在画布变量的右括号后面添加一个感叹号)

一旦完成，我们将在我们的终端测试它，我们的结果如下:

![Terminal Test Scene](img/673966eba402ed1bdea0ff3c31b78cef.png)

### 修改 CSS 并添加摄像头和半球照明

我们希望画布大小大约是屏幕大小的 70-80%。我们想要创建一个宽度和高度为 70%的画布 CSS。一旦我们实现了上述内容，我们的画布将如下所示:

现在，我们想在画布上看到一些东西——因此，我们将添加一个摄像机、一个光源和一些 3D 对象(一个地面和一个球体)。为此，我们将以下代码添加到 BabylonScene.ts 文件中的 CreateScene 方法中:

```
const Camera = new FreeCamera("camera", new Vector3(0,1,-5), this.scene);
Camera.attachControl();
const light = new HemisphericLight("light", new Vector3(0,1,0), this.scene);
light.intensity = 0.5;
//3D Object
const ground = MeshBuilder.CreateGround("ground", {width: 10, height:10}, this.scene);
const sphereball = MeshBuilder.CreateSphere("sphereball", {diameter:1}, this.scene);
sphereball.position = new Vector3(0,1,0)

```

### 解释代码片段

在创建一个 camera 变量时，我们将其值赋为 **FreeCamera** ，将其名称、起始位置、场景分别定义为 **camera、new Vector3(0，1，-5)** 、 *this.scene* 。要用鼠标控制摄像机，我们使用`attachControl`方法。

为了让相机工作，我们需要增加光线来看到我们周围的物体。为了实现这一点，我们将创建一个光线变量，并将其值指定为**半球光线**。我们将添加一个类似于 camera 变量的名称、起始位置和场景。

最后，我们将添加强度来调整**半球光**的亮度(注意，默认情况下，强度设置为 1，使环境过于明亮)。

对于 3D 对象，我们将创建一个地面和一个球体来表示我们环境中的 3D 对象。为了创建一个地面，我们创建一个地面变量并赋值`MeshBuilder.CreateGround`，同时设置名称、宽度和高度以及场景。

我们还使用 Meshbuilder 方法创建一个球形球，同时设置名称、直径和场景。要修改球的位置，我们将使用 position 方法，并将其分配到一个起始位置。

在实现上面的代码后，我们应该得到如下结果:

![Babylon 3D Mesh Example](img/2552c773b90872e78a88690ba13a5e50.png)

## 结论

js 是一个多功能的 3D JavaScript 库，能够用 3D 做任何可以想象的事情。

Babylon.js 是一个完美的 3D 库，唯一显著的缺点是它的包大小。在本教程中，我们已经向你展示了如何创建一个 Vue 组件，一个 Babylon 类，在画布上渲染一个场景并创建一个 3D 网格

感谢阅读和快乐编码！

## 像用户一样体验您的 Vue 应用

调试 Vue.js 应用程序可能会很困难，尤其是当用户会话期间有几十个(如果不是几百个)突变时。如果您对监视和跟踪生产中所有用户的 Vue 突变感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/vue-signup)

.

[![LogRocket Dashboard Free Trial Banner](img/0d269845910c723dd7df26adab9289cb.png)](https://lp.logrocket.com/blg/vue-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/vue-signup)

LogRocket 就像是网络和移动应用程序的 DVR，记录你的 Vue 应用程序中发生的一切，包括网络请求、JavaScript 错误、性能问题等等。您可以汇总并报告问题发生时应用程序的状态，而不是猜测问题发生的原因。

LogRocket Vuex 插件将 Vuex 突变记录到 LogRocket 控制台，为您提供导致错误的环境，以及出现问题时应用程序的状态。

现代化您调试 Vue 应用的方式- [开始免费监控](https://lp.logrocket.com/blg/vue-signup)。