---
title: "Next.js 教程-概览"
date: 2025-09-27T22:04:49+08:00
draft: false
tags: ["Next.js", "React", "Vue"]
categories: ["Frontend"]
summary: "Next.js 教程-概览"
---

Next.js 教程-概览
=====================

> 本文由GPT5-Thinking DeepResearch生成并手动修改校对。

欢迎来到 Next.js 教程！本指南面向有 Vue.js 使用经验但缺乏 React 背景的开发者。Next.js 是基于 React 的前端框架，类似于 Vue 生态中的 Nuxt.js——两者都是各自生态中的服务端渲染框架，都支持 SSR（服务端渲染）、SSG（静态站点生成）、基于文件的路由等功能[blog.csdn.net](https://blog.csdn.net/corruptwww/article/details/129134926#:~:text=match%20at%20L362%20Nuxt%20,js%20%E3%80%82%E4%B8%A4%E8%80%85%E9%83%BD%E4%BB%A3%E8%A1%A8%E4%BA%86%E7%8E%B0%E4%BB%A3%E5%85%A8%E6%A0%88%E5%BC%80%E5%8F%91%E8%B6%8B%E5%8A%BF%EF%BC%8C%E5%80%BC%E5%BE%97%E6%B7%B1%E5%85%A5%E5%AD%A6%E4%B9%A0%E3%80%82)。通过本教程，您将逐步学习 Next.js 的基础和中级用法，从项目结构、页面和路由，到组件与状态管理、样式方案，以及网页渲染原理等内容。在讲解过程中，我们会结合 Vue 的概念作对比，帮助您快速理解 Next.js 与 Vue 的异同。

1\. Next.js 项目结构和初始配置
---------------------

**项目创建：**要开始一个 Next.js 项目，您需要确保已安装 Node.js（建议 Node 18+）。然后使用官方脚手架命令创建项目：

```bash
npx create-next-app@latest my-next-app --typescript
```

上面的命令会交互式询问您一些配置，例如是否使用 **TypeScript**、是否集成 **Tailwind CSS**、是否将代码放入 `src/` 目录，以及是否使用 App Router（推荐）等。选择相应的选项后，脚手架将自动生成一个基础的 Next.js 项目。创建完成后，进入项目目录运行开发服务器：`npm run dev`（或 `yarn dev`），即可在浏览器打开 http://localhost:3000 预览初始项目页面。

**项目结构：**Next.js 项目的文件结构具有约定俗成的意义。默认情况下（使用 App Router），项目主要包含以下内容：runoob.com

```
my-next-app/        # 项目根目录
├─ app/            # 应用主目录（Next.js 13+ 的 App Router）
│   ├─ layout.tsx   # 应用的根布局（全局作用，必须包含 <html> 和 <body>）
│   ├─ page.tsx     # 默认首页页面（对应路径 "/"）
│   ├─ globals.css  # 全局样式表
│   └─ ...          # 其他页面目录或文件
├─ public/         # 静态资源目录（图片、字体等，可直接通过路径访问）
├─ next.config.js   # Next.js 配置文件
├─ package.json     # 项目依赖和脚本
├─ tsconfig.json    # TypeScript 配置
└─ ...             # 其他配置文件（.eslintrc、.env 等）
```

在上述结构中，`app/` 目录用于基于**文件系统路由**的页面组织（Next.js 14 推荐使用的模式）。每个页面通常是一个 `.tsx` 文件，由框架自动映射为对应的路由，无需手动配置runoob.com。`layout.tsx` 是 Next.js **根布局**文件，其作用是为所有页面提供一个包裹布局（例如定义通用的 `<head>`、导航栏等），它会保持挂载状态，不会在页面切换时卸载，从而保持跨页的状态和组件不变[nextjs.freeourdays.com](https://nextjs.freeourdays.com/nextjs14/routing/layouts-and-templates/#:~:text=%E5%B8%83%E5%B1%80%E6%98%AF%E5%9C%A8%E5%A4%9A%E4%B8%AA%E8%B7%AF%E7%94%B1%E4%B9%8B%E9%97%B4%20%E5%85%B1%E4%BA%AB%20%E7%9A%84UI%E3%80%82%E5%9C%A8%E5%AF%BC%E8%88%AA%E6%97%B6%EF%BC%8C%E5%B8%83%E5%B1%80%E4%BF%9D%E6%8C%81%E7%8A%B6%E6%80%81%EF%BC%8C%E4%BF%9D%E6%8C%81%E4%BA%A4%E4%BA%92%E6%80%A7%EF%BC%8C%E5%B9%B6%E4%B8%94%E4%B8%8D%E4%BC%9A%E9%87%8D%E6%96%B0%E6%B8%B2%E6%9F%93%E3%80%82%E5%B8%83%E5%B1%80%E8%BF%98%E5%8F%AF%E4%BB%A5%20%E5%B5%8C%E5%A5%97%E3%80%82)[nextjs.freeourdays.com](https://nextjs.freeourdays.com/nextjs14/routing/layouts-and-templates/#:~:text=)。此外，`public/` 下的文件可直接通过 `/<文件名>` 访问（类似 Vue 项目中 `public` 目录的用法）。`next.config.js` 则可用于配置构建选项等（大部分基础场景可不改动）。如果您选择在脚手架中使用 `src/` 目录，Next.js 也支持将源文件放入 `src/app/` 等，但原理相同。

**App Router vs Pages Router：**Next.js 13 引入了新的 App Router（`app/` 目录）来替代传统的 Pages 路由（`pages/` 目录），二者目前可并存。runoob.com本教程基于最新的 Next.js 14+ 并使用 **App Router** 进行讲解。App Router 提供了更灵活的路由和布局机制，以及对服务端组件等新特性的支持。如果您在阅读官方资料时遇到 `pages/` 目录的用法，可以将其类比为旧的模式；但现在新建项目默认使用 App Router，我们也将专注于这种模式。

2\. 页面构建与文件系统路由
---------------

Next.js 使用**文件系统路由**来自动映射页面路径runoob.com。也就是说，您在 `app/` 目录下创建的文件和文件夹结构会直接对应到网站URL路径，而无需像 Vue Router 那样手动配置路由表。这种约定大大简化了路由管理，使开发过程直观自然。

**基本路由规则：**在 `app/` 目录下：

*   每个文件夹代表一个路由路径段，每个文件夹下的 `page.tsx` 定义该路径下的页面组件。runoob.com
*   命名为 `page.tsx` 的文件会被当作页面。文件所在的路径结构决定了页面 URL。
*   `app/layout.tsx` 文件定义**全局布局**，通常包含 `<html>` 和 `<body>` 标签。您可以在其中引入导航栏、页脚等公共UI。
*   可以通过嵌套文件夹实现多级路由；通过特殊命名实现动态路由和其他功能（下文会提及）。

举例来说，假设我们在 `app/` 目录下有如下结构：

```
app/
├── page.tsx          // 对应根路径 "/"
├── about/
│   └── page.tsx      // 对应路径 "/about"
├── blog/
│   └── [slug]/
│       └── page.tsx  // 对应动态路径，如 "/blog/abc" 或 "/blog/123"
└── dashboard/
    ├── layout.tsx    // 仪表盘模块的局部布局
    └── settings/
        └── page.tsx  // 对应路径 "/dashboard/settings"
```

根据上述结构，Next.js 会自动创建以下路由：`"/"` 映射到根目录的 `page.tsx`，`"/about"` 映射到 `about/page.tsx`，而 `"/blog/abc"` 会匹配 `blog/[slug]/page.tsx` 并通过参数 `slug` 获取 `"abc"` 等实际值。runoob.comrunoob.com**动态路由**通过方括号语法实现，例如 `[slug]` 表示一个动态片段。在 Next.js 中，可以使用 React Hooks 来获取动态参数，如 `useParams()`（从 `next/navigation` 导入）返回一个参数对象，里面包含 `slug` 等键值，用法类似 Vue Router 提供的 `$route.params`。runoob.comrunoob.com

**页面组件：**每个 `page.tsx` 文件需要默认导出一个 React 组件。例如 `app/about/page.tsx` 可以简单编写为：

```tsx
// app/about/page.tsx
export default function AboutPage() {
  return <h1>关于我们页面</h1>;
}
```

这个组件将对应 `/about` 路径。当在浏览器访问 `/about` 时，Next.js 会自动渲染该组件的内容。

**导航链接：**在 Next.js 应用中，推荐使用内置的 `<Link>` 组件在页面之间导航，它类似于 Vue Router 的 `<router-link>`。使用方法也很简单：从 `'next/link'` 导入 Link 组件，然后像使用锚点一样定义 `href` 即可。例如，在首页的组件中添加一个链接到 About 页面：

```tsx
// app/page.tsx （首页组件）
import Link from 'next/link';

export default function HomePage() {
  return (
    <div>
      <h1>欢迎来到本站！</h1>
      <Link href="/about">前往关于我们</Link>
    </div>
  );
}
```

这样，点击该链接会在客户端进行页面切换。Next.js 的 `<Link>` 会拦截浏览器的跳转，利用**客户端路由**实现无刷新加载，并会预取（prefetch）目标页，提高导航的流畅度。

**布局与嵌套路由：**Next.js 提供布局 (`layout.tsx`) 文件用于封装页面的公共结构。例如，我们可以在根布局 `app/layout.tsx` 中定义网站通用的框架：

```tsx
// app/layout.tsx
import './globals.css';

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="zh-CN">
      <body>
        <header>我的站点头部</header>
        <main>{children}</main>
        <footer>© 2025 我的站点</footer>
      </body>
    </html>
  );
}
```

以上代码中，我们引入了全局样式表，并在 `<body>` 中包裹了 `{children}`——这个 `children` 即各路由页面的具体内容。这样，无论跳转到哪个页面，头部和尾部布局都会保持存在[nextjs.freeourdays.com](https://nextjs.freeourdays.com/nextjs14/routing/layouts-and-templates/#:~:text=%E5%B8%83%E5%B1%80%E6%98%AF%E5%9C%A8%E5%A4%9A%E4%B8%AA%E8%B7%AF%E7%94%B1%E4%B9%8B%E9%97%B4%20%E5%85%B1%E4%BA%AB%20%E7%9A%84UI%E3%80%82%E5%9C%A8%E5%AF%BC%E8%88%AA%E6%97%B6%EF%BC%8C%E5%B8%83%E5%B1%80%E4%BF%9D%E6%8C%81%E7%8A%B6%E6%80%81%EF%BC%8C%E4%BF%9D%E6%8C%81%E4%BA%A4%E4%BA%92%E6%80%A7%EF%BC%8C%E5%B9%B6%E4%B8%94%E4%B8%8D%E4%BC%9A%E9%87%8D%E6%96%B0%E6%B8%B2%E6%9F%93%E3%80%82%E5%B8%83%E5%B1%80%E8%BF%98%E5%8F%AF%E4%BB%A5%20%E5%B5%8C%E5%A5%97%E3%80%82)，仅中间的 `children` 部分根据路由切换。**嵌套布局：**类似地，您可以在某个子路由文件夹下再添加一个 `layout.tsx` 实现局部布局。例如在 `app/dashboard/layout.tsx` 定义仪表盘模块的导航侧栏等，该布局将只应用于 `/dashboard` 下的子页面[nextjs.freeourdays.com](https://nextjs.freeourdays.com/nextjs14/routing/layouts-and-templates/#:~:text=)。布局组件在导航时不会被卸载，可保留其内部状态，这一点与 Vue 中根组件不销毁子组件类似，但实现方式不同（Next 由框架自动处理布局保活）。利用布局和嵌套路由，您可以轻松组织复杂的页面结构。

3\. 组件与 Props 的使用（对比 Vue）
-------------------------

在 Next.js（React）中，“组件”是构建UI的基本单元，概念上类似于 Vue 的组件。所有页面 (`page.tsx`) 实际上也是一个 React 组件。此外，您可以创建任意可重用的**UI组件**，并在页面或其他组件中引入使用，从而实现模块化开发runoob.com。这一点和 Vue 非常相似。

**定义组件：**React 组件通常使用函数定义（函数式组件）。例如，我们可以创建一个简单的问候组件：

```tsx
// app/components/Greeting.tsx
interface GreetingProps { 
  name: string;         // 定义 props 属性的类型
}

const Greeting: React.FC<GreetingProps> = ({ name }) => {
  return <p>你好，{name}！</p>;
};

export default Greeting;
```

上述代码定义了一个 Greeting 组件：它接受一个 `name` 属性，并渲染一段包含问候语的段落。其中我们使用了 TypeScript 接口 `GreetingProps` 来定义 props 的类型，这在 React+TS 中是常见模式。与 Vue SFC 单文件组件中通过 `props: {...}` 或 Composition API 的 `defineProps` 定义传入参数类似，React 直接通过函数的参数来获取 props[blog.csdn.net](https://blog.csdn.net/corruptwww/article/details/129134926#:~:text=%E7%BB%84%E4%BB%B6%20%E7%BB%84%E5%90%88%E5%BC%8FAPI%20%E5%87%BD%E6%95%B0%E5%BC%8F%E7%BB%84%E4%BB%B6%20%E5%85%A8%E5%B1%80%E7%BB%84%E4%BB%B6%E9%80%9A%E4%BF%A1%20emit%E6%80%BB%E7%BA%BF,model%20%E5%8D%95%E5%90%91%E7%BB%91%E5%AE%9AuseEffect%E3%80%81%E4%B8%8D%E6%B8%B2%E6%9F%93%E5%80%BCuseRef%20%E5%93%8D%E5%BA%94%E5%BC%8F%E5%8E%9F%E7%90%86%E5%8F%8A%E6%9B%B4%E6%96%B0%E7%AD%96%E7%95%A5%20%E8%99%9A%E6%8B%9Fdom%2B%E5%B1%80%E9%83%A8%E8%AE%A2%E9%98%85%20%E8%99%9A%E6%8B%9Fdom%2B%E5%85%A8%E9%87%8Fdiff)。

**使用组件和传递 Props：**要在页面中使用这个 Greeting 组件，我们先确保组件已导出，然后在需要的页面文件中导入并以 JSX 标签形式使用：

```tsx
// app/page.tsx（主页文件）
import Greeting from './components/Greeting';

export default function HomePage() {
  const userName = '小明';
  return (
    <div>
      <h1>首页</h1>
      <Greeting name={userName} />
    </div>
  );
}
```

在上述 JSX 中，我们像使用 HTML 标签一样使用了 `<Greeting />` 并传入属性 `name`。这会在页面渲染时调用 Greeting 组件，将 `userName` 的值传递给它的 props，从而输出“你好，小明！”。这和 Vue 中父组件在模板里使用子组件并通过属性传值的方式是一致的，只是语法有所不同：Vue 模板中可能写`<Greeting :name="userName" />`，在 React JSX 中则是直接 `name={userName}`。

**子组件内容传递（Slot vs Children）：**在 Vue 中，可以通过插槽 `<slot>` 向子组件传入嵌套内容；在 React 则通过 `props.children` 实现类似的功能[blog.csdn.net](https://blog.csdn.net/corruptwww/article/details/129134926#:~:text=%E7%BB%84%E4%BB%B6%20%E7%BB%84%E5%90%88%E5%BC%8FAPI%20%E5%87%BD%E6%95%B0%E5%BC%8F%E7%BB%84%E4%BB%B6%20%E5%85%A8%E5%B1%80%E7%BB%84%E4%BB%B6%E9%80%9A%E4%BF%A1%20emit%E6%80%BB%E7%BA%BF,model%20%E5%8D%95%E5%90%91%E7%BB%91%E5%AE%9AuseEffect%E3%80%81%E4%B8%8D%E6%B8%B2%E6%9F%93%E5%80%BCuseRef%20%E5%93%8D%E5%BA%94%E5%BC%8F%E5%8E%9F%E7%90%86%E5%8F%8A%E6%9B%B4%E6%96%B0%E7%AD%96%E7%95%A5%20%E8%99%9A%E6%8B%9Fdom%2B%E5%B1%80%E9%83%A8%E8%AE%A2%E9%98%85%20%E8%99%9A%E6%8B%9Fdom%2B%E5%85%A8%E9%87%8Fdiff)。React 的做法是：如果您在使用组件时编写了嵌套的 JSX，比如 `<Layout>...</Layout>`，那么 `<Layout>` 组件内部可以通过接收 `children` prop来访问这些子节点。实际上，我们在上一节的布局示例中已经使用了这种模式：`RootLayout` 组件的参数解构了 `{ children }`，并将其渲染在 `<main>` 中。这和 Vue 中在组件里使用 `<slot></slot>` 占位符以渲染父组件传入的内容本质类似。**父子通信：**总体而言，React 强调**单向数据流**，父组件通过 props 将数据下传，子组件通过调用父组件传来的回调函数向上传递事件或数据（React中常见模式是父组件将一个函数以 prop 传给子组件，子组件调用该函数并传参，实现“通知”父组件）; Vue 中则有内置的事件机制（ `$emit` ）或 Vuex/Pinia 等来实现子->父通信。不同框架在细节上有所区别，但思想都是**数据向下，事件向上**的单向流动，这样能够更好地管理状态变化。

**注意：**与 Vue 不同，React 没有指令式的双向绑定（如 `v-model`）语法糖。如果需要实现类似的双向数据绑定（例如表单输入），需要手动为表单元素绑定 `onChange` 事件以更新状态，再将状态值传回组件，这被视为**受控组件**模式。这体现了 React “单向数据绑定”的理念：数据从 state 流向视图，用户操作触发事件再改变 state，进而更新视图[blog.csdn.net](https://blog.csdn.net/corruptwww/article/details/129134926#:~:text=)。虽然没有直接的 `v-model`，但通过合理封装，React 也能方便地处理表单交互，只是方式上更显式一些。

4\. 状态与变量更新机制（useState 与 useEffect 等）
-------------------------------------

交互式应用需要管理组件的**状态（state）**。在 Vue 中，状态通常是组件的 data 或 `setup()` 返回的响应式对象/变量，而 Vue 的响应式系统会追踪这些数据的变化并自动更新 DOM。React 则通过**Hook 函数**来管理状态和组件生命周期。最基础的两个 Hook 是 `useState` 和 `useEffect`。

**useState：组件状态声明** – `useState` 是一个函数，用于在函数组件中引入状态变量。它返回一个**状态值**和一个**更新该状态的函数**。用法示例：

```tsx
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0); // 声明一个名为 count 的状态，初始值为0

  return (
    <div>
      <p>当前计数：{count}</p>
      <button onClick={() => setCount(count + 1)}>+1</button>
    </div>
  );
}
```

以上代码定义了一个计数器组件：使用 `useState(0)` 声明了 `count` 状态，初始值为0，并获得更新函数 `setCount`。每当按钮点击时，调用 `setCount(count + 1)` 更新状态。React 会触发组件重新渲染，此时 `count` 新值会反映在界面上。这类似 Vue 中修改 `data` 中的数据会让模板自动更新。不过要注意，**React 禁止直接修改状态变量**，必须调用更新函数blog.3vyd.com。例如，不要尝试 `count++` 来更新（这不会触发重新渲染），而应始终使用 `setCount`。对于引用类型的状态（如对象、数组），也应通过创建新对象来更新状态（遵循**不可变数据**的理念），这样 React 才能检测到变化并触发更新。Vue 则相反，内部实现了对对象/数组的拦截，直接修改它们也能检测变化，但 React 则需要通过返回新对象的方式告知框架“有变更”。

**React 响应式机制 vs Vue 响应式机制：**Vue 的响应式是通过**数据劫持**实现的（如使用 `Proxy` 监听对对象属性的访问和修改）[cnblogs.com](https://www.cnblogs.com/longmo666/p/18003487#:~:text=%E6%80%BB%E7%BB%93%E6%9D%A5%E8%AF%B4%EF%BC%8C%E5%9C%A8Vue%E4%B8%AD%EF%BC%8C%E5%93%8D%E5%BA%94%E5%BC%8F%E6%98%AF%E9%80%9A%E8%BF%87%E5%AF%B9%E5%AF%B9%E8%B1%A1%E5%B1%9E%E6%80%A7%E7%9A%84%E5%BA%95%E5%B1%82%E6%8B%A6%E6%88%AA%E6%9D%A5%E5%AE%9E%E7%8E%B0%EF%BC%9B%E8%80%8C%E5%9C%A8React%E4%B8%AD%EF%BC%8C%E5%93%8D%E5%BA%94%E5%BC%8F%E6%9B%B4%E4%BE%A7%E9%87%8D%E4%BA%8E%E7%BB%84%E4%BB%B6%E5%B1%82%E7%BA%A7%EF%BC%8C%E9%80%9A%E8%BF%87props%E5%92%8Cstate%E7%9A%84%E5%8F%98%E5%8C%96%E6%9D%A5%E9%A9%B1%E5%8A%A8%E6%95%B4%E4%B8%AAUI%E7%9A%84%E6%9B%B4%E6%96%B0%EF%BC%8C%E5%B9%B6%20%E4%B8%94%E4%BD%BF%E7%94%A8%E8%99%9A%E6%8B%9F%20)；当您直接修改一个响应式对象的属性时（例如 `this.count++`），Vue 侦测到变化会自动更新 DOM。React 则采用**显式状态更新**模式：状态保存在组件内部，当调用 `setState`（类组件）或 Hook 返回的更新函数（函数组件）时，React 会将新的 state 与旧值对比，然后通过**虚拟DOM diff**计算出需要更新的界面并批量更新blog.3vyd.com。因此在 React 中，如果不调用更新函数，修改变量不会触发UI改变。在这一机制下，React 更加强调**单向数据流**和不可变数据，状态的变化由开发者手动触发，从而定位数据变化来源更清晰，这对于大型应用的可预测性是有利的[blog.csdn.net](https://blog.csdn.net/corruptwww/article/details/129134926#:~:text=)。

**useEffect：副作用处理** – `useEffect` 是另一个常用 Hook，用于处理组件的副作用逻辑（side effects），例如数据获取、订阅事件、手动修改DOM、打印日志等。这些操作在React渲染过程中执行会产生副作用，因而用 `useEffect` 来隔离管理。基本用法：

```tsx
import { useState, useEffect } from 'react';

function Timer() {
  const [seconds, setSeconds] = useState(0);

  // 副作用：每秒递增计时
  useEffect(() => {
    const interval = setInterval(() => {
      setSeconds((s) => s + 1);
    }, 1000);
    // 清理函数：组件卸载或更新时清除定时器
    return () => clearInterval(interval);
  }, []);  // 依赖列表为空，表示只在组件首次挂载时执行一次

  return <h2>计时：{seconds}s</h2>;
}
```

上述效果类似 Vue 的 `mounted` 钩子里启动一个定时器，以及 `beforeUnmount` 钩子里清除它。在 React 中，`useEffect` 接收两个参数：一个函数用于定义副作用逻辑，可以返回一个可选的清理函数；第二个参数是依赖数组，用于控制该副作用函数的触发时机。依赖数组类似于 Vue 3 Composition API 中 `watch` 的监听依赖 —— 只有当数组中的状态发生变化时，副作用才执行。如果依赖数组为空，则副作用只在组件初次渲染后执行一次（相当于 **mounted**）；如果没有提供依赖数组，则每次渲染都会执行（相当于每次 **updated** 都运行）；如果依赖了某些状态变量，则相当于 **watch** 那些变量的变化。[segmentfault.com](https://segmentfault.com/a/1190000044882791#:~:text=CSR%EF%BC%88Client) 通过清理函数，我们可以在组件卸载或副作用重新运行前执行清理操作（类似 **beforeUnmount/ unmounted**）。总的来说，`useEffect` 将组件的生命周期钩子（挂载、更新、卸载）统一到了一个API中，通过依赖声明来控制逻辑执行时机。

**Vue vs React 生命周期对比：**Vue 2 提供了 `mounted/updated/destroyed` 等钩子，Vue 3 Composition API 则有 `onMounted/onUpdated/onUnmounted` 以及更灵活的 `watchEffect` 等。React Hooks 模式下没有传统的生命周期钩子名称，而是一种**声明式副作用**的方式：用 `useEffect` 代替多个生命周期，根据需要设置依赖或清理即可。这对于 Vue 开发者而言是需要转变思路的地方，但理解之后会发现二者目标一致：都是在适当的时机执行副作用逻辑，只是 React 将其融合到 Hook 的机制中。

**DOM 更新机制：**React 和 Vue 都采用**虚拟DOM**来优化界面更新，但实现策略略有不同[blog.csdn.net](https://blog.csdn.net/corruptwww/article/details/129134926#:~:text=react%E5%92%8Cvue%E7%9A%84%E7%BB%84%E4%BB%B6%E5%86%85%E9%83%A8%E7%9A%84%E6%95%B0%E6%8D%AE%E5%8F%98%E5%8C%96%EF%BC%8C%E9%80%9A%E8%BF%87%E8%99%9A%E6%8B%9FDOM%E5%8E%BB%E6%9B%B4%E6%96%B0%E9%A1%B5%E9%9D%A2%E3%80%82%E8%80%8C%E4%B8%A4%E8%80%85%E7%9A%84%E8%99%9A%E6%8B%9Fdom%EF%BC%8C%E9%99%A4%E4%BA%86%E6%9B%B4%E6%96%B0%E7%AD%96%E7%95%A5%E4%B8%8D%E4%B8%80%E6%A0%B7%E5%A4%96%EF%BC%8C%E5%85%B6%E4%BB%96%E6%B2%A1%E6%9C%89%E5%A4%A7%E5%8C%BA%E5%88%AB%E3%80%82%20%E5%9C%A8%E6%9B%B4%E6%96%B0%E7%AD%96%E7%95%A5%E4%B8%8A%EF%BC%8Creact%E9%87%87%E7%94%A8%E8%87%AA%E9%A1%B6%E5%90%91%E4%B8%8B%E7%9A%84%E5%85%A8%E9%87%8Fdiff%EF%BC%8Cvue%E6%98%AF%E5%B1%80%E9%83%A8%E8%AE%A2%E9%98%85%E7%9A%84%E6%A8%A1%E5%BC%8F%E3%80%82%20react%E5%92%8Cvue%E4%B8%80%E6%A0%B7%EF%BC%8C%E5%8F%AA%E8%A7%A3%E5%86%B3%E5%90%8C%E5%B1%82%E7%9A%84diff%EF%BC%8C%E4%B8%8D%E8%A7%A3%E5%86%B3%E5%A6%82p%E6%A0%87%E7%AD%BE%E5%92%8Cdiv%E6%A0%87%E7%AD%BE%E7%AD%89%E8%B7%A8%E5%B1%82diff%E3%80%82%20%E5%85%B7%E4%BD%93%E6%96%B9%E6%B3%95%E5%8F%82%E8%80%83%EF%BC%9A%E6%B5%85%E6%9E%90react%E5%92%8Cvue%E4%B8%AD%E7%9A%84diff%E7%AD%96%E7%95%A5)。React 每当状态更新时，会重新执行组件函数，生成新的 Virtual DOM，然后和上一次的 Virtual DOM 树进行**全量diff**比较，找出差异并更新真实 DOM。这个过程对开发者透明，但需要注意频繁或不必要的重新渲染。Vue 的响应式系统则更细粒度：它追踪具体数据依赖，触发变化时只通知受影响的组件或DOM节点更新（所谓“局部订阅”）。因此在Vue中，如果一个组件的状态没变，组件内部不会重新渲染；而 React 默认会自顶向下重新渲染子组件树（除非使用 `memo` 等优化）。两者各有权衡：React的全量diff实现简单且配合不可变数据可以精确判断变化；Vue的依赖追踪减少无关组件更新但实现更复杂。实际性能都足够优秀，但作为开发者需要理解React每次状态更新都会走一次渲染/diff流程，这也是为什么要避免不必要的状态改变和组件重新渲染。在具体编码中，React 提供了优化手段（如 `React.memo`、`useMemo`、`useCallback` 等）来避免重复渲染，Vue 则通过计算属性和缓存等机制优化——这些深入内容可在后续进阶学习中了解。

**总结：**React 的状态管理通过 Hooks 明确地由开发者触发更新，这要求您遵循一些模式但换来更明确的状态流；Vue 的响应式相对自动，写起来方便但有时不易察觉背后发生的更新逻辑。对于 Vue 开发者，适应 React 时要牢记**不能直接修改状态值**以及**使用 Hooks 管理副作用**。掌握了 `useState` 和 `useEffect` 后，您就具备了处理组件大多数交互逻辑的基础。在更复杂的场景下，React 还有其它Hooks（例如 `useContext` 全局状态，`useReducer` 复杂状态逻辑，`useRef` 获取DOM或保存可变变量等）以及像 Redux、Zustand 这样的状态管理库，可以类比于 Vue 的 Pinia/Vuex 等。随着 Hooks 思想的普及，函数式组件已经完全可以胜任以前类组件的所有功能。

5\. CSS 样式与模块化设计
----------------

良好的样式组织对前端项目非常重要。Next.js 对 CSS 提供了多种支持方式，包括全局CSS、CSS模块、CSS-in-JS，以及与常用CSS框架（如 Tailwind CSS）的集成。在 Vue 单文件组件中，样式通常写在 `<style>` 标签内（可选择 scoped 或模块化）。在 Next.js 中，我们需要根据情况选择合适的方式来管理样式。

**全局样式：**可以使用全局 CSS 文件来应用于整个应用的样式，例如基础的页面Reset、字体、颜色等通用样式。在 Next.js App Router 下，全局样式文件通常是 `app/globals.css`（创建项目时已生成）。我们需在根布局文件中引入它才能生效runoob.com。例如在 `app/layout.tsx` 顶部：`import './globals.css';`。全局样式会影响所有页面，但要谨慎使用，**Next.js 官方建议仅将真正全局通用的样式放入全局CSS，而局部样式使用 CSS 模块等机制，以防样式冲突和提升性能**runoob.com。在Vue里，全局样式一般放在主入口或`App.vue`中也是类似的概念。

**CSS Modules（CSS 模块化）：**Next.js 内置支持 CSS Modules。只需将样式文件命名为 `*.module.css`（或 `.module.scss` 等），然后在组件中通过 `import styles from '.../xxx.module.css';` 导入，即可获得一个样式对象。这个对象的属性对应 CSS 文件中定义的类名，值是经过哈希处理后的类名，保证作用域仅限于该模块，不会与其他样式冲突。这有点类似于 Vue `<style scoped>`，但作用更强：不仅是添加属性选择器范围，而是完全哈希隔离。示例：

首先创建一个模块化CSS文件，如 `app/components/Profile.module.css`：

```css
/* Profile.module.css */
.card {
  padding: 1rem;
  border: 1px solid #ccc;
  border-radius: 8px;
}
.title {
  font-size: 1.2rem;
  color: purple;
}
```

然后在组件中使用这些样式：

```tsx
// app/components/ProfileCard.tsx
import styles from './Profile.module.css';

interface ProfileCardProps {
  title: string;
  children: React.ReactNode;
}
export default function ProfileCard({ title, children }: ProfileCardProps) {
  return (
    <div className={styles.card}>
      <h3 className={styles.title}>{title}</h3>
      <div>{children}</div>
    </div>
  );
}
```

这里 `styles.card` 和 `styles.title` 引用了模块CSS中的类名。渲染结果中的实际 class 会被Next.js自动转换成类似于 `Profile_card__abc123` 这样的独一无二名称，不用担心与别处冲突。这样，我们就实现了组件级别的样式封装，类似 Vue SFC 的 scoped 样式但更直观。**注意：**CSS Modules 不支持全局标签选择器或元素选择器，因为它旨在模块内使用。如果需要为 `body` 等应用全局样式，还是要放在全局CSS中。

**CSS-in-JS:** Next.js 也支持 CSS-in-JS 方案，如 styled-jsx（开箱即用）或 styled-components、Emotion 等库。styled-jsx 是 Next.js 默认支持的，您可以在 JSX 中写 `<style jsx>` 标签，其内容会限定作用于当前组件。示例：

```jsx
<div className="box">内容</div>
<style jsx>{`
  .box { background: lightblue; padding: 10px; }
`}</style>
```

这种写法直接在组件文件中书写样式，Next.js 会在编译时将其处理为独立的CSS并作用于组件。同样地，styled-components 等库提供更强大的CSS语法支持。CSS-in-JS 在React社区很常见，但相对会增加构建复杂度。对于简单项目，CSS Modules 已足够；大型项目可能会考虑CSS-in-JS做动态样式处理等。Vue 中也有类似 CSS-in-JS 的探索，但使用不如 React 生态广泛。

**Tailwind CSS 简介：**Tailwind CSS 是当下非常流行的一个**原子化CSS框架**。它提供大量**实用工具类（utility classes）**，开发者可以直接在HTML（JSX）元素的 `className` 上组合这些类来实现所需样式，而不必每写一处样式就新建CSS规则runoob.com。Tailwind 提倡的“工具优先”方法，使得开发速度快且样式一致性高。例如，想让一个按钮有蓝色背景白色文字和圆角，在 Tailwind 中可以直接写：`<button className="bg-blue-500 text-white rounded">按钮</button>`，无需编写自定义 CSS。Tailwind 在 Next.js 中集成也很方便。使用 `create-next-app` 时如果选择了 Tailwind，脚手架会自动安装和配置好；否则也可以手动安装：

```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

上面命令会生成 `tailwind.config.js`（或 `.ts`）和 `postcss.config.js` 文件。接着在全局CSS中加入 Tailwind 的指令runoob.com：

```css
/* globals.css */
@tailwind base;
@tailwind components;
@tailwind utilities;
```

并确保在 `app/layout.tsx` 中引入了这个全局CSS（这一点在前面全局样式部分已经说明）runoob.com。配置好后，即可在 JSX 中使用 Tailwind 提供的实用类名。例如：

```tsx
<div className="min-h-screen bg-gray-100 flex items-center justify-center">
  <div className="bg-white p-6 rounded-lg shadow">
    <h1 className="text-2xl font-bold text-gray-900">Hello, Next.js + Tailwind!</h1>
    <p className="mt-4 text-gray-600">这是一段示例文本。</p>
  </div>
</div>
```

上面这一段 JSX 利用了一系列 Tailwind 类：如 `min-h-screen` 设置全屏高度，`bg-gray-100` 背景灰色，`flex items-center justify-center` 将内部元素居中，等等。无需写任何自定义 CSS，就构建出了常见的居中卡片布局。Tailwind 的优势在于**开发体验**：即使不记忆具体数值，也可以通过组合语义化的类快速实现设计。例如 `p-6` 就表示 padding 1.5rem，`text-2xl` 表示字号，类名基本对应设计含义。对于习惯了 Vue + element-UI 或其他UI库的开发者，Tailwind 提供的是更灵活的自定义设计能力。

需要注意，Tailwind 虽然类很多，但通过配置 Purge（Next.js 已默认支持优化）会在构建时清理未用到的类，因此最终CSS体积并不会过大。Tailwind 也支持定制化主题，对设计系统友好。总之，在 Next.js 中使用 Tailwind 非常普遍，您可以根据需求选择是否引入。即使不用 Tailwind，Next.js 也完全支持手写 CSS 或使用其他框架，这方面与 Vue 没有本质区别。

6\. 网页渲染原理简述（SSR、CSR、SSG）
-------------------------

最后，我们来了解一些 Next.js 背后的**网页渲染原理**和渲染模式概念。这不仅是 Next.js，也是所有同构框架（如 Nuxt.js）涉及的核心思想。

现代 Web 应用主要有以下几种渲染方式：

*   **CSR（Client-Side Rendering，客户端渲染）：**由浏览器在客户端完成所有渲染工作。初始加载时服务器只返回一个基本的 HTML 框架和脚本引用，浏览器下载 JS 后由 JS 运行生成动态内容。优点是前端可以实现丰富的交互，初始部署简单；缺点是首次加载需要等待 JS 执行，首屏渲染较慢，而且对SEO不友好（爬虫拿到的初始HTML是空的，需要特殊处理）[segmentfault.com](https://segmentfault.com/a/1190000044882791#:~:text=CSR%EF%BC%88Client)[segmentfault.com](https://segmentfault.com/a/1190000044882791#:~:text=%E7%BC%BA%E7%82%B9%EF%BC%9A)。传统的 Vue SPA 或 React CRA 应用都是 CSR 模式。用户打开页面时会经历一个“白屏”阶段，然后才看到由JS填充的内容。
*   **SSR（Server-Side Rendering，服务端渲染）：**由服务器生成完整的 HTML 页面内容再发送给客户端。浏览器接收到的是已经渲染好的HTML，可以立即显示完整页面[segmentfault.com](https://segmentfault.com/a/1190000044882791#:~:text=SSR%EF%BC%88Server)。这样**首屏加载非常快**，用户体验好，同时由于有内容的HTML直接提供，**SEO 友好**[blog.csdn.net](https://blog.csdn.net/corruptwww/article/details/129134926#:~:text=SSR%E7%9A%84%E7%89%B9%E7%82%B9%EF%BC%8C%E4%BD%BF%E5%BE%97%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F%E9%9D%9E%E5%B8%B8%E9%80%82%E5%90%88%E6%90%9C%E7%B4%A2%E5%BC%95%E6%93%8E%E7%88%AC%E5%8F%96%E6%95%B0%E6%8D%AE%EF%BC%8C%E6%8F%90%E9%AB%98%E7%BD%91%E7%AB%99%E6%90%9C%E7%B4%A2%E6%9D%83%E9%87%8D%EF%BC%8C%E8%BF%99%E4%B9%9F%E6%98%AF%E5%A4%A7%E9%83%A8%E5%88%86%E4%BA%BA%E9%80%89%E6%8B%A9%E5%AE%83%E7%9A%84%E5%8E%9F%E5%9B%A0%E3%80%82)。Next.js 正是提供了对 React 的SSR支持，使我们可以用类似传统服务端模板的方式来预渲染页面。不过SSR也有缺点：每次请求都要服务器现算页面，服务器压力增大，而且开发时需要处理浏览器与服务器环境差异（例如访问 `window` 对象需要小心）。Next.js 通过自动同构（hydrate）技术，结合React的虚拟DOM，在服务器渲染HTML后，再在前端“接管”绑定事件，使页面在**SSR首屏显示**和**CSR后续交互**之间无缝衔接。简单来说，SSR+CSR结合提供了更好的性能与SEO，但实现上比纯CSR更复杂。
*   **SSG（Static Site Generation，静态站点生成）：**在**构建阶段**预先将页面渲染为静态文件。也就是根据模板和数据一次性生成静态 HTML，在部署时直接提供给客户端[segmentfault.com](https://segmentfault.com/a/1190000044882791#:~:text=%E4%BB%8B%E7%BB%8D%EF%BC%9A)。这类似于我们使用 Vue/React 进行静态编译、或用 Hugo 等静态站点生成器构建网站。SSG 的好处是**性能卓越**（因为不用每次请求都渲染，在请求时直接读取静态文件很快），非常适合内容相对固定的网站，比如博客、文档站点[segmentfault.com](https://segmentfault.com/a/1190000044882791#:~:text=%E4%BC%98%E7%82%B9%EF%BC%9A)。安全性也高，因为服务器不用运行太多逻辑。但缺点是对于需要频繁更新的数据不适用，每次内容更新都得重新构建部署。Next.js 支持 SSG，通过在页面组件中使用 `getStaticProps` 等方法可以在构建时获取数据生成静态页面。在 Next.js 13+ App Router 中，则通过**文件系统缓存**和 `fetch` 的选项实现同样的效果。总之，SSG 模式在Next.js中使用起来也很方便。

> 事实上，Next.js 支持在**同一个项目**中针对不同页面选择不同的渲染模式：例如某些营销页面用 SSG，博客文章列表用 ISR（增量静态再生，一种SSG增强模式），用户个性化页面用 SSR，等等[blog.csdn.net](https://blog.csdn.net/corruptwww/article/details/129134926#:~:text=match%20at%20L362%20Nuxt%20,js%20%E3%80%82%E4%B8%A4%E8%80%85%E9%83%BD%E4%BB%A3%E8%A1%A8%E4%BA%86%E7%8E%B0%E4%BB%A3%E5%85%A8%E6%A0%88%E5%BC%80%E5%8F%91%E8%B6%8B%E5%8A%BF%EF%BC%8C%E5%80%BC%E5%BE%97%E6%B7%B1%E5%85%A5%E5%AD%A6%E4%B9%A0%E3%80%82)。Next.js 提供了**混合渲染**的能力，非常灵活。这一点比传统 SPA 更先进，类似的在 Vue 生态里 Nuxt.js 也提供了 SSR/SSG 切换的能力。对于我们开发者来说，理解这些概念有助于做出正确的架构决策。

**小结：**本节概念较多，但核心在于理解：Next.js 让 React 应用具备了服务端渲染和预渲染的能力，从而兼顾了**首屏性能**和**SEO**。相比之下，纯 Vue 项目如果需要 SSR 则通常借助 Nuxt.js 实现。Next.js 和 Nuxt.js 都是各自生态的优秀框架，目的都是**提高性能和开发效率**，利用 SSR/SSG 提升用户体验[blog.csdn.net](https://blog.csdn.net/corruptwww/article/details/129134926#:~:text=match%20at%20L362%20Nuxt%20,js%20%E3%80%82%E4%B8%A4%E8%80%85%E9%83%BD%E4%BB%A3%E8%A1%A8%E4%BA%86%E7%8E%B0%E4%BB%A3%E5%85%A8%E6%A0%88%E5%BC%80%E5%8F%91%E8%B6%8B%E5%8A%BF%EF%BC%8C%E5%80%BC%E5%BE%97%E6%B7%B1%E5%85%A5%E5%AD%A6%E4%B9%A0%E3%80%82)。掌握了 Next.js 的这些基础模块后，您可以进一步学习数据获取（如 Next.js 的 `getServerSideProps`, `getStaticProps` 或新版 App Router 中直接在组件内获取数据的方式）、API 路由、鉴权、中间件等高级话题。

希望通过本教程，您这个熟悉 Vue 的开发者已经能够以对比的方式理解 Next.js 的用法。从项目初始化、页面路由，到组件封装、状态管理、样式方案，再到底层渲染机制，我们进行了全方位的讲解。接下来，您可以尝试亲手用 Next.js 实现一个小项目，在实践中巩固所学知识。相信很快，您就能像使用 Vue 一样游刃有余地使用 Next.js 构建出色的 Web 应用！[blog.csdn.net](https://blog.csdn.net/corruptwww/article/details/129134926#:~:text=match%20at%20L362%20Nuxt%20,js%20%E3%80%82%E4%B8%A4%E8%80%85%E9%83%BD%E4%BB%A3%E8%A1%A8%E4%BA%86%E7%8E%B0%E4%BB%A3%E5%85%A8%E6%A0%88%E5%BC%80%E5%8F%91%E8%B6%8B%E5%8A%BF%EF%BC%8C%E5%80%BC%E5%BE%97%E6%B7%B1%E5%85%A5%E5%AD%A6%E4%B9%A0%E3%80%82)

**参考资料：**

1.  Next.js 官方文档（中文翻译版）runoob.com[nextjs.freeourdays.com](https://nextjs.freeourdays.com/nextjs14/routing/layouts-and-templates/#:~:text=%E5%B8%83%E5%B1%80%E6%98%AF%E5%9C%A8%E5%A4%9A%E4%B8%AA%E8%B7%AF%E7%94%B1%E4%B9%8B%E9%97%B4%20%E5%85%B1%E4%BA%AB%20%E7%9A%84UI%E3%80%82%E5%9C%A8%E5%AF%BC%E8%88%AA%E6%97%B6%EF%BC%8C%E5%B8%83%E5%B1%80%E4%BF%9D%E6%8C%81%E7%8A%B6%E6%80%81%EF%BC%8C%E4%BF%9D%E6%8C%81%E4%BA%A4%E4%BA%92%E6%80%A7%EF%BC%8C%E5%B9%B6%E4%B8%94%E4%B8%8D%E4%BC%9A%E9%87%8D%E6%96%B0%E6%B8%B2%E6%9F%93%E3%80%82%E5%B8%83%E5%B1%80%E8%BF%98%E5%8F%AF%E4%BB%A5%20%E5%B5%8C%E5%A5%97%E3%80%82)等
2.  CSDN 博客: _《vue和react对比，next.js的优劣》_[blog.csdn.net](https://blog.csdn.net/corruptwww/article/details/129134926#:~:text=%E7%BB%84%E4%BB%B6%20%E7%BB%84%E5%90%88%E5%BC%8FAPI%20%E5%87%BD%E6%95%B0%E5%BC%8F%E7%BB%84%E4%BB%B6%20%E5%85%A8%E5%B1%80%E7%BB%84%E4%BB%B6%E9%80%9A%E4%BF%A1%20emit%E6%80%BB%E7%BA%BF,model%20%E5%8D%95%E5%90%91%E7%BB%91%E5%AE%9AuseEffect%E3%80%81%E4%B8%8D%E6%B8%B2%E6%9F%93%E5%80%BCuseRef%20%E5%93%8D%E5%BA%94%E5%BC%8F%E5%8E%9F%E7%90%86%E5%8F%8A%E6%9B%B4%E6%96%B0%E7%AD%96%E7%95%A5%20%E8%99%9A%E6%8B%9Fdom%2B%E5%B1%80%E9%83%A8%E8%AE%A2%E9%98%85%20%E8%99%9A%E6%8B%9Fdom%2B%E5%85%A8%E9%87%8Fdiff)[blog.csdn.net](https://blog.csdn.net/corruptwww/article/details/129134926#:~:text=react%E5%92%8Cvue%E7%9A%84%E7%BB%84%E4%BB%B6%E5%86%85%E9%83%A8%E7%9A%84%E6%95%B0%E6%8D%AE%E5%8F%98%E5%8C%96%EF%BC%8C%E9%80%9A%E8%BF%87%E8%99%9A%E6%8B%9FDOM%E5%8E%BB%E6%9B%B4%E6%96%B0%E9%A1%B5%E9%9D%A2%E3%80%82%E8%80%8C%E4%B8%A4%E8%80%85%E7%9A%84%E8%99%9A%E6%8B%9Fdom%EF%BC%8C%E9%99%A4%E4%BA%86%E6%9B%B4%E6%96%B0%E7%AD%96%E7%95%A5%E4%B8%8D%E4%B8%80%E6%A0%B7%E5%A4%96%EF%BC%8C%E5%85%B6%E4%BB%96%E6%B2%A1%E6%9C%89%E5%A4%A7%E5%8C%BA%E5%88%AB%E3%80%82%20%E5%9C%A8%E6%9B%B4%E6%96%B0%E7%AD%96%E7%95%A5%E4%B8%8A%EF%BC%8Creact%E9%87%87%E7%94%A8%E8%87%AA%E9%A1%B6%E5%90%91%E4%B8%8B%E7%9A%84%E5%85%A8%E9%87%8Fdiff%EF%BC%8Cvue%E6%98%AF%E5%B1%80%E9%83%A8%E8%AE%A2%E9%98%85%E7%9A%84%E6%A8%A1%E5%BC%8F%E3%80%82%20react%E5%92%8Cvue%E4%B8%80%E6%A0%B7%EF%BC%8C%E5%8F%AA%E8%A7%A3%E5%86%B3%E5%90%8C%E5%B1%82%E7%9A%84diff%EF%BC%8C%E4%B8%8D%E8%A7%A3%E5%86%B3%E5%A6%82p%E6%A0%87%E7%AD%BE%E5%92%8Cdiv%E6%A0%87%E7%AD%BE%E7%AD%89%E8%B7%A8%E5%B1%82diff%E3%80%82%20%E5%85%B7%E4%BD%93%E6%96%B9%E6%B3%95%E5%8F%82%E8%80%83%EF%BC%9A%E6%B5%85%E6%9E%90react%E5%92%8Cvue%E4%B8%AD%E7%9A%84diff%E7%AD%96%E7%95%A5)
3.  SegmentFault 社区文章: _《一文搞懂SSR、SSG、CSR》_[segmentfault.com](https://segmentfault.com/a/1190000044882791#:~:text=CSR%EF%BC%88Client)[segmentfault.com](https://segmentfault.com/a/1190000044882791#:~:text=SSR%EF%BC%88Server)[segmentfault.com](https://segmentfault.com/a/1190000044882791#:~:text=%E4%BB%8B%E7%BB%8D%EF%BC%9A)
4.  菜鸟教程 Next.js 教程runoob.comrunoob.com 等
5.  _Vue 与 React 响应式机制差异_blog.3vyd.com[blog.csdn.net](https://blog.csdn.net/corruptwww/article/details/129134926#:~:text=) (博客园 / 掘金)
6.  Next.js 框架对比 Nuxt.js 总结[blog.csdn.net](https://blog.csdn.net/corruptwww/article/details/129134926#:~:text=match%20at%20L362%20Nuxt%20,js%20%E3%80%82%E4%B8%A4%E8%80%85%E9%83%BD%E4%BB%A3%E8%A1%A8%E4%BA%86%E7%8E%B0%E4%BB%A3%E5%85%A8%E6%A0%88%E5%BC%80%E5%8F%91%E8%B6%8B%E5%8A%BF%EF%BC%8C%E5%80%BC%E5%BE%97%E6%B7%B1%E5%85%A5%E5%AD%A6%E4%B9%A0%E3%80%82)

