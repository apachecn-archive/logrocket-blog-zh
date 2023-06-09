# 防止和检测 Android 应用程序中的内存泄漏

> 原文：<https://blog.logrocket.com/preventing-detecting-memory-leaks-android-apps/>

当用户在移动设备上打开应用程序时，该应用程序会获得分配的资源，这些资源负责保持应用程序正常运行。但是，这些设备的内存有限。如果应用程序内存使用量和需求增加，应用程序将会因为没有可用内存可分配而持续崩溃。

为了确保内存的有效使用，设备使用垃圾收集器。垃圾收集器有助于在应用程序运行时清除内存，从而释放应用程序不再需要的对象。因此，内存被回收，并确保设备不会崩溃。

但是，在某些情况下，垃圾收集器可能无法释放对象并占用它们的内存。这意味着即使应用程序不再需要内存，对象也会继续消耗内存，导致内存使用效率低下。这种情况就是我们所说的内存泄漏。

当一个应该被垃圾收集的对象拥有对它的引用时，就会发生内存泄漏。随着该对象的实例越来越多，旧的实例仍然保留在应用程序的内存中。它们在内存中的长时间停留最终会耗尽分配给应用程序的所有内存。用户将被告知应用程序的内存性能很差，应用程序最终会崩溃。

作为开发人员，我们有责任通过构建高效的内存使用来避免应用程序中出现这种情况。本指南将讨论如何使用 Android Studio 检测和防止 Android 应用程序中的这些内存泄漏。

### 内容

## 如何检测和报告 Android 应用程序中的内存泄漏

每个 Android 开发者都需要了解 Android 内存管理，如何处理，以及如何组织。其中一部分是识别内存泄漏，以便修复它们。

下面我们来讨论一下 Android 中检测内存泄漏的两大方法。为此，我们将创建一个带有有意内存泄漏的示例应用程序，然后用它来演示如何检测和报告泄漏。

### 创建示例 Android 应用程序

使用 Android Studio，创建一个新的 Android 项目，并遵循以下说明。

首先，创建一个单例类。单例类是一种设计模式，它只限制一个类中的一个对象，该对象在每次应用程序执行时被实例化一次。在这里，整个代码库中只存在一个实例，不能创建该对象的多个实例。它包含对自身的静态引用，因此可以从代码中的任何位置访问该引用。

我们将使用 Java 演示泄漏场景。然而，这个实例也适用于使用 Kotlin 编写的应用程序。

要创建单例，创建一个名为`SingletonClass`的新类。然后，创建对`SingletonClass`类的静态引用，如下所示:

```
public class SingletonClass {

    private static SingletonClass singletonClassInstance;

    private Context context;

    private SingletonClass(Context context){
        this.context = context;
    }

    public static void singletonClassInstance(Context context){

        if (singletonClassInstance == null){
            singletonClassInstance = new SingletonClass(context);

        }
    }
}

```

要执行这个类，在`MainActivity`的`onCreate`方法中初始化它的上下文:

```
SingletonClass.singletonClassInstance(this)

```

## 使用 Android Profiler 检测内存泄漏

Android Profiler 是一个 Android Studio 组件，它提供了一个集成视图，可以实时了解您的 Android 应用程序的性能。

我们将使用 Android Studio 中的内存分析器来了解它是如何工作的，以及如何使用内存分析器特性来分析内存。

要使用 Android Profiler，请确保您的计算机上安装了 Android Studio v.3.0 或更高版本。

首先，从你的 Android 工作室启动 Android Profiler。

Android 启动您的档案后，点击 **+** 按钮添加新的会话。确保选择运行应用程序的设备，并选择已创建的应用程序包。

创建季节后，将启动一个新的配置文件来监控您的应用程序的实时性能。我们对会话如何记录内存使用感兴趣。

通过单击蓝色行的任意位置来选择内存行。

![android profiler dashboard](img/0b00dceef50d01040b027484515dfcb6.png)

这将打开一个更详细的视图，向您显示应用程序如何消耗内存。例如，你可以看到一旦应用程序启动`MainActivity`，内存是如何用完的。

![mainactivity memory](img/0cfcd695403f9e64f9459a8ce167c3ab.png)

到目前为止，我们还没有发现代码中哪里可能会发生内存泄漏。我们需要跟踪内存分配，以分析垃圾收集并检测任何不需要的内存分配模式。

这里，我们需要捕获堆转储，并检查在给定时间对象使用的内存。确保您的分析器选择了**捕获堆转储**，并开始记录。产生结果需要一些时间。

## 使用 LeakCanary 检测内存泄漏

我们已经看到了如何使用 Android Profiler 来查找内存泄漏。对于开发人员来说，这是一个很好的工具，但是，它可能很耗时，尤其是在大型项目中。

幸运的是，有一种更快捷的替代方法叫做 [LeakCanary](https://square.github.io/leakcanary/) 。

LeakCanary 是一个 Android 内存泄漏检测器，帮助开发人员跟踪并减少`OutOfMemoryError`崩溃。它观察 Android 应用程序生命周期以监控活动和片段，记录并检测活动、片段、视图和视图模型何时被破坏，并垃圾收集它们的实例。

LeakCanary 使用`ObjectWatcher`来保存被销毁对象的弱引用。`AppWatcher`然后观看不再需要的对象。如果这些弱引用没有在五秒钟内被清除，那么被监视的实例将被认为是保留的，并被标记为可能泄漏的实例。

当应用程序运行且可见时，`ObjectWatcher`持有的实例达到五个保留对象的阈值时，LeakCanary 将 Java 堆转储到存储在文件系统中的`.hprof`文件中。然后，它分析堆，检查防止保存的实例被垃圾收集的引用链。

让我们用一个例子来理解这些信息。首先，将 LeakCanary 依赖项添加到 Android Studio 应用程序中，如下所示:

```
dependencies {
  //Add the debugImplementation as LeakCanary framework is supposed to only run in debug builds.
  debugImplementation 'com.squareup.leakcanary:leakcanary-Android:2.8.1'
}

```

一旦您运行应用程序 LeakCanary 将自动安装在您的设备上。打开 LeakCanary，你会看到泄漏的详细视图。

![Leakcanary dashboard](img/ff3eaceb1a3d9cf25675e9b65ba182e3.png)

**details** 屏幕显示了从垃圾收集器根到传递泄漏引用的对象的内存泄漏的跟踪。

## 常见的 Android 内存泄漏实例

许多实例会导致应用程序不同组件的内存泄漏。以下是您在编写代码时应该考虑的一些方面和提示。

### `Context`

`Context`允许应用程序在不同组件之间进行通信。它允许你创建新的对象，访问资源(布局，图像，字符串等)。)，以及 Android 设备的启动活动、数据库和内部存储。

有不同的方法可以用来访问上下文:`this`和`getApplicationContext`。

上下文保持对另一个组件的引用。您在应用程序中使用它们的方式起着关键作用。

让我们举一个我们之前用过的例子，一个单例类:

```
public class SingletonClass {

    private static SingletonClass singletonClassInstance;

    private Context context;

    private SingletonClass(Context context){
        this.context = context;
    }

    public static void singletonClassInstance(Context context){

        if (singletonClassInstance == null){
            singletonClassInstance = new SingletonClass(context);

        }
    }
}

```

在这种情况下，我们使用`SingletonClass.singletonClassInstance(this)`访问`MainActivity`中的`SingletonClass`类。为了获得`SingletonClass`数据，我们使用参数`this`来获得它的上下文。

在本例中，`context`是一个 Java 类。它提供了一种获取应用程序组件或其他操作系统功能信息的方法。

然而，您会注意到，使用`this`上下文执行`MainActivity`中的`SingletonClass`会泄漏活动。

`Context`与整个应用程序的生命周期息息相关。因此，上下文的任何错误使用都可能导致内存泄漏。确保检查何时何地使用不同的上下文。

例如，`getApplicationContext`可以在对象生命周期超过活动生命周期时使用。但是，它不能用于引用任何与 UI 相关的组件。如果你有一个独生子，确保你使用的是`ApplicationContext`。

此外，当对象没有超过活动生命周期时，可以使用`this`。它可以用来引用 UI 组件。UI 组件不是长期运行的操作，不能超越活动生命周期。`This`上下文可用于不同的操作，如 XML 布局、对话、获取资源或开始活动。

在我们的例子中，我们有一个内存泄漏，因为我们没有使用适当的上下文。让我们试着修理它。我们使用的是`SingletonClass`，因此只能有一个上下文实现对象，所以使用`getApplicationContext`是合适的。

`getApplicationContext`是单例上下文。无论您访问上下文多少次，您都将获得相同的实例。因此，它的实例不会创建新的上下文。

执行如下所示的`SingletonClass`将解决内存泄漏问题:

```
SingletonClass.singletonClassInstance(getApplicationContext());

```

### 静态引用

过度使用静态成员有时会导致应用程序中的内存泄漏。静态成员具有更长的生命周期，几乎在应用程序运行时都可以保持活动状态。当应用程序将一个类加载到 Java 虚拟机(JVM)中时，它的静态成员被分配到内存中。由于它们的寿命增加了，它们将一直保留在内存中，直到该类有资格进行垃圾回收。

让我们创建一个静态视图，看看它如何处理内存泄漏。

使用静态变量从 XML 文件初始化这个`TextView`:

```
private static TextView textView;

```

创建一个类来更新`TextView`值:

```
private void changeText() {
    textView = (TextView) findViewById(R.id.testview);
    textView.setText("Update Hello World greetings!");
}

```

现在，执行`onCreate()`方法中的类:

```
changeText();

```

注意，这个静态视图是执行`changeText()`类的活动的一部分。因此，它将保存对该特定活动的静态引用。静态视图甚至会在活动的生命周期结束后继续运行。这样，活动就不会被垃圾收集，因为视图仍然保存着对活动的引用。这将为该活动造成内存泄漏。

Static 用于在所有对象之间共享给定类的相同变量。如果视图必须静态保存，我们可以在一个`onDestroy()`中销毁它的引用以避免内存泄漏。这样，当活动被销毁时，其静态引用也将被销毁，从而允许活动被垃圾回收:

```
@Override
protected void onDestroy() {
    super.onDestroy();
    textView = null;
}

```

这个例子会有效；然而，为了避免这种情况发生，最佳实践是总是在不使用关键字 static 的情况下初始化视图。如果没有必要，最好不要静态持有:

```
private TextView textView;

```

下面是另一个静态引用活动上下文的例子，它会导致活动泄漏:

```
private static Context mContext;

```

在`onCreate()`方法中执行它:

```
mContext = this;

```

即使是 Android Studio 也会警告您可能存在与这个静态字段相关的泄漏。

![Android Studio memory leak warning](img/f9d8693aa2bc1b7d61188c5441147402.png)

要解决这个问题，最好不要静态持有。如果必须将它放在静态字段中，请使用虚引用/弱引用来保存它:

```
private static WeakReference<Context> mContext;

```

在`onCreate()`方法中执行它:

```
mContext = new WeakReference<> (this);

```

您也可以通过在`onDestroy()`方法中将它设置为`null`来解决这个问题。

### 线程代码

线程代码极有可能在您的应用程序中引入内存泄漏。线程将一个执行逻辑分解成多个并发任务。

Android 使用线程来处理并发执行的多个任务。线程没有自己的执行环境，所以它们从父任务继承执行环境。因此，线程可以在单个进程的范围内轻松地相互通信和交换数据。

让我们看一下一个基本线程如何导致 Android 中的内存泄漏。

首先，初始化一个线程任务:

```
private final ThreadedTask thread = new ThreadedTask();

```

接下来，设置一个线程任务:

```
private class ThreadedTask extends Thread {
    @Override
    public void run() {
        // Run the ThreadedTask for some time
        SystemClock.sleep(1000 * 20);
    }
}

```

最后，在`onCreate()`方法中执行任务:

```
thread.start();

```

当`ThreadedTask`启动时，它的执行需要一段时间才能完成。如果在任务执行结束前关闭活动，运行的`ThreadedTask`将阻止活动被垃圾纠正。如果不小心，在后台发生的事情中引用`view`、`activity`或`context`可能会导致内存泄漏。

要修复这个漏洞，您可以使用一个静态类。静态类没有对封闭活动类的引用。或者，您可以在活动被销毁时使用`onDestroy()`来停止这个线程:

```
// make ThreadedTask static to remove reference to the containing activity
private static class ThreadedTask extends Thread {
    @Override
    public void run() {
        // check if the thread is interrupted
        while (!isInterrupted()) {
            // Run the ThreadedTask for some time
            SystemClock.sleep(1000 * 20);
        }
    }
}

```

如果活动被破坏，`isInterrupted()`将返回`true`，线程将被停止:

```
@Override
protected void onDestroy() {
    super.onDestroy();
    //kill the thread in activity onDestroy
    thread.interrupt();
}

```

### 处理程序线程

处理程序是一个 Java 后台线程。它继续在后台运行，并按顺序执行不同的任务，直到应用程序退出线程执行。

Handler 主要用于与应用程序 UI 进行通信，并基于执行线程更新不同的组件。进度条是处理程序应用程序的一个很好的例子。该处理程序将使用 loopers 来创建消息队列，因此您可以使用它来调度消息并基于不同的重复任务更新 UI。

因为处理程序是线程并且执行多次，所以根据您编写它们的方式，有可能发生内存泄漏。

下面是 Android 中的一个基本处理程序。

首先，初始化处理程序任务。

```
private final Handler handler = new Handler(Looper.getMainLooper());

```

然后，执行`onCreate()`方法中的任务:

```
handler.postDelayed(new Runnable() {
    @Override
    public void run() {
        textView.setText("Handler execution done");
    }
    // delay its execution.
}, 1000 * 10);

```

当执行该处理程序时，它会在活动中注册一个回调。这将防止活动被垃圾收集，从而导致内存泄漏。

要解决这个问题，您必须确保删除所有回调。线程在单个进程的范围内相互通信和交换数据。因此，当调用`onDestroy()`方法时，必须移除相关的回调。

这将删除处理程序引用并解决内存泄漏问题:

```
@Override
protected void onDestroy() {
    super.onDestroy();
    //remove the handler references and callbacks.
    handler.removeCallbacksAndMessages(null);
}

```

在很多情况下，线程可能会在应用程序中泄漏。为了确保线程化执行写得很好，确保从线程创建时到终止时，[线程生命周期](https://www.javatpoint.com/thread-concept-in-java)被完全执行。此外，确保观察从内部类到外部(父)类的任何隐式引用

有许多可能发生泄漏的情况。可能发生泄漏的其他情况包括:

*   听众
*   可观察量
*   一次性用品
*   碎片
*   惰性绑定
*   `ListView`装订
*   位图对象
*   内部类——非静态内部类和匿名内部类
*   `AsyncTask`
*   位置经理
*   资源对象，如光标或文件

## 结论

即使是有经验的 Android 开发人员也很容易忽略内存泄漏。以上是一些可能发生泄漏的常见场景。但是，基于您的代码，泄漏可能发生在应用程序的任何部分。

最佳实践是始终使用所讨论的任何方法运行您的应用程序，这样您就可以在发布应用程序之前发现并防止内存泄漏。

## 使用 [LogRocket](https://lp.logrocket.com/blg/signup) 消除传统错误报告的干扰

[![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/signup)

[LogRocket](https://lp.logrocket.com/blg/signup) 是一个数字体验分析解决方案，它可以保护您免受数百个假阳性错误警报的影响，只针对几个真正重要的项目。LogRocket 会告诉您应用程序中实际影响用户的最具影响力的 bug 和 UX 问题。

然后，使用具有深层技术遥测的会话重放来确切地查看用户看到了什么以及是什么导致了问题，就像你在他们身后看一样。

LogRocket 自动聚合客户端错误、JS 异常、前端性能指标和用户交互。然后 LogRocket 使用机器学习来告诉你哪些问题正在影响大多数用户，并提供你需要修复它的上下文。

关注重要的 bug—[今天就试试 LogRocket】。](https://lp.logrocket.com/blg/signup-issue-free)