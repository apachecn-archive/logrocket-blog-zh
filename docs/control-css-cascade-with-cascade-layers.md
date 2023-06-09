# 用层叠层控制 CSS 层叠

> 原文：<https://blog.logrocket.com/control-css-cascade-with-cascade-layers/>

对于大多数开发人员来说，CSS 相当容易学习，但是对于那些缺乏适当的代码结构计划的人来说，CSS 可能是不可原谅的。这主要是由于级联强烈抵制控制。

一些开发人员已经能够使用诸如倒三角 CSS (ITCSS)、面向对象 CSS (OOCSS)和块元素修改器(BEM)等方法来驯服级联。这些方法允许开发人员编写结构和逻辑代码，以既定的顺序尽可能降低选择器的特异性。

在大多数情况下，这些方法是有效的，但是它们没有完全解决选择器特异性和出现顺序之间的冲突。

谢天谢地，[层叠层](https://www.w3.org/TR/css-cascade-5/#layering)要来 CSS 了。新的 at 规则旨在将级联的控制权完全交还给开发者。使用级联层，开发人员可以决定他们希望如何对层进行排序，并且可以更容易地分离和访问各个层。

在本文中，我们将介绍 cascade `@layer` at-rule，讨论它是如何工作的，并探索如何使用这个特性来防止或解决样式规则之间的冲突。

## 了解级联

cascade 是一个核心 CSS 算法，它允许多个样式表影响文档的表示，并定义分配给文档内元素的竞争属性值的解决方案。

cascade 的主要工作是评估给定属性的 CSS 声明的无序列表，其中选择器匹配特定元素，并检查冲突值。如果级联发现任何冲突值，它将根据它们声明的优先级对它们进行排序，并输出一个级联值应用于该元素。

例如，以下代码中的段落元素具有冲突的值对`red`和`blue`:

```
/*<p class="text" id="text">...</p>*/

.text{
  color: red;
}

#text{
  color: blue;
}

```

在这种情况下，级联必须确定哪个声明优先。

为此，级联将根据以下标准[按照优先级](https://drafts.csswg.org/css-cascade/#cascade-sort)的降序对声明进行排序:

*   起源和重要性
*   封装上下文
*   样式属性
*   特征
*   出场顺序

级联根据这些标准检查每个声明，确定哪个声明具有最大的级联权重。如果某个特定标准的结果未定，级联将继续下一个标准。

## 引入级联层

层叠层是一个新的 CSS 特性，是[层叠继承级别 5 规范](https://www.w3.org/TR/css-cascade-5/)的一部分。该功能最初是由 CSS 规范作者[米丽娅姆·苏珊娜](https://twitter.com/TerribleMia/)在 2019 年提出的。层叠层允许使用@layer at-rule 将样式规则分割成几个层，并且还允许确定层的特性顺序。

有了级联层，就不需要担心层块中每个选择器的特殊性或者声明在样式表中的排列顺序。这是因为浏览器总是尊重层叠层的顺序，而忽略选择器的特性和出现顺序。我们将在本文后面更详细地讨论这一点，我们还将看一些 CSS 层顺序的例子。

## 用`@layer`创建层叠层

级联层`@layer`是一个定义名称的 at 规则，很像 CSS `@keyframe`和`@font-face` at 规则。这是定义级联层的官方语法。它接受一个标识符或名称，可以被引用并用于在样式表的任何地方创建级联层块。

可使用`@layer` at-rule 以两种方式之一定义级联层:

1.  Using URL imports with `@import` at-rule

    ```
    @import(reset.css) layer(reset);
    ```

    该方法将外部或第三方样式表的内容附加到新的或现有的层，该样式表的相对 URL 在`@import` at-rule 中指定。例如，我们可以为`reset`样式表导入并创建一个层，如下所示:

    ```
    @import(reset.css) layer(reset);

    ```

2.  使用图层块:

    ```
    @layer reset {     *{       margin: 0;       padding: 0;     } }
    ```

也可以在没有导入或层块的情况下声明层，而是使用稍后可以在同一层范围(样式表)中引用的标识符(名称)。

如果声明的图层名称与同一图层范围中定义的现有图层名称匹配，块中的嵌套样式规则将被附加到现有图层:

```
@layer reset;
@layer typography;
@layer theme;

@layer reset{ /*Appends to the reset layer*/
   *{
     margin: 0;
     padding: 0;
  }
}

@layer typography{ /*Appends to the typography layer*/
   ...
}

@layer theme{ /*Appends to the theme layer*/
   ...
}

@layer utilities{ /*Creates a new layer*/
  ...
}

```

如果图层块的标识符与现有图层不匹配，将创建一个新图层。

未定义标识符的层称为匿名层。无法引用它们，因为它们没有名称；因此，额外的样式规则不能附加到这种类型的图层。

```
/*Anonymous layer block*/
@layer {
  …
}
//Anonymous @import layer
@import(reset.css) layer;

```

### 层顺序

层的优先级基于它第一次出现在样式表中的时间。例如，我们之前检查的代码片段的层顺序如下:

1.  `reset`
2.  `typography`
3.  `theme`

当稍后在样式表中引用层的标识符时，层出现的顺序并不重要。决定层优先级的是初始外观。

```
@layer reset; /*first layer*/
@layer typography; /*second layer*/
@layer theme; /*third layer*/

@layer theme { /*Appends to third layer*/
  …
}
@layer reset { /*Appends to first layer*/
  …
}
@layer typography { /*Appends to second layer*/
  …
}

```

如这个代码片段所示，无论样式表中引用的层的顺序和位置如何，层顺序都保持不变。

还可以使用速记语法创建层堆栈，该语法接受在单行逗号分隔规则中提供的多个标识符:

```
@layer reset, typography, theme;

```

此方法的行为与多行声明顺序相同。

### 层特异性

当评估层标准时，级联将总是从与在层顺序中位于后面的级联层相关联的选择器中挑选获胜规则。这是因为级联层是按照出现的顺序排列的，就像传统的 CSS 声明一样。

让我们带回最典型的层堆栈:

```
@layer reset, typography, theme;

```

在这个堆栈中，`typography`层中定义的样式规则将优先于`reset`层，而`theme`层的样式将优先于所有之前的层。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

换句话说，一个简单的后来声明的层将总是优先并覆盖先前声明的层，即使先前声明的层具有更大的级联权重。这里有一个实际的例子:

```
/*<h2 class=”title” Id=”title”>…</h2>*/

@layer typography {
    h2 #title{
      color: orange;
    }
}
@layer headings {
    .title{
      color: #333;
    }
}

```

尽管`typography`层中的选择器比`headings`层中的选择器具有更高的特异性，但后者仍然占优势，因为它所嵌套的层在层顺序中是最后一层。

### 未分层花柱特异性

非分层样式遵循级联层之外的样式规则，并且比样式表中的任何层具有最高的优先级。因此，非分层的样式总是胜过嵌套在层中的样式，不管它们的特性或外观顺序如何。这是因为每个未分层的样式都会自动添加到隐式最终层，因此具有最高的优先级。

例如，考虑以下代码:

```
/*<p class=”subtitle”>…</p>*/

@layer article {
  .subtitle{
    Color: red ;
  }
}
P{
  color: #333 ;
}

```

即使`article` *层中的规则有更具体的选择器，它也会被不太具体的`P`选择器覆盖。因此，段落元素的内容将是灰色而不是红色。*

 *未分层样式的显示顺序不会影响它们在图层上的优先级。如果上面例子中的`P`选择器被定义或放置在`article`层之前，段落仍然是灰色的。

现在，让我们将未分层的样式添加到模型层堆栈的层范围中:

```
@layer reset, typography, theme;

h2{ /*unlayered styles*/
  …
}
@layer reset{
  …
}
@layer typography{
  …
}
@layer theme{
  …
}

```

新的层顺序如下:

1.  `reset`
2.  `typography`
3.  `theme`
4.  `unlayered styles`

然而，在涉及到`!important`标志的情况下，该层的优先级将被颠倒。先前定义的图层中带有`!important`标志的竞争样式将始终优先于稍后定义的图层中带有`!important`标志的样式，以及未分层的样式。

```
@layer reset{ /*1st layer*/
 h2{
   color: black !important;
 }
}
@layer typography{ /*2nd layer*/
 h2{
   color: #333 !important;
 }
}
@layer theme{ /*3rd layer*/
 h2{
   color: pink !important;
 }
}
H2{ /*1st unlayered style*/
color: blue;
}
H2{ /*2nd unlayered style*/
color: red !important;
}

```

本例中的特异性顺序如下:

1.  `!mportant` `reset`
2.  `!mportant` `typography`
3.  `!mportant` `theme`
4.  `!mportant` `unlayered style`
5.  `unlayered style`

### 嵌套层

图层块也可以嵌套在其他图层中:

```
@layer theme{
  @layer dark{
     ...
  }

  @layer light{
     ...
  }
}

```

当一个层嵌套在另一个层中时，它的标识符将变成父标识符和子标识符的组合，中间用句点分隔。

```
@layer typography{
  @layer headings{

  }
}

@layer typography.headings{
  h2{
     color: blue;
  }
}

```

在这个例子中，我们使用`typography.headings`图层块将额外的样式添加到嵌套在`typography`图层中的`headings`图层。

或者，可以在层堆栈的前面定义嵌套层:

```
@layer theme{
   @layer theme.dark theme.light;
}

```

嵌套层的范围在嵌套层内。因此，父层之外的层不能被嵌套层引用。相反，将创建一个具有相同标识符的新图层。

```
@layer reset theme typography;

@layer typography{
  @layer theme {
   .dark{
     background-color: #333;
   }
  }
}

```

例如，在这段代码中，我们试图用嵌套在`typography`层中的`theme`层块向`theme`层添加额外的样式。但是，由于嵌套层只能引用其范围内的层，因此创建了一个新的`theme`层。

层内的特异性行为与层顺序特异性相同。嵌套层堆栈中后面的层将具有最高优先级。

```
@layer theme{
  h2{ /*nested unlayered style*/
   color: black;
  }
  @layer dark{ /*nested layer*/
    h2{
       color: #fff;
    }
  }
}

```

在这个例子中，`h2`选择器将比`theme.dark`层选择器具有更高的优先级，因为层外的样式在层顺序中总是排在最后。

### 级联算法定位

一旦完全支持级联层，它将被定位在 CSS 级联算法中外观标准的特异性和顺序之前，使其优先于这些标准。

*   起源和重要性
*   封装上下文
*   样式属性
*   级联层
*   特征
*   出场顺序

在级联评估层标准中的竞争规则的情况下，获胜的规则是按层顺序确定的，算法不会考虑出现的特异性或顺序。

## 多填色

新的和实验性的 CSS 特性通常可以被采用或使用 polyfills 或 CSS 属性(如`@support`)逐步集成。

不幸的是，在撰写本文时，没有任何可用于层叠层的 polyfills，并且`@support`属性目前不支持该功能。

但是，您可以使用目前支持层叠层特性的几种浏览器之一，开始尝试并熟悉层叠层。

## 浏览器支持

默认情况下，级联层功能目前在以下浏览器中受实验性支持:

*   铬 99+
*   火狐每夜 97+
*   Safari 技术预览 137+

![CSS Cascade Layers Browser Support](img/3903ea0e9d749fc89c8b273d42c94a1a.png)

## 结论

在本文中，我们介绍了级联层，并探讨了层顺序、层特异性、非分层样式和嵌套层。我们还回顾了级联算法定位以及当前对级联层的 polyfill 和浏览器支持的可用性。

有了级联层，就没有必要花费宝贵的时间去纠结 CSS 层的特性或外观顺序。级联层提供了对级联和抑制复杂性的更多控制，如代码库中的回归和样式冲突。

在官方文档中阅读更多关于层叠层的信息。

## 你的前端是否占用了用户的 CPU？

随着 web 前端变得越来越复杂，资源贪婪的特性对浏览器的要求越来越高。如果您对监控和跟踪生产环境中所有用户的客户端 CPU 使用、内存使用等感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/css-signup)

.

[![LogRocket Dashboard Free Trial Banner](img/dacb06c713aec161ffeaffae5bd048cd.png)](https://lp.logrocket.com/blg/css-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/css-signup)

LogRocket 就像是网络和移动应用的 DVR，记录你的网络应用或网站上发生的一切。您可以汇总和报告关键的前端性能指标，重放用户会话和应用程序状态，记录网络请求，并自动显示所有错误，而不是猜测问题发生的原因。

现代化您调试 web 和移动应用的方式— [开始免费监控](https://lp.logrocket.com/blg/css-signup)。*