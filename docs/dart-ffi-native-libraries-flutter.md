# 使用 Dart FFI 访问 Flutter 中的本地库

> 原文：<https://blog.logrocket.com/dart-ffi-native-libraries-flutter/>

Dart 是一种功能丰富的语言，有很好的文档记录，易于学习；然而，当涉及到 Flutter 应用程序开发时，它可能缺少一些功能。例如，可能需要一个应用程序链接到外部二进制库，或者用 C、C+或 Rust 等低级语言编写一些代码可能会有好处。

幸运的是，Flutter 应用程序能够通过`dart:ffi library`使用外部函数接口(FFI)。FFI 使用一种语言编写的程序能够调用用其他语言编写的库。比如有了 FFI，一个 Flutter app 可以直接从 Dart 调用基于 C 的编译库，比如`cJSON.dylib`，或者调用 C 源代码，比如`lib/utils.c`。

在 Dart 中拥有 FFI 互操作机制的一个核心好处是，它使我们能够用编译成 C 库的任何语言编写代码。一些例子是 Go 和 Rust。

FFI 还使我们能够使用相同的代码在不同的平台上提供相同的功能。例如，假设我们希望在所有媒体中利用一个特定的开源库，而无需投入时间和精力在每个应用程序的开发语言(Swift、Kotlin 等)中编写相同的逻辑。).一种解决方案是用 C 或 Rust 实现代码，然后用 FFI 向 Flutter 应用程序公开。

Dart FFI 开辟了新的开发机会，特别是对于需要在团队和项目之间共享本机代码或提高应用程序性能的项目。

在本文中，我们将研究如何在 Flutter 中使用 Dart FFI 访问本地库。

首先，让我们从基础和基础开始。

## 使用 Dart FFI 访问动态库

让我们从用 c 编写一个基本的数学函数开始。我们将在一个简单的 Dart 应用程序中使用它:

```
/// native/add.c

int add(int a, int b)
{
return a + b;
}

```

本地库可以静态或动态链接到应用程序中。静态链接库嵌入到应用程序的可执行映像中。它在应用程序启动时加载。相比之下，动态链接库分布在应用程序中的一个单独的文件或文件夹中。它按需加载。

我们可以通过运行以下代码将我们的`C`文件转换到动态库`dylib`:

```
gcc -dynamiclib add.c -o libadd.dylib
```

这将产生以下输出:`add.dylib`。

我们将按照三个步骤在 Dart 中调用这个函数:

1.  打开包含该函数的动态库
2.  查函数(***n . b .****由于 C 和 Dart 中类型不同，我们必须分别指定各自的*
3.  调用函数

```
/// run.dart
import 'dart:developer' as dev;
import 'package:path/path.dart';
import 'dart:ffi';void main() {
final path = absolute('native/libadd.dylib');
dev.log('path to lib $path');
final dylib = DynamicLibrary.open(path);
final add = dylib.lookupFunction('add');
dev.log('calling native function');
final result = add(40, 2);
dev.log('result is $result'); // 42
}
```

这个例子说明了我们可以使用 FFI 在 Dart 应用程序中轻松使用任何动态库。

现在，是时候引入一个工具，它可以通过代码生成来帮助生成 FFI 绑定。

## 用 FFIGEN 在 Dart 中生成 FFI 绑定

有时候，为 Dart FFI 编写绑定代码可能会非常耗时或乏味。在这种情况下，[外部函数接口生成器](https://pub.dev/packages/ffigen) ( `ffigen`)会很有帮助。`ffigen`是 FFI 的绑定生成器。它帮助解析`C`头并自动生成`dart`代码。

让我们使用这个包含基本数学函数的示例`C`头文件:

```
/// native/math.h

/** Adds 2 integers. */
int sum(int a, int b);
/** Subtracts 2 integers. */
int subtract(int *a, int b);
/** Multiplies 2 integers, returns pointer to an integer,. */
int *multiply(int a, int b);
/** Divides 2 integers, returns pointer to a float. */
float *divide(int a, int b);
/** Divides 2 floats, returns a pointer to double. */
double *dividePercision(float *a, float *b);

```

为了在 Dart 中生成 FFI 绑定，我们将在`pubspec.yml`文件中添加`ffigen`到`dev_dependencies`:

```
/// pubspec.yaml 
dev_dependencies:
ffigen: ^4.1.2

```

`ffigen`要求将配置作为单独的`config.yaml`文件添加，或者添加到`pubspec.yaml`中的`ffigen`下，如下图所示:

```
/// pubspec.yaml
....

ffigen:
name: 'MathUtilsFFI'
description: 'Written for the FFI article'
output: 'lib/ffi/generated_bindings.dart'
headers:
entry-points:
- 'native/headers/math.h'

```

需要生成的`entry-points`和`output`文件为必填字段；然而，我们也可以定义并包含一个`name`和`description`。

接下来，我们将运行下面的代码:
`dart run ffigen`

这导致以下输出:`generated_bindings.dart`

现在，我们可以在 Dart 文件中使用`MathUtilsFFI`类。

## 在演示中使用 FFIGEN

既然我们已经讲述了`ffigen`的基础知识，让我们来看一个演示:

### 生成动态库

对于这个演示，我们将使用 [cJSON](https://github.com/DaveGamble/cJSON) ，这是一个超轻量 JSON 解析器，可以在`Flutter`或`Dart`应用程序中使用。

整个 cJSON 库由一个 C 文件和一个头文件组成，所以我们可以简单地将`cJSON.c`和`cJSON.h`复制到我们项目的源代码中。然而，我们还需要使用 CMake 构建系统。对于树外构建，建议使用 CMake，这意味着构建目录(包含编译后的文件)与源目录(包含源文件)是分开的。在撰写本文时，支持 CMake 2 . 8 . 5 版或更高版本。

要在 Unix 平台上用 CMake 构建 cJSON，我们首先创建一个`build`目录，然后在该目录中运行 CMake:

```
cd native/cJSON // where I have copied the source files
mkdir build 
cd build
cmake ..

```

以下是输出结果:

```
-- The C compiler identification is AppleClang 13.0.0.13000029
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Performing Test FLAG_SUPPORTED_fvisibilityhidden
-- Performing Test FLAG_SUPPORTED_fvisibilityhidden - Success
-- Configuring done
-- Generating done
-- Build files have been written to: ./my_app_sample/native/cJSON/build

```

这将创建一个 Makefile，以及其他几个文件。

我们使用这个命令来编译:

```
make

```

构建进度条将一直前进，直到完成:

```
[ 88%] Built target readme_examples
[ 91%] Building C object tests/CMakeFiles/minify_tests.dir/minify_tests.c.o
[ 93%] Linking C executable minify_tests
[ 93%] Built target minify_tests
[ 95%] Building C object fuzzing/CMakeFiles/fuzz_main.dir/fuzz_main.c.o
[ 97%] Building C object fuzzing/CMakeFiles/fuzz_main.dir/cjson_read_fuzzer.c.o
[100%] Linking C executable fuzz_main
[100%] Built target fuzz_main

```

基于平台生成动态库。比如 Mac 用户会看到`libcjson.dylib`，而 Windows 用户可能会看到`cjson.dll`，Linux 用户可能会看到`libcjson.so`。

### 生成 Dart FFI 绑定文件

接下来，我们需要生成 Dart FFI 绑定文件。为了演示如何使用分离配置，我们将创建一个新的配置文件`cJSON.config.yaml`，并配置 cJSON 库:

```
// cJSON.config.yaml

output: 'lib/ffi/cjson_generated_bindings.dart'
name: 'CJson'
description: 'Holds bindings to cJSON.'
headers:
entry-points:
- 'native/cJSON/cJSON.h'
include-directives:
- '**cJSON.h'
comments: false
typedef-map:
'size_t': 'IntPtr'

```

生成 FFI 绑定。我们必须跑起来:

```
> flutter pub run ffigen --config cJSON.config.yaml
Changing current working directory to: /**/my_app_sample
Running in Directory: '/**/my_app_sample'
Input Headers: [native/cJSON/cJSON.h]
Finished, Bindings generated in /**/my_app_sample/lib/ffi/cjson_generated_bindings.dart

```

为了使用这个库，我们创建一个 JSON 文件:

```
/// example.json

{
"name": "Majid Hajian",
"age": 30,
"nicknames": [
{
"name": "Mr. Majid",
"length": 9
},
{
"name": "Mr. Dart",
"length": 8
}
]
}

```

这个示例 JSON 文件很简单，但是想象一下同样的过程有大量的 JSON，需要高性能的解析。

### 正在加载库

首先，我们必须确保正确加载动态库:

```
/// cJSON.dart
import 'dart:convert';
import 'dart:ffi';
import 'dart:io';
import 'package:ffi/ffi.dart';
import 'package:path/path.dart' as p;
import './lib/ffi/cjson_generated_bindings.dart' as cj;

String _getPath() {
final cjsonExamplePath = Directory.current.absolute.path;
var path = p.join(cjsonExamplePath, 'native/cJSON/build/');
if (Platform.isMacOS) {
path = p.join(path, 'libcjson.dylib');
} else if (Platform.isWindows) {
path = p.join(path, 'Debug', 'cjson.dll');
} else {
path = p.join(path, 'libcjson.so');
}
return path;
}

```

接下来，我们打开动态库:

```
final cjson = cj.CJson(DynamicLibrary.open(_getPath())); 
```

现在，我们可以使用生成的 cJSON 绑定:

```
/// cJSON.dart

void main() {
final pathToJson = p.absolute('example.json');
final jsonString = File(pathToJson).readAsStringSync();
final cjsonParsedJson = cjson.cJSON_Parse(jsonString.toNativeUtf8().cast());
if (cjsonParsedJson == nullptr) {
print('Error parsing cjson.');
exit(1);
}
// The json is now stored in some C data structure which we need
// to iterate and convert to a dart object (map/list).
// Converting cjson object to a dart object.
final dynamic dartJson = convertCJsonToDartObj(cjsonParsedJson.cast());
// Delete the cjsonParsedJson object.
cjson.cJSON_Delete(cjsonParsedJson);
// Check if the converted json is correct
// by comparing the result with json converted by `dart:convert`.
if (dartJson.toString() == json.decode(jsonString).toString()) {
print('Parsed Json: $dartJson');
print('Json converted successfully');
} else {
print("Converted json doesn't match\n");
print('Actual:\n' + dartJson.toString() + '\n');
print('Expected:\n' + json.decode(jsonString).toString());
}
}

```

接下来，我们可以使用助手函数将 cJSON 解析(或转换)为 Dart 对象:

```
/// main.dart
dynamic convertCJsonToDartObj(Pointer<cj.cJSON> parsedcjson) {
dynamic obj;
if (cjson.cJSON_IsObject(parsedcjson.cast()) == 1) {
obj = <String, dynamic>{};
Pointer<cj.cJSON>? ptr;
ptr = parsedcjson.ref.child;
while (ptr != nullptr) {
final dynamic o = convertCJsonToDartObj(ptr!);
_addToObj(obj, o, ptr.ref.string.cast());
ptr = ptr.ref.next;
}
} else if (cjson.cJSON_IsArray(parsedcjson.cast()) == 1) {
obj = <dynamic>[];
Pointer<cj.cJSON>? ptr;
ptr = parsedcjson.ref.child;
while (ptr != nullptr) {
final dynamic o = convertCJsonToDartObj(ptr!);
_addToObj(obj, o);
ptr = ptr.ref.next;
}
} else if (cjson.cJSON_IsString(parsedcjson.cast()) == 1) {
obj = parsedcjson.ref.valuestring.cast<Utf8>().toDartString();
} else if (cjson.cJSON_IsNumber(parsedcjson.cast()) == 1) {
obj = parsedcjson.ref.valueint == parsedcjson.ref.valuedouble
? parsedcjson.ref.valueint
: parsedcjson.ref.valuedouble;
}
return obj;
}
void _addToObj(dynamic obj, dynamic o, [Pointer<Utf8>? name]) {
if (obj is Map<String, dynamic>) {
obj[name!.toDartString()] = o;
} else if (obj is List<dynamic>) {
obj.add(o);
}
}

```

### 使用 FFI 将字符串从 C 传递到 Dart

[`[ffi]`包](https://pub.dev/packages/ffi)可以用来将字符串从 C 传递到 Dart。我们将这个包添加到我们的依赖项中:

```
/// pubspec.yaml

dependencies:
ffi: ^1.1.2

```

### 测试呼叫

现在，让我们来看看我们的演示是否成功！

我们可以在这个例子中看到，`name`、`age`和`nicknames`的 C 字符串被成功解析成 Dart:

```
> dart cJSON.dart

Parsed Json: {name: Majid Hajian, age: 30, nicknames: [{name: Mr. Majid, length: 9}, {name: Mr. Dart, length: 8}]}
Json converted successfully

```

现在我们已经回顾了 FFI 的基本要素，让我们看看如何在 Flutter 中使用它们。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

## 使用 FFI 向 Flutter 应用程序添加动态库

Dart FFI 的大部分概念也适用于颤振。为了简化本教程，我们将重点关注 Android 和 iOS，但这些方法也适用于其他应用程序。

要使用 FFI 向 Flutter 应用程序添加动态库，我们将遵循以下步骤:

### 配置 Android Studio C 编译器

要配置 Android Studio C 编译器，我们将遵循三个步骤:

1.  转到:`android/app`

2.  创建一个`CMakeLists.txt`

    ```
    file:cmake*minimum*required(VERSION 3.4.1)add_library( cJSON SHARED ../../DART/native/cJSON/cJSON.c // path to your native code )
    ```

3.  打开`android/app/build.gradle`并添加以下代码片段:

    ```
    android { ....externalNativeBuild { cmake { path "CMakeLists.txt" } }... }
    ```

这段代码告诉 Android 构建系统在构建应用程序时用`CMakeLists.txt`调用`CMake`。它会在 Android 上将`.c`源文件编译成一个带有`.so`后缀的共享对象库。

### 配置 Xcode C 编译器

为了确保 Xcode 将使用本机 C 代码构建我们的应用程序，我们将遵循以下 10 个步骤:

1.  通过运行以下命令打开 Xcode 工作区:

```
open< ios/Runner.xcworkspace
```

1.  从顶部导航栏的**目标**下拉列表中，选择**跑步者**
2.  从选项卡行中，选择**构建阶段**
3.  展开**编译源**选项卡，点击 **+** 键。
4.  从弹出窗口中，点击**添加其他**
5.  导航到存储 C 文件的位置，例如`FLUTTER_PROJCT_ROOT/DART/native/cJSON/cJSON.c`，并添加`cJSON.c`和`cJSON.h`文件
6.  展开**编译源**选项卡，点击 **+** 键
7.  在弹出的窗口中，点击**添加其他**
8.  导航到 r `.c`文件存储的位置，例如`FLUTTER_PROJCT_ROOT/DART/native/cJSON/cJSON.c`
9.  如果需要选择**复制项目**并点击**完成**

现在，我们准备将生成的 Dart 绑定代码添加到 Flutter 应用程序中，加载库，并调用函数。

### 生成 FFI 绑定代码

我们将使用`ffigen`来生成绑定代码。首先，我们将把`ffigen`添加到 Flutter 应用程序中:

```
/// pubspec.yaml for my Flutter project
...
dependencies:
ffigen: ^4.1.2
...

ffigen:
output: 'lib/ffi/cjson_generated_bindings.dart'
name: 'CJson'
description: 'Holds bindings to cJSON.'
headers:
entry-points:
- 'DART/native/cJSON/cJSON.h'
include-directives:
- '**cJSON.h'
comments: false
typedef-map:
'size_t': 'IntPtr'

```

接下来，我们将运行`ffigen`:

```
flutter pub run ffigen

```

我们需要确保将`example.json`文件添加到 assets:

```
/// pubspec.yaml
...
flutter:
uses-material-design: true
assets:
- example.json
...

```

### 加载动态库

正如一个静态链接库可以被嵌入以在一个应用程序启动时加载，来自一个静态链接库的符号可以使用`DynamicLibrary.executable`或`DynamicLibrary.process`来加载。

在 Android 上，动态链接库以一组`.so` (ELF)文件的形式分发，每种架构一个文件。在 iOS 上，动态链接库作为一个`.framework`文件夹分发。

可以通过`DynamicLibrary.open`命令将动态链接库加载到 Dart 中。

我们将使用以下代码来加载库:

```
/// lib/ffi_loader.dart

import 'dart:convert';
import 'dart:developer' as dev_tools;
import 'dart:ffi';
import 'dart:io';
import 'package:ffi/ffi.dart';
import 'package:flutter/services.dart' show rootBundle;
import 'package:my_app_sample/ffi/cjson_generated_bindings.dart' as cj;

class MyNativeCJson {
MyNativeCJson({
required this.pathToJson,
}) {
final cJSONNative = Platform.isAndroid
? DynamicLibrary.open('libcjson.so')
: DynamicLibrary.process();
cjson = cj.CJson(cJSONNative);
}
late cj.CJson cjson;
final String pathToJson;
Future<void> load() async {
final jsonString = await rootBundle.loadString('assets/$pathToJson');
final cjsonParsedJson = cjson.cJSON_Parse(jsonString.toNativeUtf8().cast());
if (cjsonParsedJson == nullptr) {
dev_tools.log('Error parsing cjson.');
}
final dynamic dartJson = convertCJsonToDartObj(cjsonParsedJson.cast());
cjson.cJSON_Delete(cjsonParsedJson);
if (dartJson.toString() == json.decode(jsonString).toString()) {
dev_tools.log('Parsed Json: $dartJson');
dev_tools.log('Json converted successfully');
} else {
dev_tools.log("Converted json doesn't match\n");
dev_tools.log('Actual:\n$dartJson\n');
dev_tools.log('Expected:\n${json.decode(jsonString)}');
}
}
dynamic convertCJsonToDartObj(Pointer<cj.cJSON> parsedcjson) {
dynamic obj;
if (cjson.cJSON_IsObject(parsedcjson.cast()) == 1) {
obj = <String, dynamic>{};
Pointer<cj.cJSON>? ptr;
ptr = parsedcjson.ref.child;
while (ptr != nullptr) {
final dynamic o = convertCJsonToDartObj(ptr!);
_addToObj(obj, o, ptr.ref.string.cast());
ptr = ptr.ref.next;
}
} else if (cjson.cJSON_IsArray(parsedcjson.cast()) == 1) {
obj = <dynamic>[];
Pointer<cj.cJSON>? ptr;
ptr = parsedcjson.ref.child;
while (ptr != nullptr) {
final dynamic o = convertCJsonToDartObj(ptr!);
_addToObj(obj, o);
ptr = ptr.ref.next;
}
} else if (cjson.cJSON_IsString(parsedcjson.cast()) == 1) {
obj = parsedcjson.ref.valuestring.cast<Utf8>().toDartString();
} else if (cjson.cJSON_IsNumber(parsedcjson.cast()) == 1) {
obj = parsedcjson.ref.valueint == parsedcjson.ref.valuedouble
? parsedcjson.ref.valueint
: parsedcjson.ref.valuedouble;
}
return obj;
}
void _addToObj(dynamic obj, dynamic o, [Pointer<Utf8>? name]) {
if (obj is Map<String, dynamic>) {
obj[name!.toDartString()] = o;
} else if (obj is List<dynamic>) {
obj.add(o);
}
}
}

```

对于 Android，我们调用`DynamicLibrary`找到并打开`libcjson.so`共享库:

```
final cJSONNative = Platform.isAndroid
? DynamicLibrary.open('libcJSON.so')
: DynamicLibrary.process();

cjson = cj.CJson(cJSONNative);

```

在 iOS 中不需要这一特定步骤，因为当 iOS 应用程序运行时，所有链接的符号都会映射。

### 在颤动中测试呼叫

为了演示本机调用在 Flutter 中工作，我们向`main.dart`文件添加了用法:

```
// main.dart

import 'package:flutter/material.dart';
import 'ffi_loader.dart';

void main() {
runApp(const MyApp());

final cJson = MyNativeCJson(pathToJson: 'example.json');
await cJson.load();
}

```

接下来，我们运行应用:`flutter run`

瞧啊。我们已经成功地从我们的 Flutter 应用程序调用了本地库。

我们可以在控制台中查看本地调用的日志:

```
Launching lib/main_development.dart on iPhone 13 in debug mode...
lib/main_development.dart:1
Xcode build done. 16.5s
Connecting to VM Service at ws://127.0.0.1:53265/9P2HdUg5_Ak=/ws
[log] Parsed Json: {name: Majid Hajian, age: 30, nicknames: [{name: Mr. Majid, length: 9}, {name: Mr. Dart, length: 8}]}
[log] Json converted successfully

```

今后，我们可以在不同的小部件和服务中使用我们的 Flutter 应用程序中的这个库。

## 结论

Dart FFI 为将本地库集成到 Dart 和 Flutter 应用程序中提供了一个简单的解决方案。在本文中，我们演示了如何使用 Dart FFI 调用 Dart 中的 C 函数，并将 C 库集成到 Flutter 应用程序中。

您可能想进一步试验 Dart FFI，使用用其他语言编写的代码。我对 Go 和 Rust 的实验特别感兴趣，因为这些语言是内存管理的。Rust 特别有趣，因为它是一种内存安全的语言，而且性能相当好。

本文中使用的所有例子都可以在 [GitHub](https://github.com/mhadaily/native_libraries_in_flutter_using_dart_ffi) 上找到。

## 使用 [LogRocket](https://lp.logrocket.com/blg/signup) 消除传统错误报告的干扰

[![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/signup)

[LogRocket](https://lp.logrocket.com/blg/signup) 是一个数字体验分析解决方案，它可以保护您免受数百个假阳性错误警报的影响，只针对几个真正重要的项目。LogRocket 会告诉您应用程序中实际影响用户的最具影响力的 bug 和 UX 问题。

然后，使用具有深层技术遥测的会话重放来确切地查看用户看到了什么以及是什么导致了问题，就像你在他们身后看一样。

LogRocket 自动聚合客户端错误、JS 异常、前端性能指标和用户交互。然后 LogRocket 使用机器学习来告诉你哪些问题正在影响大多数用户，并提供你需要修复它的上下文。

关注重要的 bug—[今天就试试 LogRocket】。](https://lp.logrocket.com/blg/signup-issue-free)