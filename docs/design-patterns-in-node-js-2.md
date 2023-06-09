# Node.js 中的设计模式:第 2 部分

> 原文：<https://blog.logrocket.com/design-patterns-in-node-js-2/>

欢迎回到 Node.js 中的**设计模式的另一部分，这是第二部分，但是如果你想回顾第一部分，在那里我讨论了**生活**、**工厂方法**、**单例**、**观察者**和**责任链**模式，请随意[查看](https://blog.logrocket.com/design-patterns-in-node-js/)，我会在这里等候。**

但是如果你不感兴趣或者可能已经知道了它们，请继续阅读，因为今天我将讨论四个以上的模式。

我会尽可能多地提供真实生活中的用例，并将理论上的恶作剧保持在最低限度(这方面总有 [Wikipedia](https://en.wikipedia.org/wiki/Software_design_pattern) )。

让我们愉快地复习句型，好吗？

## 模块模式

模块模式无疑是最常见的模式之一，因为它似乎是出于控制共享什么和隐藏什么的需要而诞生的。

让我解释一下。Node.js(以及一般的 JavaScript)中一个非常常见的做法是将代码组织成模块(即一组相互关联的函数，因此您将它们分组到一个文件中并导出)。默认情况下，Node 的模块允许你选择分享什么和隐藏什么，所以没有问题。

但是，如果您使用的是普通的旧 JavaScript，或者在同一个文件中有几个模块，这种模式可以帮助您隐藏部分，同时让您选择要共享的内容。

## 它看起来像什么？

这个模块很大程度上依赖于生活模式，所以如果你不确定它是如何工作的，可以看看我以前的文章。

你创建一个模块的方法就是创建一个生命，就像这样:

```
const myLogger = ( _ => {
    const FILE_PATH = "./logfile.log"
    const fs = require("fs")
    const os = require("os")

    function writeLog(txt) {
        fs.appendFile(FILE_PATH, txt + os.EOL, err => {
            if(err) console.error(err)
        })
    }

    function info(txt) {
        writeLog("[INFO]: " + txt)
    }

    function error(txt) {
        writeLog("[ERROR]: " + txt)
    }
    return {
        info, 
        error
    }
})()

myLogger.info("Hey there! This is an info message!")
myLogger.error("Damn, something happened!")
```

现在，使用上面的代码，您实际上模拟了一个只导出`info`和`error`函数的模块(当然，前提是您使用 Node.js)。

代码示例非常简单，但是您仍然明白这一点，您可以通过创建一个类来获得类似的结果，是的，但是您正在失去隐藏方法的能力，例如`writeLog`或者甚至是我在这里使用的常量。

## 模块模式的用例

这是一个非常简单的模式，所以代码本身就说明了一切。也就是说，我可以介绍在您的代码中使用这种模式的一些直接好处。

### 更清晰的名称空间

通过使用模块模式，您可以确保导出函数所需的全局变量、常量或函数不会对所有用户代码都可用。我所说的用户代码是指任何会使用你的模块的代码。

这有助于保持事物的有序，避免命名冲突，甚至避免用户代码通过修改任何可能的全局变量来影响函数的行为。

*免责声明:*我不宽恕也不是说全局变量是一个好的编码标准或者你应该尝试做的事情，但是考虑到你把它们封装在你的模块范围内，它们不再是全局的了。所以在使用这种模式之前一定要三思，还要考虑它提供的好处！

### 避免导入名称冲突

我来解释一下这个。如果您碰巧使用了几个外部库(尤其是当您为浏览器使用普通 JavaScript 时)，它们可能会将代码导出到同一个变量中(名称冲突)。因此，如果你不像我将要展示的那样使用模块模式，你可能会遇到一些不想要的行为。

你用过 jQuery 吗？还记得一旦你把它包含到你的代码中，除了`jQuery`对象，你还可以在全局范围内使用`$`变量吗？嗯，当时有几个其他图书馆也在做同样的事情。所以如果你想让你的代码通过使用`$`与 jQuery 一起工作，你必须做这样的事情:

```
( $ => {
   var hiddenBox = $( "#banner-message" );
   $( "#button-container button" ).on( "click", function( event ) {
     hiddenBox.show();
   });
})(jQuery);
```

这样，你的模块是安全的，如果包含在其他已经使用了`$`变量的代码库中，也没有发生命名冲突的风险。最后一点是最重要的，如果你开发的代码将被其他人使用，你需要确保它是兼容的，所以使用模块模式允许你清理名称空间并避免名称冲突。

## 适配器模式

适配器模式是另一种非常简单但功能强大的模式。本质上，它帮助您将一个 API(这里的 API 是指特定对象拥有的一组方法)应用到另一个 API 中。

我的意思是适配器基本上是一个特定类或对象的包装器，它提供不同的 API 并在后台使用对象的原始 API。

## 它看起来像什么？

假设一个记录器类如下所示:

```
const fs = require("fs")

class OldLogger { 

    constructor(fname) {
        this.file_name = fname
    }

    info(text) {
        fs.appendFile(this.file_name, `[INFO] ${text}`, err => {
            if(err) console.error(err)
        })
    }

    error(text) {
        fs.appendFile(this.file_name, `[ERROR] ${text}`, err => {
            if(err) console.error(err)
        })
    }
}
```

您已经有了使用它的代码，就像这样:

```
let myLogger = new OldLogger("./file.log")
myLogger.info("Log message!")
```

如果突然，记录器将其 API 更改为:

```
class NewLogger { 

    constructor(fname) {
        this.file_name = fname
    }

    writeLog(level, text) {
        fs.appendFile(this.file_name, `[${level}] ${text}`, err => {
            if(err) console.error(err)
        })
    }
}
```

然后，您的代码将停止工作，当然，除非您为您的记录器创建一个适配器，如下所示:

```
class LoggerAdapter {

    constructor(fname) {
        super(fname)
    }

    info(txt) {
        this.writeLog("INFO", txt)
    }

    error(txt) {
        this.writeLog("ERROR", txt)
    }
}
```

这样，您就为新的记录器创建了一个不再符合旧 API 的适配器(或包装器)。

## 适配器模式的用例

这种模式非常简单，但是我将要提到的用例非常强大，因为它们有助于隔离代码修改和减少可能的问题。

一方面，通过为现有模块提供适配器，您可以使用它来为现有模块提供额外的兼容性。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

举个例子，包 [request-promise-native](https://www.npmjs.com/package/request-promise-native) 为 [request](https://www.npmjs.com/package/request) 包提供了一个适配器，允许您使用基于承诺的 API，而不是 request 提供的默认 API。

因此，使用 promise 适配器，您可以执行以下操作:

```
const request = require("request")
const rp = require("request-promise-native")

request //default API for request
  .get('http://www.google.com/', function(err, response, body) {
    console.log("[CALLBACK]", body.length, "bytes") 
  })

rp("http://www.google.com") //promise based API
  .then( resp => {
    console.log("[PROMISE]", resp.length, "bytes")
  })
```

另一方面，您也可以使用适配器模式来包装您已经知道将来可能会更改其 API 的组件，并编写与您的适配器的 API 一起工作的代码。如果你的组件改变了 API 或者必须被完全替换，这将帮助你避免将来的问题。

存储组件就是这样一个例子，你可以写一个包装你的 [MySQL](https://www.npmjs.com/package/mysql) 驱动的组件，并提供通用的存储方法。如果将来你需要为一个 [AWS RDS](https://aws.amazon.com/rds/) 改变你的 MySQL 数据库，你可以简单地重写适配器，使用那个模块代替旧的驱动程序，你的代码的其余部分可以保持不受影响。

## 装饰图案

装饰模式无疑是我最喜欢的五种设计模式之一，因为它以一种非常优雅的方式帮助扩展了对象的功能。这种模式用于在运行时动态扩展甚至改变对象的行为。这种效果看起来很像类继承，但是这种模式允许您在同一个执行过程中在行为之间切换，这是继承所不具备的。

这是一个如此有趣和有用的模式，以至于有一个正式的提议将它合并到语言中。如果你想了解它，你可以在这里找到它。

## 这个图案是什么样子的？

由于 JavaScript 灵活的语法和解析规则，我们可以很容易地实现这种模式。实际上，我们所要做的就是创建一个装饰函数，该函数接收一个对象并返回装饰后的版本，无论是新的方法和属性还是已更改的方法和属性。

例如:

```
class IceCream { 
    constructor(flavor) {
        this.flavor = flavor
    }

    describe() {
        console.log("Normal ice cream,", this.flavor, " flavored")
    }
}

function decorateWith(object, decoration) {
    object.decoration = decoration
    let oldDescr = object.describe //saving the reference to the method so we can use it later
    object.describe = function() {
        oldDescr.apply(object)
        console.log("With extra", this.decoration)
    }
    return object
}

let oIce = new IceCream("vanilla") //A normal vanilla flavored ice cream...
oIce.describe() 

let vanillaWithNuts = decorateWith(oIce, "nuts") //... and now we add some nuts on top of it
vanillaWithNuts.describe()
```

正如您所看到的，这个例子实际上是在装饰一个对象(在这个例子中，是我们的香草冰淇淋)。在这种情况下，装饰器添加了一个属性并覆盖了一个方法，注意我们仍然在调用方法的原始版本，这要感谢我们在覆盖之前保存了对它的引用。

我们也可以很容易地向它添加额外的方法。

## 装饰模式的用例

实际上，这种模式的要点是将新的行为封装到不同的函数或额外的类中，这些函数或类将修饰您的原始对象。这将使您能够以最少的努力单独添加额外的代码或更改现有的代码，而不必处处影响您的相关代码。

也就是说，下面的示例试图用一家披萨公司的后端来准确地展示这一点，尝试计算一个披萨的价格，该披萨可以根据添加的浇头有不同的价格:

```
class Pizza {
    constructor() {
        this.base_price = 10
    }
    calculatePrice() {
        return this.base_price
    }
}

function addTopping(pizza, topping, price) {

    let prevMethod = pizza.calculatePrice
    pizza.toppings = [...(pizza.toppings || []), topping]
    pizza.calculatePrice = function() {
        return price + prevMethod.apply(pizza)
    }
    return pizza
}

let oPizza = new Pizza()

oPizza = addTopping(
            addTopping(
                oPizza, "muzzarella", 10
            ), "anana", 100
        )

console.log("Toppings: ", oPizza.toppings.join(", "))
console.log("Total price: ", oPizza.calculatePrice())
```

我们正在做一些类似于上一个例子的事情，但是用了一个更现实的方法。对`addTopping`的每个调用都会以某种方式从前端进入后端，由于我们添加额外浇头的方式，我们将对`calculatePrice`的调用一直链接到原始方法，该方法只返回比萨饼的原始价格。

想一个更相关的例子——文本格式。这里我在我的 bash 控制台中格式化文本，但是您可以为您的所有 UI 格式化实现这一点，添加具有小变化和其他类似情况的组件。

```
const chalk = require("chalk")

class Text {
    constructor(txt) {
        this.string = txt
    }
    toString() {
        return this.string
    }
}

function bold(text) {
    let oldToString = text.toString

    text.toString = function() {
        return chalk.bold(oldToString.apply(text))
    }
    return text
}

function underlined(text) {
    let oldToString = text.toString

    text.toString = function() {
        return chalk.underline(oldToString.apply(text))
    }
    return text
}

function color(text, color) {
    let oldToString = text.toString

    text.toString = function() {
        if(typeof chalk[color] == "function") {
            return chalk\[color\](oldToString.apply(text))
        }
    }
    return text
}

console.log(bold(color(new Text("This is Red and bold"), "red")).toString())
console.log(color(new Text("This is blue"), "blue").toString())
console.log(underlined(bold(color(new Text("This is blue, underlined and bold"), "blue"))).toString())
```

[Chalk](https://www.npmjs.com/package/chalk) 顺便说一下，是一个很小很有用的库，用来在终端上格式化文本。对于这个例子，我创建了三个不同的装饰器，您可以像使用浇头一样使用它们，将它们各自的调用组合成最终结果。

上述代码的输出是:

![display of ui with chalk ](img/a1dcf880b0e4ed43cae1fbefaa9ba399.png)

## 命令模式

最后，我今天要回顾的最后一个模式是我最喜欢的模式——命令模式。这个小家伙允许你将复杂的行为封装在一个模块(或者类，请注意)中，这个模块可以由一个外部人员通过一个非常简单的 API 来使用。

这种模式的主要好处是，通过将业务逻辑分割成单独的命令类，所有命令类都使用相同的 API，您可以添加新的命令或修改现有代码，而对项目的其余部分影响最小。

## 它看起来像什么？

实现这种模式非常简单，你所要记住的就是为你的命令准备一个通用的 API。遗憾的是，由于 JavaScript 没有`Interface`的概念，我们无法使用这个构造来帮助我们。

```
class BaseCommand {
    constructor(opts) {
        if(!opts) {
            throw new Error("Missing options object")
        }
    }
    run() {
        throw new Error("Method not implemented")
    }
}

class LogCommand extends BaseCommand{
    constructor(opts) {
        super(opts)
        this.msg = opts.msg,
        this.level = opts.level
    }
    run() {
        console.log("Log(", this.level, "): ", this.msg)
    }
}

class WelcomeCommand extends BaseCommand {
    constructor(opts) {
        super(opts)
        this.username = opts.usr
    }
    run() {
        console.log("Hello ", this.username, " welcome to the world!")
    }
}

let commands = [
    new WelcomeCommand({usr: "Fernando"}),
    new WelcomeCommand({usr: "reader"}),
    new LogCommand({
        msg: "This is a log message, careful now...",
        level: "info"
    }),
    new LogCommand({
        msg: "Something went terribly wrong! We're doomed!",
        level: "error"
    })
]

commands.forEach( c => {
    c.run()
})
```

这个例子展示了创建不同命令的能力，这些命令有一个非常基本的`run`方法，您可以在这里放置复杂的业务逻辑。请注意我是如何使用继承来尝试并强制实现一些必需的方法的。

## 命令模式的用例

这种模式非常灵活，如果你处理得当，可以为你的代码提供很大的可伸缩性。

我特别喜欢将它与 [require-dir](https://www.npmjs.com/package/require-dir) 模块结合使用，因为它可以要求文件夹中的每个模块，所以您可以保留一个命令专用的文件夹，以命令命名每个文件。这个模块将在一行代码中需要它们，并返回一个对象，其中的关键字是文件名(即命令名)。这反过来允许你继续添加命令，而不必添加任何代码，只需创建文件并将其放入文件夹，你的代码将需要它并自动使用它。

标准的 API 将确保您调用正确的方法，所以同样，没有什么需要改变的。类似这样的东西会帮助你达到目的:

```
function executeCommand(commandId) {
  let commands = require-dir("./commands")
  if(commands[commandId]) {
    commands[commandId].run()  
  } else {
    throw new Error("Invalid command!")
  }
}
```

有了这个简单的功能，您就可以自由地不断扩展您的命令库，而无需做任何更改！这是一个精心设计的建筑的魔力！

在实践中，这种模式非常适合:

*   负责与菜单栏相关的操作
*   从客户端应用程序接收命令，例如游戏的情况，其中客户端应用程序不断向后端服务器发送命令消息，供其处理、运行命令并返回结果
*   一种聊天服务器，从不同的客户端接收事件，并需要单独处理它们

这个列表还可以继续下去，因为您几乎可以实现任何对基于命令的方法的某种形式的输入作出反应的东西。但是这里的要点是通过实现那个逻辑(不管它对你来说是什么)所增加的巨大价值。通过这种方式，您可以获得惊人的灵活性和伸缩或重构能力，而对其余代码的影响最小。

## 结论

我希望这有助于阐明这四种新模式、它们的实现和用例。理解何时使用它们，最重要的是，**为什么**你应该使用它们，这有助于你获得它们的好处并提高你的代码质量。

如果您对我展示的代码有任何问题或意见，请在评论中留言！

否则，**下一场见！**

## 200 只![](img/61167b9d027ca73ed5aaf59a9ec31267.png)显示器出现故障，生产中网络请求缓慢

部署基于节点的 web 应用程序或网站是容易的部分。确保您的节点实例继续为您的应用程序提供资源是事情变得更加困难的地方。如果您对确保对后端或第三方服务的请求成功感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/node-signup)

.

[![LogRocket Network Request Monitoring](img/cae72fd2a54c5f02a6398c4867894844.png)](https://lp.logrocket.com/blg/node-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/node-signup)

LogRocket 就像是网络和移动应用程序的 DVR，记录下用户与你的应用程序交互时发生的一切。您可以汇总并报告有问题的网络请求，以快速了解根本原因，而不是猜测问题发生的原因。

LogRocket 检测您的应用程序以记录基线性能计时，如页面加载时间、到达第一个字节的时间、慢速网络请求，还记录 Redux、NgRx 和 Vuex 操作/状态。

[Start monitoring for free](https://lp.logrocket.com/blg/node-signup)

.