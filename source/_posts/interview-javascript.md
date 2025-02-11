---
title: interview-javascript
date: 2025-02-11 23:23:40
tags:
  - Interview
  - JavaScript
categories:
  - Interview
top_img: /img/interview.jpg
cover: /img/interview.jpg
---

## JavaScript 数组常见的方法

```js
const arr = [1, 2, 3, 4, 5];

arr.push(); //添加元素到数组末尾,返回新数组的长度
arr.pop(); //删除数组最后一个元素,返回被删除的元素
arr.shift(); //删除数组第一个元素,返回被删除的元素
arr.unshift(); //添加元素到数组头部,返回新数组的长度
arr.reverse(); //反转数组,返回新数组
arr.every(); //判断数组中所有元素是否都满足某个条件,返回布尔值
arr.some(); //判断数组中是否存在满足某个条件的元素,返回布尔值
arr.forEach(); //遍历数组,没有返回值
arr.filter(); //过滤数组,返回新数组
arr.includes(); //判断数组中是否存在某个元素,返回布尔值
arr.map(); //映射数组,返回新数组
arr.reduce(); //累加数组,返回累加后的值
arr.indexOf(); //找索引
arr.lastIndexOf(); //索引正序，但是从后往前找
arr.findIndex(); //找索引
arr.find(); //找满足条件的元素
arr.join(); //默认以逗号隔开
arr.join(' '); //无缝链接 将数组元素拼接成字符串
arr.slice(1, 2); //截取数组的一部分，不包含头部，包含尾部，不会修改原数组
arr.splice(1, 4); //从索引1开始删除4个元素,第二个是要删除的长度，第三个往后是要添加的元素
arr.splice(2, 0, 'i'); //从索引2开始，删除0个，加入一个’i‘
arr.splice(3, 1, 'o', 'i'); //从索引3开始，删除1个，添加两个字符串。
arr.flat(); //数组降维 ，返回新数组
arr.flat(1); //降维一层
arr.flat(Infinity); //降维到没有数组为止
arr.entries(); //将数组返回一个对象，包含对象索引的键值对
```

## 同步和异步

1. **同步**：同步任务在主线程上排队执行，只有前一个任务执行完毕，才能执行后一个任务。

2. **异步**：异步任务不进入主线程，而是进入任务队列（Event Table），只有当任务队列通知主线程，某个异步任务可以执行了，该任务才会进入主线程执行。

异步的实现：

2.1 **回调函数（callback）**：将函数作为参数传递给另一个函数，在异步任务完成后执行。

示例：

```js
function asyncTask(callback) {
  setTimeout(() => {
    console.log("任务完成");
    callback(); // 执行回调
  }, 1000); // 模拟异步操作，1秒后执行
}

console.log("开始任务");
asyncTask(() => {
  console.log("回调执行");
});
console.log("任务继续");
```

输出结果：

```
开始任务
任务继续
任务完成
回调执行
```

2.2 **Promise**：`Promise` 是对异步操作的封装，它是一个对象，表示一个异步操作的最终完成（或失败）及其结果值。

示例：

```js
function asyncTask() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve("任务完成");
    }, 1000); // 模拟异步操作，1秒后成功
  });
}

console.log("开始任务");
asyncTask()
  .then(result => {
    console.log(result); // 处理成功
  })
  .catch(error => {
    console.log(error); // 处理错误
  });
console.log("任务继续");
```

输出结果：

```
开始任务
任务继续
任务完成
```

2.3 **async/await**：`async/await` 是一种基于 Promise 的语法糖，让异步代码看起来更像同步代码。`await` 会阻塞代码的执行，直到 Promise 被解决或拒绝。所以即使 Promise 是异步的，`await` 保证了异步代码的同步化。

示例：

```js
async function asyncTask() {
  const result = await new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve("任务完成");
    }, 1000);
  });
  console.log(result);
}

console.log("开始任务");
asyncTask(); // 异步调用
console.log("任务继续");
```

输出结果：

```
开始任务
任务继续
任务完成
```

## 微任务和宏任务

1. **微任务**：微任务是由 JavaScript 自身提供的异步任务，如 `Promise` 的 `.then()`、`.catch()`、`.finally()` 回调、`MutationObserver`（用于监听 DOM 变化）。

2. **宏任务**：宏任务是由宿主环境（如浏览器）提供的异步任务，如 `setTimeout`、`setInterval`、`I/O 操作（例如，读取文件、网络请求等）` 、`UI 渲染操作（例如，浏览器渲染、用户交互等）` 。

3. **事件循环与任务队列**：

JavaScript 是单线程的，但它通过事件循环机制（Event Loop）来处理异步任务。

**执行顺序**：

- 执行同步代码，将微任务和宏任务分别加入各自的队列，执行栈清空。
- 执行栈从微任务队列中取一个微任务放到执行栈中执行，若有新的微任务或宏任务产生，添加到相应的任务队列中，循环往复，直至微任务队列清空。
- 执行栈从宏任务队列中取一个宏任务放到执行栈中执行，若有新的微任务或宏任务产生，添加到相应的任务队列中。
- 这个宏任务执行完后会继续执行微任务队列，如果没有产生就继续执行下一个宏任务。
- 重复上述步骤，直到所有任务执行完毕

![event-loop](/img/event-loop.png)

## var、let、const的区别

1. **var**：

- **作用域**：`var` 声明的变量是 **函数作用域**（如果在函数内部声明）或 **全局作用域**（如果在函数外部声明）。它不受代码块（如 `if`、`for` 等）限制。
- **提升**：`var` 会被提升到作用域的顶部，但它的初始化赋值不会提升，只是声明提升。意味着变量会被提前声明，但其值仍会保持为 `undefined`，直到赋值操作执行。
- **可以重复声明**：在同一作用域内，`var` 允许重复声明同名变量。

示例：

```js
console.log(x);  // undefined，变量提升
var x = 5;

if (true) {
  var y = 10;  // `var` 只在函数作用域内有效，这里 y 会是全局变量
}
console.log(y);  // 10
```
2. **let**：

- **作用域**：`let` 声明的变量是块级作用域。它只在最近的代码块（如 `if`、`for` 循环等）中有效。
- **提升**：`let` 也会被提升到作用域顶部，但不会初始化，会进入一个被称为“暂时性死区”（Temporal Dead Zone，TDZ）的状态，直到变量的赋值操作发生之前不能访问。
- **不允许重复声明**：在同一作用域内，`let` 不允许重复声明同名变量。

示例：

```js
console.log(x);  // ReferenceError: x is not defined, TDZ（暂时性死区）
let x = 5;

if (true) {
  let y = 10;  // `let` 在代码块内有效，y 是局部变量
  console.log(y);  // 10
}
console.log(y);  // ReferenceError: y is not defined
```

3. **const**：

- **作用域**：`const` 具有块级作用域，和 `let` 一样。
- **提升**：`const` 同样会被提升，但和 `let` 一样，在赋值之前处于暂时性死区（TDZ），直到赋值发生之前不能访问。
- **常量**：声明时必须立即初始化，并且之后不能修改该变量的绑定。注意： 对象和数组的属性可以修改，但变量本身的绑定（引用）不能改变。
- **不允许重复声明**：在同一作用域内，和 `let` 一样，`const` 也不能重复声明同名变量。

示例：

```js
const x = 5;
console.log(x);  // 5

x = 10;  // TypeError: Assignment to constant variable.

const obj = { a: 1 };
obj.a = 2;  // 允许修改对象的属性
console.log(obj.a);  // 2

obj = { b: 3 };  // TypeError: Assignment to constant variable.
```

## 深拷贝和浅拷贝

1. **浅拷贝**：

浅拷贝创建一个新对象（或数组），并将原对象（或数组）的顶层属性复制到新对象中。对于嵌套的对象或数组，浅拷贝只是复制了引用，而不是复制整个对象。这意味着如果你修改嵌套对象中的值，原始对象和新对象中的值都会受到影响。

浅拷贝的实现：

- `Object.assign()`：用于对象的合并，将源对象的属性复制到目标对象。
- `Array.prototype.slice()`：用于数组的浅拷贝。
- `Array.prototype.concat()`：用于数组的浅拷贝。
- 扩展运算符 `...`：用于数组和对象的浅拷贝。


示例：

```js
const obj1 = { a: 1, b: { c: 2 } };
const obj2 = Object.assign({}, obj1);
obj2.a = 10;  // 修改 obj2 的顶层属性
obj2.b.c = 20;  // 修改 obj2 的嵌套对象

console.log(obj1);  // { a: 1, b: { c: 20 } }
console.log(obj2);  // { a: 10, b: { c: 20 } }
```

2. **深拷贝**：

深拷贝创建一个新对象（或数组），并且递归地复制所有嵌套的对象和数组。这样，新对象和原对象之间没有任何共享的引用，修改新对象的任何内容都不会影响原对象。

深拷贝的实现：

- `JSON.parse(JSON.stringify())`：用于对象的深拷贝。
- `lodash` 库的 `cloneDeep()` 方法：用于对象的深拷贝。

示例：

```js
const obj1 = { a: 1, b: { c: 2 } };

// 使用 JSON 方法进行深拷贝
const obj2 = JSON.parse(JSON.stringify(obj1));
obj2.a = 10;  // 修改 obj2 的顶层属性
obj2.b.c = 20;  // 修改 obj2 的嵌套对象

console.log(obj1);  // { a: 1, b: { c: 2 } }
console.log(obj2);  // { a: 10, b: { c: 20 } }
```

