---
title: 前端面试题准备 - Webpack
date: 2025-02-16 21:40:53
tags:
  - Interview
  - Webpack
categories:
  - Interview
top_img: /img/interview.jpg
cover: /img/interview.jpg
---

## 什么是 Webpack？

在现代前端开发中，代码通常是由 **多个模块、各种格式的资源文件** （JavaScript、CSS、图片等）以及 **第三方库** 组成的。直接将这些文件部署到浏览器环境可能会导致 **加载速度慢、兼容性问题、安全风险** 等问题。因此，我们需要使用 `Webpack`、`Vite`、`Parcel` 等前端构建工具进行打包。

`Webpack` 是一个静态模块打包工具，主要用于将前端资源（如 JavaScript、CSS、图片等）打包成一个或多个静态文件。它将所有资源视为模块，并生成一个依赖图谱，用于描述各个模块之间的依赖关系。

**主要功能：**

- **模块化打包**：支持 CommonJS、ES6 Modules、AMD 等模块化规范。
- **代码拆分**：按需加载，提高页面性能。
- **资源管理**：可以处理 CSS、图片、字体等静态资源，支持 Loaders 进行转换。
- **插件机制**：提供强大的插件系统，如 UglifyJSPlugin（代码压缩）、HtmlWebpackPlugin（自动生成 HTML）等。
- **开发优化**：
  - 热模块替换（HMR, Hot Module Replacement）：代码修改后无需刷新整个页面。
  - 代码压缩（Tree Shaking）：移除未使用的代码，减少体积。

## Webpack 的核心模块

1. **入口（entry**: 项目打包从哪里开始，打包的起点
   
2. **出口（output）**: 输出打包的文件的位置，以及名称

3. **loader**: 
  - **转换文件格式**：将非 JavaScript 文件（如 CSS、图片、字体等）转换为 JavaScript 模块。
  - **预处理**：例如，编译 Sass/SCSS 到 CSS、TypeScript 到 JavaScript。
  - **文件优化**：如压缩、转译或其他优化。

4. **plugin**: 代码优化，资源管理

5. **模块(modules)**: 在模块化编程中，开发者将程序分解为功能离散的 `chunk`，并称之为模块。

## 常用的 loader 有哪些？

- `babel-loader`: 把 ES6 转换成 ES5
- `css-loader`: 加载 CSS，支持模块化、压缩、文件导入等特性
- `sass-loader`: 将 SCSS/SASS 代码转换成 CSS
- `ts-loader`: 将 TypeScript 转换成 JavaScript
- `awesome-typescript-loader`: 将 TypeScript 转换成 JavaScript，性能优于 ts-loader
- `style-loader`: 把 CSS 代码注入到 JavaScript 中，通过 DOM 操作去加载 CSS
- `file-loader`: 把文件输出到一个文件夹中，在代码中通过相对 URL 去引用输出的文件 (处理图片和字体)
- `url-loader`: 与 file-loader 类似，区别是用户可以设置一个阈值，大于阈值会交给 file-loader 处理，小于阈值时返回文件 base64 形式编码 (处理图片和字体)
- `eslint-loader`: 通过 ESLint 检查 JavaScript 代码是否符合编码规范和统一的代码风格；审查代码是否存在语法错误；
- `postcss-loader`: 扩展 CSS 语法，使用下一代 CSS，可以配合 autoprefixer 插件自动补齐 CSS3 前缀

## 常用的 plugin 有哪些？

- `html-webpack-plugin`: 用于自动生成 HTML 文件，并将打包后的资源（如 JavaScript 和 CSS 文件）注入到 HTML 中。
- `web-webpack-plugin`: 可方便地为单页应用输出 HTML，比 html-webpack-plugin 好用
- `uglifyjs-webpack-plugin`: 用于压缩 JavaScript 文件，减少文件体积，不支持 ES6 压缩 (Webpack4 以前)
- `terser-webpack-plugin`: 用于压缩 JavaScript 文件，减少文件体积，支持压缩 ES6 (Webpack4)
- `webpack-parallel-uglify-plugin`: 多进程执行代码压缩，提升构建速度
- `mini-css-extract-plugin`: 分离样式文件，CSS 提取为独立文件，支持按需加载 (替代 extract-text-webpack-plugin)
- `speed-measure-webpack-plugin`: 可以看到每个 Loader 和 Plugin 执行耗时 (整个打包耗时、每个 Plugin 和 Loader 耗时)
- `webpack-bundle-analyzer`: 可视化 Webpack 输出文件的体积 (业务组件、依赖第三方模块)

## loader 和 plugin 的区别？

- `loader` 本质上是一个函数，接受源文件作为参数，返回转换后的结果。它在 `module.rules` 中配置，作为模块的解析规则，类型为数组。每一项是一个对象，对象中包含 `test` （识别哪些文件会被转换）和 `use` （指定使用哪些 loader 进行转换）等属性。
- `plugin` 是插件，基于事件流框架 Tapable，插件可以扩展 Webpack 的功能，在 Webpack 运行的生命周期中会广播出许多事件，Plugin 可以监听这些事件，在合适的时机通过 Webpack 提供的 API 改变输出结果。它在 `plugins` 中单独配置，类型为数组，每一项是一个 Plugin 的实例，参数都通过构造函数传入。

## Webpack 打包构建流程

1. **初始化参数**：从配置文件和 `Shell` 语句中读取与合并参数，得出最终的参数
2. **开始编译**：用上一步得到的参数初始化 `Compiler` 对象，加载所有配置的插件，执行对象的 `run` 方法开始执行编译
3. **确定入口**：根据配置中的 `entry` 找出所有的入口文件
4. **编译模块**：从入口文件出发，调用所有配置的 `Loader` 对模块进行翻译，再找出该模块依赖的模块，再递归本步骤直到所有入口依赖的文件都经过了本步骤的处理
5. **完成模块编译**：在经过第4步使用 `Loader` 翻译完所有模块后，得到了每个模块被翻译后的最终内容以及它们之间的依赖关系
6. **输出资源**：根据入口和模块之间的依赖关系，组装成一个个包含多个模块的 `Chunk`，再把每个 `Chunk` 转换成一个单独的文件加入到输出列表，这步是可以修改输出内容的最后机会
7. **输出完成**：在确定好输出内容后，根据配置确定输出的路径和文件名，把文件内容写入到文件系统。

简单概括：
- **初始化**：启动构建，读取与合并配置参数，加载 `Plugin`，实例化 `Compiler`
- **编译**：从 `Entry` 出发，针对每个 `Module` 串行调用对应的 `Loader` 去翻译文件的内容，再找到该 `Module` 依赖的 `Module`，递归地进行编译处理
- **输出**：将编译后的 `Module` 组合成 `Chunk`，将 `Chunk` 转换成文件，输出到文件系统中

## Webpack 的热更新

Webpack 的 热更新（Hot Module Replacement，简称 HMR）是一个非常强大的功能，它允许在开发过程中只更新修改过的模块，而不是重新刷新整个页面。这可以大大提高开发效率，尤其是在开发大型应用时，能够加速调试和测试过程。

热更新的工作原理：
- 当修改代码并保存时，Webpack 会检测到更改，随后只重新编译和更新被更改的模块，而不是重新加载整个页面。
- 热更新不仅更新 JavaScript 代码，还可以支持 CSS、HTML、图片等资源的更新，通常配合 Webpack Dev Server 使用。
- 通过 WebSocket 或 XHR 与浏览器进行通信，通知浏览器哪些模块已经更新，浏览器只会替换这些模块，而不重新加载页面。

## 为什么要代码分割？

代码分割（Code Splitting）是 Webpack 提供的一种优化策略，目的是减少 JavaScript 的加载时间和执行时间，提高网页的加载性能。

解决以下问题：
- **首屏加载时间过长**：如果所有代码都打包在一个 bundle.js 文件中，用户需要等待整个文件加载完成后才能使用页面。
- **资源浪费**：如果某些代码（如管理后台相关的代码）用户在当前页面用不到，却仍然被加载，导致不必要的流量消耗。
- **缓存失效问题**：任何小的代码修改都会导致整个 bundle.js 变化，使得浏览器缓存失效，用户需要重新下载整个大文件

代码分割的本质是 **将应用的整体代码拆分成更小的、按需加载的模块，并根据用户的需求动态加载这些模块。**

## Webpack 打包优化方案

当项目越来越庞大时，Webpack 的打包速度会越来越慢，这时就需要进行打包优化。

1. 优化 loader 配置
- 缩小文件搜索范围，使用 include 和 exclude 指定或者排除需要 loader 搜索的路径
- 缓存 Babel 编译过的文件，下次只需要编译更改过的代码文件即可，加快打包时间

2. 对 Webpack 的 resolve 参数进行合理配置，使用 resolve 字段告诉 webpack 怎么去搜索文件。

- `resolve.extensions`: 导入语句没带文件后缀时，webpack 会自动带上后缀后去查找文件是否存在，查询的顺序是按照配置的 resolve.extensions 顺序从前到后查找。所以应该将出现频率高的后缀排在前面
- `resolve.alias`: 给导入路径取一个别名，能把原导入路径映射成一个新的导入路径

3. 压缩代码
- `webpack-paralle-uglify-plugin`: 多进程执行代码压缩，提升构建速度
- `uglifyjs-webpack-plugin`: 开启 parallel 参数 (不支持 ES6, webpack4 之前)
- `terser-webpack-plugin`: 开启 parallel 参数(支持 es6，webpack4)
- `mini-css-extract-plugin`: 提取 Chunk 中的 CSS 代码到单独文件，通过 css-loader 的 minimize 选项开启 cssnano 压缩 CSS。

4. `DllPlugin` 和 `DllReferencePlugin` 插件
- 使用 `DllPlugin` 对某些库（如 React、Vue 等）进行提前打包，避免每次打包都重复打包这些库，只有当类库更新版本才有需要重新打包，提高打包速度
- 使用 `DllReferencePlugin` 在打包时引用 DllPlugin 打包出的 manifest 文件，避免重复打包

5. `happyPack` 多进程打包
- 受限于 Node 是单线程运行的，所以 Webpack 在打包的过程中也是单线程的，特别是在执行 Loader 的时候，长时间编译的任务很多，这样就会导致等待的情况。
- `happyPack` 可以将 Loader 的同步执行转换为并行执行，提升打包速度




