---
title: 前端面试题准备 - CSS
date: 2025-02-17 21:59:49
tags:
  - Interview
  - Frontend
  - JavaScript
categories:
  - Interview
top_img: /img/interview.jpg
cover: /img/interview.jpg
---

## BFC 是什么？

BFC 是 **块级格式化上下文（Block Formatting Context）** 的缩写。它定义了一个独立的渲染区域，包含了元素及其子元素的布局规则，决定了这些元素如何与其他元素进行交互，特别是在垂直方向上。

**BFC 特点**：

- **独立的布局环境**：BFC 内部的元素布局不受外部元素的影响。
- **垂直方向上的布局规则**：BFC 会影响元素如何在垂直方向上与其他元素进行排列。比如，BFC 内部的子元素会垂直排列，而外部的元素不会影响到内部的元素。
- **清除浮动**：浮动元素脱离了正常的文档流，可能导致父容器高度塌陷。通过创建 BFC，父容器会自动“包裹”浮动的子元素，从而解决这个问题。

**BFC 的触发条件**：

- **浮动元素** (`float: left;` 或 `float: right;`)。
- **绝对定位的元素** (`position: absolute;` 或 `position: fixed;`)。
- **行内块元素** (`display: inline-block;`)。
- **块级元素** 设置了 `overflow` 值为 `hidden`、`auto` 或 `scroll`。
- **弹性布局容器** (`display: flex;` 或 `display: inline-flex;`)。
- **网格布局容器** (`display: grid;` 或 `display: inline-grid;`)。

**BFC 的应用**：

- **清除浮动**：由于浮动元素会脱离正常文档流并可能导致父容器的高度塌陷，我们可以通过让父元素创建一个 BFC 来解决这个问题。比如，给父元素加上 `overflow: hidden;` 来触发 BFC。
- **防止 margin 重叠**：正常情况下，垂直相邻的块级元素的 margin 会发生合并，导致它们之间的空间变小。但在 BFC 中，相邻元素的 margin 不会合并。
- **布局隔离**：BFC 可以用来创建布局隔离区域，例如通过在容器上使用 `float` 或 `position` 属性，来使容器内的元素独立布局而不受外部影响。



