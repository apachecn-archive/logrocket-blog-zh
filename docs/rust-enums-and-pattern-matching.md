# Rust 枚举和模式匹配

> 原文：<https://blog.logrocket.com/rust-enums-and-pattern-matching/>

模式匹配和枚举可以用于很多事情，比如错误处理、处理空值等等。在本文中，我们将回顾模式匹配的基础，枚举，并了解枚举如何用于模式匹配，包括:

但是，要阅读本文，您必须至少对 Rust 编程语言有一个基本的了解。

## Rust 中的模式匹配

模式匹配是编程语言的一种机制，它允许程序流根据给定的输入分支到多个分支之一。

假设我们有一个名为`name`的变量，它是一个代表人名的字符串。对于每个名称，我们显示一个水果，如下所示:

*   `John => papaya`
*   `Annie => blueberry`
*   `Michael => guava`
*   `Gabrielle => apple`
*   `Others => orange`

在这里，我们创建了五个分支；`=>`左边的名字代表一个名字模式，右边的果实是会显示的分支。所以如果`name`是`John`，我们显示一个`papaya`，如果`name`是`Annie,`，我们显示`blueberry`，以此类推。

但是，如果`name`的值不是注册的模式，则默认为`Others`。

在 Rust 中，我们使用`match`语句执行模式匹配，我们可以在下面的代码中使用前面的例子:

```
match name {
  "John" => println!("papaya"),
  "Annie" => println!("blueberry"),
  "Michael" => println!("guava"),
  "Gabrielle" => println!("apple"),
  _ => println!("orange"),
}
```

Rust 中的`match`语句的工作方式类似于 C++、Java、JavaScript 等其他编程语言中的`switch`语句。但是，`match`陈述比`switch`陈述有一些优势。

首先，它检查上面第一行的变量`name`是否匹配`=>`左侧的任何值，然后执行匹配模式右侧的值。

如果所有模式与`name`变量不匹配，则默认为`_`(匹配每个值)并在终端显示`orange`。这就像我们之前看到的`Others`图案一样。

像这样创建模式匹配是非常强大的，但是如果我们想要执行一行以上的代码，我们用一个包含您想要执行的所有行的代码块替换`match`块中`=>`的右侧。

在下面的例子中，我们修改了前面的`match`语句，为每个名字打印一个数字和一种颜色以及水果:

```
match name {
  "John" => {
    println!("4");
    println!("green");
    println!("papaya");
  },
  "Annie" => {
    println!("3");
    println!("blue");
    println!("blueberry");
  },
  "Michael" => {
    println!("2");
    println!("yellow");
    println!("guava");
  },
  "Gabrielle" => {
    println!("1");
    println!("purple");
    println!("apple");
  },
  _ => {
    println!("0");
    println!("orange");
    println!("orange");
  },
}
```

在这里，我们转换了简单的行:

```
_ => println!("orange"),
```

在`match`上执行多行代码的语句:

```
_ => {
    println!("0");
    println!("orange");
    println!("orange");
  },
```

有了这个，我们可以执行不止一行。而且，如果我们想要返回值，我们可以使用 return 语句或者 Rust 的 return 快捷方式。快捷方式是通过删除最后一个表达式的分号来实现的:

```
_ => {
    println!("0");
    println!("orange");
    println!("orange");
    "This is for the others"
  },
```

或者使用以下方法:

```
_ => {
    println!("0");
    println!("orange");
    println!("orange");
    return "This is for the others";
  },
```

单线模式也将表达式的值返回到`=>`的右侧，因此您可以执行以下操作:

```
let result = match name {
  "John" => "papaya",
  "Annie" => "blueberry",
  "Michael" => "guava",
  "Gabrielle" => "apple",
  _ => "orange",
};

println!("{}", result);
```

这与第一个`match`语句示例的作用相同。

## 在 Rust 中使用枚举

枚举是表示具有多个变量的数据类型的可靠数据结构。枚举可以执行与结构相同的操作，但使用的内存和代码行更少。

我们可以在操作中使用枚举的任何变体，但是，我们只能使用基本枚举来指定我们将从函数中返回一个变体或者将它赋给一个变量。

这意味着基本枚举本身不能赋给变量。举一个枚举的例子，让我们创建一个具有三个变量的`vehicle`枚举:`Car`、`MotorCycle`和`Bicycle`。

```
enum Vehicle {
  Car,
  MotorCycle,
  Bicycle,
}
```

然后，我们可以通过编写`Vehicle::<variant>`来访问变量:

```
let vehicle = Vehicle::Car;
```

如果你想静态地输入它，你可以这样写:

```
let vehicle: Vehicle = Vehicle::Car;
```

### 枚举模式匹配

正如我们可以对字符串、数字和其他数据类型执行模式匹配一样，我们也可以匹配枚举变量:

```
match vehicle {
    Vehicle::Car => println!("I have four tires"),
    Vehicle::MotorCycle => println!("I have two tires and run on gas"),
    Vehicle::Bicycle => println!("I have two tires and run on your effort")
  }
```

在此，我们:

1.  将变量传递到一个`match`语句中
2.  创建模式以匹配单个变体
3.  编码一旦变量匹配任何模式将执行什么

所以，我们可以写一个这样的程序:

```
enum Vehicle {
  Car,
  MotorCycle,
  Bicycle,
}

fn main() {
  let vehicle = Vehicle::Car;

  match vehicle {
    Vehicle::Car => println!("I have four tires"),
    Vehicle::MotorCycle => println!("I have two tires and run on gas"),
    Vehicle::Bicycle => println!("I have two tires and run on your effort")
  }
}
```

这将导致以下结果:

```
> rustc example.rs
> ./example

I have four tires
```

### 向枚举变量添加数据

我们还可以向枚举变量中添加数据。我们必须通过添加一个括号来指定我们的变量可以保存数据，括号中包含它将保存的数据类型:

```
enum Vehicle {
  Car(String),
  MotorCycle(String),
  Bicycle(String),
}
```

然后，我们可以这样使用它:

```
fn main() {
  let vehicle = Vehicle::Car("red".to_string());

  match vehicle {
    Vehicle::Car(color) => println!("I am {} and have four tires", color),
    Vehicle::MotorCycle(color) => println!("I am {} and have two tires and run on gas", color),
    Vehicle::Bicycle(color) => println!("I am {} and have two tires and run on your effort", color)
  }
}
```

在此，我们:

1.  创建枚举的新实例，并将其赋给一个变量
2.  将该变量放在一个`match`语句中
3.  将每个枚举变量的内容析构到`match`语句中的一个变量中
4.  在模式右侧的代码块中使用了析构变量。

## `Result`和`Option`枚举

`Result`和`Option`枚举是 Rust 中用于处理函数或变量的结果、错误和空值的标准库的一部分。

### `Result`枚举

这是 Rust 中一个非常常见的枚举，用于处理函数或变量的错误。它有两种变体:`Ok`和`Err`。

如果没有错误，`Ok`变量保存返回的数据，而`Err`变量保存错误消息。例如，我们可以创建一个返回枚举变量的函数:

```
fn divide(numerator: i32, denominator: i32) -> Result<i32, String> {
    if denominator == 0 {
        return Err("Cannot divide by zero".to_string());
    } else {
        return Ok(numerator / denominator);
    }
}
```

这个函数有两个参数。第一个是分子，第二个是分母。如果分母为零，则返回变量`Result::Err`，如果分母不为零，则返回结果`Result::Ok` 。

然后，我们可以使用模式匹配函数:

```
fn main() {
    match divide(103, 2) {
        Ok(solution) => println!("The answer is {}", solution),
        Err(error) => println!("Error: {}", error)
    }
}
```

在这个例子中；

1.  我们试图`divide 103 by 2`
2.  创建`Ok`模式匹配并提取它包含的数据。
3.  在此之下，创建一个获取任何错误消息的`Err`模式

`Result`枚举还允许我们在不使用`match`语句的情况下处理错误。这意味着我们可以使用以下任何或所有选项:

*   `Unwrap`获取`Ok`变量中的数据
*   `Unwrap_err`从`result`的`Err`变体中获取错误信息
*   如果值是一个`Err`变量，则`is_err`返回 true
*   `Is_ok`确定该值是否为`Ok`变量

    ```
    let number = divide(103, 2); if number.is_err() {   println!("Error: {}", number.unwrap_err()); } else if number.is_ok() {   println!("The answer is {}", number.unwrap()); }
    ```

Rust 中的所有`Result`枚举值都必须被使用，否则我们会收到来自编译器的警告，告诉我们有一个未使用的`Result`枚举。

### `Option`枚举

在 Rust 中使用`Option`枚举来表示可选值。它有两种变体:`Some`和`None`。

当输入包含值时使用`Some`，而`None`表示没有值。它通常与模式匹配语句一起使用，以处理函数或表达式中缺少可用值的情况。

所以在这里，我们可以修改`divide`函数，使用`Options`枚举来代替`Result`:

```
fn divide(numerator: i32, denominator: i32) -> Option<i32> {
    if denominator == 0 {
        return None;
    } else {
        return Some(numerator / denominator);
    }
}
```

在这个新的`divide`函数中，我们将返回类型从`Result`更改为`Option`，因此如果分母为零，则返回`None`，如果分母不为零，则返回结果`Some`。

然后，我们可以按以下方式使用新的`divide`函数:

```
fn main() {
    match divide(103, 0) {
        Some(solution) => println!("The answer is {}", solution),
        None => println!("Your numerator was about to be divided by zero :)")
    }
}
```

在这个主函数中，我们将`Ok`变量从`Result`改为`Some`，将`Err`改为`None`。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

就像使用`Result`枚举一样，我们可以使用以下代码:

*   `unwrap`方法检索包含在`Some`中的值
*   `unwrap_or`方法收集`Some`中的数据，如果表达式为`None`，则返回默认值
*   `is_some`检查它是否不是`None`
*   `is_none`检查值是否为`None`

```
fn main() {

    let number = divide(103, 0);

    if number.is_some() {
        println!("number is: {}", number.unwrap());
    } else {
        println!("Is the result none: {}", number.is_none());
        println!("Result: {}", number.unwrap_or(0));
    }
}
```

## 结论

在本文中，我们看到了枚举和模式匹配是如何工作的，以及`match`语句如何比`switch`语句更高级。

最后，我们看到了如何使用枚举通过保存数据来改进模式匹配。

我希望这篇文章已经帮助您完全理解了模式匹配和枚举是如何工作的。如果你有任何不明白的地方，一定要在评论中告诉我。

感谢您的阅读，祝您愉快！

## [log rocket](https://lp.logrocket.com/blg/rust-signup):Rust 应用的 web 前端的全面可见性

调试 Rust 应用程序可能很困难，尤其是当用户遇到难以重现的问题时。如果您对监控和跟踪 Rust 应用程序的性能、自动显示错误、跟踪缓慢的网络请求和加载时间感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/rust-signup)

.

[![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/rust-signup)

LogRocket 就像是网络和移动应用程序的 DVR，记录你的 Rust 应用程序上发生的一切。您可以汇总并报告问题发生时应用程序的状态，而不是猜测问题发生的原因。LogRocket 还可以监控应用的性能，报告客户端 CPU 负载、客户端内存使用等指标。

现代化调试 Rust 应用的方式— [开始免费监控](https://lp.logrocket.com/blg/rust-signup)。