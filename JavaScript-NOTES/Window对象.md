# Window对象

## BOM（浏览器对象模型）

BOM (Browser Object Model) 是浏览器对象模型

window：

- `navigator`：浏览器信息
- `location`：地址栏、URL 信息
- `document`：文档对象（DOM 核心，属于 BOM 的一部分）
- `history`：浏览器历史记录
- `screen`：屏幕信息

`window`对象是一个全局对象，也可以说是 JavaScript 中的顶级对象

像`document`、`alert()`、`console.log()`这些都是`window`的属性，基本 BOM 的属性和方法都是`window`的

所有通过`var`定义在全局作用域中的变量、函数都会变成`window`对象的属性和方法

`window`对象下的属性和方法调用的时候可以省略`window`

## 定时器-延时函数

- JavaScript 内置的一个用来让代码延迟执行的函数，叫 `setTimeout`
- 语法：
```javascript
setTimeout(回调函数, 等待的毫秒数)
```
- `setTimeout` 仅仅只执行一次，所以可以理解为就是把一段代码延迟执行，平时可省略 `window`
- 清除延时函数：

```javascript
// 1. 接收定时器标识
let timer = setTimeout(回调函数, 等待的毫秒数)
// 2. 清除延时
clearTimeout(timer)
```
- 说明：`setTimeout` 会返回一个定时器唯一标识，将其传入 `clearTimeout()` 即可提前取消延迟执行。

- ### 注意点
  - 延时器需要等待，所以后面的代码先执行
  - 每一次调用延时器都会产生一个新的延时器

## JS执行机制

JavaScript 语言的一大特点就是**单线程**，也就是说，同一个时间只能做一件事。

这是因为 Javascript 这门脚本语言诞生的使命所致——JavaScript 是为处理页面中用户的交互，以及操作 DOM 而诞生的。比如我们对某个 DOM 元素进行添加和删除操作，不能同时进行。 应该先进行添加，之后再删除。

单线程就意味着，所有任务需要排队，前一个任务结束，才会执行后一个任务。这样所导致的问题是： 如果 JS 执行的时间过长，这样就会造成页面的渲染不连贯，导致页面渲染加载阻塞的感觉。

- 为了解决这个问题，利用多核 CPU 的计算能力，HTML5 提出 Web Worker 标准，允许 JavaScript 脚本创建多个线程。于是，JS 中出现了**同步**和**异步**。

### 同步
前一个任务结束后再执行后一个任务，程序的执行顺序与任务的排列顺序是一致的、同步的。比如做饭的同步做法：我们要烧水煮饭，等水开了（10分钟之后），再去切菜，炒菜。

### 异步
你在做一件事情时，因为这件事情会花费很长时间，在做这件事的同时，你还可以去处理其他事情。比如做饭的异步做法，我们在烧水的同时，利用这10分钟，去切菜，炒菜。



### 同步任务
同步任务都在主线程上执行，形成一个**执行栈**。

### 异步任务
JS 的异步是通过回调函数实现的。

一般而言，异步任务有以下三种类型：
1.  普通事件，如 `click`、`resize` 等
2.  资源加载，如 `load`、`error` 等
3.  定时器，包括 `setInterval`、`setTimeout` 等

异步任务相关添加到**任务队列**中（任务队列也称为消息队列）。

# JS执行机制
1. 先执行执行栈中的同步任务。
2. 异步任务放入任务队列中。
3. 一旦执行栈中的所有同步任务执行完毕，系统就会按次序读取任务队列中的异步任务，于是被读取的异步任务结束等待状态，进入执行栈，开始执行。

由于主线程不断的重复获得任务、执行任务、再获取任务、再执行，所以这种机制被称为`事件循环 (event loop)`。

## Location对象

location的数据类型是对象，它拆分并保存了URL地址的各个组成部分

**常用属性和方法：**

href 属性获取完整的 URL 地址，对其赋值时用于地址的跳转
```js
// 可以得到当前文件URL地址
console.log(location.href)
// 可以通过js方式跳转到目标地址
location.href = 'http://www.itcast.cn'
```
## navigation对象

- navigator的数据类型是对象，该对象下记录了浏览器自身的相关信息
- 常用属性和方法：
> 通过 userAgent 检测浏览器的版本及平台
```javascript
//检测 userAgent（浏览器信息）
!(function () {
  const userAgent = navigator.userAgent
  //验证是否为Android或iPhone
  const android = userAgent.match(/(Android);?[\s\/]+([\d.]+)?/)
  const iphone = userAgent.match(/(iPhone\sOS)\s([\d_]+)/)

  // 如果是Android或iPhone，则跳转至移动站点
  if (android || iphone) {
    location.href = 'http://m.itcast.cn'
  }
})()
```



## history对象

- history的数据类型是对象，主要管理历史记录，该对象与浏览器地址栏的操作相对应，如前进、后退、历史记录等
- 常用属性和方法：

| history对象方法 | 作用                                                       |
| --------------- | ---------------------------------------------------------- |
| back()          | 可以后退功能                                               |
| forward()       | 前进功能                                                   |
| go(参数)        | 前进后退功能 参数如果是 1 前进1个页面 如果是-1 后退1个页面 |





