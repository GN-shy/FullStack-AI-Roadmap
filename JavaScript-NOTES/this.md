#  this

- `this` 指向
- 改变 `this`

---

## 一、this 指向

`this` 的指向是在**函数调用时**确定的，而不是定义时。不同调用方式下 `this` 指向不同：

| 调用方式     | `this` 指向                                               | 示例                                                   |
| :----------- | :-------------------------------------------------------- | :----------------------------------------------------- |
| 普通函数调用 | 全局对象（浏览器中为 `window`，严格模式下为 `undefined`） | `function fn() { console.log(this) } fn()`             |
| 对象方法调用 | 调用该方法的对象                                          | `const obj = { fn() { console.log(this) } }; obj.fn()` |
| 构造函数调用 | 新创建的实例对象                                          | `new Person()`                                         |
| 箭头函数     | 定义时所在的外层作用域的 `this`，**不绑定自己的 `this`**  | `const fn = () => { console.log(this) }`               |
| 事件处理函数 | 绑定事件的 DOM 元素                                       | `btn.onclick = function() { console.log(this) }`       |

---

## 二、改变 this 指向的方法

### 1. `call()`
- 作用：调用函数，并**立即改变** `this` 指向
- 语法：`函数.call(新this指向, 参数1, 参数2, ...)`
```javascript
function fn(a, b) {
  console.log(this, a + b)
}
const obj = { name: '佩奇' }
fn.call(obj, 1, 2) // this → obj，输出 {name: '佩奇'} 3
```

### 2. `apply()`
- 作用：调用函数，并**立即改变** `this` 指向
- 语法：`函数.apply(新this指向, [参数数组])`
```javascript
function fn(a, b) {
  console.log(this, a + b)
}
const obj = { name: '乔治' }
fn.apply(obj, [1, 2]) // this → obj，输出 {name: '乔治'} 3
```

> `call` 和 `apply` 的区别：参数传递方式不同，`call` 是逐个传参，`apply` 是数组传参。

### 3. `bind()`
- 作用：**创建一个新函数**，并永久绑定 `this` 指向（不会立即调用）
- 语法：`函数.bind(新this指向, 预设参数1, 预设参数2, ...)`
```javascript
function fn(a, b) {
  console.log(this, a + b)
}
const obj = { name: '猪爸爸' }
const newFn = fn.bind(obj, 1)
newFn(2) // this → obj，输出 {name: '猪爸爸'} 3
```

---

## 三、三种方法对比

| 方法    | 是否立即执行 | 参数传递方式             | 使用场景                              |
| :------ | :----------- | :----------------------- | :------------------------------------ |
| `call`  | 是           | 逗号分隔逐个传递         | 临时改变 `this`，参数较少时           |
| `apply` | 是           | 数组传递                 | 临时改变 `this`，参数为数组/类数组时  |
| `bind`  | 否           | 逗号分隔，可预设部分参数 | 永久绑定 `this`（如回调函数、定时器） |

---

## 四、示例：定时器中绑定 this
```javascript
const obj = {
  name: '佩奇',
  say() {
    setTimeout(function() {
      console.log(this.name) // 不绑定的话 this 指向 window，undefined
    }.bind(this), 1000) // 用 bind 绑定 this 为 obj
  }
}
obj.say() // 1秒后输出 '佩奇'
```

---



