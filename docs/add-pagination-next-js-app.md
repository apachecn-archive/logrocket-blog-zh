# 如何将分页添加到 Next.js 应用程序中

> 原文：<https://blog.logrocket.com/add-pagination-next-js-app/>

[Next.js](https://blog.logrocket.com/next-js-13-new-app-directory/) 是构建生产就绪型 React 应用程序的最简单、最流行的方法之一。近年来，Next.js 经历了指数级增长，许多公司都采用它来构建自己的应用程序。在本文中，我们将学习如何在 Next.js 应用程序中添加分页。我们将讨论如何建立一个 Next.js 项目，使用`getStaticProps`方法从 API 端点获取数据，以及实现分页本身。

*向前跳转:*

## 启动我们的 Next.js 项目

首先，我们将使用命令`npx create-next-app next-pagination`创建一个[新的 Next.js](https://blog.logrocket.com/next-js-13-new-app-directory/) 应用程序。然后，我们可以使用命令`cd next-pagination`进入项目目录，并运行`yarn dev or npm run dev`在开发服务器上启动项目。对于这个项目，我们将使用[普通 CSS](https://blog.logrocket.com/tag/css/) 进行造型，因为它更注重功能。

## 使用`getStaticProps`获取数据

用于实现分页的数据将来自于`[{JSON} Placeholder API](https://jsonplaceholder.typicode.com/posts)`。将使用`getStaticProps`函数提取数据。

`getStaticProps`函数总是在服务器上运行，Next.js 会在构建时使用`getStaticProps`返回的 props 预渲染页面。[获取 API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) 将用于从前面提到的 API 端点获取数据:

```
import Head from "next/head";
import Image from "next/image";
import styles from "../styles/Home.module.css";
import Pagination from "../src/components/Pagination";
import { useState, useEffect } from "react";
import { paginate } from "../src/helpers/paginate";

export default function Home({ data }) {
console.log(data)
  return (
   <div className={styles.container}>
     <Head>
       <title>Create Next App</title>
       <meta name="description" content="Generated by create next app" />
       <link rel="icon" href="/favicon.ico" />
     </Head>

     <p>NextJS X Pagination</p>

export const getStaticProps = async () => {
 const res = await fetch("https://jsonplaceholder.typicode.com/todos");
 const data = await res.json();

 return {
   props: { data },
 };
};

```

从 props 返回的数据将在`home`组件中被析构，因此它可以被记录在控制台中。这将确认数据已被提取。因此，在控制台中，我们应该有一个包含一组`100`对象的 post，正如 JSON Typicode 网站中指定的那样:

![Next.js Pagination JSON](img/e301c34534db3df948fbb4f6b11e7d2f.png)

现在，我们可以在网页上呈现这些数据，并查看它们在用户界面上的外观:

```
import Head from "next/head";
import Image from "next/image";
import styles from "../styles/Home.module.css";
export default function Home({ data }) {
 return (
   <div className={styles.container}>
     <Head>
       <title>Create Next App</title>
       <meta name="description" content="Generated by create next app" />
       <link rel="icon" href="/favicon.ico" />
     </Head>

     <p>
       <strong>NextJS x Pagination</strong>
     </p>
     {data.map((item) => {
       return <p key={item.id}>{item.title}</p>;
     })}
   </div>
 );
}

export const getStaticProps = async () => {
 const res = await fetch("https://jsonplaceholder.typicode.com/todos");
 const data = await res.json();

 return {
   props: { data },
 };
};

```

![Pagination in Next.js Example](img/b6aca0e85a9e56408c1e458706704681.png)

对于`Pagination`组件的实现，我们将创建一个`src`和`component`文件夹。在`component`中，我们将创建一个`Pagination.js`文件，该文件将在`index.js`文件中渲染:

```
const Pagination = () => {
 return (
   <div>Pagination</div>
 )
}
export default Pagination

```

`index.js`中渲染的`Pagination`会有四个道具:`items`、`currentPage`、`pageSize`、`onPageChange`。`items`将是我们从 API 获得的数据的长度，在本例中是`100`:

![Next.js Pagination Rendered Example](img/bb3d1385a7f7a870fcaf51b7a09fbc38.png)

因为我们希望从`1-10`分页，所以`pageSize`将被设置为`10`，并且因为页面将从第一页开始，所以`currentPage`将被存储在具有默认值`1`的状态中。

`onPageChange`将是一个设置当前页面的函数，例如，从第一页移动到第二页:

```
import Head from "next/head";
import Image from "next/image";
import styles from "../styles/Home.module.css";
import Pagination from "../src/components/Pagination";
import { useState } from "react";

export default function Home({ data }) {
 const [currentPage, setCurrentPage] = useState(1);
 const pageSize = 10;

 const onPageChange = (page) => {
   setCurrentPage(page);
 };

 return (
   <div className={styles.container}>
     <Head>
       <title>Create Next App</title>
       <meta name="description" content="Generated by create next app" />
       <link rel="icon" href="/favicon.ico" />
     </Head>

     <p>
       <strong>NextJS x Pagination</strong>
     </p>
     {data.map((item) => {
       return <p key={item.id}>{item.title}</p>;
     })}
    <Pagination
       items={data.length} // 100
       currentPage={currentPage} // 1
       pageSize={pageSize} // 10
       onPageChange={onPageChange}
        />
   </div>
 );
}

export const getStaticProps = async () => {
 const res = await fetch("https://jsonplaceholder.typicode.com/todos");
 const data = await res.json();

 return {
   props: { data },
 };
};

```

然后，我们将在`Pagination`中析构这四个属性，并将它们用于分页实现:

```
import styles from "../../styles/Home.module.css";

const Pagination = ({ items, pageSize, currentPage, onPageChange }) => {
 const pagesCount = Math.ceil(items / pageSize); // 100/10

 if (pagesCount === 1) return null;
 const pages = Array.from({ length: pagesCount }, (_, i) => i + 1);
console.log(pages)

 return (
   <div>
     <div>Pagination</div>
   </div>
 );
};

export default Pagination;

```

`items`将被`pageSize`除，并存储在一个`pagesCount`变量中。静态方法`Array.from()`将用于从长度`pagesCount`创建一个新的`Array`实例，即`10`。

让我们在控制台中记录`pages`，看看我们有什么:

![Console Logged in Next.js](img/0df1e17753ca288e7f89d4e4824fa4c2.png)

现在，我们可以映射`pages array`并在其中呈现值`(1-10)`。对于每个将实现`onClick`函数的值，都有一个`anchor`元素:

```
const Pagination = ({ items, pageSize, currentPage, onPageChange }) => {
 const pagesCount = Math.ceil(items / pageSize); // 100/10

 if (pagesCount === 1) return null;
 const pages = Array.from({ length: pagesCount }, (_, i) => i + 1);

  return (
   <div>
     <ul className={styles.pagination}>
       {pages.map((page) => (
         <li
           key={page}
           className={
             page === currentPage ? styles.pageItemActive : styles.pageItem
           }
         >
           <a className={styles.pageLink} onClick={() => onPageChange(page)}>
             {page}
           </a>
         </li>
       ))}
     </ul>
   </div>
 );
};

export default Pagination;

```

`pagination`框的样式将在`Home.module.css`中完成:

```
.container {
 padding: 0 2rem;
}

.pagination {
 display: flex;
 justify-content: space-between;
 align-items: center;
 list-style: none;
}

.pageLink {
 cursor: pointer;
}

/* pagination pageItem */
.pageItem {
 display: flex;
 justify-content: center;
 align-items: center;
 width: 2rem;
 height: 2rem;
 border: 1px solid #eaeaea;
 border-radius: 0.5rem;
 cursor: pointer;
}

/* pagination page item when active */
.pageItemActive {
 display: flex;
 justify-content: center;
 align-items: center;
 width: 2rem;
 height: 2rem;
 border: 1px solid #eaeaea;
 border-radius: 0.5rem;
 cursor: pointer;
 background-color: red;
}

```

一旦我们向下滚动到`posts`数据的末尾，我们应该会看到来自`1-10`的盒子被渲染。`onPageChange`函数通过`page(1-10)`的参数传递给`anchor`元素，所以当**框**的任意部分被点击时，它会将当前页面设置为被点击的**页面** **编号**。

记住，`index.js`文件中的`onPageChange`函数:

```
 const onPageChange = (page) => {
   setCurrentPage(page);
 };

```

现在，让我们看看我们有什么:

![Next.js Pagination Example](img/86ba9fd9fc3df4f463d1f670daa25086.png)

现在，我们已经能够在我们的应用程序中实现分页，但我们仍然有 100 篇文章呈现在第一页，而不是 10 篇。为了实现这一点，我们将在`paginate.js`文件中创建一个`helper`函数:

```
export const paginate = (items, pageNumber, pageSize) => {
 const startIndex = (pageNumber - 1) * pageSize;
 return items.slice(startIndex, startIndex + pageSize);
};

```

`paginate`文件将有三个参数:`items`、`pageNumber`和`pageSize`。该函数将在`index.js`文件中被调用，参数将按如下方式传递:

```
const paginatedPosts = paginate(data, currentPage, pageSize);

```

这里，`data`作为`items`传递，T1 是从 API 端点返回的数据数组。`currentPage`代表`pageNumber`，`pageSize`作为`pageSize`传入。

在`pagination`函数中，首先通过从`1`中减去`currentPage`数并乘以`pageSize(10)`来提取`startIndex`。然后，我们对`items`进行切片。这是我们最初从`startIndex`得到的数据数组，直到我们得到`startIndex` + `pageSize`值。

例如，在第一页上，`pageNumber`将是`1`，因此`startIndex`将是:`1-1 * 10 gives 0`。

那么，`startIndex (0) + pageSize`(在`index.js`中声明为`10`)就会产生`10`。因此，切片从`0`的`Array length`开始，一直到`9`。

让我们将`paginatedPosts`记录到控制台，看看我们有什么:

![Next.js Paginated Post Example](img/1d0888fc2506619e521dbe2cb0df9fb1.png)

我们可以查看网页上现有的内容:

![Next.js Pagination Second Example](img/42d9bbd7aa41f17842b7433215dd1750.png)

我们可以对所有页面进行分页:

![Next.js Paginated Final Product](img/31eea74a65b258cff1a0e13da767e0f3.png)

我们做到了！

# 结论

感谢您的阅读！我希望这篇教程能给你在 Next.js 应用程序中添加分页所需的知识。您可以根据您的用例定制样式，因为本教程主要关注实现功能的逻辑。你可以在我的[存储库](https://github.com/Taofiqq/nextjs-paginate)中找到这个项目的完整代码，在这里你可以在 Vercel [上找到部署的项目。编码快乐！](https://nextjs-paginate.vercel.app/)

## [LogRocket](https://lp.logrocket.com/blg/nextjs-signup) :全面了解生产 Next.js 应用

调试下一个应用程序可能会很困难，尤其是当用户遇到难以重现的问题时。如果您对监视和跟踪状态、自动显示 JavaScript 错误、跟踪缓慢的网络请求和组件加载时间感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/nextjs-signup)

.

[![](img/f300c244a1a1cf916df8b4cb02bec6c6.png)](https://lp.logrocket.com/blg/nextjs-signup)[![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/nextjs-signup)

LogRocket 就像是网络和移动应用的 DVR，记录下你的 Next.js 应用上发生的一切。您可以汇总并报告问题发生时应用程序的状态，而不是猜测问题发生的原因。LogRocket 还可以监控应用程序的性能，报告客户端 CPU 负载、客户端内存使用等指标。

LogRocket Redux 中间件包为您的用户会话增加了一层额外的可见性。LogRocket 记录 Redux 存储中的所有操作和状态。

让您调试 Next.js 应用的方式现代化— [开始免费监控](https://lp.logrocket.com/blg/nextjs-signup)。