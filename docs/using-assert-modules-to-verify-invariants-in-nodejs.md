# 使用 Assert 模块验证 Node.js 中的不变量

> 原文：<https://blog.logrocket.com/using-assert-modules-to-verify-invariants-in-nodejs/>

[Node.js](https://nodejs.org/en/) 中的[断言模块](https://nodejs.org/api/assert.html)用于测试 Node.js 应用程序中功能的表达式。如果测试结果返回 false，assert 抛出一个错误并停止程序。您可以将 assert 模块与单元测试工具一起使用，如[单元 Js](https://unitjs.com/guide/assert-node-js.html) 、[摩卡](https://mochajs.org/)和[柴](https://blog.logrocket.com/unit-testing-node-js-applications-using-mocha-chai-and-sinon/)。

开发人员使用 assert 来测试应用程序中的不变量。但是什么是不变量，验证不变量的方法有哪些不同？

## 什么是不变量？

不变量是需要在程序中的某一点返回 true 的表达式或条件。让我们仔细看看下面的表达式:

```
let a = 5;
let b = 3;
let correct = (a > b);
console.log (correct);

```

当您在终端或 Bash 上运行它时，应该会返回“true”。如果由于某种原因，上面的应用程序返回 false，并且有一些其他表达式依赖于它，该怎么办？

如果我们将大于号`>`改为`<`并运行这段代码，我们应该得到“false”让我们假设上面符号的变化是一个错误，程序因为这个错误返回一个意外的结果。开发团队如何追踪这个错误并确保不再发生？

上面的表达式是不变量的一个例子。为了保证代码按预期返回“true ”, node . js 开发团队使用 assert 模块来测试表达式。

## 断言方法的类型

有不同的 assert 方法——您使用哪种方法取决于您在 Node.js 应用程序中测试不变量的目的。要使用此 Node.js 模块，请通过运行以下命令来安装它:

```
npm i assert

```

让我们深入研究不同的 assert 方法，并举例说明在我们的应用程序中可以在哪里使用它们。

### `Assert(value[, message])`法

此方法用于验证不变量的值是否为真。它是`assert.ok(value[, message])`的别名，可以以同样的方式使用。

```
//import assert module
var assert = require('assert');

//declare variables
var a = 5;
var b = 3;

//This code below would throw an assert error and end program
assert(a < b);

/**This is an expample to show how to use assert.ok(). Comment assert() out, run this code to get the same response.
assert.ok(a < b);
**/

```

上面的程序是一个如何在应用程序中使用`assert()`方法的例子。如果你仔细观察，你会发现这个表达式和第一个用来解释不变量的表达式是一样的。

注意:如果不变表达式返回 true，任何断言方法都不会抛出异常。只有当不变量返回 false 或 0 时，它们才会抛出错误。

### `Assert.deepStrictEqual (actual, expected[, message])`法

这个方法在严格模式下使用 assert，所以在检查相等性时将使用这个方法，而不是`assert.deepEqual`方法。这是因为`assert.deepEqual`使用了[抽象相等比较](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness)；因此，它可以产生令人惊讶的结果。

下面的表达式举例说明了如何使用`Assert.deepStrictEqual()`方法:

```
//This is how to import the assert module in strict mode
const assert = require('assert').strict;

// This fails because 1 !== '1 for strict equality comparison.
assert.deepStrictEqual({ a: 1 }, { a: '1' });

/**Where { a: 1 } = actual value and { a: '1' } = expected value.
AssertionError: Expected inputs to be strictly deep-equal:
**/

```

### `Assert.fail([message])`法

想象一下，当 assert 返回 false 时，您需要向不变测试添加一条定制消息。这是一种简单的调试方法。例如，开发人员会将以下内容嵌入到他们的代码中，以便他们知道错误是什么:

```
console.log('When you get here, log this so i know my error') 

```

您可以使用以下表达式插入自定义错误消息:

```
//Import assert module
const assert = require('assert').strict;

/** AssertionError [ERR_ASSERTION]: Failed. This just prints failed as error message. This is not a good practice**/
assert.fail();

// AssertionError [ERR_ASSERTION]: This is why I failed
assert.fail('This is why I failed');

// TypeError: This is why I failed
assert.fail(new TypeError('This is why I failed'));

```

### `Assert.ifError(value)`法

想象一下:你刚刚得到一个错误，你可能知道错误来自哪一行，但你不仅仅是理解程序为什么抛出一个错误。

使用`Assert.ifError`方法，您可以测试错误回调参数，以了解错误到底来自哪里。下面的表达式给出了使用这种方法的一个快速示例:

```
//This method is used in strict mode
const assert = require('assert').strict;

//Define error callback arguement
let err;
(function error1() {
  err = new Error('test error');
})();

//create an ifError function
(function ifError1() {
//return this error if program resolves into an error
  assert.ifError(err);
})();
/**AssertionError [ERR_ASSERTION]: ifError got unwanted exception: test error
at ifError1
at error1 **/

```

### `Assert.doesNotMatch(string, regexp[, message])`法

此方法用于验证实际的输入字符串与预期的表达式不匹配。node . js v 13 . 6 . 0，v12.16.0 中增加的`assert.doesNotMatch()`方法是实验性的，可能会被完全移除。此方法当前在严格模式下使用 assert。

下面的简单表达式展示了如何使用`assert.doesNotMatch()`方法来验证不变量。

```
//import assert using strict mode
const assert = require('assert').strict;

assert.doesNotMatch('I will fail', /fail/);
/** AssertionError [ERR_ASSERTION]: The input was expected to not match because fail is contained in the actual and expected string ...**/

assert.doesNotMatch(123, /pass/);
/**AssertionError [ERR_ASSERTION]: The "string" argument must be of type string.**/

assert.doesNotMatch('I will pass', /different/);
/** This passes the test with no error since actual and expeted string have no word alike **/

```

### `Assert.doesNotReject(asyncFn\[, error\][, message])`法

该方法用于检查异步函数中的承诺是否被拒绝。如果表达式没有返回一个承诺，`Assert.doesNotReject()`返回一个被拒绝的承诺。

使用这种方法不是很有用，因为捕捉拒绝只是为了再次拒绝它是没有好处的。最好的办法是在不应该拒绝的特定代码路径旁边添加注释，并使错误消息尽可能有表现力。

下面的表达式显示了如何使用此方法的示例:

```
(async () => {
  await assert.doesNotReject(
    async () => {
      throw new TypeError('Wrong value');
    },
    SyntaxError
  );
})();

assert.doesNotReject(Promise.reject(new TypeError('Wrong value')))
  .then(() => {
    // ...
  });

```

如果您想检查不变量中的 promise 是否被拒绝，该怎么办？

在这个场景中使用的断言方法是`Assert.rejects()`方法。该方法在异步函数中等待返回的承诺，以验证承诺被拒绝。

如果承诺没有被拒绝，它将抛出一个错误。下面的表达式显示了我们可以使用这种方法的简单方式。

```
(async () => {
  await assert.rejects(
    async () => {
      throw new TypeError('Wrong value');
    },
    {
      name: 'TypeError',
      message: 'Wrong value'
    }
  );
})();

assert.rejects(
  Promise.reject(new Error('Wrong value')),
  Error
).then(() => {
  // ...
});

```

## 在严格模式和传统模式下断言

严格模式下的断言包括使用[严格相等比较](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness)方法来验证不变量。在断言严格模式下，非严格方法的行为与其相对严格的方法相同。

例如，`assert.deepEqual()`会表现得像`assert.deepStrictEqual()`，而`assert.notEqual()`会表现得像`assert.notStrictEqual()`。

要在严格模式下使用 assert，请使用以下方法之一将 assert 模块导入到您的应用程序中:

```
//import the assert module using strict mode
const assert = require('assert').strict;

//import the assert module using strict mode
const assert = require('assert/strict');

```

在遗留模式中，不变量的比较涉及到使用[抽象相等比较](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness)。建议总是使用严格模式来比较不变量的实际值和期望值。

要使用传统模式将 assert 模块导入到您的应用程序中，请使用下面的表达式:

```
const assert = require('assert');

```

下面是遗留模式下不同的断言比较方法。

### `Assert.deepEqual(actual, expected[, message])`法

断言深度相等方法使用抽象相等比较。它用于检查实际结果和预期结果是否相等。使用这种方法，还会测试子属性。下面的表达式显示了如何使用`assert.deepEqual()`方法的快速示例:

```
const assert = require('assert');

const val1 = {
  x: {
    y: 1
  }
};
const val2 = {
  x: {
    y: 2
  }
};
const val3 = {
  x: {
    y: 1
  }
};

assert.deepEqual(val1, val3);
// Returns OK since val1 is equal to val3

// Values of b are different (Child properties)
assert.deepEqual(val1, val2);
// AssertionError: { a: { b: 1 } } deepEqual { a: { b: 2 } }

```

如果为了平等，你想测试深度不平等呢？在这种情况下使用的断言方法是`assert.notDeepEqual(actual, expected[, message])`。此方法用于测试深度不相等，如果实际值和期望值相等，将引发异常。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

如何使用此方法的示例如下所示:

```
const assert = require('assert');

const val1 = {
  x: {
    y: 1
  }
};
const val2 = {
  x: {
    y: 2
  }
};
const val3 = {
  x: {
    y: 1
  }
};

assert.notDeepEqual(val1, val3);
// AssertionError: { x: { y: 1 } } notDeepEqual { x: { y: 2 } }

/** Values of b are different (Child properties)**/
assert.notDeepEqual(val1, val2);
// Ok because val1 and val2 are not equal

```

注意:这种方法因为不稳定而被贬低，所以建议在严格模式下使用 assert，即`Asset.deepStrictEqual`和`Asset.notDeepStrictEqual`方法。

### `Assert.equal(actual, expected[, message])`法

该方法用于使用[抽象相等比较](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness)测试实际和预期结果之间的浅层比较。它很少用于测试子值，因为它是用于浅层测试的，并且建议使用`Assert.deepStrictEqual()`方法来代替这个方法。如何使用`Assert.equal()`的示例如下式所示:

```
//Import the assert module
const assert = require('assert');

assert.equal(1, 1);
// This throws no exception since 1 == 1

assert.equal(1, '1');
/** This throws no exception since with abstract equality comparison 1 == '1' **/

assert.equal(NaN, NaN);
// This throws no exception

assert.equal(1, 2);
// This throws an error, AssertionError: 1 == 2

assert.equal({ a: { x: 1 } }, { a: { x: 1 } });
/**This throws an error AssertionError: { a: { x: 1 } } == { a: { x: 1 } } because it dosen't check child value**/

```

`assert.notEqual()`方法用于检查浅不等式，就像`assert.equal()`用于检查等式一样。

### `Assert.notEqual(actual, expected[, message])`法

就像 assert.equal 方法用于浅相等比较一样，该方法用于浅不相等比较。

您不应该使用此方法来测试子值，因为它是用于浅层测试的。

此外，这是遗留模式不变方法，遗留模式不变方法用于抽象比较。为了进行严格的比较，也为了测试子值，使用`assert.notStrictEqual`方法。

```
//Import the assert module
const assert = require('assert');

assert.notEqual(1, 1);
// This throws an exception since 1 == 1

assert.notEqual(1, '1');
/** This throws an exception since with abstract equality comparison 1 == '1' **/

assert.notEqual(1, 2);
// This throws no error, since 1 !== 2

```

### `Assert.notDeepEqual(actual, expected[, message])`法

assert not deep equality 方法用于检查实际结果和预期结果之间的浅不相等。如果您想测试子属性之间的比较，这个方法非常适合您。

下面的表达式显示了如何使用`assert.notDeepEqual()`方法的快速示例:

```
//Import module
const assert = require('assert');

const val1 = {
  x: {
    y: 1
  }
};
const val2 = {
  x: {
    y: 2
  }
};
const val3 = {
  x: {
    y: 1
  }
};

assert.notDeepEqual(val1, val3);
// Returns an error, since val1 is equal to val3

// Values of b are different (Child properties)
assert.deepEqual(val1, val2);
// Returns no error since the child properties are not equal

```

## 结论

在本文中，我们已经看到了如何以及为什么我们可能需要使用节点。应用程序中的 Js 断言模块。

建议在编写生产代码时主要使用严格模式下的断言，因为它们的遗留对应物可能会产生意外的结果，因为它们使用抽象的相等比较，并且因为它们是当前折旧的。

您可能对这个[文档](https://nodejs.org/api/assert.html#assert_class_assert_calltracker)感兴趣，它解释了如何创建一个`call tracker object`，它将验证不变表达式是否被调用了所需的次数。

非常感谢您阅读这篇文章。

## 200 只![](img/61167b9d027ca73ed5aaf59a9ec31267.png)显示器出现故障，生产中网络请求缓慢

部署基于节点的 web 应用程序或网站是容易的部分。确保您的节点实例继续为您的应用程序提供资源是事情变得更加困难的地方。如果您对确保对后端或第三方服务的请求成功感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/node-signup)

.

[![LogRocket Network Request Monitoring](img/cae72fd2a54c5f02a6398c4867894844.png)](https://lp.logrocket.com/blg/node-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/node-signup)

LogRocket 就像是网络和移动应用程序的 DVR，记录下用户与你的应用程序交互时发生的一切。您可以汇总并报告有问题的网络请求，以快速了解根本原因，而不是猜测问题发生的原因。

LogRocket 检测您的应用程序以记录基线性能计时，如页面加载时间、到达第一个字节的时间、慢速网络请求，还记录 Redux、NgRx 和 Vuex 操作/状态。

[Start monitoring for free](https://lp.logrocket.com/blg/node-signup)

.