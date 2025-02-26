---
title: 前端面试题准备 - JavaScript
date: 2025-02-11 23:23:40
tags:
  - Interview
  - Frontend
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
arr.splice(2, 0, 'i'); //从索引2开始，删除0个，加入一个'i'
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

## 闭包

闭包（`Closure`）是指函数能够访问并记住其词法作用域（`lexical scope`）外的变量，即使这个函数在其定义时的作用域之外执行。简单说，闭包是函数与其周围状态的组合。

示例：

```js
function createCounter() {
  let count = 0; // 闭包保护的私有变量
  return {
    increment: function() {
      count++;
      return count;
    },
    getValue: function() {
      return count;
    }
  };
}

const counter = createCounter();
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.getValue());  // 2
```

1. **闭包的特性**：
- **函数嵌套**：闭包通常由一个内部函数形成，该函数被定义在另一个函数内部。
- **访问外部作用域**：内部函数可以访问外部函数的变量，即使外部函数已经执行完毕。
- **变量持久化**：由于闭包使变量保存在内存中，不会被垃圾回收机制清除，因此它可以模拟私有变量。

2. **闭包的优点**：
- **延长变量的生命周期**：闭包可以访问函数内部的变量，即使函数执行完毕，这些变量仍然存在。
- **模拟私有变量**：闭包可以模拟私有变量，通过闭包保护的变量只能在函数内部访问，外部无法直接访问。

3. **闭包的缺点**：
- **内存泄漏**：闭包会占用内存，如果滥用闭包，可能会导致内存泄漏。
- **性能问题**：闭包的访问和修改操作相对较慢，因为它们需要额外的函数调用和内存访问。

4. **常见场景**：
- **防抖**：短时间内多次触发时，只执行最后一次。通过 `setTimeout` 清除上一次定时器。
- **节流**：限制高频触发事件的执行频率。记录 `lastTime`，保证一定时间间隔执行一次。
- **数据私有化**：模拟私有变量，防止外部访问。使用闭包保存变量，仅暴露访问方法。
- **回调函数保持状态**：让回调函数记住外部作用域的变量。通过返回一个函数，函数内部可访问外部变量。
- **函数柯里化**：把多参数函数拆分为多个单参数调用。递归返回新函数，直到所有参数传递完成。

## 跨域怎么解决

当一个请求 url 的 协议、域名、端口三者之间任意一个与当前页面 url 不同即为 **跨域**。

**解决跨域**：

1. **JSONP（JSON with Padding）**：

**JSONP** 是一种通过 `<script>` 标签来绕过跨域限制的解决方案。它利用了 `<script>` 标签的 src 属性可以访问任何域的特性。

```js
// 客户端
<script>
function test(data) {
    console.log(data.name);
}
</script>
<script src="http://localhost:8080/jsonp?callback=test"></script>
```

```js
// 服务端
res.send('test({name: "John"})');
```

这个例子中，服务端返回了一个字符串，字符串中包含了一个回调函数，回调函数的参数是 `data`，`data` 是一个对象，对象中有一个 `name` 属性，值为 `John`。客户端在请求成功后，会执行 `test` 函数，并传入 `data` 参数，打印 `John`。

**JSONP 优缺点**：

- **优点**：
  - 兼容性好，支持老版本浏览器。
  - 简单易用，不需要额外配置。

- **缺点**：
  - 只支持 `GET` 请求。
  - 安全性较低，容易受到 `XSS` 攻击。
  - 失败时不会返回状态码。

2. **CORS（Cross-Origin Resource Sharing）**：

**CORS** 是一种现代的跨域解决方案，通过在服务器端设置特定的 HTTP 头来允许跨域请求。

- **普通跨域请求**：只需服务端设置 `Access-Control-Allow-Origin` 即可，前端无须设置。

- **带cookie的跨域请求**：前端设置 `withCredentials` 为 `true`，后端设置 `Access-Control-Allow-Credentials` 为 `true`。同时 `Access-Control-Allow-Origin` 不能设置为 `*`，必须明确指定域名。

3. **代理服务器**：

在自己的服务器上创建一个代理，由服务器转发请求。

4. **postMessage**：

`postMessage` 是 HTML5 提供的一种跨域通信机制，允许在不同窗口之间传递消息。

## 防抖和节流

1. **防抖（debounce）**：

在触发事件后的一段时间内，如果再次触发事件，则重新计时。只有在指定时间内没有再次触发事件，才会执行函数。

应用场景：搜索框输入查询、窗口大小调整、表单验证、按钮提交事件。

示例：

```js
function debounce(fn, delay) {
    let timer = null;
    
    return function(...args) {
        // 如果已经设定过定时器，则清空上一次的定时器
        if (timer) {
            clearTimeout(timer);
        }
        
        // 设定新的定时器
        timer = setTimeout(() => {
            fn.apply(this, args);
        }, delay);
    }
}

// 使用示例
const handleInput = debounce((e) => {
    console.log('搜索内容：', e.target.value);
}, 500);

// 绑定到输入框
input.addEventListener('input', handleInput);
```

2. **节流（throttle）**：

节流（`throttle`）在指定时间间隔内，无论触发多少次事件，只执行一次函数。

应用场景：滚动事件处理、页面 resize 事件、拖拽事件、游戏中的按键事件。

示例：

```js
function throttle(fn, delay) {
    let timer = null;
    
    return function(...args) {
        // 如果已有定时器在执行，则直接返回
        if (timer) {
            return;
        }
        
        // 设定新的定时器
        timer = setTimeout(() => {
            fn.apply(this, args);
            timer = null;  // 执行完重置定时器
        }, delay);
    }
}

// 使用示例
const handleScroll = throttle(() => {
    console.log('滚动位置：', window.scrollY);
}, 200);

// 绑定到滚动事件
window.addEventListener('scroll', handleScroll);
```

3. **防抖和节流的区别**：
  
假设 `delay` 都是 `1000ms`

防抖：连续触发事件，只在最后一次触发后的 `1000ms` 后执行一次
防抖时间轴：点击 - 点击 - 点击 - `1000ms` - 执行

节流：连续触发事件，每隔 `1000ms` 执行一次
节流时间轴：点击 - 执行 - `1000ms` - 执行 - `1000ms` - 执行

选择使用防抖还是节流的依据：

- 如果你需要等待用户完成操作后再执行函数，用防抖。例如：等用户输入完成后再搜索。
- 如果你需要保证一定时间内只执行一次函数，用节流。例如：滚动时每隔一段时间计算一次位置。

## Promise 是什么？有哪些 API？

`Promise` 是 JavaScript 中用于处理异步操作的对象。它代表了一个异步操作的最终完成（或失败）及其结果值。

`Promise` 构造函数接受一个函数作为参数，该函数的两个参数分别是 `resolve` 和 `reject`。

`resolve` 函数将 `Promise` 对象的状态从 `pending` 变为 `fulfilled`，并将结果值传递给 `Promise` 对象。

`reject` 函数将 `Promise` 对象的状态从 `pending` 变为 `rejected`，并将失败原因传递给 `Promise` 对象。

`Promise` 有三种状态：

- `pending`：初始状态，表示异步操作正在进行中。
- `fulfilled`：表示异步操作成功完成。
- `rejected`：表示异步操作失败。

`Promise` 的实例方法：

- `Promise.prototype.then()`：`then` 方法定义在 `Promise` 的原型上，用于处理异步操作成功的情况。它返回的是一个新的 `Promise` 对象，可以链式调用。
- `Promise.prototype.catch()`：`catch` 方法定义在 `Promise` 的原型上，用于处理异步操作失败的情况。
- `Promise.prototype.finally()`：`finally` 方法定义在 `Promise` 的原型上，用于处理异步操作无论成功或失败的情况。

`Promise` 的静态方法：

- `Promise.all()`：用于处理多个 `Promise` 对象。它返回的是一个新的 `Promise` 对象，只有当所有 `Promise` 对象都成功时，才会执行 `then` 方法。如果其中有一个 `Promise` 对象失败，则返回的 `Promise` 对象会立即失败，并执行 `catch` 方法。
- `Promise.race()`：用于处理多个 `Promise` 对象。它返回的是一个新的 `Promise` 对象，只要有一个 `Promise` 对象成功或失败，就会执行 `then` 或 `catch` 方法。
- `Promise.allSettled()`：用于处理多个 `Promise` 对象。它返回的是一个新的 `Promise` 对象，只有当所有 `Promise` 对象都成功或失败时，才会执行 `then` 或 `catch` 方法。
- `Promise.resolve()`：用于将一个值转换为 `Promise` 对象。
- `Promise.reject()`：用于将一个值转换为 `Promise` 对象，并立即失败。

## 懒加载

懒加载（`lazy loading`）是一种优化技术，用于 **延迟加载资源** （如图片、视频、脚本等），直到用户需要它们时才加载。这样可以减少初始加载时间，提高页面加载性能。

**基本流程**：

- **初始加载**：只加载页面渲染所需的最小资源（如关键的 HTML、CSS 和 JavaScript）。
- **用户触发**：当用户滚动到页面的某个区域或进行某些交互时，才触发其他资源的加载。
- **动态加载**：在用户需要时，加载剩余的资源（如图片、视频、额外的 JavaScript 模块等）。

**应用场景**：

- **图片懒加载**：图片通常是网页中资源加载的瓶颈，尤其是在长页面上。当用户滚动到图片所在位置时，再加载图片资源，而不是在页面加载时一次性加载所有图片。可以使用 `IntersectionObserver` 监听图片是否进入可视区域。
- **JavaScript 模块懒加载**：使用懒加载技术，只有在用户触发某个操作（如点击按钮、滚动到某个位置）时才加载某些 JavaScript 文件，这对于单页应用（SPA）尤其重要。可以使用 `Webpack` 的 `import()` 函数来实现。
- **路由懒加载**：在单页应用中，路由通常是懒加载的，只有在用户访问某个路由时才加载该路由的组件。可以使用 `Webpack` 的 `import()` 函数来实现。

**优化方法**：

- **占位符**：在图片未加载时，使用低分辨率图片占位，等图片加载后再替换为高分辨率图片。
- **预加载**：在用户需要之前，提前加载资源。
- **缓存**：使用缓存来存储加载过的资源，减少重复加载。

## 原型和原型链

**原型**：每个 JavaScript 对象都有一个属性 `__proto__`，指向该对象的原型。原型是另一个对象，它定义了该对象的共享属性和方法。

**原型链**：每个对象都有一个原型对象，原型对象也是一个对象，也有自己的原型对象，这样就形成了一条链。我们称之为原型链，表示对象及其原型之间的继承关系。

当你访问对象的某个属性时，JavaScript 引擎会沿着原型链查找该属性。如果在对象本身找不到，就会到它的原型上查找，直到找到为止，或者最终达到 `Object.prototype`（所有对象的最终原型），如果还是没有找到，返回 `undefined`。

## 发布订阅设计模式

发布订阅模式（Publish-Subscribe Pattern）是一种常见的设计模式，广泛应用于前端开发中，旨在实现 **对象之间的松耦合通信**。在这种模式下，发布者（`Publisher`）和订阅者（`Subscriber`）通过一个中介（通常称为事件总线或事件通道）进行消息传递，彼此之间无需直接依赖。这使得系统更具可扩展性和可维护性。

**概念**：

- **事件总线**：连接发布者和订阅者的中介，管理订阅关系并分发消息。
- **订阅者**：对特定消息感兴趣的对象，负责接收并处理消息。
- **发布者**：负责发布消息的对象。

**流程**：

- **订阅者** 通过 **事件总线** 订阅特定类型的消息。
- **发布者** 创建消息并发送到 **事件总线**。
- **事件总线** 将消息分发给所有相关的 **订阅者**。
- **订阅者** 接收并处理消息。

**优点**：

- **松耦合**：发布者和订阅者之间没有直接依赖关系，可以独立变化。
- **可扩展性**：可以方便地添加新的订阅者和发布者。
- **灵活性**：可以随时订阅和取消订阅消息。

**注意事项**：

- **避免内存泄漏**：如果订阅者不再需要消息，应及时取消订阅，释放资源。
- **避免重复订阅**：确保订阅者不会重复订阅同一消息，否则多次触发。
- **避免循环依赖**：确保发布者和订阅者之间不会形成循环依赖，否则会导致死循环。例如：订阅者A通知发布者B，发布者B又通知订阅者A。

**示例**：

假设我们有一个包子铺（`baoziShop`），当有新包子上架时，店主希望通知所有订阅者。 我们可以使用发布订阅模式来实现这一功能：

```js
// 事件总线类
class EventBus {
  constructor() {
    this.subscribers = {}; // 存储订阅者的回调函数, 键是事件类型，值是回调函数数组
  }

  // 订阅事件
  subscribe(key, fn) {
    if (!this.subscribers[key]) {
      this.subscribers[key] = [];
    }
    this.subscribers[key].push(fn);
  }

  // 发布事件
  publish(key, ...args) {
    const fns = this.subscribers[key];
    if (!fns || fns.length === 0) return;
    fns.forEach(fn => fn.apply(this, args));
  }

  // 取消订阅
  unsubscribe(key, fn) {
    const fns = this.subscribers[key];
    if (!fns) return;
    if (!fn) {
      fns.length = 0; // 清空所有订阅
    } else {
      this.subscribers[key] = fns.filter(item => item !== fn);
    }
  }
}

// 创建包子铺实例
const baoziShop = new EventBus();

// 小明订阅菜包子
const xiaomingFn = (price) => {
  console.log('小明：菜包子价格是', price);
};
baoziShop.subscribe('菜包子', xiaomingFn);

// 小王订阅肉包子
const xiaowangFn = (price) => {
  console.log('小王：肉包子价格是', price);
};
baoziShop.subscribe('肉包子', xiaowangFn);

// 触发事件
baoziShop.publish('菜包子', 2);  // 小明：菜包子价格是 2
baoziShop.publish('肉包子', 3);  // 小王：肉包子价格是 3

// 取消订阅
baoziShop.unsubscribe('菜包子', xiaomingFn);
baoziShop.unsubscribe('肉包子', xiaowangFn);
```

## JavaScript 的数据类型

JavaScript 的数据类型分为两大类：

1. **基本数据类型（`Primitive Data Type`）**

- `Number`：表示数字，包括整数和浮点数。
- `String`：表示字符串，包括单引号和双引号。
- `Boolean`：表示布尔值，只有两个值：`true` 和 `false`。
- `Null`：表示空值，只有一个值：`null`。
- `Undefined`：表示未定义，只有一个值：`undefined`。
- `Symbol`：表示唯一标识符，每个 `Symbol` 都是独一无二的。
- `BigInt`：表示大整数，可以表示任意大小的整数。

2. **引用数据类型（`Reference Data Type`）**

- `Object`：对象类型
- `Array`：数组类型
- `Function`：函数类型
- `Date`：日期类型
- `RegExp`：正则表达式类型
- `Map`、`Set`、`WeakMap`、`WeakSet` 等（ES6 引入）

**基本数据类型和引用数据类型的区别**：

- 基本数据类型存储在栈中，引用数据类型存储在堆中。
- 基本数据类型是不可变的，引用数据类型是可变的。
- 基本数据类型赋值时创建一个新副本，修改副本不会影响原值。引用数据类型赋值时复制的是引用，指向同一块内存区域，修改其中一个变量会影响另一个变量的值。

## 装饰器

装饰器（`Decorator`）是一种用于修改类或方法行为的函数。它可以在不改变原有代码的情况下，动态地添加、修改或删除类的行为，提高了代码的可维护性和复用性。它使用 `@` 符号来定义，直接定义在类、方法或属性的前面。

装饰器通常用于以下场景：

- 添加日志
- 添加缓存
- 添加权限控制

## MobX

MobX 是一个强大且简洁的状态管理库，适合用于需要响应式、可变状态的应用。它通过 `observable`、`action` 和 `computed` 等概念帮助开发者轻松管理和更新应用状态。

**核心概念**：

- `observable`：使对象或属性可观察，当状态变化时触发通知。
- `action`：定义修改状态的操作，可以是同步或异步的。
- `computed`：根据状态计算衍生值，当状态变化时自动重新计算。

**示例**：

```js
import { observable, action, computed } from 'mobx';
import { observer } from 'mobx-react';

// 定义用户数据对象
const user = observable({
  name: 'John',
  age: 30,
  
  // 定义更新状态的动作
  updateName: action(function(newName) {
    this.name = newName;
  }),
  
  // 定义计算属性
  get userInfo() {
    return `${this.name} is ${this.age} years old`;
  }
});

// 定义 React 组件来显示用户信息
const UserInfo = observer(() => (
  <div>
    <p>{user.userInfo}</p>
    <button onClick={() => user.updateName('Jane')}>Change Name</button>
  </div>
));

export default UserInfo;
```

**MobX 和 Redux 的区别**：

1. **设计理念**

**MobX**

- **响应式编程**：MobX 通过 响应式编程 来管理状态，依赖的状态变化时，自动更新视图。它的核心概念是可观察（observable）数据，计算值（computed values），以及动作（actions）。当数据改变时，相关的组件会自动重新渲染。
- **简洁和自动化**：MobX 的理念是通过自动管理状态和视图的同步来减少手动操作。你不需要手动调用 `dispatch` 或编写 `reducer` 来更新状态。

**Redux**

- **不可变数据和纯函数**：Redux 强调使用 **不可变数据** 和 **纯函数**。它采用 **动作（actions）** 和 **reducer** 来更新状态。每次状态变化都会生成一个新的状态对象，而不是直接修改原状态。这种设计保证了应用状态的可预测性和可调试性。
- **明确的状态流**：Redux 强调应用程序中所有的状态更新必须通过明确的动作和reducer来管理，状态的改变过程非常明确。

2. **状态管理**

**MobX**

- **可变状态**：在 MobX 中，状态通常是可变的，通过 `observable` 修饰数据，数据可以直接修改。状态更新时，相关视图会自动更新。

**Redux**

- **不可变状态**：Redux 强制使用不可变的状态，每次对状态的修改都会返回一个新的状态对象，而不是修改原状态对象。

3. **状态更新**

**MobX**

- **自动状态更新**：在 MobX 中，状态变化时，相关视图会自动更新。

**Redux**

- **手动状态更新**：在 Redux 中，所有的状态更新都必须通过 `dispatch` 动作触发，并通过 `reducer` 函数处理。

4. **性能**

**MobX**

- **细粒度更新**：MobX 只会在状态变化时更新与其相关的组件，性能较好，尤其在涉及大量动态状态和高频更新时。

**Redux**

- **全局重渲染**：Redux 中，每次状态更新时，所有相关的组件都可能会重新渲染，即使它们只依赖状态的一小部分。为了优化性能，通常需要手动实现 `shouldComponentUpdate` 或使用 `reselect` 等工具来优化选择器。

**如何选择？**

- **MobX** 适合需要响应式编程、简洁且易于使用的状态管理，特别是当状态的变更和视图的更新之间有很强的关联时。
- **Redux** 适合需要更明确的状态流和更强的控制、调试能力的大型应用，尤其是涉及复杂异步操作时。

## cookie 和 session

**cookie** 和 **session** 是用于在客户端和服务器之间存储和传输数据的两种机制。

**cookie** 是存储在客户端浏览器中的小型文本文件，用于保存用户信息、偏好设置等。每次 http 请求都会携带 cookie 信息。

**session** 是存储在服务器上的数据，用于保存用户登录状态、购物车数据等需要安全保存的数据。服务器为每个用户创建一个 session 对象，并为其分配一个唯一的 `sessionId`，`sessionId` 通过 `cookie` 存储在客户端浏览器中。

**cookie 和 session 的区别**：

- **存储位置**：cookie 存储在客户端浏览器中，session 存储在服务器上。
- **存储大小**：cookie 存储的大小有限，session 存储的大小没有限制。
- **安全性**：cookie 存储在客户端浏览器中，安全性较低，session 存储在服务器上，安全性较高。
- **生命周期**：cookie 的生命周期可以设置（分为 **会话 cookie （关闭浏览器后自动删除）** 和 **持久 cookie（设置过期时间）**），session 一般有固定的生命周期，可以配置服务器自动清理不活跃的 session。

## Map 和 Object 的区别

**Map** 和 **Object** 是 JavaScript 中用于存储键值对的两种数据结构。

1. **键的类型**

- **Object** 的键必须是字符串或符号。使用其他类型作为键时，会自动转换为字符串。
- **Map** 的键可以是任意类型，包括对象、数组、函数等。

2. **键的顺序**

- **Object** 的键没有顺序。
- **Map** 的键保证按照插入的顺序排列。

3. **使用场景**

- **Object** 适合存储结构化数据，如用户信息、配置等。
- **Map** 适合存储复杂的数据结构，如关系图谱、多对多关系等。

## this 的指向问题

`this` 的指向是动态的，取决于函数的调用方式，而不是函数定义的位置。

1. **全局作用域**

在全局作用域中，`this` 指向全局对象（在浏览器中是 `window`，在 Node.js 中是 `global`）。

```js
console.log(this); // window (浏览器环境)
```

2. **函数作用域**

在函数作用域中，`this` 指向调用该函数的对象。

```js
function showThis() {
  console.log(this);
}
showThis(); // window (浏览器环境)
```

3. **对象方法**

在对象方法中，`this` 指向调用该方法的对象。

```js
const user = {
  name: "John",
  greet() {
    console.log(this.name);
  }
};
user.greet(); // "John" (this指向user)
```

4. **构造函数**

使用 `new` 关键字调用函数时，函数内的 `this` 指向新创建的对象实例。

```js
function User(name) {
  this.name = name;
}
const user = new User("John");
console.log(user.name); // "John"
```

5. **事件处理函数**

在事件处理函数中，`this` 指向触发事件的元素。

```js
const button = document.querySelector("button");
button.addEventListener("click", function() {
  console.log(this); // button
});
```

6. **箭头函数**

箭头函数没有自己的 `this`，它会继承外层函数的 `this` 值。

```js
const obj = {
  name: "John",
  // 普通函数方法
  sayLater1: function() {
    setTimeout(function() {
      console.log(this.name); // undefined，因为this指向window
    }, 1000);
  },
  // 箭头函数
  sayLater2: function() {
    setTimeout(() => {
      console.log(this.name); // "John"，因为箭头函数继承了外部this
    }, 1000);
  }
};
```

7. **显示绑定**

通过 `call`、`apply` 或 `bind` 方法可以明确指定函数执行时的 `this` 值。

```js
function introduce() {
  console.log(`I am${this.name}`);
}

const person = { name: "John" };
introduce.call(person); // "I am John"
introduce.apply(person); // "I am John"
const boundFn = introduce.bind(person);
boundFn(); // "I am John"
```
