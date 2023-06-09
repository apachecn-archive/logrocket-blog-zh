# SQLBoiler 简介:ORMs 的 Go 框架

> 原文：<https://blog.logrocket.com/introduction-sqlboiler-go-framework-orms/>

对象关系映射(ORM)是一种编程技术，允许您在两种不兼容的类型系统之间转换数据。

构建软件时，通常会有一个数据库层和一个保存业务逻辑的应用层。通常，数据存储在数据库中的方式与您选择的编程语言不兼容，这意味着您必须在数据库和应用层之间操作数据。

数据库 ORM 通过提取样板文件使这个过程变得更容易，让您可以用编写业务逻辑的同一种语言与数据进行交互。在本文中，我们将探索一个用于生成表单的工具 [SQLBoiler](https://github.com/volatiletech/sqlboiler) 。

## 为什么应该使用 SQLBoiler？

大多数编程语言提供了各种各样的库，这些库提供了 ORM 的特性。围棋也不例外。虽然 SQLBoiler 没有像它的一些替代品一样被广泛采用，如 [Ent](https://entgo.io/) ，，但它已经被积极开发了五年多，并为我们如何推理数据库交互带来了一个全新的维度。

传统 ORM 的一个更明显的缺点是当涉及到你的模型的类型安全时的[权衡。由于 Go 中缺乏泛型，这些库依赖于使用反射来处理模式更改，这可能会严重损害应用程序的性能。但是，使用 SQLBoiler，您可以通过从数据库模式生成的代码获得完全类型安全的模型。](https://blog.logrocket.com/why-you-should-avoid-orms-with-examples-in-node-js-e0baab73fa5/)

### 数据库优先与代码优先的方法

SQLBoiler 采用数据库优先的方法，这意味着您管理自己的数据库模式，并且模型是从定义的模式中生成的。因为您的模型与您在模式中定义的类型精确匹配，所以这种方法为您提供了可预测性的优势。

代码优先的方法是相反的，意味着您定义您的模型或实体，并允许 ORM 相应地创建您的数据库和表。这样做的一个好处是能够从代码中修改数据库。

### SQLBoiler 功能

开箱即用的 SQLBoiler 包括以下特性:

*   完整的模型生成
*   急切装载
*   原始 SQL 回退
*   处理
*   模型挂钩
*   多模式支持
*   处理复杂的表关系

## SQLBoiler 入门

为了演示 SQLBoiler 的一些特性，我们将为一个大学数据库管理系统设计一个简单的模式并生成模型。

### 要求:

通过在现有的 Go 模块项目中安装 SQLBoiler 包，可以快速入门。您将需要以下内容:

*   Go v≥ 1.13
*   数据库:在本文中，我们将使用 Postgres

创建 Go 模块项目:

```
$ mdkir <project-name>
$ cd <project-name>
$ go mod init <your-github-url>

```

如果您从未使用过 SQLBoiler，请下载 code-gen 二进制文件:

```
$ go install github.com/volatiletech/sqlboiler/[email protected]
$ go install github.com/volatiletech/sqlboiler/v4/drivers/[email protected]

```

最后，安装 SQLBoiler:

```
$ go get github.com/volatiletech/sqlboiler/v4

```

## 数据库配置

在配置文件中，我们将指定数据库连接选项和其他 code-gen 标志。为了快速开始，您可以在项目的根目录下创建一个`sqlboiler.toml`文件，[从 GitHub](https://github.com/volatiletech/sqlboiler#full-example) 粘贴这个示例配置，并更新必要的字段。

### 定义模式

首先，我们在`schema.sql`文件中定义一个数据库模式:

```
// schema.sql
drop table if exists students;
drop table if exists departments;
drop table if exists staffs;
drop table if exists classes;

create table students (
   id serial not null primary key,
   firstname varchar not null,
   lastname varchar not null,
   email varchar not null,
   admission_number varchar not null,
   year int not null,
   cgpa float not null
);

create table classes (
   id serial not null primary key,
   title varchar not null,
   code varchar not null,
   unit int not null,
   semester int not null,
   location varchar not null
);

create table departments (
   id serial not null primary key,
   name varchar not null,
   code varchar not null,
   telephone varchar not null,

   foreign key (user_id) references users (id)
);

create table staffs (
   id serial not null primary key,
   firstname varchar not null,
   lastname varchar not null,
   email varchar not null,
   telephone varchar not null,
   salary bigint not null,
);

create table classes_students (
   class_id int not null,
   student_id int not null,

   primary key (class_id, student_id),
   foreign key (student_id) references students (id),
   foreign key (class_id) references classes (id)
);

create table classes_instructors (
   class_id int not null,
   staff_id int not null,

   primary key (class_id, staff_id),
   foreign key (staff_id) references staffs (id),
   foreign key (class_id) references classes (id)
);

insert into users (name) values ('Franklin');
insert into users (name) values ('Theressa');

```

SQLBoiler 没有提供现成的迁移工具，但是社区中有很多选项。 [sql-migrate](https://github.com/rubenv/sql-migrate) 是推荐使用的工具，但是，在这种情况下，我们将直接将模式文件加载到数据库中，如下所示:

```
$ psql --username <user> --password <password> < schema.sql

```

## 生成模型

接下来，我们将使用 SQLBoiler CLI 从 define 模式生成我们的模型。这个步骤的一个有趣的部分是 CLI 也为您的模型生成测试。您可以运行这些测试来确保您的模型符合定义的模式。您还可以使用`--no-tests`标志跳过测试，以减少您的应用程序二进制代码。

查看 CLI 支持的[标志列表。您可以在您的`sqlboiler.toml`文件中定义标志，或者将它们作为参数传递给 CLI 命令。要生成您的模型，请运行以下命令:](https://github.com/volatiletech/sqlboiler#initial-generation)

```
$ sqlboiler psql -c sqlboiler.toml --wipe --no-tests

```

上面的命令将创建一个包含所有数据库模型的`models`目录。就这样，您拥有了一个完整的、类型安全的 ORM 来与数据库进行交互。如果您排除了`--no-tests`标志，您可以运行`go test ./models`来运行生成的测试。

## SQLBoiler 查询模型系统

SQLBoiler 生成 starter 方法，这是您开始查询任何模型的入口点。一个示例 starter 方法类似于`models.Students()`，其中`Students`表示学生模型。

查询模块允许您指定想要进行的查询的类型，例如，`qm.Where("age=?", 2)`翻译成一个`where`子句。

SQLBoiler 为您可能需要的每个 SQL 子句生成这些方法。在自动补全的帮助下，你可以在输入`qm`时看到所有可能的子句。

结束符作为端点，附加到查询的末尾供您执行。例如，假设您想从大学管理数据库中获取所有学生。限制器将是`.All(ctx, db)`。其他完成器包括`.One(ctx, db)`、`.Count(ctx, db)`和`.Exists(ctx, db)`。

您将通过启动器、查询模块和结束器的组合在 SQLBoiler 中构建您的查询。让我们看看使用查询模式系统的完整示例:

```
// initialize a db connection
db, err := sql.Open("postgres", `dbname=<dbname> host=localhost user=<user> password=<password>`)
if err != nil {} // handle err

// Fetch all students
students, err := models.Students().All(ctx, db)
if err != nil {} // handle err

// Fetch single student
student, err := models.Students(qm.Where("id=?", 1).One(ctx, db)
if err != nil {} // handle err

// Count all students in database
count, err := models.Students().Count(ctx, db)

```

SQLBoiler 不会强迫您使用某些约定。如果您想进行非常具体的 SQL 查询，您可以轻松地创建一个原始查询，如下所示:

```
var department models.Department
err := db.Raw("select * from departments where population between 1500 and 3200").Bind(ctx, db, &department)
if err != nil {} // handle err

```

当创建原始查询时，您需要将[绑定到一个结构](https://github.com/volatiletech/sqlboiler#binding)，这个结构可以是由 SQLBoiler 生成的，也可以是您自定义的。

## 关系

在 SQLBoiler 中处理表之间的关系轻而易举，它可以为您通过外键在模式中定义的任何类型的关系生成帮助器方法，如`1-1`、`1-n`或`m-n`。

ORM 的一个常见性能瓶颈是在查询包含连接的表时出现的`n+1`查询问题。

假设我们想在数据库中查询某个系的学生名单。我们运行一个查询来获取所有的`students`，但是现在您还想包含每个学生参加的所有`classes`。您循环遍历您的`students`结果并获取所有的`classes`，这意味着对于每个学生，您都要对数据库进行额外的查询以获取他们的班级。

如果我们有`N`个学生，我们将进行`N`个额外的查询，这是不必要的，因为我们可以在初始查询中获取所有的`classes`和每个`students`。SQLBoiler 通过急切加载为这个问题提供了一个优雅的解决方案，这大大减少了对数据库的查询次数。

如果您查看我们上面定义的模式，您会注意到`departments`表包含一个引用`users`表的外键`user_id`。这是一个系有很多学生的`1-n`关系。

我们还有一个名为`classes-students`的连接表，保存引用`classes`和`students`表的外键。这是一个`class`可以有多个`students`和一个`student`可以属于多个`classes`的`m-n`关系。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

让我们看看如何通过急切加载来查询关系的示例:

```
//...
// fetch department including students
department, err := models.Departments(qm.Where("id=?", 1), qm.Load(models.DepartmentRels.Student)).One(ctx, db)
if err != nil {} // handle err

```

我们甚至可以结合查询模式来过滤急切加载的数据:

```
//...
// fetch classes including students with cgpa >= 2.6
classes, err := models.Classes(qm.Load(models.ClassRels.Student, qm.Where("cgpa >= ?", 2.6))).All(ctx, db)
if err != nil {} // handle err

```

对于每个班级，上面的查询将只返回`cgpa`大于或等于`2.6`的学生。

## CRUD 操作

我们已经看到了如何使用 Query Mod 系统执行查询。现在，让我们探索其他 CRUD 操作，比如创建、更新和删除实体。

### 创建实体

要创建一个实体，我们只需声明模型的一个实例，设置必需的字段，并调用`Insert`。对于`Insert`的第三个参数，我们将指定包含哪些列。`Infer`告诉 SQLBoiler 自动处理这个问题，但是如果您想要对列进行更细粒度的控制，其他选项包括`Whitelist`、`Blacklist`和`Greylist`:

```
//...
// create a department
var department models.Department
department.Name = "Computer Science"
department.Code = "CSC"
department.Telephone = "+1483006541"
err := department.Insert(ctx, db, boil.Infer())
if err != nil {} // handle err

```

### 更新实体

就像插入一个实体一样，执行更新也很直观。我们获取实体，将字段设置为新值，并调用`Update`:

```
//...
// update a student
student, err := models.FindStudent(ctx, db, 1)
if err != nil {} // handle err

student.year = 4
student.cgpa = 4.0

_, err := student.Update(ctx, db, boil.Infer())
if err != nil {} // handle err

```

### 删除实体

删除实体非常类似。从数据库中获取实体并调用`Delete`方法:

```
//...
// delete a student
student, err := models.FindStudent(ctx, db, 1)
if err != nil {} // handle err

_, err := student.Delete(ctx, db)
if err != nil {} // handle err

// delete multiple classes
classes, err := models.Classes(qm.Where("unit < ?", 3)).All(ctx, db)
if err != nil {} // handle err

_, err := classes.DeleteAll(ctx, db)
if err != nil {} // handle err

```

### 处理

事务让我们将多个 SQL 语句组合成一个原子操作，确保所有语句成功运行，或者如果一个或多个语句失败，将数据库恢复到事务开始时的状态。

假设我们正在创建一个新部门。创建一个或多个属于该部门的类也是有意义的。然而，如果其中一个操作失败，我们不希望数据库中有一个不指向任何部门的悬空类行。在这里，事务是有用的:

```
//...
// start a transaction
tx, err := db.BeginTx(ctx, nil)
if err != nil {} // handle err

// create a department
var department models.Department
department.Name = "Computer Science"
department.Code = "CSC"
department.Telephone = "+1483006541"
err = department.Insert(ctx, tx, boil.Infer())
if err != nil {
  // rollback transaction
  tx.Rollback()
}

// create a class
var class models.Class
class.Title = "Database Systems"
class.Code = "CSC 215"
class.Unit = 3
class.Semester = "FIRST"
err = class.Insert(ctx, tx, boil.Infer())
if err != nil {
  // rollback transaction
  tx.Rollback()
}

// add class to department
class, err := models.Classes(qm.Where("code=?", "CSC 215")).One(ctx, tx)
department, err := models.Departments(qm.Where("code=?", "CSC")).One(ctx, tx)
err = department.AddClasses(ctx, tx, class)
if err != nil {
  // rollback transaction
  tx.Rollback()
}

// commit transaction
tx.Commit()

```

首先，我们通过调用`BeginTx`启动一个事务，它返回`tx`，这是一个将在整个事务生命周期中使用的数据库句柄。我们创建一个部门和一个类，然后将该类添加到部门实体中。

如果出现错误，我们调用`Rollback`方法将数据库的状态恢复到事务开始时的状态。如果一切成功，我们只需调用`Commit`方法来保存更改。

## 结论

在本文中，我们学习了如何使用 SQLBoiler，并利用其代码生成特性，使用完全类型安全的模型和助手方法与数据库进行无缝交互。

如果您有一个想要在其上构建项目的现有数据库，那么 SQLBoiler 绝对是一个不错的选择。当然，对于您独特的用例，SQLBoiler 可能并不总是最佳选择。您可能会发现自己处于这样一种情况，您不知道您的数据库模式将会变成什么样，您只想从几个数据点开始。

在这种情况下，代码优先的 ORM 可能是理想的。此外，缺乏内置的迁移工具可能是您开发经验的一个缺点，这意味着像 [Ent](https://entgo.io/) 这样的其他 ORM 可能是更好的选择。像软件开发中的任何事情一样，为工作使用正确的工具会给你带来最好的结果。

我希望你喜欢这篇文章，如果你有任何问题，请留下评论。

## 使用 [LogRocket](https://lp.logrocket.com/blg/signup) 消除传统错误报告的干扰

[![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/signup)

[LogRocket](https://lp.logrocket.com/blg/signup) 是一个数字体验分析解决方案，它可以保护您免受数百个假阳性错误警报的影响，只针对几个真正重要的项目。LogRocket 会告诉您应用程序中实际影响用户的最具影响力的 bug 和 UX 问题。

然后，使用具有深层技术遥测的会话重放来确切地查看用户看到了什么以及是什么导致了问题，就像你在他们身后看一样。

LogRocket 自动聚合客户端错误、JS 异常、前端性能指标和用户交互。然后 LogRocket 使用机器学习来告诉你哪些问题正在影响大多数用户，并提供你需要修复它的上下文。

关注重要的 bug—[今天就试试 LogRocket】。](https://lp.logrocket.com/blg/signup-issue-free)