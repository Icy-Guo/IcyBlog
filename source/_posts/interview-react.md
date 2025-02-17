---
title: 前端面试题准备 - React
date: 2025-02-14 10:57:56
tags:
  - Interview
  - Frontend
  - React
categories:
  - Interview
top_img: /img/interview.jpg
cover: /img/interview.jpg
---

## 什么是 React？

React 是一个简单的 javascript UI 库，用于构建高效、快速的用户界面。它是一个轻量级库，因此很受欢迎。它遵循组件设计模式、声明式编程范式和函数式编程概念，以使前端应用程序更高效。它使用虚拟 DOM 来有效地操作 DOM。它遵循从高阶组件到低阶组件的单向数据流。

## React 生命周期

React 组件的生命周期可以分为三个阶段：

1. **挂载阶段（Mounting）**

当 React 首次渲染元件，并把渲染后的节点加入 DOM 中时，我们称这时为 mounting。当 React 把节点加入到 DOM 后，浏览器会渲染并绘制画面。在这个阶段，生命周期将会依照下列的顺序呼叫这些方法：

- `constructor(props)`
- `getDerivedStateFromProps(props, state)`
- `render()`
- `componentDidMount()`

2. **更新阶段（Updating）**

在 mounting 后，如果一个元件的 prop 或 state 有变化时，就会触发重新渲染。重新渲染后，React 会去比较虚拟 DOM 有哪些地方改变了，然后将改变的部分更新到实际的 DOM 上。这个阶段我们称它为 updating。当一个 component 被重新渲染时，其生命周期将会依照下列的顺序呼叫这些方法：

- `getDerivedStateFromProps(props, state)`
- `shouldComponentUpdate(nextProps, nextState)`
- `render()`
- `getSnapshotBeforeUpdate(prevProps, prevState)`
- `componentDidUpdate(prevProps, prevState, snapshot)`

3. **卸载阶段（Unmounting）**

当一个元件被从 DOM 中移除时，我们称之为 unmounting。这时将会呼叫：

- `componentWillUnmount()`

## React 中发起网络请求应该在哪个生命周期中进行？为什么？

在 `componentDidMount` 中发起网络请求。因为 `componentDidMount` 是在组件挂载到 DOM 后执行的，所以在这个生命周期中发起网络请求可以确保在组件挂载后执行。

## state 和 props 触发更新的生命周期分别？

`state` 和 `props` 触发更新的生命周期相同。但是 `getDerivedStateFromProps`、`shouldComponentUpdate`、`getSnapshotBeforeUpdate` 这几个周期中 如果是更新 `state`，参数 `prevState` 会有值，`nextProps` 是一个空对象； 如果是更新 `props`，参数 `nextProps` 会有值，`prevState` 是一个空对象。

## 虚拟 DOM 的原理是什么？

虚拟 DOM（Virtual DOM，简称 VDOM）是一种 **轻量级的 JavaScript 对象**，用于模拟真实 DOM 结构。它是 React、Vue 等前端框架中用于 **优化页面渲染性能** 的关键技术。

当页面状态（state）发生变化时，前端框架不会直接修改真实 DOM，而是先在虚拟 DOM 中进行计算，然后通过 **Diff 算法** 找到最小变更，并批量更新真实 DOM，提高性能。

更新过程一般分为 3 个阶段：

1. **创建虚拟 DOM（VNode）**

组件初次渲染时，会生成对应的虚拟 DOM 树（VDOM）。
这棵 VDOM 仅仅是一个 **JavaScript 对象**，不会影响真实 DOM。

2. **Diff 计算（对比新旧 VDOM）**

当组件的 **state 或 props 发生变化**，框架会重新生成新的虚拟 DOM。
通过 **Diff 算法**，比较新旧虚拟 DOM，找出变化的部分。

3. **更新真实 DOM（Reconciliation 过程）**

根据 Diff 计算结果，框架 **最小化真实 DOM 的变更**，只更新必要部分。
使用 **批量更新**（如 React 的 Fiber 机制）提高性能。

虚拟 DOM 的优缺点：

优点：

- 提升性能：通过 **Diff 算法** 计算最小更新范围，减少不必要的 DOM 操作，避免频繁重绘和重排（Reflow & Repaint）。React 采用 **批量更新** 和 **Fiber 架构**，进一步优化渲染。
- 跨平台渲染：由于 VDOM 只是一个 **JavaScript 对象**，它不仅能映射到浏览器 DOM，还能用于 React Native（移动端）、Server-Side Rendering（SSR）等。
- 代码简洁、易维护：通过 **声明式 UI（Declarative UI）**，开发者只需关注 **数据状态**，而不必手动操作 DOM，提高开发效率。

缺点：

- 初次渲染比原生 DOM 慢：由于需要 **创建虚拟 DOM** 并 **计算 Diff**，对于静态页面（如普通 HTML）可能反而带来额外开销。
- Diff 计算的性能开销：Diff 算法虽然高效，但仍然需要一定的 CPU 计算，如果状态频繁变更，可能影响性能（如复杂的动画、大量节点更新）。

## 什么是 JSX？

`JSX（JavaScript XML）` 是一种 JavaScript 语法扩展，用于在 React 中编写类似 HTML 的代码。它允许开发者在 JavaScript 代码中直接编写 UI 结构，提高可读性和开发效率。

`JSX` 不会直接被浏览器执行，它需要通过 Babel 转换为标准 JavaScript，本质上是 `React.createElement()` 的语法糖。

## React 组件的通信方式

1. **父组件向子组件通信**

父组件通过 `props` 向子组件传递数据。

2. **子组件向父组件通信**

子组件通过 `回调函数` 向父组件传递数据。

3. **跨级组件通信**

- `props` 传递：可能会出现多个层级，需要逐层传递，增加了复杂度。很可能这些 `props` 并不是中间组件需要的，只是为了传递给子组件。
- `context` 传递：`context` 相当是一个大容器，可以把要通信的内容放在这个容器中，不管嵌套多深，都可使用。对于跨域多层的全局数据可以使用 `context` 实现。

4. **非嵌套组件通信**

- 发布订阅：创建一个全局的 `event bus`(事件总线)，然后通过 `on` 和 `emit` 来传递消息。
- `redux` 等全局状态管理工具：通过 `Redux`、`Zustand` 等全局状态管理工具来传递消息。

## 为什么 React 渲染列表时需要加上 key？

在 React 中，渲染列表时需要为每个元素提供唯一的 `key`，其主要作用是 **优化性能、减少不必要的 DOM 操作，并确保 React 正确识别组件的变化**。

React 使用 `Diff 算法` 来比较 新旧虚拟 DOM（VDOM），找出需要更新的部分。如果没有 `key`，React 只能按元素索引 进行 Diff 计算。当列表项顺序发生变化（如插入、删除、排序），React 可能会 **错误更新或不必要地重新渲染**。

## props 和 state 的区别

`props` 是一个从外部传进组件的参数，主要作为就是从父组件向子组件传递数据，它具有可读性和不变性，只能通过外部组件主动传入新的 `props` 来重新渲染子组件，否则子组件的 `props` 以及展现形式不会改变。

`state` 的主要作用是用于组件保存、控制以及修改自己的状态，它只能在 `constructor` 中初始化，它算是组件的私有属性，不可通过外部访问和修改，只能通过组件内部的 `this.setState` 来修改，修改 `state` 属性会导致组件的重新渲染。

**区别：**

- `props` 是外部传入的，`state` 是组件内部维护的。
- `props` 具有可读性和不变性，`state` 具有可变性和可读性。
- `props` 主要用于组件之间传递数据，`state` 主要用于组件内部状态管理。

## React 单向数据流

单向数据流是指数据的流向只能由父组件通过 props 将数据传递给子组件，不能由子组件向父组件传递数据，要想实现数据的双向绑定，只能由子组件接收父组件 props 传过来的方法去改变父组件的数据，而不是直接将子组件的数据传递给父组件。

## 如何理解 setState

`setState` 是 React 中用于更新组件状态的函数。当 `setState` 被调用时，React 会根据新的状态触发重新渲染，以反映状态变化。

1. **异步更新**

`setState` 是异步的，它不会立即更新状态，而是将更新状态的操作放入队列中，等当前事件处理完成后再执行。这是为了优化性能，避免频繁的更新状态。

2. **合并更新**

`setState` 会将传入的部分状态与当前状态合并。这意味着它不会完全覆盖当前状态，而是仅更新改变的部分。

3. **批量更新**

React 会批量更新状态，尤其在事件处理和生命周期方法中，React 会将多个 `setState` 调用合并成一个更新，以提高性能。

```jsx
handleClick = () => {
  this.setState({ counter: this.state.counter + 1 });
  this.setState({ name: 'Alice' });
  // React 会将这两个 setState 合并成一次更新
}
```




